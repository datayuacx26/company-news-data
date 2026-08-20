---
schema_version: "1.0.0"
document_id: "a00000ea7cdb5d1017952cf09fe55d106f9c77ea45df4c2d7d0bf97406b5d82f"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/6-steps-to-improve-your-sender-reputation-score"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:a74d1ae55cc012783c9ee96808a8238bf7125d36cc1a59488227407247b0d868"
---

# Six Steps to Improve Your Sender Reputation

Every sender wants to land in the inbox. But if you're having trouble, the steps to recovery can be nebulous, uncertain, and unpredictable.


In this guide, we'll help you understand why emails don't land in the inbox and how to improve your reputation as a sender.


## What is a "Sender Reputation Score"?


When you send an email, each Inbox Service Provider (ISP), like Gmail, Outlook, or Yahoo, **evaluates your reputation** before deciding whether to deliver your email to the recipient's inbox.


This evaluation is sometimes called your "sender reputation score." Each provider's criteria are private, but **most evaluate similar factors** .


## What Determines Your Sender Reputation?


Your reputation is a partnership between your Email Service Provider (e.g., Resend) and your own practices. Pay special attention to the following:


### 1. Infrastructure compliance


Email inbox providers require high-volume senders to implement[strong authentication practices](https://resend.com/blog/email-authentication-a-developers-guide) or risk seeing their emails placed in the spam folder. Resend requires proper authentication by default.


[Email Authentication Developer's Guide Learn how to implement strong email authentication practices. resend.com/blog/email-authentication-a-developers-guide](https://resend.com/blog/email-authentication-a-developers-guide)


When you share domains, IP addresses, or email servers with other senders, you also share responsibility for securing that infrastructure. Once again, Resend manages this for our customers, but if you're using another ESP, make sure any shared infrastructure is properly monitored.


### 2. Sender behavior


ISPs want to see consistent and predictable sending patterns. Key variables that affect your score include:


- Your sending history
- [List hygiene](https://resend.com/docs/knowledge-base/audience-hygiene)
- Mail frequency


Traditionally, ISPs relied heavily on IP reputation. Today, however, most ISPs primarily **evaluate domain reputation** to provide more signal on actual sender behavior. If you fail to implement best practices, it can take a long time to recover a tarnished reputation.


[Why are your emails being marked as spam? When it makes sense, when it backfires, and why domain reputation is most important. resend.com/blog/why-your-emails-are-going-to-spam](https://resend.com/blog/why-your-emails-are-going-to-spam)


### 3. Recipient engagement


Opens, clicks, and replies tell mailbox providers your content is wanted. Spam reports tell them the opposite, so make unsubscribing easier than reporting.


The best way to earn positive engagement is to[get specific, opt-in consent](https://resend.com/blog/how-to-properly-get-email-consent) *before* emailing.


[How to Properly Get Email Consent Learn how to get specific, opt-in consent before emailing your recipients. resend.com/blog/how-to-properly-get-email-consent](https://resend.com/blog/how-to-properly-get-email-consent)


## Steps for a Stronger Sender Reputation


If your emails are going to spam, begin with these steps to improve your reputation.


### 1. Audit email authentication


SPF and DKIM are required for email campaigns. When you set up a domain with Resend, we[guide you through the steps](https://resend.com/docs/dashboard/domains/introduction#verifying-a-domain) to add the necessary records.


[DMARC authentication](https://resend.com/docs/dashboard/domains/dmarc) is also effectively mandatory for any professional email campaign.


- **Loosen your policies until you’re fully compliant:** If your problems stem from internal senders who are out of compliance, address that issue first. Set your DMARC policies to “allow,” and[tighten them one subdomain at a time](https://resend.com/blog/how-dmarc-applies-to-subdomains) as you improve compliance.
- **Set restrictive policies if your problems are external:** Once you're confident in your own compliance, you can tighten up your authentication settings.


For more help on setting and enforcing DMARC policies, see our guide below.


[DMARC policy modes A practical guide to DMARC policy modes—none, quarantine, and reject—and what to expect when enforcing them. resend.com/blog/dmarc-policy-modes](https://resend.com/blog/dmarc-policy-modes)


### 2. Slowly increase new sending


Sending a large volume of email from a new domain or subdomain looks suspicious. Since mailbox providers don’t know you, they don’t trust you.


- **Warm up new domains:** Gradually increase the amount of email you’re sending. See our[warm-up calculator](https://resend.com/docs/knowledge-base/warming-up#warm-up-calculator) for a personalized plan based on your needs.
- **Start new campaigns with engaged readers:** Test new email tactics with recipients who regularly engage with your emails. If your emails are landing in the inbox, gradually expand the campaign.
- **Observe, test, respond:** Observe your engagement rates, bounce or spam responses, throttling from ISPs, and more. If you're sending at scale, use a tool like[Google Postmaster Tools](https://gmail.com/postmaster/) to understand how ISPs see your content and help you know how to alter your sending behavior.


### 3. Pinpoint deliverability problems


To make improvements, locate the source of the problem first. One or two providers may be blocking you, or you may be on a blocklist. If you're a Resend customer,[we can help diagnose](https://resend.com/support) . To investigate issues yourself:


- **Identify which inbox providers are blocking you:** Analyze your[DMARC reports](https://resend.com/blog/how-to-read-a-dmarc-report) and use resources like[Google Postmaster Tools](https://gmail.com/postmaster/) to determine whether your issues are widespread or limited to one or two inbox providers.
- **Audit your domain:**[Email security scanners](https://www.gartner.com/reviews/market/email-security-platforms) ,[Google Search Console](https://search.google.com/search-console/about) , and[OWASP vulnerability scanners](https://owasp.org/www-community/Vulnerability_Scanning_Tools) can surface domain issues that affect deliverability.
- **Check if you’ve been blocklisted:** Use a tool like[Spamhaus](https://check.spamhaus.org/) or[Sender Score](https://senderscore.org/assess/blocklist-lookup/) to look up your URL and check it against multiple blocklists.
- **As a last resort, request a review:** After making fixes, prompt providers to re-evaluate. Google Search Console has built-in blocklist review; others use forms:


- [Gmail](https://support.google.com/mail/contact/gmail_bulk_sender_escalation)
- [Yahoo Mail](https://senders.yahooinc.com/contact)
- [Microsoft Outlook](https://sendersupport.olc.protection.outlook.com/pm/troubleshooting)


Reviews can be slow and may require follow-up, especially with the largest providers.


### 4. Deep clean your email list


Maintaining "clean" lists is like a regular household chore. Neglect it, and your audience engagement and deliverability will begin to suffer.


- **Set a schedule for routine list hygiene:** As a starting point, proactively manage your[audience hygiene](https://resend.com/docs/knowledge-base/audience-hygiene) . Automate as much of the process as you can using[webhooks](https://resend.com/docs/webhooks/introduction) .
- **Experiment with list segmentation:** To get higher open rates and better target your content, try breaking up your distribution lists by demographic. It may take some trial and error to find the right segmentation.
- **Review your list segmentation to reduce over-sending:** Use filtering rules to limit how often you send to each recipient.
- **Review your sign-up processes.** How are email addresses added to your lists? Review the[verification process](https://resend.com/blog/best-email-verification-apis) for each sign-up method. Ensure the process checks for common typos, ideally uses double opt-in, and[gets consent](https://resend.com/blog/how-to-properly-get-email-consent) .


### 5. Make it easy to unsubscribe


Opt-in subscribers engage more. Make joining intentional and leaving easy, and your reputation will rise with your open rate.


Inbox providers weigh recipient behavior on both sides:


Positive signals Negative signals


Opens Spam reports


Clicks Quick deletes without opening


Replies Bounces


Forwards Filtering rules to skip inbox


Adding sender to contacts Marking as "not important"


Tactics to grow the left column:


- **Use double opt-in:** Send a[confirmation email](https://resend.com/docs/knowledge-base/audience-hygiene#ensure-the-recipient-is-consenting-with-double-opt-in) after signup. Confirmed subscribers want to be there.
- **Chase positive engagement, not just the absence of negative.** Opens, clicks, and replies move your score up more than avoiding unsubscribes. Consider asking for feedback in onboarding emails (e.g., *What brought you to our service? Did you have any issues getting started?* ) when users are more likely to engage, or send a targeted campaign to engaged users.
- **Don't hide the unsubscribe link.**[Unsubscribes are far better than spam reports](https://obrienmedia.co.uk/unsubscribing-vs-reporting-as-spam-understanding-the-difference-and-its-impact-on-small-businesses) . If the link is hard to find, people will pick spam instead.
- **Offer flexible options.** Let recipients choose categories (Resend calls these[Topics](https://resend.com/docs/dashboard/topics/introduction) ) or frequency. Fewer wanted emails beat more ignored ones.


### 6. Send valuable content


A catchy subject line earns opens once or twice. But if the body doesn't deliver what you promised, engagement drops.


- **Lead with clear value:** Tell readers why you're emailing and what they get. Include one clear call to action.
- **Be cautious with inactive recipients:**[Win-back campaigns](https://www.litmus.com/blog/reengagement-winback-emails/) often fail, and repeatedly emailing people who never open will hurt your reputation.
- **Optimize for mobile:** Most opens happen on phones, so test across small screen sizes.


## What's next?


Your sender reputation work is never done. ISPs are always evaluating your sending, so keep these best practices top of mind. Observe, respond, test, observe, and repeat.


Resend's[team of email experts](https://resend.com/support) is here to help you troubleshoot any issues you may encounter. We're happy to provide personalized advice to improve your sender reputation.
