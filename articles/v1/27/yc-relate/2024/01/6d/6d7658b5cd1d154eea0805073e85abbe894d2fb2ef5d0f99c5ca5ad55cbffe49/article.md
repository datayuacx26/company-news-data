---
schema_version: "1.0.0"
document_id: "6d7658b5cd1d154eea0805073e85abbe894d2fb2ef5d0f99c5ca5ad55cbffe49"
company_key: "yc-relate"
company: "Relate"
source_id: "yc-relate-rss-6a73dfa48449"
canonical_url: "https://www.relate.so/blog/cold-email-technical-setup-guide/"
published_at: "2024-01-29T21:58:51+00:00"
first_seen_at: "2026-07-20T23:24:09.769552+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:e94367fd7d7643d25397e022f03e0bee3f51cc60ffee595c21dde2625e6ac2b5"
---

# Cold email technical setup guide in 2024

Using a separate domain for sending cold emails is one of the best practices in cold email outreach. Not having a dedicated outbound domain can significantly impact domain reputation and email deliverability.


However, many people often ignore it because they're unsure how to set it up and find it complicated.


This post guides you in setting up a dedicated outbound domain for cold emails. It covers the following:


- The importance of a dedicated outbound domain
- How to set up a dedicated outbound domain
- Key points to consider when writing cold emails


## The importance of a dedicated outbound domain


### What is an outbound domain?


An outbound domain is a separate domain used exclusively for sending cold emails, different from your company's primary domain.


- Company domain: relate.so
- Outbound domain: relatecrm.so


The outbound domain should not be an alias or a different email account.


- Company email account:won@relate.so
- Alias account:won.you@relate.so
- Another email account:chris@relate.so
- Email account for the outbound domain (needed for cold emails):won@relatecrm.so


Another common mistake is sending cold emails from a personal email account (e.g., gmail.com, yahoo.com) instead of your company's custom domain. This can easily lead to your emails being banned or marked as spam.


## Why do you need an outbound domain?


Sending a large volume of cold emails from your company domain can harm its reputation, leading to poor email deliverability for all company emails.


In the worst-case scenario, email services like Google and Yahoo may ban your domain, temporarily preventing you from sending emails or automatically flagging them as spam. A damaged domain reputation also affects your SEO.


Rebuilding a damaged domain reputation is challenging and time-consuming.


## How to set up a dedicated outbound domain


### 1) Purchase an outbound domain


