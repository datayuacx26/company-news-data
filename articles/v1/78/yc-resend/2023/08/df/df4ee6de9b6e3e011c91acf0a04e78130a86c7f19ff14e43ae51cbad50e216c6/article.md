---
schema_version: "1.0.0"
document_id: "df4ee6de9b6e3e011c91acf0a04e78130a86c7f19ff14e43ae51cbad50e216c6"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/email-authentication-a-developers-guide"
published_at: "2023-08-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:1c25ab743f190be22a6263646693ef83032156b4e3136ea9c267509a91f35287"
---

# Email Authentication: A Developer's Guide

Proper email authentication can be the difference between **reaching the human or the spam folder** , but it is often overlooked or misunderstood.


Think of your **emails as a startup getting into a competitive accelerator program** .


## SPF (Receiving Applications)


Competitive startup programs will receive tens of thousands of applications. Their first step is to see which of these applications can be thrown out without being considered.


[SPF (Sender Policy Framework)](https://www.open-spf.org/) is similar. It's the first triage of the emails coming to an inbox, checking to make sure that each email should even be considered for delivery.


The DNS record for SPF declares a list of origins (servers) that are allowed to send email for this domain, and the inbox will confirm that the message they received matches one of them. If a server isn't on the list, it's like an application being tossed out because it wasn't fully filled or the business idea is illegal.


Every domain or subdomain can only have one SPF policy, and policies on the root/apex domain (domain.com) are **not applied to subdomains** (sub.domain.com).


Your SPF policy, specified in a TXT record, probably looks like this:


Diagram explain SPF record


```text
v=spf1 include:_spf.google.com include:amazonses.com ~all


```


- **v=spf1** : The version of SPF
- **include:_spf.google.com** : Allows Google servers to send emails on your domain
- **include:amazonses.com** : Allows AWS servers to send emails on your domain
- **~all** : The policy which tells the server what to do if the SPF check fails


When a mailbox receives a message from you, it will look at the **Return-Path** in the email header and expects it to map back to one of the origins specified in the record.


## DKIM (Application Vetting)


If the application passes that initial check, then the vetting process begins to make sure all the claims the applicants made are true.


[DKIM (DomainKeys Identified Mail)](https://dkim.org/) plays a similar role to confirm the legitimacy of the message by adding a signature on each message that verifies the email sender is who they say they are.


DKIM is set with a private/public key pairing.


1. You set a public key in your DNS records (usually a CNAME or TXT record)
2. Each email you send includes a DKIM signature
3. When an inbox receives your message, it compares the signature with the public record to confirm a pair


Especially as your company becomes more well known, there are more incentives for **hackers to send an email as if it is from you** .


The DKIM, like a strong login password, is an essential way to prove who you are by providing information only you know.


It is common to have multiple DKIM records, usually one or more per email provider.


## DMARC (Selection Policy)


What if an applicant fails one of these steps? How should their application be handled?


[DMARC (Domain-based Message Authentication, Reporting & Conformance)](https://resend.com/docs/dashboard/domains/dmarc) is **the selection policy** . It sets rules for what happens if an applicant lies on an application (DKIM) vs. not demonstrate enough traction (SPF). For email, DMARC establishes your policy as a sender for what should happen to your messages if they fail DKIM or SPF.


You would likely have one DMARC policy set for your entire domain, including subdomains, in a TXT record that looks like this:


Diagram explain SPF record


```text
v=DMARC1;p=quarantine;pct=100;rua=mailto:dmarc@domain.com


```


- **v** : The version of DMARC
- **p** : What the mailbox should do (policy) if SPF or DKIM fails (none, quarantine, reject)
- **pct** : The percentage of failed messages that should be affected by the policy.
- **rua** : A valid inbox where the providers should send their DMARC reports


Implementing DMARC, particularly with a policy of **quarantine** or **reject** , enhances your domain's reputation. This is because inbox providers can rely on your commitment to prevent the delivery of suspicious messages, thereby improving their user experience within the inbox.


Check out our full guide on[how to set up DMARC](https://resend.com/docs/dashboard/domains/dmarc) .


## BIMI (Exclusive Access)


Making it into a startup accelerator is an amazing feat, but if you want to be exceptional, you need to gain the attention of the industry leaders and pioneers. There are no hacks or shortcuts to this, you simply need to prove yourself.


[BIMI (Brand Indicators for Message Identification)](https://resend.com/docs/dashboard/domains/bimi) is this kind of access in the inbox. It sets you apart from all the others by showcasing your brand and legitimacy to your users in the inbox by displaying your logo and, in some cases, a verified checkmark.


With over 347 billion emails sent every day, this is an exceptional way to stand out.


BIMI in the inbox


Obtaining BIMI is exclusive because of the long, hard process it takes to complete the verification. Here are a few things you need:


- **DMARC** : The DMARC policy must be at quarantine or reject and at 100%
- **Trademarked Logo** : The logo you want to showcase must be trademarked
- **VMC** : The certificate which verifies your identity, domain, and trademark


Diagram explain BIMI record


```text
v=BIMI1; l=https://vmc.digicert.com/00-00.svg; a=https://vmc.digicert.com/00-00.pem;


```


- **v** : The version of BIMI
- **l** : The location of the SVG logo
- **a** : The location of the Verified Mark Certificate (VMC)


Check out our full guide on[how to set up BIMI](https://resend.com/docs/dashboard/domains/bimi) .


## Delivery is the Goal


The good news is that **SPF and DKIM are handled for you when using Resend** . All you need to do is[add a domain](https://resend.com/domains) and we take care of the rest.


Ultimately, inbox providers aim to only show the emails their users want to see, and spoofed or compromised emails are not on the list.


Without these protocols, **they can't tell you from a spammer** .


Assure them you're legit, and they'll prioritize your emails. It's a win-win.
