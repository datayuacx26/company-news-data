---
schema_version: "1.0.0"
document_id: "bca598f92eaa251cc5e5e03defb09d33ec21859c5b06a823f8d81655a0d9b0ba"
company_key: "yc-fazeshift"
company: "Fazeshift"
source_id: "yc-fazeshift-news-import-fdc09a038b01"
canonical_url: "https://www.fazeshift.com/post/dunning-management-how-to-automate-payment-follow-ups"
published_at: "2025-11-25T22:20:08.322+00:00"
first_seen_at: "2026-07-25T04:29:55.225341+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:78e1de42f1cf98608bd5d594bf0747612c366c897e6b9ed31f6115d4f3e6a665"
---

# Dunning Management: How to Automate Payment Follow-ups

Payment failures and overdue invoices create significant revenue leakage for B2B companies. In B2B invoice collections,[half of all invoices in the US are paid late](https://www.kaplancollectionagency.com/business-advice/54-statistics-on-the-b2b-payment-delays/) , with companies spending an average of $39,406 annually on collection efforts.[Bad debt averages 8% of all B2B invoices](https://group.atradius.com/knowledge-and-research/reports/b2b-payment-practices-trends-us-2024) .


Many of these customers don't even know there's a problem. In subscription billing, their credit card may have expired or hit a temporary spending limit. In B2B invoicing, the invoice may be sitting in an AP queue awaiting approval, stuck with the wrong contact, or simply lost in someone's inbox. They're not intentionally avoiding payment—they either don't know or haven't gotten around to it.


Every failed payment represents a choice: let the customer slip away, or implement a systematic process to recover the revenue while maintaining the relationship.


Manual dunning processes don't scale. Someone on your team manually tracks failed payments and sends follow-up emails. The process is inconsistent, time-consuming, and often feels either too aggressive or not persistent enough.


**Automated dunning systems solve this through systematic, empathetic payment recovery.**[PYMNTS Intelligence found that top-performing subscription merchants recover 60% of failed payments](https://www.pymnts.com/subscriptions/2023/top-performing-subscription-merchants-recover-60-of-failed-payments/) , significantly outperforming industry averages.[Forrester Consulting research](https://assets.ctfassets.net/40w0m41bmydz/5cBKwBsr7Yyc9PyfUtfWUJ/f243d74c344cbfa83de93785e684b295/forrester-consulting-rethink-your-payment-strategy-us.pdf) showed that businesses actively monitoring failed payments achieve 43% better collection rates than those who don't monitor.


If you process recurring payments, you're already losing revenue to payment failures whether you track it or not. You can either recover that revenue systematically or leave it on the table.


‍


## **What Is Dunning Management?**


Dunning management is the systematic process of following up with customers when payment charges fail. The term "dunning" has roots in debt collection, but modern dunning for subscription businesses takes a completely different approach. **These customers want to pay but can't because of a technical or temporary issue.**


Payment failures and overdue invoices happen constantly.[About 10% of card transactions fail every billing cycle](https://www.chargebee.com/resources/glossaries/what-is-dunning/) . The most common causes are expired credit cards (e.g. customers forget to update their payment method), insufficient funds at the time of the charge, network issues or temporarily blocked cards, and outdated billing information after someone moves or changes banks. For B2B payments made via wire, ACH, or check, the causes differ.[ACH returns happen due to insufficient funds (R01), closed accounts (R02), or invalid account numbers (R03)](https://nacha.org/system/files/2023-05/ACH%20Risk%20Management%202023%20-%20Proposed%20Modifications%20to%20the%20Rules%20-%20May%202%202023.pdf) . Wire transfers may stall due to incorrect beneficiary details, compliance holds, or intermediary bank delays. Check payments bounce when accounts lack sufficient funds. Unlike card payments that can be silently retried, these payment types require direct customer communication to resolve.


**Traditional dunning was aggressive and transactional - collection agencies sending threatening letters. Modern dunning treats payment failures as a customer service issue.** Your customer wants to keep using your product. They just need a gentle reminder to update their payment information, or they need you to retry the charge at a better time.


[Your tone and timing directly affect whether customers update their payment method or let their subscription lapse](https://www.fazeshift.com/post/collection-letter-templates-automation) . Automated dunning systems handle this balance by implementing consistent, well-timed communications that recover revenue without damaging the customer relationship.


‍


## **Why Dunning Matters for Your Business**


**These customers want to continue doing business but can't—or haven't—completed payment.**[Acquiring a new customer costs 5-25 times more than retaining an existing one](https://hbr.org/2014/10/the-value-of-keeping-the-right-customers) , so every failed or overdue payment represents revenue you've already invested acquisition dollars to generate.


**Timing matters more than most teams realize.** According to Commercial Collection Agencies of America survey data representing 60% of US commercial collection claims, collection probability drops significantly as accounts age:[69.9% at 90 days, 52.1% at six months, and just 22.8% at twelve months](https://www.cstworldwide.com/resources-collection-statistics.html) . The earlier you follow up, the more you recover.


Manual dunning doesn't scale. Without automation, someone on your finance team spends hours each week manually tracking failed payments, deciding who to contact, drafting emails, and following up. This creates inconsistent customer experiences - one customer gets three reminder emails while another gets one, purely based on when someone reviewed the aging report. This inconsistency damages your brand and reduces recovery rates.


‍


## **How Automation Changes the Game**


Companies implementing automated churn management solutions see an average revenue lift of 8.6%. These are measurable results from businesses that systematically follow up on failed payments.


**Automated systems deliver four capabilities that manual processes can't match:**


### **Smart retry logic (for card payments)**


Smart retry logic separates payment retry attempts from customer communications. Instead of emailing a customer immediately when their payment fails, automated systems first attempt to collect the payment silently. If a card is declined due to insufficient funds, the system retries the charge at a different time of day when the customer's account balance might be higher. If the retry succeeds, the customer never even knows there was a problem.


### **ACH return handling (for bank payments)**


ACH payments require different logic.[NACHA rules permit a maximum of two retry attempts within 180 days for NSF returns](https://www.nacha.org/rules/ach-network-risk-and-enforcement-topics) (R01 insufficient funds, R09 uncollected funds)—and the retry must include "RETRY PYMT" in the Company Entry Description field. Best practice is to wait 3-5 business days before the first retry to allow for payroll deposits, then 5-7 days before the second attempt.


Administrative returns like R02 (closed account) or R03 (no account found) cannot be reinitiated until corrected information is obtained—you must contact the customer to verify banking details.[Unauthorized returns (R10, R29) should never be reinitiated; NACHA monitors these against a 0.5% threshold](https://www.nacha.org/rules/ach-network-risk-and-enforcement-topics) , with violations risking fines up to $500,000.


### **Wire transfer and check follow-ups**


Wire transfers and checks can't be "retried"—they require customer action. When a wire payment is overdue, first verify the customer has correct banking details.[SWIFT reports that 92% of cross-border payments are credited within 24 hours](https://www.swift.com/swift-resource/249536/download) , but compliance holds, intermediary bank processing, and time zone differences can extend settlement to 3-5 days.


For bounced checks, document all details immediately and contact the customer within 24 hours. Checks can typically be re-presented up to three times if the return was for insufficient funds and the customer confirms funds are now available. State laws on bad check recovery vary—[California Civil Code Section 1719 allows treble damages (minimum $100, maximum $1,500)](https://law.justia.com/codes/california/code-civ/division-3/part-3/section-1719/) ;[Texas permits the check amount plus a $30 processing fee](https://law.justia.com/codes/texas/business-and-commerce-code/title-1/chapter-3/subchapter-e/section-3-506/) —so understand your options before escalating.


### **Segmented communication**


Segmented communication tailors the dunning approach based on customer value and payment history. Your highest-value customers receive personal outreach from an account manager after the first payment failure. Mid-tier customers get automated emails with a dedicated support contact. Low-value accounts go through a fully automated sequence with self-service payment updates. The system tracks payment history and adjusts the approach automatically.


### **Multi-channel outreach**


Multi-channel outreach reaches customers through their preferred communication method. Email serves as the primary channel for most dunning communications. SMS provides escalation for time-sensitive reminders ("Your account suspends in 24 hours"). In-app notifications catch customers while they're actively using your product. Phone calls are reserved for high-value accounts where personal intervention justifies the cost.


### **Real-time tracking**


Real-time tracking provides visibility into payment status without manual reporting. Your dashboard shows which invoices were delivered, which were viewed, which are paid, and which are overdue. Exception alerts flag unusual patterns - a typically reliable customer suddenly going silent, or invoices sitting unviewed beyond normal timeframes. Predictive analytics identify high-risk accounts before payments fail, giving your team enough lead time to address issues before the payment failure happens.


**Automation handles routine follow-ups while your team focuses on exceptions and high-value accounts.** Instead of spending 20 hours per week manually tracking failed payments, your finance team spends that time on strategic work while automation handles 90% of the recovery process.


‍


## **Implementation Guidelines**


Most businesses use an escalation sequence with 3-4 touchpoints. Start with a friendly payment reminder, escalate to consequence warnings, and end with a final notice. The key is progressive urgency without harassment - each message should be more direct than the last while maintaining a helpful tone.


**Subject lines:** Include your company name for recognition. Be specific ("Update Payment Method" not "Important Information"). Avoid spam triggers. Don't use ALL CAPS until final escalation.


**Mobile optimization:** Seventy percent of payment updates happen on mobile devices. Link directly to the payment form with no login required. Test monthly across devices. If your payment update page doesn't work perfectly on a phone, you're losing the majority of potential recoveries.


**Segmentation:** High-value accounts get personal outreach. Mid-tier gets automated emails with support contact. Low-value gets full automation. Don't use one-size-fits-all approaches - a customer paying $10,000/year deserves different treatment than one paying $10/month.


**Tone:** Frame as customer service, not collections. Avoid accusatory language like "account is delinquent." Assume good faith - they forgot to update their card or their bank flagged a legitimate charge. After the fourth email with no response, stop. Continuing to email someone who's ignoring you damages your sender reputation.


‍ **Process consistency:** Manual dunning creates inconsistent customer experiences. One customer gets three follow-ups while another gets one, purely based on when someone reviewed the aging report. Automation ensures every customer receives the same systematic follow-up based on their segment and payment history.


‍


## **Final Thoughts**


Modern dunning management combines systematic follow-up with empathetic customer service. **The businesses that succeed at dunning recover more revenue while maintaining better customer relationships.**


Automation makes both possible. Your system handles the routine follow-ups with appropriate timing and tone while your team focuses on high-value accounts and exceptions that need personal attention. The recovered revenue doesn't just improve your bottom line - it represents customers who actively want to continue their subscription but couldn't because their credit card expired or their bank flagged a legitimate charge.


Most businesses lose revenue to payment failures and overdue invoices. Systematic dunning management recovers it by addressing the customer's payment barrier, not demanding payment.


‍


## **Frequently Asked Questions**


**What's the difference between dunning and collections?**


Dunning addresses payment failures from customers who want to pay but can't due to technical or temporary issues—expired cards, insufficient funds at charge time, or invoices stuck in approval queues. Collections pursues customers who are intentionally avoiding payment or have serious financial problems. The tone differs completely: dunning is customer service ("let's fix this together"), while collections is debt recovery. Most failed payments in subscription and B2B billing are dunning situations, not collections situations.


**How many reminder emails should I send before giving up?**


Most businesses use 3-4 touchpoints with progressive urgency. Start with a friendly reminder, escalate to consequence warnings, end with a final notice. After four emails with no response, stop—continuing damages your sender reputation and won't recover the payment. The exception is high-value accounts, where phone outreach from an account manager may be worth the additional effort before closing the account.


**What recovery rate should I expect from automated dunning?**


Top-performing subscription merchants recover 60% of failed payments according to PYMNTS Intelligence research. Businesses actively monitoring failed payments achieve 43% better collection rates than those who don't. Your actual rate depends on your customer base, payment methods, and how quickly you follow up. The earlier you act, the more you recover—collection probability drops from 70% at 90 days to 23% at twelve months.


**Should I handle card payments differently than ACH or wire transfers?**


Yes. Card payments can be silently retried before contacting the customer—if insufficient funds caused the decline, retrying at a different time may succeed without the customer ever knowing. ACH payments have strict NACHA rules: maximum two retries within 180 days for NSF returns, and you must include "RETRY PYMT" in the description field. Administrative returns (closed account, invalid number) cannot be retried until you get corrected information from the customer. Wire transfers and checks can't be retried at all—they require direct customer action to resolve.


**When should I automate dunning vs. handle it manually?**


Automate routine follow-ups for the majority of accounts—this ensures consistent timing and frees your team for higher-value work. Reserve manual intervention for high-value accounts where personal relationships justify the cost, and for exceptions that automation flags (typically reliable customers suddenly going silent, unusual payment patterns). Most businesses find automation handles 90% of the recovery process while their team focuses on the 10% that needs human judgment.


‍
