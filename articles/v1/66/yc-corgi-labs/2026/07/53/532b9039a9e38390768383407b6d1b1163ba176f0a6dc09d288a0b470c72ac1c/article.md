---
schema_version: "1.0.0"
document_id: "532b9039a9e38390768383407b6d1b1163ba176f0a6dc09d288a0b470c72ac1c"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/tc40-descriptor-mismatch"
published_at: "2026-07-10T21:52:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:082b619638664d959b3da35ea810fbf4a2ffc2b450a49d6755477bc62ed1bb3f"
---

# TC40 Descriptor Mismatch: Why Merchants Get VAMP Fines for Fraud They Didn't Process

## A Cloned Website Could Be Costing You $8 Per Transaction


Imagine opening your VAMP report and finding 500 TC40 fraud alerts you can't trace to any transaction in your system. At $8 each, that's $4,000 in monthly fines. The transactions are real, the fraud is real, but none of it happened on your platform.


This is the TC40 descriptor mismatch problem, and it's one of the most underreported risks in Visa's Acquirer Monitoring Program.


## What VAMP Changed About Fraud Fines


Visa's VAMP launched in April 2025, replacing the legacy Visa Fraud Monitoring Program (VFMP) and Visa Dispute Monitoring Program (VDMP). The threshold tightened to a **1.5% dispute ratio** effective April 1, 2026. Unlike its predecessors, VAMP assesses $8 per TC40 and $8 per chargeback for Excessive-tier merchants. If a TC40 later results in a chargeback (TC15), the same transaction can be fined twice, totaling $16.


There's no grace period for repeat violators. If you've been cited within the past 12 months, fines start from month one. First-time offenders get a three-month remediation window before fees kick in, but there's no early warning category, no 90-day runway to clean things up before you're classified.


For most merchants, the math is straightforward. Track your chargebacks, manage your fraud, stay under the threshold. But VAMP introduced a vulnerability that has nothing to do with the fraud you're processing, and everything to do with how TC40 reports identify merchants.


## How TC40 Reports Actually Identify You


When a cardholder reports a fraudulent transaction to their issuer, the issuer files a TC40 fraud report. That report doesn't reference your merchant identification number (MID). It references the **merchant descriptor** , the text string that appears on the cardholder's statement.


