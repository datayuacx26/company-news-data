---
schema_version: "1.0.0"
document_id: "3d635cdfdc594af1f5048d83e196cf0e1137bba0f9486c6256fe8ad1ffc6382a"
company_key: "yc-stream"
company: "Stream"
source_id: "yc-stream-news-import-7d3282ed79f9"
canonical_url: "https://www.stream.claims/post/the-build-buy-boomerang-lessons-from-the-front-lines-of-enterprise-ai"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-24T02:35:27.763107+00:00"
fetched_at: "2026-07-28T22:15:44.247179+00:00"
content_hash: "sha256:5ac841c294f16a4571e7018a8dc5e4e406ac81c5908d9f27a0477c03480f028d"
---

# The Build-Buy Boomerang: Lessons From the Front Lines of Enterprise AI

A recent article from a16z's Kimberly Tan,[Where Enterprises Are Actually Adopting AI](https://a16z.com/where-enterprises-are-actually-adopting-ai/) , reports that 29% of the Fortune 500 and 19% of the Global 2000 are now live customers of leading AI startups. That's a real shift from July of last year, when an[MIT study](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/) reported that 95% of generative AI pilots were failing to reach production.


At Stream, we've felt the shift too: every enterprise we speak to either has an AI strategy or is actively developing one. Enterprise buyers have become more sophisticated and AI-savvy, and the pilot landscape has evolved accordingly. This time last year, most buyers needed guidance on how to structure and measure pilot success. Today, the vast majority of pilots have a comprehensive grading rubric and run a bake-off with multiple vendors tested simultaneously. These bake-offs often require more than just a superior product to win. Yet despite being pitted against bigger, better-resourced competitors over the last twelve months, Stream converted over 80% of enterprise pilots, landing top P&C carriers, defense attorneys, and state agencies.


An important caveat many learn too late is that, unlike with smaller businesses, finishing a successful pilot with a large enterprise does not guarantee conversion. We have experienced it ourselves at Stream, and we've noticed a particularly interesting pattern emerge with enterprise buyers:


**The Build-Buy Boomerang**


1. You finish a successful pilot. The users and champions send a strong positive signal, and you start preparing for procurement.
2. Internally, the executive team, lacking an in-depth understanding of the product and its technical complexity, reduces the offering to “a chat feature.”
3. In-house IT teams, leveraging the latest agentic coding tools, push to build instead of buy.
4. Six months into building, they hit a roadblock: it becomes clear they underestimated the complexity and investment required to build a production-ready product.
5. Finally, after falling behind by 6–12 months, they decide to buy.


‍


In this article, I'll describe my observations from building AI in one of the most regulated and risk-averse industries, and share what enabled Stream to evolve successfully and build strong partnerships with enterprise customers.


## **Production Perfect**


Stream, the AI platform for claims experts, operates in a highly regulated industry, deals with sensitive PII and PHI, and assists adjusters and attorneys managing millions of dollars in exposure. The stakes are very high, and mistakes can be disastrous. That said, even in lower-stakes domains, the gap between “works in a demo” and “works in production” is wider than most teams appreciate.


Avoiding this pattern isn’t easy, but it shortens the sales cycle and saves prospects months of wasted effort. To prevent it you must bring them in and expose them to the inner workings of your product. In our case, we walk prospects through what the Expert-in-the-Loop version of the platform looks like, including confidence scoring and the tooling that empowers humans to verify outputs. We give an in-depth walkthrough of our ingestion and processing pipelines, as well as the tools available to our agents for different workflows. We feel confident sharing all of this because if it were easy to replicate, this venture wouldn't have been worth pursuing.


In future articles, we'll dive deeper into the technical aspects of the Stream platform and share more about the problems we've tackled and solved. In the meantime I will share a quick overview of what's coming:


**Page Stream Segmentation.** Breaking down a large merged PDF into individual documents. Sounds easy, doesn't it? For anyone who has experience building document processing pipelines, this is one of the first surprisingly non-trivial challenges you face.


