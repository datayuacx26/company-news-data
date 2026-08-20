---
schema_version: "1.0.0"
document_id: "35b2feeaf0ec0504785e8ee6ac3a6ad3f29c2cd8e1bbc664acb9e63b294c38a5"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/reducto-doe-ai-agent-customer-story"
published_at: "2026-07-21T12:00:00+00:00"
first_seen_at: "2026-07-23T23:11:50.013054+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:670c8574f24d3b22ab954404e0846d38c9b1023ca34664048ea47d9b2f1d880f"
---

# Built to Scale: How Doe Automated Over 8 Million Agent Tasks with Reducto

*“Our agents have completed more than 8 million pieces of work for customers, and Reducto handles the complex document work underneath a lot of it. For a three-person team, that's the difference between shipping and not.” — Richard Ou, Co-founder, Doe*


**Doe**


is an AI agent command center. The platform lets businesses deploy agents across thousands of tools to complete long-horizon tasks that can run for hours and touch multiple systems. In its first year, Doe’s agents completed more than 8 million tasks for startups and enterprises.


### The problem Doe solves


A lot of business work is repetitive and spread across documents, tools, inboxes, and internal systems, and it doesn't get automated because it doesn't fit neatly into a single piece of software. Doe automates that work so employees can spend their time on higher-value priorities and increase productivity. Rather than answering a single question, Doe's agents carry out a full task end to end, using whatever tools and data they need along the way.


The most popular thing customers do with Doe is what Richard Ou, Doe's co-founder, calls its Knowledge Engine: pointing the platform at a large collection of files and letting it work across all of them at once.


> *Our deep file intelligence capability is one of our most valuable features,” Richard says. “The Knowledge Engine goes through and parses all your files, leveraging proprietary embedding models to do a vector search across everything that's accessible, with all the database information the user has access to.*


The platform is also built to run agents at scale, with many working in parallel rather than a single agent working alone. A workflow can spin up a large number of agents to tackle the same problem at once. It can run those agents repeatedly on a schedule and aggregate their responses. Combining their outputs produces more consistent and reliable results than relying on any single run. That combination of breadth, long-horizon tasks, and many agents whose outputs are combined is what separates Doe from a single-purpose assistant.


> *I don't necessarily think there are direct competitors. I’d consider them peers,” Richard says. “Doe can work with other AI agents. We're a command center.*


### Doe's origin story: a dinner, a deadline, and a 58-second video


Richard met his co-founder Adrian through Z Fellows. Both had previously built startups, and after a dinner together, they decided to start a new company. With the YC deadline only days away, they wrote the application and recorded their 58-second submission video in one take.


Their first product was far narrower than the platform is today. Richard and Adrian built a spreadsheet of local businesses and visited law firms, construction companies, and dental offices, looking for repetitive work worth automating. Dental ordering stood out.


> *When a dentist wants to order more dental filling material, they'd write it on a sticky note. They'd pass the sticky note to the receptionist. Then the receptionist would go on the computer and open up 20 different windows, go to legacy websites built in the early 2000s, and maybe call up a sales agent to figure out pricing and volume, while dealing with customers at the front door*


So they built it: agents that ran the ordering workflow end to end, from the sticky note to the reorder, instead of a receptionist working through 20 windows by hand. It worked.


This realization led to their first large contract: Doe signed the second-largest dental service organization in the country, with more than 1,000 locations, but realized how few customers of that size existed in dental alone.


> *How many other dental groups are there of that scale? Like one other. And that was it. That was an entire market.*


So they set their sights even higher. The infrastructure they had built wasn't specific to dentists; it could automate this kind of work for almost any industry. So they went horizontal, opening up the same architecture to any business. Over a few months they rebuilt Doe into a self-serve platform, and customers turned up in places they never could've imagined: a law firm in rural Wisconsin, a parking company in Florida.


The same platform that started with a dentist's sticky note now runs millions of automated tasks for companies of every size, from small startups to large enterprises. The work looks nothing alike from one customer to the next; what stays constant is that it's repetitive, systems-heavy work people would rather not do by hand.


### When documents became the hard part


As Doe opened up to more industries, documents became a recurring problem. Customers uploaded PDFs and other files, and the agents had to parse them, understand the contents, extract the relevant fields, fill out forms, and pass the information into downstream workflows.


> *When users upload documents and we manipulate documents, like filling out forms, we hand it off to Reducto to handle a lot of the complex document work,” Richard says.*


Document processing was only one part of the platform, but it had to be reliable, because everything the agents did depended on reading the documents correctly. Doe first tried to handle parsing itself. That worked in early cases but struggled as customer workflows got heavier.


The clearest growing pain became obvious from volume: at one point the team was pushing a large amount of data from a single law firm through its own pipeline, and it became clear that reliable document processing was an infrastructure problem, not something to keep building in-house. Doe integrated Reducto to handle it, so a lean three-person team could stay focused on its agents rather than document edge cases.


### How Doe resolved a contract emergency in hours, not days


One story was still fresh for Richard when we spoke, because it had happened the day before. A customer, another AI startup, hit a contract problem that needed resolving immediately. It was the Fourth of July weekend, their lawyers weren't responding, and the issue couldn't wait until Monday, so they turned to Doe.


> *They were like, ‘we need to use Doe right now to figure this out.’ It went through all the contracts stored on our platform and gave answers they thought were more rigorous than the lawyers had given before. It even highlighted areas the lawyers had missed in the past.*


Working through every contract the company had on the platform, Doe surfaced problems in existing agreements that their own lawyers had overlooked, then drafted new language tailored to the situation. The customer came away half-joking that they might not need their outside counsel as much as they'd assumed, a notable result for a holiday weekend when the alternative was to wait.


### What's next


Doe is growing quickly. The number of tasks its agents complete for customers climbed into the millions during the company's first year and continues to rise. As those agents take on longer, more complex work and Doe runs more of them in parallel on each problem, the range of tasks it can automate keeps widening. Almost all of that work touches documents at some point, which makes reliable document processing more fundamental to the platform over time, not less.


Richard is candid about how early this all is, especially outside of San Francisco.


> *I think we live very much in a bubble in SF,” he says. “We sometimes have to step back and remind ourselves that the average person in New York, or really anywhere in the United States, doesn't know that much about AI and the value it can bring them.”*


Adoption, he has found, is slower than expected in many industries, and much of the work ahead is about education and building trust rather than raw capability. Most companies aren't interested in AI for its own sake; they want repetitive work off their plates, and a lot of that work still lives in documents.


As the layer that turns those documents into something Doe's agents can act on, Reducto is part of the foundation that lets everything above it run. We're excited to keep supporting Doe as the company grows.


---


Want to try Reducto on your own documents? Visit[Studio](https://studio.reducto.ai/) to sign up for free, or[request a demo](https://reducto.ai/contact?utm_source=website&utm_medium=blog) .
