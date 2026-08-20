---
schema_version: "1.0.0"
document_id: "e71c0211940ffa38dea754efa182c89bd4c8919b68fd10f5e36133d01118e6e5"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/ebpf-go-design-notes-1/"
published_at: "2025-08-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:10a9ec4e6a90965661872d04efadfbb2f15e2fd18ddb9d4493cee5e0dd04bf5a"
---

# Under the Hood with Go TLS and eBPF

# Under the Hood with Go TLS and eBPF


There are many reasons why you might want to leverage eBPF for plaintext inspection of TLS traffic including security monitoring, debugging, traffic analysis, etc. The formula for doing this for applications that use OpenSSL is largely the same: attach probes to the` SSL_read` and` SSL_write` symbols in` libssl.so` to observe function entry and function return, and then process the unencrypted data depending how you please. Go, however, makes this straightforward approach completely unusable.


But why?


In this post we’ll discuss the challenges faced in instrumenting Go applications with eBPF probes, and present options for overcoming them in order to capture plaintext TLS payloads.


---


## A Quick eBPF Refresher


[eBPF](https://docs.kernel.org/bpf) is a feature of the Linux kernel that allows you to run sandboxed programs at a variety of kernel-space and user-space hooks, giving you the ability to observe, and even influence, system or application behavior with very little overhead.


There are many[program types](https://docs.kernel.org/bpf/libbpf/program_types.html#program-types-and-elf) available that serve different purposes. For this post, we are most interested in` BPF_PROG_TYPE_KPROBE` which provides the ability to attach to a user-space function in order to observe when it is called and when it returns, called a` uprobe` and` uretprobe` respectively. This can be done for any ELF executable or shared object, so long as symbols aren’t stripped.


Additionally, eBPF programs can, and usually do, make use of generic storage mechanisms called[maps](https://docs.kernel.org/bpf/maps.html) in order to share data between kernel-space eBPF programs or between kernel-space and user-space.


For more detailed information, the following are excellent resources:


- [Kernel Documentation](https://docs.kernel.org/bpf/index.html)
- [Isovalent Community Documentation](https://docs.ebpf.io/)
- [Cilium Documentation](https://docs.cilium.io/en/stable/reference-guides/bpf/index.html)
- [libbpf Example Code](https://github.com/libbpf/libbpf-bootstrap)


---


## How TLS Capture Usually Works (Non‑Go Appliations)


Many, if not most, applications terminate TLS through user‑space libraries that provide higher level APIs that implicitly handle the complexities of TLS. And because all of this happens in user-space, any kernel-space observations will be encrypted.


Let’s briefly revisit the OpenSSL pattern mentioned earlier. OpenSSL provides two functions,` SSL_read` and` SSL_write` , that allow applications to work only with plaintext data. By creating eBPF probes to attach to these functions’ entry and exit points, we’re able to observe plaintext data *before* it’s encrypted and *after* it’s decrypted, without the need for any complex setup involving trusted CA certificates or MITM proxies. Here’s a structural example of eBPF programs for OpenSSL using[libbpf](https://github.com/libbpf/libbpf) .


```text
#include   "vmlinux.h"    // see: https://docs.kernel.org/bpf/libbpf/libbpf_overview.html#bpf-co-re-compile-once-run-everywhere


#include   <bpf/bpf_tracing.h>


SEC  (  "uprobe/SSL_read"  )
int   BPF_UPROBE  (ssl_read_call)
{
// process SSL_read call
return   0  ;
}


SEC  (  "uprobe/SSL_read"  )
int   BPF_URETPROBE  (ssl_read_return)
{
// process SSL_read return
return   0  ;
}


SEC  (  "uprobe/SSL_write"  )
int   BPF_UPROBE  (ssl_write_call)
{
// process SSL_write call
return   0  ;
}


SEC  (  "uprobe/SSL_write"  )
int   BPF_URETPROBE  (ssl_write_return)
{
// process SSL_write return
return   0  ;
}
```


Note that for trivial cases like these, the` uprobe` and` uretprobe` are attached at specific, known locations: when the function is called and when the function returns. Attaching these probes are not strictly limited to these locations. For reasons we will see in a moment, you can also attach a` uprobe` at a specific offset from the function itself, effectively giving you the opportunity to define “custom” hooks within the function body.


---


## Why Go is Different (and Tricky)


First and foremost, Go uses its own internal TLS implementation via the` crypto/tls` package in order to preserve the statically linked aspect of compiled binaries. As a result, there’s not a one-size-fits-all solution for Go applications like there would be for applications dynamically linking to` libssl.so` . This is made even more challenging by the fact that the functions we want to attach to,` Read` and` Write` of a` *tls.Conn` , will have *different* locations for each unique compiled Go binary.


Also, Go uses a different ABI convention than the Linux kernel expects. This, combined with the fact that Go stack frames can dynamically resize from an initial size, means that the use of a a normal` uretprobe` is unreliable and useless when instrumenting Go applications.


Although not necessarily a Go-specific limitation, binaries **must** be unstripped. Using` go build` with` -ldflags="-s"` strips the symbol table from the compiled ELF binary. Without the symbol table, we’re unable to attach a` uprobe` to a specific function since its address is unknowable. To demonstrate, let’s look at an example. Take the following simple Go program that performs a single TLS request and prints the response:


```text
package   main


import   (
"  fmt  "
"  io  "
"  log  "
"  net/http  "
)


func   main  () {
resp, err   :=   http.  Get  (  "https://httpbingo.org/uuid"  )
if   err   !=   nil   {
log.  Fatal  (  "failed to get page"  )
}


body, err   :=   io.  ReadAll  (resp.Body)
if   err   !=   nil   {
log.  Fatal  (  "failed to read body"  )
}


fmt.  Println  (  string  (body))
}
```


If built without` -ldflags="-s"` , the symbol table of the ELF binary can be observed using` objdump -t` . For our use-case, we want to see the` Read` and` Write` functions for` *tls.Conn` , but more on this later. If the binary is stripped, you’d see the following unusable information from` objdump` :


```text
objdump   -t   stripped-binary
```


Results:


```text
stripped-binary:       file   format   elf64-littleaarch64


SYMBOL   TABLE:
no   symbols
```


---


## This ABI or That ABI


Regarding the Go ABI, something that will affect how you might support an arbitrary Go application is the version of Go with which the application was compiled. It’s important to remember because the ABI version used by the Go application you are intending to monitor changes where you need to look for function arguments and return values (stack or registers).


In recent years, Go changed its internal ABI from a Plan9 influenced approach that passed function arguments purely on the stack (referred to as` ABI0` ) to one that uses a hybrid approach using both registers and the stack, registers being preferred (referred to as` ABIInternal` ). It was a marked performance improvement for Go applications that was initially released for amd64 in Go 1.17 and later expanded to arm64 (and more) in Go 1.18.


The intricacies of both ABI versions are outside the scope of this post, but let’s assume we only want to support the newer` ABIInternal` convention, the details of which can be found in the[specification](https://go.googlesource.com/go/+/refs/heads/master/src/cmd/compile/abi-internal.md) .


If you are building a Go application to attach eBPF probes to other Go applications, you can use the` debug/buildinfo` package in the Go standard library to enforce version checks before you attempt to attach eBPF probes to a Go binary:


```text
if   info, err   :=   buildinfo  (pathToBinary); err   !=   nil   {
// handle error
}    else   {
// check info.Version, a string like "go1.24.2" for ABI compatibility
}
```


---


## Attaching to Go Symbols for TLS


As previously mentioned, Go binaries are statically linked and utilize an internal TLS implementation rather than dynamically linking to` libssl.so` (or another TLS library). Although it’s a bit of an inconvenience, the approach is still the same: attach to the address of the symbol we want to instrument. The difference is that we have to do this on a *per-binary* basis rather than once per shared object.


For TLS instrumentation, we’re primarily interested in two symbols - the` Read` and` Write` functions for` *tls.Conn` . These correspond to the symbols` crypto/tls.(*Conn).Read` and` crypto/tls.(*Conn).Write` respectively. You can see this by using` objdump` :


```text
objdump   -t   your-binary   |   grep   -E   'crypto/tls.\(\*Conn\).(Read|Write)
```


You can also do this programatically in Go by using the` debug/elf` package in the standard library:


```text
f, err   :=   elf.  Open  (pathToBinary)
if   err   !=   nil   {
// handle error
}


symbols, err   :=   f.  Symbols  ()
if   err   !=   nil   {
// handle error
}


for   _, symbol   :=   range   symbols {
// check if symbol.Name matches tls.Conn Read/Write
}
```


However, Cilium’s[eBPF](https://github.com/cilium/ebpf) package for Go makes this check much more ergonomic via its` link` subpackage:


```text
ex, _   :=   link.  OpenExecutable  (targetPath)
up, _   :=   ex.  Uprobe  (  "crypto/tls.(*Conn).Read"  , ebpfEntryProg,   nil  )
// returns: link.Link
```


---


## What About` uretprobe` ?


Using a` uretprobe` is practically unusable for Go applications due to the reasons outlined above. So how do we work around this limitation? To answer that means getting our hands a little dirty with some assembly code, mostly so we can locate any` RET` instruction. The goal is to create our own “custom” function return probes just for the Go application we want to monitor.


Let’s take a quick peek at the disassembled contents of the` crypto/tls.(*Conn).Read` symbol in a Go ELF binary via a partial output of` objdump -d` :


```text
0000000000174560 <crypto/tls.(*Conn).Read>:
174560:       f9400b90        ldr     x16, [x28, #16]
174564:       eb3063ff        cmp     sp, x16
174568:       54001aa9        b.ls    1748bc <crypto/tls.(*Conn).Read+0x35c>  // b.plast
17456c:       f8190ffe        str     x30, [sp, #-112]!
174570:       f81f83fd        stur    x29, [sp, #-8]
174574:       d10023fd        sub     x29, sp, #0x8
174578:       f90033ff        str     xzr, [sp, #96]
17457c:       f9003fe0        str     x0, [sp, #120]
174580:       f90047e2        str     x2, [sp, #136]
174584:       f90043e1        str     x1, [sp, #128]
174588:       3900bfff        strb    wzr, [sp, #47]
17458c:       f9001bff        str     xzr, [sp, #48]
174590:       a9047fff        stp     xzr, xzr, [sp, #64]
174594:       d503201f        nop
174598:       d0000e81        adrp    x1, 346000 <go:itab.*crypto/internal/fips140/aes.CBCEncrypter,crypto/cipher.BlockMode+0x8>
17459c:       91214021        add     x1, x1, #0x850
1745a0:       90001ee2        adrp    x2, 550000 <runtime.mheap_+0x189a0>
1745a4:       91100042        add     x2, x2, #0x400
... snipped ...
174650:       f9401be0        ldr     x0, [sp, #48]
174654:       aa1f03e1        mov     x1, xzr
174658:       aa1f03e2        mov     x2, xzr
17465c:       f85f83fd        ldur    x29, [sp, #-8]
174660:       f84707fe        ldr     x30, [sp], #112
174664:       d65f03c0        ret
174668:       f9001bff        str     xzr, [sp, #48]
17466c:       f90023e0        str     x0, [sp, #64]
174670:       f90027e1        str     x1, [sp, #72]
174674:       f9401be3        ldr     x3, [sp, #48]
174678:       aa0103e2        mov     x2, x1
17467c:       aa0003e1        mov     x1, x0
174680:       aa0303e0        mov     x0, x3
174684:       f85f83fd        ldur    x29, [sp, #-8]
174688:       f84707fe        ldr     x30, [sp], #112
```


If you look closely, there’s a specific opcode to take note of: the` ret` instruction. This happens at several points in the` Read` function (and also the` Write` function). More importantly though is that we can use this to our advantage and mimic the behavior of a` uretprobe` by attaching a simple` uprobe` at each` ret` instructions based on its offset from the function symbol.


First, let’s take a look at how we can programmatically discover` ret` instructions when we want to monitor a Go application in our user-space program. There are two Go packages that can be extremely helpful here, depending on the architecture you are targeting:` golang.org/x/arch/x86/x86asm` for amd64 binaries and` golang.org/x/arch/arm64/arm64asm` for arm64 binaries.


The approach for both is largely the same, differing only slightly in how opcode size accounting is done. Specifically, arm64 opcodes are always 4 bytes whereas the amd64 (x86_64) opcodes are variable 1-15 bytes. In both cases, we’ll need to find the data of the symbol’s section in the ELF binary:


```text
import   "  debug/elf  "


func   getELFSymbolData  (  binaryPath  ,   symbolName   string  ) ([]  byte  ,   err  ) {
f, err   :=   elf.  Open  (binaryPath)
if   err   !=   nil   {
return   err
}
defer   f.  Close  ()


symbols, err   :=   f.  Symbols  ()
if   err   !=   nil   {
return   err
}


for   _, symbol   :=   range   symbols {
// "crypto/tls.(*Conn).Read" or "crypto/tls.(*Conn).Write"
if   symbol.Name   !=   symbolName {
continue
}


section   :=   f.Sections[symbol.Section]
sectionData, err   :=   section.  Data  ()
if   err   !=   nil   {
return   err
}


// the symbol is part of a larger section, so we need to get the just for that symbol
start   :=   symbol.Value   -   section.Addr
end   :=   start   +   symbol.Size


return   sectionData[start:end]
}
}
```


Now comes the architecture-depenedent disassembly work to find the offsets of the` ret` instructions. Starting with arm64, which has 4-byte opcodes as mentioned earlier:


```text
import   "  golang.org/x/arch/arm64/arm64asm  "


func   getArm64ReturnOffsets  (  data   []  byte  ) []  int   {
var   offsets []  int


for   i   :=   0  ; i   <   len  (data); i   +=   4   {
instruction, err   :=   arm64asm.  Decode  (data[i : i  +  4  ])
if   err   !=   nil   {
continue
}


if   instruction.Op   ==   arm64asm.RET {
offsets   =   append  (offsets, i)
}
}


return   offsets
}
```


Moving on to amd64 (x86_64), whose opcode sizes are variable 1-15 bytes:


```text
import   "  golang.org/x/arch/x86/x86asm  "


func   getAmd64ReturnOffsets  (  data   []  byte  ) []  int   {
var   offsets []  int


for   i   :=   0  ; i   <   len  (data); {
instruction, err   :=   x86asm.  Decode  (data[i:],   64  )
if   err   !=   nil   {
continue
}


i   +=   instruction.Len


if   instruction.Op   ==   x86asm.RET {
offsets   =   append  (offsets, i)
}
}


return   offsets
}
```


Again, pretty much the same, but you’ll have to know the architecture of the ELF binary you’re dealing with. That’s easily done by checking` .Machine` of the file returned by` elf.Open()` . It will be either` elf.EM_AARCH64` for arm64 or` elf.EM_X86_64` for amd64.


Now that we have these offset values, what do we do with them? Once again, Cilium’s[ebpf](https://github.com/cilium/ebpf) package makes this very easy. Putting everything together to attach both our function entry` uprobe` and the` ret` specific ones, we might have something that looks like the following:


```text
import   (
"  debug/elf  "


"  github.com/cilium/ebpf  "
"  github.com/cilium/ebpf/link  "
)


var   (
uprobeEntryProgram   *  ebpf  .  Program
uprobeExitProgram    *  ebpf  .  Program
)


func   attachGo  (  binaryPath   string  ) ([]  link  .  Link  ,   error   {
var   links []  link  .  Link


ex, err   :=   link.  OpenExecutable  (binaryPath)
if   err   !=   nil   {
return   links, err
}


// attach the entry uprobe
l, err   :=   ex.  Uprobe  (  "crypto/tls.(*Conn).Read"  , uprobeEntryProgram,   nil  )   // or crypto/tls.(*Conn).Write
if   err   !=   nil   {
return   links, err
}
links   =   append  (links, l)


// get RET offsets
data, err   :=   getELFSymbolData  (binaryPath,   "crypto/tls.(*Conn).Read"  )
if   err   !=   nil   {
return   links, err
}


f, err   :=   elf.  Open  (binaryPath)
if   err   !=   nil   {
return   links, err
}
defer   f.  Close  ()


var   offsets []  int


switch   f.Machine {
case   elf.EM_AARCH64:
offsets, err   =   getArm64ReturnOffsets  (data)
if   err   !=   nil   {
return   links, err
}
case   elf.EM_X86_64:
offsets, err   =   getAmd64ReturnOffsets  (data)
if   err   !=   nil   {
return   links, err
}
default  :
return   links, fmt.  Errorf  (  "invalid architecture:   %v  "  , f.Machine)
}


// process each RET offset
for   _, offset   :=   range   offsets {
opts   :=   &  link  .  UprobeOptions  {
Offset:   uint64  (offset),
}


l, err   :=   ex.  Uprobe  (  "crypto/tls.(*Conn).Read"  , uprobeExitProgram, retOpts)
if   err   !=   nil   {
continue
}


links   =   append  (links, l)
}


return   links,   nil
}
```


And we’re done! We’ve now attached our TLS probes at the entry *and* return of` *tls.Conn.Read()` . Keep in mind though that the demonstration above will attach to all running processes of the` binaryPath` argument. You can limit this on a per-PID basis by specifying the` PID` attribute of` UprobeOptions` .


---


## But Wait, There’s More (C Code)!


So far you’ve only seen a rough skeleton of some eBPF C code followed by some Go code to attach` uprobe` programs to a specific binary, but that’s only a partial picture. The real work is on the eBPF side and the actual probes themselves. The approach is largely formulaic: in the function call` uprobe` , grab any pointers you need after the function returns and store them in a map, then in the function return get those points from the map and process them as you need to. Let’s start with the function call` uprobe` for the` Read()` function, keeping in mind that we are limiting our program to support` ABIInternal` binaries built with Go >= 1.18 (through Go 1.24):


```text


#include   <bpf/bpf_helpers.h>
#include   <bpf/bpf_tracing.h>


struct   {
__uint  (type, BPF_MAP_TYPE_HASH);
__type  (key, __u64);
__type  (value,   uintptr_t  );
__uint  (max_entries,   1024  );    // or whatever suits your needs
} go_read_map   SEC  (  ".maps"  );


SEC  (  "uprobe/go_tls_conn_read"  )
int   BPF_UPROBE  (go_tls_conn_read_call)
{
// store the pointer to the []byte argument for later. refer to the abi specification for details on
// register layout: https://go.googlesource.com/go/+/refs/heads/master/src/cmd/compile/abi-internal.md
uintptr_t   byte_slice_ptr   =   (  uintptr_t  )  GO_REGS_PARM2  (ctx);


// identifier
__u64 pid_tgid   =   bpf_get_current_pid_tgid  ();


bpf_map_update_elem  (  &  go_read_map,   &  pid_tgid,   &  byte_slice_ptr, BPF_ANY);


return   0  ;
}
```


The function call` uprobe` is really just about setting up for the function return. One think you may have noticed here is` GO_REGS_PARM2` ; that’s not in any kernel eBPF header nor is it in any` libbpf` header. This` GO_REGS_PARM2` macro is a utility I’ve setup to simplify getting the correct register depending on the architecture my eBPF program is running on. Here’s what it looks like:


```text
#include   <bpf/bpf_helpers.h>


#ifdef   __TARGET_ARCH_x86
#define   __GO_PARM1_REG   ax
#define   __GO_PARM2_REG   bx
#endif


#ifdef   __TARGET_ARCH_arm64
#define   __GO_PARM1_REG   __PT_PARM1_REG
#define   __GO_PARM2_REG   __PT_PARM2_REG
#endif


#define   GO_REGS_PARM1  (  x  ) (  __PT_REGS_CAST  (x)->__GO_PARM1_REG)
#define   GO_REGS_PARM2  (  x  ) (  __PT_REGS_CAST  (x)->__GO_PARM2_REG)
```


Moving on, the real work lies in processing the return. At this point, we need to retrive the` \[\]byte` pointer we stored when the function was called and process the bytes that were read or written, an amount obtainable via the first return value` n` . Since Go allows multiple return values, we can’t rely on the` libbpf` macro` PT_REGS_RET` . Instead, it’s more reliable to refer to Go’s` ABIInternal` spec and pull the return values we need from registers (or the stack). Luckily for us, this is retrievable via the registers` R0` for arm64 and` RAX` for amd64 (x86_64):


```text
SEC  (  "uprobe/go_tls_conn_read"  )
int   BPF_UPROBE  (go_tls_conn_read_return)
{
// identifier
__u64 pid_tgid   =   bpf_get_current_pid_tgid  ();


uintptr_t  *   byte_slice_ptr   =   bpf_map_lookup_elem  (  &  go_read_map,   &  pid_tgid);
if   (  !  byte_slice_ptr)
return   0  ;


// get the return value "n". only process if there were more than 0 bytes
size_t n   =   (size_t)  GO_REGS_PARM1  (ctx);
if   (n   <=   0  )
return   0  ;


// process the data


return   0  ;
}
```


Processing the data becomes a little more involved. eBPF programs have a limited stack size by design, so doing any sort of data processing on the eBPF side is out of the question. In this situation the path forward means sending data from your kernel-space eBPF program to a user-space one that doesn’t have the same set of strict limitations. This is generally done with the use of a specific type of eBPF map:` BPF_MAP_TYPE_PERF_EVENT_ARRAY` . Although the name can be misleading since we aren’t dealing with data and not events, it can be leveraged by eBPF programs in order to send larger amounts of data from kernel-space to user-space. With that in mind, we can configure a perf event map that will allow us to send chunks of TLS data to user-space.


```text
struct   {
__uint(  type  ,   BPF_MAP_TYPE_PERF_EVENT_ARRAY  );
__type(  key  ,   __u32  );
__type(  value  ,   struct   chunk  );
__uint(  max_entries  ,   1024  );    // or whatever you need
} go_tls_chunk_map   SEC  (  ".maps"  );
```


A caveat to this is that perf event arrays have a key size limited to 32 bits, which can hold` pid` or` tgid` , but not both. For the sake of simplicity, let’s just assume that we’re only monitoring a single PID that isn’t handling multiple concurrent TLS operations (supporting this would involve` struct chunk` including additonal context to identify the specific stream and whether it was a` Read()` or a` Write()` ).


So why am I referring to the map value as a “chunk”? Recall that eBPF programs are tightly sandboxed with strict requirements on what they can and cannot do. One of those things is dynamically allocate memory, so using` malloc()` is out of the question. As it turns out, there’s a map “trick” to this: create a per-CPU array map that pre-allocates a single` struct chunk` containing a` char` array of a known size:


```text
struct   {
__uint(  type  ,   BPF_MAP_TYPE_PERCPU_ARRAY  );
__type(  key  ,   __u32  );
__type(  value  ,   struct   chunk  );
__uint(  max_entries  ,   1  );
} chunk_map   SEC  (  ".maps"  );
```


The idea is to create chunks of data and send them to user-space. As a side note, this can be a bit of a hurdle by itself since in older kernels (<= v6.4), the eBPF verifier needed to know that a program being loaded would actually terminate or not. This meant that` for` loops needed to be bounded to a maximum number of iterations that can be determined by the verifier. This isn’t the case in kernel versions 6.4 and later that support[open coded iterators](https://docs.kernel.org/bpf/bpf_iterators.html#open-coded-bpf-iterators) . The details of handling the iteration are outside the scope of this post, so we’ll assume that what we have is acceptable by the eBPF verifier.


Let’s proceed into processing our data in the return probe. We have the pointer to the` \[\]byte` function argument, the return value` n` either being the number of bytes read or the number of bytes written, a perf event array to push chunks to, and a single entry per-CPU array of pre-allocated space to hold data chunks. Here’s what we do:


```text
#define   CHUNK_SIZE   1024
#define   MAX_CHUNKS   512


SEC  (  "uprobe/go_tls_conn_read"  )
int   BPF_UPROBE  (go_tls_conn_read_return)
{
// identifier
__u64 pid_tgid   =   bpf_get_current_pid_tgid  ();


uintptr_t*   byte_slice_ptr   =   bpf_map_lookup_elem  (  &  go_read_map,   &  pid_tgid);
if   (  !  byte_slice_ptr)
return   0  ;


// get the return value "n". only process if there were more than 0 bytes
size_t   n   =   (  size_t  )  GO_REGS_PARM1  (ctx);
if   (n   <=   0  )
return   0  ;


// process the data
__32 num_chunks   =   MIN  ((n   /   CHUNK_SIZE)   +   ((n   %   CHUNK_SIZE)   >   0   ?   1   :   0  ), MAX_CHUNKS);


// get pre-allocated chunk buffer
__u32 zero   =   0  ;
struct   chunk  *   c   =   bpf_map_lookup_elem  (  &  chunk_map,   &  zero);
if   (  !  c)
return   0  ;


__u32 i   =   0  ;
__u32 offset   =   0  ;


// reference
void*   data   =   (  void*  )byte_slice_ptr;


for   (i   =   0  ; i   <   num_chunks; i  ++  ) {
int   chunk_size   =   MIN  (n   -   (i   *   sizeof  (c->buf)),   sizeof  (c->buf));
if   (chunk_size   <=   0  )
break  ;


c->len   =   chunk_size;


if   (  bpf_probe_read_user  (c->buf, chunk_size, data   +   offset)   <   0  )
break  ;


if   (  bpf_perf_event_output  (ctx,   &  go_tls_chunk_map, BPF_F_CURRENT_CPU, c,   sizeof  (  struct   chunk))   <   0  )
break  ;


offset   +=   chunk_size;
}


return   0  ;
}
```


And we’re done! At this point we should be able to consume the plaintext payloads in our user-space Go application:


```text
import   (
"  binary  "
"  os  "


"  github.com/cilium/ebpf  "
"  github.com/cilium/ebpf/perf  "
)


func   readPerfEvents  (  map   *  ebpf  .  Map  )   error   {
perfReader, err   :=   perf.  NewReader  (  map  , os.  Getpagesize  ())
if   err   !=   nil   {
return   err
}


var   record   perf  .  Record


for   {
if   err   :=   perfReader.  ReadInto  (  &  record); err   !=   nil   {
return   err
}


if   len  (record.RawSample)   ==   0   {
continue
}


b   :=   bytes.  NewReader  (record.RawSample)


var   chunk   generatedPkg  .  Chunk


if   err   :=   binary.  Read  (b, binary.LittleEndian,   &  chunk); err   !=   nil   {
continue
}


// do what you wish with the chunk
}
}
```


---


## Wrapping Up


To summarize what we’ve covered about the TLS capture process for Go applications:


1. Go applications present a unique challenge for eBPF TLS capture.
2. Supporting two different Go ABIs is tricky, but limiting support to Go >= 1.18 (through Go 1.24) simplifies things.
3. Go applications you want to monitor **must** be unstripped.
4. A simple` uprobe` works fine but not a` uretprobe` . For the TLS symbols you want to monitor (` crypto/tls.(*Conn).Read` or` crypto/tls.(*Conn).Write` ), disassemble the symbol’s data and walk through it to find` RET` instructions. Attach a` uprobe` at the offsets of those` RET` instructions relative to the beginning of the function.
5. Setup a pointer handoff from the function entry` uprobe` to the return` uprobe` .
6. Retrieve the stored pointer in the return` uprobe` and then process the data by creating chunks and sending it to user-space by way of a perf event array.
7. Pull entries from the perf event array in user-space and process them as you need to.


Instrumenting Go applications with eBPF for TLS capture is significantly more involved than the typical OpenSSL approach, but it’s far from impossible. The primary source of the complexity stems from Go’s design decisions (e.g. static linking, dynamic stack sizing), that prioritize performance and operational simplicity over instrumentation convenience.


The techniques covered here represent somewhat of an agreement between the eBPF world and the Go runtime. The hurdles are unlikely to change, and yes, you’ll need to handle some disassembly and manual probe attachment, but the core patterns are reusable across different Go applications.


That having been said, the juice is worth the squeeze if you need deep visibility into TLS traffic without the use of a proxy-based solution. Just remember: Go applications you want to monitor must be unstripped.
