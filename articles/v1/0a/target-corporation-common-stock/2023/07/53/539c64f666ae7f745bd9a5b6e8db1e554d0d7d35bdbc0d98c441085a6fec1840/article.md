---
schema_version: "1.0.0"
document_id: "539c64f666ae7f745bd9a5b6e8db1e554d0d7d35bdbc0d98c441085a6fec1840"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/cybersecurity-easysweep"
published_at: "2023-07-18T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:38c091b3369beadf7d6a9571c86ac65cdf5ad98470b74eb76ac7f2d73dd2b93c"
---

# Target's EasySweep – Simplifying Skimmer Detection for All

How often when you use an ATM, payment terminal, or gas pump do you wonder “Is my card information safe?” I know I ask myself that every time I swipe or insert my card. Research by


[FICO](https://www.fico.com/blogs/us-card-skimming-grew-nearly-5x-2022-new-fico-data-shows) shows that more and more criminals are trying to steal payment card information using devices called skimmers, which read the information off your payment card, either from the chip or the magnetic strip. From 2021—2022, the number of these devices found in the US surged 368% with more than 161,000 cards now reportedly being stolen annually. With skimming devices becoming more common and more difficult to spot, how can retailers protect their guests?


At Target, we decided to tackle this issue head-on in 2020 when we first started to see reports of skimmers found at other retailers. These skimmers could be used on our payment terminals. We started designing a program to protect against these devices. We identified four main goals:


1: Make detecting skimmers simple


2: Make the detection cost effective


3: Find skimmers fast enough to keep guests safe


4: Discourage criminals from placing skimmers at Target


With these four goals in mind, I took the lead on developing a detection tool for skimmers. Normally, in the information security world, we immediately turn to data. Since these devices do not connect to our in-store network, there was no data to analyze. There was also an early idea to look for these devices with an electronic tool as some of them use Bluetooth, but then we found a new type of skimmer that did not use Bluetooth. Somehow, we needed to figure out how to easily identify these devices physically, rather than based on electronics or data.


I decided to 3D-print a clone of a skimmer just to be able to hold it in my hands and think about how to detect it. That’s when it hit me: we know how big skimmers are, so could we simply measure for them?


I set to work making a 3D design that could measure our payment terminals. My first attempts were made after measuring photos of payment terminals and skimmers. After several attempts and a visit to a local Target store to make final measurements, I had a draft design that worked well enough to bring forward to Target’s leadership as a potential solution. After a few tweaks to adapt the design from focusing on 3D printing to injection molding thanks to Target’s 3D Labs team, I had a design that was ready to mass produce and met all the project goals.


I named the tool EasySweep, as it allows any Target team member to easily sweep a store for skimmers. Following development, I launched a small test in partnership with Target’s Store and Asset Protections teams. The results were incredible. The time to sweep a store went from one to two hours physically examining each of the 20-30 payment terminals per store down to a mere 15 minutes. I also tested the EasySweep against every known skimmer for the Verifone MX925 and MX915 payment terminals and found the tool to be 100% accurate.


The usage is very simple: Insert the tool into the payment terminal’s chip card slot. If it can insert fully, the terminal is safe. If it gets stopped, there might be a skimmer!


Three years later, the now-


[patented EasySweep payment terminal evaluator](https://patents.google.com/patent/US11507762B2/) has been used to check for skimmers on Target payment terminals millions of times with inspections being done daily in all our store locations. Each inspection is logged, and any anomalies are immediately reported to Target’s Cyber Fusion Center, the centralized team that detects and responds to attacks against Target including cyber-fraud and cyber-attacks, which allows us to quickly coordinate a response to remove and secure any skimmers or suspect devices found. These daily inspections ensure the highest traffic terminals in our stores are inspected frequently enough that even if a criminal tried to place a skimmer in our store, we would very likely have the device removed before they could ever try to recover the device or credit card info. More than 60,000 EasySweeps have been shipped to Target stores with replenishments done twice a year to ensure the tools are always readily available for our Store team members.


Based on the success we saw with EasySweep, we decided to offer the design, for free, to other retailers. To access the Step CAD file for the design, any retailer can email


easysweep@target.com from a corporate email account, and we will send you a production-ready file. The design works on Verifone MX925 and MX915 terminals as well as Ingenico Lane 5000, 7000, and 8000 terminals. *(


Update December 2023: The design also works on Verifone M400 terminals


). The design is inexpensive to injection mold at less than one dollar per unit and can be changed to work with nearly any other payment terminal. We hope that this tool can help all retailers keep their guests’ information safe and make skimming unprofitable for criminals.


With


[law enforcement constantly warning](https://www.fbi.gov/how-we-can-help-you/safety-resources/scams-and-safety/common-scams-and-crimes/skimming) of new skimmers being found at retailers worldwide, it is important everyone is mindful and takes steps to keep their payment cards safe. At Target, you can feel better knowing we are making sure your card information is safe, every day, and that we continue to innovate and discover new ways to get ahead of scammers.


## RELATED POSTS


### Target’s ECCO Platform: Achieving Resiliency and High Availability


By Shesadri Parthasarathy, December 1, 2022


A deep dive on Target's Enterprise Cart & Checkout (ECCO) platform - the underlying technology that powers our checkout experience across Target.com, the Target mobile app, and nearly 2,000 stores.


### Moving from "Or" to "And"


By Tech @ Target, April 11, 2023


Interview with Senior Director Tom Sheffield to explore what Identity Management means at Target.


### Cybersecurity “State of the State”


By Tech @ Target, November 4, 2022


October is Cybersecurity Awareness Month, recognized annually in the United States since 2004 as an opportunity to educate people about steps they can take to enhance the security of their digital worlds.
