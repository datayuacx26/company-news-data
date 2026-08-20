---
schema_version: "1.0.0"
document_id: "4495e1b0af7e17faa47d08198db11f1a66a3b105e62603e8cec4e0f38785487a"
company_key: "yc-invert"
company: "Invert"
source_id: "yc-invert-news-import-968576ef11e8"
canonical_url: "https://invertbio.com/blog/engineer-blog-series-from-bioprocess-to-software-with-anthony-quach"
published_at: "2025-11-19T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:27.297188+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:0c9f04019d4b3a0a604df5e6a6f7332f0f55e94a3d3d37ca0a23927a5d1c52ca"
---

# Engineer Blog Series: From Bioprocess to Software with Anthony Quach

*Welcome to Invert’s Engineer Blog Series — a behind-the-scenes look at the product and how it’s built.*


*In this post, software engineer*[Anthony Quach](https://www.linkedin.com/in/anthony-quach) *shares how his career in bioprocess development led him into software, and how that experience shapes the engineering decisions behind Invert.*


## Tell us about your background before joining Invert.


My educational background is in chemical engineering and I spent nearly a decade working in bioprocess development and process engineering. Most of my work was in CDMOs in an R&D group, where I focused on next-generation manufacturing processes, fed-batch development, and perfusion bioreactor technologies.


That work involved running a lot of experiments and handling an even larger amount of data. I was constantly juggling different process versions, clones, and customer programs — and almost all of that work required pulling data from multiple systems and stitching it together manually.


Those years gave me a deep understanding of how difficult bioprocess data management really is.


## How did you transition from bioprocess into software engineering?


It happened slowly. Even though I enjoyed the science, I kept running into the same issue: most of my bottlenecks were issues with data, rather than issues with process-development. I was spending more time fixing spreadsheets, dealing with missing metadata, or looking for historical data than actually analyzing anything.


I got really interested in the software side and eventually pursued a second bachelor’s degree in computer science while working full-time. I was lucky to work under leaders who supported that growth and understood that the biotech industry could learn from the tech industry.


After leaving biopharma, I wanted to stay close to the domain, but focus on solving the problems I understood firsthand. Invert was a perfect overlap — the intersection of bioprocess, data, and software engineering.


## How does your bioprocess experience influence the way you build software at Invert?


It influences everything. I’ve sat in the seat of the scientist using these tools, so I think a lot about reducing manual lift and cognitive burden.


When I build features, I ask myself:


- Would this have saved me hours back then?
- Is this eliminating duplicate work?
- Does this make scientific communication smoother?
- Is the experience intuitive?


One example is **Saved Views** , which I helped build. When you’re supporting multiple programs, you spend a huge amount of time curating subsets of runs manually. Saved Views lets you pre-filter your data so you can instantly load exactly what’s relevant. It seems simple, but it’s extremely high-value when you’re switching contexts all day.


Another example is **Reports** , which is one of my favorite features. In my old roles, communicating findings was painful because the context behind your data never traveled with the analysis. With Reports in Invert, the underlying data, quantities, and events automatically move with the document. Everyone sees the same context — something I really wish I’d had during my time in the lab.


## From your perspective, what makes bioprocess data uniquely challenging?


A few things make it hard:


- The sheer volume of data
- The cost and value of generating it
- The amount of metadata that’s missing, inconsistent, or hidden
- The fact that most context only exists in someone’s head


Scientists end up spending a lot of energy deciding what’s relevant and important to analyze. Without the right infrastructure, the mental load is huge.


One thing I appreciate about Invert is that our data model is designed specifically around bioprocess. It reflects how data is actually created and used, which means we can support new equipment, processes, and workflows without forcing scientists into unnatural structures.


## Do you feel like you’re building tools you wish you had earlier in your career?


Absolutely. Pretty much every feature hits a pain point I remember dealing with personally.


Reports is a great example of that. Scientific communication is one of the hardest parts of process development. Without shared context, teams spend a lot of time debating data instead of discussing insights. Reports eliminates that gap — the context originates from a transparent, single source of truth.


If I’d had a tool like that earlier, it would have saved me countless hours and would have improved the quality of communication.


## What are you most excited to work on next?


I’m really excited about **Invert Assist** , our AI-powered analysis feature we just released. Bioprocess data is an ideal use case for LLMs because there’s so much depth, and a lot of the insights are buried in time series and metadata relationships.


Assist can surface insights or hypotheses in minutes — work that normally takes hours or even days manually. Even when it’s not perfect, it can nudges scientists toward the right question to ask or highlights something unexpected. That’s a massive shift from the tools that are available to the industry today.


There’s nothing else like it right now, and I’m excited to keep improving it.


**Anthony Quach** is a Software Engineer at Invert and a former bioprocess development engineer. His domain experience and engineering background help drive the development of intuitive, high-impact tools built for scientists.


‍
