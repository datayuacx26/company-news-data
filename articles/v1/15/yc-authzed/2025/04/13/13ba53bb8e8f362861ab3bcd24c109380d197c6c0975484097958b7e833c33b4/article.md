---
schema_version: "1.0.0"
document_id: "13ba53bb8e8f362861ab3bcd24c109380d197c6c0975484097958b7e833c33b4"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/kubecon-europe-2025-highlights-navigating-authorization-challenges-in-fintech-with-authzeds-jimmy-zelinskie-and-pierre-alexandre-lacerte-from-upgrade"
published_at: "2025-04-08T16:15:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:b9115ce7cf6cbbc08b36eb93875c5380195b293c6d6baf16cb341b1c9ff07be0"
---

# Techstrong.tv Interview with Jimmy Zelinskie and Pierre-Alexandre Lacerte from Upgrade

## Navigating Authorization Challenges in FinTech with Jimmy Zelinskie and Pierre-Alexandre Lacerte


At this year's KubeCon + CloudNativeCon Europe 2025 in London, AuthZed CPO Jimmy Zelinskie sat down with Pierre-Alexandre Lacerte, Director of Software Development at Upgrade, for an insightful discussion on modern authorization challenges and solutions. The interview, hosted by Michael Vizard of Techstrong TV, covers several key topics that should be on every developer's radar.


## Watch the Full Interview


