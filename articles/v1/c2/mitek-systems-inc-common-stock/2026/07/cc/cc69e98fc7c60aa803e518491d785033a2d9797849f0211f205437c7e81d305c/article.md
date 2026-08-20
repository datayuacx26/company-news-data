---
schema_version: "1.0.0"
document_id: "cc69e98fc7c60aa803e518491d785033a2d9797849f0211f205437c7e81d305c"
company_key: "mitek-systems-inc-common-stock"
company: "Mitek Systems Inc."
source_id: "mitek-systems-inc-common-stock-rss-3bf79578716e"
canonical_url: "https://www.miteksystems.com/blog/what-is-synthetic-identity-fraud-and-why-is-ai-making-it-harder-to-detect"
published_at: "2026-07-21T17:49:10+00:00"
first_seen_at: "2026-07-21T18:36:40.036348+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:a2c134d1ce707aa73ac928f1b92d74e32d5e29f91ad81ddb4eea6e0ab98b46f9"
---

# What is synthetic identity fraud and why is AI making it harder to detect?

Charge-offs happen at every financial institution. But as fraud investigators conduct their post-mortem reviews, they’re sometimes surprised to learn that the “customer” who stopped making their card or loan payments was a person who never really existed in the first place. Synthetic identity fraud, where a fictitious person is constructed from a composite of real and fabricated data, has become the fastest-growing financial crime in the United States.


