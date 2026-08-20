---
schema_version: "1.0.0"
document_id: "77a57894c2b65acb22b035ff74f02638a16d474edbf8d29a8c5d3573e4943840"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/gdpr-feedback-personal-data"
published_at: null
first_seen_at: "2026-07-29T11:59:35.846279+00:00"
fetched_at: "2026-07-29T11:59:37.149226+00:00"
content_hash: "sha256:50942d6edca48963e9e50057c7f421cf1dd924dc4e66550f3334835a88eeaaaf"
---

# How Do You Analyze Customer Feedback Without Mishandling Personal Data Under GDPR?

Open-text feedback is the messiest personal data most companies hold. A customer typing into a survey box doesn't stay on topic. They give their name, their account number, the branch they visited, the agent who helped them, and sometimes a health condition or a complaint about a family member. Then that column gets exported and handed to an analytics vendor, and someone in legal asks whether that was allowed.


You analyze it lawfully by getting four things right, in this order: establish that you're the controller and your vendor is the processor, sign the contract that says so, mask personal data before analysis rather than after, and make sure retention and deletion actually execute. Thematic operates as a processor under the[General Data Protection Regulation](https://eur-lex.europa.eu/eli/reg/2016/679/oj) (GDPR). The customer decides what to upload, from whom, and why. Thematic processes it only on those instructions. Thematic can also mask emails, names, phone numbers, addresses, and identifiers before analysis begins.


One thing to settle first, because most guidance on this question gets it wrong: masking personal data doesn't take your feedback out of GDPR scope. Below is what redaction actually buys you, who owns which obligation, and the checklist to run before the next upload. This is an explanation of mechanics, not legal advice.


## Why redaction doesn't take feedback out of GDPR scope


