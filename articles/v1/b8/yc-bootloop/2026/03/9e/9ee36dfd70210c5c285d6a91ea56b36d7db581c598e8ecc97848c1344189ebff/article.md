---
schema_version: "1.0.0"
document_id: "9ee36dfd70210c5c285d6a91ea56b36d7db581c598e8ecc97848c1344189ebff"
company_key: "yc-bootloop"
company: "BootLoop"
source_id: "yc-bootloop-news-import-a69f589313cd"
canonical_url: "https://bootloop.ai/blog/every-big-trend-at-embedded-world-makes-firmware-harder"
published_at: "2026-03-03T00:00:00+00:00"
first_seen_at: "2026-07-23T03:56:50.469294+00:00"
fetched_at: "2026-08-20T00:02:08.149327+00:00"
content_hash: "sha256:fc0074a41fcd788c8e2d0040900f6d818c5a87c793bf2d23d0d7671e3b758388"
---

# Every Big Trend at Embedded World Makes Firmware Harder. That's a Problem. - BootLoop

Every year, Embedded World puts the future of our industry on display. This year in Nuremberg, the agenda is packed with edge AI, RISC-V going automotive, the EU Cyber Resilience Act, Rust for safety-critical systems, and robots that need real-time firmware running across dozens of chips simultaneously.


All of these are exciting. And every single one of them makes the firmware engineer's job harder.


I've been going through the Embedded World 2026 programme obsessively for the past few weeks, and there's a pattern I can't unsee. The conference has over 200 sessions, five headline expert panels, and more than 1,100 exhibitors. The hardware side of the industry is moving at an incredible pace. But the firmware side? We're still writing drivers by hand, cross-referencing thousand-page datasheets, and debugging by staring at serial output. The gap between what the silicon can do and what firmware teams can ship is getting wider, not narrower. 80% of embedded engineering job postings go unfilled for months to years. The talent pool isn't scaling with the ambition.


Let me walk through what I mean.


### **Edge AI is moving to microcontrollers. The firmware to support it isn't keeping up.**


The opening keynote from Richard Simoncic, COO of Microchip, is literally called "Learning from the Octopus: Nature's Blueprint for Intelligence Everywhere." The message is clear: intelligence is being pushed to the smallest, most distributed endpoints. TinyML, NPUs on MCUs, deep learning on devices that run on microwatts.


That's incredible for what products can do. But it means firmware engineers now need to integrate ML frameworks alongside real-time control loops, manage memory on devices where every byte matters, and make it all work reliably in the field. The number of people who can actually do this well is not growing at the same rate as the demand. There are over 99,000 open firmware engineering roles in the US alone, and the average embedded position takes 3-6 months to fill while software roles close in weeks.


### **RISC-V is going mainstream. That's more architectures to support.**


There's a dedicated panel asking "Is RISC-V Ready for Automotive?" with Infineon, SiFive, Siemens, and Quintauris on stage. Across every industry the answer is increasingly yes. Over 10 billion RISC-V cores have shipped to date, the architecture holds roughly 25% of the global processor market, and the market is growing at a 44% CAGR.


For firmware teams, this means yet another target, another set of toolchains to learn, another debugging environment to master. If you're already supporting ARM Cortex-M, Cortex-A, Xtensa, and maybe a DSP or two, RISC-V is one more thing on the pile. The hardware ecosystem is flourishing. The firmware to match it is still catching up. NVIDIA alone shipped a billion RISC-V cores in 2024. This isn't a niche experiment anymore.


### **The EU Cyber Resilience Act is about to make firmware security non-optional.**


One of the most consequential panels at EW26 features the EU Commission, BSI, and CENELEC discussing CRA compliance. This regulation requires manufacturers to maintain software bills of materials, handle vulnerability disclosure, and demonstrate CE conformity for connected products.


Most of that burden lands squarely on firmware. SBOMs for embedded code. Vulnerability scanning for C/C++ running on bare metal. Patch management for devices deployed for a decade or more. Mandatory vulnerability reporting kicks in September 2026, with full compliance required by December 2027. The clock is already ticking.


