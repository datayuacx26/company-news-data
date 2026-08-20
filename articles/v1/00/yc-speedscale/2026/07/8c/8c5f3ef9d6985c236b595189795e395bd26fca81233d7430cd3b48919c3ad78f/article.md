---
schema_version: "1.0.0"
document_id: "8c5f3ef9d6985c236b595189795e395bd26fca81233d7430cd3b48919c3ad78f"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/ebpf-garbage-http-payloads-scatter-gather-buffers/"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T18:02:56.371078+00:00"
fetched_at: "2026-07-29T18:02:59.945422+00:00"
content_hash: "sha256:3699f34ad81fbee9bfce6a0a4d94b9d1bd20068738e78478d191bc597f7d2134"
---

# eBPF: Preventing Garbage HTTP Payloads When Reading Kernel Scatter-Gather Buffers

Recently someone on our team opened a traffic snapshot and found an HTTP request that was captured with our eBPF capture agent,` nettap` . Our protocol dissector parsed *most* of the response correctly, but that correctness ended once the response headers were processed. What they ended up with was a recording of an HTTP request/response where the response body was just an incorrect collection of garbage binary data when it should have been JSON text.


But here’s the kicker: it only did this *some of the time* and didn’t negatively affect the application being monitored (i.e. the app either received or sent correct HTTP payloads). This didn’t help me isolate the problem as strictly an eBPF problem, a protocol dissector problem, or both.


It turned out to be an eBPF problem after all: the TCP probes were incorrectly (or rather, *incompletely* ) processing and reading the kernel’s scatter-gather` struct iov_iter` buffers.


🎯 Key Takeaways


- ` tcp_recvmsg` reads into a` struct iov_iter` , not a flat buffer, and by` fexit` time the kernel has usually already advanced that iterator — so reading it at return time gives you the tail, not the head.
- The` enum iter_type` values and the` iov` /` __iov` field name are *not* stable across kernel versions; hardcoding them silently reads garbage. CO-RE (` bpf_core_enum_value*` , struct flavors,` bpf_core_field_exists` ) is how you stay portable.
- The fix: capture each segment’s` iov_base` /` iov_len` at` fentry` into a` BPF_MAP_TYPE_TASK_STORAGE` map, then read them back at` fexit` and bound the total by the return value with` MIN(remain, iov_len)` .
- Every failure mode here is silent — nothing crashes, nothing logs — because` bpf_probe_read_user()` tolerates bad pointers.


## What Is Available to TCP eBPF Probes?


` nettap` captures plaintext TCP payloads by attaching BPF` fentry` and` fexit` trampolines to` tcp_recvmsg` and` tcp_sendmsg` , then reading the application’s buffer directly out of the` struct msghdr` the kernel was given. For` tcp_recvmsg` all of the heavy lifting was in the` fexit` probe since you get both the function call arguments **AND** its return value in a single eBPF program. The return value in this case is the number of bytes that were read. Pretty standard C stuff.


