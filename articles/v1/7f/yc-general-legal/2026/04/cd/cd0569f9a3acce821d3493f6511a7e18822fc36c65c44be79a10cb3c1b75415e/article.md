---
schema_version: "1.0.0"
document_id: "cd0569f9a3acce821d3493f6511a7e18822fc36c65c44be79a10cb3c1b75415e"
company_key: "yc-general-legal"
company: "General Legal"
source_id: "yc-general-legal-news-import-cc42147fa719"
canonical_url: "https://general.legal/blog/gdpr-vendor-international-data-transfers"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-29T04:44:54.246082+00:00"
fetched_at: "2026-07-29T04:44:55.619052+00:00"
content_hash: "sha256:996fc251259af38421662fa4783743d6cc9f66f9e5db00aa524444c98507b396"
---

# A GDPR-Compliant Vendor Doesn't Automatically Mean Your Data Transfer Is Legal

We keep running into the same mix-up with startup clients: a vendor has a DPA, calls itself GDPR compliant, and everyone assumes the data transfer to the US is therefore covered too. It isn't - those are two separate legal questions.


A Data Processing Agreement covers how a vendor handles your data - security measures, sub-processors, what happens if there's a breach. It says nothing about whether you're legally allowed to send that data to the US in the first place. That's a separate legal basis under GDPR, and you need one for every transfer, every vendor.


## The ARC Test


Every time personal data leaves the EU, check these three things in order: **Adequacy, Registration, Clauses.**


### A - Adequacy: Does the Destination Country Have an Adequacy Decision?


The EU Commission has already decided some countries offer adequate protection - Switzerland, Japan, South Korea, and a few others. If yes, you're done; no further paperwork.


The US doesn't have a general adequacy decision.


### R - Registration: Is Your Specific Vendor Certified Under the EU-US Data Privacy Framework?


This is the one people get wrong most often. The DPF is not a blanket US adequacy decision. It's a self-certification scheme - individual companies opt in and get listed on a public register. If your vendor is actually on that register, the transfer is covered. If they're not, the DPF does nothing for you, regardless of what their compliance page says.


### C - Clauses: Do You Need Standard Contractual Clauses?


If neither of the above applies, you need Standard Contractual Clauses - and one more step most people skip.


SCCs alone aren't enough. Since the 2020 Schrems II decision, you're also required to do a Transfer Impact Assessment: checking whether the destination country's surveillance laws could undermine what the SCCs promise on paper. A lot of companies sign the SCCs and never do this part, even though it's been a requirement since 2020 - years before this year's DPF news.


## The R in ARC Has a Shaky History


The DPF has a history of not lasting. Its two predecessors, Safe Harbor and Privacy Shield, were both struck down by the **CJEU** , in 2015 and 2020.


In June 2026, the US Supreme Court ruled in Trump v. Slaughter that federal agencies like the **FTC** have less protection from presidential removal than before. Max Schrems and noyb have already written to the European Commission arguing this undermines one of the **DPF** 's core requirements - independent oversight of EU data. People are calling the expected case "Schrems III."


The DPF hasn't been struck down. If it's your only transfer mechanism for a given vendor though, worth knowing that letter of ARC has failed twice before, and there's already movement toward a third round.


## What Should Companies Do in Practice?


If you're covered by adequacy, there's nothing to do. If you're relying on clauses, check whether anyone actually did the Transfer Impact Assessment - if not, that's a gap sitting in your files right now, not a hypothetical one. If you're relying on DPF registration, check the vendor is actually on the current register, and don't assume that status is permanent for the life of the contract.


Run the ARC test on your vendor list and you'll usually find you're not sitting on just one letter - most companies have a mix, vendor by vendor.


## Get Help Reviewing Your International Data Transfers


Happy to help if you want a Transfer Impact Assessment done properly, or just want a second pair of eyes on where your vendors land on ARC.
