---
schema_version: "1.0.0"
document_id: "1cc9a12d9def80f6b4f9b922592c8a6d7bc95b4d623be47d741005f0a1ebd57b"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/ipv4-address-swamp-new-normal"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:5f226ae2da13895cd35d117d5a056988478cb7806e15a0f52763b601fcb6e713"
---

# The IPv4 Address Swamp: The New Normal

## Executive Summary


IPv4 addresses have run out! It would have been fashionable to make this claim in 2011 when the last of the IPv4 addresses in the “free pool” were allocated. It took several years, but today most of those remaining addresses are accounted for. How has the distribution and use of these last addresses been made in comparison to what was once commonly referred to as the IPv4 address swamp? Has IPv4 allocation and assignment changed for the better in the 21st century? Or are the prefixes getting smaller and even more diverse? What implications might this have on internet security?


Outside of its historical context, we rarely refer to swamp space any longer. Why is this? Perhaps it is because the majority of the IPv4 address space now closely resembles what was once an outlier in address management, organization, and structure? Perhaps there is a new swamp, just like the old swamp.


## Key Findings


- The legacy address “swamp” is what a lot of the address space now resembles.
- Address registrations and routes are growing in number, while prefix sizes are getting smaller.
- Address volatility greatly affects the performance of threat mitigation.


## Background


In the 1990s and well into the 21st century, network operators often referred to a portion of the IPv4 address space as “The Swamp.” As far as we know, the phrase was never formally defined, but it was commonly used to refer to a subset of allocations in the original classful C address hierarchy. In practice, small /24 assignments first came out of 192/8, the start of the class C block. The negative connotation implied by the word “swamp” suggests dirty, disheveled, and inefficient. In terms of IP address management and routing, these attributes often fit. Network operators worried that if trends continued, the size of routing tables would quickly overwhelm router capacity. By the early 21st century, approximately 80 percent of the 192/8 address space was already assigned, and much of it was seen in the internet routing tables as many disaggregated /24 routes.


The sheer number and diverse assignment of these /24 prefixes effectively prohibited address aggregation. Over time, the routing system evolved to handle an ever-increasing number of prefixes, but few, if any, routers from the early days would be able to load and compute the routing tables that exist today. Small prefixes and the routing table entries continue to grow. As of this writing, a full IPv4 routing table is approximately 1 million entries. Two decades prior, there were only 150,000 routes. When people suggest the internet is a collection of loosely cooperating autonomous systems, the swamp might have been considered “exhibit A,” foreshadowing the new normal of IPv4 addressing disorganization.


The last of the /8 IPv4 address allocations from the Internet Assigned Numbers Authority (IANA) to regional internet registries (RIRs) were made in 2011. Obtaining previously unassigned IPv4 addresses is now becoming a thing of the past. IPv4 address scarcity has led to a variety of reactions from users and the market. Large blocks of assigned IPv4 addresses (/16 or larger) are routinely transferred from one holder to another for hundreds of thousands of dollars. Waiting lists and address-leasing companies are now part of the IPv4 address assignment landscape. Many organizations with lots of addresses in the legacy class A or B networks have split them up or transferred them to the highest bidder. It is well known that the big cloud providers such as Amazon and Microsoft have gobbled up many IPv4 address blocks on the market, divvying them up across their global data center infrastructure. Network Address Translators (NATs) continue to be widely used and relied upon. Interest in and deployment growth of IPv6 addressing continues to grow every year. We began to wonder about all these changes, especially considering how the IPv4 address space structure and organization have changed. The implications the new normal has on routing are obvious, but less well understood has been what effect these changes have had on address and network reputation.


Maybe we don’t use the term swamp anymore because, increasingly, the entire IPv4 address space has all the telltale signs of the original swamp?


## The Last of the Free Pool


IP address assignment for small address blocks in the original class C hierarchy was primarily first drawn from 192/8 and almost entirely in /24 chunks at a time. A /24 prefix is widely observed to be the smallest block of addresses that can be successfully announced and seen by most internet routers. At the turn of the century, approximately 10 percent of the total number of IPv4 route table entries may have come from just the subprefixes in 192/8 alone. Then and today, all /24 route advertisements account for more than 50 percent of all routing table entries, although the trend hovers closer to 60 percent today. See Figure 1 for a recent look at the IPv4 routing table by prefix size distribution.


