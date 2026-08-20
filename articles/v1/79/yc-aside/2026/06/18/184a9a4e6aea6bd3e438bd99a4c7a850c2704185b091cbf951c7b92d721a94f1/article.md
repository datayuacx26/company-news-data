---
schema_version: "1.0.0"
document_id: "184a9a4e6aea6bd3e438bd99a4c7a850c2704185b091cbf951c7b92d721a94f1"
company_key: "yc-aside"
company: "Aside"
source_id: "yc-aside-news-import-e4a720ca74ad"
canonical_url: "https://aside.com/blog/developers"
published_at: "2026-06-22T07:00:00+00:00"
first_seen_at: "2026-07-21T08:01:04.221364+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:556be8b0d37f47480225ce375441285309eea53d2b2d8920b56d0477245210dc"
---

# Aside for Developers

Your code review is blocked on evidence outside the repo: a failed CI run, a Datadog trace, a feature flag, and a staging page only your browser can open.


Ask Aside for that browser evidence from your coding workflow. It uses the real browser, keeps working while you stay in the editor, and returns log links, screenshots, files, and a reviewable note.


**Use Aside when the work leaves your editor:** Keep code changes in your coding agent. Use Aside for CI pages, private dashboards, staging screenshots, docs, and the browser evidence a reviewer needs.


## CLI, MCP, and REPL for private browser evidence


Use the CLI when the browser check belongs inside a development workflow instead of a separate chat. Use MCP when your coding agent needs evidence from private browser pages, such as a CI log, feature-flag dashboard, Datadog trace, internal admin page, or staging screenshot.


Use the REPL when you need deterministic browser inspection. It feels like Playwright: you get` page` , tabs, locators, screenshots, downloads, and JavaScript. The difference is that Aside's REPL also gives you the same page snapshots and stable element refs that the browser agent uses, so a click or assertion lines up with what Aside will actually act on.


```text
aside "Open the failed GitHub Actions run for this PR, find the first failing step, compare it with the last passing run, and draft a PR comment with log links. Do not post it."


```


```text
aside "Open localhost:3000, run through signup, capture desktop and mobile screenshots, and write a short QA note."


```


```text
aside repl


```


```text
aside mcp


```


Those interfaces are for the same job: getting browser evidence back into developer work without pasting private dashboard content into a separate chat.


Private evidence


Use Aside from my coding agent to inspect the private CI run, feature-flag dashboard, Sentry issue, and staging page for this PR. Return the first failing log link, rollout state, screenshots, and a PR comment draft. Do not post or change settings.


## Failed CI run triage


Google's SRE book defines toil as work that tends to be manual, repetitive, automatable, tactical, low in lasting value, and likely to scale as the service grows[1](https://sre.google/sre-book/eliminating-toil/) . Failing CI checks are a clean example: the work is browser-heavy, evidence-based, and usually ends with a comment or issue note.


Aside can open the failing run, identify the first failing step, compare it with the last passing run, and draft a PR comment with log links. It waits for you before posting.


Output What the developer checks


Failing job Job name, first failing step, and log URL


Diff against last green run Dependency, env var, test, or command that changed


PR comment draft Short diagnosis, links, and one suggested next step


CI failure review


Open the failed CI run for this PR, identify the failing job, compare it with the last passing run, and draft a comment with links to the relevant logs. Do not post it.


Example comment draft:


```text
The first failure is checkout-web / Playwright mobile, step 14.
Last green run: 74210391. Current failing run: 74288420.
The failing trace shows the payment iframe never reached ready state after the env var rename in 8f31c9a.
Suggested next step: restore NEXT_PUBLIC_STRIPE_KEY in preview or update the deployment secret before rerun.


```


## PR evidence packet before merge


Before merge, reviewers often need evidence that lives outside the diff. Aside can open the Linear issue, GitHub PR, failed or passing checks, preview deployment, Sentry issue, feature-flag dashboard, and staging page. It brings back proof for the reviewer.


The packet should include log links, screenshots, rollout state, known risks, and anything that needs a reviewer decision.


PR evidence packet


Build a PR evidence packet before merge. Open the Linear issue, GitHub PR, latest checks, preview deployment, Sentry issue, feature-flag dashboard, and staging page. Capture screenshots, log links, rollout state, and known-risk evidence. Do not post or change settings.


## Feature flag audit


