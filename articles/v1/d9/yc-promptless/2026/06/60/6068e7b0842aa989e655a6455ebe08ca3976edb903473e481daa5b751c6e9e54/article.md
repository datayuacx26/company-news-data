---
schema_version: "1.0.0"
document_id: "6068e7b0842aa989e655a6455ebe08ca3976edb903473e481daa5b751c6e9e54"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/slite-context-source/"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-08-07T08:03:50.270603+00:00"
fetched_at: "2026-08-07T08:03:52.181710+00:00"
content_hash: "sha256:755ae21e313b6935e0f5d29682aa84b1a26d853bd2531ec4f8c81bf7c3342549"
---

# Connect Your Slite Knowledge Base to Promptless

# Connect Your Slite Knowledge Base to Promptless


[← Back to Blog](https://promptless.ai/blog)


Promptless reads your Slite workspace when it generates documentation suggestions. Connect Slite with an API key from the Integrations page. Promptless then searches your Slite notes for context each time it creates a suggestion.


## The problem


Section titled “The problem”


Teams that scope and plan in Slite had to keep that context separate from their documentation workflow. When Promptless generated a suggestion from a merged PR, it could read the diff. It could pull context from Jira or Linear if you connected those tools. Anything stored in Slite stayed invisible.


The gap mattered because the code did not always show decisions your team had already documented. Those decisions included the official feature name and the retry limit settled in a design thread. They also included the reasoning your product team wrote down before engineering started. When Promptless generated documentation, it inferred those details from the diff instead of reading the spec.


The result was documentation that needed more revision than it should have. Some terminology did not match the rest of the product. Some descriptions missed context the team had already worked out.


## What it does now


Section titled “What it does now”


When you connect Slite as a context source, Promptless searches your workspace for notes related to the current suggestion. It looks for product specs, RFCs, design threads, and other notes that relate to the code being documented.


If a PR description links to a Slite note, Promptless fetches that note’s content directly. You can point the agent at a specific spec by including the link. Promptless reads it while creating the suggestion.


Promptless never writes to Slite. It reads Slite in real time and caches results for the duration of a single documentation session. It then discards the cached results and does not store Slite content.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## Who benefits most


Section titled “Who benefits most”


Teams that use Slite as their primary internal knowledge base benefit most. If your team writes a spec in Slite before building a feature, Promptless can use it. The spec’s feature name, constraints, and rationale can reach the documentation Promptless generates. The agent reads the spec. It does not infer those details from the diff alone.


This also helps teams keep consistent terminology across documentation. A feature can have an official name decided in a Slite thread. Once you connect the workspace, that name is more likely to appear in Promptless’s output.


## How to connect


Section titled “How to connect”


Slite uses API key authentication, not OAuth.


1. In Slite, go to **Settings > API** and create a new API key. The key grants read access to all notes visible to that account. We recommend using a service account with appropriate scope.
2. Go to the[Integrations page](https://app.gopromptless.ai/integrations) in your Promptless dashboard.
3. Click **Connect** on the Slite integration card.
4. Enter your API key and click **Connect** .


Once connected, Slite appears as an available context source in your` promptless.yaml` . Promptless then uses your workspace for new suggestions.


## More from the blog


- [Fix skill slop before it makes your AI workforce worse](https://promptless.ai/blog/product-updates/introducing-promptless-for-agent-instructions) Product Updates


- [Grant Promptless Access to Files in Private and Shared Teams Channels](https://promptless.ai/blog/product-updates/teams-private-channel-file-access) Product Updates


- [Promptless Now Alerts You When an Integration Has a Problem](https://promptless.ai/blog/product-updates/integration-health-alerts) Product Updates


[← Back to Blog](https://promptless.ai/blog)
