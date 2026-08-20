---
schema_version: "1.0.0"
document_id: "d73c88cb74d915acb4cc84d62b0727d5b7979644d5385c8884a09b46c27a9df5"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/debugging-hetzner-uncovering-failures-with-powerstat-sensors-and-dmidecode"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:1ebbdb7c88c59a76abbd39a1649e05c9372effc4ea3c7943531a972427c6c7d5"
---

# Debugging Hetzner: Uncovering failures with powerstat, sensors, and dmidecode

## Debugging Hetzner: Uncovering failures with powerstat, sensors, and dmidecode


February 17, 2025 · 5 min read


Burak Yucesoy


Principal Software Engineer


At Ubicloud, we build software that turns bare metal providers into cloud platforms. One of the providers we like is Hetzner because of their affordable and reliable servers.


About a year ago, Hetzner launched the AX162 server line. It offers better performance and a lower price than its predecessor, AX161. We were very excited to adopt it, but we soon encountered serious reliability issues. We observed that the new servers were 16 times more likely to crash. After months of debugging and working with Hetzner, the solution only came after several hardware updates. Although the journey was painful, we learned a lot from it and wanted to share our experience.


### What Happened?


Three weeks after purchasing our first AX162 server, one of the servers crashed. We checked the system logs and found NULL bytes. These usually mean there was an abrupt failure, like a power loss, which stopped the system from finishing its writing process. Hetzner performed a hardware check but found nothing unusual. A week later, we experienced another crash, followed by several more over the next few days.


In the days that followed, the crash frequency increased. For each crash, Hetzner checked the hardware. Sometimes, they found a defect and replaced the server. Other times, they found nothing unusual. We contacted Hetzner about the frequent crashes, but it was hard to find a clear cause.


At this point, we observed a few interesting patterns:


- All crashes occurred on AX162 servers.
- There were two types of crashes:


- The server comes back online after a manual restart.
- The server wouldn't respond to restart requests or diagnostic codes sent by Hetzner engineers. Hetzner would replace the server in these cases.


- The servers usually run smoothly for an extended period. However, once a server experiences its first crash, further crashes become more likely. After the server experiences the first type of crash several times, it would eventually have the second type of crash and be replaced.


### Initial Investigations


We started testing different ideas to find out what caused the crashes.


#### System Load


We considered the possibility of increased load on the machine causing issues. The AX162 machines come with 96 vCPUs, and we had workloads that utilized all of them at the same time. Consistent high load, for example, could lead to increased temperatures and unexpected issues. However, when we reviewed the load levels at the times of crashes, we found several instances where crashes occurred even under low or no load.


#### Temperature


We wanted to check if there is a correlation between high temperatures and crashes. It is possible to collect the temperature of various components in the system with sensors command.


```text
$> sensors
coretemp-isa-0000
Adapter: ISA adapter
Package id 0:  +51.0°C  (high = +100.0°C, crit = +100.0°C)
Core 0:        +45.0°C  (high = +100.0°C, crit = +100.0°C)
Core 4:        +46.0°C  (high = +100.0°C, crit = +100.0°C)
Core 8:        +51.0°C  (high = +100.0°C, crit = +100.0°C)
Core 9:        +51.0°C  (high = +100.0°C, crit = +100.0°C)
Core 10:       +51.0°C  (high = +100.0°C, crit = +100.0°C)
Core 11:       +51.0°C  (high = +100.0°C, crit = +100.0°C)
Core 12:       +49.0°C  (high = +100.0°C, crit = +100.0°C)
Core 13:       +49.0°C  (high = +100.0°C, crit = +100.0°C)
Core 14:       +49.0°C  (high = +100.0°C, crit = +100.0°C)
Core 15:       +49.0°C  (high = +100.0°C, crit = +100.0°C)
```


We wrote a simple cron job to collect temperature data. When the servers crashed again, we checked the data. The temperature levels were not significantly higher than average at the time of the crashes.


#### Faulty Components


Commands like lshw


and dmidecode


are useful to gather information regarding hardware parts, including model and serial numbers.


```text
$> dmidecode -t 2
# dmidecode 3.3
Getting SMBIOS data from sysfs.
SMBIOS 3.3.0 present.
Handle 0x0200, DMI type 2, 8 bytes
Base Board Information
Manufacturer: Dell Inc.
Product Name: 0H3K7P
Version: A08
Serial Number: .51R1H04.MXWSJ0039D004Z.
```


We compared the components of AX162 servers that had crashed with those that hadn’t. We found no significant differences. We even checked how serial numbers increase, because we thought older components might fail more often. But crashes happened even in servers with the latest serial numbers.


#### Power Consumption


