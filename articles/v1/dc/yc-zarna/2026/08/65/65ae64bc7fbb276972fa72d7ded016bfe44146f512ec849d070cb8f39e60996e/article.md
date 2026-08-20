---
schema_version: "1.0.0"
document_id: "65ae64bc7fbb276972fa72d7ded016bfe44146f512ec849d070cb8f39e60996e"
company_key: "yc-zarna"
company: "Zarna"
source_id: "yc-zarna-news-import-0c4a0266984e"
canonical_url: "https://www.zarnaai.com/blog/how-to-build-agents-for-private-equity"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-07T18:16:08.302681+00:00"
fetched_at: "2026-08-07T18:16:10.407834+00:00"
content_hash: "sha256:afb1ecb76510da8018bdda5b04ca59b491fd9fc8c80124736304882416432184"
---

# How to Build Agents That Work in Private Equity Environments

Most of what gets written about building agents is about the technology. This piece is about everything else: the non-coding part of building enterprise-ready agents that need to be deployed across private equity, and what the partnership between firms and agent vendors has to look like for any of it to work.


## Start with workflow mapping


The crucial first step, one that I harp about a lot, is workflow mapping. Map out all workflows in the organization, separated by roles and groups. The way to do this is simply to look through the chronological steps of each job to be done for each function in the company.


For example, a VP of business development at a private equity firm will look at how he takes a deal from the first time he hears about it to handing it off to the M&A team for further diligence. That usually requires multiple steps: analyzing the teaser, processing the NDA, requesting supplementary information, creating a one-pager, meeting and prepping with the banker for an intro call on the deal, logging all of it into the CRM, and then packaging everything up for the Monday morning new deals review meeting. This is by no means a completely linear process.


The deal could be originated from a banker in our network on a quarterly catch-up call, an outbound email in a niche sub-sector we are looking to develop a thesis in, or a net-new broker we have never spoken with. Each makes the process slightly different and creates hundreds of nuances along the way. Agent vendors have to pick a niche and go deep enough in it that these edge cases are covered a few months into implementation for the first few clients, and in a few weeks for most future clients. The devil truly does lie in the details.


## A live example


I understand this can be quite abstract to imagine, so let's take a live mock example from what we see on the daily.


We are on a quarterly catch-up call with our guy at Harris Williams. He mentions a DSO based in Arizona coming to market in five months, not giving us much more than a revenue and EBITDA range and a couple of points on the ownership. Currently, this is one of the 30 things he mentions on the call. A Head of BD somewhere in New York or London is probably jotting it down in OneNote and creating a reminder to reach back out about the opportunity in a few months. That is also the optimal case. Investors don't do all of this for every new deal opportunity, because it simply takes too much manual work today.


## The same call, redesigned around agents


In a collaborative workspace, redesigned for human PE investors to co-work with agents, here is how that same situation plays out.


Prior to the quarterly catch-up, Zarna's agent gathers context from its memory on the relationship, which is a synthesis of data that lives in systems like DealCloud, SharePoint, Box, the Outlook inbox, PitchBook, and web data. A brief can contain a variety of details that would previously have slipped through the cracks. An example: "these are the three deals Matt from Harris Williams said would be coming live around Labor Day across our last four conversations with him, here is the background on each, along with a one-liner on potential fit at our firm." The agent will also pull up a transaction announcement from PitchBook that Matt was on last week that would have fit our criteria but we never got a look at. The list goes on. So far, pretty agent-led.


The call is yours to dictate and own. You control the flow. This part is human-led. Our agents connect to your note-taker, whether that is Granola, the Zoom notetaker, or manual notes stored in a folder, and post-meeting they port all of it into your CRM and our reminder system with action items from the conversation.


One action item might be connecting Matt at HW with one of our deal partners to discuss an interesting portco add-on opportunity. Another might be checking on a specialty chemicals teaser that should come out in two weeks. Zarna's agent drafts that email and creates that reminder, with the draft ready to go. No human input required; you press send. Agent-led, human behind the wheel for the final decision.


## Bridge the gap before you deploy


Figuring out how to align your business processes for cohesion between agents and humans is what you need to crack as a firm. This is a joint effort between the organization itself and the agent vendor it decides to go with. Usually, organizations will have done some work beforehand and have some ideas. But agent vendors have seen what has worked and what hasn't across the industry, and have a much better understanding of what an agent is going to be able to do and not, at least off the bat. The two teams have to work together to bridge that gap before deploying anything.


## The playbook


1. Map out all workflows in the organization, separated by roles and groups, by walking the chronological steps of each job to be done.


2. Map out who leads and decides what at each step: the owner. Do not overlook this step, and make sure there is internal agreement between key stakeholders. Ownership is critical to understand. Who the agent is reporting to matters more than you think.


3. Make a decisive call on whether each step should be human-led (deciding whether to reach out to an expert the agent suggested), agent-led (one-pager creation), completely human (speaking at IC), or completely agentic (CRM and file storage entry).


4. Redesign your firm workflows and redistribute the work.


5. Design and launch agents.


6. Run agent and human evals. Is our thesis shaping up correctly? Is the agent outperforming the human where it should be? Is the human outperforming the agent where he should be? If not, rethink the division of labor.


7. Track KPIs: number of new deals sourced, number of bids placed.


These are all learnings from several years of building agents for complex enterprise environments, for clients with no appetite for risk in output or performance. Thanks for reading.


[All articles](https://www.zarnaai.com/blog) Share