**Electronic Health Records (EHRs).** Because we don't operate inside a healthcare system, FHIR APIs and native EHR integrations aren't available to us. In practice, that means there's no standard format for the clinical records we ingest. Epic, Cerner, Allscripts, and Meditech each emit something different, and each installation tweaks the layout further. Without bespoke processing pipelines and a reliable structural layer, everything downstream starts drifting.


**OCR and Vision.** Bad scans, handwriting, checkboxes, diagrams, and scribbles: despite advances in AI, these remain a critical gap in every document processing pipeline. We've tested every serious OCR and AI-driven solution on the market. None are perfect.


**Layered Verification.** Every datapoint you get wrong causes a ripple effect. A wrong extraction can cause a wrong conclusion and a bad outcome. At scale, small errors don't average out. *They multiply* . Building the right controls and guards is a must in order to combat real-world edge cases.


Production systems don't get the luxury of failing gracefully, and in industries like ours, you can't afford to fail at all. That's the bar you need to build toward every day. It's the foundation everything else rests on.


## **Earned Autonomy**


Agentic coding has made building products faster than at any point in the history of software. Any decent engineer can ship a polished prototype in a weekend. This change has a counterintuitive consequence: the scarce resource has shifted. The advantage is no longer whether you can build it, but whether you understand the customer well enough to know what to build.


At Stream, we leaned into this state of mind early and built our platform to facilitate it. Our account owners embed with every customer during pilot and onboarding, tailoring the product to their playbooks, terminology, and edge cases. There is no one-size-fits-all configuration in insurance claims. A carrier's auditing rubric, a defense firm's file review process, and a state agency's chronology preferences aren't edge cases. They're the work.


SaaS orthodoxy says high-touch doesn't scale. In enterprise AI, we believe the opposite. With AI, you can scalably “do things that don't scale.” The unglamorous work of sitting with adjusters and counsel, listening, adjusting, and co-designing against the actual workflow *is the product* . Skipping it is how you end up with demos everyone loves and nobody uses.


Change management is a critical part of the value chain. No user wants to replace a workflow they've been doing for 10 years in favor of yours just because you say your tool can do it. You have to meet them where they are, integrate into their existing systems, adapt to their vocabulary and SOPs, and move the line on what's automated only as fast as their teams trust you to move it.


Autonomy is earned, not assumed. In the world we operate in today, building products has been democratized. The trust-earning part is still human work, and in enterprise AI, it's where real value is created.


## **The Eighth Wonder**


Compound interest is often called *the eighth wonder of the world* because of its unique ability to generate exponential growth by earning interest on interest. The compounding effect is not limited to investing, and with precise planning and tireless execution, it can generate value of many kinds, including in enterprise AI.


To achieve this wonder, you first have to put the right pieces in place. For Stream, those pieces are the foundations and partnerships we've invested in. The foundation, as described in Production Perfect, provides the confidence and scaffolding on which we build. The partnership is the common language and understanding you've developed with your customers. Without your customers, your vision cannot become real, so you must bring them along on the journey and make them part of the story for both of you to succeed.


Most enterprise software, insurance included, has been built around systems of record for the last 40 years: document stores, databases, and workflow engines that file and route but don't decide. The vision we see, and are building toward, is a **system of action** .


Stream is the agentic harness for adjudicating claims. Just as Claude Code or Codex lets developers ship 10x, Stream lets claims experts become the most highly leveraged version of themselves, elevating users from administrative processors to strategic decision-makers.


Doing the hard things early, putting the right pieces in place, and building toward a vision alongside your customers creates a compounding advantage. We've felt this in our enterprise pilots: every hard problem you solve makes it easier to create the next layer of value, easier for customers to see it, and harder for competitors to replicate it.


## **Conclusion**


Many enterprise AI pilots still fail, and the ones that succeed aren't distinguished by better prompts or better models. They're distinguished by teams that get three things right: they engineer for perfection, they earn autonomy by fostering trust and partnering with the customer, and they build systems whose value compounds over time.


That's the bar we hold ourselves to at Stream. And if you're evaluating AI vendors, it's the bar worth holding them to as well.


‍


*Written by Eilam Levitov, CTO & Co-founder, Stream Claims*
