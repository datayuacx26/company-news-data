---
schema_version: "1.0.0"
document_id: "2895f1d1f38952d1fbb062139d907255bbfc07e0045db2fe3f281690b8316d79"
company_key: "yc-hightouch"
company: "Hightouch"
source_id: "yc-hightouch-news-import-8089bfc075a8"
canonical_url: "https://hightouch.com/blog/the-agentic-cdp"
published_at: null
first_seen_at: "2026-07-25T08:06:20.021758+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:d965fe3d169a0bf19d9fcc605b738ed6949d61ab3ff93e2319687c0dbf1719d3"
---

# The next chapter: Agentic CDP

Five years ago, we made a bold prediction: customer data was moving to the warehouse, and[the traditional CDP was dead](https://hightouch.com/blog/friends-dont-let-friends-buy-a-cdp) . *Why buy another database when your data already lives in the warehouse?*


That idea became the Composable CDP. Today, thousands of companies run on the architecture, and nearly every vendor has either adapted to it or been left behind.


Yet no CDP, Composable or not, has delivered on its biggest promise of proactive intelligence. For the last 10 years, CDPs have added intelligence features and aspired to become the brain behind your marketing strategy. But let’s be real. It hasn’t happened.


Marketers still log in, build the audience they already had in mind, launch the campaign they already planned, and log out. **The CDP executes ideas, but it has never had one of its own.**


Real intelligence requires models that can reason, plan, investigate, and work through open-ended problems. Models capable of doing that reliably only recently became available, and our research team has spent the time since learning how to build reliable products around them.


So now, we're building what every CDP always wanted to be.


## An Agentic CDP works while you sleep


Five years ago, we thought we were building a better CDP architecture. In reality, we were building the foundation for intelligent agents. By moving audiences, journeys, and activation onto the warehouse, the Composable CDP connected marketing directly to a company’s richest customer and business context.


The Agentic CDP now builds on that foundation with agents that understand your data, campaigns, goals, creative, and brand rules. The agents know what matters, what campaigns are already in flight, and where new campaigns or improvements could have the greatest impact.


We’ll come back to how it works. First, here’s what it changes for marketers.


Imagine you run marketing at a national fashion retailer and want to apply AI to your top initiatives. You connect your Agentic CDP to your data warehouse and the systems that hold your marketing, creative, and brand context. Then, you tell it what you want help driving:


- Make this fall’s flagship product launch a breakout success.
- Increase purchase frequency, especially among workwear buyers.
- Drive more second purchases from customers who buy once and never return.


The Agentic CDP then runs agents continuously to uncover insights and identify new ways to improve your marketing.


When marketers log in, they no longer start from a blank canvas. Instead, they see a ranked list of opportunities tied to their business goals, sized by potential impact, and accompanied by a recommended strategy and draft work their team can review. Those opportunities will look different for every business, but here are a few examples of what the system might surface for this retailer.


**New product launch opportunity**
The agent finds that past product-drop creative consistently outperforms evergreen ads with similar audiences. Knowing two new collections launch next week, it recommends shifting spend toward launch-specific campaigns and drafts the audiences, messaging, and creative briefs.


**Churn prevention behavior**
The agent discovers that customers who purchase across multiple categories are far less likely to churn. It finds that targeted text offers are the most effective way to encourage that behavior, then recommends and drafts a two-message cross-sell SMS journey for at-risk first-time buyers.


**Underserved audience opportunity** The agent reviews next month’s marketing calendar and finds that “new workwear buyers” are barely being targeted. It sizes the opportunity, recommends campaigns for the audience, and drafts the audiences, messaging, and creative briefs needed to launch.


A year ago, this would have sounded like science fiction. It isn't anymore, and later in this post, we'll show you what we built to make it real.


## Isn't every CDP already "Agentic"?


Nearly every CDP and martech platform has bolted an AI chat interface onto its data and invited you to ask it anything. That interface is useful, and we offer it too, but it quickly reaches its limits. If you ask a tactical question like, “What were my top-selling products last month?” it works, assuming it has access to the right data. But when you ask a broader strategic question like, “How can I boost my subscription sales?” it falls short.


The reality is that today’s agents have three fundamental limitations:


- **They only see the data inside the platform they live in,** giving them an incomplete view of customers, campaigns, and business priorities.
- **They are designed to answer quickly** , so they surface the first plausible ideas rather than deeply exploring alternatives to identify the best one.
- **They struggle with long, open-ended investigations** , losing context and becoming less reliable as they process more information.


To really answer broader questions like ‘How can I boost my subscription sales?’ you need agents that continuously explore in the background.


## What makes this all possible


We’ve been experimenting with running marketing agents continuously in the background for a while now. Most of what we tried didn’t work. Our initial implementations failed in ways any marketer would recognize:


- They surfaced 30 “opportunities” a day, with no sense of which mattered.
- They confused normal seasonal movement with a meaningful change in customer behavior.
- They proposed discounts on premium products that never go on sale because nobody had told them not to.


What finally worked was combining three things:


1. **A long-running agent harness** built specifically for marketing, so agents can continuously explore, evaluate, and prioritize ideas.
2. **A composable context layer** that understands the entire marketing operation and builds memory with every campaign.
3. **Forward-deployed teams** that help adapt the technology to how each business actually works.


### A long-running agent harness that understands marketing


Last summer, we wrote about our[agent harness](https://www.amplifypartners.com/blog-posts/how-hightouch-built-their-long-running-agent-harness) . It now lets our agents investigate for hours without context rot, repeated work, or drifting off the original goal.


The system works as a chain. Exploration agents continuously generate hypotheses about what a marketing team should try, change, or stop doing. The strongest ideas are passed to specialized agents that investigate further and test the evidence. A prioritization loop then ranks each opportunity the way a marketing leader would, based on how many customers it could reach, what the outcome is worth, and how likely the idea is to work. Finally, a verifier challenges every finding and rejects anything the data cannot support.


These agents deeply understand marketing. They evaluate awareness and conversion campaigns differently, understand the difference between attribution and incrementality, and check historical baselines before flagging a trend. The result is a ranked list of credible opportunities a team can actually act on.


### A composable context layer that sees all your marketing


Agents are only as smart as the context they can reach, and most of our early failures were context failures. The industry's reflex is to copy everything into a central context store. We made the opposite bet and leaned into composability (again): bring the AI to where the context lives.


- Our agents connect directly to your data warehouse without copying data.
- They tap into and can be powered by tools and technologies you’ve already invested in, from Snowflake Cortex, Databricks Genie, Looker, and Omni.
- Brand and creative context stay in the systems that already own it, from your DAM to unstructured data lakes to Figma.
- You control and input the strategy by sharing documents with us or via MCPs: your goals, trade-offs, and specific business rules.


By embracing the idea of “composable context”, our agents can see the entire operation without forcing you to create and maintain another copy.


#### Customer and marketing memory that compounds over time


That context is constantly evolving. Every customer interaction, campaign, experiment, and business outcome creates new evidence about what works. Our agents mine all of it, continuously, in the background, and turn what they learn into reusable memory.


They investigate your active initiatives, audiences, journeys, campaigns, and customer behavior, then catalog what they learn into reusable memory that every agent can access. Over time, the system learns things like:


- Which audiences won't convert, no matter the offer
- Which ones respond best to SMS
- Where in a journey an offer meaningfully changes customer behavior
- Which subject-line traits drive opens among your highest-value customers


That memory builds at every level. Every initiative you define stores context from its previous runs, so each recommendation builds on what came before rather than repeating it. We also maintain memory of individual audiences, journeys, campaigns, and messages, so an agent evaluating a campaign already understands its history before the investigation begins.


A recommendation is only as good as what the agent remembers, and ours remembers more every day.


### Forward Deployed Marketers who turn intelligence into outcomes


Technology alone won't carry this transition. We’re embedding Forward Deployed Engineers and Forward Deployed Marketers with leading brands to make the technology work inside each business. They help teams engineer the context agents need, build and evaluate new skills, establish guardrails, and redesign the workflows around them.


In some companies, a highly technical marketing team may do much of this itself. In others, our forward-deployed teams will help build the capability.


## Completing the agentic marketing flywheel


We’re nowhere near done. Agents sometimes pursue patterns a marketer would quickly dismiss, and deciding which ideas deserve attention remains one of the hardest problems we are working on. But finding a great idea is only the beginning. The next challenge is turning it into a live campaign.


Today, even a simple campaign can require a marketer to write a brief, a designer to create assets, a developer to build the experience, and an operations team to assemble the audience and journey. That process can take weeks, whether the team is launching an email, an ad, a landing page, or a journey.


An Agentic CDP could surface the perfect opportunity, but it has limited value if the team still needs weeks to act on it. This is exactly why we are building the[Agentic Marketing Platform](https://hightouch.com/blog/agentic-marketing-platform) in tandem. The Agentic Marketing Platform (AMP) turns marketing ideas into complete campaigns by creating on-brand assets across paid and owned channels.


Together, the Agentic CDP and AMP create a continuous loop. The Agentic CDP identifies the opportunities worth pursuing. AMP turns those opportunities into campaigns. Marketers review, refine, and launch them. The results then become new evidence that helps the Agentic CDP make better recommendations next time.


## You should own your context. You should own your AI


As intelligence reaches every layer of the stack, every company will need to decide where its marketing intelligence should live.


We believe it must be composable.


Companies will build and run AI across platforms like Snowflake, Databricks, OpenAI, and Anthropic. Hightouch connects to and benefits from all of them. But marketing still needs an intelligence layer that can see across the entire operation, including customer data, campaigns, creative, strategy, and the relationships between them. The Composable CDP is uniquely positioned to become that layer.


And intelligence should never be trapped at any level. It should be owned by you as a customer. We're building Hightouch on open standards like MCP, so the context and intelligence you build are yours and usable in any tool that comes next.


## What happens next


The last CDP shift was architectural, and an entire generation of vendors failed to survive it. This shift changes what the CDP actually does.


Others will race to announce Agentic CDP capabilities, and soon the term will appear across martech websites. Much of what ships will be a chat interface stapled to a system of record. Data warehouses may even claim the title of customer data platform. Fair enough. By our own definition, that is where the customer data lives.


But Hightouch will remain the place where marketing gets done.


We are hiring AI engineers, Forward-Deployed Creatives and Marketers, and many other roles to help us build this future:[https://hightouch.com/careers](https://hightouch.com/careers)
