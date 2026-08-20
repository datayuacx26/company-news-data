---
schema_version: "1.0.0"
document_id: "e8586a2a56ca9f94c0a53d0af48c34436faafbede7f20ac14ffafab7731d3ecd"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/how-to-warm-up-a-new-domain"
published_at: "2025-04-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:9ec3c322cc95da760a2ed04961d2754d24ce7eb78b410d5914cc9df3f3715e3a"
---

# How to warm up a new domain

When you start sending emails from a brand-new domain, you have to prove to inbox service providers that you're not a spammer.


As we launched our new product,[new.email](https://new.email/) , a specialty-trained LLM that helps create email templates, **we needed a plan to warm up this new domain** .


## Why do we have to warm up a domain?


When a domain with no sending history suddenly starts blasting out emails, inbox providers take notice.


Since they prioritize protecting their users from spam, they’ll often take a cautious approach, closely monitoring engagement signals like opens, clicks, and spam complaints.


If they don’t see positive engagement—or worse, if users start marking your emails as spam—your domain’s reputation takes a hit, making it harder or impossible for your emails to reach the inbox.


Think of it like building credit. At first, lenders don’t know if they can trust you, so you have to start small and establish reliability over time. Without this reputation, inbox providers might slow down your deliveries, send your emails straight to spam, or even block them entirely.


Since this is a topic that often concerns our customers, we wanted to transparently share our own experience warming up a domain that was critical to our business.


This guide focuses on brand-new domains. If you’re working with an existing domain, we also have some[general best practices](https://resend.com/docs/knowledge-base/warming-up) that apply to both new and established senders.


## What key questions should you ask?


Every good plan starts with a clear goal. The easiest way to approach domain warm-up is to work backwards from your target goals by asking key questions.


1. What volume will you be sending?
2. When do you need your reputation established?
3. Which subdomains to use?


Let's break down each of these.


### 1. What volume will you be sending?


For new.email, we planned to send at least **three types of emails** in varying volumes:


- account-related emails like signups and password resets
- in-app emails that users could generate and send to themselves
- marketing emails to our waitlist


We had an ever-growing waitlist, which meant a potential surge in volume for each type as new users gained access to the platform. In all, we wanted to plan on having the capacity to send **thousands of emails per day** .


### 2. When do you need your reputation established?


Our goal was to launch new.email on **March 24** .


Based on our experience and the expected email volume, we aimed for a six-week ramp-up period (starting on February 11) to **gradually increase volume** while staying within safe thresholds that inbox providers expect to see from new senders.


Generally speaking, the larger your target volume, the longer you should plan for warm-up. Since we were using an invite-based model, our initial volume was naturally small and gradually increased—perfectly mimicking an organic warm-up schedule.


### 3. Which subdomains to use?


We followed a best practice we always recommend: separating transactional, marketing, and product-generated emails onto different domains.


Subdomains help protect deliverability by ensuring that issues with one type of email (e.g., marketing campaigns) don’t negatively impact critical transactional emails.


In one way, however, this added a bit of complexity to our warm-up plan. We needed to warm up **three domains** instead of just one. Also, brand-new domains—those registered only recently—often face even more scrutiny because spammers frequently buy fresh domains and burn them out quickly.


## What strategies should you pursue?


Once you know your target volume and timeline, the next step is figuring out how to actually start sending emails from your new domains in a way that looks natural to inbox providers. Here’s what we did.


1. Use a double opt-in form
2. Use multiple subdomains
3. Gradually increase sending volume


### 1. Use a double opt-in form


To ensure that our first emails were landing in engaged inboxes, we made our early access signups double opt-in. This meant that when someone signed up, they received an email asking them to **confirm their address before being added to our list** .


This approach provided **multiple benefits** .


1. It eliminated fake or mistyped email addresses, keeping our list clean.
2. It ensured that every recipient was a real person who wanted to hear from us—exactly the kind of engagement signals inbox providers look for when evaluating a new sender.
3. We added email validation to prevent invalid or risky addresses from signing up in the first place, reducing the likelihood of bounces.


If you want to learn more about keeping your email list clean, check out our guide on[Audience Hygiene](https://resend.com/docs/knowledge-base/audience-hygiene) .


### 2. Use multiple subdomains


As mentioned earlier, we split our sending across multiple subdomains:


- one for transactional emails
- one for marketing
- one for product-generated messages


This wasn’t just for organization—it was a deliberate strategy to protect our reputation. If marketing emails ever ran into deliverability issues, our critical transactional messages wouldn’t be affected.


Inbox providers evaluate sending reputation at the domain and subdomain level. By keeping different email types separate, we reduced the risk of a single problem affecting all our emails. This setup did add complexity to our warm-up plan, but it was well worth it for long-term deliverability.


### 3. Gradually increase sending volume


We started with extremely small volumes and increased gradually. Initially, we invited early users via DMs on 𝕏 (formerly Twitter) to keep volume low and controlled.


As our warm-up progressed, we began sending marketing emails in progressively larger batches, focusing first on our most engaged audiences.


At the same time, every new account triggered a confirmation email, steadily increasing our transactional volume in a natural way.


## How you can prepare


If you're planning to warm up a new domain, here’s a simple checklist to follow:


1. Plan your target sending volume.
2. Set a clear deadline for when you need your reputation established.
3. Verify your domains as early as possible.
4. Make sure you’re sending to high-quality, engaged recipients.
5. Increase your sending volume gradually over time.
6. Monitor your deliverability metrics and adjust as needed.


Warming up a domain is a process, but it pays off by ensuring your emails reliably land in inboxes instead of spam folders. If you take the time to do it right, you’ll build a strong sender reputation that serves you well in the long run.