Before a flag moves from 10% to 50%, ask Aside to find the internal rollout doc, open the dashboard, compare the current value against the plan, and stop before changing anything.


Flag audit


Find our internal docs for feature flags, then open the flag dashboard and check whether the new checkout flag matches the rollout plan. Stop before changing any flag.


## Debug context from browsing history


Atlassian's 2025 Developer Experience research says developers spend 16% of their time coding, while top time wasters include finding information, adapting to new technology, and switching between tools[2](https://www.atlassian.com/blog/developer/developer-experience-report-2025) . Aside can handle the "where did I see that fix?" part of the job.


Debug context


Search my browsing history for the docs page I used to debug OAuth redirects last week. Reopen the page, capture the relevant section, and add the source links.


## Staging QA screenshots


Visual QA often means opening the app, resizing the browser, clicking through a flow, saving screenshots, and writing a short note. Aside can do that browser pass while the developer keeps working in the editor.


Staging QA


Capture screenshots from staging for the onboarding flow at desktop and mobile widths. Save the images and write a short QA checklist.


## Skills for team-specific browser work


Skills let a team write down browser procedures that should not live in every prompt. A skill can tell Aside how your release checklist works, which dashboards matter during rollout, how to inspect an internal admin page, or what counts as a valid QA screenshot.


Skills help when the task has team context: "use the release skill," "follow our staging QA skill," or "check this incident using the Datadog triage skill." The developer still reviews the output, but the browser agent starts with the right local rules.


Use a skill


Use our staging QA skill for this PR. Open the preview URL, run the signup flow, capture desktop and mobile screenshots, and report any selector, layout, or console issues. Stop before filing tickets.


## Monitoring pages that need human judgment


Some incidents start as a dashboard nobody wants to babysit. Aside can watch Datadog, Sentry, Grafana, Cloudflare, or a custom admin page, then wake the task when the signal changes. The note should name the monitor that changed, the time window, the log or trace link, the likely blast radius, and the action it left for a person.


Datadog monitor


Watch the Datadog dashboard for the checkout service for the next hour. If error rate stays above 2% for five minutes, capture the dashboard screenshot, open the related logs and traces, identify the top failing endpoint, and stage an incident note. Do not page anyone or change settings.


## Complex setup and browser-only debugging


Some developer tasks fail because the state lives in a product console, not in code. Cloudflare DNS setup, Supabase CPU spikes, a custom billing admin, or a staging feature flag may require opening several private pages and comparing settings.


Have Aside inspect those pages, download logs or CSVs, screenshot the current state, and return the exact steps it would take. Keep production changes behind review.


Console setup


Review our Cloudflare DNS setup for the new docs domain. Open Cloudflare, the deployment provider, and the internal setup doc. Compare the expected records with the live records, capture screenshots, and stage the exact changes needed. Do not save changes.


Supabase CPU


Investigate the Supabase CPU spike. Open Supabase metrics, recent query stats, deployment logs, and Datadog traces. Capture the time window, top suspicious queries, related deploy, and source links. Do not change database settings.


## Copy a web animation into your app


When you see an interaction you like, Aside can inspect the live page, capture the animation frames, read computed styles, and bring the pattern back to your code task. Have it describe the motion, timing values, screenshots, and implementation notes your coding agent can adapt.


Animation reference


Inspect this pricing-page card animation in the browser. Capture screenshots before, during, and after hover, record timing, easing, transform, opacity, and layout changes, then write an implementation note for our React component. Do not copy site code.


## Turn checks into Routines


Use Routines for browser checks that developers repeat. After a release cut, Aside can reopen the rollout dashboard every 30 minutes, capture the error-rate screenshot, compare it to the release checklist, and stop if a threshold looks wrong. For monitoring work, a Routine can keep checking the same Datadog view and wake the task only when the log pattern or dashboard threshold changes.


**Routine candidates:** Long CI runs, deploy rollout checks, Datadog threshold watches, staging smoke tests, weekly dashboard summaries, docs drift checks, and release checklist reviews.


## Boundary for developer work


Aside fits the work that leaves your editor. It can open the failing CI run, read the linked issue, check the rollout dashboard, inspect Datadog logs, capture the staging screenshot, and bring back the log link, screenshot folder, PR comment draft, or rollout mismatch note. Keep the code change in your coding agent.


## Questions developers ask


###


###


###


###


###


###


###
