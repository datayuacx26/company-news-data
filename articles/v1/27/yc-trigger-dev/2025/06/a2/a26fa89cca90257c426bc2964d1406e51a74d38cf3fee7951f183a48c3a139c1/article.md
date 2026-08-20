---
schema_version: "1.0.0"
document_id: "a26fa89cca90257c426bc2964d1406e51a74d38cf3fee7951f183a48c3a139c1"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/self-hosting-trigger-dev-v4-docker"
published_at: "2025-06-19T12:00:00+00:00"
first_seen_at: "2026-07-22T17:13:09.827889+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:56f6a15b86d40f5c6394082c4a1f211e162496d648d4bbc0be0d7fb8c122d4ca"
---

# Self-hosting Trigger.dev v4 using Docker

With v4, we took everything we learned from the v3 self-hosting experience and rebuilt the process to be as smooth as possible. Our goal: you shouldn’t need to be a DevOps expert just to get up and running. If you want to run Trigger.dev yourself, it should feel approachable,[well-documented](https://trigger.dev/docs/self-hosting/overview) , and reliable.


This post focuses on self-hosting with Docker, which is the easiest way to get started. If you’re interested in Kubernetes, we’ll have a guide for that soon as well.


In this post, I'll walk you through:


- Why you might want to self-host
- What’s new in v4 for self-hosters
- What to expect
- A brief self-hosting overview


## Why self-host Trigger.dev?


For most people, I think I can safely say that using[Trigger.dev Cloud](https://cloud.trigger.dev/) is the best experience. It's fully managed, scales automatically, and you get dedicated support. But sometimes, you need more control.


### When does self-hosting make sense?


- You have strict compliance requirements (e.g. data residency).
- You want to run Trigger.dev in an air-gapped or private environment.
- You're building something that needs deep integration with your own infra or custom limits.


### When doesn't it?


- You don’t want to manage updates, scaling, or security patches.
- You want the fastest path to production and don’t want to manage infra.


### Cloud vs. self-hosted v4:


Here's a quick comparison of the features available in Cloud and self-hosted v4. I go into more detail below.


Feature Cloud Self-hosted


Warm starts ✅ ❌


Auto-scaling ✅ ❌


Checkpoints ✅ ❌


Dedicated support ✅ ❌


Community support ✅ ✅


ARM support ✅ ✅


## What’s new in v4 for self-hosting?


#### Major changes from v3 to v4:


- No more custom startup scripts – just simple Docker Compose commands.
- Built-in registry and object storage, so you don’t need to wire up S3 or GCS unless you want to.
- Simpler, more robust worker management.


#### Setup is easier:


- The Docker Compose setup is streamlined. Fewer moving parts, less YAML to wrangle.
- Environment variables for the[webapp](https://trigger.dev/docs/self-hosting/env/webapp) and[supervisor](https://trigger.dev/docs/self-hosting/env/supervisor) are better documented and more consistent.


#### Horizontal scaling for workers:


- You can now scale out workers easily by adding more containers.
- Resource limits and security are improved – less risk of noisy neighbors.


## The self-hosting experience: what to expect


Running Trigger.dev yourself gives you a lot of flexibility, but it also comes with new responsibilities and trade-offs. Here’s what you can expect if you go the self-hosted route:


#### You’re responsible for:


- Provisioning and maintaining infrastructure (servers, storage, networking).
- Handling updates, security patches, and scaling.
- Monitoring uptime and performance.


#### What’s provided out of the box:


- A ready-to-run Docker Compose setup.
- Built-in registry and object storage.
- All the core features of Trigger.dev, minus a few Cloud-only perks.


#### What’s not included:


- Managed scaling.
- Warm starts.
- Checkpoints (non-blocking waits), so long waits use more resources.
- You’re on your own for infra, but the[Discord community](https://trigger.dev/discord) is active and helpful.


#### Premium support:


- We offer enterprise support for self-hosters.[Get in touch](https://trigger.dev/contact) if that's something you're interested in.


## Self-hosting overview


Self-hosting Trigger.dev v4 is more approachable than ever, but it does ask for a bit more hands-on involvement. Here’s what you should know before you dive in:


### What you’ll need


- **Modern infrastructure:** A server or VM with Docker and Docker Compose. Linux is preferred for production, but you can experiment locally on macOS or Windows.
- **Database:** Postgres is included in the default Docker Compose stack, but you can use your own (self-hosted or managed) if you prefer.
- **Email provider:** For magic link logins, set up SMTP credentials using a provider such as[Resend](https://resend.com/) .


### Planning for scale


- Start small, but plan for growth. You can add more worker containers as your workload increases.
- For production, consider splitting webapp and workers across different machines or VMs for reliability.
- Monitor resource usage and be ready to adjust CPU/memory as needed.


### Best practices & gotchas


- **Version locking:** Always pin your Docker images to a specific version tag to avoid surprises when updating.
- **Environment variables:** Double-check your .env file – misconfigurations are the #1 source of setup pain. For local testing, things should work out of the box with the provided defaults.
- **Email setup:** Test your magic link email flow early to avoid login headaches. You can check the webapp logs for any errors when no email config is setup.
- **GitHub auth:** You can now use GitHub authentication as an alternative to email.
- **Security:** Don’t expose your webapp or registry to the public internet without authentication. Use strong secrets and keep everything patched.


### Operating & upgrading


- Scaling is as simple as adding more worker containers.
- Upgrades are straightforward: update your image tags and restart your containers.
- If you ever feel like you’re spending more time on ops than building, remember you can always move back to[Trigger.dev Cloud](https://cloud.trigger.dev/) .


## Get started with self-hosting


- Follow the[Self-hosting Docker guide](https://trigger.dev/docs/self-hosting/overview) to get started.
- Join the[Discord self-hosting channel](https://trigger.dev/discord) for support.
