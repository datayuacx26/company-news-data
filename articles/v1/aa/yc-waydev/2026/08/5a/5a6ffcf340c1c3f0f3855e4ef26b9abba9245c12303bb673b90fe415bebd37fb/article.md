---
schema_version: "1.0.0"
document_id: "5a6ffcf340c1c3f0f3855e4ef26b9abba9245c12303bb673b90fe415bebd37fb"
company_key: "yc-waydev"
company: "Waydev"
source_id: "yc-waydev-rss-a82ef0eb6171"
canonical_url: "https://waydev.co/forward-deployed-engineers-are-everywhere-can-you-prove-theyre-working/"
published_at: "2026-08-13T09:07:08+00:00"
first_seen_at: "2026-08-13T09:45:09.710402+00:00"
fetched_at: "2026-08-13T09:45:11.241504+00:00"
content_hash: "sha256:b7a40489978b9b0a7cb199bf85eded502c8ad660aa77522f5f77573745e567f2"
---

# Forward Deployed Engineers Are Everywhere. Can You Prove They’re Working?

Waydev Insights · Engineering Intelligence


Every AI company is hiring FDEs. Almost none of them can measure whether the pain-to-product loop is actually converting. That’s an engineering data problem, and it’s solvable.


**A note on inspiration:** this post was sparked by an excellent essay from Jesse Zhang, CEO of Decagon, on the rise of the FDE role and why his team chose a product-driven path instead. His piece looks at the question from the builder’s seat. This one looks at it from the measurement seat: how engineering leaders can see, in their own data, whether an FDE motion is compounding or quietly becoming a services business.


“Forward deployed engineer” has become the default answer to every hard question in AI go-to-market. Deployments are painful? Hire FDEs. Customers can’t self-serve? FDEs. Product isn’t ready yet? FDEs. Anthropic and OpenAI have both built enterprise deployment arms modeled on Palantir, and FDE job postings have exploded over the past year.


What’s strange is that this role was, until recently, something you got criticized for. Putting engineers inside customer accounts was read as a signal that you didn’t have a real product. Your revenue was lower quality. Your margins were structurally capped. Nothing about those economics has changed. What changed is that AI created a wave of genuinely new categories where nobody, not the vendor and not the customer, knows what the workflow looks like yet.


So companies are sending engineers into the field to find out. The question engineering leaders should be asking is a different one: **how do you know when it’s working, and how do you know when to stop?**


## The pain-to-product loop


Palantir popularized the FDE model in the mid-2000s. Their CTO Shyam Sankar had a line for it:


“FDEs eat pain and excrete product.”


Shyam Sankar · CTO, Palantir


Early deployments were deeply bespoke, built to answer one question for one customer. But the point was never the bespoke work itself. The point was that every painful, custom deployment fed platform primitives back into the core product. The ontology, the permissioning model, the workflow engine. Those primitives became Foundry, margins climbed into the 80s, and the company eventually moved away from an FDE-led motion entirely.


The FDE team was never the business model. It was the mechanism for building the right product.


That framing contains a measurable claim, and this is where most companies lose the plot. The pain-to-product loop only works if pain is actually converting into product. If your FDEs are eating pain and excreting more pain, you don’t have an FDE team. You have a services business with a better title.


## The trap is not starting. It’s not stopping.


Keeping FDEs in the field is easier in every single sprint. You never have to make a hard product tradeoff. You never have to decide which of two customer requests wins, or where the configuration surface ends. Nobody says no to anyone, no painful architectural call gets made, and the customer is delighted.


Meanwhile your cost to serve never declines. Your growth becomes bounded by hiring. Every bespoke fix in the field is a product decision you chose not to make.


The problem is that this failure mode is invisible from the outside. Revenue looks fine. NPS looks fine. Customers are happy because they’re getting free custom engineering. The rot only shows up in the engineering data, and most organizations aren’t looking at it.


The pain-to-product loop


Field pain →


platform primitive
Bespoke share of work ↓


every quarter
Time to first value ↓


per deployment cohort
Field code lands in core repos


Cost to serve ↓


as customers grow


A services business in disguise


Field pain →


one-off patch
Bespoke share of work flat or ↑


Deployment 30 takes as long as deployment 3
Field code dies in customer branches


Growth bounded by hiring