But what you have to remember is that` tcp_recvmsg` doesn’t use “just a buffer”, it uses` msg->msg_iter` , which is a` struct iov_iter` - an abstraction on top of “here is some data in memory, which is one of a few different types, you need to iterate over it to get the whole thing”. LWN’s[introduction to the interface](https://lwn.net/Articles/625077/) is a fantastic, plain-English writeup on what it’s for (and it makes **perfect** sense).


Here’s the shape of it as of v6.12, from[include/linux/uio.h](https://elixir.bootlin.com/linux/v6.12/source/include/linux/uio.h) (just the important parts):


```text
struct   iov_iter {
u8 iter_type;
bool   nofault;
bool   data_source;
size_t   iov_offset;
union   {
struct   iovec __ubuf_iovec;
struct   {
union   {
const   struct   iovec   *  __iov;
const   struct   kvec   *  kvec;
const   struct   bio_vec   *  bvec;
/* ... */
void   __user   *  ubuf;
};
size_t   count;
};
};
union   {
unsigned   long   nr_segs;
u8 folioq_slot;
loff_t   xarray_start;
};
};
```


The problem with this and writing eBPF programs is that you can’t use the kernel APIs for actually iterating over this data in a type-agnostic way. If it’s not an exposed` kfunc` you can’t use it. So what does that mean for processing this data structure?


First, the` union` doesn’t guarantee type safety; you have to know which to use based on the` iter_type` struct field which holds the value of a specific` enum iter_type` (all of this is in` include/linux/uio.h` by the way). Pre-5.14 kernels use` unsigned int type` instead of` u8 iter_type` , but it still holds the value of an` enum iter_type` . For this exercise, we’re only focusing on` __iov` and` ubuf` from the struct shown above (again, kernel version **does** matter so pay attention to that).


Second, the thing you need to be mindful of in this situation (or any eBPF program, really) is that on` fexit` , the kernel may have (read: probably has) already processed and consumed the iterator, rendering it completely useless. You can’t go backwards once it’s been advanced. In other words, at` fexit` time, if the kernel has processed more than one segment, you’re only going to know where you ended up, not where you started.


Where the original implementation got lucky was in handling TCP reads/writes that could fit the entire request/response in a single segment. This, of course, broke when spanning more than one segment and we ended up with a return value` n` being larger than what the last segment buffer actually was.


See where I’m going? Buffer over-read. And that’s how we ended up with the garbage binary data.


```text
flowchart TD
A[tcp_recvmsg returns n bytes] --> B{Iterator spans<br/>one segment?}
B -->|Yes, nr_segs == 1| C[Read n bytes from<br/>the single buffer]
C --> D[Correct payload]
B -->|No, multiple segments| E[Read n bytes from<br/>last segment only]
E --> F[n larger than<br/>that segment's length]
F --> G[Buffer over-read:<br/>garbage trailing bytes]


```


So how did we run into this? We have a set of demo applications implemented in several different programming languages and the ones that hit this the most, if not consistently: the Node apps. I didn’t dig into the “why” on the Node-side, I just left it at that and focused on the fix.


## Reminder: Kernel Versions Matter


Before going any further, let’s talk about kernel versions and portability. Remember that we have to check the` iter_type` value in order to know what struct field to actually use. The problem is that those values are not stable across kernel versions. And I don’t mean “they were renamed once in 2015”; I mean they have changed a few times:


Kernel` ITER_IOVEC`` ITER_KVEC`` ITER_BVEC`` ITER_UBUF`


<= 5.13 4 (bitmask) 8 16 n/a


5.14 - 5.19 0 1 2 n/a


6.0 - 6.4 0 1 2 6


6.5 - 6.6 0 1 2 5


6.7 - 6.11 1 3 2 0


6.12+ 1 3 2 0


The takeaway here is that` ITER_UBUF` is *relatively* new and was added in v6.0.` ITER_PIPE` was deleted entirely in 6.5. The entire enum was reordered with` ITER_UBUF` being first in 6.7, and` ITER_FOLIOQ` being added in 6.12. On kernels 5.14 through 6.6,` 0` means` ITER_IOVEC` . On 6.7 and later,` 0` means` ITER_UBUF` .


Yeesh… so how do we handle this?


An eBPF probe that hardcodes the assumption that` ITER_IOVEC` is equal to` 0` will, on any reasonably current kernel, take a single userspace` char*` and dereference it as a` struct iovec*` (it *absolutely* is not). Fortunately, though, nothing crashes because` bpf_probe_read_user()` assumes the given pointer is inherently unsafe, so reads are safe. But in this case, it meant that we ended up reading garbage data, the exact failure mode we were looking for in the first place.


This is where CO-RE really becomes a godsend.` libbpf` provides a couple of extremely useful functions for this specific sort of quagmire:` bpf_core_enum_value_exists()` and` bpf_core_enum_value()` . By leveraging the former, you can programmatically check if the enum value you want, like` ITER_UBUF` , actually exists. The latter lets you get the *actual* value from the machine on which your application is running.


The same story applies to struct field renames. In this particular case,` struct iov_iter` would have a` const struct iovec*` member named either` iov` or` __iov` , and potentially at *different* offsets, depending on the kernel version (6.4 is where it picked up the underscores). CO-RE handles this one too, but the mechanism is a bit weirder. You declare a “flavor” of the struct that contains only the fields you care about, then let` bpf_core_field_exists()` decide which one applies to the kernel you happen to be running on:


```text
// IMPORTANT: CO-RE flavors only work if you suffix the struct name with *THREE* underscores
struct   iov_iter___6_x_old {
const   struct   iovec  *   iov;
}   __attribute__  ((preserve_access_index));


static   __always_inline   const   struct   iovec  *   get_iovec  (  const   struct   iov_iter  *   iter  )
{
// 6.4 and later renamed .iov to .__iov
if   (  bpf_core_field_exists  (iter->__iov))
return   BPF_CORE_READ  (iter, __iov);
else   if   (  bpf_core_field_exists  (((  const   struct   iov_iter___6_x_old  *  )iter)->iov))
return   BPF_CORE_READ  ((  const   struct   iov_iter___6_x_old  *  )iter, iov);


return   NULL  ;
}
```


```text
flowchart LR
A[struct iov_iter] --> B{Field __iov<br/>exists?}
B -->|Yes, 6.4+| C[BPF_CORE_READ<br/>iter, __iov]
B -->|No| D{Flavor field .iov<br/>exists?}
D -->|Yes, pre-6.4| E[BPF_CORE_READ<br/>flavor, iov]
D -->|No| F[return NULL]


```


That three underscore business is not a typo and it is not optional. Use two and` libbpf` treats your flavor as a completely unrelated type: no relocation, no warning, no error, just garbage data again. Andrii Nakryiko’s[CO-RE reference guide](https://nakryiko.com/posts/bpf-core-reference-guide/) is about the only place I’ve ever seen that documented, and it’s an incredible reference on all of this. Read it front to back if you spend any time in this space.


## (Another) Reminder: Kernel Versions Matter!


Turns out, enum variations between kernel versions were just *one* of the compatibility issues I ran into.


The other? Changes in the *function signature* itself between versions. An` fentry` program written against` tcp_recvmsg` ’s signature from one kernel version will fail to attach on another, because the arguments no longer agree with the system’s BTF prototype. Here’s how that signature has drifted over time:


```text
// pre-5.19 kernels
int   tcp_recvmsg  (  struct   sock   *  sk  ,   struct   msghdr   *  msg  ,   size_t   len  ,   int   nonblock  ,   int   flags  ,   int   *  addr_len  );


// 5.19 and 6.x kernels
int   tcp_recvmsg  (  struct   sock   *  sk  ,   struct   msghdr   *  msg  ,   size_t   len  ,   int   flags  ,   int   *  addr_len  );


// 7.1 kernels
int   tcp_recvmsg  (  struct   sock   *  sk  ,   struct   msghdr   *  msg  ,   size_t   len  ,   int   flags  );
```


Dealing with this is trivial, but not on the surface. First, you need to define` fentry` and` fexit` programs for *each* of those function signatures:


```text
SEC  (  "fentry/tcp_recvmsg"  )
int   BPF_PROG  (tcp_recvmsg_fentry_nonblock,   struct   sock  *   sk  ,   struct   msghdr  *   msg  ,   size_t   len  ,
int   nonblock  ,   int   flags  ,   int*   addr_len  );


SEC  (  "fentry/tcp_recvmsg"  )
int   BPF_PROG  (tcp_recvmsg_fentry,   struct   sock  *   sk  ,   struct   msghdr  *   msg  ,   size_t   len  ,
int   flags  ,   int*   addr_len  );


SEC  (  "fentry/tcp_recvmsg"  )
int   BPF_PROG  (tcp_recvmsg_fentry_no_addr_len,   struct   sock  *   sk  ,   struct   msghdr  *   msg  ,   size_t   len  ,
int   flags  );
```


Then, prior to attaching your programs, use the system’s BTF information to help you prune the incompatible ones. Straightforward in Go using` bpf2go` and` github.com/cilium/ebpf` with a little bit of legwork:


```text
import   (
"  github.com/cilium/ebpf  "
"  github.com/cilium/ebpf/btf  "
)


func   removeIncompatiblePrograms  (  spec   *  ebpf  .  CollectionSpec  ) {
// load the system BTF (usually at this path)
kernelBTF, err   :=   btf.  LoadSpec  (  "/sys/kernel/btf/vmlinux"  )
if   err   !=   nil   {
log.  Fatalf  (  "cannot load BTF:   %w  "  , err)
}


var   fn   *  btf  .  Func
if   err   :=   kernelBTF.  TypeByName  (  "tcp_recvmsg"  ,   &  fn); err   !=   nil   {
log.  Fatalf  (  "tcp_recvmsg not found in kernel BTF:   %v  "  , err)
}


proto, ok   :=   fn.Type.(  *  btf  .  FuncProto  )
if   !  ok {
log.  Fatalf  (  "tcp_recvmsg BTF type is   %T  , expected *btf.FuncProto"  , fn.Type)
}


// depending on the param count, remove the incompatible ones
switch   len  (proto.Params) {
case   6  :
// keep: with nonblock
delete  (spec.Programs,   "tcp_recvmsg_fentry"  )
delete  (spec.Programs,   "tcp_recvmsg_fentry_no_addr_len"  )
case   5  :
// keep: removed nonblock, with addr_len
delete  (spec.Programs,   "tcp_recvmsg_fentry_nonblock"  )
delete  (spec.Programs,   "tcp_recvmsg_fentry_no_addr_len"  )
case   4  :
// 7.1+
delete  (spec.Programs,   "tcp_recvmsg_fentry_nonblock"  )
delete  (spec.Programs,   "tcp_recvmsg_fentry"  )
}
}


// *bpf.YourProjectObjects is a generated name done by bpf2go that contains all programs, maps, and global
//vars. this is just for demonstration
func   linkTracingProbes  (  objects   *  bpf  .  YourAppObjects  ) {
name   :=   "fentry/tcp_recvmsg"


// try to attach all the variants. we'll go with the first one that succeeds (the one that wasn't removed)
variants   :=   []  *  ebpf  .  Program  {
objects.TcpRecvmsgFentryNonblock,
objects.TcpRecvmsgFentry,
objects.TcpRecvmsgFentryNoAddrLen,
}


for   _, prog   :=   range   variants {
if   prog   ==   nil   {
continue
}


opts   :=   link  .  TracingOptions  {
Program:    prog,
AttachType: ebpf.AttachTraceFEntry,
}


//
if   l, err   =   link.  AttachTracing  (opts); err   ==   nil   {
// success
break
}


// it failed, wrong function argument count, try the next
}
}
```


## Revisiting the Initial Implementation


The initial implementation seemed correct on the surface (at least for the majority of the cases we were able to observe and test against), so we shipped it. The eBPF programs that pushed TCP data from the kernel to our protocol dissectors were essentially just doing a simple pass over` struct iov_iter` assuming a couple of known differences between versions we had been directly exposed to. In a nutshell, we’d look for the` iter_type` value, check that against a constant value we *probably* knew, then read the struct field we needed to use.


Once we had the` iter_type` we needed to know, we assumed that a single segment (i.e.` nr_segs == 1` ) was Probably OK™ and just read whatever data we could based on the return value we observed (the number of bytes read/written).


In hindsight, assuming reads/writes were small enough to fit into a single buffer segment was a hacky, naive implementation, but it worked given the dataset we could test against at the time. We lacked exposure to kernel and application variations that would have surfaced the issue prior to release. But let’s be honest: unless you work for[NASA](https://spinroot.com/gerard/pdf/P10.pdf) , you likely go for a reliable set of compatibility rather than an exhaustive one, otherwise you’d never release anything.


It’s a simple fix: if it’s a` struct iovec` , iterate` nr_segs` times to read` iov_len` amount of data from each` struct iovec*` , offset from the initial` struct iovec*` . And that’s fine if the pointer hasn’t been advanced by the kernel by the time you read the data. But what if it did?


A quick note about state at` fentry` vs` fexit` (or` kprobe` and` kretprobe` if you aren’t using eBPF trampolines): things *can* change between the time you first see something and the time you last see something. This really isn’t an issue for` fentry` on` tcp_sendmsg` ; it’s trivially handled because you have both access to the data before pointers are mucked with *and* a` size_t` that tells you how much data is there.


Handling` tcp_recvmsg` is a little different; a chicken/egg problem. You need the pointer to the start of the data in` fentry` , but you need the return value in` fexit` , the point at which the kernel has likely advanced pointers on you. In other words, don’t attempt to look at the data *only* at` fexit` time since the pointer you assume is the head of the list could be, and probably would be, pointing to the wrong data. In a nutshell, you need the unmodified pointer at` fentry` time but the return value at` fexit` time.


How can we solve this?


```text
sequenceDiagram
participant App as Application
participant K as Kernel tcp_recvmsg
participant FE as fentry probe
participant TS as Task-local storage
participant FX as fexit probe
participant US as Userspace
App->>K: recv() into iov_iter
FE->>K: read iov_base / iov_len per segment
FE->>TS: save unadvanced segments
K->>K: copy data, advance iterator
K->>App: return n bytes
FX->>TS: load saved segments
FX->>FX: bound total by n, MIN(remain, iov_len)
FX->>US: send correct payload


```


## Tracking Data Between` fentry` and` fexit`


One option for tracking a potentially volatile pointer value between` fentry` and` fexit` is to just use a` BPF_MAP_TYPE_HASH` or` BPF_MAP_TYPE_LRU_HASH` map. Key the data on the value of` bpf_get_current_pid_tgid()` and track whatever value you need for later. It’s a common pattern, it’s simple, and it works well.


A second approach, and the one that we opted for, is to use a specific type of map` BPF_MAP_TYPE_TASK_STORAGE` . These task-local storage maps provide space usable to eBPF programs that are *specific* to the current task. In other words, adding or reading to and from these maps automatically handles the task-specific key management shown above using` bpf_get_current_pid_tgid()` . Another, perhaps even more attractive attribute of these maps is that any entry for a task is *automatically* removed whenever the task itself ends. A pretty handy (and clean) approach to the problem. Here’s a trivial, dummy example of what that might look like:


```text
struct   task_data {
void*   something_to_track;
};


struct   {
__uint  (type, BPF_MAP_TYPE_TASK_STORAGE);
__uint  (map_flags, BPF_F_NO_PREALLOC);
__type  (key,   int  );    // note: must always be 32-bit value
__type  (value,   struct   task_data);
} task_storage_map   SEC  (  ".maps"  );


SEC  (  "fentry/some_kernel_func"  )
int   BPF_PROG  (handle_fentry,   int   first_arg  ,   void*   second_arg  )
{
struct   task_struct  *   task   =   bpf_get_current_task_btf  ();
struct   task_data  *   data   =   bpf_task_storage_get  (  &  task_storage_map, task,   NULL  , BPF_LOCAL_STORAGE_GET_F_CREATE);


if   (  !  data)
return   0  ;


// get the pointer to whatever you need to track for later
data->something_to_track   =   second_arg;


return   0  ;
}


SEC  (  "fexit/some_kernel_func"  )
int   BPF_PROG  (handle_fexit,   int   first_arg  ,   void*   second_arg  ,   int   ret  )
{
// you have access to the return value at this point, so you can make decisions based on that


// retrieve it
struct   task_struct  *   task   =   bpf_get_current_task_btf  ();
struct   task_data  *   data   =   bpf_task_storage_get  (  &  task_storage_map, task,   NULL  ,   0  );


if   (  !  data)
return   0  ;


// data->something_to_track is a void* now accessible


return   0  ;
}
```


## Fixing` tcp_recvmsg` for Real


Now that we’ve covered the basic structure of the` fentry` and` fexit` handoff using task-local storage, let’s get back to fixing our initial, incorrect implementation. We’ll worry about` tcp_sendmsg` later; we don’t need the return value as the data size is one of the function arguments. We do, however, for` tcp_recvmsg` .


The basic approach for` tcp_recvmsg` is this: for` nr_segs` worth of` struct iovec*` , track their values for` void* iov_base` and` __kernel_size_t iov_len` . We’ll retrieve and use them in their original state during the` fexit` call. Otherwise, in the return, we might incorrectly process the` fexit` argument` struct msghdr*` as if its` msg_iter.iov` pointer were the start of the iterator and not its tail. I’ll intentionally leave out some of the more tricky/nuanced aspects of handling kernel version differences, but it might look like this:


```text
// NOTE: we limit the number of segments we process to 32 to make sure the verifier doesn't think
// the loop is unbounded. From observations, though, *most* have just a single segment or a handful at max
#define   MAX_ITER_SEGS   32


struct   iovec_seg {
void*   iov_base;
__kernel_size_t   iov_len;
};


struct   iovec_segs {
__u32 nr_segs;
struct   iovec_seg   segs  [MAX_ITER_SEGS];
};


struct   {
__uint  (type, BPF_MAP_TYPE_TASK_STORAGE);
__uint  (map_flags, BPF_F_NO_PREALLOC);
__type  (key,   int  );    // always 32-bit
__type  (value,   struct   iovec_segs);
} tcp_iovec_segs_map   SEC  (  ".maps"  );


SEC  (  "fentry/tcp_recvmsg"  )
int   BPF_PROG  (nt_tcp_recvmsg_fentry,   struct   sock  *   sk  ,   struct   msghdr  *   msg  ,   size_t   len  ,
int   flags  ,   int*   addr_len  )
{
// TIP: some applications might only be peeking at data rather than consuming it, so unless
// you want duplicated data in the end, add this to your code
if   ((flags   &   MSG_PEEK)   ==   MSG_PEEK)
return   0  ;


struct   iov_iter iter   =   BPF_CORE_READ  (msg, msg_iter);


// example: just iovec for now
if   (  !  is_iter_type_iovec  (  &  iter))
return   0  ;


// get the version-dependent field
const   struct   iovec  *   base_iov   =   get_iovec  (  &  iter);


// get our task-local storage to save the pointers for later
struct   task_struct  *   task   =   bpf_get_current_task_btf  ();
struct   iovec_segs  *   data   =   bpf_task_storage_get  (  &  tcp_iovec_segs_map, task,   NULL  ,
BPF_LOCAL_STORAGE_GET_F_CREATE);


if   (  !  data)
return   0  ;


data->nr_segs   =   iter.nr_segs;


// see note on MAX_ITER_SEGS above
for   (  int   i   =   0  ; i   <   MAX_ITER_SEGS; i  ++  ) {
if   (i   >=   data->nr_segs)
break  ;


// current
const   struct   iovec  *   cur_iov   =   base_iov   +   i;


// save to struct in task-local storage
data->segs[i].iov_base   =   BPF_CORE_READ  (cur_iov, iov_base);
data->segs[i].iov_len   =   BPF_CORE_READ  (cur_iov, iov_len);
}


return   0  ;
}
```


The work in` fentry` here is intentionally dumb and only tracks data. It’s in` fexit` where all of the heavy lifting for our implementation lives. At that point, we have all of the necessary information we need to consume the iterator’s underlying data and send that to userspace for further processing:


```text
SEC  (  "fexit/tcp_recvmsg"  )
int   BPF_PROG  (nt_tcp_recvmsg_fexit,   struct   sock  *   sk  ,   struct   msghdr  *   msg  ,   size_t   len  ,
int   flags  ,   int*   addr_len  ,   int   ret  )
{
// NOTE: checking return value is wise here - you can exclude failures
if   (ret   <=   0  )
return   0  ;


// NOTE: at this point you can do all kinds of things not covered by this post: port filtering (i.e. known
// TLS ports), PID exclusions, etc.


// pull it from task-local storage
struct   task_struct  *   task   =   bpf_get_current_task_btf  ();
struct   iovec_segs  *   data   =   bpf_task_storage_get  (  &  tcp_iovec_segs_map, task,   NULL  ,   0  );


// validity checks
if   (  !  data)
return   0  ;


// process and send to userspace
const   struct   iovec  *   base_iov   =   (  const   struct   iovec  *  )data->segs;


// `ret` bytes remaining to process
__u32 remain   =   ret;


for   (  int   i   =   0  ; i   <   MAX_ITER_SEGS   &&   remain   >   0  ; i  ++  ) {
if   (i   >=   data->nr_segs)
break  ;


// current
const   struct   iovec  *   cur_iov   =   base_iov   +   i;


// how much of this segment should we read
__u32 read_len   =   MIN  (remain,   BPF_CORE_READ  (cur_iov, iov_len));


// send something to userspace (this is only for example)
struct   tcp_data chunk   =   {
.buf   =   (  void*  )  BPF_CORE_READ  (cur_iov, iov_base),
.len   =   read_len,
};


send_to_userspace  (  &  chunk);


remain   -=   read_len;
}


return   0  ;
}
```


Don’t forget that the` tcp_recvmsg` return value carries the *actual* total number of bytes you need to read. This is important to remember because` iov_len` is the buffer’s *capacity* , not the size of the data written there. I know this because my first fix used` iov_len` directly, ignored the function return value entirely, and (shocker) still ended up with garbage trailing data. Silly mistake to have made in hindsight, but that’s why you see the remain count bookkeeping and` MIN(remain, iov_len)` size limit in the code above.


And there you have it. We’ve fixed the naive, single segment approach to one that handles them all correctly.


## Final Notes


We only really discussed the` ITER_IOVEC` and` ITER_UBUF` types here and none of the others. We’ve intentionally left those out as unimplemented/unsupported for now, purely as a scope call. The 32-segment cap from earlier is in the same bucket: reads with more segments than that get their tail silently truncated, which is a known gap rather than something we’ve actually solved.


So if there’s a portable lesson buried in all of this, it’s that reading another process’s memory from inside the kernel is never as simple as “follow the pointer.” The pointer becomes a lie the moment the copy starts. The constant you compare it against could change meaning every couple of releases. And the length field you instinctively reach for first is the wrong one. Every single one of those failures is *silent* - nothing crashes, nothing logs, nothing fails a test. They just hand you a screenful of zeros and leave you to work out why. Which, eventually, we did.
