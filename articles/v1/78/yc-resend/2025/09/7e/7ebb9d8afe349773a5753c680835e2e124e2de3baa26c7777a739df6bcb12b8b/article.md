---
schema_version: "1.0.0"
document_id: "7ebb9d8afe349773a5753c680835e2e124e2de3baa26c7777a739df6bcb12b8b"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/four-ways-to-hurt-your-sender-reputation"
published_at: "2025-09-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:e53f9669e999f12cbc0168015818e539fd414b67e643c1b232dedf448df07f5f"
---

# Four Ways to Hurt Your Sender Reputation

How trustworthy are the emails sent from your domain?


This is a critical metric estimated by your **sender reputation score** , which determines inbox placement for your messages and their likelihood of being marked as spam.


No single system will give you a fixed number, but you can make an educated guess about your own sender reputation. Let’s start that education with some basics:


- **Who sets my sender reputation score?** While each email service provider ( **ESP** ) and mailbox provider ( **MBP** ) has its own proprietary algorithm to determine your score, they all use similar factors in their calculations.
- **What happens if my score is bad?** A poor score means your emails are likely to be marked as spam or blocked completely.
- **How do I find out what mine is?** ESPs and MBPs keep their formulas and scores secret, but you can get an estimate from public score checkers.


Here are some public score checkers you should consider:


