---
schema_version: "1.0.0"
document_id: "daba044c2435fbae6e6a86aa4a3d182c8bd5652243571538471c9964653b6356"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/sunsetting-new-email"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:af95bea188b85fa2aedb33b4b2de0d70802b9be392592dff62be5d98a2765980"
---

# Sunsetting new.email

Today we're announcing that we're **sunsetting[new.email](https://new.email/)** .


The product will remain available for you to export your templates until **August 7, 2026** . After that, new.email will **no longer accept new prompts or sends** . If you need help migrating, you can[reach out to us](https://resend.com/contact) .


I wanted to take a moment to talk about why we're doing this, what we would do differently next time, and what we would do exactly the same.


## Why we built it


Every day, product teams need to send emails to their users. Some teams have a developer to build their templates. Many depend on marketing, product, or design. The result is often **a mix of emails that don't look or feel like the rest of the product** .


We believed emails should be **consistent, beautiful, and responsive** , regardless of who builds them.


In February 2025,[we announced new.email](https://resend.com/blog/introducing-new-email) , a tool where anyone could create email templates using **natural language** , powered by[React Email](https://react.email/) components and the deliverability lessons we learned from sending millions of emails every day.


At that time, Claude Code and Codex didn't exist. People were using GPT-4 and Sonnet 3.5, and those models weren't as good as they are today for email creation.


The core bet was right. But we also made two decisions early on that made sustaining the product harder than it needed to be.


## What we would do differently


### Lesson 1: New brands require a ton of investment


We launched new.email with a **completely separate identity** . New name, new domain, new logo, new visual language.


This was intentional. We thought we were targeting a different audience than Resend, so a different brand made sense on paper.


In practice, **maintaining one high-quality brand is already a lot of work** , especially with a small team. Every brand needs its own website, its own docs, its own social presence, its own design decisions, and its own care.


In retrospect, we would **fold everything into the Resend brand from day one** . A different audience doesn't necessarily require a completely different identity.


### Lesson 2: A separate stack is fast to launch and slow to live with


Because we went with a separate brand, we also built new.email on a completely **separate tech stack** . Separate repo, separate infrastructure, separate deployment.


This was also intentional. It let us **move fast without touching the core product** , and it worked. We shipped quickly.


The cost showed up after launch. Different pipelines, different conventions, different operational knowledge. Over time, maintaining and evolving new.email became harder than building it.


**Speed at launch, paid for in maintenance.** If we did it again, we'd build inside the same codebase and accept a slower first week for a faster next year.


## What we would do the same


Some decisions we'd make again, exactly the same way.


### Lesson 3: Move fast on new problems


We were early to this space, and being early gave us a tremendous advantage. We got to learn what AI-generated emails should feel like before it was obvious, and those lessons are now baked into Resend.


I hope we keep doing this: **building new projects, and shutting them down if needed** . Sunsetting new.email doesn't mean the experiment failed. It means we're still trying novel things and still willing to be wrong in public.


The day we stop launching things that might not work is **the day we stop learning anything new** .


### Lesson 4: Let the market decide


From day one, we faced a nagging question: **how often do people need to create an email template?**


We took inspiration from Lovable, Bolt, and v0, but our hunch was that **emails are not like websites** . You don't create and update an email template every week. You build a handful of templates once, and then send them many times.


While we could have killed the idea before it started, we shipped new.email and let the market decide. It turns out the market showed us people didn't need new templates constantly, but I'd rather **test an assumption against reality** than debate it in a meeting room.


Opinions are good. Untested opinions are guesses.


## What happens to the ideas


The most important things we built for new.email are not going away. **They're moving into Resend** , where they should have lived all along, for free to all users.


The Resend email editor now does what new.email set out to do, and more:


- You can give it a URL and it will **extract your brand** into a ready-to-send draft.
- You can select any block and ask the AI to **rewrite or improve it** .
- You can have LLMs **review your content** before you send.
- Your own agent can create and edit emails through the[Resend MCP server](https://resend.com/mcp) .


**One product, one brand, one stack.**


## Thank you


Thank you to everyone who prompted, tested, and shipped emails with new.email.


You showed us **the demand was real** , and you continue to shape the Resend editor today.


new.email was **the right idea in the wrong place** . **Now it's home.**
