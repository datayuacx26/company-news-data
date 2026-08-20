---
schema_version: "1.0.0"
document_id: "6eb18ffbf8a873fef3ef85cd20479580b3a7a341d53c2d4ae11c891af7615b0b"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-sherlock-depot-ci-analysis"
published_at: "2026-05-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:13:26.363881+00:00"
content_hash: "sha256:2d5e0b10cc9ba67066cbbbf0a1b5a9581aee4cacf80d35e844f6eb6ef01e04f4"
---

# Now available: Sherlock can analyze Depot CI workflows and jobs

We previously taught our AI assistant Sherlock how to analyze container builds, GitHub Actions jobs, CI analytics, and project settings.[Depot CI](https://depot.dev/docs/ci/overview) now has the same dashboard-aware experience: when you're looking at a workflow or job, Sherlock can fetch the relevant context and help you figure out what happened without copying IDs, logs, or screenshots into chat.


## Why we built this


Depot CI already gives you logs, CPU and memory metrics, job summaries, retry attempts, and a handy AI failure diagnosis in the dashboard. That context is useful, but debugging still often means moving between a workflow summary, a job page, or an external AI tool to get the information you need.


Sherlock can help when you want an explanation that pulls those pieces together. It can start from the workflow or job you're already viewing and use the available Depot CI data to investigate.


## What Sherlock can analyze


For Depot CI workflows, Sherlock can fetch run context, workflow metadata, other workflows from the same run, job statuses, timing, current attempts, and errors.


For Depot CI jobs, Sherlock can fetch job metadata, workflow context, attempts, step details, job summaries, CPU and memory metrics, failure diagnosis summaries, and error log lines. When more detail is needed, Sherlock can pull logs for a specific step.


That means you can ask questions like:


- Why did this Depot CI workflow fail?
- What changed between attempts?
- Which step failed, and what should I try next?


## Failure diagnosis follow-up


Depot CI failure diagnosis cards give you a short explanation of what went wrong and a suggested next step. When you want more detail, click **Ask Sherlock** from a failure diagnosis card.


Sherlock analyzes the selected workflow or job, including the failing steps, related logs and metrics, and concrete fix suggestions. This is useful when the diagnosis card points you in the right direction, but you want to discuss the reasoning and surrounding context, or get a more complete repair path.


## How to use it


Open a Depot CI workflow or job in the Depot dashboard, then ask Sherlock about what you're viewing using the **Ask AI** button in the left sidebar. You can also click **Ask Sherlock** from a failure diagnosis card to start with the selected failure.


Sherlock uses the current workflow or job as context, and you can keep asking follow-up questions in the same conversation.


## Pricing


Sherlock's Depot CI analysis is included with Depot at no additional charge. Depot CI usage is billed as usual according to[Depot pricing](https://depot.dev/pricing) .


## Limitations


Sherlock is AI-powered, so you should still double-check its answers before making changes. It can only analyze Depot CI data your account is allowed to access, and log analysis depends on the logs and metrics available for the selected workflow or job.


If Sherlock gives you an answer that is confusing, incomplete, or wrong, send your feedback our way through[support](https://depot.dev/help) or our Discord server. Happy building!


## Related posts


- [Now available: Sherlock can analyze your builds and CI](https://depot.dev/blog/teaching-sherlock-new-tricks)
- [Now available: Depot CI](https://depot.dev/blog/now-available-depot-ci)
- [Introducing Sherlock: AI assistant for Depot docs](https://depot.dev/blog/introducing-ai-assistant-sherlock)


Pedro Guerra


Enterprise Support Lead at Depot
