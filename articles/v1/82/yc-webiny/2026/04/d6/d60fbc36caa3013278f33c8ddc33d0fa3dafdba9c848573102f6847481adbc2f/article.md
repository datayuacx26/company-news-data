---
schema_version: "1.0.0"
document_id: "d60fbc36caa3013278f33c8ddc33d0fa3dafdba9c848573102f6847481adbc2f"
company_key: "yc-webiny"
company: "Webiny"
source_id: "yc-webiny-news-import-09ea055e8444"
canonical_url: "https://www.webiny.com/blog/webiny-v6-an-ai-programmable-cms-built-for-complexity"
published_at: "2026-04-07T13:31:46.827+00:00"
first_seen_at: "2026-07-22T19:34:09.199841+00:00"
fetched_at: "2026-07-28T22:16:00.015330+00:00"
content_hash: "sha256:289289cc4ece8c73d354a5c185f1ebedfefbb2b4071cf73a16b8c15ad6e24c1f"
---

# Webiny v6: The AI-Programmable Self-Hosted CMS | Webiny

We've been building Webiny for developers working on hard problems. Multi-tenant platforms. Content infrastructure for large organizations. SaaS products that need to embed content management without building it from scratch. Complex editorial operations spanning multiple teams, brands, and markets.


v6 is the largest release we've shipped. Not because we added the most features — because we made a set of interconnected decisions that change what it means to build on top of Webiny. This post walks through what changed, why it changed, and what it means for teams already running v5.


Here's the short version if you're not reading the whole thing:


- **AI-programmable via a local MCP server** — AI agents now have deep, expert-level knowledge of Webiny's architecture and best practices, running entirely in your environment. No data leaves your infrastructure.
- **Single-package project structure** — the monorepo is gone. One entry point, one pattern, dramatically simpler codebases that new developers can orient in without a senior guide.
- **Website Builder replaces Page Builder** — your framework renders the pages, your infrastructure hosts them. CSR, SSR, and ISR all supported. Custom components registered with a single prompt. Comes with a Next.js starter kit.
- **Headless CMS: field-level permissions and layout components** — control view/edit access per field, and structure content models with sections, tabs, and conditional visibility rules.
- **Webiny SDK** — stop writing GraphQL queries by hand. The SDK handles it with proper TypeScript types and significantly less boilerplate.
- **Tenant Manager with programmatic API** — create, install, configure, and disable tenants via API. The feature that makes Webiny viable as a SaaS backend.
- **Overhauled Publishing Workflows** — simpler for editors and reviewers, fewer support requests landing in developer inboxes.
- **New UI built on shadcn and Tailwind** — predictable, customizable, and no longer looks like legacy enterprise software.
- **Learn Webiny course** — a structured path from zero to productive, replacing the "dig through reference docs and hope" experience.


---


## The Positioning Shift: What "AI-Programmable" Actually Means
