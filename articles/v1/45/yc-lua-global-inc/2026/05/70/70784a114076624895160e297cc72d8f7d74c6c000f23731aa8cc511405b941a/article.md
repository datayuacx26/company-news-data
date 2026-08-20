---
schema_version: "1.0.0"
document_id: "70784a114076624895160e297cc72d8f7d74c6c000f23731aa8cc511405b941a"
company_key: "yc-lua-global-inc"
company: "Lua Global Inc"
source_id: "yc-lua-global-inc-rss-87d3f9cc68e5"
canonical_url: "https://blog.heylua.ai/a-letter-from-the-cto-chair-what-i-am-actually-seeing-in-ai-right-now/"
published_at: "2026-05-30T09:00:53+00:00"
first_seen_at: "2026-07-24T10:03:36.530538+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:823bcc40f694094b99b72d0012ccf670b1b00e69ccb497491c9bfebeca70c98d"
---

# A letter from the CTO chair. What I am actually seeing in AI right now.

## The cost collapse is real and I do not think enough people have processed what that means


A year ago I was on calls with our finance lead arguing about which workflows we could afford to run on a frontier model. Today I am on calls with frontier labs trying to give me their tokens for free.


The numbers, because they are extraordinary: average cost per million tokens across the major providers has collapsed from roughly $10 to around $2.50 in a single year, an 80% drop ([Finout, May 2026](https://www.finout.io/blog/ai-model-cost-breakdowns-the-complete-2026-comparison-guide?ref=blog.heylua.ai) ). GPT-4o input is down from $5.00 to $2.50 per million ([Finout](https://www.finout.io/blog/openai-vs-anthropic-api-pricing-comparison?ref=blog.heylua.ai) ). OpenAI’s o4 Mini is $0.55 in. Claude Haiku 4.5 is $1.00 in, $5.00 out ([Anthropic](https://platform.claude.com/docs/en/about-claude/pricing?ref=blog.heylua.ai) ). Google’s pitch at I/O 2026 was that moving 80% of frontier workloads to Gemini 3.5 Flash plus a frontier model would save large enterprises over $1B a year ([Investing.com](https://www.investing.com/analysis/the-ai-token-pricing-crisis-behind-openai-and-anthropics-revenue-race-200680777?ref=blog.heylua.ai) ). Both Anthropic and OpenAI now offer ~90% prompt-caching discounts and 50% off batch APIs. The pricing surface has more knobs than most enterprise SaaS contracts I have ever signed.


If you want a single image that captures where we are: ten days ago, on 20 May, Sam Altman walked into Y Combinator and offered every Spring 2026 startup $2M of OpenAI tokens for equity. Uncapped SAFE. One hundred and sixty-nine companies. Equity converts at Series A in the 1% to 4% range. The offer extends to the Summer batch ([TechCrunch](https://techcrunch.com/2026/05/20/sam-altman-makes-mic-drop-offer-to-every-y-combinator-startup/?ref=blog.heylua.ai) ,[The Information](https://www.theinformation.com/briefings/sam-altman-offers-yc-founders-2-million-openai-tokens-equity?ref=blog.heylua.ai) ).


Stop and think about what that actually says. A year ago you could not get enough compute to ship a product. Today the largest AI company on the planet is paying for distribution in the only currency it has surplus of, because the marginal cost of an additional token is approaching zero and the marginal value of being inside the next generation of AI-native software is approaching the entire enterprise software market.


This is what the end of a supply-constrained market looks like. If you are still architecting your product as though tokens are the scarce resource, you are solving a problem that no longer exists.


---


## I keep being asked who survives. Here is what I actually think.


Two days ago, on 28 May, Anthropic closed a $65B Series H at a $965B valuation ([Bloomberg](https://www.bloomberg.com/news/articles/2026-05-28/anthropic-raises-at-965-billion-valuation-eclipsing-openai?ref=blog.heylua.ai) ,[TechCrunch](https://techcrunch.com/2026/05/28/anthropic-raises-65-billion-nears-1t-valuation-ahead-of-ipo/?ref=blog.heylua.ai) ,[CNBC](https://www.cnbc.com/2026/05/28/anthropic-open-ai-startup-value.html?ref=blog.heylua.ai) ). The round was co-led by Altimeter, Dragoneer, Greenoaks, Sequoia, Capital Group, Coatue, and D1. Read the cap table carefully. Samsung, SK Hynix, and Micron joined as strategic infrastructure participants. That is not a financial round. That is Anthropic locking in HBM memory and packaging supply at the cap-table level, because their compute scarcity question is two years long and they know it.


The revenue trajectory underneath the valuation is genuinely without precedent. Anthropic went from $87M ARR in January 2024 to $1B by December 2024, $9B end of 2025, $14B in February, $19B in March, $30B in April, and roughly $45B as of this month ([VentureBeat](https://venturebeat.com/technology/anthropic-says-it-hit-a-30-billion-revenue-run-rate-after-crazy-80x-growth?ref=blog.heylua.ai) ,[SaaStr](https://www.saastr.com/anthropic-just-passed-openai-in-revenue-while-spending-4x-less-to-train-their-models/?ref=blog.heylua.ai) ,[Sacra](https://sacra.com/c/anthropic/?ref=blog.heylua.ai) ). That is a doubling every six weeks, sustained for over a year. I spent ten years scaling payment infrastructure across Africa, and I have never seen anything grow like that. Dario Amodei said publicly that the actual revenue has beaten Anthropic’s internal forecasts by a factor of eight. I believe him. I do not know how anyone forecasts this market.


OpenAI raised $122B in March at an $852B valuation, with Amazon putting in $50B and Nvidia and SoftBank each contributing $30B ([CNBC](https://www.cnbc.com/2026/03/31/openai-funding-round-ipo.html?ref=blog.heylua.ai) ,[Bloomberg](https://www.bloomberg.com/news/articles/2026-03-31/openai-valued-at-852-billion-after-completing-122-billion-round?ref=blog.heylua.ai) ). They hit roughly $25B annualised by February, are tracking around $33B run rate as of mid-2026, and are projected to lose $14B this year alone, with cumulative losses through 2028 of roughly $44B, profitability not arriving until 2029 at the earliest ([Fortune](https://fortune.com/2025/11/12/openai-cash-burn-rate-annual-losses-2028-profitable-2030-financial-documents/?ref=blog.heylua.ai) ,[ainvest](https://www.ainvest.com/news/openai-14-billion-2026-loss-burn-priced-2603/?ref=blog.heylua.ai) ,[Sacra](https://sacra.com/c/openai/?ref=blog.heylua.ai) ). ChatGPT crossed 900M weekly active users in February with about 50M paying subscribers ([TechCrunch](https://techcrunch.com/2026/02/27/chatgpt-reaches-900m-weekly-active-users/?ref=blog.heylua.ai) ). That is roughly a 5.5% conversion. OpenAI is projecting $200B in revenue by 2030. I will believe it when I see it.


The shift that I think most people are still underweighting: **Anthropic has overtaken OpenAI in revenue.** Per Menlo Ventures, Anthropic now captures 40% of enterprise LLM API spend, up from 24% a year ago and 12% in 2023. OpenAI is down from 50% in 2023 to 27%. Google is up from 7% to 21% ([Menlo Ventures](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/?ref=blog.heylua.ai) ,[Yahoo Finance](https://finance.yahoo.com/news/enterprise-llm-spend-reaches-8-130000140.html?ref=blog.heylua.ai) ). Those three together are 88% of enterprise LLM spend. Anthropic’s coding share alone is 54% to OpenAI’s 21%, up from 42% just six months ago, and that is mostly Claude Code eating the developer surface. I see this every week in our own engineering team. We started ours on GPT for code. Today the team uses Claude Code by default. We did not change a vendor on principle. We changed because it is better.


And under all of this sit the hyperscalers, who have collectively committed hundreds of billions in multi-year capex, most of it locked into Nvidia Blackwell forward orders that consume the vast majority of TSMC’s CoWoS packaging capacity through end of 2026 and into 2027 ([Clarifai](https://www.clarifai.com/blog/gpu-shortages-2026?ref=blog.heylua.ai) ,[tech-insider.org](https://tech-insider.org/us-ai-data-center-delays-cancellations-7gw-capacity-crisis-2026/?ref=blog.heylua.ai) ). The widely-cited framing of $700B in capex against $50B in revenue is the cleanest summary of where the imbalance sits ([analyst commentary](https://vinvashishta.substack.com/p/700-billion-in-capex-50-billion-in?ref=blog.heylua.ai) ). This will compress. I do not think it looks like 2000, because the underlying tech is generating real production value at real customers and the cash piles are deep. But by end of 2027 the lab landscape is smaller than it is today, and the conversation we are having about “which lab” will be about two or three names, not five.


So here is the bet, raw and personal. **My money is on Anthropic and Google.** Anthropic survives as the enterprise generalist. The cost curve is already bending the right way, their corporate share is rising, they have a brand around safety and governance that turns out to be the brand that monetises when the regulators arrive, and they have just put memory suppliers on the cap table. Google survives as the integrated specialist. They own the silicon (TPU), the cloud (GCP), the model (Gemini), and the distribution (Search, Workspace, Android, YouTube). When inference cost becomes existential, owning the silicon is the difference between a viable business and a permanent burn rate.


OpenAI is the question I cannot resolve. The consumer brand is the most powerful in tech, but a 5.5% paid conversion against a $14B annual loss is not a business, it is a bridge to a business. The IPO will tell us whether they can cross it. I genuinely do not know.


I will say what I am tired of. I am tired of every conversation about AI companies treating revenue growth as the only metric that matters. A company doubling every six weeks while bleeding $14B a year is not the same shape as a company doubling every six weeks while bending toward positive cash flow. Both are exciting. Only one is durable.


---


## Why every lab is shipping every product. Because none of them has figured out how to charge for the model.


This is the part the press coverage almost always misses.


Look at the product surface. OpenAI ships ChatGPT Plus, ChatGPT Pro, ChatGPT Enterprise, the API, Operator, Apps, the GPT Store, a hardware collaboration with Jony Ive, and now a token-for-equity venture program. Anthropic ships Claude Code, Managed Agents, the desktop app, and an enterprise sales motion that drove them from 24% to 40% corporate share in eighteen months. Google ships Gemini into every product they already own. Most of these are good. Some of them are great. But the *breadth* is the tell. When the price of intelligence at the model layer is collapsing 80% a year and your fixed costs are doubling annually, you ship into every adjacent surface you can find and hope one of them turns into the durable revenue line that pays for the next training run.


This is normal. It is also unstable. The labs that survive will be the ones who figure out which adjacent layer they actually own. Anthropic’s bet is the enterprise platform. Google’s bet is the integrated stack. OpenAI is still hedging.


Here is the thing nobody who builds on top of a frontier API wants to hear, but you have to hear it. **If your product strategy assumes any one of these labs is still the same shape in 24 months, you are going to get hurt.** Pricing will change. Features will move behind enterprise tiers. APIs will be deprecated and the migration path will be ugly. Models you fine-tuned for will be retired. A lab will get acquired by a hyperscaler and the SLA you signed will be renegotiated. A lab will pivot a business unit and your integration partner will be unreachable for three months.


Build for portability or stop being surprised when it bites you. This is not theoretical. This is what we live with at Lua. The customer’s policy, the customer’s memory, the customer’s audit chain, the customer’s agents, all of it sits above the model layer in our architecture, so the model becomes interchangeable infrastructure underneath. That was not a clever design choice. That was a forced move because we did not trust any one lab to be the same shape next year.


You should not trust them either.


---


## The next 24 months are about small models running close to the data, and I think this is more important than the frontier story


The most interesting curve in AI right now is not the frontier capability curve. It is the open-source efficient-model curve. And almost nobody outside the engineering org is paying attention to it.


Llama 4, Qwen 3.5, DeepSeek V3.2, GLM-5, Google’s Gemma 4. All of these are now matching or beating proprietary alternatives on key benchmarks while running on commodity hardware that costs a fraction of an H100-class training rig ([HuggingFace](https://huggingface.co/blog/daya-shankar/open-source-llms?ref=blog.heylua.ai) ,[ComputingForGeeks](https://computingforgeeks.com/open-source-llm-comparison/?ref=blog.heylua.ai) ). Gemma 4 fits in 14GB and runs at 85 tokens per second on consumer hardware. DeepSeek Coder V2 Lite uses mixture-of-experts to run a 16B-parameter model on a 12-16GB GPU by activating only a subset of parameters per token ([Morph](https://www.morphllm.com/best-ollama-models?ref=blog.heylua.ai) ). Research is now showing credible CPU-only inference for small models on mobile devices ([arXiv](https://arxiv.org/pdf/2505.06461?ref=blog.heylua.ai) ). The H100-or-bust narrative is dying.


Apple’s WWDC starts in nine days, on 8 June. On-device AI is the central announcement ([Apple Newsroom](https://www.apple.com/newsroom/2026/05/apple-kicks-off-worldwide-developers-conference-on-june-8/?ref=blog.heylua.ai) ,[MacRumors](https://www.macrumors.com/2026/05/28/apple-to-make-on-device-ai-key-focus/?ref=blog.heylua.ai) ,[AppleInsider](https://appleinsider.com/articles/26/05/28/apple-doubling-down-on-on-device-ai-at-wwdc-2026?ref=blog.heylua.ai) ). Their published 3B-parameter on-device foundation model already handles writing tools, notification summarisation, photo semantic search, and personal context queries entirely locally. Their Private Cloud Compute architecture handles the in-between cases without your data ever leaving Apple silicon ([Apple ML Research](https://machinelearning.apple.com/research/introducing-apple-foundation-models?ref=blog.heylua.ai) ). Reports also suggest Apple has cut a deal with Google to use a distilled version of Gemini for the heavier local model alongside their own foundation work, which is the kind of pragmatic move you make when you do not have time to wait for your own larger model to ship.


I want to be clear about why this matters, because it is not what most people think it is. Apple is not doing this for privacy theatre. Apple has done the math on inference cost at hyperscaler datacenter rates and concluded that running it on the silicon they already ship is structurally cheaper. The privacy story is the marketing wrapper. The economics is the strategy.


If that is right, and I think it is, the next 24 to 36 months are about every enterprise CIO finally asking out loud the question their CISO has been asking quietly for three years: **why is our data going to someone else’s GPU?** I have had this conversation with bank CTOs in Abu Dhabi, with conglomerate CIOs in Cairo, with central-bank-regulated payment processors in Lagos. Every single one of them. The answer used to be that there was no alternative that performed. That answer is dead.


What we will see, and we are already seeing it in our pipeline, is customers deploying their own models on their own hardware. Some will use commodity Nvidia. Some will use Apple Silicon. Some will use Google TPU. Some will rent regional GPU capacity in their own jurisdiction. The substrate is fragmenting. The question that decides who wins the next decade of enterprise software is: who builds the layer above that substrate that makes all of these choices interchangeable?


That is what we are betting on. I will not pretend otherwise.


---


## The bottleneck is not capability. It is governance. And almost nobody is built for it yet.


This is the part I am most certain about, because it is the customer conversation I have every week. And I am tired of how few people in the industry actually take it seriously.


Gartner has projected that 40% of enterprise applications will include task-specific AI agents by end of 2026, up from less than 5% in 2025 ([Techment](https://www.techment.com/blogs/agentic-ai-orchestration-scalable-ai-2026/?ref=blog.heylua.ai) ). That number is going to land. The capability is there. The frontier labs have given us enough raw model performance to automate most of the white-collar workflows our customers actually run.


What is gating the next leg is not capability. It is governance. It is the audit trail. It is the data residency posture. It is whether the CISO can answer *what happened, when, who approved it, why* when the regulator calls.


I will give you a concrete example. We were in a meeting last quarter with a UAE-licensed financial institution that wanted to deploy an agent that triaged customer support cases. The agent worked. We demoed it. The Head of Operations loved it. The CISO blocked the deal. Not because the agent was unsafe. Because they could not show their regulator a single record of which model the agent used, which policy approved each action, which customer data was queried, and where the audit log lived. That is the conversation, every week, in every regulated industry in every market we sell to.


The EU AI Act becomes fully applicable in nine weeks, on 2 August 2026 ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai?ref=blog.heylua.ai) ,[artificialintelligenceact.eu](https://artificialintelligenceact.eu/implementation-timeline/?ref=blog.heylua.ai) ). The Commission’s supervision and enforcement powers against GPAI providers turn on the same day. High-risk system rules extend to 2 December 2027. The EU’s “Digital Package on Simplification” reached political agreement on 7 May ([Latham & Watkins](https://www.lw.com/en/insights/ai-act-update-eu-resolves-to-change-rules-and-extend-deadlines?ref=blog.heylua.ai) ), which softens some deadlines but does not change the trajectory.


In our home markets the picture is the same shape and in many cases more prescriptive. The UAE AI Office, the Saudi PDPL framework, and the data protection regulators across Kenya, Nigeria, Egypt, and South Africa are all converging on a posture that requires enterprise AI to be governable, auditable, and increasingly deployable inside the customer’s jurisdiction. The MENA regulators, contrary to every Western assumption I had when I started this company, are *ahead* of the US on the operational specifics of what AI governance has to look like.


If you are building enterprise software and your roadmap does not have an answer for this, and by an answer I do not mean “we will add governance later” but “here is how a CISO can answer the regulator’s question on day one,” your roadmap has a hole. The hole is going to bite you in the next 12 months. I am writing this in a letter rather than a memo because I want you to actually feel that, not nod and scroll past.


---


## What I would watch over the next 12 to 24 months. With my conviction levels labelled.


This is a prediction list. Predictions in AI right now have the half-life of a poorly cached prompt. I am giving you my conviction level on each one so you know which ones I would bet on and which ones I am hedging.


**1. Token costs halve again. High conviction.** Industry average drops from ~$2.50 per million today to roughly $1.00 to $1.25 by mid-2027. Caching and batch discounts push the effective floor for cache-heavy workloads under 50 cents per million. The labs that survive figure out how to charge for something other than raw tokens. I would bet the company on this curve. We have.


**2. One frontier lab does something structural. High conviction.** Acquires a smaller lab. Gets acquired by a hyperscaler. Files the IPO Anthropic is widely reported to be heading for. Splits its consumer and enterprise businesses. The pricing environment makes the current four-or-five-frontier-labs landscape unstable. By end of 2027 it is two or three.


**3. Open-source models hit 30B-parameter parity with 2024-class frontier at a fraction of inference cost. High conviction.** This is the curve that resets the conversation about whether enterprises rent or own. Apple’s WWDC announcement is the next visible accelerant.


**4. On-prem and sovereign-region deployment becomes a default ask in enterprise procurement. Very high conviction.** For the regulated and sovereign segment, table stakes. The platforms that ship on-prem alongside SaaS, on the same release train, win the high end. This is not a prediction. It is what is already happening in our pipeline.


**5. Governance and audit infrastructure hits product-market fit at scale. Very high conviction.** Forrester has already named the category. By end of 2026 the AI control plane is a budget line in the typical F2000 CIO planning cycle. By end of 2027 it is *the* budget line. I would not bet a company on this thesis if I were not already betting on it.


**6. A regulator does something that resets the conversation. Medium-to-high conviction.** EU AI Act high-risk classifications start being enforced. A US state passes something stricter than the federal posture. A Gulf regulator imposes a hard data-residency rule on financial services AI. The geography is the variable. The trigger is coming.


**7. The on-device-plus-cloud hybrid pattern becomes the default new architecture. Medium conviction.** Small fast models close to the data, frontier reserved for the cases where it is genuinely necessary. WWDC on 8 June is the likely accelerant. If Apple actually delivers what is being reported, this gets pulled forward.


**8. The agent market starts to look like the early SaaS market. Medium conviction.** A few dozen vertical specialists, a few horizontal substrate platforms. The substrate is where the value compounds because the agents are interchangeable and the substrate is the lock-in. I am betting on this one with our product roadmap. I could be wrong.


---


## If you are an engineering leader. Read this part.


The strategic implications. This is the part I would write on the back of a napkin and hand to a friend who runs another engineering org.


**Do not bet the company on a single frontier lab.** Read what I wrote in section 3 again. The pricing collapse, the funding pressures, and the diverging strategies make single-vendor lock-in materially riskier than it was 12 months ago. Build for portability. Run evals across multiple providers. Treat the model layer as commodity infrastructure and architect accordingly. If your engineers cannot swap providers in a week, your architecture has a flaw.


**Inference cost is not going down. It is just going down per token.** Falling token prices do not mean your bill goes down. They mean your usage goes up faster than the per-token price falls. Plan for the workload to grow into the available capacity. Watch unit economics like a hawk.


**Build the governance layer now. Not later.** Not because regulation is coming, although it is. Because every meaningful enterprise sales cycle stops dead at the question of audit, policy, residency, and accountability. If your platform cannot answer those four questions on day one of a procurement cycle, you will lose deals you should win, and you will not understand why.


**Build for hybrid local-and-hosted from the start.** The architecture that assumes “we will just call GPT” is about to look as dated as the architecture that assumed “we will just put everything in S3” did in 2018. Plan for the world where your customer’s data lives on their hardware, in their region, behind their firewall, and the heavy frontier call happens only when it has to.


**Read the regulators, not the headlines.** The actual published guidance from the EU AI Office, the UAE AI Office, the Saudi PDPL framework, NIST, the relevant state AGs. The companies that quietly built compliance into their substrate in the last 18 months are the ones whose customers do not have to choose between buying their product and passing their next audit. That is going to be the most undervalued moat in enterprise software for the next five years.


**Pick your durable bet. Build for ten years.** The model layer is commoditising. The substrate layer above it is wide open. The customer relationship below it is up for grabs. Decide which of those three you intend to own, and build for the long version of the answer.


---


## What I actually believe, written down


The technology curve is real and it is bending faster than I expected when we started this company a year ago. The economics under the hood are unstable and will reset at least once before the dust settles. The regulatory and sovereignty layer is becoming load-bearing in a way that almost nobody outside the customer-facing teams is pricing in yet. And the next durable layer of value, the one I think the next decade of enterprise software gets built on, is being built right now in the boring middle: the substrate that makes enterprise AI auditable, portable, deployable inside the customer’s perimeter, and ownable.


That is what we are building at Lua. That is also what I think the next 12 to 24 months are going to be about, whether you build at Lua or somewhere else.


If you are inside a frontier lab right now, you are running a fascinating, brutal race against your own cost curve. If you are inside a hyperscaler, you are placing the biggest capex bet of your professional life. If you are inside an enterprise CIO function, the buyer’s market is finally starting to form and you should be using your leverage. And if you are an engineering leader at a company building on top of all of this, your job for the next two years is to make architecture choices that survive whichever way the next 12 months break.


That is my view from here. I will be wrong about parts of it. I have written down the parts I will hold the line on.


If any of this helps, write back. If you think I am wrong, definitely write back.
