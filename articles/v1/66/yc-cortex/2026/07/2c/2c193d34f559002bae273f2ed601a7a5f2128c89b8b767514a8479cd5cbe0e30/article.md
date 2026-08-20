---
schema_version: "1.0.0"
document_id: "2c193d34f559002bae273f2ed601a7a5f2128c89b8b767514a8479cd5cbe0e30"
company_key: "yc-cortex"
company: "Cortex"
source_id: "yc-cortex-news-import-d57bc7656b70"
canonical_url: "https://builders.cortex.io/blog/sandboxing-agents-part-1/"
published_at: "2026-07-09T16:30:00+00:00"
first_seen_at: "2026-07-21T15:05:12.617246+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:98e59cdafdf4e7debed88e74c2a49706245ebb07ec734c9148b34dc1652a6802"
---

# Sandboxing AI agents: Building a microVM

## Why an AI agent needs a sandbox


An AI coding agent is, by design, a thing that runs commands you didn’t write.


The security implications are horrifying! Exploits can be as “mild” as` rm -rf /` up to hijacking your employee credentials. Despite strides in recent frontier models where complete slipups are rarer, the agent is still a black box — and impressionable to prompt injection from the outside world.


Typically, coding agents deal with this in two ways:


1. Explicitly ask the user if the command or class of command is safe to run, *every time it wants to run something*
2. Have the user upfront allowlist commands or classes of commands.


