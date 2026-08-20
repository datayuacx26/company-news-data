---
schema_version: "1.0.0"
document_id: "3a176be72798b41a23f4fc6a3621d7b69e41303dcd757588e3c734c0d310a0aa"
company_key: "yc-intuned"
company: "Intuned"
source_id: "yc-intuned-news-import-ba3d72b83236"
canonical_url: "https://intunedhq.com/blog/run-webwright-scripts-in-the-cloud-with-intuned"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-25T09:50:45.873485+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:72675a7c03b4508e78a404dff5fbe498271e005969e5a4d6ca52a80702514426"
---

# Run Webwright scripts in the cloud with Intuned

Webwright does something genuinely useful: give it a task and it writes a working, re-runnable browser script. Running that automation locally is fine while you build it and for one-off tasks. The moment you want it called externally, run it on a schedule, or share it with your team, you need a home to run it, not a script on your laptop. The[webwright-to-intuned](https://github.com/Intuned/skills) skill helps you make that jump: your webwright craft becomes a hosted API, with the logic you wrote left alone.


## A craft is a script. Production needs a service.


A Webwright craft nails the hard part: figuring out the page and writing code that works. It just stops at the edge of your machine. The day something else has to depend on it, you need more than a script:


- **Your app needs to call it.** A script on your laptop has no endpoint.
- **It needs to run unattended.** On a schedule, and able to shrug off a slow page instead of failing the run.
- **It needs to get past bot defenses.** Stealth, proxies, and CAPTCHA solving aren't part of a local Playwright run.
- **Its output needs to reach other systems.** A JSON file in a run folder isn't something your stack can consume.


## How the skill helps


The` webwright-to-intuned` skill gives your coding agent the steps to port a Webwright craft into an Intuned project: create the` automation(page, params)` function, scaffold the project, map the parameters, verify the run locally, and deploy through the Intuned CLI.


Point your agent at the craft folder Webwright produced (the one with` final_script.py` and` plan.md` ), and the skill walks it through the rest. One craft function in, one hosted API out.


From craft to verified cloud run: scaffold, port, local run, deploy.


## What you get


The same craft, now a service:


- **A callable API** your app, a cron job, or a teammate can trigger.
- **Automatic retries** so an unattended run survives a flaky load.
- **Stealth mode, CAPTCHA solving, and proxies** you can turn on for sites that block automation.


The deployed run returning live results: the last 7 days of TechCrunch startup news.


## Your code stays your code


None of this touches what you wrote. It's a **port, not a rewrite.** Take a craft that scrapes[the last 7 days of TechCrunch startup news](https://github.com/Intuned/skills/blob/main/examples/webwright-to-intuned/real/techcrunch-startup-news/craft/final_script.py) . The skill drops the parts that only made sense on your laptop (the browser it launched for itself, the screenshots, the local file write), swaps the one line of navigation, and keeps your selectors and filtering exactly as written:


The local-only plumbing comes out; the craft logic stays intact.


## Try it


1. **Install the skill:**` npx skills@latest add Intuned/skills --skill webwright-to-intuned`
2. **Set up Intuned:** create a free[Intuned](https://intunedhq.com/) account and install the CLI (` npm i -g @intuned/cli` ).


Bring a Webwright craft, run the skill, and your working automation becomes a service the rest of your stack can use.
