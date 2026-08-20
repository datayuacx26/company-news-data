---
schema_version: "1.0.0"
document_id: "3a7d85611561ac4d0dfcb224a9a6b64ba472c9d07f61556c0390cab0fcc5bea6"
company_key: "yc-netbeez"
company: "NetBeez"
source_id: "yc-netbeez-rss-1a1c74f0723d"
canonical_url: "https://netbeez.net/blog/ebpf-network-tools-on-linux/"
published_at: "2026-06-24T16:35:55+00:00"
first_seen_at: "2026-07-20T23:24:05.097992+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:45a2c939fd339b23770c14764a91886be9fd315f8bcc3863678cad46858b4f36"
---

# eBPF Network Tools on Linux: A Practical Guide for Network Engineers

If you spend time on Linux systems troubleshooting network problems, you probably reach for the same set of tools every time. top for process resource usage. ss or netstat for current connections. tcpdump or Wireshark when you need to see what is on the wire.


These tools are indispensable, but they each leave a blind spot. top tells you nothing about which process is using the network. ss gives you a snapshot of current connection state but no history, no bytes transferred, no duration. tcpdump captures everything at the packet level but requires you to decode it yourself, and the overhead of copying every packet to userspace adds up fast on a busy host.


What is missing is a live, per-process, per-connection view of network activity with kernel-level accuracy. Something that can tell you not just what is connected right now, but what was happening 30 seconds ago when that latency spike occurred, and which process was responsible.


That is the gap eBPF tools fill.


## **What eBPF Is**