Karisse Hendrick, a fraud prevention expert and host of the[Fraudology podcast](https://fraudology-podcast.captivate.fm/) , described the problem plainly in a July 2026 episode: "The TC40 goes to the descriptor. It doesn't go to the MID. So if somebody clones your site and uses a descriptor that starts with your brand name, you get the TC40."


That distinction matters. Your MID is unique to your merchant account. Your descriptor is a text string that someone else can approximate, copy, or spoof.


A cloned website calling itself "NikeDeals" or "Nike-Outlet-Store" generates transactions under a descriptor that starts with "Nike." When cardholders file fraud reports on those transactions, the TC40s can be routed to the legitimate merchant whose descriptor matches, not to the fraudster's actual processing account.


## The Notification Gap That Shrinks Your Response Window


Even if you catch these misattributed TC40s, responding in time is harder than it should be.


Per Visa's VAMP rules, merchants have a 10-day window to dispute a TC40 after the acquirer is notified. That sounds reasonable. The problem is the gap between when your acquirer knows and when you know.


As Hendrick explained: "The acquirer gets notified, and then the acquirer notifies the merchant. That can typically take two to three business days. Now your 10-day window is seven days."


**Roughly seven calendar days to investigate fraud you didn't process.** That's the effective window most merchants are working with, though the exact lag varies by acquirer. For a payment ops team already managing legitimate disputes, adding misattributed TC40 investigations to a compressed timeline is a significant operational burden.


The practical sequence looks like this:


1.


A cloned site processes a fraudulent transaction using a descriptor similar to yours


2.


The cardholder reports it as fraud, triggering a TC40


3.


The TC40 is routed to your acquirer based on descriptor matching


4.


Your acquirer receives the TC40 and starts the 10-day clock


5.


Typically two to three business days later, your acquirer notifies you


6.


You have roughly seven days left to investigate and dispute a transaction you can't find in your records


Each step in that chain adds friction. None of it is within your control.


## Agentic Commerce Makes This Worse


Cloned merchant websites aren't new. What's new is how they reach consumers.


A June 2026 investigation by The Guardian found cloned versions of legitimate retailer websites appearing in ChatGPT search results. In one case, a fake version of Russell & Bromley, a UK shoe retailer, showed up when users searched for the brand through an AI assistant. The fake site used a similar domain and branding, making it difficult for even attentive shoppers to distinguish from the original. ChatGPT subsequently removed the fraudulent sites from its search index after the Guardian investigation, but the underlying technique remains available to fraudsters. (The specific Guardian-documented cases, including Russell & Bromley, were remediated after publication. The vulnerability class persists.)


**Now extend that scenario to AI shopping agents.**


Agentic commerce, where AI agents browse, compare, and purchase on behalf of consumers, is accelerating. These agents optimize for the lowest price and fastest delivery. They don't yet have a reliable mechanism to verify that a site is legitimate.


Hendrick put it directly: "AI agents are optimizing for the lowest price. They don't have a mechanism to verify that the site is legitimate."


A human shopper might notice a slightly off URL, a missing padlock icon, or an unusually low price. An AI agent running a purchase flow won't apply that judgment. It will find the cheapest listing, complete the transaction, and move on. If that transaction happens on a cloned site using your brand's descriptor, the resulting TC40 lands on your account.


While human consumers following AI-recommended cloned sites is well-documented, fully autonomous AI purchasing agents are still in early deployment. The risk is directional: as these agents scale, so does the exposure. The infrastructure for it already exists:


-


**Cloned sites have already been indexed** by AI search tools (and removed reactively, not proactively)


-


**AI purchasing agents** are in early deployment


-


**TC40 descriptor routing** doesn't distinguish between legitimate and cloned merchants


The combination creates a fraud vector that scales without the fraudster needing to do anything beyond maintaining a convincing clone.


## What This Costs at Scale


The financial impact of descriptor misattribution compounds quickly. VAMP's $8 per-TC40 fine is only the starting point. If a TC40 results in a chargeback (TC15), the combined fine can reach $16 per transaction.


Consider a hypothetical mid-market merchant processing $50 million annually. If, for example, cloned-site fraud generates 1,200 misattributed TC40s per month, the fine alone is **$9,600 monthly, or $115,200 annually** . That number doesn't include:


-


Chargeback fees on transactions that convert from TC40s (potentially doubling the per-transaction fine to $16)


-


Staff time spent investigating transactions that don't exist in your system


-


Representment costs for disputes you choose to fight


-


Revenue lost when your dispute ratio pushes you closer to the VAMP threshold


At higher volumes, the math gets worse:


Monthly Misattributed TC40s


Monthly VAMP Fine


Annual VAMP Fine


500


$4,000


$48,000


2,000


$16,000


$192,000


5,000


$40,000


$480,000


These are fines for fraud you didn't process, on transactions you never saw, from customers who never visited your site. For a payment ops team measured on dispute ratios and chargeback costs, misattributed TC40s are a budget line item with no corresponding revenue.


## Why Existing Tools Don't Catch This


Most fraud prevention and chargeback management tools focus on transactions you processed. They analyze your authorization data, score your orders, and help you fight chargebacks on transactions in your system.


Descriptor misattribution sits outside that frame entirely.


A competitive review of nine fraud and payments vendors (including Pagos, Signifyd, Checkout.com, Forter, Riskified, Sift, Sardine, Primer, and Spreedly) found **zero coverage of TC40 descriptor misattribution** or any form of merchant early-alert system specifically designed for VAMP disputes. This audit did not include dedicated chargeback management platforms such as Chargebacks911, Midigator, or CB-ALERT, which may have partial capabilities in this area.


That gap exists because the problem isn't about your fraud. It's about someone else's fraud being attributed to you based on a text match. Traditional fraud tools look inward at your transaction stream. This vulnerability requires looking outward at what's being reported under your descriptor.


## What You Can Do About It


While there's no single fix for descriptor misattribution, there are practical steps payment ops teams can take now:


1.


**Audit your descriptor regularly.** Know exactly what appears on cardholder statements. Search for variations, typos, and similar strings that a cloned site might use.


2.


**Monitor your TC40 reports at the descriptor level.** Don't just track totals. Look for TC40s that reference transaction amounts, dates, or patterns that don't match your processing records.


3.


**Establish a rapid dispute workflow.** With only seven effective days to respond, your process for flagging and disputing misattributed TC40s needs to be faster than your standard chargeback workflow.


4.


**Communicate proactively with your acquirer.** Ask about their notification timeline. If they're consistently taking three days to notify you, that's a conversation worth having. Some acquirers can set up faster alert routing.


5.


**Track brand impersonation.** Monitor for cloned sites using your brand name, especially those appearing in AI search results and shopping agent indexes.


These steps won't eliminate the risk, but they give you visibility into a problem that most merchants don't know they have until the fines arrive.


## Turning Payments Data Into Early Detection


The core challenge with descriptor misattribution is timing. By the time a TC40 shows up in your monthly report, the dispute window may already be closing. What merchants need is near-real-time visibility into what's being reported under their descriptor.


This is where payments analytics moves from reporting to prevention. Tools that surface TC40 activity as it happens, rather than in batch reports, give payment ops teams the window they need to investigate and dispute before fines lock in. Corgi Intelligence is built to dig into your payments data at this level of granularity, connecting transaction-level signals to dispute patterns so your team can act on misattributions before they become line items.


No tool can stop a fraudster from cloning your site. But the right analytics can tell you it's happening before the VAMP fines pile up.


If your team is managing VAMP compliance and wants earlier visibility into TC40 patterns, including descriptor-level monitoring,Book a Demo → to see how Corgi Intelligence surfaces these signals.


---


## Sources


-


Fraudology Podcast, Karisse Hendrick, "[The Agentic Era: Visa’s OpenAI Partnership, VAMP Pitfalls, and the Threat of Cloned Sites](https://fraudology-podcast.captivate.fm/episode/the-agentic-era-visas-openai-partnership-vamp-pitfalls-and-the-threat-of-cloned-sites) " July 7, 2026.


-


The Guardian, "[Cloned sites: the shopping scams that lead ChatGPT to fake stores](https://www.theguardian.com/money/2026/jun/07/ai-chatgpt-shopping-scams-fake-websites) ," June 7, 2026.


-


[Visa Core Rules and Visa Product and Service Rules, April 2026](https://usa.visa.com/dam/VCOM/download/about-visa/visa-rules-public.pdf) (VAMP amendments effective April 2026).
