---
schema_version: "1.0.0"
document_id: "4a3b85f3ae363ddc6c0799298afd2da4ebea1d59e3ad2629a491cf96b6e2021a"
company_key: "yc-zeal"
company: "Zeal"
source_id: "yc-zeal-news-import-baea200ed606"
canonical_url: "https://www.zeal.com/blog/how-we-rebuilt-i-9-for-the-way-staffing-really-works"
published_at: "2025-11-20T20:57:36.470+00:00"
first_seen_at: "2026-07-22T20:56:59.281452+00:00"
fetched_at: "2026-07-28T22:25:10.100738+00:00"
content_hash: "sha256:a1151408a3310287aa248a49ec732693027e5a27df57d874aac17b9039f30fd4"
---

# How We Rebuilt I-9 for the Way Staffing Really Works

After delivering modern payroll for staffing companies for years, we noticed something: onboarding was breaking down at the I‑9. And the way staffing companies were handling employment verification was outdated, similar to payroll.


It didn’t matter how fast payroll ran or how seamless billing was. If workers couldn’t complete their I‑9 quickly, they didn’t start their shifts. Admins were cycling between manual processes and managing exceptions. Staffing companies were losing trust and revenue.


We realized I‑9 isn’t just a compliance task. It is a critical part of staffing operations and the worker experience. It is a key moment that shaped everything else that followed for workers. So we decided to build a better solution.


## **Why I‑9?**


For most staffing platforms, the I‑9 is a routine onboarding process until it becomes a problem. It is the largest bottleneck in the onboarding process. Workers are ready, jobs are open, money is on the table, and the only thing standing in the way is a compliance‑heavy government form built for an office in 1986.


Onboarding slowdowns creates missed shifts and big impacts for staffing companies:


- Lost revenue
- Upset workers
- Frustrated clients
- Operational overhead


The biggest risk is talent drop‑off. Staffing platforms spend real money to attract workers through advertising, referrals, and text campaigns, only to lose them in broken onboarding flows. A confusing or delayed I‑9 experience is often the first and last impression a worker has of a platform. If it feels complicated or unreliable, they leave.


While I‑9 compliance is a legal requirement, it can also be a key step that shapes trust, speed, and profitability across the entire staffing funnel.


## **The Hypothesis**


When Zeal began building its I‑9 product, the goal was to deliver something better than what was on the market.


Some companies already use vendors built solely for handling I‑9 employment verification. Zeal saw an opportunity to provide a better version: one that was modern, cost‑effective, and designed for how staffing companies actually operate.


The hypothesis centered on three ideas:


- **I‑9 is simple to adopt** Switching payroll systems can feel overwhelming because it touches every part of a business. Replacing an outdated I‑9 flow, however, is more straightforward. For many teams, it is as simple as updating the onboarding link. That simplicity makes I‑9 a smart starting point for companies that want immediate improvement without the commitment of a full system overhaul.


- **Modern staffing platforms want embedded workflows.** Most digitally-native staffing companies want to control the candidate experience from start to finish. Redirecting workers to third‑party portals breaks that flow, slows completion, and erodes trust. Zeal’s I‑9 lives inside the same environment as onboarding and payroll so the process feels seamless for both admins and workers.


- **A stronger starting point unlocks more growth.** By helping staffing companies improve one critical bottleneck, staffing companies can get more workers working, more quickly. This allows them to have happier workers, clients, and teams. They can avoid costly compliance violations.


## **Building the MVP**


To launch quickly, we focused on one goal: exceed what customers were getting from existing I-9 products in the market. That meant:


- Completing the full I‑9 digitally, including Section 1, Section 2, and document uploads.


- Supporting remote verification through the Authorized Representative model.


- Submitting data to E‑Verify and receiving verification results.


- Branding everything in a White-Label UI to eliminate third-party handoffs.


Customer demand accelerated our timeline—Wonolo was ready to go live in a month. To deliver, we split development: one engineer owned the backend, another tackled the UI, and we shipped in stages. The first release allowed users to fill out the I‑9 and complete remote verification. While Wonolo tested the worker and authorized representative experience, we finished the E-Verify integration.


## **Lessons from Real-World Usage**


The early feedback from our customers shaped the next iteration of our product:


- **Admin Review** : Teams needed a way to review submissions before sending them to E‑Verify—catching typos, expired IDs, or mismatched documents before they became legal issues.


- **AI-Powered Guidance** : We saw common patterns in errors, especially among non-native English speakers or workers rushing to start a job. We implemented AI to flag mismatches between selected status and uploaded documents. For example identifying that someone using a green card instead uploads a foreign passport.


- **Dynamic Document Collection** : Instead of asking users to understand List A/B/C rules, we prompt them to upload a document, then intelligently guide them based on what’s still needed, reducing drop-off and increasing accuracy.
‍


These changes had a real impact: average I‑9 completion time dropped from 2–3 days to just over 2 hours.


## **E‑Verify, Built-In and Backed by Humans**


To make I-9’s as effortless as possible, Zeal manages the full E‑Verify process from start to finish. That includes submitting data through the USCIS API, performing identity crosschecks, and ensuring compliance with government requirements.


We don’t just rely on automation. Zeal includes a layer of human oversight. Our internal support team reviews critical checkpoints like passport image comparisons when required by E‑Verify. Instead of asking your team to handle these edge cases, our team of compliance experts does it for you.


This human review helps catch mismatches, reduce errors, and prevent rework. It expedites fixes and gives your admins peace of mind that someone is watching out for accuracy and compliance behind the scenes.


Compliance should not feel like a guessing game. With Zeal, it’s both automated and accountable.


## **What’s Next**


At Zeal, we will continue to improve I‑9 employment verification with AI and empathy.


We’re using machine learning to detect when workers are stuck, and stepping in with just-in-time support. Our goal is to reduce friction while maintaining compliance and speed.


Because for staffing companies, every minute matters. Every document matters. Every impression matters.


And onboarding? That’s the first real moment of trust.
