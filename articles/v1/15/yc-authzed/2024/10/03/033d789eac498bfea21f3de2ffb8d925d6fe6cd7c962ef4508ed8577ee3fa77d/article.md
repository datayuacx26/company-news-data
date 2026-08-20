---
schema_version: "1.0.0"
document_id: "033d789eac498bfea21f3de2ffb8d925d6fe6cd7c962ef4508ed8577ee3fa77d"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/how-im-learning-spicedb"
published_at: "2024-10-31T05:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:d1e1e934aba3937164097b1875dfb1d1b06c2e8870a074bcafec9ee9267d160c"
---

# How I'm Learning SpiceDB

I recently joined AuthZed as a Developer Advocate, and I want to document my learning journey for those going through a similar process. Here are the 4 steps that helped me ramp up my knowledge of SpiceDB. I hope you'll find these helpful on your own learning journey!


## 1. Start with the Basics


It's always beneficial to have strong foundational knowledge. In the past, my eagerness to code got the better of me, and I dove headfirst into building something only to backtrack to actually *understand* how it works. This time, I didn't want to repeat that mistake, so I started with a refresher on[Authorization](https://authzed.com/blog/authentication-vs-authorization) ,[ABAC, RBAC & ReBAC](https://authzed.com/blog/exploring-rebac) . If these acronyms are new to you, I'd suggest starting here.


I then read[the Google Zanzibar paper](https://research.google/pubs/zanzibar-googles-consistent-global-authorization-system/) that inspired SpiceDB, and re-read it - this time[with annotations](https://zanzibar.tech/) . I have to admit - I find it hard to parse academic papers (who doesn't wish for a TikTok-style summary sometimes?). That's where this presentation by Jake Moshenko came in really handy. His explanation brings to life all the concepts listed in the paper and reinforces understanding of how Zanzibar works.


Although SpiceDB is inspired by Zanzibar, there are some key differences.[Here are some differences](https://authzed.com/docs/spicedb/concepts/zanzibar#differences-with-spicedb) in a Q&A format that helped clarify the concepts. If the number of new concepts and terminologies seems overwhelming, that's okay! You don't have to understand all of it from the start, and hopefully, the rest of this article will help with your learning journey.


## 2. Get the Hang of Schema Design


Schema design is central to SpiceDB and was a new concept for me. A schema essentially defines the types of objects in your system, how those objects relate to one another, and the permissions that can be computed from those relations. I started by watching[this video](https://youtu.be/x3-B9-ICj0w?si=gU3yDuI4ngy4pdPi) on modeling the GitHub permissions system using Schema.


For practice, I used real-life examples (such as Google Groups or a banking system) and sketched out the different users, objects, and relationships between them. Progressing from a basic user-document schema to a complex real-life example provides valuable practice in designing schemas for SpiceDB.


You can experiment with modeling these in[the SpiceDB playground](https://play.authzed.com/) . I encourage you to try it out.


## 3. Build Something Starting from a Point of Familiarity


Having worked at companies like Amazon Web Services (AWS) and Fermyon, I have background knowledge in Cloud, Compute, and Serverless technologies. I looked through the documentation for familiar territory and found[Deploying SpiceDB on Elastic Kubernetes Service](https://authzed.com/docs/spicedb/ops/deploying-spicedb-on-eks) . My experience with Amazon EKS helped me understand how SpiceDB integrates into that system.


If you come from an application development background, you might prefer starting with[one of our client libraries](https://authzed.com/docs/spicedb/getting-started/client-libraries) to build a simple app that communicates with a local SpiceDB instance. Our getting started guide[Protecting A Blog Application](https://authzed.com/docs/spicedb/getting-started/protecting-a-blog) can be particularly helpful. For those with authorization experience, we offer guides on[how SpiceDB compares with Open Policy Agent (OPA)](https://authzed.com/docs/spicedb/getting-started/coming-from/opa) or[a comparison with Ruby on Rails CanCanCan](https://authzed.com/docs/spicedb/getting-started/coming-from/cancancan) . Both show different approaches but share some common ground.


SpiceDB is[completely open-source](https://github.com/authzed/spicedb/) , and we welcome community contributions! Whether you'd like to suggest improvements, fix documentation typos, or contribute to the community, please feel free to do so. Check out our[Good First Issues](https://github.com/authzed/spicedb/labels/good%20first%20issue) and join our[Discord community](https://authzed.com/discord) .


## 4. Use AI Strategically


While learning to deploy SpiceDB on Amazon EKS, I encountered some challenges (a natural part of learning) and consulted ChatGPT about these errors. Here's a debugging step that I received:


(For context: zed is the AuthZed CLI tool)


Pretty straightforward, right? Well, except that` config` is not a zed CLI command. LLMs can hallucinate and often do so with a lot of confidence. Watch out for inconsistencies like these that could trip you up when copying code from an LLM.


This highlights an important distinction between "learning something" and "building something". Asking ChatGPT "How do I install SpiceDB on EKS" and then just spamming the copy-paste keys is not the best way to **learn** something. I can attest to this because it's exactly what I did at the start! Only partway through did I realize that I hadn't achieved what I set out to do and had to backtrack. On the other hand, asking an LLM about *how* I could start debugging certain errors gave me a good understanding of what's under the hood. Use these tools thoughtfully and purposefully.


## One Final Thought


I'm on a roll with the advice, so here's one more thing (yes, that's a[Stevenote](https://en.wikipedia.org/wiki/Stevenote) reference). This has held me in good stead over the years when learning anything new: **enjoy the process** , the results will follow.


Happy Learning!


P.S. Here's a webinar I recorded for CNCF about Deploying SpiceDB in EKS. There's nothing quite like learning in public! 😎


On this page


- 1. Start with the Basics
- 2. Get the Hang of Schema Design
- 3. Build Something Starting from a Point of Familiarity
- 4. Use AI Strategically
- One Final Thought


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
