---
schema_version: "1.0.0"
document_id: "fd66b9ebf72391258358318a3f16c3139ffd0e0d4ac9402b5f8a13ec5cf3d96d"
company_key: "yc-cofactr"
company: "Cofactr"
source_id: "yc-cofactr-news-import-06c16db4e4eb"
canonical_url: "https://www.cofactr.com/articles/itar-for-software-for-hardware"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-27T08:34:13.149498+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:387b67d7cf7058e7450f5209d345f02180e663df9372aa0498c731a121e0b44b"
---

# How We Accomplished ITAR Compliance for Our Software

## Part 1


If you are involved with building software for use by hardware teams working in aerospace or defense, there is a good chance that ITAR compliance has come up.


When we founded Cofactr, we knew early on that we would eventually want to become ITAR compliant, but we found it very difficult to find clear information on what would actually be involved in that process. Ultimately, customer demand pushed us to prioritize ITAR compliance and, with the help of some friendly experts, we were able to complete the process of becoming ITAR registered and compliant in just a couple of months.


My goal with this post is to share some of what we learned through that process and provide some clear, simple, guidance on what is actually involved in becoming ITAR compliant if you are building software for use by hardware teams.


## What is ITAR?


ITAR stands for[International Traffic In Arms Regulations](https://en.wikipedia.org/wiki/International_Traffic_in_Arms_Regulations) and specifically refers to a United States Department of State regulatory regime and related laws. When we talk about ITAR, we are often using this term as a catch-all for multiple related US regulatory programs including ITAR, the Arms Export Control Act (AECA), the United States Munitions List (USML), and Export Administration Regulations (EAR).


Fundamentally, all of these laws are related to global trade and exports. The US government’s goal is to restrict and regulate exports from the United States of defense-related technologies. Other countries have similar laws (for example CGP in Canada), but those are outside the scope of this discussion.


It’s important to remember that technical data related to physical items regulated under ITAR is also, by extension, regulated under ITAR in much the same way as the physical item itself. For example, if the USML includes a rocket, not only are we restricted from exporting that rocket from the United States, but we are also restricted from exporting data that could help someone build that rocket themselves such as a technical drawing or bill of materials.


### Does ITAR apply to me?


If you are building hardware in aerospace or defense, there is a very good chance that ITAR applies to you, but this article probably won’t cover the information that you need to know.


If you are building software, particularly cloud software, that could be used by teams building hardware in aerospace or defense, there is ALSO a very good chance that ITAR applies to you, and this article will cover some information that you need to know.


This is not the kind of export of data that we are talking about here.


### Why does ITAR apply to my software product?


When a user uploads technical data that is related to an export-controlled item to your software product, you have a responsibility to prevent intentional or unintentional export of that technical data.


Thanks to the movies, exporting defense technical data might bring to mind images of a USB key in a titanium attache case, but in practice, it’s much easier and more boring to run afoul of these laws. All it takes is an individual who is not a US-person viewing a file and, bam!, it’s been illegally exported. For example, if a member of your team who is not a US person opens up an export-controlled customer file to help troubleshoot a software bug, that file has just been illegally exported.


### What’s this US-person thing?


These various export controls are all about preventing non-US-persons from accessing defense data, so it helps to know what a US person even is. In classic legalese form, a US person does not have to be a US citizen, and they don’t even have to be a person.


They do have to be a lawful permanent resident of the United States or any corporation, business association, partnership, society, trust, or any other entity, organization or group that is incorporated to do business in the United States. It also includes any governmental (federal, state or local) entity.


## Okay, I want to become ITAR certified. What do I do?


Let me stop you right there. Unlike audit-driven certifications like ISO9001 or SOC2, ITAR is not a certification so you can’t become ITAR certified.


You can (and must) become ITAR **registered** , which involves filling out some paperwork with the U.S. Department of State, Directorate of Defense Trade Controls (DDTC), and paying an annual fee that is currently $2,250 or a bit more depending on some specifics of your business.


You also will need to become ITAR **compliant,** which is a matter of putting in place the staff training, technical controls, and internal procedures required to ensure that you actually are following the relevant laws.


There is no process for the government or a 3rd party to come to check that you are actually following the rules, but if you get caught not following the rules, the consequences can be very severe (think massive fines and possible jail time), so this isn’t a good area to move fast and break things.


Practically, for a software business, you’ll need to worry about implementing the required procedural and training programs and ensuring that your software technology meets the relevant data security requirements.


For your procedural and training programs, it is very unlikely that you will be able to figure that stuff out yourself unless you’ve done this before (in which case you shouldn’t be wasting your time with this article) so you will need to hire a consultant to help you out. This will look pretty similar for every business so your consultant will be able to give you some fairly out-of-the-box solutions like pre-made training materials and then they will tweak them as needed for the specifics of your situation. Expect to spend somewhere in the neighborhood of $15-30k and receive a bunch of slide shows that you can use for employee training and guidance on any areas where you will need to tighten things up within your operations. If you need recommendations for a consultant, you can reach out to me on LinkedIn, and I will introduce you to the one that we worked with.


Additionally, you will need to implement some administrative programs like Restricted/Denied Party Screening to make sure your staff, office visitors, vendors, customers, etc aren’t on terrorist watchlists and such. This will cost you a few thousand dollars per year for access to the relevant software from a company like Descartes or Thompson Reuters.


Ensuring your software itself is compliant is the hardest topic to get clear guidance on but, in practice, likely won’t prove to be super difficult. The key requirement is that you are ensuring that only US-persons have access to export controlled data.


You will need some basic protections like end-to-end encryption compliant with Federal Information Processing Standards Publication 140-2 (FIPS 140-2) but, in all likelihood, you already have this if you are using a modern web development stack.


At Cofactr, we have a pretty common stack with a React/TS frontend and a Django/Postgres backend, and we didn’t have to change anything about our core application to meet the end-to-end encryption requirement.


So many acronyms....


You will also need to use cloud hosting providers that are authorized by the Federal Risk and Authorization Management Program (FedRAMP) for portions of your application that process or store export-controlled data. In our case, this meant switching from a normal AWS region to an AWS GovCloud region. If you use one of the major cloud providers, you should be able to make the switch to the FedRAMP version of their service unless your application relies on their more esoteric or recently introduced services. In our experience, customers who need ITAR seem to prefer that we host with AWS GovCloud or Azure FedRAMP, but that’s mostly anecdotal.


The hardest part is ensuring that technical data can’t be exposed to any related services that you may use as part of your tech stack such as log monitoring, product analytics, customer support software, etc. If you determine that technical data could be exposed to one of those tools, you have three options:


1. Migrate to an on-prem/private-cloud version of the product that is hosted in a FedRAMP cloud
2. Disable that tool for customers with export-controlled data
3. Engineer some sort of data obscuration into your application to prevent controlled data from being exposed to that tool


At Cofactr, we have employed all three strategies. We have migrated some products we use to on-prem in the same AWS GovCloud region we use for our own application, we have disabled some product analytics entirely for the ITAR-compliant version of our product, and we have engineered other parts of our system to insulate export-controlled technical data from SaaS products that cannot legally handle that data.


### Is there any way around this?


The only way to avoid having to comply with ITAR is to prevent customers from using your product with export-controlled technical data. This will cut off your ability to serve customers in defense and much of aerospace, but only you can determine whether you will get enough business from these industries to justify the cost and effort required to comply with ITAR. At[Cofactr](https://www.cofactr.com/get-started) , we love working with aerospace startups, so it was an easy decision for us to become ITAR compliant.


## Part 2: CMMC and FedRAMP — What Changed Since I Wrote This


*Added June 2026*


I wrote the original version of this post in 2024, and in the time since, the ground has shifted enough that a Part 2 is warranted. Two acronyms that were lurking at the edges of the ITAR conversation back then — CMMC and FedRAMP — have moved to center stage. CMMC is now actually being enforced, and FedRAMP is in the middle of its biggest reinvention since it launched. If you're building software for hardware teams in defense, both of these now matter to you in a way they didn't when I first hit publish.


The good news is that the mental model from Part 1 still holds, and most of the hard infrastructure work you'd do for ITAR is the same work you'd do here. The bad news is that there's more of it, and some of it now comes with someone showing up to check. Let me walk through it.


## What is CMMC?


CMMC stands for Cybersecurity Maturity Model Certification. It's a Department of Defense program, and it exists to make sure that the contractors and subcontractors in the defense supply chain are actually protecting sensitive government information on their systems, rather than just promising they are.


Here's the thing that makes CMMC interesting in contrast to ITAR. Remember how I told you that ITAR isn't a certification — that you become **registered** and **compliant** , but no one comes to check? CMMC is the mirror image. CMMC very much **is** a certification, and depending on the level you need, somebody absolutely does come to check.


It's organized around two categories of government data. The lighter one is Federal Contract Information (FCI): information provided by or generated for the government under a contract that isn't meant for public release. The heavier one is Controlled Unclassified Information (CUI): the more sensitive bucket that includes technical drawings, specifications, and the kind of data that — if you were paying attention to Part 1 — sounds an awful lot like the export-controlled technical data we were already worried about for ITAR. That overlap is the whole reason this belongs in the same post.


On top of those data types sit three levels. Level 1 covers FCI and is satisfied by an annual self-assessment — the lightest touch. Level 2 covers CUI, is built on the 110 security controls in NIST SP 800-171, and depending on the contract is either a self-assessment or, more often for anything that actually matters, a third-party assessment by a Certified Third-Party Assessment Organization (a "C3PAO"). Level 3 is for the highest-sensitivity programs, layers on controls from NIST SP 800-172, and is assessed directly by the government's own DIBCAC (the Defense Industrial Base Cybersecurity Assessment Center).


So unlike ITAR, where you self-certify into the void, CMMC Level 2 and Level 3 mean a real audit by a real assessor. The move-fast-and-break-things warning from Part 1 still applies, except now the consequences can show up at assessment time rather than only after a violation.


## Does CMMC apply to me?


If you're a hardware company in defense, almost certainly — and like the ITAR section, that's somebody else's article.


If you're a software company whose product is used by defense hardware teams, then it applies to you indirectly but unavoidably, and this is the part worth understanding.


The mechanism is a DFARS clause you'll get to know well: 252.204-7012, universally called "7012." It's in basically every DoD contract that touches CUI, and it says that if a contractor uses an external cloud service to store, process, or transmit that CUI, the cloud service has to meet security requirements **equivalent to the FedRAMP Moderate baseline.**


You are an external cloud service. If your customers put CUI into your product — and if you serve defense hardware teams, they will — then their CMMC obligations flow straight downhill to you. Your customers literally cannot use your product for that data unless you can demonstrate FedRAMP Moderate or equivalent. CMMC took this from a nice-to-have into a hard gate, and it made your customers legally responsible for verifying you're on the right side of it.


## A caveat about our own situation


I should flag a wrinkle here, because Cofactr doesn't actually fit the clean version of the story I just told — and the difference matters enough that I don't want to lead anyone astray.


Everything above describes a pure software vendor: a cloud service that customers log into to handle their own CUI. If that's you, the rule is refreshingly simple. You're a Cloud Service Provider in the government's eyes, your obligation is FedRAMP Moderate (or equivalency), and you do **not** need to be CMMC certified yourself. Your customers carry the CMMC obligation; you just have to be a cloud service they're allowed to use.


Cofactr is different. We're technically a service business — procurement, kitting, and managed supply chain — and the software is one part of how we deliver that service rather than the whole product. Because we handle our customers' CUI ourselves in the course of doing that work, and not merely as a tool they operate, we land on the other side of the line: we're a service provider in the defense supply chain, so the framework that applies to **us** is CMMC, not FedRAMP.


This is genuinely an edge case, and it's not where most readers will end up. If you're building normal software for hardware teams — a product your customers use to manage their own data — you almost certainly don't need CMMC. You need FedRAMP Moderate or equivalency, full stop. CMMC only lands on the vendor itself when, like us, the vendor is actually operating as a service provider or subcontractor that handles CUI to perform work, rather than simply providing a cloud tool. If you're genuinely unsure which bucket you're in, that's exactly the question to run by an expert before you spend a dollar — the two paths look nothing alike in cost or effort.


## Why this matters now and not in 2024


When I wrote Part 1, CMMC was perpetually "coming soon." It's not coming soon anymore. The DoD published the final acquisition rule in September 2025, and it took effect on November 10, 2025. That's the date CMMC requirements started appearing in new DoD contracts, and it's phasing in over three years.


Phase 1 began on November 10, 2025, with Level 1 and Level 2 self-assessments showing up as conditions of award. Phase 2 starts November 10, 2026, when Level 2 third-party C3PAO certifications become required where applicable. Phase 3 starts November 10, 2027, bringing Level 3 / DIBCAC assessments into play for the most sensitive programs. Phase 4 lands November 10, 2028, for full implementation across all applicable contracts.


The practical takeaway: the deadline that matters isn't 2028, it's whenever the phase that triggers your customers' contracts arrives. If they need you to support their CMMC obligations, plan against that date, not the end state.


## The FedRAMP plot twist


In Part 1, I mentioned FedRAMP exactly once, as something you *consume* — you pick a FedRAMP-authorized cloud region (we moved to AWS GovCloud) and move on. That advice is still correct as far as it goes. But the equivalency requirement above changes the stakes, because there's now real pressure for your **product itself** — not just the infrastructure underneath it — to carry FedRAMP authorization.


A quick refresher, since I breezed past it last time. FedRAMP (the Federal Risk and Authorization Management Program) is the US government's standardized program for security-assessing and authorizing cloud products. It has three impact levels — Low, Moderate, and High — and for CUI, Moderate is the relevant bar.


Here's the wrinkle that catches people. Running on AWS GovCloud, which holds a high FedRAMP authorization, does **not** mean your application is FedRAMP authorized. The infrastructure's authorization and your application layer's authorization are two different things, and the government cares about the thing your customer actually logs into.


For years there was a softer path here called "FedRAMP Moderate equivalency," where instead of a full authorization you asserted that you met the equivalent controls. Then, in a December 2023 memo, the DoD defined what "equivalent" actually means — and the short version is "basically the whole thing." Equivalency now requires a full assessment by a third-party assessor (a 3PAO) and a complete body of evidence, with the contractor on the hook for verifying it. The shortcut got a lot less short.


The cleanest answer, where it's achievable, is a real FedRAMP Moderate authorization listed on the FedRAMP Marketplace. The DoD has been explicit that a FedRAMP Moderate authorized offering satisfies the entirety of the 7012 cloud requirement and can be leveraged without further assessment. The problem, historically, is that getting there was brutal.


## What is FedRAMP 20x, and why it's actually good news


Traditional FedRAMP authorization is the kind of thing that makes a startup wince: 18+ months, a mountain of documentation, six or seven figures of cost, and — the real killer — a requirement to find a federal agency willing to sponsor you before you could even start. For an early company, that sponsorship requirement was often a non-starter on its own.


FedRAMP 20x is the government's attempt to fix this, and it's the most significant change to the program since it was created. It launched as a pilot in early 2025. The core idea is to replace slow, manual document review with automation. Instead of writing prose about how you secure data and having a human read it, 20x uses Key Security Indicators (KSIs) — discrete, specific, machine-checkable requirements. Rather than "data must be secured," it's "data is encrypted with this validated algorithm," and that gets validated automatically against machine-readable evidence.


Three things about that matter for startups serving A&D. The first is speed: the goal is to compress authorization from 18+ months down to something closer to a few months. The second is that the new path drops the agency-sponsor requirement, which by itself opens the door to startups that previously couldn't even get in line. The third is that it leans on continuous, automated validation rather than a giant point-in-time paperwork exercise, which fits how a modern software team actually operates.


As for where it stands as I write this in mid-2026: Phase One piloted the Low baseline through 2025, was open to the public, drew far more interest than the program expected, and produced the first batch of 20x authorizations. Phase Two focused on the Moderate baseline — the level that actually matters for CUI — ran with small, invite-only cohorts from late 2025 into early 2026, and has since wrapped. The wide, public rollout of Low and Moderate is the next milestone, targeted for the back half of 2026, with High and a migration of the older "Rev5" authorizations to follow after that.


Treat those dates as directional. This program is iterating in public and the timeline has already moved more than once. But the direction is unambiguous: FedRAMP is becoming something a startup can realistically achieve, which — combined with CMMC enforcement going live — is exactly the convergence that makes all of this worth caring about now rather than later.


## Where this leaves you (and us)


Stack Part 1 and Part 2 together and here's the mental model for a SaaS company serving defense hardware teams in 2026:


1. **ITAR** governs whether non-US-persons can touch export-controlled technical data in your product. Register, get compliant, and lock down US-person access.


2. **CMMC** is your customers' obligation, but it flows down to you the moment they put CUI in your product. You can't be the weak link in their certification.


3. **FedRAMP** Moderate — or genuine equivalency — is increasingly the concrete bar you have to clear to be that not-weak-link, and 2 **0x** is finally making it attainable on a startup timeline and budget.


At Cofactr, the GovCloud foundation we built for ITAR turned out to be the right base to build everything else on, which is the quiet upside of doing the hard infrastructure work early. The controls overlap heavily, so the marginal cost of each additional framework is lower than the first one was. If you're staring down this stack for the first time and want to talk through how the pieces fit — or you want an intro to the assessors and consultants who actually know this space —[reach out to me on LinkedIn](https://www.linkedin.com/in/matthew-haber-/) , same offer as Part 1.


## Important Legal Disclaimer


I am very much not a lawyer. Sadly, I haven't even played one on TV. This article reflects our experience with the process of becoming ITAR compliant, and there may be all sorts of specifics of your business that are different than ours. This information is provided for informational purposes only, and should not be construed as legal advice on any subject matter. You should not act on the basis of this content without consulting your own relevant lawyers and experts first.


And the disclaimer goes double for Part 2: CMMC and FedRAMP 20x are moving fast enough that anything specific here — dates, phases, what "equivalent" means this quarter — could be out of date by the time you read it. Check the current rules and talk to your own experts before you act.
