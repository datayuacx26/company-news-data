---
schema_version: "1.0.0"
document_id: "61d0dd2e27db8fb5053a2e527829b53ee9d6fb5eb2656859f3b114206188f10f"
company_key: "palantir-technologies-inc-class-a-common-stock"
company: "Palantir Technologies Inc."
source_id: "palantir-technologies-inc-class-a-common-stock-rss-f87a89a6619a"
canonical_url: "https://blog.palantir.com/ready-set-build-with-the-nhs-federated-data-platform-41405fa4c226"
published_at: "2026-04-30T17:39:05+00:00"
first_seen_at: "2026-07-20T03:31:10.454667+00:00"
fetched_at: "2026-07-28T20:51:50.549866+00:00"
content_hash: "sha256:d6082c764a966fc2014968b98c209da6ac51a1dfcbb6f55dda02bff1b6b6b636"
---

# Ready, Set, Build with the NHS Federated Data Platform

# **Ready, Set, Build with the NHS Federated Data Platform**


[Palantir](https://palantir.medium.com/?source=post_page---byline--41405fa4c226---------------------------------------)


6 min read


·


Apr 30, 2026


--


Press enter or click to view image in full size


The National Health Service (NHS) has delivered universal healthcare to an entire nation for over 75 years. With 1.5 million staff providing care to approximately 57 million patients across hundreds of hospital trusts — using decades old legacy infrastructure — the NHS is one of the most operationally complex organisations on earth.


For most of its history, the NHS has run on data that couldn’t talk to itself. Patient records, waiting lists, operating theatre schedules, discharge plans: all this critical information was stored in separate systems that NHS staff couldn’t see, couldn’t share, and couldn’t act on together, as one system. Together with the NHS, Palantir built the NHS Federated Data Platform (NHS FDP) to address this challenge.


Press enter or click to view image in full size


## Built to Connect Data, Safely and Securely


The NHS FDP is an NHS England programme delivered through a consortium led by Palantir. It provides a data infrastructure layer that connects the NHS to itself, bringing together operational data scattered across disconnected systems. The platform is giving clinicians, analysts, and operational staff access to the information they need, when they need it, in a secure and safe environment.


Built on Palantir Foundry, the NHS FDP operates with a federated architecture, meaning that each NHS organisation operates its own separate ‘tenant’ of the FDP, for which they are the data controller. All data therefore stays under control of the local organization while still benefiting from nationally developed digital tools and a shared ontology, called the[Canonical Data Model](https://www.england.nhs.uk/digitaltechnology/nhs-federated-data-platform/nhs-fdp-explained/how-does-the-nhs-federated-data-platform-work/#:~:text=to%20do%20so.-,Canonical%20Data%20Model,-The%20Canonical%20Data) — a single, standardised representation of the NHS ecosystem that unifies not just data, but the logic, actions, and security policies that together power how applications built atop the platform interoperate at scale. Privacy by design is at the heart of the platform: all data is encrypted, access is purpose-based, and[Privacy Enhancing Technology (PET)](https://www.england.nhs.uk/digitaltechnology/nhs-federated-data-platform/security-privacy/how-we-are-protecting-privacy-and-confidentiality/) is used to de-identify personal information whenever data flows nationally.


On the foundation provided by the platform, hospital trusts gain access to a suite of nationally developed core products — purpose-built tools for managing waiting lists, coordinating discharges, and validating patient pathways — while also gaining the ability to build their own solutions to the challenges most specific to them.


The result is a platform designed not just to process data, but to enable action, safely and securely.


## Impact at Scale


The[results](https://www.england.nhs.uk/digitaltechnology/nhs-federated-data-platform/impact/fdp-uptake-and-benefits/) are already visible. Hospital trusts using the platform have delivered measurable improvements across the system:


- 110,078 additional patients have undergone procedures in hospital theatres since the FDP product has been implemented — supporting more people to receive the treatment they need to stay healthy.
- 797,728 patients have been safely removed from NHS waiting lists using the FDP tools, helping ensure the right people are waiting for the right treatment.
- 297,943 patients have been safely discharged from hospital using the software, with around a 15% reduction in discharge delays for patients observed since trusts started using FDP — freeing up beds for those who need them most.
- 6.8% more patients with referrals for suspected cancer are now receiving answers within 28 days compared to the 12 months before FDP.


These numbers represent real reductions in waiting times, discharge delays, and administrative burden — and real differences in the lives of patients across England.


**Data as of end of December 2025 (updated quarterly).*


## Local Solutions, System-wide Impact


The core products enabled by the NHS FDP are just the beginning. What sets the platform apart is that the same infrastructure powering nationally developed tools is available to every trust to build on. The Canonical Data Model that sits at the platform’s foundation means that locally built applications can interoperate, be validated, and ultimately be scaled across the system.


## Get Palantir’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


A solution built by a nurse at one Trust to solve a problem on her ward can become a tool used by nurses across England. This principle — that local challenges are best solved by the people closest to them on a platform powerful enough to carry those solutions system-wide — is what the NHS FDP is built for.


## From Principle to Practice


**Build with FDP** was created to put that vision to the test. This flagship developer event is a 48-hour hackathon bringing together engineers, analysts, clinicians, and operational staff from NHS organisations across England to explore the platform capabilities and build solutions to real problems on the NHS Federated Data Platform.


In the weeks before the inaugural event, NHS staff submitted operational challenges they wanted solved. Those problems became the foundation of the day: participants were grouped into multidisciplinary teams, each tackling a challenge nominated from the frontline, with access to the full NHS FDP toolset and support from Palantir engineers throughout.


As Ines Pinto, Product Manager at Chelsea and Westminster Hospital NHS Foundation Trust,[put it](https://www.linkedin.com/posts/nhs-transformation-directorate_buildwithfdp-nhsfdp-digitalhealth-activity-7448316376966074369-QR-4?utm_source=share&utm_medium=member_desktop&rcm=ACoAAC4i9qsBRSI10JVmjvEqFohBYKZ3xOtmrW0) : *“The FDP, being so simple to use and intuitive, has actually created a really interesting blend between the non-technical and the technical world…you can actually integrate both in a way that’s going to make things work, because you can’t have one without the other.”*


Press enter or click to view image in full size


## 48 Hours of NHS FDP


At the inaugural event, over 120 NHS developers and analysts built 25 working prototypes on a notional version of the CDM — functional applications built from frontline expertise. A selection of the hackathon projects includes:


**Early Warning System** — For overstretched ward teams, the ability to act earlier — rather than react to deterioration once it has happened — is the difference between prevention and crisis. The prototype tool would help nurses identify patients at risk of clinical deterioration before it occurs. Leveraging models trained on vitals, interventions, and outcomes from across the NHS and be able to suggest proactive interventions and connect learnings system-wide.


Press enter or click to view image in full size


**Bed Demand Prediction** — When Emergency Departments surge, reactive bed management costs time that patients don’t have. This prototype tool would enable live predictions of which ED patients are likely to require admission, enabling teams to get ahead of demand before it becomes a crisis. Trained on 300,000 rows of notional data and integrated with AI-generated recommendations based on real-time ward availability, hospitals could use the foresight to manage surge rather than just respond to it.


**CAMHS Workflow Demand Balancer** — In Child and Adolescent Mental Health Services, planning for demand has historically meant working from incomplete pictures and outdated data. This prototype tool demonstrated clinicians, managers, and executives could be given a live view of future demand and capacity to identify bottlenecks and surface where additional staff might be needed before problems escalate. Able to generate multiple business cases in minutes the prototype turned what was once a multi-week planning process into something operational staff could run themselves.


Press enter or click to view image in full size


**Genomic-Optimised Waitlist Recommendations —** This prototype tool automatically flags patients on the waitlist eligible for genomic tests and determine their optimal treatment. By surfacing the right candidates at the right time, it helps ensures operational efficiency in the use of precision medicine and faster paths to effective care.


Press enter or click to view image in full size


These four projects are a snapshot of what was built. Across the full 25 solutions, teams tackled everything from workforce planning and discharge risk to automated appointment booking and AI-driven clinical coding — each one a direct response to a challenge nominated by NHS staff.


## Building What’s next


Each team was given the opportunity to present their project to a panel of senior NHS leaders and government representatives — including chief executives, chief information officers, and heads of data and analytics — to demonstrate what 48 hours on the NHS FDP makes possible and prove that the next chapter of digital transformation is already underway.


The NHS FDP was designed as a builder’s platform, and what the NHS developers built at the first **Build with FDP** event proved that they were just getting started.


Press enter or click to view image in full size


The next event will take place this spring.
