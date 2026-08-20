---
schema_version: "1.0.0"
document_id: "4e2d7f7c20aeed6b72b7c9b9ae29b95dbe24ed1fac0f10888e9c5b7af5907b9d"
company_key: "yc-intuned"
company: "Intuned"
source_id: "yc-intuned-news-import-ba3d72b83236"
canonical_url: "https://intunedhq.com/blog/introducing-the-new-intuned"
published_at: "2026-04-04T00:00:00+00:00"
first_seen_at: "2026-07-25T09:50:45.873485+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:195f7145578784c6134f600e341b0427ebba495aa3b5b0288e6b932f07431c56"
---

# Introducing the New Intuned

We've been working in browser automation for over two years now. We've talked to hundreds of companies, worked closely with dozens, and built more scrapers and RPA workflows than I can count.


Today we're releasing the New Intuned — a code-first browser automation platform with an AI agent that builds and maintains your automations as deterministic, production-ready code. This is the biggest release we've ever done, and we're excited to share what we've built and how we got here.


## The gap


If you've ever built browser automations — scrapers, RPA, anything that controls a browser programmatically — you know how much is involved. The scripts are tedious to write, fragile to maintain, and break constantly as websites change, and then there's everything you need around it to run reliably in production. It adds up.


Recently, AI agents and LLM-backed tools have made it possible to navigate websites and extract data in real time — which is great for one-off tasks. But for repetitive automations that run regularly, at scale, you need the speed, predictability, and low cost that only code gives you.


That's the gap the new Intuned fills.


The gap between AI agents and code-based automation


## How it works


Intuned has two parts: a platform for deploying and running code-based browser automations, and an AI agent that can build and maintain them.


### Intuned Agent


Intuned Agent doesn't just write browser automation code — it understands the platform and works within it. Describe what you need in a conversation and the agent builds a complete Project, validates it against the live site, and delivers production-ready code you can review, edit, and deploy. It can also edit existing projects, and fix failed runs by pulling error context and traces to diagnose what went wrong.


You can also enable self-healing on any project — turning it into an autonomous project that closes the loop between monitoring and fixing. When something changes — a website updates its layout, response sizes shift, data patterns look off — Intuned detects it, determines whether it's a real issue, and the agent analyzes what went wrong and writes a fix. The level of autonomy is fully configurable — you control how much the agent does on its own.


Watch the agent[build a scraper](https://youtu.be/Es390qwW87w?si=LcyPw2EWxeYJpXrt) ,[create an RPA](https://youtu.be/D1Uf42OS1cs?si=dFuae0A8aMQwszcC) ,[fix a failed run](https://youtu.be/dPqyOy-brOE?si=xAaIFIIH5HqETExm) , or[self-healing in action](https://youtu.be/22IYRA-ZqQk?si=OX3etrMgjy8wXViE) .


How self-healing works


### Platform


You write browser automations in TypeScript or Python, organize them into Projects, and deploy to Intuned. Run them on demand via API or on a schedule.


Websites fight back against automation — CAPTCHAs, bot detection, IP blocking. Intuned handles that with built-in stealth mode, proxies, and CAPTCHA solving. For sites that require login, AuthSessions manage authentication and session state across runs.


When you need to run at scale, Jobs handle batching, retries, concurrency, and result delivery. Every run gets full observability — logs, browser traces, and session recordings.


Develop in the browser-based IDE or locally with the CLI.


Intuned HomeJobs, runs, and traces


## What you can build


**Scrapers.** Extract structured data from websites on a schedule — product prices, job listings, public records, news. Write the extraction logic yourself or let the agent build it.


**RPA automations.** Interact with websites on behalf of users — form submissions, data entry, report downloads. Essential when a service doesn't offer an API.


**Crawlers.** Discover and collect data across many pages. Follow links, parse sitemaps, index content at scale.


**AI-powered automations.** Combine deterministic code with LLM-driven interactions. Use frameworks like Browser Use or Stagehand alongside your automation code, all running on Intuned's infrastructure.


## Why we built this


One thing we heard over and over from customers: nobody wants to write and maintain browser automation code. Many asked us to just do it for them as a service. We offered that — and it became one of the most popular things about working with us.


Over the past two years, we've processed millions of automation runs for teams across e-commerce, insurance, healthcare, government, and more. The patterns are the same everywhere: critical work that depends on browser automation, and engineers who'd rather be doing anything else.


That's what the agent replaces. The same work, done by AI — faster, cheaper, and available to everyone.


## Try Intuned


Sign up for free and start building at[app.intuned.io](https://app.intuned.io/) , or[book a demo](https://cal.com/team/intuned/get-started) if you'd like a walkthrough. To go deeper, head to our[docs](https://docs.intunedhq.com/) , watch the[demos](https://www.youtube.com/playlist?list=PLpfAYMc-7VcpdtAmwLIpbYyu6ZaNdxVPC) to see the agent in action, or read about how[Intuned compares to other approaches](https://intunedhq.com/docs/main/01-learn/deep-dives/intuned-vs-others) .
