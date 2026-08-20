---
schema_version: "1.0.0"
document_id: "1e4ce207c3b5f53c0b071b03580d402954987c1440717e7049e98ecdbe5bfb22"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/accelerating-builds-improve-ec2-boot-time"
published_at: "2025-05-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:a3c60f633832cd0ce351a139a981841293208591480e1f16cda1b60bd7a88575"
---

# Accelerating builds: Improving EC2 boot time from 4s to 2.8s

We at Depot like making shit fast, whether that's Docker image builds, Github Actions runners, Bazel caching, Turborepo, or even[our own infrastructure.](https://depot.dev/blog/github-actions-breaking-five-second-barrier)


Recently, we've been working to make EC2 instances boot faster. Why? Because the faster we can boot an EC2 instance, the faster we can get your build running. We've previously tackled other optimizations around this problem like[warming our EBS volumes](https://depot.dev/blog/faster-ec2-boot-time) , and it's a fantastic read if you're looking to make a huge impact on boot times (no bias, of course).


But this recent round of optimizations brought us down deep into the boot process to analyze what we could remove that is consuming precious milliseconds of boot time. An important thing to note, this article was written with Ubuntu 24.04 in mind. But, the general approach should apply to 22.04 and future versions as well.


[Depot builds Depot, so we ship fast. Obviously 😎 Check out what we’ve shipped recently. View the changelog →](https://depot.dev/changelog)


## Analyzing EC2 boot time with systemd-analyze


### What is consuming time during boot?


Knowing is half the battle. To know what to optimize, we need to know what's slow. One of the simplest and most powerful tools for this is` systemd-analyze` . It's a tool that allows us to view what systemd units are taking the most time to start up.


It also allows us to see what services are blocking other services from starting up. Let's start by running the command without arguments:


```text
root@depot-machine   ~#   systemd-analyze
Startup   finished   in   1.784s   (kernel) + 6.999s (  userspace  ) = 8.784s
graphical.target   reached   after   6.959s   in   userspace.
```


This tells us that the kernel took 1.784 seconds to boot, and the userspace took 7 seconds to boot. This is a total of ~8.8 seconds. This is a good starting point, but we want to know more about what is taking so long in userspace.


### Exploring the critical chain and boot graph


To do that, we'll take advantage of two commands:` systemd-analyze critical-chain` and` systemd-analyze plot` . The` systemd-analyze critical-chain` will show us the critical chain of services that are blocking other services from starting up. While this is incredibly useful for finding large bottlenecks, it doesn't give us the whole story.


We also want to know what services are taking the most time to start up. To do that, we'll use` systemd-analyze plot` , which generates an SVG that shows us a graph of all the services that are starting up and how long they took to start.


We also need to know what systemd target we want to optimize for. In our case, we care the most about how long it takes for our network to come up, since that's the point where you can connect to BuildKit for our Docker image builders or our GitHub Actions runners. Because of that, we'll be optimizing for the` network-online.target` . This is the target that is reached when the network is up and running.


Let's check the` critical-chain` and` plot` for that target:


```text
root@depot-machine   ~#   systemd-analyze   critical-chain   network-online.target
The   time   when   unit   became   active   or   started   is   printed   after   the   "@"   character.
The   time   the   unit   took   to   start   is   printed   after   the   "+"   character.


network-online.target   @3.993s
└─systemd-networkd-wait-online.service   @2.142s   +1.848s
└─systemd-networkd.service   @1.937s   +197ms
└─network-pre.target   @1.927s
└─ufw.service   @1.898s   +27ms
└─local-fs.target   @1.840s
└─run-user-1001.mount   @3.380s
└─local-fs-pre.target   @962ms
└─systemd-tmpfiles-setup-dev.service   @941ms   +19ms
└─systemd-tmpfiles-setup-dev-early.service   @855ms   +80ms
└─kmod-static-nodes.service   @699ms   +98ms
└─systemd-journald.socket   @634ms
└─system.slice   @525ms
└─-.slice   @526ms
```


```text
root@depot-machine   ~#   systemd-analyze   plot   >   /tmp/boot.svg
```


This gives us a full view of the services and their time to start to reach the state of` network-online` . With this in hand, we know where to start looking.


We see there is 1.8 seconds of kernel boot time and 4 seconds of userspace boot time. We also see that the network-online.target is taking 1.8 seconds to start, which is a good place to start looking.


We'll start by going through some of the more generic optimizations we can do, then get more specific and detailed.


## Improving disk performance at boot


### File system optimizations


We can begin by looking at our disks, since every service we load is inevitably going to interact with it. Taking a look at our` /etc/fstab/` we see the following:


```text
root@depot-machine   ~#   cat   /etc/fstab
LABEL  =  cloudimg-rootfs     /          ext4     discard,commit=30,errors=remount-ro       0   1
LABEL  =  BOOT        /boot     ext4      defaults          0   2
LABEL  =  UEFI        /boot/efi         vfat      umask=0077        0   1
```


It looks like we're running ext4 on both our root and boot disks. There are a ton of options for how the kernel interacts with ext4, so it can be difficult to know where to start.


Thankfully, the[Arch Linux wiki](https://wiki.archlinux.org/title/Ext4#Improving_performance) provides an excellent starting point. The first thing we can do is disable access time updates. This is a feature that updates the last access time of a file every time it's read. This can be useful for a small percentage of applications, but for most running on an ephemeral runner, it's not necessary. We can disable this by adding the` noatime` option to our` fstab` entry for the root and boot disks:


```text
root@depot-machine   ~#   cat   /etc/fstab
LABEL  =  cloudimg-rootfs     /          ext4     discard,commit=30,errors=remount-ro,noatime       0   1
LABEL  =  BOOT        /boot     ext4      defaults,noatime          0   2
LABEL  =  UEFI        /boot/efi         vfat      umask=0077        0   1
```


Now, let's check in on our old friend` systemd-analyze` and see if this has made any difference:


```text


network-online.target   @3.765s
```


This has saved us a pretty decent 200ms! For those of you with a keen eye, you'll notice that our Github Actions runners already have the` discard` and` commit` options set. Both help to improve disk performance.


There are also options like setting` barrier` to 0 or disabling journaling, which could help to improve write performance. However, as these options wouldn't necessarily help improve boot times, where read performance is more important, we skip those for now.


> **As an aside:**` rclone mount` is incredibly useful for editing files on many remote file systems, including SSH servers. By setting the` --vfs-cache-mode writes` option, you can edit files on the remote server as if they were local. I use it throughout the article to edit files on this EC2 instance.


[Make Your Docker Builds 40x Faster. Run your builds on our optimized infrastructure with zero configuration. Get started →](https://depot.dev/pricing)


### Disable fsck as ephemeral build machines don't need it


The next thing we can do is skip` fsck` when booting. This is a feature that checks the file system for errors before booting. This is useful for some systems, but again, ephemeral machines like these don't care about things like disk corruption 🙂. We can do this by adding the` fsck.mode=skip` option to our grub config:


```text
root@depot-machine   ~#   cat   /boot/efi/EFI/ubuntu/grub.cfg
terminal_input   console
terminal_output   console
set   menu_color_normal=white/black
set   menu_color_highlight=black/light-gray


insmod   gzio
insmod   part_gpt
insmod   lvm
insmod   ext2
search   --no-floppy   --label   --set=root   BOOT
linux     /vmlinuz   root=LABEL=cloudimg-rootfs    mitigations=off   audit=0   pci=nocrs   nvme_core.io_timeout=4294967295   nopti   console=ttyS0   fsck.mode=skip
initrd    /microcode.cpio   /initrd.img
boot
```


Then, all we need to do is run` update-grub` to apply the changes.


This isn't the only place we run` fsck` , however. Taking a look at the plot SVG from earlier, you'll notice that systemd also runs` fsck` while booting.


systemd runs fsck while booting


This is a service that runs` fsck` on all of the disks before booting. We can disable this by masking this service:


```text
systemctl   mask   systemd-fsck-root.service
```


Notably, the` initramfs` is the program that checks the file system for grub, so we'll need to check both kernel and userspace start times:


```text
root@depot-machine   ~#   systemd-analyze
Startup   finished   in   1.533s   (kernel) + 6.484s (  userspace  ) = 8.018s
graphical.target   reached   after   5.277s   in   userspace.
```


This is a pretty small but notable improvement from where we started!


## Removing snaps to reduce boot overhead


Snaps are a package management system created by Canonical, designed to be cross-distro and easy to install without having to worry about dependency management. They're also the bane of anyone who cares about boot times.


Their cold start times are very slow; due to a combination of decompressing an entire squashfs file system, mounting said file system, and loading in every dependency of the snap. You can read a little more about this[here](https://forum.snapcraft.io/t/squashfs-performance-effect-on-snap-startup-time/13920) .


As you can see, snaps take up a lot of boot time! Even with each snap mounting in parallel, it takes around 30 milliseconds to mount the squashfs, and 300 milliseconds to get the loop back devices ready!


Each snap mounts a squashfs file system at boot, so removing all snaps should help to improve boot times. Let's take a look at what snaps we have installed:


Checking what snaps are installed


```text
root@depot-machine   ~#   snap   list
Name                Version      Rev      Tracking           Publisher     Notes
amazon-ssm-agent    3.3.987.0    9881     latest/stable/…    aws✓          classic
core22              20250210     1802     latest/stable      canonical✓    base
snapd               2.67.1       23771    latest/stable      canonical✓    snapd
```


This lines up with what we're seeing in the plot. The core22 snap is the base for all snaps, and snapd is of course the snap daemon. We also need the amazon-ssm-agent snap, since it's used to connect to the EC2 instance for this example.


What we can do in this case is install the amazon-ssm agent as a regular deb package. That way, we can remove all snaps and still have the agent installed:


```text
snap   remove   amazon-ssm-agent
curl   -LO   https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
sudo   dpkg   -i   amazon-ssm-agent.deb
```


This will remove the snap and install the deb package. This is a pretty small change, but it should help to improve boot times a little bit. Next, we can remove the core22 and snapd packages, since we don't need them. Since there won't be any snaps to mount at boot, this should save us around 300 milliseconds:


```text
# remove all core
snap   list   --all   |   grep   -v   'snapd'   |   awk   'NR>1 {print $1}'   |   xargs   -I  {} snap remove {}


# remove snapd
snap   list   --all   |   awk   'NR>1 {print $1}'   |   xargs   -I  {} snap remove {}
root@depot-machine   ~#   systemd-analyze
Startup   finished   in   1.429s   (kernel) + 5.405s (  userspace  ) = 6.834s
graphical.target   reached   after   5.385s   in   userspace.
```


apparmor service time to start


As you can see, the loop device mounting is gone! However, problems still remain: Taking a look at the plot again, apparmor is taking around 400 ms to start:


## Handling AppArmor and on-demand snap services


App Armor is a security module for the kerne that helps to restrict what programs can do. In this case, it's used by snaps for permissions. While this is an important security consideration for regular servers, however, we have a different security posture when it comes to ephemeral machines like ours. One way we can approach this is to disable the apparmor service, but start it automatically when snap or other applications ask for it:


```text
# first, disable snap and app armor
systemctl   disable   snapd.socket
systemctl   disable   snapd.service
systemctl   disable   snapd.seeded.service
systemctl   disable   snapd.apparmor.service
systemctl   disable   apparmor.service


# Create a systemd socket unit that will listen and activate the service when needed
cat   >   /etc/systemd/system/snapd-on-demand.socket   <<   EOF
[Unit]
Description=Socket activation for snapd service


[Socket]
ListenStream=/run/snapd.socket
SocketMode=0666


[Install]
WantedBy=sockets.target
EOF


# Create the corresponding service that will be started on-demand
cat   >   /etc/systemd/system/snapd-on-demand.service   <<   EOF
[Unit]
Description=Snap Daemon (on-demand)
After=network.target
Requires=snapd.apparmor.service apparmor.service


[Service]
ExecStart=/usr/lib/snapd/snapd
Type=simple


[Install]
WantedBy=multi-user.target
EOF
systemctl   daemon-reload
systemctl   enable   —-now   snapd-on-demand.socket
```


## Disabling unnecessary systemd services


Since we're deep in the world of systemd anyways, I have a lightning round list of services we can disable to help improve boot times. These are all services that are either not needed or can be started on-demand:


- ` systemctl mask systemd-journal-flush.service` : takes a ridiculous 5 seconds to start, and we don't need to worry about the journal disappearing on an ephemeral machine.
- ` systemctl mask lvm2-monitor.service` : this service monitors Logical Volume Management (LVM) devices for changes. We use a static LVM config, so this has no benefit
- ` systemctl mask multipathd.service` : the multipath daemon manages redundant physical paths to storage devices. Since we don't use multipath storage, this service is unnecessary and can be safely disabled.
- ` systemctl mask multipathd.socket`


### Userspace-level unnecessary systemd services


While figuring out why` systemd-networkd-wait-online` was taking so long, I learned about an OS feature called[duplicate address detection](https://www.ibm.com/support/pages/what-duplicate-address-detection) . It effectively does what it sounds like, it verifies that IPv6 home addresses are unique on the local link before assigning them to an interface


While it can be incredibly useful for some servers, it's likely[only harming](https://www.agwa.name/blog/post/beware_the_ipv6_dad_race_condition) your boot times on cloud instances. Disabling it with systemd is pretty straightforward. This will disable IPv6 duplicate address detection for all network interfaces. :


```text
mkdir   -p   /etc/systemd/network/10-netplan-pseudo.network.d
echo   -e   '[Network]\nIPv6DuplicateAddressDetection=0\n'   >>
/etc/systemd/network/10-netplan-pseudo.network.d/override.conf
```


Another check of our boot times:


```text


network-online.target   @2.779s
└─systemd-networkd-wait-online.service   @1.914s   +861ms
└─systemd-networkd.service   @1.579s   +327ms
└─network-pre.target   @1.571s
...
```


That's another 600 ms shaved off our times!


## Removing cloud-init and replacing with static netplan


Another userspace optimization is to disable a feature called[cloud-init](https://cloudinit.readthedocs.io/en/latest/index.html) . It queries Amazon's metadata service to automatically set the host name and configure networks. While this is useful for some systems, it's just not necessary in our case, and adds to the boot time. We can also automatically configure our netplan config so that` cloud-init` is unnecessary:


```text
# /etc/netplan/50-cloud-init.yaml
network  :
ethernets  :
pseudo  :
optional  :   true
dhcp4  :   true
dhcp6  :   false
accept-ra  :   false
mtu  :   1500
match  :
name  :   en*
version  :   2
```


We've been giving systemd so much love that we've barely tackled other parts of boot! One area to claw back milliseconds from is the` initramfs` . It handles setting up devices on the system before deferring over to the rest of the boot process. We discovered that the` initramfs` was doing quite a bit of work involving udev rules, and made sure to remove any rules in` /usr/lib/udev/rules.d/` that weren't necessary for boot, such as` 66-azure-ephemeral.rules` .


## Where we're looking to optimize next


We managed to improve our boot times from 4 seconds to 2.8 seconds focusing on the detailed optimizations above.


But we're always looking for the next few milliseconds we can squeeze out. We have some theories around where we are going to look next after this most recent optimization round.


### Kernel boot time: What's still taking 1.7 seconds?


For most of this article, we barely dug into the ~1.7 seconds of kernel boot time.


There's a number of ways to approach this, but a relatively easy one is compiling the kernel with different flags. Every module compiled into the kernel adds boot time, whether from loading the bytes from that module from the disk, preparing and activating the module, or loading it dynamically as needed by userspace applications.


If you know which modules you can do without, compiling them out isn't too tedious of a task.


### Mo initramfs, mo problems


We took our first pass at optimizing initramfs, but we only scratched the surface there with our basic rule disabling. The reality is that initramfs does quite a bit of work during boot, including setting up devices, loading modules, etc. We think there are additional opportunities to optimize this further, maybe even booting the kernel[without initramfs at all](https://firasuke.github.io/DOTSLASHLINUX/post/booting-the-linux-kernel-without-an-initrd-initramfs/) .


## FIN


We've managed to improve boot times by focusing heavily on the OS side of things, implementing common advice like setting` noatime` , all the way up to entirely removing snap and cleaning up udev rules. This is just a fraction of the work we've been doing at Depot to help get your builds running even faster.


If you want to see how smoothly Depot fits into your workflow, check out our[integrations](https://depot.dev/integrations) . Or if you’re ready to try it yourself,[get started here](https://depot.dev/sign-up) .


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Billy Batista


Software Engineer at Depot