Figure 1. Same job title, same happy customers, opposite trajectories. Only the engineering data tells them apart.


## What engineering intelligence tells you about your FDE motion


This is fundamentally a measurement problem, and it’s exactly the kind of question engineering intelligence exists to answer. If you’re running an FDE or deployment engineering motion today, five signals separate a pain-to-product loop from a services business in disguise.


1


Bespoke work vs. core product work, over time


Scope the work your deployment engineers do against customer-specific repos, branches, or services versus the core platform. In a healthy motion, the bespoke share of total engineering effort declines quarter over quarter. If it’s flat or growing while your customer count grows, your FDEs are absorbing pain, not converting it.


2


Deployment velocity as a trend, not a snapshot


Each deployment should make the next one easier. Time from contract to first value should compress with every cohort. If deployment 30 takes as long as deployment 3, nothing is compounding. Decagon reports that two-thirds of its deployment work now runs autonomously through its own product, with first launches taking days even at large banks and telcos. That compression is the visible output of a working loop.


3


Where the code from field work actually lands


When an FDE returns from a customer engagement, does anything ship into the mainline product? You can see this directly in the flow of work: commits, pull requests, and reviews that originate from deployment context and land in core repositories. If field artifacts live and die inside a single customer’s environment, the discovery benefit you’re paying for isn’t materializing.


4


Rework and duplication across deployments


If three different FDEs solved the same integration problem three different ways for three different customers, that’s a product gap generating waste on a loop. Duplicated effort across customer-facing work is one of the clearest signals that something should have been promoted into the platform already.


5


The cost side of the equation


FDEs are expensive senior engineers. Their fully loaded cost against the accounts they serve is part of your real cost to serve, and it belongs in the same ROI conversation as your AI tooling spend. A deployment motion that never gets cheaper caps your margins permanently, and that ceiling should be visible to your CFO before it’s visible in a down round.


## The distinction that matters: discovery versus implementation


One more thing worth separating in your data. FDE work is not the same as implementation work. “Build this integration into their ticketing system” is real and necessary, but it’s execution against a known spec, not discovery of an unknown one. Bundling both under one title is how companies convince themselves that a growing services org is a product investment.


The distinction matters more now because AI is rapidly eating the implementation half. Models write integration code well enough that much of what an implementation team did in 2023 is becoming something the product does itself. If you can separate discovery work from implementation work in your engineering data, you can see which half of your deployment cost AI should be eliminating, and whether it actually is. That’s an AI ROI question, and it deserves the same auditable rigor as any other one.


Two kinds of field work, two different futures


Discovery


Finding a workflow nobody has seen yet. Irreplaceable, human, and the only legitimate reason to keep senior engineers in the field. Output: product requirements.


Implementation


Execution against a known spec. Necessary today, but increasingly something AI and the product itself should absorb. Output: shrinking cost to serve, if you’re measuring it.


## Ask the real questions, with real data


Go forward-deployed early if you’re in a genuinely new category. Get the signal. There is no substitute for watching your product break in ways your tests never imagined.


But then ask the questions Palantir eventually asked, and answer them with data rather than intuition. Is the bespokeness in your customer’s environment, or in your own product gaps? Is the last mile irreducible, or just unbuilt? Are your FDEs discovering something, or absorbing something? And what, concretely, got built into the product the last time one of them came back from the field?


Every one of those questions has an answer sitting in your engineering data right now. The companies that win the FDE era won’t be the ones that hired the most forward deployed engineers. They’ll be the ones that could see, quarter by quarter, whether the pain was becoming product.


Waydev · Engineering Intelligence


See whether your pain is becoming product.


Waydev measures where engineering effort goes, how deployment velocity trends, and what AI investment actually returns, with auditable evidence your CFO can stand behind.


[Book a demo](https://waydev.co/demo/)


Sources and inspiration


1. Jesse Zhang (CEO, Decagon), essay on X about the rise of the FDE role, Palantir’s pain-to-product history, and Decagon’s product-driven deployment approach with Duet. This post was directly inspired by his piece.
2. Shyam Sankar (CTO, Palantir), the “FDEs eat pain and excrete product” framing, as recounted in the essay above.
3. Joe Lonsdale (co-founder, Palantir), on the long-held view of Palantir as a consultancy and forward deployment as a necessity in a category with no established workflows.
