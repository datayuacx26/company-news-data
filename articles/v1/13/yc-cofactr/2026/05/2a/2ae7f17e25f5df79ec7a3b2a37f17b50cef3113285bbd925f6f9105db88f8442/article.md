---
schema_version: "1.0.0"
document_id: "2ae7f17e25f5df79ec7a3b2a37f17b50cef3113285bbd925f6f9105db88f8442"
company_key: "yc-cofactr"
company: "Cofactr"
source_id: "yc-cofactr-news-import-06c16db4e4eb"
canonical_url: "https://www.cofactr.com/articles/what-is-decapsulation-and-when-to-use-it"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-27T08:34:13.149498+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:e4a24fd59b27ed07043827deb0964ae6d94f5d47b35f0d4fa4a1a5c4e6576eff"
---

# What Is Decapsulation and When to Use It

## Introduction


That moment usually comes with a bit of anxiety. Decap gets mentioned, you don’t know exactly what it means, and the subtext is counterfeit risk or some kind of hidden defect.


Here’s the framing you need: decapsulation is not a routine inspection step. It’s a destructive, high-confidence method used by a lab when other approaches can’t answer the question.


By the end of this, you’ll know what decap actually is, what it’s used for, and when it makes sense to use it without overreacting or wasting parts.


## What Is Decapsulation (Decap)?