According to[Datos Insights](https://www.miteksystems.com/resource-library/white-papers/the-synthetic-identity-crisis-detection-prevention-and-the-ai-arms-race) , U.S. unsecured credit synthetic identity fraud losses reached approximately US$2.94 billion in 2025, up from US$1.80 billion in 2020, on a trajectory that is continuing upward.


To appropriately defend against synthetic identity fraud, institutions must understand what it is, how it can evade traditional detection, and how the ubiquity of generative AI tools makes synthetic identity fraud even easier.


## What is synthetic identity fraud?


As defined by the Federal Reserve, synthetic identity fraud is the use of a combination of personally identifiable information (PII) to fabricate a person or entity, with the intent of committing a dishonest act for personal or financial gain. In practice, the foundation of the synthetic identity is often a real Social Security number (SSN) belonging to a child, an older person with limited credit activity, a dead person, or someone with no credit history. These SSNs typically have no or little credit history (referred to as a “thin file”). This SSN is then paired with a fabricated name, date of birth, address, and other information. The result is a synthetic identity sometimes called a “Frankenstein ID,” because it’s assembled from some real parts, but doesn’t belong to a real person.


To create a working synthetic identity, the real SSN is the linchpin – applications without valid SSNs are rejected outright by identity verification systems. The valid SSN allows the synthetic identity to pass this basic onboarding check that’s used by most financial institutions.


Uniquely, synthetic identity fraud is different from other financial crimes in one major way – the length of the operation. Unlike account takeovers or other types of fraud where criminals will try to extract as much money as possible as fast as they can before the victim notices, synthetic identity fraud is a long-running scheme. The synthetic identity needs to incubate for a period of time where it look ordinary and generates a legitimate credit history with increased credit limits before the fraud occurs. This makes synthetic fraud even more difficult to detect, and even more costly in the long run.


## How it differs from traditional identity theft


In traditional identity theft schemes, a real person is victimized – a criminal obtains their login credentials or PII, and uses this info to access their existing accounts or open new accounts in their name. It usually isn’t very long before the victim notices strange charges, or gets an alert from a credit monitoring service. The victim then files a report, and the bank’s investigation begins.


But synthetic identity fraud is much different. The primary victim is not any real person, but instead, the financial institution. The real person whose SSN was the foundation of the synthetic identity may never be aware that their number was used. They won’t get alerts or see fraudulent charges themselves. The synthetic identity is basically operating in parallel to them, accumulating credit under a different name, with a different address.


For financial institutions, that means detecting synthetic identities requires a proactive process rather than their typical reactive response to fraud investigation. Institutions often discover traditional identity theft when they’re alerted to it by a victimized consumer, and react accordingly. Synthetic fraud is silent until the fraudster chooses to “bust out” the identity, max out their credit lines and stop paying. Often times, institutions charge these accounts off and classify the loss as a credit default, never realizing it was fraud at all. If a financial institution is dependent on consumer fraud alerts to trigger their investigations, they’ll consistently miss synthetic identity fraud.


## Why blended real and fake data evades controls


A properly designed synthetic identity is specifically engineered in ways that exploit the gaps in identity verification systems. Fraudsters know that most financial institutions use three things to verify identity: document verification, a match to a known record in a credit bureau or government database, and the absence of fraud flags (e.g., history of fraudulent activity) associated with the provided information.


With a real SSN, the identity has been given a legitimate anchor in government records. Then, the supporting details like name, address and date of birth are designed to be plausibly aligned to it. Fraudsters will choose attributes that are just consistent enough with the SSN owner, like an address in the right region (the state of issue for a SSN used to be discernible from its first three digits), a date of birth that’s aligned with the SSN issue date, and a name that’s common enough to belong to a multitude of real people. Even though the identity is completely fictitious, the combination looks realistic enough to pass a basic algorithmic check.


The credit bureau thin file provides no data that disputes this identity, either. When a new identity applies for credit with no associated credit history, this is interpreted by many lenders neutrally. They see it as the absence of negative reporting, rather than as a red flag. While the first application is often immediately denied, this also isn’t a bad thing for the synthetic identity. The act of applying creates a new file at the credit bureau, and the fraudster can continue applying and establishing the identity’s existence. Automated underwriting systems will often approve low-limit credit lines for these thin files, and once the first account is opened, the process of building a larger credit profile for the synthetic identity can begin. The account holder will make their payments on time for months, “seasoning” the identity and increasing its creditworthiness, just like responsible activity would for a real person.


## How AI accelerates creation, scale and sophistication


Synthetic identity fraud isn’t new – it’s been around for decades. But the emergence of artificial intelligence (AI) tools, especially generative AI, have made creating a synthetic identity much less labor-intensive.


Generative AI tools can produce highly realistic identity documents. This includes drivers’ licenses, passports, utility bills, and pay stubs, all of which pass visual human inspection and can even challenge automated document verification systems. Unlike manually created documents that were scanned and modified, the generated versions can even contain the metadata signatures associated with real documents, rendering them much harder to detect.


AI can also rapidly create plausible backstories, address histories, and more for synthetic personas. For a synthetic identity applying for a mortgage, fraudsters might generate an employment history, references, and even a digital footprint that aligns with the identity. This process formerly required a highly time-consuming amount of manual research, but it can now be automated at scale with minimal human review.


Synthetic images and videos can also be created with AI. As high-quality image and video generation tools become more accessible, the attack surface for synthetic identity fraud broadens. AI-generated images, and even deepfake videos, are being used to defeat liveness detection, making it look like a real and live person is present during remote onboarding. \[I went light on detail here, assuming we’d link to our other blogs on this\]


The threat is only expected to intensify: 74% of fraud executives globally anticipate AI-powered voice cloning and deepfake fraud will grow significantly over the next three years, and 55% expect significant growth in synthetic identity fraud fueled by compiled data-breach information that gives these AI-powered systems the raw materials they need to build ever-more-plausible synthetic identities.


The ease of generating synthetic identities with AI enables a significantly higher volume of fraud attempts. Before, fraudsters would each manage just a handful of synthetic identities, manually; now, with AI-powered automation tools, they can create and maintain hundreds or thousands. This scale can dramatically increase the potential losses caused by any one fraud ring and can also make it harder for investigators to identify patterns that link accounts together. Highly sophisticated fraud rings can also leverage AI-powered tools to simulate the sort of tests a financial institution will use on their identities and documents, allowing them to experiment with what works and what doesn’t before they take their actions live.


[The Datos report](https://www.miteksystems.com/resource-library/white-papers/the-synthetic-identity-crisis-detection-prevention-and-the-ai-arms-race) indicates that 40% of financial institutions have already seen evidence of increased attack rates that are directly attributable to generative AI. It’s a figure that almost certainly understates true exposure, since AI-assisted identity construction typically occurs weeks, months, or even years before a synthetic identity is actually used to apply for an account - so these early, visible examples are likely a harbinger of how many synthetic identities created by AI are incubating right now.


## Why this matters now for financial services


Financial institutions and fintechs are facing a convergence of pressures that amplify the risk from synthetic identity fraud. Frictionless, digital-first onboarding, which accelerated during the pandemic and has become a customer expectation, has enormously expanded the attack surface. The friction presented by visiting a branch and presenting documents in person made the effective use of a synthetic identity much more challenging. Seamless remote onboarding presents an opportunity that sophisticated fraud operations are actively exploiting.


Institutions are also facing regulatory pressure to take action in this area. The Consumer Financial Protection Bureau (CFPB) and the Federal Reserve have both issued guidance on synthetic identity fraud and the problem presented when it is misclassified as credit loss, demonstrating their increased focus in this area. If a financial institution can’t differentiate between a genuine credit default and a synthetic fraud bust-out, it’s mismanaging its risk models and its compliance obligations.


Competitive pressure to get customers onboarded as quickly and painlessly as possible is also creating opportunities for fraudsters. Institutions that seek to maximize onboarding speed and user experience, particularly fintechs and digital lenders, are disproportionately targeted by synthetic fraud rings for precisely this reason. The fraudsters know these organizations’ controls are calibrated to generate the least friction possible. But traditional banks and credit unions aren’t immune either, as their legacy systems often lack signal integration that can help catch sophisticated synthetic identities as they operate over extended periods of time.


The consequences of inaction go far beyond direct financial losses. The biggest risk is portfolio contamination: active fraud rings that commit synthetic fraud concentrated in particular products or origination cohorts can degrade credit portfolio quality over months or even years before their bust-out wave. By the time the losses occur, intervention is impossible.
