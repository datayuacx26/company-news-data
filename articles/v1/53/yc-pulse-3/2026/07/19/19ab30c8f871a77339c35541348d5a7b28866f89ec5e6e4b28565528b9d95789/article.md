---
schema_version: "1.0.0"
document_id: "19ab30c8f871a77339c35541348d5a7b28866f89ec5e6e4b28565528b9d95789"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/introducing-the-pulse-cli"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-24T09:39:59.698897+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:4ac1903171371d8ac9322d3a2e8c9bbe15aabb5e8d2871249ffc24e3ea68608d"
---

# Introducing the Pulse CLI

The Pulse CLI is live today. It brings the full extraction pipeline to your terminal: point it at a PDF, a folder, or even a URL, and get back clean markdown and structured JSON in a single command, agent-ready data flowing straight into your scripts.


## **How it works**


The CLI talks to the same API as the platform and the MCP server, and anything you run is visible in your extraction library on the platform. It exposes the pipeline as a small set of commands:


- **extract** : parses files, folders, or URLs into clean markdown and JSON, with an optional schema for structured fields in the same run.
- **schema** : drafts or refines a JSON extraction schema from a plain-English description of the fields you want, or applies one to a prior extraction.
- **split** : segments an extraction into topic-based page ranges, so you work on the three pages that matter rather than all two hundred.
- **tables** : pulls tables out of a prior extraction, merging tables that span pages.
- **jobs** : watches, resumes, or cancels asynchronous batch runs.


Every command has a pipe-safe JSON output mode that flows straight into the next tool in your script, which makes the CLI a natural fit for shell scripts, cron jobs, and CI.


## **Getting started**


Install with uv or pip (the package is pulse-ai-cli), then run pulse login. Your browser opens, you approve a short code, and the CLI picks up your organization's API key automatically. From there, pulse extract on any document returns markdown and JSON in one run. Full setup instructions live at[docs.runpulse.com/cli/overview](https://docs.runpulse.com/cli/overview) .


## **When to reach for it**


The CLI is built for working hands-on in a terminal: one-off extractions, local files and folders, shell scripts, cron jobs, and CI steps. When you want an agent to read and structure documents on demand, deciding the sequence of steps for itself, the MCP server is the better fit. And when the goal is a production pipeline in your own application code, the Python and TypeScript SDKs and the REST API remain the right choice, since they give you precise and repeatable control over every step.


## **What this means**


Most document work starts with a file sitting in a folder and a question about what is inside it, and the answer has always been at least a script, an upload, or an agent away. With the CLI, the distance between "I have this document" and "I have its data" is one command, run from the place developers already live.


The Pulse CLI is live today. Install it, run pulse login, and point it at your hardest document.
