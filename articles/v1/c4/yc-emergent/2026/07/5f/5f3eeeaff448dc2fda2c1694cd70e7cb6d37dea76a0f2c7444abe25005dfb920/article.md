---
schema_version: "1.0.0"
document_id: "5f3eeeaff448dc2fda2c1694cd70e7cb6d37dea76a0f2c7444abe25005dfb920"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/gpt-5-6-launch"
published_at: "2026-07-09T23:48:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:108b3af0248643c147d989d6312842847609c0d20e721632893091fad8d26182"
---

# GPT-5.6 Launches Today: Everything You Need to Know

OpenAI just shipped three models and three products at once. GPT-5.6 Sol, Terra, and Luna are[now generally available](https://openai.com/index/gpt-5-6/) across ChatGPT, Codex, and the OpenAI API, following a[limited preview that began on June 26](https://openai.com/index/previewing-gpt-5-6-sol/) . Alongside the models, OpenAI also launched[ChatGPT Work](https://openai.com/chatgpt-work/) (a new mode for complex tasks), a redesigned desktop app with local file and browser access, and Hosted Sites for publishing interactive web pages directly from ChatGPT.


That is the headline worth paying attention to. Not just "new model is smarter," but "new model does more work per dollar." For anyone building products with AI, that shift changes the math on what you can afford to ship.


## Three Tiers, One Family


GPT-5.6 is not a single model. It is a family of three, each designed for a different job.


**Sol** is the flagship. According to[OpenAI's announcement](https://openai.com/index/gpt-5-6/) , it scored 53.6 on Agents' Last Exam, a benchmark that tests long-running professional workflows across 55 fields. OpenAI says that is 13.1 points above[Claude Fable 5](https://emergent.sh/learn/what-is-claude-fable-5) with adaptive reasoning.


On the Artificial Analysis Coding Agent Index, Sol with max reasoning scored 80, which OpenAI reports is 2.8 points above Fable 5, while using less than half the output tokens and costing about one-third less.


Sol also introduces an ultra setting that coordinates four agents working in parallel. OpenAI says this trades higher token usage for stronger results and faster completion on demanding tasks, with the option to scale up to 16 parallel agents.


**Terra** is the mid-tier option. OpenAI positions it as delivering performance competitive with GPT-5.5 at roughly half the price. On the Artificial Analysis Coding Agent Index, Terra scored 77.4, which OpenAI says is just above Fable 5's 77.2. On Agents' Last Exam, Terra scored 50.4, above GPT-5.5's 46.9.


**Luna** is the cost-efficient model. OpenAI says it outperforms Claude Opus 4.8 on the Coding Agent Index (74.6 vs. 72.5) and handles tasks in roughly one-third the time at approximately one-quarter the estimated cost. For high-volume workloads where speed and cost matter more than peak reasoning, Luna is the intended choice.


## What GPT-5.6 Actually Does Better


Beyond the benchmark numbers, OpenAI highlights three capability areas that matter for anyone building products.


**Design judgment**


OpenAI says GPT-5.6 delivers a "step change in design judgment." With only high-level direction, it can create functional interfaces. Its computer-use capabilities let it inspect the rendered result, not just the code, and catch visual and functional issues before handing the work back. The blog post includes examples of games, websites, and interactive visualizations built entirely from natural-language prompts.


For non-technical builders, this is significant. Generating a working interface from a description is one thing. Having the model inspect what it built and fix problems before you even see the output is a meaningful quality improvement.


**Knowledge work**


OpenAI says GPT-5.6 Sol improves quality in presentations, documents, and spreadsheets. It can create fully editable presentations from scratch, and the improvement is especially pronounced when following templates and reference decks. According to OpenAI, GPT-5.6 can infer a deck's design system, including layouts, typography, spacing, colors, and rules embedded in the Slide Master, and apply those conventions consistently to new material.


The blog post includes a side-by-side comparison where GPT-5.5's output is missing key components from a master slide, while GPT-5.6 follows the reference structure more faithfully. OpenAI also claims improvements in equations, financial models, typography, and page layout.


**Efficiency per token**


OpenAI says GPT-5.6 can write and run lightweight programs that coordinate tools, process intermediate results, and choose the next action as work unfolds. They call this Programmatic Tool Calling. The practical result, according to OpenAI: tool-heavy tasks advance with fewer tokens, fewer model round trips, and less manual guidance.


## Not Just Models: Three Product Launches


GPT-5.6 did not ship alone. During the[July 9 livestream](https://www.youtube.com/live/Wq45rvPGNHs?si=pXWRBhaowza57ocR) , OpenAI announced three product updates alongside the models.


**ChatGPT Work** is a new mode inside ChatGPT, separate from regular chat, designed for complex multi-step tasks. It works across web, mobile, and desktop. During the livestream, OpenAI's finance team demonstrated it pulling data from Slack, running a variance analysis, updating an Excel forecast model, generating a PowerPoint presentation, and publishing an interactive dashboard as a shareable site, all from a single conversation. Free and Go users get Terra in Work mode. Paid users get Sol.


**A redesigned ChatGPT desktop app** now has access to local files, open browser tabs, and other apps on your computer. In the demo, a team member dragged in a spreadsheet, pointed ChatGPT at a folder of mixed documents (PDFs, security reviews, user research interviews) plus three open Chrome tabs, and got a fully formatted slide deck back in about 90 seconds. The app can also operate other applications directly using computer use, with its own cursor running in the background.


**Hosted Sites** lets you publish interactive websites directly from ChatGPT Work with one click. The livestream showed finance dashboards, collaborative tracking tools, interactive prototypes, and 3D visualizations, all generated by the model and shareable via link. Hosted Sites is available to all paid users.


For builders, the product launches may matter more than the benchmark numbers. ChatGPT Work turns the model into something closer to a work partner that can act across your tools. Hosted Sites turns it into a publishing tool. These are not API features that require code to use. They are ready to go from the ChatGPT interface.


## Pricing: The Real Story


GPT-5.6 pricing per 1 million tokens,[according to OpenAI](https://openai.com/index/gpt-5-6/) :


- **Sol:** $5 input / $30 output
- **Terra:** $2.50 input / $15 output
- **Luna:** $1 input / $6 output


For context, GPT-5.6 also introduces more predictable prompt caching. OpenAI says cache writes are billed at 1.25x the uncached input rate, while cache reads receive a 90% cached-input discount, with a 30-minute minimum cache life.


The tiered pricing is where this gets practical. Luna at $1/$6 per million tokens gives builders a cost-efficient option for high-volume tasks. Terra at $2.50/$15 delivers what OpenAI claims is GPT-5.5-level performance at half the cost. Sol at $5/$30 is the premium tier for complex work. The ability to route different tasks to different tiers based on complexity is where the real savings add up.


## How Benchmarks Stack Up


Here are the key benchmark results OpenAI reported, with the caveat that these are vendor-reported figures from[OpenAI's own announcement](https://openai.com/index/gpt-5-6/) . Independent verification will fill in the full picture over time.


**Professional tasks (Agents' Last Exam):** Sol scored 52.7% (53.6% with max reasoning). Terra scored 50.4%. Luna scored 50.3%. For comparison, OpenAI reports GPT-5.5 at 46.9% and Claude Fable 5 at 40.5%.


**Coding (Artificial Analysis Coding Agent Index v1.1):** Sol scored 80. Terra scored 77.4. Luna scored 74.6. OpenAI reports GPT-5.5 at 76.4, Fable 5 at 77.2, and Opus 4.8 at 72.5.


**Computer use (BrowseComp):** Sol scored 90.4% (Sol Ultra at 92.2%). Terra scored 87.5%. Luna scored 83.3%. OpenAI reports GPT-5.5 at 84.4% and Opus 4.8 at 84.3%.


**Knowledge work (OSWorld 2.0):** Sol scored 62.6%, which OpenAI says surpasses Opus 4.8 (54.8%) while using 85% fewer output tokens.


All of these numbers come from OpenAI's own evaluation setup. As with any model launch, treat them as a useful starting point rather than the final word.


## Where to Access GPT-5.6


According to[OpenAI's announcement](https://openai.com/index/gpt-5-6/) , GPT-5.6 is available across:


**ChatGPT:** Plus, Pro, Business, and Enterprise users access Sol through medium and higher effort settings. Pro and Enterprise users can also select Sol Pro for the highest-quality results.


**ChatGPT Work and Codex:** Free and Go users get Terra. Plus, Pro, Business, and Enterprise users can choose among Sol, Terra, and Luna and set an effort level. The ultra setting is available to Pro and Enterprise users in ChatGPT Work, and to Plus and higher plans in Codex.


**API:** Developers can access all three models. Programmatic Tool Calling is available in the Responses API. Multi-agent support is in beta.


## What This Means for Builders


The practical takeaway from GPT-5.6 is not that AI got smarter. It is that capable AI got cheaper and more efficient. Luna and Terra make it realistic to add AI-powered features to products without burning through your budget. Sol's design judgment improvements mean less back-and-forth when building interfaces from descriptions. And the three-tier structure means you can match the model to the task instead of paying flagship prices for everything.


### Start Building with ChatGPT 5.6


GPT-5.6 Sol and Terra are[already live on Emergent](https://emergent.sh/news/gpt-5-6-live-on-emergent) for all users. No separate OpenAI account, no API key setup, no code. Just describe what you want to build and pick your model. Sol for complex reasoning and polished front-end output. Terra for everyday production workloads at half the cost.


[Start building on Emergent](https://emergent.sh/) and put GPT-5.6 to work.


Stay tuned to[Emergent News](https://emergent.sh/news) for more on AI tools, launches, and what they mean for builders.
