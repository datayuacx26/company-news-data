---
schema_version: "1.0.0"
document_id: "f7f440df69b92411e7e038f87b47e15a5beb37530bf0484ebb5ce53eb6770258"
company_key: "yc-ai-2"
company: "&AI"
source_id: "yc-ai-2-rss-536318f0f92f"
canonical_url: "https://www.tryandai.com/blog/sound-view-innovations-v-hulu"
published_at: "2026-02-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:52.502979+00:00"
fetched_at: "2026-07-28T22:21:24.537254+00:00"
content_hash: "sha256:4ef5558379b6c7099274a0bb932f86fdc2b0e414c844dc639d35bbba466e658a"
---

# Sound View Innovations, LLC v. Hulu, LLC: Federal Circuit Affirms Noninfringement

[Back to insights](https://www.tryandai.com/blog)


# Sound View Innovations, LLC v. Hulu, LLC: Federal Circuit Affirms Noninfringement


February 7, 2026


[Caleb Harris](https://www.linkedin.com/in/caleb-m-harris/)


On January 29, 2026, the Federal Circuit, in a precedential decision, affirmed a grant of summary judgment of noninfringement in *Sound View Innovations, LLC v. Hulu, LLC* , No. 2024-1092 (Fed. Cir. Jan. 29, 2026) ( *Hulu II* ). Circuit Judge Chen authored the opinion, with Circuit Judges Prost and Wallach joining.


## Key takeaways


- The Federal Circuit affirmed summary judgment of noninfringement for Hulu (precedential, Jan. 29, 2026) because claim 16 of the expired U.S. Patent 6,708,213 requires its steps in the written order—which Hulu's products undisputedly did not follow.
- Implicit step ordering was compelled by grammar and logic: the past participle "requested" modifying "SM object" means a request must precede the later step referencing "said requested SM object."
- The court reversed the district court's narrower "specialized buffer" construction but affirmed on the independent step-ordering ground.
- Implicit ordering does not require showing the later step is inoperable without the earlier one, which appears to lower the threshold.
- Practical caution: past-participle modifiers used for antecedent basis in method claims can inadvertently lock in step order and deserve early scrutiny.


## The Case


U.S. Patent No. 6,708,213, which is now expired, discloses methods for reducing network latency in streaming media systems by using intermediate “helper servers” to cache content, coordinate distribution, and adjust data transfer rates. By the time of this appeal, only independent claim 16 of the ’213 patent remained at issue.


### Procedural Posture


In 2017, Sound View sued Hulu in the Central District of California, alleging infringement of six patents. After construing several terms, the district court granted summary judgment of noninfringement because it found that Hulu’s edge servers (the alleged “helper servers”) did not download and retrieve portions of the same streaming media object in the same buffer. The Federal Circuit affirmed the district court’s claim constructions but vacated summary judgment and remanded with instructions to adopt “an affirmative construction of the term ‘buffer.’” *Sound View Innovations, LLC v. Hulu, LLC* , 33 F.4th 1326, 1334–36 (Fed. Cir. 2022) ( *Hulu I* ).


On remand, the district court, again, granted summary judgment of noninfringement on two independent grounds, based on its findings that (1) claim 16 requires a specialized buffer and (2) the first limitation of claim 16 must be performed before the second limitation.


As discussed in detail below, the Federal Circuit reversed the district court on its first ground, but affirmed the second.


## Analysis


### Buffer Construction


On remand, the district court construed the claimed “buffer” as a specialized buffer dedicated to a single streaming media object, thereby excluding Hulu’s general-purpose buffers from infringement. The Federal Circuit disagreed, finding nothing in the claim language, specification, or prosecution history that narrowed “buffer” beyond its plain and ordinary meaning.


### Implicit Step Order


The more interesting, dispositive issue, however, was whether claim 16 requires its first two steps to be performed in the order written. (Neither party disputed that Hulu’s accused products do not perform the steps in that order.)


Method claims do not ordinarily require an order of steps unless “the claim language, as a matter of logic or grammar, requires that the steps be performed in the order written, or the specification directly or implicitly requires\[\] an order of steps.” *Interactive Gift Express, Inc. v. Compuserve Inc.* , 256 F.3d 1323, 1342 (Fed. Cir. 2001); *Mformation Techs., Inc. v. Rsch. in Motion Ltd.* , 764 F.3d 1392, 1398 (Fed. Cir. 2014).


Here, the Federal Circuit held that both grammar and logic compelled an implicit order. Particularly, the past participle “requested” in claim 16 acts as an adjective modifying “SM object,” and as a result, logically, a request must have occurred before the streaming media object can be described as “requested.” Because the second step expressly references “said requested SM object,” the Court concluded that it necessarily depends on the first step having been performed.


### Sound View’s Rebuttals


Sound View raised three arguments against implicit ordering, with the Federal Circuit rejecting all three.


First, Sound View pointed to claims 10 and 13 of the ’213 patent, which use explicit numbering and conditional phrasing to indicate step order, arguing that claim 16’s absence of such markers means its steps may be performed in any sequence. The Court disagreed, noting that the doctrine of claim differentiation prevents the Court from importing limitations of other claims into another.


Second, Sound View argued that implicit ordering requires a showing that the later step cannot be performed without first performing the earlier step. The Federal Circuit clarified that its case law requires no such finding of inoperability. Rather, the inquiry is whether “the steps of a method claim had to be performed in their written order because each subsequent step referenced something logically indicating the prior step had been performed.” *Altiris, Inc. v. Symantec Corp.* , 318 F.3d 1363, 1370 (Fed. Cir. 2003).


Finally, Sound View relied on Figure 7B of the ’213 patent, which the patentee cited as providing support for claim 16 during prosecution, to show that the steps could be performed in any order. But the Federal Circuit found the figure irrelevant because it skips over the relevant portion of the overall process that claim 16 could cover.


Accordingly, the Federal Circuit affirmed summary judgment of noninfringement.


## Takeaway


*Hulu II* highlights a potential risk of using past-participle modifiers for antecedent basis in method claims. The opinion also clarifies that implicit ordering does not require a showing of inoperability, which appears to lower the threshold. Therefore, method claims reciting language similar to that at issue in claim 16 may deserve early scrutiny in any infringement or validity analysis.


## Frequently asked questions


What did the Federal Circuit hold in Sound View Innovations v. Hulu (Hulu II)?


On January 29, 2026, the Federal Circuit issued a precedential decision affirming a grant of summary judgment of noninfringement in Sound View Innovations, LLC v. Hulu, LLC, No. 2024-1092 (Fed. Cir. Jan. 29, 2026). Circuit Judge Chen authored the opinion, joined by Circuit Judges Prost and Wallach. The court reversed the district court on its buffer construction but affirmed noninfringement on the ground that claim 16 requires its first two steps to be performed in the order written.


What was U.S. Patent No. 6,708,213 about in the Hulu case?


U.S. Patent No. 6,708,213, now expired, discloses methods for reducing network latency in streaming media systems by using intermediate "helper servers" to cache content, coordinate distribution, and adjust data transfer rates. By the time of the appeal in Hulu II, only independent claim 16 of the '213 patent remained at issue.


How did the Federal Circuit rule on the "buffer" claim construction?


The Federal Circuit reversed the district court's narrow buffer construction. The district court had construed the claimed "buffer" as a specialized buffer dedicated to a single streaming media object, which would have excluded Hulu's general-purpose buffers from infringement. The Federal Circuit disagreed, finding nothing in the claim language, specification, or prosecution history that narrowed "buffer" beyond its plain and ordinary meaning.


When do method claim steps have to be performed in the order written?


Method claims do not ordinarily require an order of steps unless the claim language, as a matter of logic or grammar, requires that the steps be performed in the order written, or the specification directly or implicitly requires an order of steps. In Hulu II, the Federal Circuit held that both grammar and logic compelled an implicit order because the past participle "requested" modifies "SM object," meaning a request must occur before the object can be described as "requested," and the second step expressly references "said requested SM object."


Does implicit step ordering require showing the later step is inoperable without the earlier one?


No. The Federal Circuit clarified in Hulu II that its case law requires no finding of inoperability for implicit ordering. The inquiry is whether the steps had to be performed in their written order because each subsequent step referenced something logically indicating the prior step had been performed. The opinion notes this appears to lower the threshold for finding an implicit order of steps.


What is the takeaway from Hulu II for patent drafting and litigation?


Hulu II highlights a potential risk of using past-participle modifiers for antecedent basis in method claims, since such language can create an implicit order-of-steps requirement. The opinion also clarifies that implicit ordering does not require a showing of inoperability, which appears to lower the threshold. As a result, method claims reciting language similar to claim 16 may deserve early scrutiny in any infringement or validity analysis.
