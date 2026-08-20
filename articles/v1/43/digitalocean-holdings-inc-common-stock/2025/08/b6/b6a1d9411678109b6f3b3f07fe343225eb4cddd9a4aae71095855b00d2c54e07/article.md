---
schema_version: "1.0.0"
document_id: "b6a1d9411678109b6f3b3f07fe343225eb4cddd9a4aae71095855b00d2c54e07"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/seanotes-saas-starter-kit"
published_at: "2025-08-21T14:20:21.584+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:a6c03cea1ecc7f4fadd0fe551c224d084a20872de12e1428bcbf90c0f2087f95"
---

# Stop Building SaaS from Scratch: Meet the SeaNotes Starter Kit

There’s a moment in every SaaS project where you realize…


**You’re not building your product yet.**


You’re setting up auth. You’re wiring up Stripe. You’re figuring out how to send emails, where to store files, how to deploy it — and now, how to sprinkle in just enough AI to make it feel modern.


Even in 2025, LLMs still struggle with this part. They’re great at scaffolding UI and generating business logic. But they don’t know how to spin up a database, integrate Stripe, or deploy an actual app.


**That’s exactly what SeaStack solves.**


[SeaStack](https://getseastack.dev/) is a new series of open-source starter kits and reference apps from DigitalOcean — built to help developers ship real apps, faster.


It’s our way of saying: “ *Here’s how you can build real things with DigitalOcean — and here’s the source code to get started.* ”


And **SeaNotes** is the first one.


- [Live Demo](https://notes.getseastack.dev/)
- [View the GitHub Repo](https://github.com/digitalocean/sea-notes-saas-starter-kit)


## What is SeaNotes?


SeaNotes SaaS Starter Kit is an open source GitHub repo that gives developers a **simple, production-ready foundation** to build real SaaS apps — fast.


It’s a full-stack notes app that comes pre-wired with real services most SaaS apps need:


- ✅ User Auth with[NextAuth.js](https://nextauth.js/)
- ✅[Stripe](https://stripe.com/) billing (upgrade, cancel, customer portal)
- ✅ File uploads using DigitalOcean[Spaces](https://www.digitalocean.com/products/spaces)
- ✅ Transactional Email via[Resend](https://resend.com/)
- ✅ PostgreSQL database[hosted on DigitalOcean](https://www.digitalocean.com/products/managed-databases-postgresql)
- ✅ AI features powered by DigitalOcean’s[Gradient AI™ Platform](https://www.digitalocean.com/products/gradient/platform)
- ✅ One-click deploy to DigitalOcean[App Platform](https://www.digitalocean.com/products/app-platform)


## Who It’s For


SeaNotes is built for:


- Indie hackers
- Solo developers
- Early-stage startup teams
- Developers validating new ideas
- Anyone looking to build fast with best practices baked in


## It Solves the Right User Stories (So You Don’t Have To)


SeaNotes handles the foundational features most SaaS products need:


- ✅ Sign up and log in
- ✅ Verify email and reset password
- ✅ Log in via magic link or password
- ✅ Upload a profile image
- ✅ Upgrade or downgrade billing plans via Stripe
- ✅ Generate invoices
- ✅ Generate AI-powered content using[Gradient AI™ Platform](https://www.digitalocean.com/products/gradient/platform)
- ✅ Create notes — and if you skip the title, the AI generates one automatically
- ✅ Start with a clean, simple notes UI


All of this is baked in — so you can **skip the glue work** and focus on building your actual product.


## Works Great with LLMs


You can use SeaNotes in two ways:


1. **Starter Kit** – Clone your own fork of the repo, build your business logic on top, and launch.
2. **Reference App** – Point Claude, ChatGPT, or Cursor at the codebase and say:


*“Build me something like this… but for customer support tickets.”*


*“Rewrite this to manage bookings instead of notes.”*


*“Add a feature that lets users tag notes and filter by tag.”*


Because the infra is already handled, LLMs can focus on **your logic** , not boilerplate setup.


## Complete Feature Set


### 🧾 Billing and Invoice Generation


### 🧑‍💼Admin Dashboard


## 🧠 Generate a note with Gradient Serverless Inference


## 👤User Profile Settings


### System Status


There’s even a built-in[/system-status](https://notes.getseastack.dev/system-status) page to show you what’s working and what’s not.


## Try It Out


If you’ve been meaning to build a SaaS app — this should make it easier.


Everything’s set up: auth, billing, email, storage, AI, deployment. You can use it as a starting point or just see how things are wired together.


### Get Started


The safest way to work with SeaNotes is to fork it first, then clone your fork locally. That way, all your changes live in your own repo.


**Step 1:** Fork the repo to your own GitHub account (button in the top right on GitHub)


**Step 2:** Clone your fork locally:


git clone[https://github.com/](https://github.com/) /sea-notes-saas-starter-kit.git cd sea-notes-saas-starter-kit/application


**Step 3:** Install dependencies


```text
npm install


```


**Step 4:** Start local PostgreSQL with Docker (-d runs it in the background)


```text
docker-compose up -d


```


**Step 5:** Prepare the database schema


```text
npx prisma generate
npx prisma migrate deploy


```


**Step 6:** Run the app locally


```text
npm run dev


```


- [Live Demo](https://notes.getseastack.dev/)
- [GitHub Repo](https://github.com/digitalocean/sea-notes-saas-starter-kit)
- [One-Click Deploy](https://cloud.digitalocean.com/apps/new?repo=https://github.com/digitalocean/sea-notes-saas-starter-kit/tree/main)


We’ll be iterating on this and releasing more kits as part of SeaStack — so if there’s something you’d like to see baked in, or a service you wish we’d integrate next, let us know.


Feedback, ideas, feature requests — all welcome.


We can’t wait to see what you *ship* !


### About the author(s)


Amit Jotwani


Author


Developer Educator


[See author profile](https://www.digitalocean.com/community/users/amitjotwani)


Amit is a Developer Advocate at DigitalOcean 🐳, where he helps developers build and ship better apps on the cloud. Compulsive Seinfeld quoter. LEGO nerd. 🧱 AMA.


[See author profile](https://www.digitalocean.com/community/users/amitjotwani)


Haimantika Mitra


Author


Engineer & Writer


[See author profile](https://www.digitalocean.com/community/users/haimantika)


A Developer Advocate by profession. I like to build with Cloud, GenAI and can build beautiful websites using JavaScript.


[See author profile](https://www.digitalocean.com/community/users/haimantika)
