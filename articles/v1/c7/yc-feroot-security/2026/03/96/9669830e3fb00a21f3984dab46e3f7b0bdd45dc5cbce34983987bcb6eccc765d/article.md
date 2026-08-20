---
schema_version: "1.0.0"
document_id: "9669830e3fb00a21f3984dab46e3f7b0bdd45dc5cbce34983987bcb6eccc765d"
company_key: "yc-feroot-security"
company: "Feroot Security"
source_id: "yc-feroot-security-news-import-5cc226b40d32"
canonical_url: "https://feroot.com/blog/saq-a-ep-run-time-script-monitoring/"
published_at: "2026-03-10T14:08:24+00:00"
first_seen_at: "2026-07-22T07:46:19.024825+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:fcbc1b715a5f0fb55ded9a8863bcdca6095b066ac17f8168eb0bcb972cc4f302"
---

# Why SAQ-A-EP Fails Without Run-Time Script Monitoring

In 2024, Recorded Future’s Fraud Intelligence Report found over 11,000 e-commerce domains actively running payment page skimmers, a nearly 300% increase from the year before. The majority of those merchants had no run-time monitoring in place.Most of them were processing payments through legitimate, PCI-certified processors. Some of them were almost certainly SAQ-A-EP merchants who believed their processor’s compliance covered their risk.


**It doesn’t. And PCI DSS 4.0.1 is now explicit about why.**


SAQ-A-EP merchants outsource payment processing but host the pages customers interact with before card data ever reaches the processor. For years, that distinction didn’t carry much compliance weight. If the processor handled the card data, the reasoning went, the processor’s PCI certification covered the exposure. Requirements 6.4.3 and 11.6.1 exist precisely because that reasoning was wrong, and the QSA community is enforcing them now.


**What you’ll learn:**


