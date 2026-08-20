---
schema_version: "1.0.0"
document_id: "9ce22fc21f653560f01e323917eefbdaace0da773c02c37328fe219e04449a78"
company_key: "yc-talkable"
company: "Talkable"
source_id: "yc-talkable-news-import-628a4b01a13c"
canonical_url: "https://www.talkable.com/blog/how-to-use-klaviyo-talkable-segments-to-improve-bfcm-referral-results/"
published_at: "2026-07-30T20:56:33+00:00"
first_seen_at: "2026-07-31T21:56:56.949151+00:00"
fetched_at: "2026-07-31T21:56:58.696183+00:00"
content_hash: "sha256:473f2cea112878bb245bdd4279d927c15c9680df9474dc03d2e9947a0ecc5af6"
---

# How to Use Klaviyo + Talkable Segments to Improve BFCM Referral Results

[All Posts](https://www.talkable.com/blog/) >>


How to Use Klaviyo + Talkable Segments to Improve BFCM Referral Results


Email Marketing


Referral Marketing


# How to Use Klaviyo + Talkable Segments to Improve BFCM Referral Results


**Jeremy Foreshew**


Head of Marketing & Partnerships


**Jeremy Foreshew**


Head of Marketing & Partnerships


1 day ago


6


min read


Klaviyo × Talkable | BFCM field guide


BFCM creates buying intent.
Referral gives it a second path.


The sale is an opportunity to give your strongest customers a clear reason to bring a friend—and to use referral behavior to decide who needs a nudge, a thank-you, or a different follow-up. This guide explains how Talkable and Klaviyo work together to turn referral behavior into timely messages, cleaner audiences, and more qualified acquisition through Black Friday and Cyber Monday.


BFCM PLAYBOOK


## The BFCM referral premise


Referral should not be treated as one blast. It is a lifecycle sequence: activate advocates, confirm shares, welcome referred customers, and make earned rewards easy to use. During peak shopping, a friend benefit gives customers an immediate reason to share rather than another generic sale email.


The referral flywheel


**01**
Activate


Give advocates a reason to share


**02**
Confirm


Meet action with fast reassurance


**03**
Convert


Welcome friends, issue rewards


- **Protect margin:** Use a deliberate referral offer alongside the sitewide promotion. Set clear rules so discounts do not accidentally stack.
- **Use the moment:** Make the friend value, customer reward, and deadline obvious.
- **Follow behavior:** Non-sharers, active sharers, top advocates, and referred customers should not receive the same message.
- **Build next-quarter value:** Referred customers and proven advocates become useful retention audiences after the weekend.


BFCM PLAYBOOK


## How the Talkable + Klaviyo integration works


Talkable writes referral context into Klaviyo in real time. When an advocate opts in—or a friend passes email gating and opts in—Talkable can sync the profile. Enabled actions also send referral metrics that can trigger Klaviyo flows. Install through the Talkable App Store, authorize Klaviyo, enable the integration, and send a sample payload before turning on live flows.


### Core Klaviyo metrics


- **Talkable Email Opted In**
- **Talkable Offer Shared**
- **Talkable Referral Approved**
- **Talkable Reward Issued**


The integration can also support phone opt-ins and loyalty events when those actions are enabled. Metric-triggered flow emails can use event properties. For campaigns or flows triggered by profile changes, use durable` talkable_*` profile properties instead.


BFCM PLAYBOOK


## The four starter segments—and one important guardrail


Talkable creates four ready-made audiences that make a practical starting point for BFCM planning:


1. **Opted In Never Shared:**` talkable_advocate = true` and` talkable_shares_count = 0` . Use for a clean activation nudge with one easy share CTA.
2. **High-Value Advocates:**` talkable_referral_count_approved ≥ 3` . Use for early access, appreciation, or a controlled boosted reward.
3. **Referred Customers:**` talkable_referred_by_email is set` . Use for a referral-aware welcome and post-purchase journey; do not treat these people as cold prospects.
4. **Lapsed Advocates:** advocates who have shared before but whose latest offer is 30+ days old. Use a fresh holiday prompt, not a copy-paste resend.


**Guardrail:** Keep these starter segments intact. Talkable recognizes them by name on activation; you can edit conditions in place, but renaming or deleting a segment may cause Talkable to recreate the original the next time the app is activated. Segment creation requires Klaviyo’s` segments:read` and` segments:write` OAuth scopes.


BFCM PLAYBOOK


## BFCM audience plan: add commerce intent to referral behavior


- **Ready advocates:** High-Value Advocates AND engaged email audience. Send early access plus a referral share invitation; exclude recent unsubscribes and reward-risk profiles.
- **Unactivated advocates:** Opted In Never Shared AND engaged in the last 90–180 days. Send a single value proposition and one share CTA; frequency-cap against sale campaigns.
- **Lapsed advocates:** Lapsed Advocates AND recent site or email engagement. Give them a new holiday angle; exclude anyone who has already shared this campaign.
- **Referred customers:** Referred Customers with no purchase since referral, or a first purchase during BFCM. Clarify the friend offer, welcome them, and introduce advocacy only when they have their own offer.


Choose engagement windows according to account deliverability health and consent status. The goal is not a larger blast; it is a more relevant one.


BFCM PLAYBOOK


## Flow 1: activate the advocate


Use one BFCM ask and one obvious way to act. Target customers most likely to share, then make the friend value, customer reward, and deadline painfully clear.


1. **Early access:** Give high-value advocates the holiday referral message before general sale noise.
2. **Black Friday nudge:** Send opted-in non-sharers a focused share request—not a giant product catalog.
3. **Cyber Monday angle:** Use a fresh subject line and reason to share. Stop when the customer shares.
4. **Last-call logic:** Reserve this for engaged non-sharers only and respect the overall campaign frequency cap.


Implement campaign sends to segments with a flow filter that exits anyone once *Talkable Offer Shared* occurs. Do not keep asking after a customer has shared.


BFCM PLAYBOOK


## Flow 2: turn action into confidence


Use Talkable metrics for messages that are actually timely:


- **Offer Shared:** Confirm the share and briefly explain what happens when a friend qualifies. Personalize with share channel, campaign name, or referral counts. Do not resend the generic share request.
- **Referral Approved:** Celebrate the qualified referral and tee up the reward expectation. Use approved referral count and advocate identity on the friend profile where appropriate. Keep it transactional and fast.
- **Reward Issued:** State the value, expiry, and exactly how to use it. Reward details can be passed as custom event properties; give redemption its own CTA.


BFCM PLAYBOOK


## Flow 3: the referred customer journey


A referred friend is not just a new email address. Their profile can carry the referring advocate’s name and email. Use that context only where it makes the message more useful and aligns with privacy standards.


1. **Friend acquisition:** Make the referral offer and sale terms easy to understand, with the goal of converting during the BFCM window.
2. **First purchase:** Confirm the purchase and keep transactional promises clear.
3. **New advocate:** After they receive their own offer, invite the next share at the right moment.
4. **Retention:** Keep referred customers as a distinct cohort for reporting and later lifecycle work.


Do not mix acquisition, reward, and advocate asks into one dense email. Talkable sets` talkable_referred_by_email` and referring-name properties on referred-customer profiles after referral approval.


BFCM PLAYBOOK


## Work backward from the BFCM weekend


- **October — Foundation:** Install or reauthorize the app, validate scopes, send sample payloads, build segments, and warm engaged audiences.
- **Early November — Preview:** Activate high-value advocates; finalize offer terms, frequency caps, and customer-support macros.
- **November 27 — Black Friday:** Lead with the sale and a clean referral reason; trigger share confirmation in real time.
- **November 30 — Cyber Monday:** Use a new angle for engaged non-sharers and keep the referral path fast on mobile.
- **December 1+ — Close the loop:** Deliver rewards, welcome referred customers, report by segment, and retain the best flows.


BFCM PLAYBOOK


## Measurement and BFCM tests


Report referral as a funnel, then compare cohorts:


- **Activation:** segment delivered, click rate, and share rate.
- **Conversion:** friend visits, qualified referral approvals, and friend conversion.
- **Economics:** reward cost, referral sales, fraud prevented, and ROI.
- **Quality:** referred-customer repeat purchase and unsubscribe rate.


Two useful tests: first, compare high-value advocates with engaged never-shared advocates using the same offer and send window—test audience before offer. Second, compare a share-confirmation flow with a generic post-sale reminder—test moment before frequency. Keep a holdout only when volume supports it. Klaviyo metric analytics can filter and group custom payload attributes such as campaign name or share channel.


BFCM PLAYBOOK


## Launch checklist


- Confirm consent and opt-in paths for advocates and friends.
- Reauthorize the app if it predates segment creation or lacks required scopes.
- Test every enabled Talkable action with a sample payload.
- Verify metric-triggered flows, exit criteria, UTM links, and mobile rendering.
- Document offer terms, reward timing, stackability, and customer-support escalation.
- Set send-frequency caps across sale and referral campaigns.


Creative reference gallery


## 10 fictional BFCM campaign concepts


Illustrative creative directions only. Each campaign is intentionally built as a distinct email world, so the flow logic is easier to scan and the examples do not blur together.


FLOW 01


VIP early access


Activate top advocates before general sale noise.


FLOW 02


New advocate share


A simple give/get proposition with one clear CTA.


FLOW 03


Offer shared confirmation


Confirm the action and explain what comes next.


FLOW 04


Referral approved


Celebrate the qualified referral and reward expectation.


FLOW 05


Reward issued


Make reward value, expiry, and redemption unmistakable.


FLOW 06


Referred friend welcome


Clarify the friend offer before introducing advocacy.


FLOW 07


Cyber Monday share


Use a fresh angle for engaged non-sharers.


FLOW 08


Lapsed advocate


Re-engage with a holiday-specific prompt.


FLOW 09


New buyer share


Invite advocacy after a first purchase.


FLOW 10


Last chance


Reserve urgency for engaged audiences that have not shared.


BFCM PLAYBOOK


## Sources


- [Talkable Documentation: Klaviyo integration](https://docs.talkable.com/email_marketing_and_automation/klaviyo/)
- [Klaviyo App Marketplace: Talkable](https://marketplace.klaviyo.com/en-us/apps/01K5Y7ZHEZFQTNF4SHY6V6JVN0/)
- [Klaviyo: 2026 BFCM & Holiday Marketing Strategy Guide](https://www.klaviyo.com/bfcm/marketing-strategy)
- [Klaviyo: Email marketing best practices](https://www.klaviyo.com/products/email-marketing/best-practices)


**Bottom line:** Start with the advocate segment that can move this BFCM. Pick offer rules, build the four starter segments, and ship the first three referral flows before sale traffic hits.


## Expert Insights, Referral Trends & Growth Strategies


## Popular Posts


Referral Marketing


[BFCM 2026: The Referral Strategies That Will Actually Drive Growth](https://www.talkable.com/blog/bfcm-2026-the-referral-strategies-that-will-actually-drive-growth/)


Jeremy Foreshew


Email Marketing


Referral Marketing


[How to Use Klaviyo + Talkable Segments to Improve BFCM Referral Results](https://www.talkable.com/blog/how-to-use-klaviyo-talkable-segments-to-improve-bfcm-referral-results/)


Jeremy Foreshew


Growth Marketing Strategies


Referral Marketing


[The 45-Day Window: Why Your BFCM Success Story Hasn’t Been Written Yet](https://www.talkable.com/blog/the-45-day-window-why-your-bfcm-success-story-hasnt-been-written-yet/)


Jeremy Foreshew


Referral Marketing


Jeremy Foreshew


Email Marketing


Referral Marketing


Jeremy Foreshew


Growth Marketing Strategies


Referral Marketing


Jeremy Foreshew


## What We Learned at NRF 2026


Retail’s Big Show throws 40,000+ people into the Javits Center and basically says “figure out the future.” This year’s theme was “The Next Now,” which sounds like marketing fluff until you actually walk the floor and realize… yeah, the next now is already here.


[Read More](https://www.talkable.com/blog/what-we-learned-at-nrf-2026/)