Before diving into the highlights, you can[watch the complete interview on Techstrong TV here](https://techstrong.tv/videos/kubecon-cloudnativecon-europe-2025/navigating-authorization-challenges-in-fintech-with-jimmy-zelinskie-and-pierre-alexandre-lacerte-kubecon-europe-2025) . It's packed with valuable insights for anyone interested in authorization, security, and cloud-native architectures.


## Key Highlights from the Conversation


### Origins of SpiceDB and the Zanzibar Approach


Jimmy shares the origin story of AuthZed, explaining how his experience building Quay (one of the first private Docker registries) revealed fundamental challenges with authorization:


> "When you think about it, the only thing that makes a private Docker registry different from like a regular Docker registry where anyone can pull any container down is literally authorization... the core differentiator of that product was authorization."


The turning point came when Google published the Zanzibar paper in 2019:


> "We read this paper and said, this is actually how you're supposed to solve these problems. This would have solved all the problems we had building Quay."


### What is Relationship-based Access Control?


One of the most valuable segments of the interview explains the concept of relationship-based access control:


> "The approach in the Zanzibar paper is basically this idea of relationship-based access control, which is not how most people are doing things today. The idea is essentially that you can save sets of relationships inside of a database and then query that later to determine who has access."


Jimmy illustrates this with a simple example that makes the concept accessible:


> "Jimmy is a part of this team. This team has access to this resource. And then if I can find that chain from Jimmy through the team to that resource, that means Jimmy has access to that resource transitively through those relationships."


### Why Upgrade Chose Not to Build In-House


Pierre-Alexandre explains the decision-making process that led Upgrade to adopt SpiceDB rather than building an in-house solution:


> "We're a fintech, so we offer personal loans, checking accounts. But eventually we started developing more advanced products where we had to kind of change the foundation of our authorization model... we're kind of not that small, but at the same time we cannot allocate like 200 engineers on authorization."


Their evaluation involved looking at industry leaders:


> "We started looking at a few solutions actually, and then also the landscape, like what is GitHub doing? What is the Carta, Airbnb doing?... a lot of those solutions were kind of hedging into the direction of Zanzibar or Zanzibar-ish approach."


### The Power of Centralization


The interview highlights a critical advantage of centralized authorization systems:


> "The real end solution to all that is centralization. If there's only one system of record, it's really easy to make sure you've just removed that person from the one single system of record."


Pierre-Alexandre describes how Upgrade implemented this approach:


> "When someone leaves the company or when someone changes teams, we do have automation that would propagate the changes across the applications you have access to down to the SpiceDB instance. So we have this kind of sync infrastructure that makes sure that this is replicated within a few seconds."


### Cloud-Native Requirements


For companies operating in regulated industries like fintech, having a cloud-native solution is essential. Pierre-Alexandre emphasizes:


> "We're on Amazon EKS, so Kubernetes Foundation... For us, finding something that was cloud native, Kubernetes native was very important."


### Authorization for AI: The Next Frontier


One of the most forward-looking parts of the discussion addresses the intersection of authorization and AI:


> "The real kind of question is actually applying authorization to AI and not vice versa... now with AI, we don't have that same advantage of it just being like a couple folks. If you train a model or have tons of embeddings around your personal private data, now anyone querying that LLM has access to all that data at your business."


Upgrade is already exploring solutions:


> "In our lab, we're exploring different patterns, leveraging SpiceDB where we have a lot of internal documentation and the idea is to ingest those documents and tag them on SpiceDB and then leveraging some tools in the GenAI space to query some of this data."


### The Bottom Line: Don't Build Your Own Authorization


Perhaps the most quotable moment from the interview is Jimmy's passionate plea to developers:


> "If there's like one takeaway from kind of us building this business, it's that folks shouldn't be building their own authorization. Whether the tool is SpiceDB that they end up choosing or another one, like developers, they wouldn't dream of building their own database when they're building their applications. But authorization systems, they've been studied and researched and written about in computer science since the exact same time. Yet every developer thinks they can write custom code for each app implementing custom logic for a thing they don't have no background in, right? And I think this is kind of just like preposterous."


Pierre-Alexandre adds a pragmatic perspective from the customer side:


> "Obviously, I probably have decided to go with SpiceDB sooner. But yeah, I mean, we had to do our homework and learn."


## Beyond the Highlights


The full interview covers additional topics not summarized here, including:


- The distinction between authentication and authorization (and why the terms are confusing)
- Security implications of centralized authorization
- Enterprise features for enhanced control and monitoring
- How SpiceDB handles audit logging and security events


## Join the Conversation


Interested in learning more about modern authorization approaches after watching the interview?


- [Star our GitHub repository](https://github.com/authzed/spicedb)
- [Join our Discord community](https://authzed.com/discord)
- [Read the Zanzibar paper](https://authzed.com/zanzibar)
- [Try our interactive SpiceDB playground](https://play.authzed.com/)


Don't miss this insightful conversation that challenges conventional wisdom about authorization and provides a glimpse into how forward-thinking companies are approaching these challenges.[Watch the full interview now →](https://techstrong.tv/videos/kubecon-cloudnativecon-europe-2025/navigating-authorization-challenges-in-fintech-with-jimmy-zelinskie-and-pierre-alexandre-lacerte-kubecon-europe-2025)


On this page


- Navigating Authorization Challenges in FinTech with Jimmy Zelinskie and Pierre-Alexandre Lacerte
- Watch the Full Interview
- Key Highlights from the Conversation
- Origins of SpiceDB and the Zanzibar Approach
- What is Relationship-based Access Control?
- Why Upgrade Chose Not to Build In-House
- The Power of Centralization
- Cloud-Native Requirements
- Authorization for AI: The Next Frontier
- The Bottom Line: Don't Build Your Own Authorization
- Beyond the Highlights
- Join the Conversation


## Related


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)


[Community The Importance of Off-Sites for a Remote Company While many companies push return-to-office, AuthZed stays remote-first. Our secret is regular off-sites where bonding and business coincide. When we prioritize being human together, we return with more empathy, better communication, and renewed drive to solve hard problems. Dec 30, 2025 · 3 min](https://authzed.com/blog/the-importance-of-off-sites-for-a-remote-company)[Community The Importance of Off-Sites for a Remote Company While many companies push return-to-office, AuthZed stays remote-first. Our secret is regular off-sites where bonding and business coincide. When we prioritize being human together, we return with more empathy, better communication, and renewed drive to solve hard problems. Jenessa Petersen · Dec 30, 2025 · 3 min](https://authzed.com/blog/the-importance-of-off-sites-for-a-remote-company)


[Community AuthZed 2025 Year in Review Five years in, our mission remains the same, fixing access control. 2025 was about making our authorization infrastructure available to more teams in more ways. Dec 19, 2025 · 5 min](https://authzed.com/blog/authzed-2025-year-in-review)[Community AuthZed 2025 Year in Review Five years in, our mission remains the same, fixing access control. 2025 was about making our authorization infrastructure available to more teams in more ways. Sam Kim · Dec 19, 2025 · 5 min](https://authzed.com/blog/authzed-2025-year-in-review)
