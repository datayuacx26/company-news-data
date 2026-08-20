---
schema_version: "1.0.0"
document_id: "a6e734ee381ef0179db4feaca94872b22fde5b6019de33997baa95275feddcdd"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/bux-launch-blog"
published_at: "2026-04-25T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:29.258885+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:d5cf92b114cf3fe1a12ef4fcf19891b60b2955d93da74dbd4c5a2d9d43d25f61"
---

# BUX: Your 24/7 Remote Agent with Browser Harness

## Introducing BUX


Today we're shipping Browser Use Box (BUX). A 24/7 remote VM with Claude Code and[Browser Harness](https://browser-use.com/posts/bitter-lesson-agent-harnesses) pre-installed, controlled from Telegram, the web, or SSH.


## How we got here


Everyone at Browser Use is obsessed with the combination of Claude Code and Browser Harness. However, we wanted the experience to be as seamless as possible.


Now with BUX you can access this combination from your phone 24/7.


## What we use it for


It can complete tasks in complicated dashboards like Microsoft Azure and Google Workspace that browser agents historically struggled with.


From the gym, in an Uber, walking somewhere, we throw any task at it:


> "deploy PR #235 to staging and test the new flow"
>
>
> "book the earliest flight Zurich to SF next Wednesday"
>
>
> "take a picture of our granola, order this on Amazon"


We are yet to find a work-related or everyday-web task that BUX can't do.


## How it works


Spin-up takes 30-60 seconds from a pre-baked AMI with Claude Code and Browser Harness pre-installed. Login to Claude, set up your Telegram, and you've got a 24/7 personal agent.


The VM runs under a locked-down IAM role with zero AWS permissions. It can't touch your infra or ours.


Telegram talks to one resumed Claude Code session, so context persists. Browser sessions rotate every 240 minutes.


On[cloud.browser-use.com/bux](https://cloud.browser-use.com/bux) you can access your VM directly through a browser terminal. And SSH in locally. The architecture is open source. See the[bux repo](https://github.com/browser-use/bux) if you're curious, or you'd like to host it yourself.


## Setup (under 5 min)


1. Go to[cloud.browser-use.com/bux](https://cloud.browser-use.com/bux) and start your VM.
2. Log in to Claude Code in the web terminal.
3. Create a Telegram bot via @BotFather and paste the token. It stays in your box.


## Giving it access


BUX is only as useful as the accounts it can reach. Use[Browser Use Profiles](https://cloud.browser-use.com/profiles) to sync your local Chrome cookies or build new ones by hand.


When the agent hits a login wall, it sends you a live URL from the remote browser. You sign in, it picks up where it left off, and saves the cookies for next time.


## Want it set up for a team?


For teams that already run sales, support, partner onboarding, or operations in Telegram, we are testing a managed BUX pilot: one scoped private operator workflow, launched in 7 days, with weekly tuning and human handoff for $1,000/month.


See the[managed pilot page](https://browser-use.github.io/bux/pilot.html) or the[demo transcript](https://browser-use.github.io/bux/managed-pilot-demo.html) .


## Try it


Head to[cloud.browser-use.com/bux](https://cloud.browser-use.com/bux) . All you need to get started is a Browser Use account and Claude Code.