Pseudonymized data is[still personal data](https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2025/guidelines-012025-pseudonymisation_en) under GDPR. European Data Protection Board guidance is explicit about it: pseudonymized data that could be attributed to a person using additional information remains information about an identifiable person. Article 32 names[pseudonymization as an appropriate safeguard](https://iapp.org/news/a/top-10-operational-impacts-of-the-gdpr-part-8-pseudonymization) , and a safeguard lowers your risk. It doesn't end your obligation.


Two consequences that catch teams out:


- **An opinion can be personal data on its own.** If a comment can be linked back to an individual, the content of that comment is personal data even after the name is stripped. This holds for customer feedback, employee surveys, and stakeholder research alike.
- **Redaction is best-effort, not a guarantee.** Any tool that masks free text works on pattern detection. It will miss things. A vendor who tells you their redaction is perfect is telling you they haven't tested it.


Redaction reduces how much personal data reaches the analysis, and it shrinks the blast radius if something goes wrong. It doesn't convert a regulated dataset into an unregulated one. It doesn't remove your duty to define a lawful basis, honor data subject rights, or delete data on schedule.


The practical consequence: treat redaction as one control among several, not as the compliance step.


## Who owns which obligation


Most confusion on this question comes from teams assuming the vendor carries duties that are legally theirs. Under GDPR, the company collecting the feedback is the controller and the analytics platform is the processor. The controller decides the why and the what. The processor acts on instructions.


Obligation Who owns it What that means in practice


Lawful basis for processing You, the controller Decide and document why you may analyze this feedback before you upload it


What data gets uploaded You, the controller The vendor can't decide your scope. If you upload a column you didn't need, that's your decision


Written processing contract Both, required of you Article 28 requires a written agreement. Get it signed before the first upload, not at renewal


Technical safeguards Your processor Encryption, access control, masking, isolation, and destruction to a stated standard


Answering a data subject request You, with processor support You respond. The vendor has to give you the means to find and delete the record


Retention and deletion Both You set the period. The vendor has to actually execute it, including in backups


Subprocessor changes Your processor notifies you You need to know who else touches the data, including translation services


## What to fix in the analysis pipeline


Four mechanics decide whether an analysis setup is defensible. None of them is exotic, and all four are usually skipped.


**Mask before analysis, not after.** Order matters more than teams expect. If personal data is stripped only at the reporting layer, it was already ingested, modeled, and stored in the clear. Redaction has to run in preprocessing, before the first analysis job, so the models never see the raw identifiers. Summaries and derived scores are built from the comment text, so anything that reads that text reads whatever you failed to mask.


**Only upload the columns you need.** The cheapest[privacy control](https://getthematic.com/insights/generative-ai-security-concerns) is not sending the data at all. Customer ID, email, and full name usually travel along out of habit rather than necessity. If you only need a pseudonymous key to join results back, send only that.


**Know where non-English text goes.** Multilingual feedback is often translated before analysis, which routes the text through a translation service. That service is a subprocessor in your processing chain, and your privacy reviewer will ask about it.


**Make deletion provable.** A retention policy that nobody executes is worse than no policy, because it documents an intention you're failing to meet. Ask for the destruction standard and the timeline in writing, and ask specifically what happens to backups, which are the usual exception.


## Where feedback analytics setups commonly go wrong


Most of these are configuration choices in a[feedback analytics](https://getthematic.com/insights/customer-feedback-analysis-with-text-analytics) rollout rather than anything a vendor decided for you.


The failures cluster:


- Redaction bolted on at the reporting layer, so raw personal data was ingested anyway.
- A retention period in the policy with no mechanism that deletes anything.
- Every available column uploaded because the export was easier that way.
- Translation treated as a feature rather than as a subprocessor relationship.
- Backups excluded from deletion commitments without anyone noticing.
- "We're GDPR certified" accepted as an answer. Nobody is GDPR certified. It's a regulation, not a certification.


One question to put to a vendor at demo time: **ask them to show you a comment before and after redaction, on your own data, and then ask what their redaction misses.** A vendor who names the failure modes has tested it. A vendor who says it catches everything hasn't.


## How Thematic handles personal data in feedback


**Thematic is a processor, and the split is explicit.** The customer solely determines what data to upload, from whom, and for what purpose. Thematic doesn't classify or represent customer data, and processes it only on the customer's instructions. Thematic signs a contract governing the processing of EU personal data. The customer keeps responsibility for data outside that scope, such as anything downloaded locally or printed.


**Redaction runs in preprocessing, before analysis.** Thematic masks email addresses, web addresses, unique identifiers, encoded data, physical addresses, names, and phone or number sequences, replacing each with a token such as` \[EMAIL\]` or` \[NAME\]` . It also strips quoted email history by detecting the "On ... wrote:" line and removing everything below it. Because summaries and synthetic scores are built from comment text, Thematic's guidance is to run redaction before the initial analysis job rather than after.


**Redaction is best-effort and priced as an add-on.** It isn't guaranteed to catch every instance, and it's configured through your customer success manager rather than switched on by default. That's worth knowing during evaluation rather than discovering later.


**Dataset-level modify and delete supports erasure.** Customers can modify and delete individual datasets, which is what makes an erasure request actionable against analyzed data.


**Retention and destruction are stated, including the backup exception.** Customer data is kept for the contract lifetime and deleted within 30 days of termination using NIST 800-88 sanitization. Backups are the exception and are purged on their own rotation. Personal data on a user account is kept for the account duration plus three months. All customer data is classified as Restricted, the most protective of the three tiers Thematic uses.


**Subprocessors and transport are governed.** Customers are notified of subprocessor list changes under a documented policy. Customer data moves only through approved channels, transfer by USB drive or email is prohibited, and access is from encrypted laptops only. Non-English feedback is translated into English via Google Translate so themes stay consistent across languages, with the untranslated source preserved alongside it.


Thematic's full posture is published on its[security and compliance](https://getthematic.com/product/security-and-compliance) page. For the certification and audit side of a vendor review, including[SOC 2 scope and penetration testing](https://getthematic.com/insights/vendor-security-certifications) , that's a separate question with a separate answer. So is whether the[EU AI Act's emotion-recognition ban](https://getthematic.com/insights/eu-ai-act-emotion-recognition-sentiment-analysis) touches sentiment analysis.


## A privacy checklist before you upload feedback


Ten questions, aimed at your own team as much as at the vendor. They sit alongside the wider[requirements worth putting in an analytics RFP](https://getthematic.com/insights/customer-feedback-analytics-rfp-requirements) .


1. Is the written processing agreement signed, and does it name the processing purpose?
2. Which columns are you actually sending, and can you drop any of them?
3. Does masking run before the first analysis job, or only at reporting?
4. What does the vendor's redaction miss, in their own words?
5. Can you delete a single individual's record, or only a whole dataset?
6. What's the retention period, and what mechanism enforces it?
7. What destruction standard applies at contract end, and what happens to backups?
8. Who are the subprocessors, including translation, and how are changes notified?
9. Where is the data hosted, and can you specify the region?
10. What's the transfer mechanism if data leaves the region?


## The short answer


Establish the controller and processor split in writing, mask personal data in preprocessing before the first analysis job, upload only the columns you need, and confirm that retention and deletion actually execute including in backups. Thematic operates as a GDPR processor. It masks personal data before analysis as a best-effort priced add-on. It supports erasure through dataset-level modify and delete. It destroys customer data within 30 days of contract end to the NIST 800-88 standard.


Do not accept redaction as your compliance story. It's a safeguard that lowers exposure, and the obligations sit with you either way.
