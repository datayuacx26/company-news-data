---
schema_version: "1.0.0"
document_id: "220d967eb57c0d1d79aa411828d392713bda443acfde14fd022064927cdf16fb"
company_key: "yc-halluminate"
company: "Halluminate"
source_id: "yc-halluminate-news-import-9819d7d93bdb"
canonical_url: "https://www.halluminate.ai/blog/webarena-vs-web-bench"
published_at: null
first_seen_at: "2026-08-11T04:50:25.359940+00:00"
fetched_at: "2026-08-11T04:50:26.871958+00:00"
content_hash: "sha256:e507efeed5a045865e28ab99958021600e87c73622ed355ef4b3e290633282d5"
---

# WebArena vs Web Bench

[WebArena](https://webarena.dev/) and[Web Bench](https://www.halluminate.ai/blog/benchmark) get compared because both evaluate browser agents on “realistic” websites. The resemblance ends there.


WebArena asks: *Can an agent complete long-horizon intents in a frozen, self-hosted world that looks like shopping, Reddit, GitLab, and maps?*


Web Bench asks: *Can that same class of agent read and write on today’s public internet—through logins, captchas, proxies, and UI churn?*


Those are both important. Treating a WebArena win as a live-web win is how teams ship demos that die in production.


## The numbers people look up


WebArena Web Bench


Sites Self-hosted clones of a few site families **452** live websites in the open set


Tasks **812** long-horizon intents **~2,454** open tasks (from ~5,750 feasible)


Grading Programmatic functional checks Human trajectory review in the launch study


Strength Reproducibility and paper comparison Real-web difficulty, including writes


Weakness Misses captcha, proxy, and day-to-day drift Sites change; some tasks go stale


**WebArena task count:** 812. That is the usual answer to “how many tasks does WebArena have?”


## What WebArena got right


Before WebArena, web-agent papers were hard to compare. Everyone had a different toy site or a different scrape of the live web. Zhou et al. shipped Dockerized environments with real open-source software underneath, authentic-scale content, and outcome-based grading: did the repository update, did the cart contain the right item—not “did the agent emit the golden click path.”


That design is why WebArena became the default academic yardstick. If you are ablating memory, observation format, or planner structure, you want that kind of control.


## Where controlled clones stop being enough


When we ran agents on live sites for[Web Bench](https://www.halluminate.ai/blog/benchmark) , a different failure distribution showed up:


- Proxy and geo blocks
- Captcha / bot detection mid-trajectory
- Login and authentication friction that does not exist in a lab account model
- UI and content changes that invalidate tasks between dataset creation and eval


None of that is WebArena’s fault. It is simply out of scope. A benchmark that freezes the world cannot measure hostility from the world.


We also care about **writes** . Web Bench tags tasks as READ, CREATE, UPDATE, DELETE, and file manipulation. Launch results were lopsided: strong agents often cleared **>70%** of READ tasks, while the best fully automated NON-READ score was **Skyvern 2.0 at 46.6%** . Overall fully automated SOTA was **Anthropic Computer Use at 66.0%** . Extraction looking “solved” while form-heavy workflows remain half-broken is exactly the gap product teams feel—and exactly what a read-skewed live suite can hide.


## How to use both without lying to yourself


A pattern that works:


1. Develop and ablate on WebArena (or BrowserGym / WorkArena for enterprise UI).
2. Train in simulators when you need RL volume—[Westworld](https://www.halluminate.ai/blog/westworld) exists for that reason.
3. Gate releases on[Web Bench](https://webbench.ai/) if the agent will touch the public web.
4. If live scores collapse, run[BrowserBench](https://www.halluminate.ai/blog/browserbench) before you rewrite the planner. Infrastructure alone moved Web Bench accuracy by large margins in our study.


WebArena success means the agent can think through realistic UIs. Web Bench success means that skill still works when the website is allowed to fight back.


For the broader map of evals, see[Browser agent benchmarks in 2026](https://www.halluminate.ai/blog/browser-agent-benchmarks) . Full Web Bench methodology and charts live in the[launch post](https://www.halluminate.ai/blog/benchmark) .