[eBPF](https://www.brendangregg.com/blog/2016-11-30/linux-bcc-tcplife.html) (extended Berkeley Packet Filter) has been part of the Linux kernel since 2014. It is not new, not experimental, and not something you need to compile or patch your kernel to use. It allows small sandboxed programs to run directly inside the kernel in response to events such as network connections, function calls, and system calls, without modifying kernel source code or loading kernel modules.


The key distinction from traditional monitoring tools is where the observation happens.


Traditional tools like tcpdump, nethogs, and netstat work in userspace. To capture data, the kernel has to copy it up from kernel space into your program. At high traffic volumes this copy overhead becomes significant. Tools like tcpdump essentially process every matching packet twice, once in the kernel and once in userspace.


eBPF tools run their measurement logic inside the kernel. They hook directly into the TCP stack, library calls, or kernel functions, aggregate data in kernel space, and only surface summaries to userspace. No packet copies, no polling, near-zero overhead.


A useful analogy: traditional monitoring tools are like asking someone standing outside a building what is happening inside. eBPF is a camera mounted in every hallway.


eBPF programs pass through a kernel verifier before they run. The verifier checks that the program cannot crash the kernel, loop infinitely, or access memory it should not touch. This is what makes eBPF safe to run in production without the risks associated with traditional kernel modules.


## **What eBPF Tools Measure and What They Do Not**


One important boundary to understand before diving in: eBPF tools measure host behavior and performance. They observe what is happening on the Linux system they are running on, which processes are making connections, what the kernel TCP stack is doing, how long DNS lookups are taking for each process.


They are not network probes. They do not send test traffic to remote hosts, measure end-to-end path latency, or tell you anything about what is happening between your host and a destination. For that you still need tools that generate synthetic traffic such as ping, traceroute, or[dedicated monitoring agents](https://netbeez.net/network-monitoring/) .


eBPF tools look inward at the host. Network probes look outward at the path. You need both.


## **Before You Install: Check Your Kernel**


Modern eBPF tools require a Linux kernel with BTF (BPF Type Format) enabled. BTF is the kernel’s self-description of its own data structures. It is what allows pre-compiled eBPF tools to adapt to your specific kernel version without needing to compile anything on your machine.


Check if your kernel supports it:


` $> ls /sys/kernel/btf/vmlinux
/sys/kernel/btf/vmlinux
$> uname -r
6.12.48+deb13-cloud-amd64`


If the first command returns a file path rather than an error, you are good. If it returns “No such file or directory”, the tools in this guide will not work on your kernel.


Any modern Ubuntu (22.04+), Debian (Bookworm+), or cloud Linux instance will pass this check. Kernel 5.8 or later is the practical minimum.


**Note on Raspberry Pi:** the standard Raspberry Pi OS kernel does not enable BTF, which means these tools will not run on stock Pi images regardless of kernel version. This is a known limitation of the Raspberry Pi Foundation’s kernel builds.


## **Installation**


On Debian and Ubuntu the tools are packaged in libbpf-tools. These are the modern CO-RE (Compile Once, Run Everywhere) versions that use BTF and do not require kernel headers or a compiler on your machine:


` sudo apt update && sudo apt install libbpf-tools`


No LLVM, no kernel headers, no compilation. The package installs pre-compiled binaries that adapt to your running kernel at load time using BTF.


Verify it worked:


` sudo tcplife-libbpf`


If it starts without errors you are ready. Hit Ctrl-C and proceed.


## **tcptop: top for TCP Connections**


If you know top, you already understand tcptop. Where top shows CPU and memory usage per process refreshing every second, tcptop shows network bandwidth per TCP connection, also refreshing every second.


` sudo tcptop-libbpf`


Sample output on a quiet host:


` 17:57:09 loadavg: 0.01 0.05 0.07 1/153 1890910
PID COMM LADDR RADDR RX_KB TX_KB
685480 nbagent_prod 172.31.29.48:33965 52.86.107.50:443 0 2
1840637 sshd-session 172.31.29.48:22 99.35.16.139:51313 0 0`


In two lines you can see every active TCP connection on the host, which process owns it, source and destination with ports, and bytes transferred in the current interval. No packet capture, no protocol decoding, no overhead.


The closest traditional Linux equivalent is nethogs, which gives a similar per-process view. The difference is in how they work: nethogs polls /proc/net/tcp on a timer, which means it can miss short-lived connections that open and close between polls. tcptop hooks into the kernel TCP stack via eBPF and is event-driven, so it catches every connection regardless of how briefly it lives.


## **Practical tcptop Examples**


Basic live view, refreshing every second:


` sudo tcptop-libbpf`


Five-second snapshots, 12 times, then exit (60 seconds total):


` sudo tcptop-libbpf 5 12`


The positional arguments are interval and count. This is the most useful form for a defined observation window.


Log to a file for later analysis with screen clearing disabled:


` sudo tcptop-libbpf -C 5 12 | tee /var/log/tcptop-$(date +%Y%m%d-%H%M).log`


The -C flag is important when logging to a file. Without it, tcptop sends terminal clear codes that make the log file unreadable. With it you get clean timestamped snapshots you can grep through afterwards.


Sort by most data sent:


` sudo tcptop-libbpf -s sent 5 12`


Filter to a specific process:


` sudo tcptop-libbpf -p $(pgrep myprocess) 5 12`


The $(pgrep myprocess) resolves the PID inline. Useful when a specific application is under investigation and you want to cut noise from other processes.


## **Watching a Speed Test with tcptop**


Running a network speed test while tcptop is logging is one of the clearest ways to see what the tool actually reveals. The following output was captured with tcptop-libbpf -C 5 12 running continuously while a speed test was launched in a second terminal:


```text
19:54:46 loadavg: 0.00 0.02 0.03 1/141 2095038
PID     COMM         LADDR                 RADDR                RX_KB  TX_KB
1840637 sshd-session 172.31.29.48:22       99.35.16.139:51313         1     1
1994066 nbagent_prod 172.31.29.48:41031    52.86.107.50:443          0      0


19:55:21 loadavg: 0.00 0.02 0.02 1/144 2096040
PID     COMM         LADDR                 RADDR                 RX_KB  TX_KB
2096008 ndt7-client  172.31.29.48:54558    173.205.4.24:443      436533     1
2096008 ndt7-client  172.31.29.48:33408    192.178.155.121:443        6     0
1994066 nbagent_prod 172.31.29.48:41031    52.86.107.50:443           0     0


19:55:31 loadavg: 0.30 0.08 0.05 4/142 2096304
PID     COMM         LADDR                 RADDR                 RX_KB TX_KB
2096008 ndt7-client  172.31.29.48:54558    173.205.4.24:443     1819252     0
2096008 ndt7-client  172.31.29.48:37530    173.205.4.24:443           7428743
1994066 nbagent_prod 172.31.29.48:41031    52.86.107.50:443           0     0


19:55:36 loadavg: 0.36 0.10 0.05 4/135 2096456
PID     COMM         LADDR                 RADDR                 RX_KB TX_KB
1994066 nbagent_prod 172.31.29.48:41031    52.86.107.50:443           0     1
2096008 ndt7-client  172.31.29.48:37530    173.205.4.24:443        24 2209864


```


Reading this snapshot by snapshot:


At 19:54:46 the host is quiet. Only an SSH session and a monitoring agent are visible, both with near-zero traffic.


At 19:55:21 a new process appears: ndt7-client, which is the NDT7 speed test client. It has opened two connections to 173.205.4.24 and is already pulling 436 MB in the current 5-second window. That is the download phase starting.


At 19:55:31 the picture shifts. The download connection is still active at 1.8 GB per interval, but a second connection from the same PID now shows 428 MB of TX. The test has moved into upload phase and is running both directions simultaneously on separate TCP streams.


At 19:55:36 the upload stream is at 2.2 GB per interval, the download has dropped to near zero, and the monitoring agent is still running quietly in the background.


The speed test application reported a single headline number. tcptop shows the underlying mechanics: which process, which connections, which direction, and how the test ramped from download to upload across discrete TCP streams, all logged with timestamps and available in the file for later review.


If you saw 2 GB of outbound traffic on a production host without knowing a speed test was running, tcptop would give you the process name, PID, and destination IP within seconds.


## **gethostlatency: DNS Lookup Latency Per Process**


While tcptop answers which process is using the network and how much bandwidth, gethostlatency answers a different question: which process is making DNS lookups, to what hostnames, and how long are they taking?


` sudo gethostlatency-libbpf`


Sample output from a host running scheduled network monitoring tests:


` TIME PID COMM LATms HOST
17:35:08 1854848 fping5 2.609 google.com
17:35:08 1854851 traceroute 3.670 gmail.com
17:35:11 1854940 curl 9.077 gmail.com
17:35:11 1854971 curl 2.597 teams.microsoft.com
17:35:18 1855166 traceroute 19.725 world.tr.teams.microsoft.com
17:35:26 1855357 dig 0.021 1.1.1.1
17:35:26 1855357 dig 0.002 127.0.0.53
17:35:43 1857463 fping5 3.223 gmail.com
17:35:47 1857547 traceroute 15.486 world.tr.teams.microsoft.com`


Unlike dig, which tells you DNS is working when you run it manually, gethostlatency watches every DNS resolution made by every process on the host continuously. Several things stand out in this output.


**The resolver chain is visible.** Every dig call shows two entries, first 1.1.1.1 (the upstream Cloudflare resolver) and then 127.0.0.53 (the local systemd-resolved stub at sub-millisecond latency). You can see the full resolution path without configuring anything.


**Per-process attribution is automatic.** You can see fping5, traceroute, curl, and dig all making DNS lookups simultaneously. Without gethostlatency you would know DNS was slow but you would not know which process was affected.


**The Microsoft Teams hostname is significantly slower.** Compare these two traceroute DNS lookups taken from the same host at the same time:


` traceroute 3.670ms gmail.comtraceroute 19.725ms world.tr.teams.microsoft.com`


world.tr.teams.microsoft.com consistently resolves in 15-20ms versus 3-4ms for Gmail across every measurement interval. This is a four-hop CNAME chain that Microsoft uses for Teams routing. The complexity of that chain adds real latency before a single data packet leaves the host. This is invisible to ping, invisible to traceroute, and invisible to tcpdump. It only becomes visible when you observe the DNS layer at the application call level.


**DNS cache behavior is visible.** The first resolution of gmail.com at 17:35:11 takes 9ms. By 17:36:11 the same hostname resolves in 4ms because the resolver cache has warmed. You can watch caching behavior in real time across all processes simultaneously.


## **Filtering gethostlatency**


Watch only one specific process:


` sudo gethostlatency-libbpf -p $(pgrep myprocess)`


Watch for a specific hostname across all processes:


` sudo gethostlatency-libbpf | grep "teams.microsoft.com"`


Log Teams DNS latency continuously to a file:


` sudo gethostlatency-libbpf | grep --line-buffered "teams" | tee -a /var/log/teams-dns.log`


This last command is useful during an active investigation. Run it in the background during a problem window and you have a timestamped record of every Teams DNS lookup and its latency for as long as you need.


## **tcplife: The Full TCP Session Log**


tcptop shows what is happening right now. gethostlatency shows DNS. tcplife fills a third gap: it records the complete lifecycle of every TCP session from open to close, with process name, bytes transferred in each direction, and total duration in milliseconds.


` sudo tcplife-libbpf`


Sample output:


` PID COMM LADDR LPORT RADDR RPORT TX_KB RX_KB MS
1234 curl 172.31.29.48 54321 93.184.216.34 443 1.90 7.53 142.33
5678 ssh 172.31.29.48 22 99.35.16.139 51313 0.40 0.20 3600000`


Each line appears when a session closes. You see the process that opened it, both endpoints, total bytes in each direction, and how long the session lasted.


This is what netstat and ss fundamentally cannot give you. They show current state, not history. If a backup job opened a connection, transferred 500 MB, and closed it, netstat would never have captured it unless you happened to be watching at exactly that moment. tcplife logs every session automatically.


## **How These Tools Compare to Traditional Alternatives**


**What you want to know** **Traditional tool** **Limitation** **eBPF tool**


Which process is using bandwidth? nethogs Polls /proc, can miss short connections tcptop


What connections existed? ss / netstat Snapshot only, no history tcplife


Why is DNS slow? dig One lookup at a time, no process attribution gethostlatency


What went over the wire? tcpdump High overhead, no process attribution tcplife + tcptop


## **Limitations Worth Knowing**


**Linux only.** eBPF is a Linux kernel technology. Microsoft has an eBPF for Windows project but it is explicitly not production-ready as of mid-2025. macOS has no equivalent. These tools run on Linux servers, VMs, and cloud instances.


**Kernel version matters.** You need kernel 5.8 or later with BTF enabled. Any modern cloud Linux instance has this. Older or embedded kernels may not.


**Host visibility only.** These tools see what the host is doing. They do not measure network paths, WAN latency, or anything between your host and a destination. They complement network probes rather than replace them.


**Root access required.** All of these tools need sudo. This is the same requirement as tcpdump, so no more privileged than tools you are likely already using.


## **Getting Started**


The full install on any BTF-enabled Linux host:


#` verify your kernel supports BTFls /sys/kernel/btf/vmlinux
# install the toolssudo apt install libbpf-tools
# live bandwidth per process, refreshing every secondsudo tcptop-libbpf
# DNS lookup latency per processsudo gethostlatency-libbpf
# full TCP session log with duration and bytessudo tcplife-libbpf`


The tools have been available for years. If you have been curious about eBPF but assumed it required kernel programming, this is the entry point.


Get your free trial


now


Monitor your network from the user perspective


You can share


[Twitter](http://twitter.com/share?url=https://netbeez.net/blog/ebpf-network-tools-on-linux/)[Linkedin](https://www.linkedin.com/sharing/share-offsite/?url=https://netbeez.net/blog/ebpf-network-tools-on-linux/)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://netbeez.net/blog/ebpf-network-tools-on-linux/) Copy link


Link copied