To set up a dedicated outbound domain, purchase an additional domain from a domain registrar like[GoDaddy](https://www.godaddy.com/en-za?ref=relate.so) or[Namecheap](https://www.namecheap.com/?ref=relate.so) . When purchasing an outbound domain, choosing a name similar to your existing company domain is recommended.


Example:


- Company domain: unicorn.com
- Outbound domain: unicorncrm.com


💡


****Can I use a subdomain (e.g. mail.unicorn.kr) for outbound?****


Using a subdomain (e.g., mail.unicorn.kr) is better than using the main domain for outbound purposes. However, purchasing a separate root domain is still recommended for better results.


### 2) Create a business email account


After purchasing the new domain for outbound, create a business email account. If you use Gmail, you can create a[Google Workspace](https://workspace.google.com/?ref=relate.so) account and set up a new email address.


### 3) Set up your email records: SPF, DKIM, DMARC


SPF, DKIM, and DMARC are protocols that authenticate domains for email sending, preventing security risks like spamming and phishing.


- **SPF** : Stands for Sender Policy Framework. It allows the receiving mail server to verify that a message comes from an authorized source within the listed domain.
- **DKIM** : Stands for DomainKeys Identified Mail. It helps prevent spoofing and spam.
- **DMARC** : Stands for Domain-based Message Authentication, Reporting, and Conformance. It ensures that target email systems trust messages sent from your domain by defining policies for handling messages from domains that fail SPF or DKIM checks.


💡


****What is Email Spoofing?****


Email spoofing is a technique used in spam and phishing attacks, where the attacker deceives the recipient into thinking that the email is from a trusted source.


Setting up SPF, DKIM, and DMARC records is crucial for increasing email deliverability when sending cold emails. Without these settings, your emails may not be delivered at all or end up in recipients' spam folders.


You can set up SPF, DKIM, and DMARC records where you registered your domain. Alternatively, if you use a tool like[Cloudflare](https://www.cloudflare.com/?ref=relate.so) to manage your DNS, you can configure SPF, DKIM, and DMARC there.


### 4) Verify that your SPF, DKIM, and DMARC records are set up correctly


To ensure that your records are set up correctly, you can use services like[Learn and Test DMARC](https://www.learndmarc.com/?ref=relate.so) . It's a free service recommended by Y Combinator for their batch companies.


You can verify the records by sending an email to the email account provided by the Learn and Test DMARC service.


After sending the email, wait a few minutes. The service will then show you the results to confirm whether your SPF, DKIM, and DMARC records are set up correctly.


### 5) Warm up your email


Having your email records set up doesn't mean you can start sending cold emails in bulk right away. Email services will likely flag your emails as spam if you send a large volume from a new domain.


To avoid this, it's essential to warm up your domain and email account. Start by sending a maximum of 10 emails per day and gradually increase the volume to 20-30 while monitoring your email deliverability, open rate, and reply rate.


Typically, it takes at least a month, and sometimes up to three months, to warm up a new domain and email account. Therefore, it's essential to prepare before making cold email your primary sales tactic.


Tools like[Mailwarm](https://www.mailwarm.com/?ref=relate.so) can assist you in the warm-up process.


## Key points to consider when writing cold emails


### 1) Must include an unsubscribe link


When sending automated cold emails, always include an unsubscribe link. Without it, recipients may mark your emails as spam if they are not interested. Make the unsubscribe link easy to find and use without additional actions. This reduces the chances of being marked as spam.


### 2) Continuously manage domain reputation


Even after setting up your outbound domain and sending cold emails, maintaining your domain reputation is necessary. You can check your domain reputation using free tools like[Google Postmaster Tools](https://www.gmail.com/postmaster?ref=relate.so) and[Talos](https://talosintelligence.com/reputation_center?ref=relate.so) . For assessing the spamminess of your outbound domain emails, try[Mail-Tester](https://www.mail-tester.com/?ref=relate.so) .


If you notice lower-than-usual email deliverability or a decline in cold email performance (open rate, reply rate, etc.), it's worth investigating.


Additionally, managing your cold email performance is crucial. Monitor and improve deliverability, open rate, reply rate, bounce rate, and spam rate. Outbound sales automation tools like[Relate Engage](https://www.relate.so/engage?ref=relate.so) can help with this.


To maintain domain reputation and email deliverability, it's important to keep the bounce rate of all emails sent from your domain below 5% and the spam rate below 0.3%. If the bounce rate exceeds 5%, your domain may be banned by Google. A 3-4% bounce rate increases the chances of being filtered as spam. Ideally, keep the bounce rate below 2%.


### 3) Limit the number of emails you send from one mailbox


A good rule of thumb is to limit the number of cold emails sent from one mailbox to a maximum of 50 per day. If your reply rate is above 5%, you can increase it to 100.


Remember, the success of cold emails doesn't solely depend on the volume sent. It's more effective to send fewer personalized emails to the right people. For new domains under three months old, it's recommended to limit the number of emails to 10-30.


## Setting up your emails is key to cold email success


While it's vital to write concise and interesting cold emails with a clear call to action, the success of your efforts relies on proper email setup. No matter how well-written your email is, it won't generate results if it's not delivered, bounces, or ends up in spam folders.


If sending cold emails is a primary sales tactic, I recommend following this guide to create a dedicated outbound domain. You can[schedule a meeting](https://relate.so/cal/relateteam?ref=relate.so) with the Relate team if you need further assistance.


---


Feel free to explore other outbound and cold email-related posts written by the Relate team:


- [How to find high-intent prospects for outbound sales teams](https://www.relate.so/blog/how-to-find-high-intent-prospects/)
- [Cold email sequence best practices](https://www.relate.so/blog/how-to-run-a-cold-email-campaign/)
- [3 Steps to successful prospecting](https://www.relate.so/blog/3-steps-to-successful-prospecting/)
