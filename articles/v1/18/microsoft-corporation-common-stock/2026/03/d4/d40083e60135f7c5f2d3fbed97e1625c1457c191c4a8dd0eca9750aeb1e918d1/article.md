---
schema_version: "1.0.0"
document_id: "d40083e60135f7c5f2d3fbed97e1625c1457c191c4a8dd0eca9750aeb1e918d1"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-news-import-8ffc5114d8b6"
canonical_url: "https://news.microsoft.com/source/features/innovation/using-inexpensive-microleds-microsoft-networking-innovation-aims-to-make-datacenters-more-efficient/"
published_at: "2026-03-17T07:00:05+00:00"
first_seen_at: "2026-07-21T04:09:11.113728+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:5c0086c1672712d4f452c1ad321cdcc30f844633f3a292d452f8c3e686266f40"
---

# Using inexpensive MicroLEDs, Microsoft networking innovation aims to make datacenters more efficient

CAMBRIDGE, U.K. — Before appearing on a screen in your hand or on your desk, this story and its photos existed as pulses of light, fired by lasers, that passed through strands of glass as fine as a human hair.


It’s an invisible miracle of engineering. Because data and services are delivered to us seamlessly through fiber optical cables, we don’t think about it.


Innovative changes are coming in what one Microsoft expert called the “digital plumbing” that makes that magic happen.


A Microsoft invention that uses inexpensive MicroLEDs and is designed to be a more efficient alternative to cables that transmit data within datacenters today is expected to be commercialized with industry partners late in 2027, said the project’s lead researcher.


The development of this new system is timely. With the rapid growth of AI and cloud demand, existing networking technologies are increasingly constrained by physical limits in distance, power consumption, density and reliability.


