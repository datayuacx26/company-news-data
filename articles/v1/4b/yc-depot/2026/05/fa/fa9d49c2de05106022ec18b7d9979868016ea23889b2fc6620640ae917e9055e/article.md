---
schema_version: "1.0.0"
document_id: "fa9d49c2de05106022ec18b7d9979868016ea23889b2fc6620640ae917e9055e"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/optimizing-microvm-boot-times"
published_at: "2026-05-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:14:13.187831+00:00"
content_hash: "sha256:e4c483b18dfa38014ead42ad8cecd515e131e57345330092d9fa3342a87cdf40"
---

# How we got microVMs booting in under a second

## Prologue


We built a just-in-time VM scheduler for[Depot CI](https://depot.dev/products/ci) , which means VMs only start when a build request actually arrives. As you might expect, fast boot times are essential to provide a stellar experience for our users. There's no pre-warming and no warm pool of standby VMs. Adding to the complexity, our short-lived sandboxes run on the same VMs that power our long-running builds. In this post, I'll walk you through the optimizations we layered on to get microVM cold starts down to where they are today.


## Baseline and evaluation


Our VMM of choice is Cloud Hypervisor (v51.1.0) running on KVM. The host is an i7i.metal-24xl bare metal instance running Debian 13. For the guest, we use the latest Ubuntu cloud image, converted to qcow2. This qcow2 image is attached to the VM as a disk (with Direct I/O) and serves as the boot device. A second disk, an ISO file, holds the cloud-init configuration. Inside the guest, a small agent receives and executes commands from the host over vsock. For our measurements, we'll consider a VM "sufficiently booted" once it can execute` /usr/bin/date` . By that point, the network is also up and running.


To conduct the measurements, we built a small helper that interacts with our VM agent. It starts an 8-core, 16GB RAM VM and runs` /usr/bin/date` against it. A measurement looks like this:


```text
$   time   helper   exec   "/usr/bin/date"
Tue   Apr   28   13:14:24   UTC   2026


real      0m8.026s
user      0m0.004s
sys       0m0.002s
```


By wrapping the helper in` time` , we get end-to-end boot time. Our unoptimized baseline lands between 7 and 9 seconds.


## Direct kernel boot


The vanilla Ubuntu image is far from optimized. It loads kernel modules, drivers, and filesystems we don't strictly need, and each one costs us milliseconds at boot. Cloud Hypervisor supports direct kernel boot, which looks like an obvious win. All we need to do is compile a minimal kernel with only the modules we actually use. Cloud Hypervisor's[kernel config](https://github.com/cloud-hypervisor/linux/blob/ch-6.12.8/arch/x86/configs/ch_defconfig) is a reasonable starting point, and we trim it down further from there. Passing the resulting kernel via the` --kernel` flag at VM start, let's see how much time we save:


```text
$   time   helper   exec   "/usr/bin/date"
Tue   Apr   28   13:20:02   UTC   2026


real      0m4.751s
user      0m0.010s
sys       0m0.004s
```


After several runs, we can confidently say boot time now lands between 3 and 5 seconds.


## Ask systemd for help


To dig into where boot time is actually spent, systemd ships with a few useful tools:


```text
# prints a list of all running units, ordered by the time they took to initialize
systemd-analyze   blame
# prints a tree of the time-critical chain of units
systemd-analyze   critical-chain
```


Together, they expose the slowest units and processes during boot. For our VM,` systemd-analyze` reports:


```text
Startup   finished   in   193ms   (kernel) + 3.375s (  userspace  ) = 3.568s
These   are   the   top   ones:
systemd-analyze   blame
1.482s   cloud-init-local.service
580ms   cloud-config.service
352ms   lvm2-monitor.service
348ms   cloud-init.service
252ms   dev-vda1.device
250ms   cloud-final.service
138ms   networkd-dispatcher.service
113ms   ssh.service
92ms   systemd-logind.service
78ms   user@1000.service
75ms   systemd-udevd.service
73ms   keyboard-setup.service
```


Looking at critical chain paints a similar picture:


```text
graphical.target   @2.676s
└─multi-user.target   @2.676s
└─networkd-dispatcher.service   @2.537s   +138ms
└─basic.target   @2.509s
└─sockets.target   @2.509s
└─uuidd.socket   @2.509s
└─sysinit.target   @2.507s
└─cloud-init.service   @2.159s   +348ms
└─cloud-init-local.service   @662ms   +1.482s
└─systemd-journald.socket   @242ms
└─system.slice   @218ms
└─-.slice   @218ms
```


Cloud-init is the biggest offender, and we'll deal with it in the next section. For now, let's focus on the other services.


### systemd services


We aren't using LVM, so we can disable it and save 352ms:


```text
systemctl disable lvm2-monitor.service
systemctl mask lvm2-monitor.service
```


We also don't need the network dispatcher, since we aren't relying on network hooks. That's another 138ms:


```text
systemctl   disable   networkd-dispatcher.service
```


While not applicable to this image, there are a few other systemd services worth disabling by default on our VMs:


```text
# Multipath (for SAN storage, not needed in VMs)
systemctl disable multipathd.service
systemctl mask multipathd.service


# Apport (Ubuntu crash reporting)
systemctl disable apport.service
systemctl mask apport.service


# Grub (we boot with an external kernel)
systemctl disable grub-common.service
systemctl mask grub-common.service grub-initrd-fallback.service


# e2scrub (ext4 online scrubbing, not needed for ephemeral VMs)
systemctl disable e2scrub_reap.service
systemctl mask e2scrub_all.timer e2scrub_reap.service


# rsyslog (redundant when using journald)
systemctl disable rsyslog.service
```


Not strictly a systemd service, but since our VMs don't support IPv6, we can set


```text
dhcp6: false
```


in the netplan config so the system doesn't wait for DHCPv6 during boot.


With these changes baked into our base image, let's rerun the VM:


```text
$   time   helper   exec   "/usr/bin/date"
Tue   Apr   28   13:45:54   UTC   2026


real      0m4.050s
user      0m0.091s
sys       0m0.004s
```


## Optimizing cloud-init


After a modest ~0.7 second reduction, cloud-init is still the major contributor to userspace boot time. The ISO has to be mounted, the configuration read, and the resulting actions applied. Let's see how much of that we can trim:


First, we narrow the datasource list to just` NoCloud` so cloud-init doesn't iterate through every cloud provider trying to detect its environment:


```text
datasource_list: [ NoCloud ]
```


Next, we run` cloud-init analyze show` , which produces a detailed breakdown of module timings. Based on that, here are our optimization candidates:


Module Time Notes


**config-ssh** 1.28s SSH host key generation


**config-grub_dpkg** 0.2s Unnecessary with external kernel


As an extra, we can also remove the following configs from` /etc/cloud/cloud.cfg` :


- ` growpart`
- ` resizefs`
- ` grub_dpkg`
- ` apt_configure`
- ` locale`
- ` ssh_authkey_fingerprints`


The biggest win here is skipping host key generation on every VM boot. That said, I wouldn't recommend reusing the same host key across all VMs. A better approach is to maintain a pool of pre-generated unique host keys and assign one to each VM at boot, but that's an implementation detail for another post.


Let's re-measure with the cloud-init optimizations in place:


```text
$   time   helper   exec   "/usr/bin/date"
Tue   Apr   28   13:56:02   UTC   2026


real      0m3.022s
user      0m0.001s
sys       0m0.000s
```


## But… Do we really need cloud-init?


Optimizing cloud-init bought us roughly a second, but it's still taking a considerable chunk of boot time. And really, all we need is a lightweight way to hand a small config to the guest-agent. So why not replace cloud-init entirely?


Enter the[Firmware Configuration Device](https://github.com/cloud-hypervisor/cloud-hypervisor/blob/main/docs/fw_cfg.md) : a QEMU-compatible device that lets the host pass data directly to the guest operating system. It's a promising fit, though it takes a few steps to wire up.


Cloud Hypervisor needs to be rebuilt with this feature enabled:


```text
cargo build --features fw_cfg
```


Our kernel config needs a new option:


```text
CONFIG_FW_CFG_SYSFS=y
```


With both in place, the guest can read the passed data directly from sysfs:


```text
/sys/firmware/qemu_fw_cfg/by_name/opt/org.example/config/raw
```


With cloud-init and its ISO disk out of the picture:


```text
$   time   helper   exec   "/usr/bin/date"
Tue   Apr   28   14:16:02   UTC   2026


real      0m2.294s
user      0m0.003s
sys       0m0.001s
```


## Ditching systemd


Systemd does a lot of useful things, but it also does plenty of things we don't actually need. We're building a stripped-down, highly optimized guest, not a general-purpose OS. So let's consider what it would take to replace systemd entirely:


1. Set up the rootfs
2. Mount the necessary devices
3. Mount any disks specified in the configuration
4. Configure the network
5. Reap zombie processes
6. Set the hostname
7. Start any services specified in the configuration (e.g. the guest-agent)
8. Anything else not strictly required for boot


Items 1 through 5 are the bare minimum we need at boot, and the ones we can optimize most aggressively. So we're sold on replacing systemd, but how do we actually get our init binary into the VM?


### To initramfs, or not to initramfs?


We have two obvious options:


- Drop our init binary into` /sbin/init` in the guest
- Use an initramfs, which is a single file we can hand to Cloud Hypervisor via a flag


For Depot CI, we went with the initramfs approach. It enables quick iterations and keeps init system deployment decoupled from the rest of the guest image.


### Parallel initialization


The great thing about owning the init process is that we control exactly what runs and when. That opens the door to running boot steps in parallel where it makes sense, for example:


- Setting the hostname
- Starting the Docker daemon
- Setting up swap
- Starting sshd
- Setting kernel parameters
- And so on


Let's see if ditching systemd was worth it:


```text
$   time   helper   exec   "/usr/bin/date"
Tue   Apr   29   12:14:02   UTC   2026


real      0m1.041s
user      0m0.002s
sys       0m0.001s
```


## Low-hanging fruit


The numbers are starting to look good! Let's see if we can squeeze out a few more milliseconds here and there.


### Kernel command line


Even though we control the init process, the kernel itself still does a few things at boot that we can trim.


#### Logs


Adding the following options to the kernel cmdline silences boot output, saving a few milliseconds that would otherwise be spent writing to the console:


```text
quiet   loglevel=0
```


#### Time


These flags tell the kernel to use the KVM paravirtualized clock and skip a couple of timer sanity checks that would otherwise add overhead during boot:


```text
clocksource  =  kvm-clock   no_timer_check   tsc=reliable
```


### Serial console


We can also disable the serial console entirely so the kernel doesn't waste cycles on output we never read.


Time to check our boot numbers again:


```text
$   time   helper   exec   "/usr/bin/date"
Tue   Apr   29   14:47:02   UTC   2026


real      0m0.939s
user      0m0.004s
sys       0m0.002s
```


### Hugepages and pre-allocate


We're now under the magical 1-second mark, but there's still room to push further.


Allocating hugepages on the host and using them as backing memory for the VM can meaningfully reduce page faults and allocation overhead. We went with 1GB pages, and the benefit grows with the amount of memory the VM actually uses.


Before settling on hugepages, we experimented with Cloud Hypervisor's prefault option, which allocates physical memory and sets up page tables before the VM boots. It improves build performance but takes a real toll on boot speed, so we decided against it. Hugepages give us a better balance.


```text
$   time   helper   exec   "/usr/bin/date"
Tue   Apr   29   14:47:02   UTC   2026


real      0m0.789s
user      0m0.003s
sys       0m0.001s
```


## vhost-user-blk is fun


In practice, boot times aren't constant. Our P50 sits around 0.6 seconds, while our P90 can climb to 1.2 seconds.


Since we started supporting[snapshots](https://depot.dev/docs/ci/how-to-guides/custom-images) , we've had to rethink how we serve root disks at scale, moving beyond plain qcow2 files. We now store VM root disks and snapshots in the[Depot Registry](https://depot.dev/docs/registry/overview) in an OCI-compatible way. Disk chunks are cached on the hosts in a multi-tier setup, and any missing chunks are served on-demand from the registry. This architecture has some impact on boot times today, but we have promising improvements in the pipeline that should let us strike a better balance between fast cold boots and storage pressure. That's a topic worth its own blog post.


We're also experimenting with VM memory snapshot and restore, which could push boot times down even further.


Thanks for following along to learn why starting a build on Depot CI feels so snappy. Until next time!


## FAQ


What's the biggest single optimization for microVM boot times?


Direct kernel boot, by a wide margin, going from 7 to 9 seconds down to 3 to 5 seconds. Cloud Hypervisor lets you pass a kernel directly via` --kernel` , and when you do, you can swap the stock Ubuntu kernel for one compiled from a minimal config. That cuts out drivers, modules, and filesystems you don't actually need, and each one was costing milliseconds at boot.


How do you use the QEMU firmware configuration device to replace cloud-init?


Cloud Hypervisor supports the same fw_cfg device that QEMU uses. You rebuild Cloud Hypervisor with` cargo build --features fw_cfg` , add` CONFIG_FW_CFG_SYSFS=y` to your kernel config, and the guest reads host-provided data from` /sys/firmware/qemu_fw_cfg/by_name/opt/org.example/config/raw` . The guest-agent reads its config from there on startup, so cloud-init and its ISO disk are out of the picture entirely.


If you skip SSH host key generation during boot, how do you keep keys unique per VM?


The post touches on this: generating keys at boot is the slow part, but reusing the same key across all VMs is a bad idea. A better approach is to maintain a pool of pre-generated unique host keys and assign one to each VM at boot. It turns a boot-time problem into a provisioning problem, and the details are worth their own post.


## Related posts


- [The differences between QEMU microvm and Cloud Hypervisor](https://depot.dev/blog/differences-between-qemu-and-cloud-hypervisor)
- [Accelerating builds: Improving EC2 boot time from 4s to 2.8s](https://depot.dev/blog/accelerating-builds-improve-ec2-boot-time)
- [Pulling containers faster with eStargz](https://depot.dev/blog/booting-containers-faster-with-estargz)


Peter Hasko-Nagy


Staff Engineer at Depot
