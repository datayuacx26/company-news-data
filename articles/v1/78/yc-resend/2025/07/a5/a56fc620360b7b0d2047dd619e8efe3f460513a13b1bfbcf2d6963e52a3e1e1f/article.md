---
schema_version: "1.0.0"
document_id: "a56fc620360b7b0d2047dd619e8efe3f460513a13b1bfbcf2d6963e52a3e1e1f"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/dmarc-policy-modes"
published_at: "2025-07-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:0fda80eeb2e9a79df1f7943976b391ab9a938571f4fba5b9248436df5016fb42"
---

# DMARC Policy Modes

One of the most malicious types of abuse is called email spoofing where bad actors make it look like their email was from a trusted source.


To combat this abuse, email inbox providers developed protocols to help domain owners protect their email domains from spoofing and phishing. These protocols are published as DNS records and include:


- **SPF:** a list of allowed IP addresses
- **DKIM:** a signature of the email to verify the sender's identity
- **DMARC:** a policy for what to do if the email fails SPF or DKIM checks


In this article, we'll focus on DMARC, paying special attention to the different DMARC policy modes—` none` ,` quarantine` , and` reject` .


**TLDR;** Unless you're confident in how DMARC, SPF, and DKIM work together and you’ve reviewed your domain’s sending sources, we recommend a basic` p=none` policy. It's the safest way to start getting visibility without risking legitimate mail disruption.


## Why you should care about DMARC


[Google, Yahoo](https://resend.com/blog/gmail-and-yahoo-bulk-sending-requirements-for-2024) , and[Microsoft](https://resend.com/blog/microsoft-bulk-sending-requirements-2025) have all announced requirements for bulk senders to publish a DMARC policy on their domains.


If you’re sending large volumes of mail—especially to consumers—you may *need* to enforce DMARC or risk your mail being marked as spam.


## A basic introduction to DMARC policy levels


DMARC (Domain-based Message Authentication, Reporting, and Conformance) policies are published as DNS records and tell receiving mail servers what to do with messages that fail authentication. There are three options:


- ` p=none` : no enforcement, just monitoring. This is your observation mode and is ideal for gathering data.
- ` p=quarantine` : treat unauthenticated mail as suspicious (i.e., often by filtering it to spam).
- ` p=reject` : outright block unauthenticated mail from being delivered (i.e., return a bounce message).


The real-world effects of these modes vary across providers and mailflows. Let’s take a closer look.


## If you publish p=none


Setting` p=none` means you're not asking mail receivers to take any action against unauthenticated messages. Instead, you’re just watching.


This mode is the right default for most senders. It's crucial for:


- Establishing a baseline of who’s sending on your behalf.
- Catching unauthorized or misconfigured senders without affecting delivery.
- Reviewing DMARC reports (via` rua` /` ruf` ) to identify issues before enforcement.


For most senders—especially those just starting out—this is the recommended starting point. It provides visibility without disruption.


## If you publish p=quarantine


When your domain has a` p=quarantine` DMARC policy, you’re telling receivers: “If this message fails DMARC, handle it cautiously, but don’t outright block it.” What that means in practice can vary.


Some possible outcomes:


- **Sent to spam** : Most mailbox providers (Gmail, Outlook, etc.) will route failing mail to the recipient’s junk or spam folder.
- **Held temporarily** : Some systems may delay or sandbox the message for additional analysis.
- **Subject to stricter filtering** : Messages might be scored more harshly by spam filters, increasing the chance they get flagged or dropped.


What’s important to remember: *quarantine doesn’t necessarily stop mail from arriving* , it just makes it more likely that it gets deprioritized or filtered.


It’s possible then for legitimate sources that fail DMARC to go unnoticed. The mail may still be delivered (albeit to spam), and unless the sender is watching closely, they may not notice the problem until users complain or performance dips.


## If you publish p=reject


Publishing` p=reject` is a much stronger stance. It tells recipients: *“Do not accept any mail from my domain unless it passes DMARC.”*


There are two main ways a reject policy is enforced:


1. **SMTP rejection** : The recipient refuses the message at the time of sending. This is the cleanest approach. Delivery fails immediately, and a bounce message can be generated.
2. **Post-SMTP blocking** : The message is accepted initially but discarded before reaching the inbox. This can cause silent drops, where no bounce is returned.


In both cases, the result is the same: mail that fails DMARC isn’t delivered. This is highly effective at blocking unauthorized mail, especially phishing attempts spoofing your domain.


However, if any of your legitimate senders are misconfigured or unaware of DMARC, their mail will get blocked, and you’ll hear about it quickly. A reject policy makes DMARC failures visible, loud, and immediate.


## What to expect when stepping up enforcement


If you're considering moving from` none` to` quarantine` or` reject` , it's worth planning carefully.


Here’s how the rollout often plays out:


- **With quarantine** , delivery issues are subtle. Legitimate-but-broken senders may keep sending, unaware their mail is landing in spam.
- **With reject** , issues are obvious. Mail bounces, people complain, and broken flows surface fast.


While the DMARC protocol includes the` pct` parameter for controlling the percentage of messages that should be authenticated, it is not widely followed by mailbox providers and this setting may not be respected or followed.


The beauty of DMARC is in the visibility it gives you. When used properly—with reporting enabled (i.e.,` rua` ,` ruf` )—you can identify and fix issues **before** enforcing stricter policies.


And if you're managing subdomains or want to understand how DMARC applies across them, we’ve covered that in our companion piece on[DMARC’s sp tag and subdomain behavior](https://resend.com/blog/how-dmarc-applies-to-subdomains) .


[How DMARC applies to subdomains Understanding how DMARC applies to subdomains is crucial for managing your email domain. resend.com/blog/how-dmarc-applies-to-subdomains](https://resend.com/blog/how-dmarc-applies-to-subdomains)


## Wrap-up


Both` quarantine` and` reject` are valuable tools for protecting your domain from abuse, but they come with tradeoffs.


Quarantine is a gentler middle ground, and often useful during the transition phase. Reject is definitive and offers the strongest protection—but only when you’re confident everything that should be sending on your behalf is properly authenticated.


DMARC was designed with gradual rollout in mind. If you take advantage of its reporting capabilities and iterate carefully, you can get to a strong enforcement policy with minimal pain.


For more help with DMARC, view our[DMARC docs](https://resend.com/docs/dashboard/domains/dmarc) or[reach out to our support team](https://resend.com/support) .
