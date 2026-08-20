---
schema_version: "1.0.0"
document_id: "95302cb4f9d0dc7b6acfbd2f832d0b3dc35af188875b25f0ab6431302a508d7c"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/how-contentful-builds-demos-with-firecrawl"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:35.258497+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:a2d720e78aa74b0cb948ad4c3adf3a658c44f99f9a43d0fe6b03e51e6df55b8f"
---

# How Contentful Turns Any Website Into a Personalized Demo With Firecrawl

*Contentful uses Firecrawl to turn any prospect's website into structured content for DemAI, an AI-driven demo app that assembles live web pages into a personalized Contentful demo.*


## About Contentful


[Contentful](https://www.contentful.com/) is a digital experience platform (DXP) that helps teams personalize, optimize, and create standout digital experiences at scale. Founded in 2013, it pioneered the headless CMS space and now powers content experiences for more than 4,000 organizations, including 30% of the Fortune 500, with brands like KFC, Docusign, and Mailchimp building on it.


## Winning Contentful demos start with a prospect's own site


Scott Rouse is a Solution Specialist at Contentful, where he builds demos that help prospects understand what's possible with the platform. For him, the best demo is a prospect's own site running in Contentful. To deliver that experience at scale, his team built DemAI, an AI-driven demo app that turns any company's website into a personalized Contentful demo.


The hard part is turning those pages into structured content. At first the team did it by hand, pulling markdown from a prospect's site and extracting the image tags one by one. Every website is built differently, so every new prospect meant redoing that extraction from scratch.


Now, Contentful trusts Firecrawl to automatically turn any prospect's website into structured content for its demos.


> Firecrawl lets us build powerful demos for any company, from startups with dozens of pages to Fortune 500s with thousands.


*— Paolo Negri, Co-Founder & CTO, Contentful*


## Every prospect's site is built differently; Firecrawl returns them all as clean markdown


When a solution engineer starts a new demo, DemAI uses Firecrawl to map the prospect's site and fetch its pages live. Most requests go through[/scrape](https://docs.firecrawl.dev/features/scrape) , which returns any page as clean markdown that maps onto a content model. DemAI also uses[/map](https://docs.firecrawl.dev/features/map) to discover the URLs across a prospect's site and pick which pages to pull.


The solutions team ran its first job in early 2025, and Firecrawl has since spread to a dozen teams across Contentful, as more of them build[AI workflows that need the web](https://www.firecrawl.dev/use-cases/ai-platforms) .


> I'm watching someone put a workflow together now and they just replaced a generic web search tool with Firecrawl to get richer context, faster.


*— Scott Rouse, Solution Specialist, Contentful*


---


**Ready to power your AI application with reliable web data?**[Try Firecrawl](https://www.firecrawl.dev/) and ship faster.
