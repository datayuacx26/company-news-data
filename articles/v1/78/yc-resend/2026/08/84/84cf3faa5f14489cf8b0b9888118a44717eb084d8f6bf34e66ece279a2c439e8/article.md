---
schema_version: "1.0.0"
document_id: "84cf3faa5f14489cf8b0b9888118a44717eb084d8f6bf34e66ece279a2c439e8"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/apple-branded-mail"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T13:15:56.682552+00:00"
fetched_at: "2026-08-06T13:15:58.922422+00:00"
content_hash: "sha256:338fbd82668018c6d15fddd9063b7ade11a0145fea236f6138f98632422b13ec"
---

# How to Show Your Logo in Apple Mail

Capturing attention in the inbox is difficult, and yet for many companies it's key to success. One of the quickest ways to get attention is to **display your logo in the inbox** , which has several additional benefits:


- Company **recognition** and **authority**
- Recipient **trust** and **credibility**
- Visual **appeal** vs generic logo


And in fact,[recent studies](https://www.businesswire.com/news/home/20210720005361/en/Red-Sift-and-Entrust-Survey-Showing-a-Logo-Positively-Affects-Consumer-Interaction-With-Emails-Open-Rates-Buying-Behavior-Brand-Recall-and-Confidence) show meaningful benefits of displaying your logo in the inbox:


- Increases brand recall by **18%** .
- Improves open rate by **21%** .
- Boosts purchase likelihood by **34%** .
- Reinforces confidence in email by **90%** .


## The problem


While no one system can guarantee logo placement in the inbox, the typical approaches are **inconsistent and expensive** .


Almost every email provider has its own way of adding a profile picture to an inbox:


- [Gmail account settings](https://resend.com/docs/knowledge-base/how-do-i-send-with-an-avatar#gmail)
- [Outlook profile settings](https://resend.com/docs/knowledge-base/how-do-i-send-with-an-avatar#outlook)
- [Yahoo account settings](https://resend.com/docs/knowledge-base/how-do-i-send-with-an-avatar#yahoo)


The cross-platform standard,[BIMI](https://resend.com/docs/dashboard/domains/bimi) , requires a paid certificate, an expensive yearly subscription, and a trademark for the most complete coverage.


## Why Apple Branded Mail


In 2024, Apple shipped[Branded Mail](https://www.apple.com/newsroom/2024/10/apple-expands-tools-to-help-businesses-connect-with-customers/) in iOS 18.2. It's a proprietary Apple format that displays your logo as an avatar in the inbox of Apple Mail.


Apple Mail is **more important than you may realize** . According to[the email client market share report](https://www.litmus.com/email-client-market-share) studying over 1 billion opens, Apple Mail accounted for **64.66% of email opens** , followed by Gmail at 24.11%. While Apple open rates are[likely inflated due to prefetching](https://datainnovation.io/en/apple-mpp-email-open-rate-fix/#:~:text=senders%20with%20Apple%20Mail%2Ddominant%20audiences%20saw%20reported%20open%20rates%20climb%2018%2D32%20percentage%20points%20above%20verified%20engagement%20benchmarks) , they still represent a significant portion of the email market.


If you can display your logo in Apple Mail, you will likely cover a sizable market share of your recipients. It doesn't require a trademark or a certificate, and it's **free to set up** .


## The prerequisite: DMARC


Apple shows your logo only if your domain properly authenticates your emails. To ensure your logo appears with Apple Branded Mail, set your DMARC policy to either p=quarantine; or p=reject;.


```text
v  =  DMARC1  ;   p  =  quarantine  ;   rua  =  mailto  :  dmarcreports@example  .  com
```


If your DMARC policy is set to` p=none` , start with our guide on[DMARC policy modes](https://resend.com/blog/dmarc-policy-modes) to move your policy to` p=quarantine` or` p=reject` safely.


[DMARC Policy Modes Learn how to set up DMARC policy modes to protect your domain from spam and phishing. resend.com/blog/dmarc-policy-modes](https://resend.com/blog/dmarc-policy-modes)


## How to set up Apple Branded Mail


Setting up Apple Branded Mail is a quick process.


Prefer watching a video? Here's a quick walkthrough.


The bulk of the setup for Apple Branded Mail is done in[Apple Business Connect](https://www.apple.com/business/connect/) .


- **Create an account** with your company name and address.
- **Add your brand.** Open Branded Mail and enter your brand name and website.
- **Upload a logo.** At least **1024 x 1024 px** , as a` .png` ,` .heif` , or` .jpeg` .
- **Pick your domains.** If you set up a logo on your root domain, it also covers subdomains that don't have a logo set up.
- **Prove your identity.** U.S. companies use their Federal Taxpayer Identification Number. Others can use their local equivalent, plus a DNS verification record.


Verification takes **up to seven business days** . A real reviewer checks your brand's identity, which helps reinforce trust.


[Read the full setup guide](https://resend.com/docs/knowledge-base/how-do-i-set-up-apple-branded-mail) for more details.


## If Apple Branded Mail isn't enough


For many brands, Apple Branded Mail is a great solution. It's a free, easy way to show your logo in Apple Mail, and it's a great way to build trust with your customers and surface your brand in Apple inboxes.


However, if you need a more robust solution, implement **BIMI** . It's the industry standard and a great investment for brands that send a lot of emails and need to ensure that their emails appear consistently across all clients.


To learn more about implementing BIMI,[read our setup guide](https://resend.com/docs/dashboard/domains/bimi) .
