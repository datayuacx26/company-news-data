---
schema_version: "1.0.0"
document_id: "3cbaa8d8b5025a5aab9845f44284d773abc893d7f37c50c3fb475bb1c25ab02e"
company_key: "yc-flycode"
company: "FlyCode"
source_id: "yc-flycode-news-import-523b281c6a73"
canonical_url: "https://www.flycode.com/blog/how-to-deal-with-failed-payments-if-you-re-using-stripe"
published_at: "2024-12-12T00:00:00+00:00"
first_seen_at: "2026-07-23T09:50:45.349061+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:fa19bc9cfb041c958e376c535ec97a95cd0662d54fb29ef4fd887357e808b555"
---

# Top 8 strategies to recover more failed payments

# How to deal with Failed Payments if you're using Stripe


💡 **TL;DR:** If you sell SaaS B2C/B2B subscriptions, we’re here to help you recover lost revenue due to failed payments and involuntary churn.


## **❌ THE PROBLEM: Recovering Failed Payments is Complex**


Involuntary churn refers to the loss of subscribers due to payment failures, not due to them actively canceling. While it is both possible and a worthwhile strategy to attempt to win back a customer during the cancellation process — involuntary churn is not intentional and in most cases, your customers aren’t even aware that their payment failed.


## **💰 Bottom line?**


Effective payment optimization does reduce involuntary churn and will increase ARR by as much as 5-10%


## **🎟 What causes payment failures and involuntary churn?**


There’s no single cause of this problem that can be isolated. In fact, there are dozens of reasons \[insufficient funds, fraud, expired cards, AVS, SCA, invalid card, etc.\], and solutions to each failure need to be applied individually. Let’s look at a few examples:


1.


**Batch Processing and Fixed-Interval Retries.** The majority of subscription payments and retries are processed in bulk during off-hours. If your customers are based in multiple time zones, this can increase payment failures and reduce the success rate of retries. Remember when you had to notify your bank when you were traveling? The concept is the same, a 3am charge is more likely to get flagged as fraud than a 9am charge.


2.


**Expired Cards or Invalid Card Numbers:** These are frustrating because you think you have to ask your customers to do something, surprisingly there are many cases when you don’t. When you lose a credit card and replace it, do you ever wonder how Netflix still manages to charge you? It’s all thanks to Card Account Updater and Network Tokenization — if you don’t have it implemented, it’s an easy win.


3.


**Customers with Insufficient Funds:** Timing is a critical factor, especially with these types of errors. If they don’t have funds, don’t force it. When retry attempts are spread out or targeted around periods of the month when cash inflow is expected, success rates will go up.


4.


**Payments that are incorrectly classified as fraud:** This can cause financial losses, as well as a negative impact on customer experience. It is important to prevent those legitimate payments from failing.


# **Stripe use-case: how to recover more failed payments with Stripe?**


When you’re on the receiving end, teams generally believe that failed payments are not only a black box, but the cost of doing business. Some believe that when a payment fails a new card is required so they rely on sending emails or making phone calls to resolve them.


We thought it might be useful to share some tips to allow you to recover more payments and reduce churn triggered by payment failures in Stripe. The advice applies to most payment providers when it comes to recurring billing.


Also, if you want help setting up recovery rules, we’re more than happy to lend a hand. This includes more advanced rules and automations as well as the queries for a detailed payment audit in Stripe - please don’t hesitate to reach out


**Get our Stripe app:**


