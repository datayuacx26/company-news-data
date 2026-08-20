---
schema_version: "1.0.0"
document_id: "67c2805304e7ad0c3c3568108ed3407bbbfb1258a271cf1f5997fd33506b1c3b"
company_key: "yc-weave-3"
company: "Weave"
source_id: "yc-weave-3-news-import-b8160165addf"
canonical_url: "https://weaveos.com/blog/weave-raises-13.5m-series-a-to-build-engineering-and-token-intelligence"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T03:27:14.435285+00:00"
fetched_at: "2026-07-29T03:27:15.559785+00:00"
content_hash: "sha256:6f1d17f3dc7ff01fbb86dea0b3361161b92a275b1137ef3d3e1135b4c917f923"
---

# Weave Raises $13.5M Series A to Build Engineering and Token Intelligence

We started Weave to answer a question every engineering leader is now asking: what is our AI spend actually returning? Since then, our platform has analyzed more than 2 million human and AI code contributions across 20,000+ engineers at 1,000+ companies, including Robinhood, Reducto, and PostHog.


Today, we're announcing our $13.5M Series A led by[Standard Capital](https://standardcap.com/) , with participation from Y Combinator, Moonfire, Burst Capital, IrregEx, and the Agent Fund. This funding accelerates our mission to become the operating system that measures and optimizes how engineers and AI impact business outcomes.


## The problem: tokenmaxxing is costing companies billions


Companies are pouring tens of billions of dollars into AI coding tools, but when an AI can generate thousands of lines of code in seconds, teams inadvertently optimize for volume over real engineering progress.


Legacy metrics like commit counts, lines of code, DORA, SPACE were built for human output, so they actively reward artificial bloat and create a false illusion of velocity. Everyone has always known the best engineers sometimes write negative lines of code. Now the problem is worse, because the code being counted isn't even written by people.


The result is that executives are flying blind. They're paying for generated tokens instead of business value, with no way to calculate the true ROI of their AI spend. Half the money is being wasted, and nobody knows which half.


Engineers are burning tokens by using the best model on every prompt. This is rarely necessary but it's the default state as it takes manual work to switch off and there's typically no incentive internally to optimize AI spend.


Weave is build to fix this.


## Where we are today


LLMs can look at code and understand what it's actually doing, so we trained our own model to do exactly that: read every pull request, review, and deployment, attribute it to human or AI contribution, and normalize it into a single, objective unit of output. Our model asks a simple question of every piece of work: how long would this have taken a human in the pre-AI era?


From there, everything else follows:


-


**Code Output** : a fair, normalized view of productivity by team or individual engineer, human and AI alike.


-


**AI ROI** : a real dollar figure. For every dollar spent on tokens, how many hours of human-era engineering work are you getting?


-


**AI Skills** : show engineers exactly where and how to get better at working with agents.


-


**Prompt Routing** : use your own production data to route every query to the cheapest model that won't sacrifice quality or speed, including open source models, directly inside Claude Code, Cursor, or Codex. It looks at every turn and reclassifies what the best model for that prompt will be.


The router is the beginning of the optimization layer. Because we see the entire path from prompt to production, our routing decisions get better over time: we know not just how a model answered, but how that answer performed once it shipped. Complex system design goes to a frontier model. Centering a button doesn't need one. Customers using the router have cut costs anywhere from 20 to 80 percent, and more importantly, teams adopting the full stack are seeing 20 to 25 percent increases in output for the same spend.


## What our customers (Robinhood, Telnyx and Posthog) have seen


Robinhood came to us because their internal pipelines and default tool analytics couldn't tell them the most basic thing: how much of our code is actually written by AI, and is it helping? As a financial institution with a self-hosted deployment and serious security requirements, they needed answers without compromise.


Telnyx runs about 150 engineers with a single VP of Engineering and no managers. Instead of relying on managers to coordinate and coach, every engineer sees their own data and uses it to level up. As David Casem, Co-Founder and CEO of Telnyx, put it: "Our goal is to ship the highest quality product as quickly and efficiently as possible for our customers. We use Weave to get objective measurement of our teams and agents as well as ways to optimize."


Posthog let's all 80+ software engineers on their team see their own data, optimize their workflows and have some friendly competition.


The headlines saying AI barely helps enterprises? We've found it's an implementation problem. Working with some of the fastest-growing engineering teams in the world, we don't see 10 percent lifts. We see 200 to 300 percent and our team of FDEs is built to unlock this for you.


## What comes next


This funding will be focused on three priorities:


1.


**Deeper models** : advancing our code output and routing models so every measurement and every routing decision gets more accurate as the data compounds.


2.


**The learning loop** : closing the loop from measurement to optimization, so Weave doesn't just tell you what happened, it makes every engineer and every agent better.


3.


**Customer success** : supporting our growing enterprise deployments, including self-hosted environments with enterprise-grade security.


The agent era will produce more software than any era before it. Engineers are becoming managers of agents, and a new role is emerging between engineering and finance: deciding where compute, tokens, and human attention should go. Those decisions need data. When product analytics arrived, not measuring your product became unthinkable, and entire new functions like growth were born. We believe the same is true now: not measuring your AI is like not measuring your product. Weave is the tool those teams of the future will be built on.


The era of tokenmaxxing is over. Lines of code are dead as a metric. What replaces them is one objective measure of real output, human and AI, so engineering can finally be managed like every other part of the business.


## Thank you to everyone on this journey


To our investors: thank you for believing in us. Dalton Caldwell, Paul Buchheit, and Bryan Berg at Standard Capital have been true partners from the first conversation, and we're grateful to Y Combinator, Moonfire, Burst Capital, IrregEx, and the Agent Fund for backing us early. As Dalton put it: "AI spend is the most powerful force in the world, and right now there is not an easy way to measure it. The opportunity for Weave is to enable every organization to effectively track and route their spend."


To our customers: thank you for betting on a young startup. From Robinhood to Telnyx to PostHog, you trusted us with a question nobody had answered before, and you're the reason our models keep getting better.


To the Weave team: you took on a problem that was previously thought impossible, actually measuring software engineering, and you solved it. Five days a week, in person, in San Francisco, pushing the cutting edge of what an engineering team can be. This is just the beginning.