The teams I talk to every day are already stretched thin just shipping features. The typical firmware project already spends 50% of its schedule on debugging and testing alone. Now those same teams need to build a security practice on top. And the stakes are real: 57% of IoT devices are vulnerable to medium- or high-severity attacks.


### **The toolchain landscape is fragmenting, not consolidating.**


Rust is getting serious attention for embedded. Dieter Nazareth has a dedicated session on it. Zephyr RTOS, now in its 10th year since its Linux Foundation launch, is pushing for IEC 61508 safety certification. Yocto has multiple sessions on long-term stability. These are all good developments. But they also mean firmware teams need to be proficient across more frameworks, more languages, and more build systems than ever before.


A few years ago, you could get by with one RTOS, one toolchain, and deep knowledge of one chip family. That world is gone. Today's firmware engineer needs to move fluidly between bare metal and Linux, C and Rust, proprietary HALs and open-source frameworks.


### **The real theme of Embedded World 2026**


The official themes are edge AI, security, RISC-V, and embedded vision. But if you step back and look at what all of these trends demand, there's a deeper story: the firmware layer is becoming the bottleneck for the entire industry.


Hardware is getting more capable. The software abstractions above firmware are maturing. But the work of actually writing, testing, and debugging the code that makes hardware do what it's supposed to do? That's still largely manual, painfully slow, and dependent on a shrinking pool of engineers who know how to do it.


The downstream cost of this bottleneck is already showing up. In 2024, 44% of all recalled vehicles in the US were recalled for software issues, up from just 16% in 2021. That's 15 million cars. The embedded industry is shipping more complex products with fewer qualified people to write the firmware. Something has to give.


This is the problem we're working on at BootLoop. We built an AI agent that writes firmware and tests it directly on real hardware. It ingests your datasheets and schematics, writes framework-native code for your specific target, compiles against your toolchain, flashes to your board, and iterates until it runs clean. When we say hardware-in-the-loop, we mean it literally.


We've also built in static analysis that catches CWEs and security vulnerabilities before they ship, which feels increasingly relevant given everything the CRA panel will be discussing.


### **Come see for yourself**


We'll be at booth 5-121, right next to the RISC-V booth. Our CTO Chris Markus and I will be there all three days. We're running live demos of agentic debugging and one-click driver generation on real hardware. No slides. No mockups. Real boards, real code, real-time.


If you're an engineer dealing with any of the challenges I described above, or a leader trying to figure out how to ship firmware faster without growing your team 3x, come say hi. We'd love to show you what we've been building.


And if you want to grab a coffee and just talk about the state of firmware development, I'm genuinely up for that too. Book a time with us or just swing by the booth.


See you in Nuremberg.


Noah


### Links to sources


1.


[80% of embedded engineering job postings go unfilled for months to years.](https://runtimerec.com/the-great-embedded-engineer-shortage-why-80-of-job-postings-go-unfilled/)


2.


[There are over 99,000 open firmware engineering roles in the US alone](https://ziprecruiter.com/)


3.


[the average embedded position takes 3-6 months to fill while software roles close in weeks](https://runtimerec.com/the-great-embedded-engineer-shortage-why-80-of-job-postings-go-unfilled/)


4.


[Over 10 billion RISC-V cores have shipped to date, the architecture holds roughly 25% of the global processor market, and the market is growing at a 44% CAGR](https://theshdgroup.com/wp-content/uploads/2025/04/RISC-V-Market-Analysis-2024-Abridged-Report-updated.b.pdf)


5.


[NVIDIA alone shipped a billion RISC-V cores in 2024](https://riscv.org/blog/how-nvidia-shipped-one-billion-risc-v-cores-in-2024/)


6.


[Mandatory vulnerability reporting kicks in September 2026, with full compliance required by December 2027](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting)


7.


[The typical firmware project already spends 50% of its schedule on debugging and testing alone](https://www.ganssle.com/blog/blog/the-cost-of-firmware.html)


8.


[57% of IoT devices are vulnerable to medium- or high-severity attacks](https://unit42.paloaltonetworks.com/iot-threat-report-2020/)


9.


[In 2024, 44% of all recalled vehicles in the US were recalled for software issues, up from just 16% in 2021. That's 15 million cars](https://www.sdvinsider.com/p/the-hidden-automotive-software-crisis)
