---
schema_version: "1.0.0"
document_id: "59a8b80f81835de43a03f718649dc7d59803a6c6a3ffcce8eb18eae0778a4bbc"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/why-we-built-mage-after-kirkland"
published_at: "2026-04-26T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:30.029696+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:dcd39e7d5bfc32b40525a0625df25b79db960dd4d735f4613a62d5e3f2c3cc93"
---

# Why We Built Mage After Kirkland

I spent the early part of my career at Kirkland & Ellis, on M&A. The work was interesting and the people were excellent. The hours were also brutal in a way I now think was fixable.


This is the long form of why Mage exists. I get asked it on roughly every customer call, and the founder pages on most legal AI sites read like marketing. So this is the actual answer.


## What the work felt like


A typical Tuesday during a deal: get the data room around midday, work through the night reading the most material commercial contracts, surface the obvious issues for partner review, then start over the next morning on the second tier of contracts. By Friday, ten or twelve associates would have read the same anti-assignment, change-of-control, and indemnity clauses across 1,200 documents, taking the same notes, flagging the same issues that flagged on every deal.


The work was real. Each issue mattered. The cumulative effect was that a roomful of very smart, very expensive lawyers spent most of their time on the lowest-leverage part of the practice.


This is not a complaint about Kirkland. The same shape exists at every M&A practice I have seen since. It is the structure of the work, not any one firm's failing.


## What I noticed


The associate work that mattered most — the calls with clients, the structuring conversations, the negotiation prep, the partner-mentorship moments — happened in the small windows around the diligence reading. The diligence reading itself was the constraint.


When I would step back, the obvious question was: why are humans the engine for this? The reading is mechanical. The judgment about what to do with the readings is the part that requires an attorney's training.


The answer in 2018 was: because there was nothing else. Generic AI was bad at contract analysis. The legal AI tools that existed were extraction-focused, single-document oriented, not built around the full M&A workflow. Practitioners who had tried them mostly went back to associates because the tools missed too much.


By 2023-2024 the picture had changed. Frontier LLMs were good enough that the model layer was no longer the bottleneck. The bottleneck was now the chassis around the model — the workflow shape, the multi-document reasoning, the output quality. We covered this in[The F1 Engine Problem](https://magelegal.com/blog/f1-engine-problem) . The model is commodity; the chassis is the work.


That is when Mage made sense.


## What we are building


Mage is the chassis I wish I had when I was reading 1,200 contracts on a Tuesday. The tool ingests the data room, classifies every document, runs a partner-defined risk pass overnight, resolves amendment chains, drafts the disclosure schedule from the underlying agreements, and produces a memo in firm voice. The associate's Tuesday looks different now: they review the system's findings, push back on what they disagree with, and spend the rest of the time on the high-leverage work the partner used to wait until Friday for.


It is not a small thing for the practice. It is, I think, the biggest shift in how M&A is done in the last twenty years.


## Why I am the right person to build this


There are a lot of legal AI startups in 2026. Most are built by engineers who have never been inside a data room. The build-for-what-you-imagine-the-work-is gap shows up the first time a real deal hits the platform.


I have lived inside the work. I know what an indemnity package interaction looks like at the partner level. I know which clauses M&A counsel actually scan for and which are noise. I know how a deal team's day flows, where the bottlenecks are, and what partner-grade output looks like.


Raymond, our CTO, was a Document AI engineer at Google Cloud. He built document-understanding systems at scale before founding Mage with me. The combination — practitioner plus systems builder — is what produces a tool that fits the work as practiced.


We have an honest team page at[/team](https://magelegal.com/) with the credentials. The product is what we built together.


## What I want for the next generation of associates


When I think about the associates who will go through M&A practice in the next ten years, I want them to do different work than I did. The reading-everything-once stage should not be the bottleneck. The judgment-and-counseling stage should be where they spend their hours. The career trajectory should be: start on negotiation prep and structuring earlier, learn to think like a partner faster, lead deals sooner.


That is the practice the tool unlocks. Mage is not a productivity tool for partners. It is a leverage tool for associates, built so the next generation gets the career the previous generation didn't quite get.


## What we are not


We are not a firm-wide AI assistant. We do not try to be Harvey or Legora; those tools serve a different need. We have written about that distinction in[Mage vs. Harvey](https://magelegal.com/blog/mage-vs-harvey-feature-by-feature) and the broader category piece in[Legal AI vs. Harvey vs. Generic AI](https://magelegal.com/blog/harvey-vs-kira-vs-infrastructure-legal-ai) .


We are not a generalist contract analysis tool. We do not try to be Kira or Luminance; those tools serve a different shape of work. See[Mage vs. Kira](https://magelegal.com/blog/mage-vs-kira-feature-by-feature) and[Mage vs. Luminance](https://magelegal.com/blog/mage-vs-luminance-feature-by-feature) for the comparisons.


We are M&A. End to end. Specifically.


## How to see it


If you are at a firm with serious M&A volume and you want to see what the tool actually does on a real deal,[request a demo](https://magelegal.com/?demo=1) . Bring a current deal or a recent one. We will run end-to-end diligence on it and walk you through the result against your manual workproduct.


If you would rather read first, the master view is[Legal AI for M&A: The Practitioner's Guide](https://magelegal.com/blog/topics/due-diligence) . The operational view is[AI Due Diligence: An Operational Playbook](https://magelegal.com/blog/topics/due-diligence) . Both are honest about what the tool does and what it doesn't.


— Raffi
