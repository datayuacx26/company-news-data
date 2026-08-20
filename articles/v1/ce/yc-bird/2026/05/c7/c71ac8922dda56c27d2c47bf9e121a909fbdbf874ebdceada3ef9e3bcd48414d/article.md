---
schema_version: "1.0.0"
document_id: "c71ac8922dda56c27d2c47bf9e121a909fbdbf874ebdceada3ef9e3bcd48414d"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/how-to-warm-up-a-new-email-domain"
published_at: "2026-05-25T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:fae1dbdfc39efd332ca83cc0d0e8758a004578584549c596d1f521b7e949b8fe"
---

# How to Warm Up a New Email Domain

Warming up a new email domain means building its sending reputation gradually, by starting with low volume to your most engaged recipients and increasing over time as mailbox providers learn to trust you. A brand-new domain has no track record, and to a filter, no history reads as risk. Send a large burst from a cold domain and providers will throttle, defer, or junk it. Ramp slowly and let positive signals accumulate, and you earn the room to send at full volume.


This is about the sending domain itself, the reputation attached to where your mail comes from. It is closely related to IP warmup, but not the same thing, and the difference matters.


## Why does a cold domain get filtered?


Mailbox providers decide what to trust based partly on reputation, a running assessment of how recipients react to your mail. A new domain has none of that history. When you suddenly send a high volume from it, the pattern looks like what spammers do: spin up a fresh domain, blast, and disappear before reputation catches up. Providers respond defensively by limiting how much they accept and where they place it.


Warming up replaces that absence of history with a record of healthy sending. Each batch that gets opened, read, and not reported teaches the provider that mail from your domain belongs in the inbox.


## How do you warm up a domain?


The mechanics are simple; the discipline is in the pacing. The principles below hold regardless of your exact volumes:


- **Start small.** Begin with a modest volume that a provider will comfortably accept from an unknown sender, then grow from there.
- **Lead with your most engaged recipients.** Send first to the people most likely to open and click: recent sign-ups, active customers, anyone who has shown interest. Positive early engagement is the strongest signal you can send.
- **Increase steadily.** Raise volume in measured steps rather than one jump. Let each step settle and confirm the signals look healthy before the next increase.
- **Spread across providers.** Your audience spans Gmail, Outlook, Yahoo, and others, each with its own reputation history for you. Warming evenly means you build trust everywhere rather than overwhelming one provider.
- **Watch bounces and complaints at every step.** A rising bounce rate or a complaint spike is a signal to slow down, not push through. For the bounce distinction, see[what are bounced emails](https://bird.com/en-us/blog/what-are-bounced-emails) . If anything looks off, hold volume steady or step back until it recovers.


A note on numbers: resist the urge to follow a rigid day-by-day schedule copied from somewhere. The right pace depends on your total list size, your engagement, and how each provider responds to you. Think in principles and ranges, increase when the signals are healthy, and slow down when they are not.


## How is this different from IP warmup?


Domain reputation and IP reputation are separate, and you may need to warm both.


- **Domain warmup** builds trust in the domain your mail comes from. This matters for every sender, whether you are on shared or dedicated infrastructure.
- **IP warmup** builds trust in the specific IP address sending the mail. It applies mainly when you are on a dedicated IP, where the address has no history of its own. On shared infrastructure, you inherit an established IP reputation, so the IP side is largely handled and you focus on the domain.


If you are moving to a[dedicated IP](https://bird.com/en-us/products/email/dedicated-ips) , you typically warm the IP and the domain together, following the same gradual ramp. The IP-specific steps are in[the IP warmup guide](https://bird.com/en-us/docs/guides/email/ip-warmup) , and the records that establish your domain are in[the sending domains guide](https://bird.com/en-us/docs/guides/email/sending-domains) .


## Plan the structure before you warm


Decide your subdomain layout first, because each sending subdomain warms on its own and you only want to do this once per lane. Separating marketing and transactional streams onto distinct subdomains is standard practice; the reasoning is in[email subdomain best practices](https://bird.com/en-us/blog/email-subdomain-best-practices) . Warmup is one piece of the broader picture in[email deliverability best practices](https://bird.com/en-us/blog/email-deliverability-best-practices) , and if mail is already getting filtered,[why emails go to spam](https://bird.com/en-us/blog/why-are-my-emails-going-to-spam) covers diagnosis.


## FAQ


### How long does warming up a domain take?


There is no universal timeline. It depends on your list size, how engaged your recipients are, and how each provider responds. The honest answer is that you warm until the signals (delivery, opens, low bounces, low complaints) hold steady at your target volume. Rushing it sets you back further than going slow.


### Do I have to warm up if I send low volume?


If your volume is genuinely small and steady, a cold domain may ramp naturally without a formal plan. The need for a deliberate warmup grows with volume. When in doubt, start conservative and lead with engaged recipients regardless of size.


### What happens if I send too much too soon?


Providers respond by deferring or rejecting mail and placing more of it in spam, and the early reputation damage makes the rest of the ramp harder. If you see bounces or complaints climbing during warmup, hold or reduce volume until they recover rather than pressing ahead.


Warming a domain is patient work, but it sets the ceiling for everything you send afterward. Bird's[dedicated IP](https://bird.com/en-us/products/email/dedicated-ips) and domain tooling support the gradual ramp, so you can grow volume on signals instead of guesswork.
