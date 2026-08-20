---
schema_version: "1.0.0"
document_id: "e88ffc76003f1d2ff6efaaad95e9b3d17183a4e2b7838c4b4490d47d5f95ca65"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/rebuild-biotech-for-the-ai-era"
published_at: "2025-09-08T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:c18be1d67a5377cdcb592f55162fec5e90bdb0dbc02b6aebe9dc2219f80f6a88"
---

# Rebuild biotech for the AI era

*Curing disease is a noble cause, but the odds are stacked against us. My perspective on biotech is a little unusual: I’m not a scientist, though I briefly worked in a biology lab. I’m a software engineer who has spent more than a decade immersed in the biopharma industry, building tools for the scientists who drive it forward. The frustrations I hear haven’t changed. Making a new medicine is too slow, too costly, and too uncertain. This essay distills what I’ve learned about where the system is broken and why, with AI as a catalyst, we have a once-in-a-generation chance to fix it.*


*Read*[how we're building an AI Scientist](https://www.benchling.com/blog/ai-scientist-that-deserves-the-name) *with the ambition to get molecules to the clinic twice as fast.*


### Medicines are humanity’s greatest invention, yet our system for making them is broken


Fifty years ago, scientists discovered they could cut and paste DNA, quietly igniting modern biotechnology. Within a decade, synthetic insulin became the first genetically engineered medicine to reach patients. Breakthroughs have followed in waves, with the last decade marked by the rise of biologics and new therapeutic modalities: CAR-Ts that reprogram the immune system, mRNA vaccines taken by over a billion people, and GLP-1s which have transformed obesity into a treatable condition.


These advances should inspire us. Medicines are among humanity’s greatest inventions and behave unlike anything else in healthcare: they are technology. A drug today is the most expensive version it will ever be. Over time it will go generic, becoming cheap and widely accessible, while remaining effective. Most people don’t think twice about taking medicines like statins or ibuprofen, perhaps unaware just how old they are.


In a world of soaring healthcare costs — over $5T a year in the United States alone — only a small fraction ([9%](https://www.cms.gov/files/document/highlights.pdf?utm_source=chatgpt.com) ) goes towards prescription drugs. For all the attention drug prices get, medicines are among the smartest dollars we spend in healthcare. They often prevent far more expensive hospitalizations and chronic care. We should desperately want more and better medicines, delivered faster.


While medicines are a great deal, the process of making them is not. Remarkable science masks an uncomfortable truth: we have not gotten better at creating new medicines. Most drug candidates fail late in clinical trials after incurring enormous costs. It takes over $2 billion and 10 years to bring a drug to market. That frequently quoted statistic has come to define biotech. It is so often repeated that it’s treated like the laws of physics, an inevitability imposed by nature.


But it’s not. It’s a symptom of an industry that’s grown to a trillion dollars of sales without shedding its artisanal roots. We can do science fiction like reprogram cells and edit genomes, but struggle to systematize the process around it. That’s the paradox: our most advanced science is trapped in one of our least efficient systems.


That system is now breaking under economic pressure. One path forward is the status quo: few medicines, high costs, and gradual decline. The other, powered by AI, could bend the curve on how fast we make medicines, and ultimately how many lives we save.


### When drug development costs too much, we dream too small


The biopharma industry has been stuck around 50 new drug approvals annually for the past decade, despite investing over $250B/year in R&D.


Anyone who’s worked in drug development knows why: the journey to a new medicine is a maze filled with dead ends. Companies must identify a biologically meaningful target, design and optimize a molecule, validate it through animal studies, recruit patients, execute multiple clinical trials, scale a manufacturing process, navigate country-by-country regulation, and build a global supply chain.


It is so long, expensive, and failure-prone that startups often sell to pharma once there’s “early” evidence of clinical success after 7-10 years and hundreds of millions invested. Pharma has the infrastructure and expertise to finish the job.


Acquisition as the rational path restricts what medicines we pursue. It is playing moneyball for medicine, swinging for singles and doubles in well-trodden indications while lowering the ambitions of the entire industry. Grand slams happen, more by accident than design. GLP-1s will soon be the biggest medicine of all time and yet obesity treatments were unfundable a few years ago. Keytruda, the most important cancer medicine ever, was an afterthought through multiple acquisitions and almost outlicensed.


Both were hiding in plain sight. The core science behind GLP-1s dates to the 1990s. The Keytruda molecule sat on a shelf until a competitive threat revealed its potential. These stories illustrate a simple truth: the[returns to intelligence in science](https://www.darioamodei.com/essay/machines-of-loving-grace) are extraordinarily high. One well-placed insight and the conviction to act on it can save millions of lives.


Increasing the speed and reducing the cost of making medicines isn’t only about dollars and cents. It’s about freedom for scientists to follow their curiosity and pursue cures to the world’s biggest diseases.


### Better tools can scale modern science


Biotech remains stubbornly artisanal. The word “artisanal” evokes images of craftsmanship, like a hand-assembled watch or carefully brewed espresso. The flip side of craftsmanship is it is slow, expensive, and scarce. We don’t want artisanal medicines; they are far too important.


Every industry must pass through this bottleneck, from craft to composable. Toolmakers can catalyze the transition by standardizing, systematizing, and automating what’s repeatable, freeing experts to focus on what’s novel. For example:


-


Synopsys made chip design modular and verifiable.


-


AWS turned data centers into a metered utility.


-


Stripe unified payments. Figma unified design.


-


NVIDIA turned graphics chips from niche hardware into AI factories.


Biology has seen real systematization in the physical realm. Illumina put DNA sequencers in every lab. New England Biolabs turned cloning into a plug-and-play kit. IDT made ordering oligos as easy as hitting the print button.


But the digital realm, where data lives, collaboration happens, and decisions are made, lags far behind. There is no standardized pipeline or dominant set of tools and practices, making knowledge fragile and difficult to transfer. Each lab reinvents how experiments are designed, samples are identified, complex data are structured, analysis pipelines run, and results are shared.


That’s not even considering work that’s unnecessarily repeated. When a drug program succeeds and a molecule is handed off, or worse it fails and a company shuts down, most of what has been learned is lost. The experiments, data, and processes live on only as tribal knowledge among the scientists there.


It’s not that scientists prefer the artisanal approach to the digital realm. It’s the reality of heterogeneous experiments, biological variability, and the pace of cutting-edge science that doesn’t wait for software to catch up.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


### Cloud brought science online, AI will transform it


By 2012, cloud software was the norm in almost every industry. Yet most scientific organizations were stuck on-premise, in spreadsheets, and on paper. Our earliest challenge at Benchling was evangelizing to bring science online.


Resistance eventually gave way to necessity. Advanced modalities, new petabyte-scale data generation techniques, and complex cross-functional collaboration demanded better tools. Old on-premise software couldn’t keep up. Neither could homegrown software. Pioneering startups couldn’t afford either. Cloud was the delivery vehicle that made better tools accessible.


Benchling helped power that transition; it took the better part of a decade and is still playing out. It turns out that cloud was only a prerequisite, not the end. The current speed and cost of drug discovery is almost uninvestable. AI for science can compress timelines, reduce costs, and scale output so that medicines reach patients in years, not decades.


### Two vectors for AI in science


Every industry is hearing the same message: AI will revolutionize everything.[So far, it hasn’t](https://mlq.ai/media/quarterly_decks/v0.1_State_of_AI_in_Business_2025_Report.pdf) . We believe it can and have a view of how the industry can make progress.


Achieving the potential of AI for science requires data and seamless access. Predictive models can’t be trained without high-quality experimental data. Scientists won’t adopt tools they don’t trust or that add friction to work.


The AI that succeeds is the AI that reaches scientists, embedded directly into their existing workflows like an invisible assistant. We see two main vectors to build towards:


### 1. AI that helps scientists design better molecules


Today, only a small fraction of experiments are informed by simulation. Meanwhile, there’s been an explosion of foundation models that can predict structures, assess biophysical properties, generate molecules, and evaluate everything from safety to manufacturability. While these models are by no means perfect predictors of clinical success, they are quickly improving, and are a valuable design partner for scientists. But they’re too hard to access right now.


Models are the domain of computational specialists. Wet lab scientists might dabble with AlphaFold, but that’s usually where it ends. The overhead is too high: deciding which model to use, how to configure it, and whether to trust the output. It’s often easier to just run the experiment. Today’s models are like GPT before ChatGPT. The leap came with the right interface.


Simulation should be as routine as pipetting. The right model, in the right context, exactly when it’s needed. No command lines, no GPU clusters, and no emailing spreadsheets. A scientist designing antibodies gets immediate developability predictions without leaving their sequence editor and their team’s model is automatically fine-tuned as experimental data are captured. If designed seamlessly, the distinction between “wet lab” and “dry lab” scientist should fade, leaving just scientists moving faster together.


The payoff on even small improvements is huge. Most drug candidates fail late in clinical development, after billions have been spent. Better models, applied earlier in the drug discovery process, can help avoid entire categories of expensive failures and transform early-stage investment.


### 2. Agents that help scientists get back to science


Every day matters for a drug program and for patients. Yet scientists are drowning in tedious, manual work. They must capture data, move it between systems, stitch together program updates, verify complex data analyses, and write report after report. All this effort diverts great minds away from what matters.


AI agents can help scientists get back to science, turning weeks and months of work into minutes. Scientists can instead focus on designing the best possible experiments, while agents surface historical data and suggest improvements. The burden of running the experiment shifts to agents that manage robotic automation, convert instrument data into final analysis, and generate reports that are traceable from raw data to conclusion.


It's underappreciated how powerful agents acting on institutional knowledge can be. We’re already seeing this impact. A scientist recently showed us how our[Deep Research Agent](https://www.benchling.com/blog/complex-questions-fast-answers-benchling-deep-research) helped narrow 20 potential mouse oncology models down to two, saving eight months of redundant animal studies, all by identifying models that had already been tested years ago by an acquired company.


### We are beginning a once-in-a-generation rebuild of scientific infrastructure


Western science faces a perfect storm: China investing heavily in biotech, tariffs disrupting supply chains, basic research under attack, and drug pricing facing political pressure.


We are debating the wrong end of the problem. The price of a medicine is the output of the machinery that makes it. If we want medicines that are more affordable, we need a system that produces more of them and much faster. The artisanal approach that got us here won't get us where we need to go.


Accelerating science means creating tools that systematize it. AI is a catalyst for this, but only if paired with the right infrastructure, giving it the right data and embedding it in scientist’s everyday work.


Over $2B and 10 years isn’t fate. Now is the time to rebuild a biotech industry worthy of its science.
