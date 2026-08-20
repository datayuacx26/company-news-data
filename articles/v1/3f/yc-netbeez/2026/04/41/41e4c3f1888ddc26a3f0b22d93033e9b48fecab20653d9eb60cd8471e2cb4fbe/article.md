---
schema_version: "1.0.0"
document_id: "41e4c3f1888ddc26a3f0b22d93033e9b48fecab20653d9eb60cd8471e2cb4fbe"
company_key: "yc-netbeez"
company: "NetBeez"
source_id: "yc-netbeez-rss-1a1c74f0723d"
canonical_url: "https://netbeez.net/blog/testing-mptcp-with-iperf3/"
published_at: "2026-04-29T19:56:05+00:00"
first_seen_at: "2026-07-20T23:24:05.097992+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:422462580cf4272fa86063ea1a54b2d5891347af829a9e08dcebbf2667ef9b43"
---

# Testing Multi-Path TCP (MPTCP) with iPerf3

We’ve covered many aspects of[iPerf on our blog](https://netbeez.net/blog/?s=iperf) , and recently I found that that iPerf3 version 3.19 added native Multi-Path TCP (MPTCP) support. In this post we’ll explain what MPTCP is, why it matters, and walk through a hands-on demo using two Raspberry Pis to show its resilience in action.


## **What is MPTCP?**


Multi-Path TCP is an extension to standard TCP (defined in[RFC 8684](https://www.rfc-editor.org/rfc/rfc8684) ) that allows a single TCP connection to use multiple network paths at the same time.


With regular TCP, a connection is tied to a single pair of IP addresses. If that path degrades or fails, the connection drops. MPTCP solves this by splitting one logical connection across multiple physical paths, each as a separate “subflow.”


The main benefits are:


- **Resilience** – if one path fails, traffic shifts to another without dropping the connection
- **Throughput aggregation** – on truly independent paths, bandwidth can be combined
- **Better resource utilization** – traffic shifts dynamically toward less congested paths


The protocol was developed by Olivier Bonaventure and his team at[UCLouvain in Belgium](https://perso.uclouvain.be/olivier.bonaventure/blog/html/index.html) , and its first major real-world deployment came from Apple. Many people use Siri while walking or driving. As they move farther away from a WiFi access point, the TCP connection used by Siri to stream voice eventually fails, resulting in error messages. To address this, Apple has been using MPTCP since iOS 7 — when a user issues a Siri voice command, iOS establishes a connection over both WiFi and cellular, so if WiFi drops, the connection hands over to cellular seamlessly as described on this video about[MPTCP at Apple](https://www.youtube.com/watch?v=BucQ1lfbtd4&t=533s) .


MPTCP is now used on all iPhones to provide seamless handovers and improve performance for Siri, Apple Music, and other applications. This deployment has also encouraged 3GPP to adopt MPTCP for the ATSSS service, which will allow future 5G smartphones to seamlessly switch between WiFi and cellular networks. Cloudflare has also written about[how it is changing connectivity](https://blog.cloudflare.com/multi-path-tcp-revolutionizing-connectivity-one-path-at-a-time/) more broadly. On the infrastructure side, the Linux kernel has supported MPTCPv1 natively since kernel 5.6 (2020).


## **Installing iPerf3 3.19 or Later from Source**


One way to demonstrate MPTCP is by using iPerf3 3.19 or later. Most Linux distributions lag behind on this. Debian Bookworm’s repository ships iPerf3 3.12, so you need to build from source.


First install the kernel headers matching your running kernel. This step is required for MPTCP to be detected at compile time:


```text
apt install linux-headers-$(uname -r) git build-essential
```


Then clone and build:


```text
git clone https://github.com/esnet/iperf.git
cd iperf
git checkout 3.21
./configure
make
make install


```


Verify MPTCP support was compiled in:


```text
iperf3 --help | grep mptcp
-m, --mptcp               use MPTCP rather than plain TCP
```


If the ‘–mptcp’ flag appears, you’re good.


You also need a kernel with MPTCP support enabled:


```text
iperf3 --help | grep mptcp
-m, --mptcp               use MPTCP rather than plain TCP
```


Raspberry Pi OS Bookworm (kernel 6.6+) has MPTCP enabled by default. Older Raspbian kernels do not.


## **Lab Setup**


For this demo we used two Raspberry Pi 4s running Raspberry Pi OS Bookworm, each with a wired (eth0) and wireless (wlan0) interface on the same 172.31.0.0/24 network:


**Device** **eth0** **wlan0**


Client 172.31.0.133 172.31.0.173


Server 172.31.0.218 172.31.0.237


## **Configuring MPTCP Endpoints**


MPTCP needs to know which local interfaces to use as subflow endpoints. These settings are not persistent across reboots, so a reboot will cleanly reset them if something goes wrong.


**On the client (172.31.0.133):**


```text
ip mptcp endpoint flush
ip mptcp endpoint add 172.31.0.133 dev eth0 subflow
ip mptcp endpoint add 172.31.0.173 dev wlan0 subflow
ip mptcp limits set subflows 2 add_addr_accepted 4
```


**On the server (172.31.0.218):**


```text
ip mptcp endpoint flush
ip mptcp endpoint add 172.31.0.133 dev eth0 subflow
ip mptcp endpoint add 172.31.0.173 dev wlan0 subflow
ip mptcp limits set subflows 2 add_addr_accepted 4
```


The ‘signal’ ’flag on the server tells MPTCP to advertise the server’s additional addresses to the client via the ADD_ADDR option. This allows the client to open subflows to both server interfaces.


## **Running the Test**


Start the server with an explicit IPv4 bind address. Without ‘-B’, iPerf3 defaults to an IPv6 listener and MPTCP on Linux currently only supports IPv4:


```text
iperf3 -s -B 0.0.0.0
```


On the client, open two terminals. In the first, monitor MPTCP subflow events:


```text
ip mptcp monitor
```


In the second, run the test:


```text
iperf3 -c 172.31.0.218 -t 10 --mptcp
```


## **Watching MPTCP Negotiate Subflows**


As soon as the connection starts,’ip mptcp monitor’ shows the subflow negotiation in real time:


```text
$>ip mptcp monitor
[       CREATED] token=26d472c7 remid=0 locid=0 saddr4=172.31.0.133 daddr4=172.31.0.218 sport=56420 dport=5201
[   ESTABLISHED] token=26d472c7 remid=0 locid=0 saddr4=172.31.0.133 daddr4=172.31.0.218 sport=56420 dport=5201
[     ANNOUNCED] token=26d472c7 remid=2 daddr4=172.31.0.237 dport=5201
[SF_ESTABLISHED] token=26d472c7 remid=0 locid=2 saddr4=172.31.0.173 daddr4=172.31.0.218 sport=58871 dport=5201 backup=0 ifindex=3
```


Step by step:


1. The main subflow is created and established over eth0: 172.31.0.133 to 172.31.0.218
2. The server advertises its wlan0 address (172.31.0.237) via ADD_ADDR
3. A second subflow is established over the client’s wlan0: 172.31.0.173 to 172.31.0.218


Two independent paths are now active for a single TCP connection.


## **The Failover Demo**


With the test running, we brought down wlan0 on the client at around the 3-second mark:


```text
ip link set wlan0 down
```


Here is the iPerf3 output:


```text
$>iperf3 -c 172.31.0.218 -t 10 --mptcp
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-1.00   sec   113 MBytes   947 Mbits/sec   93    208 KBytes
[  5]   1.00-2.00   sec   110 MBytes   924 Mbits/sec   66    212 KBytes
[  5]   2.00-3.00   sec   108 MBytes   904 Mbits/sec   75    209 KBytes
[  5]   3.00-4.00   sec  34.4 MBytes   288 Mbits/sec   22    192 KBytes  <-- wlan0 down
[  5]   4.00-5.00   sec  85.2 MBytes   715 Mbits/sec    0    342 KBytes  <-- recovering
[  5]   5.00-6.00   sec   110 MBytes   925 Mbits/sec    0    407 KBytes  <-- fully recovered
[  5]   6.00-7.00   sec   110 MBytes   919 Mbits/sec    0    421 KBytes
[  5]   7.00-8.00   sec   109 MBytes   914 Mbits/sec    0    427 KBytes
[  5]   8.00-9.00   sec   109 MBytes   918 Mbits/sec    0    430 KBytes
[  5]   9.00-10.00  sec   110 MBytes   919 Mbits/sec    0    431 KBytes


- - - - - - - - - - - - - - - - - - - - - - - - -


[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-10.00  sec   998 MBytes   837 Mbits/sec  256            sender
```


At second 3, throughput dropped from around ~900 Mbits/sec to 288 Mbits/sec. MPTCP detected the subflow loss, retransmitted in-flight data, and shifted all traffic to the surviving eth0 subflow. Within two seconds the connection was back to full speed, without dropping.


With regular TCP, taking down the active interface would have killed the connection outright.


## **A Note on Bandwidth Aggregation**


You might expect MPTCP to show higher throughput than regular TCP since it uses two interfaces. In our lab it did not. MPTCP averaged 837 Mbits/sec compared to 937 Mbits/sec for regular TCP. Both interfaces share the same upstream switch, so there are no truly independent paths to aggregate. MPTCP also adds some overhead for managing subflows and resequencing data.


Bandwidth aggregation with MPTCP requires genuinely independent paths, for example a wired connection and a cellular link on separate ISPs. The resilience benefit however works even on a shared LAN, as shown above.


## **Conclusion**


MPTCP support in iPerf3 3.19 makes it straightforward to test and validate MPTCP deployments on Linux. The setup requires a kernel with MPTCP enabled (Linux 5.6+), kernel headers installed before building iPerf3 from source, and endpoint configuration via \`ip mptcp\`.


The main takeaway from this demo is that MPTCP’s value in most deployments is not bandwidth aggregation but connection resilience. A two-second dip and full recovery is a very different outcome from a dropped connection, which is why Apple, Cloudflare, and a growing number of network operators are deploying it.


Get your free trial


now


Monitor your network from the user perspective


You can share


[Twitter](http://twitter.com/share?url=https://netbeez.net/blog/testing-mptcp-with-iperf3/)[Linkedin](https://www.linkedin.com/sharing/share-offsite/?url=https://netbeez.net/blog/testing-mptcp-with-iperf3/)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://netbeez.net/blog/testing-mptcp-with-iperf3/) Copy link


Link copied