[Decapsulation](https://www.sensiblemicro.com/blog/electronic-component-testing-decapsulation-delidding) , or delid, is the process of removing the outer package of a semiconductor device so you can see what’s inside. That means exposing the die, the bond wires, and the internal structure that normally sits hidden under plastic or ceramic.


Once opened, you can directly inspect:


- The die surface and markings
- Bond wire connections and geometry
- Leadframe or internal construction


There are a few ways labs do this:


- Chemical methods, usually acid-based etching
- Mechanical removal or milling
- Plasma-based processes for more controlled exposure


No matter the method, one thing is consistent. The part does not survive. This is irreversible and requires specialized lab equipment and experienced operators.


So think of decap as the moment you stop inferring and start directly observing what’s inside the component.


## What Is Decap Used For?


### Two Primary Use Cases


Decap shows up in two main contexts:


- Failure analysis
- Counterfeit detection and mitigation


If you’re sourcing parts, the second one matters far more.


### Failure Analysis (Context Only)


Failure analysis teams use decap to figure out why a component failed. That could mean identifying a cracked bond wire, corrosion on a pad, or[damage from electrical overstress](https://landandmaritimeapps.dla.mil/programs/milspec/ListDocs.aspx?BasicDoc=MIL-STD-1580) .


This work is usually handled by semiconductor manufacturers or specialized labs. It’s relevant to you, but indirectly.


### Counterfeit Detection and Mitigation (Primary Use Case)


For sourcing, decap is a high-confidence inspection method used to confirm whether a part is actually what it claims to be.


It sits inside a broader physical analysis workflow defined by standards like MIL-STD-1580 or[counterfeit mitigation frameworks](https://www.sae.org/standards/as5553-counterfeit-electronic-parts-avoidance-detection-mitigation-disposition) such as AS6081.


### Key Distinction: “New” vs “Original”


This is a key concept most people aren’t clear about. If you’re concerned about counterfeiting you probably got the parts from a broker. The broker will quote, and you should designate on your purchase order, the parts are new and original.


“New” means unused. The part hasn’t been installed or stressed.


“Original” means it is the correct device from the correct manufacturer, and it matches what the markings claim.


Decap tells you about originality.


It can confirm:


- Die markings match the package markings
- The manufacturer is consistent with expectations
- Internal construction aligns with the intended device


It can expose:


- Remarked parts
- Substituted dies
- Relabeled components


What it cannot tell you is whether the part is new. It won’t reveal prior use, thermal cycling, or whether the part was pulled from scrap and cleaned up.


## When to Use Decap


Decap is the right move when you’re questioning originality. That could be because your newness inspection wasn’t definitive, or because you want a high-confidence answer before moving forward. In some cases, you just want to remove doubt and confirm the part is exactly what it claims to be.


### After Initial Inspection Has Been Completed


Start with newness inspection. Always.


External visual inspection (EVI) is used to determine whether a part is new, meaning unused and free from prior handling, installation, or rework. This step is faster, cheaper, and often decisive.


If newness is clearly compromised, you already have your answer. There’s no need to escalate.


Only after newness has been evaluated, and remains inconclusive or acceptable, does it make sense to ask the next question: should you confirm the part is original?


Decap comes after confirming newness, not before.


### When Non-Destructive Methods Are Not Enough


Decap destroys the device, so before decap you can try:


- X-ray
- Basic electrical testing


These methods can localize anomalies or raise suspicion, but they often stop short of fully[proving what’s inside](https://www.sciencedirect.com/science/article/pii/S0263224123016299) .


Decap is used when you need direct internal inspection to confirm identity.


### In High-Confidence or High-Risk Scenarios


Some situations demand a definitive answer:


- Supplier disputes
- Compliance requirements under standards like AS6081
- Safety-critical or high-reliability applications


In these cases, uncertainty costs more than the part you’re about to destroy.


### When Proper Resources Are Available


You need the right prerequisites:


- A qualified lab
- Multiple samples, if possible
- Ideally, a known-good comparison part (a golden sample)


## When NOT to Use Decap


Decap is powerful, but it’s easy to misuse.


### Before Initial Inspection Has Been Performed


If you haven’t done basic inspection, stop. You’re skipping steps and risking unnecessary destruction.


### When Newness Inspection Is Sufficient


If you already see clear signs of prior use, damage, or resurfacing, you can disposition the part without decap.


Decap won’t add value if the problem is already obvious.


### When You Only Have One Sample


Decap destroys the part. If you only have one unit, you’re eliminating your only piece of evidence. That can create problems in supplier disputes or further analysis.


[Standards and lab practices](https://www.iso.org/ISO-IEC-17025-testing-and-calibration-laboratories.html) emphasize preserving samples when evidence is limited or sensitive.


### When Lower-Risk Methods Can Answer the Question


If X-ray, electrical testing, or other non-destructive methods give you enough confidence, stop there.


Good analysis is about asking the smallest necessary question, not using the biggest tool available.


## What to Expect After Decap


### Lab Process Overview


The lab will expose the internal structure using chemical or plasma methods under controlled conditions. The goal is to reveal the die, bond wires, and key features without introducing artifacts.


### Internal Inspection and Analysis


Once exposed, the inspection focuses on:


- Die markings
- Manufacturer identifiers
- Construction details


These are compared against expected designs or known-good samples.


### Importance of a Known-Good Sample


If you can provide an authentic reference part, do it.


It allows direct comparison of:


- Die layout
- Markings
- Construction features


This dramatically improves confidence and reduces ambiguity in the results.


### Typical Lab Report Contents


Expect:


- High-resolution images of the die and internal features
- Documentation of observed markings
- Comparison against expected construction
- A conclusion about authenticity


### Possible Outcomes


You’ll usually get one of three answers:


- Confirmed original, internal structure matches expectations
- Suspect or counterfeit, mismatches are found
- Inconclusive, not enough reference data or conflicting evidence


## Key Takeaways


Decap is a method for exposing the internal structure of a component.


Its primary value in sourcing is verifying originality, not determining whether a part is new.


It sits in advanced counterfeit detection workflows and is typically used after non-destructive methods have been exhausted.


Use it deliberately. Start with the question you need answered, and only escalate when decap provides information you cannot get any other way.


**Ready to let Cofactr handle sourcing, negotiations, storage, kitting, and delivery while your team focuses on building products? It’s free to get started with Cofactr today.**


## Frequently Asked Questions


**What is decapsulation in semiconductor analysis?**


Decapsulation is a destructive lab process that removes a component’s outer package to expose the die, bond wires, and internal structure for direct visual inspection and verification.


**How is decapsulation performed on electronic components?**


Labs use chemical etching, mechanical milling, or plasma processes to remove packaging material. Each method aims to reveal internal features without introducing artifacts that could distort inspection results.


**Why does decapsulation destroy the component?**


The process physically removes protective packaging and exposes sensitive internal structures. Once opened, the device cannot function again, making decapsulation irreversible and unsuitable for reusable testing scenarios.


**What is decapsulation used for in sourcing?**


Decapsulation verifies originality by comparing internal die markings, construction, and manufacturer identifiers against expectations. It helps detect remarked, relabeled, or substituted components in counterfeit risk situations.


**Can decapsulation confirm if a part is new?**


Decapsulation cannot determine newness. It reveals internal structure and identity but does not show prior use, thermal stress, or whether a component was previously installed or refurbished.


**When should decapsulation be used in inspection workflows?**


Use decapsulation after external visual inspection and non-destructive tests when results remain inconclusive, especially in high-risk applications, compliance requirements, or supplier disputes requiring definitive internal verification.


**Is it necessary to have multiple samples before decapsulation?**


Multiple samples are recommended because the process destroys the part. Having extras preserves evidence for comparison, dispute resolution, and additional testing if results are unclear or contested.


**What results should you expect after a decapsulation analysis?**


Lab reports typically include high-resolution die images, documented markings, and construction comparisons. Outcomes usually classify parts as authentic, suspect or counterfeit, or inconclusive based on available reference data.
