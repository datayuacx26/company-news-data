---
schema_version: "1.0.0"
document_id: "c82adc40e81709221a19584bfee7fa4f4d3ac97d48cba5789ca2b51c2024b653"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/opendoor-case-study"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-21T19:22:10.257736+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:61ad16b475a91ca274bf1009929331b55490199e89dab5af4d3e26c181368f67"
---

# How Opendoor processes millions of pages to accelerate homeownership

> At this point, in even just a few months' time, it has put hundreds of thousands of dollars into our bottom line. And it's prevented us from making mistakes that, if a human had been looking at those documents, it's very likely they would have made a mistake.
>
>
> **Yan Lhert, Staff Engineer, Opendoor**


[Opendoor](https://www.opendoor.com/) exists to tilt the world in favor of homeowners and those hoping to become homeowners. The company has served 300k+ homeowners by making homeownership simpler, faster, and fairer. Behind every transaction is a mountain of paper: an average home sale involves 200 to 300 documents, each of which can run 200+ pages.


Yan Lhert is a Staff Engineer on Opendoor's title and escrow team. We spoke with him about why Opendoor stopped trying to build document processing in-house, how his team reached near-production in days, and what it looks like to run millions of pages per month through Extend.


Jump to the full transcript ↓


## **The challenge: non-negotiable accuracy for mission-critical documents**


Buying and selling homes is one of the most document-heavy processes in the economy, and almost none of that data arrives structured.


> A home transaction will involve somewhere between 200, 300 documents. Most of the context is locked inside of PDFs. PDFs are the API that run home sales.


Opendoor historically closed that gap with manual labor. Teams spent most of their document processing time manually typing out fields from PDFs and comparing documents against each other by hand, which was a cumbersome process rife with errors. And in title and escrow, an error is not a negligible mistake.


> The stakes are extremely high. \[A mistake could result in\] as much as the full loss of a property or very serious litigation.


## **Build vs buy considerations**


Opendoor has the engineering talent to build almost anything, so the team's first instinct was to build document processing themselves. That instinct didn't survive production contact with the frequently encountered edge cases like bounding boxes, eval sets, splitting documents, and tables that span multiple pages.


> The build-versus-buy equation is more biased towards building than ever, and we ran into a number of accuracy issues when we were trying to build this ourselves. Things like bounding boxes, eval sets, splitting things, tables that span multiple pages. Quickly we learned we need a better tool that can help us. One of the other engineers on our team, Dharma, sat me down and said, 'Hey, why don't we just build this ourselves?' It didn't take long before he came back around to me and said, 'Yeah, we should just use Extend.'


The team evaluated the market before committing, starting with AWS Textract and moving through other startups in the space.


> Extend was by far the best in terms of all-around performance and easiest to set up, best team to work with. It was kind of a no-brainer.


## **The solution: accurate, scalable document processing with Extend**


Once Opendoor chose to implement Extend, the question was how fast the team could get to production and how many use cases they could scale up to. Extend's documentation and an` llms.txt` that dropped straight into a coding agent compressed deployment into a matter of days.


> The docs were incredibly good, and with llms.txt we were able to throw it into our favorite Claude Code session and get rolling right away to near production experience with the product within a matter of days.


Today, Extend runs in several production paths at Opendoor, across settlement extraction and AI HUD QC, processing millions of pages per month. In title and escrow, HUD QC verifies thousands of critical fields on every file.


> AI HUD QC allows us in title and escrow to verify thousands of pieces of critical information. It's been a huge win for us.


Just as important as the product was the team behind it.


> There were moments when it felt like Extend was part of our team. You would forget that they were not even working here.


## **The impact: hundreds of thousands of dollars saved across multiple use cases**


For a team where a single missed field can mean the loss of a property, the mistakes that never happen are worth as much as the hours saved. Within a few months, both showed up: in dollars recovered and in errors avoided.


The dollars are real, but Lhert is just as quick to point to the errors Extend catches that a human reviewer, hundreds of pages deep, would likely have missed. And when asked what would happen if Extend disappeared, he didn't hedge:


> I'd probably have to quit Opendoor if Extend vanished tomorrow, because I've learned to rely on it for so many things and I absolutely love using it.


---


## **The complete transcript**


*The full interview with Yan Lhert.*


### Where do documents come into the picture at Opendoor?


**Lhert:** Buying and selling homes is extremely document-heavy. A home transaction will involve somewhere between 200, 300 documents. Most of the context is locked inside of PDFs. PDFs are the API that run home sales.


### What was document processing like at Opendoor before Extend?


**Lhert:** Opendoor historically has spent most of its document processing time on humans, manually typing out stuff that's in a PDF. It's mostly comparing documents against each other. Extremely cumbersome process, rife with errors.


### What's at stake when document processing goes wrong?


**Lhert:** The stakes are extremely high. It could be as much as the full loss of a property or very serious litigation. The build-versus-buy equation is more biased towards building than ever, and we ran into a number of accuracy issues when we were trying to build this ourselves. Things like bounding boxes, eval sets, splitting things, tables that span multiple pages. Quickly we learned we need a better tool that can help us.


### How did you evaluate the options? What did you benchmark against?


**Lhert:** We started out looking at some tools like Textract and moved on to some of the other startups in the space. Extend was by far the best in terms of all-around performance and easiest to set up, best team to work with. It was kind of a no-brainer.


### Did you consider building it in-house?


**Lhert:** One of the other engineers on our team, Dharma, sat me down and said, "Hey, why don't we just build this ourselves?" It didn't take long before he came back around to me and said, "Yeah, we should just use Extend."


### What differentiated Extend in your testing?


**Lhert:** The docs were incredibly good, and with llms.txt we were able to throw it into our favorite Claude Code session and get rolling right away to near production experience with the product within a matter of days.


### Which use cases have you rolled out on Extend?


**Lhert:** Extend is now used in a number of production paths at Opendoor. AI HUD QC allows us in title and escrow to verify thousands of pieces of critical information. It's been a huge win for us.


### Has Extend materially impacted Opendoor's business?


**Lhert:** At this point, in even just a few months' time, it has put hundreds of thousands of dollars into our bottom line. And it's prevented us from making mistakes that, if a human had been looking at those documents, it's very likely they would have made a mistake.


### If Extend vanished tomorrow, what would be the impact?


**Lhert:** I'd probably have to quit Opendoor if Extend vanished tomorrow, because I've learned to rely on it for so many things and I absolutely love using it.