[https://marketplace.stripe.com/apps/flycode-payments](https://marketplace.stripe.com/apps/flycode-payments)


# **How to recover more revenue with Stripe?**


1.


### **Pick the best option from the Stripe’s basic “Revenue Recovery”**


Stripe’s Revenue Recovery has two options: (1) Custom Retries (2) Smart Retries.


The Custom Retry policy has fixed intervals with up to 3 retries every 1, 3, 5 or 7 days, which we refer to as a brute force strategy (up to 8 with Billing Scale). If you have very specific needs of charging on specific days or within a short timeframe, then this option is fine. The probability of recovery will be much lower but it’s better than nothing.


The disadvantages? With the majority of subscription payments and retries happening in bulk during off-hours, timing is not on your side. For example, if your customers are based in multiple time zones, off-hour processing can increase payment failures and will certainly reduce the success rate of retries. Remember when you had to notify your bank when you were traveling? The concept is very similar, a 3am charge is more likely to get a higher risk score or flagged as fraud by a bank than a 10am charge.


The Smart Retry policy is a better option if you want an out of the box solution. Currently you can pick 4 or 8 retries and run them from 1 week to 2 months:


The best payment optimization solutions will try to recover payments with (1) the least number of retries (2) as quickly as possible and (3) with the fewest customer communications. Stripe’s Smart Retries are limited to 8, however, the card networks allow up to 15 retries times in a 30-day window for certain error codes — often referred to as soft declines.


The disadvantages? Smart Retries is a better option than the Custom Retries but it’s also limited in the recovery time and number of retries. Not every failure can be recovered with retries so sending communications is critical but there’s a fine line until it creates the opposite effect with email fatigue. Each time a payment fails an email will be triggered, which means up to 8 emails on top of other campaigns or manual comms sent by the team.


1.


### **Try to recover a payment before you are communicating with your customers**


The vast majority of legacy recovery platforms are essentially email outreach tools. The main challenges with email and sms are that (1) they aren’t read immediately and if already recovered it hurts the customer experience (2) it’s shifts the problem to your customer when at the moment it’s yours (3) and most importantly it increases the risk that your customer will actively cancel and churn because of a failed payment (we refer to this motion as ‘Passive to Active’ or ‘Involuntary to Active’ churn).


For specific error codes, the suggestion is to hold off on comms for a few days until you’ve tried to recover the payment. The best option is to coordinate the communications with the retries automatically for each customer's payments and why they’re failing.


Stripe Recovery emails settings have a toggle option to send or not to send emails.


At the moment there is no easy way with Stripe recovery to delay the emails or to coordinate the emails with retries so we suggest either using Stripe’s basic email option or building a campaign with one of the email outreach products.


1.


### **Understand the real MRR and LTV impact of lost payments**


When a failed payment is unrecoverable you not only lose the immediate revenue but also a significant chunk of potential LTV. If your average customer stays for 6-12 months, increasing your recovery rate from 45% to 55% can increase ARR by 6%-10%. The impact of saving failed payments has a huge impact on the bottom line.


1.


### **Keep the subscription or invoice status active for longer**


If it works for you operationally, for your product, or for your margins consider keeping the status in a ‘past-due’ state for longer. Once canceled. a subscription cannot be recovered, and the customer will need to re-subscribe. It’s not unusual for customers to need more time to take actions. Try to find the right balance between sufficient recovery time and accurate revenue/churn forecasting.


Many companies prefer to cancel the subscriptions at the end of the retry period to have clear visibility to their forecasting. This totally makes sense, however, if the recovery period is set for 2 weeks you are limiting your ability to recover the customer.


Furthermore, we’re all in the camp of ‘build something people want’ and if that’s the case, many customers will reactivate once they realize their account is frozen/downgraded or they never received their shipment. This is (1) a much easier workflow to support (2) a better customer experience and (3) typically more profitable since they owe you money.


1.


### **Most payment failures aren’t due to expired cards**


It should probably be at the top, but most failed payments do not originate from expired cards. Annual subscriptions tend to have more, but it means that a significant portion of your payment recovery doesn’t necessarily require the customer in the loop. On top of that, solutions like Card Account Updater (CAU) and Network Tokens help to replace old details like expiration date automatically (make sure you have this implemented).


One of the biggest challenges is that payment providers, issuing banks, and card networks do not have unified error codes. Each may have errors classified into different categories, which adds complexity and leads to many errors getting bucketed together, such as ‘Do not honor’. Here are some of the common decline codes:


**Customers with Insufficient Funds:** Timing is a critical factor, especially with these types of errors. If they don’t have funds, don’t force it. When retry attempts are targeted around time of day, day of the week or month success rates will go up. For example, there are certain periods in a month when deposits are expected or when balances are paid down for debit and credit cards.


1.


### **Personalized your communications and make sure they’re transactional**


This is not unique to failed payment recovery - personalized emails that look and feel like their from a real person perform better. Whether it’s a real person's name from your support email or from a personal email tied to a distribution, the CTA dramatically improves.


Pro tip 1: make sure your payment emails are transactional and optimize the change card flow


Pro tip 2: dump the CSS, use text only emails to increase deliverability


Pro tip 3: add transactional SMS


Pro tip 4: add in-app notifications after each login


1.


### **Treat your top customers differently**


**Outreach:**


When a top customer has a payment in recovery and is at churn-risk, try different ways to reach out — send a personal note, try emailing from different accounts, or give them a call.


Pro tip 1: set up a workflow to identify top customers that are at churn-risk for manual outreach


Pro tip 2: if the main account contact doesn’t reply, you can consider to reach out to other users in the account.


**Cancel subscriptions:**


You **don’t** want to cancel the subscriptions for your top customers automatically. Try reaching out via multiple channels and adding a person to the loop for your top customers. I’ve spent a lot of time automating what’s technically possible in this area but leave room for a personal touch. The customer may still churn but if you manage to connect with them you may also gain some valuable feedback on why they churned.


1.


### **Add your failure and recovery rate to your main KPI’s and track it monthly**


Your failure and recovery rate have a clear impact on your MRR. It’s a good practice to add these percentages to your monthly KPIs. Changes in the failure rate and recovery rate will provide immediate signals about customer quality based on acquisition channel, the offers or promotion driving buying decisions, and most importantly the payment health of your business.


There are many more nuances to this as there are with each of **your businesses** so it’s important to adjust the recovery settings and these suggestions for optimal results. If you want help with this, please reach out and we’d love to help you.


## **FLYCODE SOLUTION 💡**


[FlyCode](https://www.flycode.com/) leverages ML and AI to provide smart payment retries and recovery communications to maximize subscription revenue and reduce churn. We apply a unique model tailored to each Merchant and their customers to ensure optimal results.


**Get our Stripe app:**[https://marketplace.stripe.com/apps/flycode-payments](https://marketplace.stripe.com/apps/flycode-payments)


⚡️ **Automatically** recover more failed subscription payments and reduce passive churn


⚡️Access advanced ML & AI to find the **best time** to retry and email/SMS your customers


⚡️ Improve **customer experience** by coordinating payment retries with customer emails/SMS


🛠️ We have **1-Click install** **+ Custom Integrations** for Merchants and White-Label for Platforms