- [Google Postmaster Tools](https://gmail.com/postmaster/)
- [Spamhaus](https://www.spamhaus.org/domain-reputation/)
- [Trellix](https://trustedsource.org/)
- [Talos](https://talosintelligence.com/reputation_center)


You should also stay on top of blocklists using a tool like:


- [MXToolbox](https://mxtoolbox.com/)
- [Multirbl](https://multirbl.valli.org/lookup/)


The best approach is to avoid common pitfalls instead of obsessing over a specific score number. Since scoring algorithms use similar factors, fixing basic mistakes can boost your score across ESPs and MBPs.


Below, we’ll explain **four ways you might be hurting your sender reputation** . Tackling these issues is a great way to improve your reputation, inbox placement, and open rates.


## 1. Sloppy, Unsegmented Recipient Lists


If you fail to maintain a[clean email list](https://resend.com/docs/knowledge-base/email-list-hygiene) , you’ll soon see an impact on the rate of[hard and soft bounces](https://resend.com/docs/dashboard/emails/email-bounces) , as well as the rate of[unsubscribes](https://resend.com/docs/knowledge-base/should-i-add-an-unsubscribe-link) , all of which impact your sender reputation score.


The ideal email list contains only:


- valid email addresses
- that have not unsubscribed
- that engage with your content


**Validating email lists is crucial** to prevent sending to defunct email addresses, full mailboxes, and misspelled addresses. A full mailbox may still receive your mail after a delay — that’s a soft bounce. Invalid email addresses, on the other hand, mean hard bounces. Too many of those can be harmful to your sender reputation score.


At a minimum, regularly run a script to remove improperly formatted email addresses from your lists. You don’t have to do it manually, either. You’ll find tools for email list maintenance in a variety of free and paid options.


[Our Email Verification API Guide A detailed analysis of the top verification APIs for devs. resend.com/blog/best-email-verification-apis](https://resend.com/blog/best-email-verification-apis)


High unsubscribe rates are another sign you need to better manage your recipient lists, even if you’ve validated the addresses. Instead of sending the same emails to everyone on your list, **consider segmenting your audience** by specific topics.


Recipients who get too many[generic or irrelevant emails](https://resend.com/blog/how-to-properly-get-email-consent) from the same source will unsubscribe. When they do, your reputation score takes a hit.


## 2. Looking like Spam When You’re Not


You know you’re not spam, but how do recipients or MBPs know this? What qualifies as spam can be subjective, so take steps to avoid being marked as a malicious sender.


### Maintain Healthy Spam and Bounce Rates


Keep your spam complaint rate below **0.08%** and your bounce rate below **4%** . You can track these rates in Resend in[the Metrics dashboard](https://resend.com/metrics) .


If you go above these thresholds, you are likely damaging your domain reputation and risk being suspended by your email service provider.


To keep rates low:


- Remove inactive user email addresses from email lists.
- Give users the option to[unsubscribe from your emails](https://resend.com/docs/dashboard/audiences/managing-unsubscribe-list) .
- Only send to recipients who have[given proper consent to receive email](https://resend.com/blog/how-to-properly-get-email-consent) .
- When testing, avoid sending to fake email addresses. Use Resend’s[test email addresses instead](https://resend.com/docs/dashboard/emails/send-test-emails) .
- If you are using open/click tracking, periodically remove recipients who are not engaging with your emails from your email lists.
- [Use webhooks](https://resend.com/docs/dashboard/webhooks/introduction) to remove addresses that bounce or block your emails.


### Avoid Emailing Too Frequently


If you appear to be engaging in suspicious or manipulative behavior, your sender reputation score will suffer. Sometimes, that behavior may be as simple as sending too many emails to too many people.


Especially if your domain is new or if you're planning on increasing sending, make sure you[warm up your domain](https://resend.com/blog/how-to-warm-up-a-new-domain) before sending at volume.


As a general rule, send mass marketing emails sparingly. Getting the rate or distribution wrong can look like you’re trying to overwhelm filters and get in front of recipients at all costs.


### Don’t Treat All Recipients the Same


Email is a great way to build your brand or connect with your audience, but if you’re not intentional, it may have the opposite effect.


Too many unsubscribe requests are bad, but being marked as spam is worse. If you're sending[Broadcasts](https://resend.com/features/broadcasts) , Resend can[manage your unsubscribe lists](https://resend.com/docs/dashboard/audiences/managing-unsubscribe-list) automatically. For transactional emails, add[unsubscribe links when it makes sense](https://resend.com/docs/knowledge-base/should-i-add-an-unsubscribe-link) .


[Customize your unsubscribe page Add your branding and logo to your unsubscribe page. resend.com/docs/dashboard/settings/unsubscribe-page](https://resend.com/docs/dashboard/settings/unsubscribe-page)


Always prioritize recipients’ choices to maximize your reputation. As a start, always obtain specific consent for each type of email and send to your most engaged recipients first.


If you make it hard to unsubscribe, recipients will mark your hard work as spam instead.


## 3. Ignoring Your Email Open Rate


Not all sending should[track open rates](https://resend.com/docs/knowledge-base/why-are-my-open-rates-not-accurate#does-open-tracking-impact-inbox-placement%3F) , but if you are tracking opens, pay close attention to your open rates. A low email open rate can negatively impact your sender reputation. If recipients aren’t opening your emails, your score will go down.


But a low reputation can also contribute to the problem of open rates. If your sender reputation prevents you from getting good inbox placement, fewer people will open your emails. It’s a feedback cycle that can drive both your sender reputation and open rate lower over time.


Monitoring your email open rate is a valuable source of information and a potential way to improve your sender reputation score. This may not be as simple a fix as some of the other issues we’ve identified here, but the payoff for improvements is significant.


Better inbox placement, catchier subject lines, and more insights from analytics tools all help you maximize the value of your emails.


## 4. Missing or Misused DMARC Policies


DMARC is an industry standard for email authentication that helps prevent phishing and spoofing attacks. It’s an essential part of your[email toolbox](https://resend.com/docs/dashboard/domains/dmarc) , but misusing it can be as damaging to your reputation score as not having it at all.


Without DMARC, attackers can easily send emails that appear to come from your domains. DMARC certifies that emails from[your domains and subdomains](https://resend.com/blog/how-dmarc-applies-to-subdomains) are legitimate, and that the contents haven’t been altered. ESPs and MBPs value DMARC highly in their sender reputation algorithms. Going without it is costly.


Excessively *strict*[DMARC policies](https://resend.com/blog/dmarc-policy-modes) can cause almost as much trouble. If your DMARC settings are too stringent, even minor compliance issues can lead to your emails being blocked or marked as spam.


It can seem odd for a security standard to lower your sender reputation score, but that’s exactly what happens if you set aggressive DMARC policies without proper testing. When your policy is too strict, it can block your own sending, signaling you are untrustworthy.


To start, opt for the least restrictive settings and monitor carefully. Once you feel confident your internal compliance is in order,[you can start tightening your policies to focus on outside threats](https://resend.com/blog/dmarc-policy-modes#what-to-expect-when-stepping-up-enforcement) .


*→[Learn how to implement DMARC with Resend](https://resend.com/docs/dashboard/domains/dmarc)*


## Avoiding Mistakes Is Step One—What’s Next?


Your recipient lists are validated. Your emails are authenticated with DMARC. You’re selective about how many to send and to whom. However, you still haven’t achieved your desired delivery rate and open rate.


There are additional steps you can take to enhance your sender reputation score, delivery rate, and open rate. Avoiding basic mistakes is a good place to start, but taking proactive steps to improve deliverability will help you build[even better email campaigns](https://resend.com/blog/top-10-email-deliverability-tips) .