The new[MicroLED technology](https://www.microsoft.com/en-us/research/blog/breaking-the-networking-wall-in-ai-infrastructure/?msockid=0a5bdbaad5dd6fde1745c96cd4be6ebf) , designed by the Microsoft Research Lab in Cambridge, U.K. in collaboration with teams from Azure Core, Azure Hardware Systems and Infrastructure, and Microsoft 365, has several advantages over the kinds of cables currently in use.


The researchers expect that it will use about 50% less energy than mainstream laser-based optical cables, based on the team’s lab tests of the new system and estimates of how it will perform when deployed.


Citing the team’s[peer-reviewed research](https://www.microsoft.com/en-us/research/wp-content/uploads/2025/08/benyahya25mosaic.pdf) , the lead researcher on the project, Paolo Costa, said it will also be less expensive to make and will have other benefits, like longer lifespan. The Microsoft team has recently completed a[proof of concept project with MediaTek](https://www.mediatek.com/press-room/mediatek-develops-active-optical-cable-technology-with-microsoft-research-to-deliver-significant-improvements-in-data-center-efficiency) and other suppliers to miniaturize the MicroLED technology and incorporate it into a transceiver device that is compatible with equipment used in datacenters today.


From left, Kaoutar Benyahya, Kai Shi and Paolo Costa – Microsoft researchers on the team that developed the new MicroLED cabling system – with one of the prototypes they used to develop the technology. Photo by Chris Welsch for Microsoft.


The new technology uses inexpensive and commercially available MicroLEDs instead of lasers and a different kind of commercially available cable, generically known as imaging fiber, to carry photons end to end.


”Imaging fiber looks like a standard fiber, but inside it has thousands of cores,” said Costa, a Microsoft partner research manager. He explained that the technology already existed in the cables used for medical endoscopy, which essentially sends a tiny camera into the human body. “That was the missing piece. We finally had a way to carry thousands of parallel channels in one cable.”


Inside datacenters, two types of cables are typically used to transmit data between servers: fiber optical cable carrying photons fired by lasers and, for closer, faster and more reliable connections, copper cable, transmitting data with electrons.


Each has its limitations. Copper can only connect as far as about two meters while transmitting high levels of data. Copper cabling is typically used in a single rack to connect graphics processing units, or GPUs, now in common use, especially in AI applications. The fiber optical cables can stretch much farther (like all the way across the floor of the Pacific Ocean, for example). But with distance and volume of data come issues with reliability and energy consumption.


The new technology resolves these limitations to a large degree; MicroLEDs can cover tens of meters, are more reliable than fiber optical cables fed by lasers, which are vulnerable to temperature changes and even dust; and use much less energy.


Today’s laser-based fiber optical cables deliver data in pulses of light over a few channels. As Costa describes it, this is the “narrow and fast” approach to transmitting data.


The MicroLED system, with thousands of independent channels, delivers photons in patterns that Costa compared to QR codes. This he describes as the “wide and slow” approach. It delivers as much data because of the width, like a broad slow-moving river versus a narrow fast-moving stream, both carrying the same volume of water.


“The early concept of using LEDs to send data more cheaply — and lower power — than both copper and fiber optics seemed like a fantasy,” said Doug Burger, technical fellow and corporate vice president at Microsoft Research. “This breakthrough has the potential to change nearly every aspect of computing infrastructure … starting with high-bandwidth optical cables.”


## Hollow Core Fiber already in use


Microsoft’s Hollow Core Fiber (HCF) technology is designed to work with existing Single Mode Fiber (SMF) equipment, making installation easy and creating the flexibility to use it broadly. Photo courtesy of Microsoft.


The MicroLED cabling is not the only networking innovation changing the way data is transmitted. Another development is called[Hollow Core Fiber](https://techcommunity.microsoft.com/blog/azurenetworkingblog/microsoft-azure-scales-hollow-core-fiber-hcf-production-through-outsourced-manuf/4455953) , or HCF, and it’s already in use in some Microsoft Azure regions and is in the process of being deployed in more of them globally.


Both technologies will be showcased at the[2026 Optical Fiber Communications Conference and Exhibition (OFC)](https://www.ofcconference.org/home/) in March, including research and advances in HCF and MicroLED technologies.


Instead of carrying photons in fiber, HCF, as the name suggests, carries signals in a hollow core, through air, which allows the light to move even faster, ultimately decreasing the latency over the same distance or spanning a longer distance at the same latency. That means a datacenter could be placed farther away without losing the speed and reactivity that a consumer is used to. The innovation was chosen as one of the top inventions of the year in 2025 in[Time magazine](https://time.com/collections/best-inventions-2025/7318263/microsoft-hollow-core-fiber/) .


Microsoft has agreed to manufacturing collaborations to help increase HCF production to equip more of its datacenters globally.


Frank Rey is Microsoft’s general manager of Azure Hyperscale Networking. He said that while he sometimes describes his job as being a “digital plumber,” his team is actually “responsible for all the rooms, boxes and wires that make up Microsoft’s global network.”


HCF and the new MicroLED system are complementary technologies, both of which help Microsoft reach its goals of delivering Azure cloud services in the fastest and most efficient way, according to Rey.


Generally speaking, the MicroLED system will serve a role inside datacenters, connecting servers and GPUs, its designers say. HCF has the capacity to cover great distances, serving customers and connecting datacenters, Rey said, although it could also eventually play a role inside datacenters.


Two of the big advantages of Hollow Core Fiber, Rey said, are that HCF delivers up to 47% faster data transmission and approximately 33% lower latency compared with conventional Single Mode Fiber (SMF), according to[published research](https://www.orc.soton.ac.uk/news/7210) . HCF was developed at the University of Southampton and further developed in a spinoff company called Lumenisity, which Microsoft acquired in 2022.


“With MicroLED, you have the pure efficiency of LED over a laser,” he said. “That has a pure bottom-line impact to power usage at any given datacenter. And then Hollow Core allows us to extend that area served by one datacenter and an Azure region. And outside of an Azure region, if you can go a much greater distance before you need to do any signal amplification, that means less buildings, less power, less generators, less energy.”


Rey said that both HCF and the MicroLED system are designed so they can be installed quickly and without difficulty in Microsoft and other datacenters.


## Investigating what’s possible


##


Paolo Costa holds one of the new MicroLED cables with two transceivers attached. The transceivers contain equipment that converts photons into usable data. Photo by Chris Welsch for Microsoft.


Kaoutar Benyahya, a Microsoft researcher in Cambridge, U.K., at work on an element of the MicroLED system designed to replace fiber optic cables in datacenters. Photo by Chris Welsch for Microsoft.


In the Cambridge lab, a working prototype of the MicroLED system covers much of a worktable with a tangle of glowing cables draped over metal framing that supports lenses, image sensors and MicroLED lights. To tackle a problem of this complexity, Microsoft deployed a team of experts that includes a computer scientist, a digital logic expert, optical engineering designers, electronic integrated photonics and packaging experts, a mechanical engineer and optical communications and signal processing experts.


Costa began his work at Microsoft Research with a project on[optical switches](https://www.microsoft.com/en-us/research/publication/sirius-a-flat-datacenter-network-with-nanosecond-optical-switching/) , but the researchers soon realized they also needed an efficient link technology to fully unlock their benefits.


“Just before the pandemic started, we started looking into how could we make the links more efficient, and this is when we turned toward developing the MicroLED approach,” he said.


Since then, Costa and his team developed the system one can see on the workbench in the lab, and in collaboration with MediaTek and other suppliers, they have more recently miniaturized all of it to fit into a metal transceiver, which plugs into a server and is roughly the size of a big thumb.


That transceiver contains a miniature version of everything on that cluttered lab bench. Tiny lenses guide the light onto photodiodes that turn it into electrical signals carrying usable data.


Once the data reaches a server, that data becomes our emails, blog posts, photos of cats, streaming movies and AI chats. An engineering miracle that is invisible because it works.


*Top image: A prototype of the new MicroLED system designed to replace fiber optic cables and copper cabling in datacenters on a workbench in one of the labs at the Microsoft Research Center in Cambridge, U.K. All of the elements present on this table were miniaturized to fit into a transceiver that is about the size of a thumb and can be plugged into servers. Photo by Chris Welsch for Microsoft.*


**Related links** :


**Read more** :[Breaking the networking wall in AI infrastructure](https://www.microsoft.com/en-us/research/blog/breaking-the-networking-wall-in-ai-infrastructure/)


**Learn more** :[Mosaic: Breaking the Optics versus Copper Trade-off with a Wide-and-Slow Architecture and MicroLEDs](https://www.microsoft.com/en-us/research/wp-content/uploads/2025/08/benyahya25mosaic.pdf)


**Learn more** :[Microsoft Azure scales Hollow Core Fiber (HCF) production through outsourced manufacturing](https://techcommunity.microsoft.com/blog/azurenetworkingblog/microsoft-azure-scales-hollow-core-fiber-hcf-production-through-outsourced-manuf/4455953)


**Read more** :[New hollow-core fibres break a 40-year limit on light transmission – Physics World](https://physicsworld.com/a/new-hollow-core-fibres-break-a-40-year-limit-on-light-transmission/)


**Read more** :[Disrupting the AI infrastructure with MicroLEDs – Microsoft Research](https://www.microsoft.com/en-us/research/video/disrupting-the-ai-infrastructure-with-microleds/)


**HCF honored** :[Microsoft Hollow Core Fiber: The Best Inventions of 2025 | TIME](https://time.com/collections/best-inventions-2025/7318263/microsoft-hollow-core-fiber/)


*Chris Welsch is a reporter and photographer based in France covering AI, Innovation and a variety of other topics for Microsoft Source EMEA. He’s recently written about AI-guided driverless cars. Before joining Microsoft Source in 2022, Welsch was a staff editor at the International New York Times in Paris and before that a senior reporter and photographer at the Minnesota Star Tribune. Follow him on[LinkedIn](https://www.linkedin.com/in/chris-welsch?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=ios_app) .*


#### Tags:


- [Technology](https://news.microsoft.com/source/tag/technology/)
