---
schema_version: "1.0.0"
document_id: "e1be8684290c2e0dab687470461f9856a2b2cfb9f4d7a6c0be741f3ff413684c"
company_key: "yc-substrate"
company: "Substrate"
source_id: "yc-substrate-news-import-ed25994f1cce"
canonical_url: "https://www.substrateai.com/blog/introducing-the-substrate-appeals-agent-for-medical-records"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-07-24T02:38:43.425400+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:7806540d14f65bac9fbd4ab32ae6bfe49cc3535d916f4cbdcebba48fff6c8232"
---

# Introducing the Substrate Appeals Agent for Medical Records

Many specialties frequently burn cycles submitting attachments to payers. Some of these include behavioral health, dermatology, orthopedics, cardiology, and rheumatology, among others. This process is onerous and time consuming, for several reasons:


- You often dont get notified that you need to submit records until after the claim is denied. If you’re lucky, you’ll know when it’s pended, but often you find out when you get an EOB.
- Requests for records often come in paper/lockbox/correspondence form, so there’s no electronic way to be notified.
- Its often not clear what specific documents you need to submit
- Getting the specific documents and preparing them for submission can be time consuming
- Even after doing all that, its often not clear if the documentation is sufficient.
- After you submit, you have to keep monitoring the claim to see if it gets paid or denied


### How it Works


The Substrate Appeals Agent brings together a lot of the infrastructure we’ve built to help resolve claims. Here’s how it works:


- It utilizes the[Substrate Claim Status Agent](https://www.substrateai.com/claim-status) to find out the latest status of a claim. This enables it to utilize EDI data, portal data, EOBs and more.


The Substrate Claim Status Agent performs the first step of AR Follow Up


- Analyzes and determines the right next course of action based on current context (claims data, denial codes, CARC and RARC codes) and broad context (other claims etc).


The Substrate Appeals Agent analyses the claim status response to see if medical records need to be submitted.


- Utilizes the policy agent to validate that the procedure would be considered medically necessary according to the payer’s policies
- Retrieves, parses and validates medical records autonomously, directly from your EHR or PM. For example, a medical record might include hundreds of pages spanning imaging reports, visit notes, referral documents, lab results and more - the Appeals Agent does all that by itself. Here's an example using a fictional patient Johnny Applesneed, who is visiting a fictional Dr. Who.


The Substrate Appeals Agent reviews the attached clinical notes to ensure they match what the payer has on file


- Validates other clinical documents according to your SOP. For instance, if you want to always submit prior authorizations with your attachments, the Substrate Appeals Agent will retrieve prior authorizations from your EMR, and validate them as well before submission. A human in the loop is only required when a field does not match.


The Substrate Appeals Agent also analyses any other clinical documents, per your SOP.


- Writes appeal letters that specifically reference and address context in the payer policy medical necessity directly


The Substrate Policy Agent evaluates the clinical documents against the payer medical necessity policy, and includes that evaluation in the appeal letter.


- Submits attachments and appeals to the payer portals by itself
- Post submission, it uses a variety of tools to monitor the status of the appeal - this includes the claim status agent, EOB retrieval agents and more.


Post appeal, the Substrate Appeals Agent monitors the appeal status until the claim is fully adjudicated.


All of this is easily auditable and customizable to fit the nuances and SOPs of each practice.


### Results


The Substrate Appeals Agent is currently live in Behavioral Health and Ortho across major payers like Aetna, UHC, and some of the Blues. Since launch, we’ve helped practices recover ~50% of dollars trapped in medical records appeals, completely hands free. For one early client, the Substrate Appeals Agent is responsible for over 2% of net collections, without requiring a human in the loop.


Sample recovery funnel from a real practice.


### What Comes Next


We’re keen to deploy this in more specialties and across more payers. In addition, we’re only just beginning to learn from payer decisions; a key improvement will be enabling the agent to learn from and incorporate payer responses and success rates.


Learn more[here](https://www.substrateai.com/claim-status) , or book a[demo](https://www.substrateai.com/demo) here!


‍
