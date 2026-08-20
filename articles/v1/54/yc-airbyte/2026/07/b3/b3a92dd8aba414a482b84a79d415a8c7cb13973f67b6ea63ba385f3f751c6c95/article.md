---
schema_version: "1.0.0"
document_id: "b3a92dd8aba414a482b84a79d415a8c7cb13973f67b6ea63ba385f3f751c6c95"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/ai-sovereignty"
published_at: "2026-07-17T18:17:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:2b178d2dd2eae28009d22bdb9cae529379995064198751866046cfbad00e3d9d"
---

# Sovereignty Starts Below the Model

AI sovereignty is now mainstream. Palantir and Nvidia recently announced a Sovereign AI Operating System built so government agencies own the hardware, the data, and the model weights outright. The All-In podcast hosts gave this topic a full thirty minutes. If you want the entire debate in one sentence, Chamath Palihapitiya supplied it.


"You can't rent intelligence from the same place that rents it to your competitor."


He's right, and the consensus forming around that idea is mostly right too. But I've spent six years building[the infrastructure that moves companies' most sensitive data](https://airbyte.com/data-replication) , and from that seat the debate looks like it's missing its bottom layer. Everyone is arguing about who owns the model. Almost nobody is asking who owns the context the model reasons over.


That's the layer where sovereignty is actually won or lost.


## **The trade is asymmetric by design**


Why are enterprises uneasy about frontier models in the first place? Because when you run your business through a rented model, you're not just buying tokens. You're sending a continuous, detailed signal about where value concentrates in your company: which workflows you automate first, which data you reach for, which prompts you run ten thousand times a day.


You get intelligence your competitor can buy tomorrow at the same price. Your provider gets the map.


Platforms have followed this map before. Microsoft spent the nineties watching which categories thrived on top of Windows. Then it took over spreadsheets and word processing. Google ran a subtler version of the same play, keeping a little more search traffic on its own properties each year once it understood where that traffic wanted to go. Neither of them had anything close to the visibility a model provider gets today, when a customer's actual working process arrives as prompts, all day, labeled by team.


To be clear, I don't think this is villainy. It's gravity. A platform with a large enough mandate eventually has to win the application layer, and its customers show it where to go.


So the discomfort is rational. The relationship helps them more than it helps you, and the gap widens the longer you feed it.


## **Retention is survival**


Palantir's launch manifesto had a line worth keeping: "Data retention is your treasure. Transfer it at your own peril."


I'd go further, because almost every other input to AI is for sale. The models are converging on each other. Compute is a line item in any cloud provider's quote. Even the scarce people in the room change jobs. The exception is what your business knows. Nobody sells that. Your customer history, the way you operate, the challenges you've endured over time, the edge you built after a decade of effort. All of this is proprietary information.


This knowledge is retained and governed by your company, and it creates an advantage that no one else can replicate. But if and when it starts informing someone else's platform, it becomes the industry baseline for everyone else.


There are also many problems with renting these AI models that have nothing to do with leaking information, but result from the terms being set by someone else. The consequences?


Many things, none of which are good. There are retention policies you did not create. The AI models might be deprecated on someone else's schedule. It’s also likely that the pricing structure will reflect someone else's financial needs. Worst of all, access can be restricted by a government decision overnight.


None of this requires bad faith from the AI companies. This is simply how the rented models work. It's strange to place a company's memory somewhere that arbitrary.


So data retention isn't just your treasure. For most companies, it's survival.


## **The layer the prescriptions skip**


Here's where I part ways with the standard advice. The prescriptions coming out of this moment are all model and compute: buy GPUs, run open weights, fine-tune your own model. Good steps. But walk through what happens next, because this is where I see companies stall in practice.


Your sovereign model still has to answer questions about your business. That means it needs context: your CRM, your tickets, your codebase, your contracts. And two problems show up almost immediately.


The first is that you can't verify your boundary. When context is assembled ad hoc, there's no record of what the model accessed, on whose behalf, or why. And in practice these stacks rarely stay as closed as their architecture diagram claims. Somewhere in the pipeline an embedding call goes out to a hosted API, or a connector was easier to buy as SaaS than to run, and data leaves the building without anyone having decided it should. If you don't have[governance and lineage at the data layer](https://airbyte.com/mcp-gateway) , you can't even check. Sovereignty becomes something you assert in a slide rather than something you can audit.


The second is quality. If the context is stale, fragmented, or stripped of meaning, you now own the means of production for bad answers. Owning the weights doesn't help a model reason over data it can't trust.


This is why I think about sovereignty as a stack, built from the bottom up. Where your data lives. What it means. Who and what can touch it, with an audit trail. Then, and only then, whose weights reason over it. The model sits at the top, and it's the most replaceable layer in the whole stack. It also happens to be where the current debate begins and ends.


The regulated companies I talk to figured this out ahead of everyone, mostly because they had no choice. What they ask for is telling. The model is a decision they want to keep making and remaking forever, as prices and capabilities shift. The context layer is the decision they want to make once, on infrastructure no model provider owns. They aren't asking for a better model. They're asking for[a neutral layer underneath all of them](https://airbyte.com/blog/beyond-etl-the-case-for-context) .


## **What this means in practice**


This is the thesis Airbyte is built on. We don't make models and we don't build your agents. We do one thing underneath both: move your business data, structured and unstructured, into a[governed context layer inside your own boundary](https://airbyte.com/context-store) , and let any model come to it. A frontier model today, an open-weight model next quarter, your own fine-tuned model next year. The context, the meaning, and the audit trail stay yours the whole time.


To be clear, the goal isn't abstinence from frontier models. They're extraordinary, and for plenty of workloads they're the right call. The goal is an architecture where you can use rented intelligence without the rented intelligence using you.


## **Where this lands**


Most companies are going to land on a mix. Rented frontier capability where it's clearly the best tool for the job, open weights on their own hardware where the economics or the data demand it, and a rotating set of experiments at the edges. That ratio will keep moving for years as prices and capabilities shift, and it should.


The context layer is the part that shouldn't move. Whatever blend of models a company ends up with, the ones that exit this era stronger will be the ones that kept their own context underneath it all. Owning your models while someone else effectively holds your context is renting with extra steps.


Sort out the bottom layer, and everything above it stays a choice you get to keep remaking.


If you're working through what this looks like for your own stack, the[Airbyte Agents launch post](https://airbyte.com/blog/airbyte-agents) covers how the Context Store works. Start small:[connect a source](https://app.airbyte.ai/sign-in) , point an agent at it, and see what your context is worth when you own it.
