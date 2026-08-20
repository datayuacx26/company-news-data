---
schema_version: "1.0.0"
document_id: "ffce8c845d26cf05085c2db705d948cc7a1a0b30dd17715b83c66716ac847c8d"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/pipeline-local-dev"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:541c2036adabc1cfc11013d2bb2787c256bede87c2eefd399a9135a36abc14e6"
---

# Local development for data pipelines

### [Local development for data pipelines](https://www.windmill.dev/changelog/pipeline-local-dev)


Data pipelines


CLI


[v1.745.0](https://github.com/windmill-labs/windmill/releases/tag/v1.745.0)


[Docs](https://www.windmill.dev/docs/core_concepts/pipelines#local-development)


Edit, preview and run data pipelines from local files without deploying.` wmill pipeline show/run --local` builds the graph from your working tree with the same parser the UI uses,` wmill pipeline dev` live-previews the graph in the browser on every save, and` wmill pipeline docs` writes a PIPELINE.md for coding agents.


#### New features


- \`wmill pipeline show <folder> --local\` renders the pipeline graph from working-tree files, fully offline
- \`wmill pipeline run <folder> --local\` runs the whole pipeline in topological order via previews, with \`--from\`/\`--to\`/\`--dry-run\` bounds
- \`wmill pipeline dev \[folder\]\` watches the folder and live-reloads the browser graph view on every save, with run buttons, run forms and live activity
- \`wmill pipeline docs <folder>\` writes PIPELINE.md (plus AGENTS.md/CLAUDE.md pointers) describing the graph and datatable schemas for an editor or agent
- \`--partition <value>\` runs partitioned scripts on an explicit partition (time kinds default to the current UTC period locally) and doubles as a headless backfill
- \`--arg <script>:<param>=<value>\` (repeatable) and \`--upload <script>=<file>\` parameterize scripts in the cascade
