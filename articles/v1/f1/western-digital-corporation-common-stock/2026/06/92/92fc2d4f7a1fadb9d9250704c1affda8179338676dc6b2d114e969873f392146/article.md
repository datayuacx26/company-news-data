---
schema_version: "1.0.0"
document_id: "92fc2d4f7a1fadb9d9250704c1affda8179338676dc6b2d114e969873f392146"
company_key: "western-digital-corporation-common-stock"
company: "Western Digital Corporation"
source_id: "western-digital-corporation-common-stock-rss-a1b03adbb2f1"
canonical_url: "https://blog.westerndigital.com/ai-storage-demand-data-center-build-out/"
published_at: "2026-06-08T16:11:18+00:00"
first_seen_at: "2026-07-20T23:21:38.045244+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:7a35dd997d560dbc40f5488b4d0acc0e48e3ac05112f18615907138bf6aa2301"
---

# AI Doesn’t End at Compute—It Compounds as Data

June 8, 2026


8 min read


[Data](https://blog.westerndigital.com/category/data/)


# AI Doesn’t End at Compute—It Compounds as Data


[Brad Warbiany](https://blog.westerndigital.com/?guest_author=brad-warbiany)


## Key Takeaways


1. AI storage demand is **cumulative and persistent** , unlike compute demand, which is event-driven and tied to build-out cycles.
2. Every AI inference interaction generates new data—output, metadata, and logs—all of which require long-term storage.
3. WD projects **significant growth in HDD exabyte demand** , driven significantly by AI data generation beyond the initial build-out phase.
4. AI-generated content multiplies across platforms and use cases, compounding storage requirements at each stage of the data lifecycle.
5. Even if AI infrastructure build-out decelerates, **existing AI factories continue generating storage demand** through ongoing inference and model operations.


The last year has seen a remarkable growth in demand across the entire chain of AI infrastructure. With 2026 hyperscaler CapEx forecast by Data Center Knowledge at an eye-popping $700B, and projected to rise to $820B next year,1 it’s showing no signs of slowing down.


Much of the focus has been on GPU, the “engine” of the AI workflow. But nearly every component needed to build out an AI data center has been in short supply. Whether it’s networking, DRAM/HBM, flash, HDD, or even physical rack hardware and available power generation, it seems that demand is consistently outstripping supply.


AI factory build-out is a tremendous demand tailwind for an HDD industry already in secular growth. WD projected in our February 2025 Investor Day a “base case” of 15% CAGR exabyte (EB) demand growth, and as high as 23% with AI uplift.2 Unrelated to AI, the world keeps making more data. A year later, that AI uplift has materialized, with our projection today of 25% CAGR,3 even above the February 2025 estimates. For an HDD industry with already full sails, the AI build-out is only accelerating growth.


But this is only part of the story. The focus is overwhelmingly on AI data center build-out, and the demands it is placing on supply chains. What is missing is what is *driving* all of this build-out. AI is merely a tool; it’s what that tool is used for and how that is the real story. The build-out continues because we think we need more tools, and we need more tools because what AI produces is in high demand.


## AI data center as a tool for building new data


So the question on the tip of everyone’s tongue is: What if this AI data center build-out slows down, or stalls? What does that mean for HDD?


To answer that question, it’s important to understand the role of HDD storage in AI. And that story starts with data. **Data is both the foundation of AI—and its output.** All of the GPUs and memory and networking are useless without the massive data lakes and training sets that inform the models. And without demand for the output of the models—even more data—the models wouldn’t exist. So, AI not only relies on data to function, it’s also generating new data with every input prompt. Every training run, every inference, every interaction creates new data that must be stored, managed, and retained. In the cloud, that data overwhelmingly lives on HDDs.4


Beyond output, data created with AI often “multiplies” in underappreciated ways. For example, the (hilarious) social media account[DogPack](https://www.instagram.com/officialdogpack/?hl=en) uses AI to generate podcasting dog videos.5 If you think of their process, each AI video will generate data that must be stored on the AI system—perhaps in many cases interim production that is stored before the final video is ready. These videos will then be downloaded and stored locally by the creators. And of course, they will be uploaded to social media.


But not just one social media site—they are on Instagram, YouTube, Facebook, and TikTok, as well as their own app. Between the initial creation site, the creators’ storage, and these multiple social media platforms, that’s at least seven copies of every video that will be stored. Of course, social media influencers releasing videos cross-platform is not new, but the ability for AI to accelerate the speed of video content creation is. Each new AI video created drives an incremental and multiplied storage demand.


We can think of an operational AI model the same way we might think of a car. A car is a collection of components (engine, transmission, seats, etc.) and a computer controlling it all. Once a customer buys the car, all they need to do is keep it fueled and maintained, and it can fulfill its purpose of transportation for many years and miles. In many ways, an operational AI model is the same in that it requires a collection of components, GPU/DRAM/flash/networking, and a trained LLM controlling it all. Once you have bought the hardware and model, you have the ability to generate responses to user prompts for many years. Buying the car or the model is an event—but it then becomes an asset that may deliver benefits for many years.


## What is an AI factory, really?


When we look at the relationship between AI data center build-out and data storage demand, we must remember why we call these data centers “AI factories” in the first place. When we think of factories, we think of large buildings that exist to produce something else. In this case, that “something else” is data. Which means while existing automotive factories could be described as car generators, existing AI data centers are data generators. Thus, every already-existing AI data center is now filling up hard drives with newly stored data. The factory isn’t the story; the output— **data** —is the story.


We can even think of every existing AI data center as a circular generator of data. They are trained on large-scale datasets that need to be persistently stored, because future versions of models will need to be trained. During training and inference, they generate a tremendous amount of metadata that must be retained to understand model behavior and drive further improvement. And during inference, the model is generating new data that must be stored. Both the metadata and the newly generated data—as well as synthetic data expressly generated for the purpose of model training—can then be incorporated to improve and update the training of future models.


## AI is a data system, not just a compute system—and it’s accelerating


Compute hardware is deployed in discrete events. It is continuously recycled in use. It is built, optimized, and eventually upgraded. The demand for compute is based on how much we think we’re going to need to use on any given day.


Data behaves differently. It is persistent. It accumulates. It compounds. The demand for storage is based on what we think we’ll need long-term to store **everything** we’ve produced.


When we think about demand for GPU, networking, DRAM, or the myriad other equipment needed to build an AI data center, we can think of this demand as fulfilling an event. The demand for these components is based on how many AI data centers are *built* .


Meanwhile, the demand for storage—HDD storage in particular—grows during the course of operating the AI factory. It is an ongoing and cumulative demand, driven by inference. The demand for HDDs is based on how much the AI data centers are being *used* .


And what we see is that use is accelerating, both with deployment of new architectures and efficiency gains in existing deployments. In his 2026 NVIDIA GTC keynote, NVIDIA CEO Jensen Huang highlighted an existing customer that saw an increase in average tokens/second generated from 700 to almost 5,000, a seven-fold increase, merely through software algorithm efficiency upgrades.6 Additionally, the tight coupling of all layers of the architecture cited led to NVIDIA’s Vera Rubin supercomputer being 35 times more efficient per megawatt in token generation than the previous architecture. These efficiency improvements enable more inference, and each increase in inference means more data is generated and must be stored.


This is driving real storage demand. Omdia Research, for example, recently stated that “In a recent research study, three quarters of enterprises said they are anticipating their data growth rates to be higher or significantly higher over the next 24 months, versus the previous 12 months,” citing AI as the top reason.


## But what if the build-out slows?


The tailwind of AI data center build-out is real, and it is affecting the rate of *growth* of the HDD storage industry. But it’s important to understand that HDD storage was already a secular growth story, and is already experiencing stronger demand based upon *existing* AI data center deployments continuously generating new data. Even if AI infrastructure build-out slows, the data already created—and continuously generated—must still be stored, accessed, and retained. This makes storage demand structurally different from compute demand.


1. [https://www.datacenterknowledge.com/hyperscalers/hyperscaler-capex-snowballs-toward-700b-as-firms-stage-ai-capacity-builds](https://www.datacenterknowledge.com/hyperscalers/hyperscaler-capex-snowballs-toward-700b-as-firms-stage-ai-capacity-builds)
2. [https://investor.wdc.com/static-files/e16a6f3b-49af-4eea-90a2-6220aa24ebc3](https://investor.wdc.com/static-files/e16a6f3b-49af-4eea-90a2-6220aa24ebc3)
3. [https://www.investing.com/news/transcripts/western-digital-at-morgan-stanley-conference-strategic-growth-amid-ai-boom-93CH-4538788](https://www.investing.com/news/transcripts/western-digital-at-morgan-stanley-conference-strategic-growth-amid-ai-boom-93CH-4538788)
4. IDC Source: Worldwide IDC Global StorageSphere Forecast, 2024-2028
5. [https://www.businessinsider.com/ai-dog-podcasters-dogpack-sign-with-talent-giant-wme-2025-10](https://www.businessinsider.com/ai-dog-podcasters-dogpack-sign-with-talent-giant-wme-2025-10)
6. [https://www.investing.com/news/transcripts/nvidia-at-gtc-2026-ai-expansion-and-strategic-partnerships-93CH-4564073](https://www.investing.com/news/transcripts/nvidia-at-gtc-2026-ai-expansion-and-strategic-partnerships-93CH-4564073)


[Artificial Intelligence](https://blog.westerndigital.com/tag/ai/)[Data Center](https://blog.westerndigital.com/tag/data-center/)[Technology and Strategy](https://blog.westerndigital.com/tag/technology-and-strategy/)
