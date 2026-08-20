---
schema_version: "1.0.0"
document_id: "632021f2c0a3b8ce434e7ea54e6ab9534cd5e87e6cb57d32c7da68e112cb311e"
company_key: "yc-bootloop"
company: "BootLoop"
source_id: "yc-bootloop-news-import-a69f589313cd"
canonical_url: "https://bootloop.ai/blog/embedded-is-changing"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-23T03:56:50.469294+00:00"
fetched_at: "2026-08-20T00:02:08.149327+00:00"
content_hash: "sha256:75865fd090644dfa177c1564f05c4f628a0c27eba02473f49a7bb4e0a55dacb4"
---

# Embedded is Changing. Software is overtaking Hardware. - BootLoop

For decades, "embedded" meant hardware. But the industry is starting to shift.


Sensors Converge has become the US embedded industry's annual state-of-the-union, and the 2026 snapshot makes the shift hard to ignore.


We classified the 2,176 companies walking the floor at the show this year by what they’re building - and the data shows a fundamental change in that union.


## **The current mix is still mostly atoms, not bits**


Hardware Devices and Systems is the largest single bucket at 17.6%. These are the companies building products that rely on the processors, sensors, and software produced by others - automotive, medical devices, consumer electronics, and more.


Together, manufacturing, silicon, sensors, and hardware systems make up about 46% of the show. There is still a lot of physical product on the floor.


But the moment you look at *when* those companies were founded, the data starts to paint a different and more important picture.


## **The industry develops in waves**


The embedded stack didn't arrive all at once. It arrived in waves, and you can see each one in this chart.


The first wave built the fabrication technology itself. The oldest companies are overwhelmingly manufacturing and materials. They built the physical infrastructure of the industry, things like silicon, packaging, substrates, and equipment, and they were durable enough to outlast nearly everyone else.


The second wave was the silicon and IC companies that built on top of that fabrication. By the 80s and 90s, this is where new entrants concentrated. Many started with their own fabs. Most eventually went fabless as the industry consolidated around a small number of foundries. We have already lived through one full cycle of this, and the result is what manufacturing and materials looks like today: a handful of large players who absorbed nearly everyone else.


The silicon, IC, sensor, and module industry is now starting to consolidate the same way. A few big players will remain for a long time. The rest will be bought or quietly die. The survivors won't necessarily be the ones with the biggest book of business today. They will be the ones at the core of the next generation of hardware products being built right now. There is a fresh wave of hardware product companies designing the next 20 years of devices, and the silicon and sensors that find their way into those designs are the ones that come out the other side of consolidation intact.


As pure-spec commoditization continues, that competition is increasingly about something else: which silicon vendors make it easiest to evaluate, adopt, and build on their platforms, and which sensor vendors ship the drivers, SDKs, and integration tooling that get a customer from datasheet to working prototype in days instead of months. From what we’ve seen working with next-gen hardware companies, STMicroelectronics and Silicon Labs are leading the charge here.


## **The rise of software**


The bottleneck has moved.


The thing standing in the way of hardware product companies today is not the capabilities of the silicon, or the designs of the boards, or even the supply chains (more on that later). It is the time it takes to migrate between chips, bring up a board, deploy an AI model to the edge, secure the hardware, build a robust OTA stack, pipe data into the cloud, and test and validate that the hardware works the way you expect.


Look at the chart again. The newest companies in the industry are pointed almost entirely at this bottleneck. They are building the developer tools, the AI, OTA and cloud infrastructure, and the test and validation layers that hardware teams have been quietly begging for. The crossover in the chart happened around 2010, and it has only sharpened since. The 1980s cohort of the stack was 60%+ manufacturing and materials and under 3% software. The 2020s cohort has flipped: 6% manufacturing, 50% software and dev tools.


**What this means**


**If you are an engineer,** the floor for what you can ship just moved. With developer tools like[BootLoop](https://bootloop.ai/) , it is easier to write, test, and debug embedded code than at any point in the history of this industry. The things you have always known you needed but couldn't quite justify the extra work to get (real hardware-in-the-loop testing, near-instant driver generation) are available now.


**If you are a director of engineering,** your bottleneck is firmware, software, and testing - not hardware. Even the things that might look like pure hardware problems need a second look. Supply chain instability is a great example. It is relatively easy to have PCBs produced around a different chip and manufacturers are beginning to encourage this with pin-compatible alternatives. It is almost impossible to take your firmware stack and quickly port it over. That gap is what gets you stuck, and it is finally starting to close. The vendors closing it are the ones worth a serious look this year.


**If you are an IC, silicon, or sensor company,** your moat is no longer the chip. It is the experience around the chip. The customers you want to win in the next decade are running lean firmware teams with no patience for hand-rolled HALs, undocumented errata, or SDKs that haven't been touched since 2019. The vendors who invest now in developer experience, integration tooling, and partnerships with the new wave of embedded dev-tools companies are the ones who will end up at the core of the next 20 years of hardware. The vendors who keep treating software as a checkbox will get consolidated into someone who didn't.


Embedded always has and will continue to be focused on hardware. But the things that have made hardware “hard” have changed. It’s no longer limited by the chip, it’s limited by everything that’s needed to turn capable silicon into a shipping product.


## **Appendix: Full Composition**


For readers who want the unfiltered view, here is the same chart with all 13 industry categories restored, including hardware products and systems, services, distribution, capital, academia, and the unknown bucket.


A few things this view adds. Engineering Services and Corporate Services have grown steadily as the industry has professionalized. Distribution had a long tail of new entrants from 1960-2000 and basically stopped in the 2010s as the channel consolidated. The Capital category (VCs, accelerators, and investors active in the space) is small but newer, mostly post-2000. The Unknown bucket reflects the limit of public information on smaller exhibitors and stays roughly flat as a share over time. The number of Hardware Devices companies - those building the systems that rely on the processors, sensors, and software of others - has roughly flat as a share over time.


## **Methodology**


The dataset is based on the list of companies attending Sensors Converge 2026, enriched with founding year (~78% coverage) and other company-level information from public registry data where available. We classified each company by core product offering into one of 13 buckets using an LLM classifier with native web search and schema-enforced output. To smooth over LLM stochasticity, we ran the classifier three times and took the per-company vote as final: 78% of companies received identical classifications across all three runs, 19% had a 2-of-3 majority, and 2% (47 companies) had no consensus and were resolved by deferring to the first run plus manual review. Overall bucket-level shares shifted <1% between runs showing a high level of stability. Categories with very small sample sizes in the most recent windows (2025-2026, n=11 in the core-stack view) should be read directionally, not precisely. This is a snapshot of who attended SC2026, not a study over time.