Both approaches are non-starters for a true[software factory](https://builders.cortex.io/blog/software-factories-are-operating-models/) , where we want to scale out to thousands of agents, with as little human intervention as possible.


There is a combinatorial explosion in the space of all possible commands (up to length` n` ), and a similarly massive space of all “good” and bad” commands (if that’s even well defined) such that we cannot *possibly* allow / deny a flat set of commands up front.


The solution at scale is to give each agent its own sandbox: a real throwaway Linux box it can wreck, with strict egress policies to prevent exfiltration. Treat each agent as potentially adversarial and eliminate the blast radius completely by completely limiting what I/O it is permitted to do:


- If it wants to run` rm -rf /` , by all means go ahead
- If there’s a supply chain attack in NPM, that’s fine — it literally cannot pull new packages


## Firecracker and MicroVMs


The agent runs untrusted commands, so the sandbox has one job: *contain the blast radius.* Your options:


- A container shares the host kernel. A kernel bug or a` --privileged` slip and the agent is loose on the host — and on every other agent’s files. Fine for code you trust; wrong threat model for code an LLM just wrote.
- A cloud VM isolates hard, but boots in tens of seconds and bills you for every idle minute. You can’t spin one up per agent-task and stay solvent.
- **A microVM** is the middle path: a stripped-down VM with a minimal device model that boots in ~125 ms, isolated by the same hardware virtualization a full VM uses. It’s what Firecracker — the engine under AWS Lambda — was built for.


Firecracker is a virtual machine monitor (VMM) that is extremely lightweight, and what many platforms, including our own software factory,` engrams` , use to cheaply spin up microVM sandboxes at scale.


It uses Linux KVM, an open source kernel module that exposes the hardware virtualization features of modern CPUs to user space.


Where Firecracker shines over hypervisors like QEMU is that it has a minimal device model, and does not emulate a full PC. It only exposes the devices that are necessary to run a Linux guest, such as a virtual CPU, memory, and a few essential devices like a serial console and network interface.


This allows Firecracker to run microVMs with minimal overhead and *very* fast boot times, making it ideal for quickly spinning up and tearing down microVMs.


## Let’s build a microVM


A major motivation for building` engrams` for us is understanding every moving part of software factories deeply.


While` engrams` uses Firecracker directly, as it’s production hardened and secure, I wanted to understand how it actually worked under the hood, so that it feels less like magic, and wanted to share those learnings with you all.


This presented a bit of a pedagogical challenge for me, as viewers at home may not have machines with KVM enabled machines to follow along with the code snippets.


So embedded in this article is a live playground that runs a Linux kernel on a software CPU, with toy KVM implementation to illustrate the concepts. Each snippet is editable + runnable, so roll up your sleeves and let’s get virtualizing!


** ** **


sandbox — microVM · v86


idle


host


A real Linux kernel + ext4 rootfs, booted in your browser via a software CPU (v86).


By the end, the whole sandbox reduces to four moving parts you can hold in your head:


1. **Boot** a machine.
2. Give it a **wire** to talk to.
3. Give it a **world** to work in.
4. **Drive** a process inside it.


## Lesson 1: boot a sealed box


The unit of isolation is a microVM, and VMM like Firecracker is what boots one. Underneath, a VMM is barely anything:


1. Open` /dev/kvm`
2. Create a VM
3. Give it some guest RAM
4. Create a virtual CPU
5. ` KVM_RUN`


Our first guest is two instructions — it computes 2 + 2 and stops:


```text
add   al  ,   bl     ; al = 2 + 2
hlt            ; freeze the CPU
```


The host loads those bytes, calls` KVM_RUN` , and waits.


**Before you run it, make a prediction: where does the` 4` come out?**


editable · compiled live by tcc in the guest


not run


```text
#include   <stdint.h>
#include   <stdio.h>
#include   <string.h>
#include   <fcntl.h>
#include   <unistd.h>
#include   <sys/ioctl.h>
#include   <sys/mman.h>
#include   <linux/kvm.h>


int   main  (  void  ) {
// Open `/dev/kvm`
int   kvm   =   open  (  "/dev/kvm"  , O_RDWR   |   O_CLOEXEC);


// Create a VM
int   vmfd   =   ioctl  (kvm, KVM_CREATE_VM,   0  );


// Give it some guest RAM
const   uint8_t   code  []   =   {
0x  00  ,   0x  d8  ,     // add al, bl   (al = 2 + 2)
0x  f4  ,           // hlt
};
void   *  mem   =   mmap  (
NULL  ,
0x  1000  ,
PROT_READ   |   PROT_WRITE,
MAP_SHARED   |   MAP_ANONYMOUS,
-  1  ,
0
);
memcpy  (mem, code,   sizeof  (code));


struct   kvm_userspace_memory_region region   =   {
.slot   =   0  ,
.guest_phys_addr   =   0x  1000  ,
.memory_size   =   0x  1000  ,
.userspace_addr   =   (  uint64_t  )(  uintptr_t  )mem,
};
ioctl  (vmfd, KVM_SET_USER_MEMORY_REGION,   &  region);


// Create a vCPU
int   vcpufd   =   ioctl  (vmfd, KVM_CREATE_VCPU,   0  );
size_t   run_size   =   ioctl  (
kvm,
KVM_GET_VCPU_MMAP_SIZE,
0
);
struct   kvm_run   *  run   =   mmap  (
NULL  ,
run_size,
PROT_READ   |   PROT_WRITE,
MAP_SHARED,
vcpufd,
0
);


struct   kvm_sregs sregs;
ioctl  (vcpufd, KVM_GET_SREGS,   &  sregs);
sregs.cs.base   =   0  ; sregs.cs.selector   =   0  ;
ioctl  (vcpufd, KVM_SET_SREGS,   &  sregs);


struct   kvm_regs regs   =   {
.rip   =   0x  1000  ,
.rax   =   2  ,
.rbx   =   2  ,
.rflags   =   0x  2
};
ioctl  (vcpufd, KVM_SET_REGS,   &  regs);


// KVM_RUN
ioctl  (vcpufd, KVM_RUN,   0  );       // run the guest until it stops


struct   kvm_regs after;
ioctl  (vcpufd, KVM_GET_REGS,   &  after);
return   0  ;
}


```


```text


```


…nothing useful. That’s the point. A freshly-booted microVM is a sealed box: no screen, no keyboard, no network. The guest computed` 4` — but the only way to find out is by freezing it after it halts and reading a register.


` KVM_RUN` runs the guest until it traps back to you; with no I/O, the only trap you ever get is “it halted.” To run an agent in here, you need a live wire to the outside.


## Lesson 2: give it a wire


So let the guest talk *while it runs* . The guest changes by one idea: after computing the answer, it writes a byte to an I/O port.


```text
add al, bl        ; al = 2 + 2
+   add al, '0'       ; to ASCII — '4'
+   out dx, al        ; write it to port 0x3f8
hlt
```


…and the host changes by one idea to match — instead of reading a register after the halt, it loops, and catches the trap the` out` produces:


```text
-   ioctl(KVM_RUN);   // run once, then read a register after it halts
+   for (;;) {
+     ioctl(KVM_RUN);                  // runs until the guest traps back
+     if (exit_reason == KVM_EXIT_IO)  // it touched the port — take the byte
+       fwrite(io_data, 1, io_size, stdout);
+     if (exit_reason == KVM_EXIT_HLT) break;
+   }
```


Before you run it, predict two things: will the host see the byte *before* the guest halts, and who decides what that byte *means* ?


editable · compiled live by tcc in the guest


not run


```text
#include   <stdint.h>
#include   <stdio.h>
#include   <string.h>
#include   <fcntl.h>
#include   <unistd.h>
#include   <sys/ioctl.h>
#include   <sys/mman.h>
#include   <linux/kvm.h>


int   main  (  void  ) {
int   kvm   =   open  (  "/dev/kvm"  , O_RDWR   |   O_CLOEXEC);
ioctl  (kvm, KVM_GET_API_VERSION,   0  );
int   vmfd   =   ioctl  (kvm, KVM_CREATE_VM,   0  );


// 16-bit real-mode guest: compute 2+2,
// OUT '4' to port 0x3f8, OUT '\n', HLT.
const   uint8_t   code  []   =   {
0x  ba  ,   0x  f8  ,   0x  03  ,    // mov dx, 0x3f8
0x  00  ,   0x  d8  ,          // add al, bl
0x  04  ,   '0'  ,           // add al, '0'
0x  ee  ,                // out dx, al
0x  b0  ,   '  \n  '  ,          // mov al, '\n'
0x  ee  ,                // out dx, al
0x  f4  ,                // hlt
};


void   *  mem   =   mmap  (
NULL  ,
0x  1000  ,
PROT_READ   |   PROT_WRITE,
MAP_SHARED   |   MAP_ANONYMOUS,
-  1  ,
0
);
memcpy  (mem, code,   sizeof  (code));


struct   kvm_userspace_memory_region region   =   {
.slot   =   0  ,
.guest_phys_addr   =   0x  1000  ,
.memory_size   =   0x  1000  ,
.userspace_addr   =   (  uint64_t  )(  uintptr_t  )mem,
};
ioctl  (vmfd, KVM_SET_USER_MEMORY_REGION,   &  region);


int   vcpufd   =   ioctl  (vmfd, KVM_CREATE_VCPU,   0  );
size_t   run_size   =   ioctl  (
kvm,
KVM_GET_VCPU_MMAP_SIZE,
0
);
struct   kvm_run   *  run   =   mmap  (
NULL  ,
run_size,
PROT_READ   |   PROT_WRITE,
MAP_SHARED,
vcpufd,
0
);


struct   kvm_sregs sregs;
ioctl  (vcpufd, KVM_GET_SREGS,   &  sregs);
sregs.cs.base   =   0  ; sregs.cs.selector   =   0  ;
ioctl  (vcpufd, KVM_SET_SREGS,   &  sregs);


struct   kvm_regs regs   =   {
.rip   =   0x  1000  ,
.rax   =   2  ,
.rbx   =   2  ,
.rflags   =   0x  2
};
ioctl  (vcpufd, KVM_SET_REGS,   &  regs);


for   (;;) {
ioctl  (vcpufd, KVM_RUN,   0  );
switch   (run->exit_reason) {
case   KVM_EXIT_IO:
if   (run  ->  io.direction   ==   KVM_EXIT_IO_OUT   &&   run  ->  io.port   ==   0x  3f8  ) {
char   *  data   =   (  char   *  )run   +   run->io.data_offset;
fwrite  (data,   1  , run->io.size, stdout);
fflush  (stdout);
}
break  ;
case   KVM_EXIT_HLT:
puts  (  "[guest halted]"  );
return   0  ;
default  :
fprintf  (stderr,   "unexpected exit_reason   %u\n  "  , run  ->  exit_reason  );
return   1  ;
}
}
}


```


```text


```


There’s your` 4` , live, the instant the guest produced it — a byte pushed through a hole in the box.


And nothing more escaped! The guest never touched the host’s terminal. It wrote to a *virtual* port, the CPU trapped out to the host, and the host *chose* to print it. That trap-and-emulate loop is the seed of every device a VMM has.


A full Linux guest turns that same port into a serial console — the terminal you booted up top. But a console is a raw byte stream: fine for logs, awkward for a control plane that has to send a task and read a structured answer back. To drive the machine at scale, you want a real socket:[vsock](https://man7.org/linux/man-pages/man7/vsock.7.html) .


vsock is a host↔guest socket with no networking stack in between — addressed by a context ID and a port. It’s like TCP but point-to-point between the VMM and its guest.


The socket affords the guest and host better ergonomics than a raw port: the guest can` accept` a connection,` read` a command, and` write` the output back.


In` engrams` the guest microVMs all run a small server called` agentd` , at PID 1, that communicates via` vsock` with its Firecracker hosts over a wire protocol — length-prefixed bincode RPCs over the configured transport, with verbs like:` Exec` ,` Stat` ,` StartShell` ,` Ping` ,` Shutdown` , and` SpawnHarness` .


In our toy example the guest server looks something like:


```text
int   s   =   socket  (AF_VSOCK, SOCK_STREAM,   0  );          // not TCP — a host↔guest pipe
bind  (s, { .svm_cid   =   VMADDR_CID_ANY, .svm_port   =   9999   });
listen  (s,   1  );
int   c   =   accept  (s,   0  ,   0  );                            // the orchestrator connects here
read  (c, cmd, ...);                                  // "run this"
/* ...run it... */
write  (c, output, ...);                              // stream the result back
```


editable · compiled live by tcc in the guest


not run


```text
// vsock_demo.c — the control plane over vsock: a guest-side "agentd" that accepts a
// connection, reads a command, runs it, and streams output back. Both ends run in-guest
// over the kernel's vsock loopback (a browser emulator has no host-side vsock device), but
// the server is exactly the production code — in production the client is the host.
#include   <stdio.h>
#include   <string.h>
#include   <unistd.h>
#include   <sys/socket.h>
#include   <sys/wait.h>
#include   <linux/vm_sockets.h>


#ifndef   AF_VSOCK
#define   AF_VSOCK   40
#endif
#define   PORT   9999


static   void   agentd  (  void  ) {                            // the guest-side server
int   s   =   socket  (AF_VSOCK, SOCK_STREAM,   0  );
struct   sockaddr_vm a   =   { .svm_family   =   AF_VSOCK, .svm_cid   =   VMADDR_CID_ANY, .svm_port   =   PORT };
bind  (s, (  struct   sockaddr   *  )  &  a,   sizeof   a);
listen  (s,   1  );


int   c   =   accept  (s,   0  ,   0  );
char   cmd  [  256  ];   int   len   =   0  , n;                    // read the command the client sent
while   ((n   =   read  (c, cmd   +   len,   sizeof   cmd   -   1   -   len))   >   0  ) len   +=   n;
cmd  [len]   =   0  ;


FILE   *  p   =   popen  (cmd,   "r"  );                         // run it; stream output back over vsock
char   buf  [  256  ];
while   (p   &&   fgets  (buf,   sizeof   buf, p))   write  (c, buf,   strlen  (buf));
if   (p)   pclose  (p);
close  (c);   close  (s);
_exit  (  0  );
}


int   main  (  void  ) {
if   (  fork  ()   ==   0  )   agentd  ();                         // start the agent in a child
sleep  (  1  );                                          // give it a moment to listen


int   s   =   socket  (AF_VSOCK, SOCK_STREAM,   0  );          // the client (stands in for the host)
struct   sockaddr_vm a   =   { .svm_family   =   AF_VSOCK, .svm_cid   =   VMADDR_CID_LOCAL, .svm_port   =   PORT };
if   (  connect  (s, (  struct   sockaddr   *  )  &  a,   sizeof   a)   !=   0  ) {   perror  (  "connect"  );   return   1  ; }


const   char   *  task   =   "uname -sm"  ;
write  (s, task,   strlen  (task));
shutdown  (s, SHUT_WR);                              // signal end-of-command
printf  (  "-> sent over vsock:   %s\n  "  , task);
printf  (  "<- agent replied  : "  );
char   buf  [  256  ];   int   n;
while   ((n   =   read  (s, buf,   sizeof   buf))   >   0  )   fwrite  (buf,   1  , n, stdout);
close  (s);
wait  (  0  );
return   0  ;
}


```


```text


```


With both client and server, we have a toy control plane: a request in, a command run, output streamed back, all over a real` AF_VSOCK` socket. Swap the` uname` for the agent, put the client out on the host, and it’s the spine of a sandboxing service.


## Lesson 3: give the agent its world


A wire is no good if there’s nothing on the other end. The rootfs is the agent’s machine — its userland, its tools, its environment. Whatever you put in it is what the agent can reach: the language runtimes,` git` , the repo, the agent CLI itself.


In` engrams` , the user defines their rootfs via Docker images, and the orchestrator flattens that into a block device the microVM can boot:


```text
# flatten the image's layers into a directory, then bake an ext4 straight from it —
# no mount, no root, just filesystem bytes written into a file:
skopeo   copy   docker://your/agent-image   oci:img   &&   oci-unpack   img   rootfs/
mke2fs   -d   ./rootfs   -t   ext4   rootfs.ext4   64M
```


Attach that as the boot disk and the VM comes up inside the agent’s world. This program asks the sandbox what world it woke up in — its kernel, its distro, the tools you baked in:


editable · compiled live by tcc in the guest


not run


```text
// world.c — what world does an agent wake up in?
//
// The rootfs you build *is* the agent's machine: its kernel, its userland, its tools.
// This program asks the sandbox what it's made of. Run it.
#include   <stdio.h>
#include   <string.h>
#include   <unistd.h>
#include   <sys/utsname.h>


int   main  (  void  ) {
struct   utsname u;
uname  (  &  u);
printf  (  "kernel :   %s   %s   (  %s  )  \n  "  , u.sysname, u.release, u.machine);


FILE   *  f   =   fopen  (  "/etc/os-release"  ,   "r"  );
char   line  [  160  ];
while   (f   &&   fgets  (line,   sizeof   line, f))
if   (  !  strncmp  (line,   "PRETTY_NAME="  ,   12  ))
printf  (  "distro :   %s  "  , line   +   12  );     // already has its own newline
if   (f)   fclose  (f);


// The agent's toolbox is whatever you baked into the rootfs. Here it's a shell and a
// C compiler; in production you'd bake node, python, git, the agent CLI...
const   char   *  tools  []   =   {   "/bin/sh"  ,   "/usr/bin/tcc"  ,   "/usr/bin/cc-run"  ,   0   };
printf  (  "tools  :"  );
for   (  int   i   =   0  ;   tools  [i]; i  ++  )
if   (  access  (  tools  [i], X_OK)   ==   0  )   printf  (  "   %s  "  ,   tools  [i]);
printf  (  "  \n  "  );
return   0  ;
}


```


```text


```


In production that toolbox is` node` ,` python` ,` git` , the agent binary. Here it’s a shell and a C compiler — which is how you’ve been compiling everything on this page.


## Lesson 4: drive it


The` agentd` in Lesson 2 ran your command with` popen` .


In` engrams` what our real` agentd` actually does is operate more like a real server:` fork` +` exec` + a pipe.


editable · compiled live by tcc in the guest


not run


```text
// exec.c — the seed of the whole control plane.
//
// To drive a sandbox you need one primitive: run a command inside it and capture what it
// prints. That's fork + exec + a pipe. This is exactly what the orchestrator does to
// launch an agent and stream its output back to your UI. Run it.
#include   <stdio.h>
#include   <unistd.h>
#include   <sys/wait.h>


int   main  (  void  ) {
int   out  [  2  ];
pipe  (out);


if   (  fork  ()   ==   0  ) {                         // child: become the command
dup2  (  out  [  1  ], STDOUT_FILENO);           // wire its stdout into the pipe
close  (  out  [  0  ]);
close  (  out  [  1  ]);
execlp  (  "uname"  ,   "uname"  ,   "-a"  , (  char   *  )  0  );
_exit  (  127  );                            // only reached if exec failed
}


// parent: read everything the command printed
close  (  out  [  1  ]);
printf  (  "captured from the guest:  \n    "  );
char   buf  [  256  ];
ssize_t   n;
while   ((n   =   read  (  out  [  0  ], buf,   sizeof   buf))   >   0  )
fwrite  (buf,   1  , n, stdout);
wait  (  NULL  );
return   0  ;
}


```


```text


```


Putting it all together, this is roughly what is happening at a high level within` engrams` :


1. Firecracker spawns microVMs with KVM, creating a` vsock` connection
2. ` agentd` spawns at PID 1 within the guest, accepting requests
3. Requests come in over` vsock` , and` agentd`` fork` s +` exec` s the requested command, streaming its output back over the wire.


For driving harnesses like Claude Code, there is a small wrapper that translates its JSONL output into standardized harness messages that can be transferred over the` vsock` wire. Similarly, incoming wire messages are sent as` stdin` to the Claude Code process to send messages.


## What’s next


Two things we skipped, each its own post:


- **Density.** You can’t keep thousands of idle agent-VMs resident in RAM. Snapshots, restore, and copy-on-write memory sharing are how you pause an idle agent and resurrect it in milliseconds.
- **Isolation hardening.** We booted on the happy path.` jailer` , seccomp, and an egress proxy are what make it actually safe to run code an LLM wrote against the open internet.
