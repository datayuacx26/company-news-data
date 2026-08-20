---
schema_version: "1.0.0"
document_id: "4b0b474a991375e53c4f32ad940f54d6ae1f7c174ea5900ec28410aeabbaa02e"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/ai-disruptors"
published_at: "2026-05-29T21:30:04.614+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:a18f5094d56fc5866a8c31f632f83b2b8c4b432f712a498ac07998c357359788"
---

# AI Disruptors: How the Next Generation of Business is Being Built

Getting your hands on a capable AI model is the easy part now. Every team can reach the same frontier models through an API, so a strong model is not what sets a product apart. What separates a working product from a demo is everything around the model. You have to measure whether the agent is actually doing its job, then keep grinding on reliability until it stops making expensive mistakes in front of real users.


I moderated a panel on exactly that at[DigitalOcean’s Deploy 2026](https://www.digitalocean.com/deploy) conference in San Francisco, a forty-minute conversation with four founders on what they’ve learned shipping AI products that people depend on:


-


**Angela Hoover** , co-founder and CEO of[Andi AI](https://andisearch.com/) , an ad-free consumer search engine that pairs generative AI with live web data to give people direct answers instead of a page of ad-heavy links.


-


**Alex Mashrabov** , co-founder and CEO of[Higgsfield AI](https://higgsfield.ai/) , a platform that lets creators and agencies produce cinematic video without any physical production.


-


**Hovsep Seraydarian** , co-founder and CTO of[LawVo](https://lawvo.com/) , a Canadian legal platform that pairs hundreds of AI agents trained in specific legal areas with human lawyers who verify their accuracy.


-


**Peter Elias** , founder of[Probably](https://www.probably.dev/) , a data analysis agent that lets non-technical people query their data in plain English and runs calculations on a local engine instead of an LLM so it can decline to answer when the data does not support a clear result.


The discussion got into what each founder underestimated once their agents had to run at scale, how they choose models from a field that keeps growing, what “agentic” actually means in production, and where a real moat comes from when everyone builds on the same foundation.


**Watch the full session from Deploy 2026** :


## Making agents work in production


When the founders were asked what they underestimated once their agents had to run in production, none of them pointed to the model.


### You need creative DNA


Higgsfield spent a year on R&D without traction. What finally moved the product was bringing on people who understood how creative work actually happens, then putting them next to the engineers every day.


> “We started to see success when we got non-technical people on the team, like ex-creative directors, who now work daily with engineers to wrap this powerful technology and make it accessible for creatives.”
>
>
> Alex Mashrabov, Higgsfield AI


### In high-stakes domains, AI plus humans beats AI alone


LawVo assumed its agents could handle legal guidance with little human involvement. That did not survive contact with real users.


> “We need human lawyers to verify the data and test these agents every day.”
>
>
> Hovsep Seraydarian, LawVo


When asked whether that human role shrinks as the agents get smarter, Hovsep said the opposite is happening. The team watches what its lawyers do and folds that judgment back into the agents one step at a time, which means leaning on people more, not less.


### Measurement infrastructure is unavoidable


Peter’s point was that one of the first problems you have to solve when you build an agent is the infrastructure that tells you whether the thing works at all. Probably went through several rounds of this until it built an analytics system that watches everything the agent does, and the product now evaluates its own behavior.


> “As long as we record everything the AI is doing, this particular AI can now actually aid us in improving its own performance.”
>
>
> Peter Elias, Probably


Angela made the same point from the oversight side.


> “We’re still early on in implementing agents at Andi. We’ve noticed that when you let them run wild, they’ll do anything. You really do have to make sure that they get high quality, accurate, grounded data. We keep an eye on the agents that we have running; we haven’t let them be fully autonomous.”
>
>
> Angela Hoover, Andi AI


## Model selection is a four-variable problem


The number of available models has exploded over the past year, with frontier releases from Anthropic and OpenAI followed within weeks by open-source alternatives. Capable models are everywhere now. It’s challenging to choose among dozens of them when each carries a different mix of cost and capability.


Peter broke the decision into four variables he is always trading against each other: cost, latency, intelligence, and capacity. Smaller models tend to be faster and cheaper but give up intelligence, and an agent that fires many parallel calls runs into capacity limits fast.


> “You want to get to the dumbest model you can get to before you actually go below the product performance that you need.”
>
>
> Peter Elias, Probably


He also warned that users have less patience than founders expect.


> “People will start to get impatient. We found that users were more latency-sensitive than we wanted them to be.”
>
>
> Peter Elias, Probably


Alex runs evaluations every week at Higgsfield because the proprietary data about how users take action has to stay current as models change. He has also moved away from fine-tuning small models toward prompting larger ones, which he finds faster with fewer hallucinations.


> “The rules of machine learning do not change. Understand the customer, understand the business goal.”
>
>
> Alex Mashrabov, Higgsfield AI


Hovsep’s rule for any new startup is to start on a frontier model but architect for independence, so only a small slice of the system depends on the LLM and the rest lives in your own application and orchestration. Angela took the lean path from day one, relying on open-source models wherever they were good enough at a lower cost.


### ‘Agentic’ doesn’t mean autonomous


None of these founders treat “agentic” as a synonym for autonomous. I asked what happens as agents move from co-pilots toward systems that act on their own, and what guardrails that calls for.


Hovsep described a legal field run by dinosaurs, where regulation moves slowly and full autonomy is simply not on the table.


> “Regulations won’t allow you to go fully autonomous. You literally get shut down if you do that in this space.”
>
>
> Hovsep Seraydarian, LawVo


What makes that constraint interesting is that LawVo’s agents already beat human lawyers on accuracy.


> “We have 92% accuracy on our average agent performance. Your average lawyer has 87%. If you go to a lawyer 100 times, 13 times they’re going to make a mistake. We’re paying for that.”
>
>
> Hovsep Seraydarian, LawVo


Peter pushed on the word “agent” itself.


> “Agency is the ability to spontaneously take action with no external input. LLMs are not agents. They don’t have agency. That’s why we have to prompt them.”
>
>
> Peter Elias, Probably


His view is that an LLM never acts on its own. You point it in a direction and keep pushing until it produces what you want.


> “It’s being poked with a stick in whatever direction you’re trying to get it to do something.”
>
>
> Peter Elias, Probably


That has a practical consequence for anyone building. A model cannot reliably check its own work, and stacking one model to verify another tends to break down, so a human stays in the loop to verify. Peter pointed to the experiment where Claude was put in charge of running a store and lost a remarkable amount of money, the kind of failure that shows up the moment you take human judgment out. His read on the fear that AI will replace everyone is that it is overblown, because these systems are not agents in any real sense. We’re just calling them that.


Angela put the actual job of an agent in plain terms. Building one means doing the prompting on the customer’s behalf. A task that might take fifty prompts by hand gets compressed into a single step, so the person states the outcome they want and the product runs the prompts behind the scenes and hands back the finished result.


## The moat is in the execution


Access to foundational models is getting commoditized. Open-source alternatives trail frontier releases by weeks, and any team can build on the same intelligence. When everyone can reach the same models, what actually sets a company apart?


Hovsep had a simple test.


> “There are startups that are science projects, and there are startups solving real-world problems. Are you solving a real-world problem? That’s it. It ends there for me.”
>
>
> Hovsep Seraydarian, LawVo


Peter described where the value is going right now. Some of it flows to the labs training the models. A lot of it flows to the inference platforms in the middle, which are making enormous money simply by running GPUs. The application layer holds value because getting these products to behave reliably is genuinely hard, and that reliability is the moat.


> “I could race anybody in building an agent, and it would be: how fast until your agent is as reliable as mine? I will probably win that race because I spent two years getting it to not screw up. That is the moat.”
>
>
> Peter Elias, Probably


Reliability also compounds. A product that works attracts users, the users put their data into it, and that data makes the next version better in a way competitors cannot copy. Peter also pointed to where he thinks the biggest opening is. Software can finally speak plain English, which means whole categories of tools that were stuck with tiny markets can suddenly reach far more people, because the only thing holding them back was an interface too complicated for a normal person to use.


Angela’s own moat is the data underneath Andi. It started as a consumer search engine, and building it surfaced something more valuable, which is data accurate enough for other systems to depend on. That data has turned into a business of its own as more AI agent companies look for a trustworthy source to ground their answers.


> “There’s a lot of AI agent companies now that need access to high quality, accurate, grounded data.”
>
>
> Angela Hoover, Andi AI


Both the product and that insight came from the same place: the work.


> “When you’re actually in the trenches building, you learn some insightful things, and then you can build out your moat.”
>
>
> Angela Hoover, Andi AI


---


I ended by asking each founder for one piece of parting advice, and the through-line was demand. Alex framed it as a warning, that too many AI companies build for other AI companies and never check whether real customers want what they are selling. Angela put it more directly, that you should talk to your users and test their willingness to pay as early as you can. The most capable agent in the world is still just a demo until a customer pays for it.


DigitalOcean’s AI-Native Cloud is built for teams at every stage, from testing early demand to scaling into the enterprise. It’s one integrated stack from silicon to agent runtime. You get more than 70 models on a single endpoint, with an Inference Router that handles model selection for you. One API and one bill, with economics that improve as you scale.


[→ Get started with DigitalOcean’s AI-Native Cloud](https://cloud.digitalocean.com/registrations/new)
