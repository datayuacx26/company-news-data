---
schema_version: "1.0.0"
document_id: "fb757a86436af1070fa65b9931e677005325716d2670c258268f0b40f370175e"
company_key: "yc-stable"
company: "Stable"
source_id: "yc-stable-news-import-17708a39dff9"
canonical_url: "https://www.usestable.com/blog/remote-check-deposit-for-business"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T23:17:32.335056+00:00"
fetched_at: "2026-08-10T23:17:34.914439+00:00"
content_hash: "sha256:68c3ab098b0f0893b357f0b2b422e9f956b1ee3603b0e4e98370a0a7038ec103"
---

# Remote check deposits for business | Stable

**Remote deposit capture (RDC) lets businesses deposit checks without visiting a bank, usually by scanning them with a phone or desktop scanner. Stable goes a step further with two hands-off options — mail-in deposit and, for eligible customers, electronic deposit — so checks that arrive in your business mail get deposited without you scanning, signing, or driving anywhere.**


‍


As a small business owner, banking can waste a lot of valuable time. Between driving to bank branches, waiting in lines, and manually depositing paper checks, banking throws a wrench in your day-to-day operations — and business accounts usually come with higher volume and stricter reconciliation needs than a personal account.


‍


This guide focuses on what remote deposit capture means specifically for business owners: the real costs, the compliance responsibilities, and how Stable's approach removes most of the manual work. If you want the fundamentals of how RDC works in general — fraud detection, endorsement rules, typical processing times —[see our full explainer here](https://www.usestable.com/blog/remote-check-deposit) .


### Key takeaways


- Remote deposit capture (RDC) lets you deposit checks without a trip to the bank, typically by scanning them yourself with a desktop scanner or mobile app.
- Stable offers two hands-off alternatives instead: mail-in deposit and electronic deposit, with no scanning or equipment required on your end.
- Remote deposit capture can meaningfully cut a business's overhead, such as courier fees, staff time, and desktop scanner costs.
- Stable's electronic deposit works with Mercury and Brex, two banks that don't support standard mobile check deposit for business accounts (a common gap for startups.)


## What remote deposit capture means for a business, specifically


‍


