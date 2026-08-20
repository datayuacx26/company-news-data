---
schema_version: "1.0.0"
document_id: "c614e96caa5b1f915734c206ee548262debaf4742c52a8114a5853ef71cc41fa"
company_key: "yc-sphinx"
company: "Sphinx"
source_id: "yc-sphinx-news-import-f18a1b608f6d"
canonical_url: "https://sphinxhq.com/blog-posts/introducing-sphinx-doc-fraud-ai-forgery-detection"
published_at: "2026-06-25T21:55:15.601+00:00"
first_seen_at: "2026-07-27T05:21:33.517040+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:731a4afdb1181404f2af43651bebcf0ba3df32b0995c3a71d7a7a5f016b1f091"
---

# Introducing Sphinx Doc Fraud: AI Forgery Detection

Today we're launching Document Fraud Detection, built to catch the AI-generated and manipulated documents getting through onboarding at banks and fintechs.


Onboarding runs on documents. KYC, KYB, proof of income, source of funds: every check rests on files an applicant submits, and on a reviewer's ability to tell real from fake. Generative AI broke that. The forgeries that matter no longer look wrong; they look perfect. Here's what we built to catch them.


## How it works


Every document follows the same path. The moment it lands, Sphinx inspects it and scores how likely it is to be a forgery. That score decides what comes next: clean files clear in milliseconds, borderline ones draw closer scrutiny, and only the genuinely hard cases reach a person.


1. **Automated screening:** Every document runs through Sphinx's automated checks the moment it arrives. The obvious cases, clean files and forgeries alike, settle in milliseconds, before anyone has to touch them.
2. **Cross-signal analysis:** When a document isn't clear-cut, Sphinx weighs the evidence in context. Real files are messy: re-saved, reformatted, and re-exported by ordinary systems, each leaving harmless traces of its own. The task is separating that normal noise from a deliberate edit, and it's where tools built before generative AI tend to fail. No single oddity decides anything; a document is flagged only when the evidence lines up.
3. **Forensic review:** The genuinely hard cases reach a human analyst, who inspects the evidence Sphinx has already assembled and makes the call. Most documents never get this far, which keeps Sphinx fast across an entire portfolio while spending real effort only where it's warranted.
4. **Verdict:** Every document ends with a clear decision and the evidence behind it. Clean documents pass without friction; flagged ones arrive with the supporting evidence already attached, ready for a reviewer to act on.


## What Sphinx checks on every document


- **Edit history and production method:** how the file was actually made, whether generated in a browser, edited in Adobe, or run through a PDF tool. The story of a document's creation is usually the story of the fraud.
- **The full metadata trail:** creation vs. last-modified timestamps and editor fingerprints. A statement "issued" last week but first created two years ago tells you something.
- **Issuer-template matching:** for a Bank of America statement, the font, layout, and standard phrasing all have to line up.
- **Numeric consistency:** totals and figures that should reconcile, and don't.
- **Generative-AI artifacts:** the signatures left behind by AI image and text models.
- **Known-fraud patterns:** templates and forgeries recycled across cases, caught the moment they reappear.


*An altered Bank of America statement. The forged balances render in the wrong font, size, and baseline, a textbox pasted over the template, and Sphinx caught every one.*


## Results


After one customer replaced their previous document-fraud vendor with Sphinx here were the results they found on the same dataset of documents:


*Sphinx vs. a leading document-fraud tool on the same documents.*


- **Accuracy:** Sphinx delivers the correct verdict on 94.3% of documents, miles better than other vendors.
- **Fraud Caught:** Sphinx flags 84.4% of forgeries, more than three times what other vendors detect.


We've already processed over 1 million documents and we've caught thousands of real fraud cases.


## Built for scrutiny


A flag you can't explain is a liability. So every decision Sphinx makes is traceable. When a document is rejected, your team sees exactly what changed: the X-ray view sets the original and the altered version side by side and highlights the edit, so reviewers see the manipulation instead of taking the system's word for it.


When an examiner asks how a call was made six months ago, you can show the exact signals that triggered it, the document version that was reviewed, and the analyst who signed off.


## Why we built it


Before building this, we tried the tools already on the market. They were too shallow, drowned reviewers in false positives, and still missed the fraud that counted. Most were built on image-forensics techniques designed for a clumsy splice or a cloned logo, not for a document a model generated from scratch. So we built our own engine.


Deepfake incidents in fintech[jumped 700% in a single year](https://www.deloitte.com/us/en/insights/industry/financial-services/deepfake-banking-fraud-risk-on-the-rise.html) , and Deloitte projects that gen-AI fraud losses in the US could reach[$40 billion by 2027](https://www.deloitte.com/us/en/insights/industry/financial-services/deepfake-banking-fraud-risk-on-the-rise.html) , up from $12.3 billion in 2023. And the fraudsters aren't lone operators anymore. They're running coordinated teams of AI agents.


## See Sphinx Doc Fraud in action


Pointed at a live portfolio, Sphinx routinely finds fraud that was already approved and onboarded, some of it cleared months ago and sitting in your customer base right now.


The fastest way to see what's getting through is to run it on your own documents. It's live today and works on top of the systems you already use, with no migration and no lift on your side.


If you're a BSA Officer, Chief Compliance Officer, Head of Financial Crime, or running onboarding, we'll run Sphinx Doc Fraud against your real cases and show you the forgeries already sitting in your portfolio.


– Alex & Chris
