---
schema_version: "1.0.0"
document_id: "d60d0a988e7b7072b29a9e4d39a78c795355f100552a9a3df2f5e4bc7524824c"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/introducing-durable-ui/"
published_at: "2026-02-16T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T22:20:15.340641+00:00"
content_hash: "sha256:b4ff428c6d0900d7fa69bde186119344a0cda59ed30a69a07e79844ea04fe7d6"
---

# Introducing Durable UI: The 30% That Vibe Coding Can't Teach You

Open any app you’ve built with AI in the last 6 months. Now do this:


1. Apply some filters on a list page
2. Hit refresh
3. Copy the URL and paste it in a new tab


If the filters are gone and the URL shows nothing — you’ve built a **Vibe UI** . It demos well. It falls apart the moment a real user touches it.


I’ve been shipping production apps with Sails.js and Inertia for years —[Sailscasts](https://sailscasts.com/) ,[Hagfish](https://hagfish.io/) , and others — and I keep running into the same class of bugs. In every UI I interact with — vibe coded or not, made by a solo builder or a big engineering team. The bugs are always the same:


- Filters reset on refresh. Users apply 4 filters, hit F5, start over.
- URLs say` /dashboard` and nothing else. Can’t share. Can’t debug. Can’t reproduce.
- The back button destroys form progress. Four steps of input, gone.
- Sidebars reopen on every page load. The user closes it. It reopens. A loop that should not exist.
- Dropdowns don’t close when you press Escape or click outside them.


These aren’t edge cases. This is the **default output** of AI-generated UI code. And it’s getting worse, not better.


## The Decision AI Can’t Make


[CodeRabbit analyzed thousands of pull requests](https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report) : AI co-authored code has **1.7x more major issues** and **up to 2.74x more security vulnerabilities** .[16 of 18 CTOs](https://www.finalroundai.com/blog/what-ctos-think-about-vibe-coding) reported production disasters from AI-generated code.


Why? Because AI is excellent at generating code and terrible at making **placement decisions** .


An LLM generates a` ref()` in milliseconds. What it cannot do is answer: *“Given this piece of state, where should it live?”*


That single question — state placement — is the difference between a UI that demos well and one that survives real users. And no course anywhere teaches it. I checked Frontend Masters, Egghead, Udemy. Every state management course teaches a *library* (Redux, Pinia, Zustand). Nobody teaches the *decision* .


## What Durable UI Is


**Building Durable UIs in the Era of Vibe Coding** is a new course on Sailscasts. It teaches the engineering judgment that sits between “the code works” and “the product survives.”


The core framework is the **State Placement Map** :


State Type Where It Lives Example


Shareable URL query params Filters, search, pagination


User preference localStorage Sidebar state, theme, column order


Session Memory Form draft in progress, modal open


Server truth Database / API User data, invoices


Derived Computed, not stored Filtered list, totals


One question drives every decision: **“If someone else clicks this link, should they see this state?”**


- Yes → URL.
- Persist across sessions, not shareable → localStorage.
- Ephemeral → memory.
- Source of truth → server.
- Calculable from other state → derived. Don’t store it.


You’ll build real composables during the course —` useDurableUrl()` ,` useDurableStorage()` ,` useFormDraft()` ,` useWizardDraft()` ,` useClickOutside()` — tools that don’t exist anywhere else in the Vue + Inertia ecosystem. The patterns behind them translate directly to React hooks, Svelte stores, or Angular signals.


```text
// Vibe UI
const   filters   =   ref  ({})   // gone on refresh. can't share. invisible.


// Durable UI
const   filters   =   useDurableUrl  (  'status'  , {
default: [],
server:   true  ,
})   // survives refresh. shareable. debuggable.
```


The course also ships with a **Durable UI skill for Claude Code** . Install it, and when you ask Claude to add filters or a sidebar, it makes the right placement decision by default. The course and the skill evolve together.


## Start Testing Today


You don’t need the course to start. Run these five checks on any app you’re building right now:


**1. The URL Audit.** Open every page in your app. Does the URL encode the view state? If` /dashboard` looks different depending on filters but the URL doesn’t change — that’s a durability bug.


**2. The Refresh Test.** On every page with interactive state, hit refresh. Whatever resets is state in the wrong place.


**3. The Shareability Test.** Copy the URL and open it incognito. Does the other person see what you see? If not, you have shareable state trapped in memory.


**4. The Back Button Contract.** Navigate away from any form. Hit back. Is the user’s work preserved?


**5. The Cleanup Check.** Open a dropdown. Navigate away without closing it. Come back. Is the listener still attached?


These five checks surface 80% of durability issues in any app. The course teaches you how to fix every one of them — and how to build so they never happen in the first place.


## Early Access Is Open


The first 1,000 **Founding Vibe Coders** get the course at **$59.99** (one-time, lifetime). Nigerian developers can pay **₦29,500** via Paystack. Price goes up to $99.99 after that.


11 chapters. 60 episodes. All composable source code. A Claude Code skill. Released weekly.


You don’t need to use The Boring JavaScript Stack. The mental models — State Placement, Shareability Test, Back Button Contract — work in any framework. Vue is used for examples. The thinking is universal.


**[Get Early Access →](https://sailscasts.com/courses/durable-ui)**


*“If your UI can’t survive a refresh, it’s not real.”*


---


**Next read:**[Durable UI Patterns](https://blog.sailscasts.com/durable-ui-patterns) — the 8 patterns behind durable interfaces, with code examples and a checklist you can use today.
