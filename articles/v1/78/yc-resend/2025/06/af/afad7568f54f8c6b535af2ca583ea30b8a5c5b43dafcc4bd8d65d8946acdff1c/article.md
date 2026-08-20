---
schema_version: "1.0.0"
document_id: "afad7568f54f8c6b535af2ca583ea30b8a5c5b43dafcc4bd8d65d8946acdff1c"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/do-you-need-a-dedicated-ip"
published_at: "2025-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:047305dc48452eee0c778fee8626de78cbc68b132d161f73c2f46e33d8f12b77"
---

# Do You Need a Dedicated IP?

Many claim dedicated IPs are the **instant fix to deliverability problems** . Maybe you're wondering if they're the missing piece in your email strategy.


**But the reality is more nuanced.** Dedicated IPs have their place, but only for a particular kind of sender. For everyone else, they can do more harm than good.


In this post, we'll explore:


- why **domain reputation** is most important
- when dedicated IPs are **helpful**
- when dedicated IPs are **not helpful**
- **how to decide** what’s right for *your* sending goals


## What is a dedicated IP?


A dedicated IP is a unique IP address assigned to your email domain. No other computer or server on the internet can share the same IP address.


By default, Resend (and many other email providers) use shared IPs, but you can also request a dedicated IP.


## Domain vs. IP reputation: what's the real driver?


People tend to fixate on IP reputation because when it fails, it fails loudly—increased bounce rates, scary SMTP responses, and a sense that everything's broken. Mailbox providers, however, care more about your domain reputation.


True IP blocks are rare, temporary, straightforward to fix, and usually impact very little mail. When domain reputation goes wrong, on the other hand, it's often more ambiguous and complicated to fix. Learn more about[why emails go to spam](https://resend.com/blog/why-your-emails-are-going-to-spam) and how that impacts your deliverability.


Your sending behavior and domain history do the heavy lifting and are much more important factors than the IP address you use.


**IP filtering is less valuable today**


With the rise of IPv6 and senders frequently changing email providers or rotating IPs, the sending IP has become a less helpful signal for filtering.


**Why domain reputation is more critical**


Unlike IP reputation, your domain reputation reflects *you* , not your infrastructure. It's shaped by what you send, how people engage with it, and how reliably you follow best practices (like setting up SPF, DKIM, and DMARC, maintaining low bounce and complaint rates, and driving positive engagement).


Domain reputation also follows you across providers, platforms, and IPs.


## When a dedicated IP makes sense


If domain reputation is the most critical metric, does your IP no longer matter? Sometimes, a dedicated IP is the right tool for the job.


A dedicated IP makes sense when:


1. **High volume:** You send hundreds of thousands of emails per month, and that volume is consistent week to week.
2. **Predictable patterns:** You send on a regular cadence (e.g., daily or weekly) without significant gaps or sudden spikes.
3. **Deliverability expertise:** You understand IP warming, list hygiene, engagement metrics, and how to avoid common pitfalls.


In these cases, a dedicated IP gives larger, more experienced senders greater control. While it can make troubleshooting easier when things go wrong, you're also fully responsible for building and maintaining your IP reputation from scratch.


Resend helps you[manage warming up a dedicated IP](https://resend.com/docs/knowledge-base/warming-up) and monitoring your performance.


## When a shared IP is a better choice


For many senders, a shared IP is the better option. Here are some signs it might be the right fit:


1. **Moderate volume:** You send a relatively small or moderate number of emails.
2. **Inconsistent patterns:** Your traffic isn't always steady. Some days are busy, others are quiet.
3. **Reputation still growing:** You're in the early stages of building trust with mailbox providers.
4. **Prefer shared responsibility:** You don't want to manage deliverability entirely on your own.


Shared IPs give you a head start. You benefit from the established reputation of other trusted senders, which can lead to better inbox placement from day one.


A shared IP can also protect you from the challenges of warming up an IP on your own. Warming requires gradually increasing your volume and maintaining strong engagement to build trust with mailbox providers. That's a big lift if you don't have the volume or infrastructure to do it properly.


At Resend, we've built our shared IP pools with high-quality senders in mind. We actively monitor performance, enforce best practices, and remove bad actors. Even if you're just getting started, you'll benefit from that trusted sending environment.


## What's the right choice for you?


Ultimately, the right choice depends on your sending habits, goals, and familiarity with email best practices and troubleshooting.


A high-volume SaaS platform sending daily product updates might thrive on a dedicated IP. A startup sending a monthly newsletter to a few thousand users? You are probably better off on a shared IP.


What matters most is **building and maintaining a solid domain reputation** , following[best practices](https://resend.com/docs/knowledge-base/audience-hygiene) , and understanding your audience. Your IP address is just one piece of the puzzle.


## Need help? We've got you.


Resend's[Support team](https://resend.com/support) includes email deliverability experts with decades of combined experience.


Whether you're trying to decide between shared vs. dedicated IPs, planning your warmup strategy, or just want to make sure you're set up for long-term success, we're here to help.
