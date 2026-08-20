---
schema_version: "1.0.0"
document_id: "8a1d4124f9ba1fe674269344cffeab8335822c76c3a3c568338a483683d67ff0"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/top-10-email-deliverability-tips"
published_at: "2025-01-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:e4ea363ffd717978f6e9fb714b412f13213fb287a15d080f29d61c775f5904ff"
---

# Top 10 Email Deliverability Tips

Email deliverability can be hard to improve. Why do some emails land in the spam folder and others in the inbox?


Here are our top 10 tips for reaching the inbox.


1. Use a subdomain
2. Set up DMARC
3. Match URLs to the sending domain
4. Avoid link and open tracking
5. Keep emails small and accessible
6. Don't use look-a-like domains
7. Test emails properly
8. Maintain a clean email list
9. Don't use no-reply emails
10. Send consistently


**Prefer watching a video?**


## 1. Use a subdomain


The domain you choose to send emails with can significantly impact deliverability. Generally speaking, you should send emails from a subdomain instead of a root domain.


Sending from a subdomain holds a few benefits:


- It helps you communicate the email's purpose.
- It can limit any reputation damage to that subdomain.


If you use a subdomain, you can separate your reputation. If there's an issue with your marketing subdomain reputation, it's less likely to impact your transactional subdomain.


[→ Learn about subdomains](https://resend.com/docs/knowledge-base/is-it-better-to-send-emails-from-a-subdomain-or-the-root-domain)


## 2. Set up DMARC


DMARC is an email authentication protocol that tells mail servers what to do if an email message fails SPF and DKIM.


Name Type Value


_dmarc.example.com TXT v=DMARC1; p=none; rua=mailto:report@example.com;


In this way, it protects against email spoofing. A DMARC record builds trust with mailbox providers, as it allows them to verify that your emails are authorized by your domain.


[→ Learn about DMARC](https://resend.com/docs/dashboard/domains/dmarc)


## 3. Match URLs to the sending domain


Ensure that the URLs in the body of your email match the sending domain. Mismatched URLs can trigger spam filters since spammers often use this technique.


In other words, if you send an email from` example.com` , whenever possible make sure that the URLs in the email are also hosted on` example.com` .


When possible, linked images should also match the sending domain.


## 4. Avoid link and open tracking


While link tracking and open tracking are great to measure the impact of marketing emails, they can actually hurt your deliverability for transactional emails like notifications, magic links, and more.


Mailboxes can see the intent of the email, and when a magic link, for instance, doesn't lead to your domain, it can make the email look suspicious.


Setting up separate subdomains for transactional and marketing emails can often make it easier to include or remove these practices for different email types.


[→ Learn about link and open tracking](https://resend.com/docs/dashboard/emails/deliverability-insights#disable-click-tracking)


## 5. Keep emails small and accessible


Gmail has a size limit of 102KB for each email message. Once that limit is reached, the remaining content is clipped.


Including a plain text version of your email is also good practice as it ensures that your message is accessible to all recipients, including those who have email clients that don't support HTML.


To help with both email size and accessibility, we recommend avoiding emails with a lot of images.


## 6. Don't use look-a-like domains


Spammers often phish from cousin or look-a-like domains, so avoid using them.


For example, if your domain is` coca-cola.com` , don't use` cocacola-mails.com`


Instead of registering an email-specific domain, which can cause brand confusion and look like spam, separate your sending using the subdomains method we covered earlier.


## 7. Test emails properly


When testing, it's tempting to send to fake email addresses to test for bounces, among other things.


While testing *is* important, intentionally hard-bouncing emails can damage your sending reputation. The bounces are real and will damage your reputation.


Instead, you should use a testing email address provided by your email service or create a testing email account (like testing@\[your domain\].com).


[→ Learn about testing emails on Resend](https://resend.com/docs/dashboard/emails/send-test-emails#how-to-send-test-emails)


## 8. Maintain a clean email list


You should only send to those who've asked to be sent to and *don't* send to unsubscribers, those who haven't engaged with your content, or those who've marked your previous emails as spam.


Testing should account for all of these scenarios. To keep a clean email list, you can capture bounces or spam complaints using a webhook and remove them from it.


[→ Learn about Resend webhooks](https://resend.com/docs/dashboard/webhooks/introduction)


## 9. Don't use no-reply emails


No-reply emails indicate to mailbox providers that the email is one-way communication and decreases trust.


It signals to inboxes that they cannot provide feedback on your emails (like reporting spam).


## 10. Send consistently


Mailbox providers are suspicious when you suddenly change the volume of sending.


If you want to send thousands of emails in a few months, you need to warm up your domain by sending them regularly ahead of time. Otherwise, you risk a large bounce rate and a big hit to your reputation, which can be hard to recover from in the future.


[→ Learn more about deliverability](https://resend.com/docs/knowledge-base/how-do-i-avoid-gmails-spam-folder)