- Why hosting the page that leads customers to your payment processor puts you in scope for[6.4.3 and 11.6.1](https://www.feroot.com/blog/pci-dss-4-0-1-guide-to-requirements-6-4-3-11-6-1/) , regardless of whether you ever touch cardholder data
- Why your WAF, SIEM, and server-side monitoring don’t satisfy what PCI DSS 4.0.1 now requires of SAQ-A-EP merchants, and what that gap looks like when a QSA walks through it
- What continuous compliance evidence actually means in practice, and why manual audits can’t produce it no matter how thorough they are


## The SAQ-A-EP security gap


The embedded payment form creates a security dependency most merchants haven’t fully accounted for. Your processor renders a payment iframe. Your customer enters card data into it. The processor handles everything from that point. Within that model, the processor’s compliance really does cover the payment processing itself.


### What it doesn’t cover is your page.


Your checkout page is the environment the iframe loads into. Scripts running on that page execute in the same browser context as the payment form. A compromised script on your page can render a pixel-perfect overlay on top of the processor’s iframe before your customer types a single digit. It can capture keystrokes before they reach the processor.


It can silently redirect form submissions to an attacker-controlled server while passing the transaction through normally so nothing appears wrong to your customer or your ops team.


The processor’s PCI certification has no line of sight to any of this. That’s not a failure of the processor’s security. It’s a structural limit of what server-side compliance can see. And this gap existed in prior versions of PCI DSS because run-time monitoring wasn’t explicitly required. PCI DSS 4.0.1 closes it. The question is whether your compliance program has caught up.


## Why server-side monitoring isn’t enough


If you already have a WAF, a SIEM, or server-side monitoring in place, here’s the question that surfaces the gap: can your current stack tell you which scripts ran on your checkout page in the last 24 hours, and whether any of them were modified from their authorized versions?


If the honest answer is no, or if answering it requires a manual check, your monitoring doesn’t satisfy what 6.4.3 and 11.6.1 now require. That’s worth being direct about before we go further.


Server-side security monitors your infrastructure. It watches what your servers deliver. What it can’t see is what happens inside your customer’s browser after that delivery. Third-party scripts load directly from external CDNs and never pass through your servers.


Tag manager containers execute code that isn’t in your codebase and doesn’t appear in your logs. A malicious modification pushed to an analytics script hosted by a vendor upstream happens entirely outside your infrastructure, and your SIEM has no mechanism to detect it.


Server-side and run-time visibility are solving different problems. For most SAQ-A-EP merchants operating under PCI DSS 4.0.1, only one of them is actually in place. The requirements now demand both.


## How attackers exploit unmonitored SAQ-A-EP pages


Here’s where it gets specific, because the attack scenario that makes this gap most concrete is also the one most merchants find hardest to believe is possible.


### A formjacking attack targeting your checkout page works like this.


An attacker, either through automated scanning or by compromising a third-party script vendor whose library your page loads, injects a small JavaScript payload that activates specifically when a customer reaches the payment step. The payload renders a fake payment form as a transparent overlay positioned precisely over your processor’s legitimate iframe.


Your customer sees what looks like a normal checkout experience. They enter their card number, expiration date, and CVV. The payload captures that input and sends it to an attacker-controlled server. Then it removes itself from the DOM and passes the customer through to the real iframe, which processes the transaction normally.


The purchase goes through. The customer gets their confirmation email. You get your revenue notification. Your processor’s logs are clean. Your server logs show a normal transaction. Nobody notices anything until stolen card data surfaces on a dark web marketplace and gets traced back to your checkout page, weeks later.


### The attack succeeded entirely because your page had no visibility into what scripts were running.


That’s the condition every other browser-based attack vector shares. Keyloggers injected to capture input before form submission work on the same principle.


So do malicious modifications pushed through compromised tag manager containers, where an attacker who gains access to your GTM account can deploy harvesting code to your checkout page without touching your codebase. Analytics or chat scripts weaponized through supply chain compromise follow the same exploitable logic.


The common thread isn’t the technique. It’s the assumption that outsourced payment processing means the risk lives somewhere else.


It doesn’t. It lives on your page.


## What PCI DSS 4.0.1 changed for SAQ-A-EP


Both requirements became mandatory for SAQ-A-EP merchants after March 31, 2025, and both address the specific gap described above.


**Requirement 6.4.3** mandates that you maintain a complete inventory of all scripts loaded and executed in the consumer’s browser on payment-affecting pages, with a documented authorization for each script and a mechanism to verify that none of them have been modified from their authorized versions.


The scope is explicitly browser-based: scripts running in the browser on pages that can affect payment security, regardless of whether your server ever touches cardholder data.


**Requirement 11.6.1** requires a change and tamper-detection mechanism that alerts you to unauthorized modifications of HTTP headers and payment page content, evaluated at least weekly.


The “continuous” framing is deliberate.[A QSA isn’t looking for evidence that you checked once before the assessment.](https://www.feroot.com/blog/what-qsas-expect-to-see-for-pci-dss-6-4-3-and-11-6-1-compliance/) They’re looking for evidence that monitoring was running between assessments and that your team was being notified when something changed.


**Here’s the distinction worth making precisely: 6.4.3 is a compliance requirement, but it’s also the control that would have stopped the attack described above. 11.6.1 is a compliance requirement, but it’s also the mechanism that would have caught the modification before weeks of cardholder data had been exfiltrated. Compliance and security aren’t the same thing, but in this case they’re pointing at exactly the same gap.**


## Evidence QSAs expect for SAQ-A-EP script monitoring


Most compliance content stops at “here’s what the requirement says.” The more useful question is what passing actually looks like, because that’s what’s forming in your mind right now.


A QSA examining 6.4.3 will want a complete inventory of all scripts present on your payment-affecting pages, with each one identified by source and purpose and a named owner who authorized its presence. They’ll want integrity verification showing scripts haven’t been modified from their authorized versions. Not a one-time check. A demonstrated method that’s running continuously.


For 11.6.1, they’ll want change detection logs showing that monitoring was active between your last assessment and this one, with alert history showing how your team responded when a modification was detected. If no alerts have ever fired, that’s not evidence of a clean environment. It’s evidence that monitoring may not be configured to catch changes at all.


Here’s the part worth being direct about:[without run-time monitoring tooling, producing this evidence requires manual audits.](https://www.feroot.com/blog/how-to-prove-pci-dss-6-4-3-11-6-1-compliance-qsa/) And a manual audit conducted quarterly proves you checked once. It doesn’t prove your checkout page was clean between checks. That distinction matters to a QSA now in a way it didn’t under earlier versions of the standard, because continuous evidence is explicitly what the requirement calls for.


## How PaymentGuard closes the SAQ-A-EP gap


You understand the problem precisely at this point. What you need is a direct mapping from the compliance requirement to what closes it.


### Requirement 6.4.3.


PaymentGuard continuously discovers and maintains an automated inventory of all scripts on your checkout and payment-affecting pages without requiring manual updates from your team. The authorization workflow produces documented justification records per script, with named approvals, satisfying the authorization mandate. Real-time integrity monitoring detects modifications against authorized baselines and flags deviations before they become assessment findings.


### Requirement 11.6.1.


PaymentGuard’s change detection layer provides immediate alerting when unauthorized modifications are made to scripts or HTTP headers on your payment pages. The monitoring runs continuously between assessments, not only before them, which is what produces the evidence of ongoing compliance a QSA now expects.


Audit-ready evidence exports, formatted specifically for QSA documentation requirements under both requirements, replace the manual audit burden with a structured, reviewable evidence trail.


The precision of that mapping matters. You’re not adding a feature to your stack. You’re closing documented compliance gaps that have a specific shape, and the question worth sitting with is whether your current setup can produce that evidence trail or whether it can’t.


## The checkout page is in scope. The evidence has to be there.


SAQ-A-EP without run-time script monitoring was always a security gap. PCI DSS 4.0.1 made it a compliance gap as well. That’s not a subtle distinction. It means the merchant who believed their processor’s PCI certification covered their checkout page is now facing a QSA asking for documentation they can’t produce, for a window of exposure they couldn’t see.


Compliance doesn’t make you secure. But the controls 6.4.3 and 11.6.1 require are the same controls that would have made those 11,000 compromised domains visible to the merchants running them. That’s not a coincidence. It’s what good compliance requirements look like when they’re written around the actual attack surface.


If you can’t currently answer the question “which scripts ran on our checkout page in the last 24 hours, and were any of them modified?”, that’s the gap to close first. Assess your run-time visibility against what 6.4.3 and 11.6.1 now require, and[see how PaymentGuard closes the distance between the monitoring you have and the evidence your QSA will ask for](https://www.feroot.com/demo-paymentguard-ai/) .