**Figure 1- : IPv4 routes by prefix size**


In February 2011, IANA distributed the last five remaining IPv4 /8 blocks of addresses to each of the RIRs (AFRINIC, APNIC, ARIN, LACNIC, and RIPE). We wondered if registration and assignment of prefixes would resemble the 192/8 swamp or something different. Understanding how these new blocks of previously unallocated addresses were distributed might give us clues on general IPv4 addressing usage trends in other portions of the address space.


> Note: Registries denote a status for the address space they manage. For our purposes, we focus on address blocks RIRs have designated as assigned or allocated. The former is when an address block has been delegated from the RIR to an end user, such as an ISP, while the latter suggests it hasn’t yet been assigned but is available for distribution. However, we have found that the distinction between these two states is often unclear. We group both types of address blocks together for simplicity, imperfect as this may be. Other designations can include “available” and “reserved,” which we ignore in our analysis unless otherwise specified.


At the end of 2011, there were very few allocations and assignments made from these five blocks. By 2014, two registries had allocated or assigned almost all this new address space, APNIC and ARIN, about one-third, and AFRINIC had yet to make any registrations. Today, all RIRs have allocated and assigned most, if not practically all, addresses in the last /8s from the remaining free pool. See **tables 1, 2, and 3** below for details.


**Table 1-: Last of the IPv4 free pool 2011**


**Table 2: - Last of the IPv4 free pool 2014**


**Table 3: - Last of the IPv4 free pool 2024**


We could have guessed that practically all available IPv4 addresses would ultimately be put to use. We can observe some patterns in the rate of allocation and assignment by RIR, which may say something about the demand in their regions and their distribution policies. However, what might be more useful to see is the size of blocks of addresses within these last free pools as they are allocated, assigned, and routed. We know that address registrations and routing are often incongruous, but we can get a sense of the structure, especially compared with the 192/8 block, to see whether or not usage for all the address space gravitates toward one recognizable pattern.


In **Figure 2** below, we plot the distribution of address block sizes for the last five free pool allocations from IANA (102/8, 103/8, 104/8, 179/8, and 185/8) in 2014. We separate them by their corresponding registry. Note: It is possible that a registrant moved its assignment to another registry, but this is relatively uncommon, and we don’t believe it markedly alters any observations made.


AFRINIC had not yet begun distributing prefixes from its 102/8 at that time. For the others, the average size of the prefix varied but tended toward the small side of around /22 for all registries. The picture changes slightly when we fast forward to 2024.


**Figure 2: – IPv4 last free pool registration prefix sizes (2014)**


By the end of 2024, AFRINIC had distributed the majority of its 102/8 address pool. The average size of address blocks from APNIC and RIPE appears to have remained largely unchanged. ARIN and LACNIC, however, appear to now have smaller blocks than 10 years ago. Also note RIPE appears to have a few block registrations smaller than a /24. The reasons for this can vary, but often contiguous “micro” registrations are ultimately assigned to the same entity that chooses to separately manage even smaller portions of a larger address space block. **Figure 3** below compares the distribution of block size registration with legacy 192/8.


**Figure 3: - IPv4 last free pool registration prefix sizes (2024)**


The majority of 192/8 address space is currently managed by ARIN, but a sizeable portion is managed at RIPE and other smaller portions at the remaining registries. In **Figure 4** , we can clearly see block sizes tend toward /24. This is not particularly surprising.


Figure 4: - IPv4 192/8 registration prefix sizes (2024)


We also note that blocks of addresses are often reserved throughout the entire IPv4 address space. For example, 192.168.0.0/16, you may know is reserved for private use as defined in IETF RFC 1918—address allocation for private internets. This subprefix is not allocated or managed by any of the RIRs.


Address registration paints a partial picture of distribution and usage. We can draw a more complete picture by considering how blocks of addresses appear in the internet routing tables. We will limit our scope here to a recent view of an internet routing table at the end of 2024. We can then make some comparisons to what appears in the registries and what appears in a typical routing table. There is often overlap, but the two are incongruent.


