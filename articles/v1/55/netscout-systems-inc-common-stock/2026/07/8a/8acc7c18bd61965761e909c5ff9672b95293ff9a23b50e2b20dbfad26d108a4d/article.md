---
schema_version: "1.0.0"
document_id: "8acc7c18bd61965761e909c5ff9672b95293ff9a23b50e2b20dbfad26d108a4d"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/decoding-tcp-syn-stronger-network-security"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:0c6343e03d281dabddeca84d3a04cec8ad1865dae426e138374708cce2edc815"
---

# Decoding TCP SYN for Stronger Network Security

## **Executive Summary**


Analyzing transmission control protocol (TCP) SYN segments,[the initial step in the TCP three-way handshake](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Connection_establishment) , can reveal patterns and anomalies in network traffic, providing insights into potential threats. In this article, we use data collected from NETSCOUT honeypots, which are[systems designed to capture unsolicited internet traffic](https://www.techtarget.com/searchsecurity/definition/honey-pot) , to examine TCP SYN segments. By focusing on packet headers, as network routers typically do, we explore trends in source addresses, IPv4 time-to-live (TTL) values, and TCP header lengths. This narrow perspective demonstrates how even limited packet data can yield actionable intelligence without delving into payloads or complex attack patterns.


### Key Findings


- We found no evidence of source address–spoofed TCP SYN segments, despite expectations based on their prevalence in malicious activities.
- IPv4 TTL values from the same source address varied widely, likely due to path diversity or IP header manipulation.
- Many Internet Protocol (IP) and TCP header values follow operating system defaults, while anomalies can often be associated with “crafted” packets and various types of “nuisance” traffic.


### TCP Header Introduction


**Figure 1:** TCP header format as defined in[IETF RFC 9293](https://datatracker.ietf.org/doc/html/rfc9293)


All legitimate TCP-based communications begin with a client sending an active TCP open to a server’s passive TCP listener. The SYN segment contains a TCP header, as defined in IETF RFC 9293, including fields such as the initial sequence number (ISN), window size, and optional parameters such as window scale. These fields can reveal patterns, because their values often reflect operating system defaults or, in the case of anomalies,[crafted packets](https://en.wikipedia.org/wiki/Packet_crafting) created outside standard TCP/IP stack processes. Careful analysis of TCP headers has often been used to fingerprint specific types of operating systems and attack patterns.


### Analyzing TCP SYN Data


Our investigation starts in NETSCOUT honeypots, configured as low-interaction systems that capture but do not serve content, that recorded TCP SYN segments in April 2025. We focused on three aspects: source IP addresses, IPv4 TTL values, and TCP header lengths.


We wanted to understand the origins of unsolicited TCP SYN packets by mapping their source IP addresses to geolocations and projecting them onto a world map. This map shows the global source origins of TCP SYN packets sent to NETSCOUT honeypots from January to June 2025, with larger dots indicating a higher density of sources.


**Figure 2:** Sources of TCP SYNs to honeypots


Examine the geographic distribution of unsolicited SYNs and identify any patterns. Note that this may be a trick question, because the distribution resembles a world population map, indicating that these SYNs originate from nearly all internet-connected regions. In the following analysis, we investigate whether this apparent widespread origin is genuine or if it is distorted by source-address spoofing.


### Searching for Source Address–Spoofed SYNs


Source IP spoofing is commonly associated with malicious activities, such as distributed denial-of-service (DDoS) amplification attacks, state exhaustion attacks, or network scanning, where attackers mask their origin to evade detection or overwhelm targets. Given this prevalence, we expected to find evidence of spoofed TCP SYN segments in our honeypot data. To investigate, we searched for bogon addresses—invalid or reserved IPs unlikely to appear in legitimate traffic.


Now the question remains, how many bogon-sourced SYNs did we see?


**The answer is zero.** Was that a trick question? We don’t think so. We see evidence of source-address spoofing in many DDoS attacks; surely it isn’t unreasonable to see some spoofed traffic arriving at our honeypots. This caused us to question whether we missed some key element to these incoming SYNs.


There may be a few reasons to explain the lack of any bogon source addresses:


- One, maybe there just wasn’t any source-spoofed SYN traffic. Maybe this really is a rare or unlikely phenomenon.
- Two, maybe something or someone is filtering out bogons before they reach our honeypots.


We'd like to think these explanations are plausible, particularly the possibility that bogons are being filtered before reaching our honeypots. If true, it remains entirely possible we are seeing spoofed SYNs, but we cannot detect them because we lack the telltale bogon addresses that would normally indicate source spoofing. We performed one additional test to help us determine if we may be seeing spoofed SYNs.


We grouped the sources together and looked at the distribution of the IPv4 TTL (or IPv6 hop limit) field value. If we see enough SYNs with the same source address and the distribution of this field varies significantly, we might consider this strong evidence of spoofed traffic from multiple diverse sources (or likely named bots in this case). We looked at one of our honeypots and plotted the standard deviation of IPv4 TTL values, grouped by source IP address, to observe this distribution.


**Figure 3:** IPv4 variability in external traffic to honeypots


If there was no spoofing, and the paths from each source to our honeypot remained consistent, we’d expect the standard deviation to be zero almost all the time. As you can see, while the distribution is skewed toward zero, there are a significant number of outliers. Some sources vary between two distinct TTL values. When we manually examined these, they appear likely due to the environments where load balancing or network address translation (NAT) may be in use.


We then looked at sources that oscillated between not just two TTL values but many more and sent at least 10 SYNs to our honeypot. One source at which we looked more closely was sending SYNs to TCP destination port 22 (SSH) and 23 (TELNET). Interestingly, not only was the TTL seemingly random in each packet, but the TCP header length field for all SYNs from this source oscillated between 20 and 40 bytes. Is this source-address spoofing? Was this a NAT address? A NAT might explain the different header sizes and TTL values, but would we really expect to see multiple different sources behind a NAT all trying to connect to our honeypot’s SSH or TELNET ports? Maybe, but it would probably depend on the network. The purported source network in question in this case turned out to be a network often associated with[network nuisance traffic](https://www.netscout.com/blog/asert/nuisance-network-traffic) activity! Our educated guess in this case is that the source was altering some parts of the IP and TCP header, but the source IP address was probably not spoofed.


Because we didn’t find solid evidence of widescale spoofed source addresses, we ended this line of research.


### TCP Header Length Distribution


Once we became reasonably convinced that we were not seeing much source-address spoofing, we were interested in patterns related to the TCP header size. A TCP header ranges from 20 to 60 bytes. A 20-byte TCP header contains no TCP options. In practice, the lack of TCP options in a SYN segment is very unusual. In our experience, the only legitimate 20-byte SYNs have been some VPN traffic and old Solaris-based systems. Historically, a great deal of TCP scanning traffic and TCP-based worms are responsible for many TCP SYNs containing no options. We anticipated a fair number of these 20-byte TCP header SYNs. Figure 4 shows the distribution of TCP header sizes we observed from one of our honeypots.


**Figure 4:** TCP header length distribution


The header length will be a multiple of 4. So, a 20-, 24-, 32-, and 40-byte header length is valid, but a 21-, 37-, or 43-byte TCP header length is not. Minimum-length, 20-byte TCP headers were relatively infrequent but visible compared with other more common lengths. If we wanted to create a filter on highly suspicious TCP traffic, we could eliminate about 5 percent of all SYNs if we simply dropped them when the TCP header length is 20 bytes. The question that must be asked is, would this be safe to do in your network?


Another interesting TCP header option to consider is the TCP window size. Recall that there is a TCP option to utilize larger window values than the original 16-bit field allows, but let’s ignore that option for now. Check out the distribution of this value we see on one of our honeypots.


**Figure 5:** TCP window size distribution


From this plot, we can see some values dominate, and one was more popular than most, while some values are practically never seen. The most popular TCP window size value we see is 64,240. This is the default value used by many Microsoft Windows systems. Another value that stands out is 29,200. This seems like an unusual value that merits an explanation. This value is often associated with a variety of[suspicious activity such as proxies, cryptocurrency miners, and scanners](https://medium.com/foxio/ja4t-tcp-fingerprinting-12fb7ce9cb5a) .


Another way to visualize patterns of the TCP header length is to correlate the values observed in a plot with other parameters such as the window size and sequence number.


**Figure 6:** TCP header length and window size by source distribution


We can see more clearly the relative frequency of TCP header length values when shown in relation to the received TCP sequence number and TCP window size parameters. We limit the display where the TCP sequence value received is zero. Recall that the ISN should be a random value, but at our honeypot we see a larger proportion of SYNs with a sequence value of zero than we’d expect, at least if each source were performing adequate ISN generation. Not only is a sequence value of zero suspicious, but so is the window size of zero. Here we can also see how the TCP header length of 20 and a TCP sequence of zero often go together, suggesting many of these packets are crafted. If we wanted to reduce SYN traffic a little further with a filter, we may be able to filter on some other combination of TCP header length, sequence number, or window size values.


### Conclusion


A low-impact honeypot observes a lot, even if we limit the observations to just SYN segments. We can see when SYNs reflect the makeup of regular operating system populations and when they do not. By carefully considering what is normal, we can often pinpoint not only unexpected activity, but potentially that which is generally unwanted. If we can confidently exclude certain types of “magic bit combinations,” we can not only reduce unnecessary or undesirable traffic, but also potentially decrease risk. There are a lot of potential bit combinations in IP and TCP headers, so it may seem the attack space is still significant even if we were to eliminate just half of them. But we also know attackers can be lazy. If our attack surface is less than the average, that may be good enough to avoid becoming the low-hanging fruit in the next wave of attacks. That may be reason enough to know what the normal TCP SYN traffic trends on your network are.