Remote deposit capture (RDC) is banking technology that lets you deposit a check by scanning it and sending a digital image to your bank instead of visiting a branch.[Our full RDC explainer](https://www.usestable.com/blog/remote-check-deposit) covers how the underlying technology works, how banks screen for fraud, and typical processing times if you want the complete picture.


‍


For a business, the practical questions are usually different than they are for an individual depositing an occasional check:


- Who on the team is authorized to deposit?
- What happens when volume outpaces what a phone camera can handle?
- And who's responsible for the physical check once it's been scanned?


The rest of this guide focuses on those business-specific questions.


## Mobile vs. desktop RDC: What businesses actually pay for each


‍


If you're depositing checks through your own bank, you're choosing between two tools:


### Mobile business deposit (mRDC)


**Best for:** Businesses depositing a handful of checks, with no dedicated finance staff.


‍


**How it works:** Use your bank's mobile app to photograph the front and back of the check.


‍


**Pros:** Free or low-cost, no equipment required.


‍


**Cons:** Lower deposit limits, no multi-user workflow, and someone still has to be the one holding the check.


### Desktop scanner RDC


**Best for:** Businesses processing a high volume of checks that need higher deposit limits and accounting-software integration.


‍


**How it works:** A dedicated check scanner connects to a PC and feeds checks through your bank's business online banking software.


‍


**Pros:** Higher limits, batch processing, direct integration with accounting software.


‍


**Cons:** Hardware and monthly service fees (commonly $20–$50+), plus a commercial banking agreement, and someone still has to physically receive and feed the checks.


### The option most businesses don't know they have


Stable removes the equipment and the manual step entirely, with two paths depending on your setup:


- **Mail-in deposit:** we mail the physical check to your bank on your behalf via USPS Priority Mail, correctly endorsed and addressed. Available to nearly every customer.
- **Electronic deposit:** our mail facility deposits the check via mobile deposit into your Stable holding account. Funds typically settle within 5 business days (as fast as 1 business day on our Custom plan), then an ACH transfer to your bank is initiated (more on that below).


Electronic deposit eligibility depends on Stripe Treasury's requirements (a verifiable U.S. business address and an authorized representative); mail-in deposit remains the fallback either way. One thing worth knowing if you bank with a newer platform: Stable's electronic deposit works with Mercury and Brex, which don't support standard mobile check deposit for business accounts.


## Why remote deposit capture matters for businesses specifically


‍


Adopting remote deposit capture isn't just about skipping a trip. It also changes how a business handles cash flow, bookkeeping, and overhead.


### Cash flow and cutoff times


Bank cutoff times (often 2:00 PM or 3:00 PM) determine whether a deposit posts same-day or waits until the next business day. Remote deposit generally extends that window to 6:00 PM or 8:00 PM, depending on the bank —[see our RDC guide](https://www.usestable.com/blog/remote-check-deposit) for the full mechanics of how cutoff times work.


### Bookkeeping and integration


Manually driving to the bank and then updating your accounting software separately means duplicate work and more room for data-entry errors. Many banks' desktop RDC systems export check data directly into accounting software like QuickBooks or Xero as deposits are processed.


‍


Stable takes a similar approach from the mail side: every deposit is logged in your Stable Dashboard automatically, and Stable can be connected to the accounting systems you already use through our open API and webhooks. If you manage mail-heavy bookkeeping for a client or your own books,[our guide to virtual mailboxes for accounting](https://www.usestable.com/blog/virtual-mailbox-for-accounting-technology) goes deeper on that workflow.


### Overhead and hidden costs


Every bank run costs money beyond gas. Employee wages for the drive and the wait in line, mileage reimbursement, and courier fees if you're not sending an employee yourself.


‍


Remote deposit capture eliminates that overhead outright, and consolidating multiple checks into a single digital deposit ticket saves additional time on top of that.


## Security, compliance, and who's responsible for what


‍


Depositing checks remotely comes with real compliance responsibilities: specific endorsement language, retention periods, and secure destruction.[Our RDC guide](https://www.usestable.com/blog/remote-check-deposit) covers the general rules that apply no matter which bank or provider you use. Here's how the responsibility shifts when Stable handles it for you:


‍


Task If you do it yourself With Stable


Endorsement Sign the check and write "For mobile deposit only at \[Bank Name\]" before depositing. Our mail facility endorses using your account number — no signature required from you.


Custody Store the physical check for the bank's validation period (commonly 14–90 days) before you're allowed to destroy it. Checks stay at our facility the whole time. You never take possession of the physical check.


Destruction Securely shred the check yourself once it's cleared and validated. Secure shredding is built into your plan — request it anytime from the Dashboard.


Eligibility Varies by bank; most personal and business accounts qualify for mobile deposit. Checks must be payable to your business's verified legal name or DBA to qualify for electronic deposit; mail-in deposit is the fallback.


Holding period Varies by bank, typically a few days to a few weeks. Funds typically take 5 business days to settle in your Stable holding account for Grow and Scale customers (as fast as 1 business day on our Custom plan); an ACH transfer to your bank is initiated once funds settle.


‍


## How Stable deposits checks for you


‍


Stable gives your business a real mailing address and an online dashboard to manage physical mail from anywhere — including mailed checks. When a check arrives, we handle the deposit (mail-in or electronic) on your behalf: no scanner, no specialized equipment, and no trip to the bank.


- Multi-entity support: if you receive checks for more than one entity at the same address, you can set a separate deposit rule for each, and filter deposit activity by entity in your Finance dashboard.
- No limit on the number of bank accounts you can connect, so growing or multi-account businesses aren't boxed in.
- Beyond checks, the same platform handles state mail, legal notices, and compliance documents. If you also need a[registered agent](https://www.usestable.com/products/registered-agent) in any state, that runs through the same dashboard.


[Try Stable](https://dashboard.usestable.com/onboard/begin) and see how digital mail management — check deposits included — can simplify your small business. Check our[pricing](https://www.usestable.com/pricing) to find the plan that fits your check volume.


## Frequently asked questions


### How long does it take for a Stable check deposit to become available?


For electronic deposits, funds typically take 5 business days to settle in your Stable holding account for Grow and Scale customers (as fast as 1 business day on our Custom plan); an ACH transfer to your bank is initiated once funds settle. Mail-in deposits take about 2–3 days for the check to reach your bank via USPS Priority Mail, plus your bank's own processing time once it arrives.


### Can Stable deposit checks for multiple business entities?


Yes. If you receive mail for more than one entity at the same Stable address, you can configure a separate automatic-deposit rule for each one and filter deposit activity by entity in your Finance dashboard. Note that entity-level filtering applies to electronic deposits only — mail-in deposits aren't tied to a specific entity.


### Does Stable's electronic deposit work with Mercury or Brex?


Yes. Stable's electronic deposit works with Mercury and Brex. Those banks don't accept Stable's mail-in check deposits, but electronic deposits move funds via ACH rather than mailing a physical check, so they're unaffected by that restriction. This is one of the more common reasons startups use Stable's electronic deposit specifically.


### What are the standard daily deposit limits for business mobile banking?


Standard mobile deposit limits for business banking typically range from $5,000–$25,000, with higher limits available for enterprise accounts. Switching to a dedicated remote deposit setup, like Stable's, can raise your effective limit since deposits aren't capped by a single phone camera's mobile banking app.


### Why might a business mobile check deposit get rejected?


Rejections are usually caused by improper endorsement, a mismatched deposit amount, or poor photo quality.[Our RDC guide](https://www.usestable.com/blog/remote-check-deposit) covers this in more detail — with Stable, our team handles endorsement and image quality for you, which removes the most common causes of rejection.
