---
schema_version: "1.0.0"
document_id: "2d0c343ff113c72cf496a263fe670e3636e2159cb37b027d0f903105ec136fb3"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/powering-ai-innovation/"
published_at: "2025-03-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:395fd6bf2a85c98e3f3b03e0023b24d50905d94f855449e10daf059bcb2132ac"
---

# Powering AI Innovation

[Home](https://www.advancedenergy.com/en-us/) /


[About AE](https://www.advancedenergy.com/en-us/about/) /


[News & Events](https://www.advancedenergy.com/en-us/about/news/) /


[Blog](https://www.advancedenergy.com/en-us/about/news/blog/) /


Powering AI Innovation


# Powering AI Innovation


### Posted


March 30, 2025 by


[Harry Soin](https://www.advancedenergy.com/en-us/about/news/authors/harry-soin/)


Blog Summary


1. Rising Energy Consumption:


Data center energy consumption is projected to exceed 1,000 terawatt hours (TWh) next year, driven by the rapid adoption of generative AI technologies. This is double the energy usage of 2022 and is expected to triple by 2030.


2. Efficiency Improvements:


Despite the increasing power demands, there have been significant efficiency improvements in GPUs and algorithms. For example, NVIDIA’s GPUs have improved efficiency by 80% over two generations, and new algorithms like those used by DeepSeek are claimed to use half the energy per interaction compared to ChatGPT.


3. Power Delivery and Cooling:


The power supply industry is evolving to meet the higher power demands of AI systems. Advances in power density and cooling technologies, such as liquid cooling, are essential to support the increasing power requirements of modern AI infrastructure.


---


According to estimates from the International Energy Agency (IEA), data center energy consumption is potentially set to exceed 1,000 terawatt hours (TWh) next year. This is more power consumed than many countries.


To put these figures in context, Goldman Sachs data suggests that this usage is approximately double 2022's usage, and the share of total power used by data centers is set to more than triple over this decade to 2030. The reason for this acceleration... generative AI.


Generative AI has been around for many years but was not readily adopted until recently. Now it has become one of the fastest growing technologies due to a record rise in adoption levels in the commercial and business sectors. To put this in context, OpenAI, which was founded in 2015, introduced ChatGPT as a product in November 2022, and in just five days, it reached 1 million users. Similarly, since coming on the scene in January 2025, DeepSeek reached 1 million registered users in less than two weeks. As a comparison, Instagram took approximately 2.5 months to reach 1 million users, Spotify took five months, Facebook took 10 months, and Netflix took nearly 3.5 years.


This unprecedented growth is straining the supply of power.


Reducing AI power consumption


Increasing consumption is posing a challenge for hyperscale companies, requiring more power without compromising efficiency. There have been generational efficiency advances made by GPU developers. In just two generations (DGX A100=>DGX H100=>DGX B200), NVIDIA’s GPUs have improved efficiency by 80%, with the DGX 200 Blackwell consuming 0.2 kW per petaFLOPS.


Elsewhere, advances in training and inference algorithms are also helping gain efficiency and reduce energy use. For example, it has been claimed (caution should be applied due to the lack of verified available information) that DeepSeek uses approximately half the energy per interaction as ChatGPT. If this is true, this is a significant breakthrough in LLM training, especially as it has been undertaken (or is presumed to have been undertaken) on non-leading-edge chipsets, such as NVIDIA’s H800. H800 was created for the Chinese market that meet the U.S. export control requirements and would be significantly less capable.


AI infrastructure


Even with these efficiency gains, electricity consumption is still increasing, and the faster leading-edge processors require significant power.


If we look at two of the most recent GPUs, Hopper and Blackwell, despite becoming more efficient per operation,[these still consume](https://www.datacenterdynamics.com/en/news/new-nvidia-blackwell-accelerator-will-consume-1kw-of-power/) 700 and 1000 watts respectively. There is also the assumption that NVIDIA’s next generation of AI processer (Rubin) will increase this further.


However, the latest generations of LLMs require increased amounts of compute power. For instance, Meta’s Llama 3.1 405B model, which launched last year,[required 38 billion petaflops](https://developer.nvidia.com/blog/nvidia-contributes-nvidia-gb200-nvl72-designs-to-open-compute-project/) of accelerated compute to train. Therefore, training and serving these large models needs to be parallelized across massive GPU clusters such as NVIDIA’s GB 200 NVL72, which brings together 72 GPUs (or 144 GPUs based on NVIDIA’s updated definition).


Such systems have also needed a redesign in terms of how they are powered, with the industry undertaking a shift from the standard form factor PSUs to the[ORv3-based power shelf](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/power-shelves/ocp-compliant/?bz-page=1&bz-results-per-page=10) . The ORv3-based power shelves fit into a 21-inch-wide rack, with each power shelf holding six ORv3 PSUs, with one additional slot for the power shelf management controller (PMC).


*Fig. 1. ORv3 Power Shelf*


Increasing Power Delivery


The power supply industry has responded to the challenge of the rapidly changing datacenter requirement with PSUs power density increasing significantly, especially through advances in wide bandgap technologies. For example, Advanced Energy’s ORv3 PSU has almost doubled the power supplied from the previous generation, from 3 kW per PSU (18 kW per power shelf) to 5.5 kW per PSU (33 kW per power shelf and 100 kW racks) by December 2024. This output will double again by the end of 2025 with up to 12 kW per PSU (up to 72 kW per shelf).


This 21-inch shelf delivers an output voltage of 50 V and an output current of 600 A, with a peak efficiency of 97.5%. This efficiency percentage is significantly above the industry’s highest efficiency standard of[80 Plus Titanium](https://en.wikipedia.org/wiki/80_Plus) at 96% (at 50% of load in a 230 V internal redundant supply).


Future evolutions


Even with advances in GPU and algorithm efficiency improvements that are taking place, the meteoric rise of AI will continue, and higher power will be needed by AI systems.


Power consumption and therefore power delivery and power density will need to rise further. This, in turn, leads to cooling. The limit for air-cooled supplies is close to 100 kW per shelf, which means that to reach 200 kW per shelf, or more, liquid cooling technology is needed. With traditional air-cooling at its limit for servers supporting more power-intensive AI processing, a transition to liquid-cooling will be the future.


For more information on Advanced Energy’s ORv3-standard power supplies and power shelves please visit the[OCP Compliant Power Shelves page.](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/power-shelves/?bz-page=1&bz-results-per-page=10)


Share


### Harry Soin


Advanced Energy


Harry Soin is a Senior Director of Technical Marketing, Hyperscale at Advanced Energy with over 25 years of global technical design and leadership experience.


[More posts by Harry Soin](https://www.advancedenergy.com/en-us/about/news/authors/harry-soin/)


Browse


[Categories A-Z](https://www.advancedenergy.com/en-us/about/news/categories/)


Latest from AE


[News & Press Releases](https://www.advancedenergy.com/en-us/about/news/press/)[Videos](https://www.advancedenergy.com/en-us/about/news/videos/)[Trade Shows & Conferences](https://www.advancedenergy.com/en-us/about/news/events/)[Webinars](https://www.advancedenergy.com/en-us/about/news/webinars/)


Join Our Mailing List


Subscribe


Recent Posts


[View on X](https://twitter.com/advenergy)
