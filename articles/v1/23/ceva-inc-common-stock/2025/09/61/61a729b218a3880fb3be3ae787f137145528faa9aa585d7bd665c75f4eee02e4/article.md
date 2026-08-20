---
schema_version: "1.0.0"
document_id: "61a729b218a3880fb3be3ae787f137145528faa9aa585d7bd665c75f4eee02e4"
company_key: "ceva-inc-common-stock"
company: "CEVA Inc."
source_id: "ceva-inc-common-stock-rss-438e9c80ad28"
canonical_url: "https://www.ceva-ip.com/blog/unleashing-the-power-of-wi-fi-7-multi-link-operation-configurations-demystified/"
published_at: "2025-09-25T13:26:38+00:00"
first_seen_at: "2026-07-20T23:21:57.347824+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:2bd9dae3210acf3b0153c4da9e37cb850fa7dfafa33eb599913c7aadbdc98633"
---

# Unleashing the Power of Wi-Fi 7. Multi-Link Operation Configurations Demystified

4 min read


Modern communications technologies have ushered in new connectivity opportunities we couldn’t have imagined ten years ago, allowing everything to be wirelessly connected and interoperating within our homes, offices, factories, cars, and with our phones. The downside is that allocated frequency bands are becoming increasingly crowded, especially in Wi-Fi. The 2.4GHz band is notoriously crowded, where Wi-Fi devices compete not only with each other but also with Bluetooth and ZigBee. The 5GHz band was intended to solve that problem but now also is overflowing. And the recently added 6GHz band supported by Wi-Fi 6E & 7 may ultimately suffer the same fate. Possibly more bands will need to be added, but equally important is using already available bandwidth as effectively as possible. One of the most important features of Wi-Fi 7, multi-link operation (MLO), aims to address this need.


## Opportunityhttps://www.ceva-ip.com/blog/unleashing-the-power-of-wi-fi-7-multi-link-operation-configurations-demystified/


Wi-Fi usage continues to grow fast, leveraging our existing Wi-Fi investment in support of connectivity for laptops, now extending to home and workplace automation. Even our phones tap into Wi-Fi when available to avoiding drawing on cellular data plans. As a result, Wi-Fi units shipped are expected to grow at a 7% CAGR from around 3.4B in 2023 to over 4.6B in 2028, driven mostly by IoT, smart home, wearable and entertainment markets.


Wi-Fi 5 (5GHz) adoption in new devices is already declining steeply and will continue to decline going forward, with strong growth already being picked up by Wi-Fi 6 and Wi-Fi 7. New shipments for STAs (stations, the Wi-Fi term for end-user client devices) in premium products will support 2×2 configurations (2 transmit paths and 2 receive paths). In simpler and low power devices 1×1 (1 transmit path and 1 receive path) will be sufficient.


Access points (APs) allow for 4×4 or even 8×8 configurations to support a high number of simultaneous users particularly in dense and public areas. However, over 50% of APs will stick to 2×2, a more optimal solution in terms of cost and power while providing enough performance for small and medium size networks such as homes and small offices. The market for access points is expected to grow at a very healthy 50% from 2022 through 2028, reflecting the reality that Wi-Fi is still the preferred solution where available for high performance, high bandwidth traffic.


## Understanding Wi-Fi 7 Multi-Link Operation


How does Wi-Fi 7 MLO more effectively use available bandwidth? The basic idea is illustrated in the figure above. In MLO, a connection between an AP and an STA has two communication links, allowing for simultaneous transmit/receive on each link for higher throughput, or listening on the 2 links simultaneously while using one link to communicate for lower latency. Such a setup may take advantage of two different bands in 6GHz for example, or one in 6GHz and one in 5GHz, or one in 2.4GHz and one in 5/6GHz, as channels become available.


MLO increases available channel capacity in the connection and can reduce latency for the same reason. It also increases reliability of the connection; if quality in one channel drops, the system can instantly switch to another channel. Higher throughput, low latency and high reliability should be attractive in many premium applications such as AR/VR headsets, gaming, and video conferencing.


It is important to realize that Wi-Fi 7 MLO builds on advances from Wi-Fi 6 in MIMO (multi-input, multi-output) technology. For instance, in a Wi-Fi 7 2×2 MLO solution, each of the two radios may be configured to listen on different channels. When a reception is detected on one radio, the system may instantly reconfigure the other radio on the same channel to execute a full speed 2×2 MIMO data transfer. This is a very efficient way to handle 2×2 MIMO traffic with low latency and low extra cost.


## Ceva Wi-Fi 7 MLO options for a range of market needs


A multi-link multi-radio (MLMR) 2×2 configuration is clearly the most powerful Wi-Fi 7 solution, shown as the rightmost configuration in the figure below. Two parallel channels use two MLO links, each in turn providing 2×2 MIMO capability. Today this Wi-Fi 7 MLMR configuration is the most popular solution shipping on the market. It is the most capable Wi-Fi 7 configuration but also the most expensive. At Ceva we address next generation Wi-Fi 7 innovative products by serving a range of feature profiles from value-based to premium, and we’re helping our customers meet that need with support for several configurations spanning that range, all compliant with the Wi-Fi 7 standard.


At the low end of this range is a single link option, called multi-link single radio (MLSR), which emulates multi-link through time-sharing. Needing only one 1×1 radio this option is very cost effective and still can show substantial performance improvement over Wi-Fi 6 thanks to faster switch. Next up is a dual-link MLMR, each link offering a 1×1 connection and generally supporting concurrent communication on each link for higher throughput with low latency.


Another option that is proving popular is called enhanced MLSR (eMLSR), offering for instance 1 link with a 2×2 radio, which can be reconfigured as 2 links each with a 1×1 radio. The use model here is quite interesting. Both 1×1 links can listen, monitoring for access point requests on 2 different channels. Once a good channel is detected, the whole system is reconfigured instantly to handle the data transfer on this channel. The beauty of this is that the 2 1×1 radios can be combined, effectively exploiting these as a 2×2 radio and thus enjoying higher throughput. Needing only 2 1×1 radios, this is a cheaper option than full 2×2 MLMR and is proving very popular as a lower cost option for smartphones, PCs, laptops, tablets and XR applications, with similar latency performance and good enough throughput (e.g. 2.88Gbps for 160MHz channel bandwidth).


Finally as mentioned earlier the full 2×2 MLMR serves the premium end of this range. The range of configurations may seem a bit confusing at first but makes perfect sense for OEMs who need to serve different markets at the right cost/power/performance point while also future proofing for Wi-Fi advances and being able to take advantage of the marketing buzz around Wi-Fi 7.


Learn more about[how Ceva is supporting Wi-Fi 7 options](https://www.ceva-ip.com/product/ceva-waves-wi-fi/) .