Power, rather than space, often limits data center expansion. To increase the number of machines under power constraints, data center operators usually cap power use per machine. However, this can cause motherboards to degrade more quickly. Although we didn’t know if Hetzner was limiting power consumption, the symptoms suggested this might be a factor. Repeated server crashes after a long period of stability usually mean the hardware is wearing out. We also eliminated all other hypotheses we had one by one, which only left power limiting as a strong hypothesis.


With the powerstat


tool, we measured the maximum power consumption over a long period.


```text
$> powerstat -R
Time   User Nice  Sys  Idle   IO Run Ctxt/s  IRQ/s Fork Exec Exit  Watts
14:17:15  3.1  0.0  0.0  96.9  0.0   5    430   1593    0    0    0 166.54
14:17:16  3.1  0.0  0.0  96.9  0.0   5    425   1638    1    1    1 166.51
14:17:17  3.1  0.0  0.0  96.9  0.0   5    570   1737    0    0    0 166.50
14:17:18  3.1  0.0  0.0  96.9  0.0   5    609   1787    0    0    0 166.48
14:17:19  3.1  0.0  0.0  96.9  0.0   5    469   1662    0    0    0 166.49
...


```


We then compared our measurements with the advertised amounts.


Model Advertised Max. Power Consumption (Watt) Measured Max. Power Consumption (Watt)


AX161 147 ([1](https://web.archive.org/web/20240223142827/https://www.hetzner.com/dedicated-rootserver/matrix-ax/) )


168


AX162 408 ([2](https://web.archive.org/web/20240228172003/https://www.hetzner.com/dedicated-rootserver/matrix-ax/) )


266


Based on these numbers, we suspected that Hetzner might indeed be limiting power usage.


#### Data Collection on Crash Rates and Comparison


Although we were observing an increased crash rate, we wanted to support this observation with data. A common way to measure hardware reliability is the Annualized Failure Rate (AFR). It's like the annual run rate, but for component failures. The formula for AFR is:


AFR has its own limitations, but it is simple enough to give us a starting point, so we decided to use it. Here are our initial measurements:


Model Total Failure Count Total Days in Service Annual Failure Rate


AX161 11


3784 1.06


AX162 34


737 16.84


Our observations indicated that AX162 servers are 16 times more likely to experience a failure compared to other models. The data also backed up our first finding: after a server crashes once, it is very likely to crash again. In fact, 80% of servers that crashed once had a second crash within 24 hours


### Observing Stability with New Hardware


We submitted a detailed support ticket with the additional data on power limiting and annualized failure rates. Hetzner didn’t confirm or deny the possibility of power limiting but informed us that they had identified a defect in a batch of motherboards. They had recently received a new batch and recommended replacing the motherboards in our affected servers. Normally, replacing a big part of our fleet can disrupt customer workloads. However, we had already moved most critical tasks from the AX162 servers because they kept crashing, so replacing them was manageable.


We replaced the motherboards but kept critical workloads off the AX162 servers. We weren't sure the issue was fully resolved. Based on prior experience, we knew that servers appearing stable could still begin to crash frequently even after a month. Thus, we decided to monitor them carefully over an extended period.


At first, we saw no crashes. Then, after two weeks, servers with the new motherboards started crashing as well.


Model Total Failure Count Total Days in Service Annual Failure Rate


AX161 11


3784 1.06


AX162 34


737 16.84


AX162 -v2 11


758 5.30


AX162 servers with new motherboards crashed less frequently, but the crash rate was still high. After contacting Hetzner again, we learned of an even newer version of the motherboard with improved reliability. We migrated our servers to this latest version and began monitoring reliability.


After monitoring the new servers for several months, we concluded that the crash issue is indeed resolved. Additionally, the AFR of these servers is now even better than that of the AX161 servers.


Model Total Failure Count Total Days in Service Annual Failure Rate


AX161 11


3784 1.06


AX162 34


737 16.84


AX162 -v2 11


758 5.30


AX162 -v3 4


3738 0.39


### Process Improvements


Adopting a new line of servers early on can come with unforeseen issues. We were quick to adopt the new servers because their specs were exciting. Also Hetzner’s decision to discontinue the AX161 model suggested the new line was production-ready. Looking back, waiting six months could have helped us avoid many issues. Early adopters usually find problems that get fixed later. Moving forward, we will make the following changes:


- We will conduct a thorough vetting of future server models.
- We will introduce new hardware gradually, beginning with non-critical workloads.
- We will add more bare metal providers to distribute the risk. In fact, we already support two more bare metal providers; Leaseweb and Latitude. We are also working on adding the fourth one.


We hope our lessons offer valuable insights to others navigating similar issues. As we develop a solid, open-source alternative to traditional cloud providers, these experiences motivate us to keep improving. We aim to deliver cloud solutions that are both reliable and adaptable.


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)