## Considering Border Gateway Protocol (BGP)


Recall the last free pool blocks IANA delegated to the RIRs eventually became nearly completely assigned and allocated. Nearly 80 percent of legacy 192/8 blocks were accounted for by as early as 2004. With practically all the address space in these six /8s assigned and allocated, we would expect this to be reflected in the internet’s routing tables. Something interesting appears, however, when you compare IP address registration and IP address routing. Except in the case of route leaks or hijacking, we might expect the coverage of these address spaces in the routing table to lag registration. The difference, however, is quite striking. We’ve reconstructed the tables this time with the 192/8 prefix and routing information added (see **tables 4 through 6** below).


**Table 4 -: IP address registrations and routes (2011)**


**Table 5 -: IP address registrations and routes (2014)**


**Table 6 - : IP address registrations and routes (2024)**


These tables provide a clearer picture for IP address assignment and usage. Independently, registration and routes follow a similar trend. Their numbers increase over time. The differences, however, are stark. The number of routes covering 192/8 in 2024 was a little over 14,000, but the proportion of the address space these routes covered was only slightly more than 62 percent of the total possible, which means nearly two-fifths of the 192/8 address pool is not directly reachable on the internet. As we’ve mentioned, there are reserved allocations in this range, but their numbers would not make up for the bulk of missing routes. All last free pool /8s have better route coverage in 2024. We could make some educated guesses or conduct more analysis to understand why, but we believe ultimately it comes down to the “legacy” of 192/8.


More interesting to us is not just that the last free pool space is more accessible, but that the number of routes covering each varies significantly. For example, we see more than 42,000 routes covering the 103/8 (APNIC) block, but only slightly fewer than 6,000 for 179/8 (LACNIC), and the latter covers nearly 100 percent of /8 while only 72 percent of APNIC’s 103/8 is covered. What is going on here? A look back at Figure 4 gives a clue. LACNIC’s assignment of addresses in its /8 tends to be larger than APNIC’s, and this is ultimately reflected in routing. Consequentially, we see a lot more smaller prefixes such as /23s and /24s for APNIC prefixes in the routing table.


Another observation that lends support to this article’s thesis is that the last of the free pool /8s looks surprising like a swamp when compared with the original 192/8 swamp. There are many small registrations and routes to many different entities. What distinguishes the legacy swamp from the modern swamp seems to be little more than age.


## Recommendations


To detect and mitigate abusive, ever-changing networks of varying size and duration, we recommend the following:


- **Real-time visibility** into volumetric traffic floods and distributed attack patterns. Tools such as NETSCOUT[Arbor Sightline](https://www.netscout.com/product/arbor-sightline) can help surface early signs of trouble and trigger flow-specification and Remotely Triggered Black Hole (RTBH) defenses to upstream providers.
- **Proactive mitigation** with automated systems such as Arbor Threat Mitigation System (TMS) or Arbor Edge Defense (AED). These can stop both volumetric floods and more-complex, multivector attacks.
- **Intelligence-driven defense** with feeds such as NETSCOUT’s ATLAS Intelligence Feed (AIF). These provide information about context, what’s trending, who’s being targeted, and how actors are evolving.


Staying ahead of threat actors is an ever-changing job and requires a broad view of where these attacks come from, how they operate, and where they could strike next.


## Conclusion


We know IP address prefixes have become increasingly transient. That is, prefixes move from registrant to registrant, often across the world after an exchange. As modern IP address volatility proliferates, an association of activity and reputation with addresses rapidly changes as well. An address block used by a low-cost hosting provider has a very different profile than when that address is used by end users on Wi-Fi hot spots, for example. We have been experiencing increasing distributed denial-of-service (DDoS) attack, scanning, scraping, proxy, and address reputation volatility in our data for a while now. You probably have as well. This instability in address/reputation pairing has implications for how well security threat mitigation services can perform. There is a risk for false positives, false negatives, over-blocking, and under-blocking. This is an area of work we are increasingly focused on to help better understand and respond to security threats in this new normal.
