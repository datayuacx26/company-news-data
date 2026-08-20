---
schema_version: "1.0.0"
document_id: "fb8591f2e4c9ebc48dbccd5f5d222048340093423a2bb9c4e44293a1c271f28d"
company_key: "yc-litellm"
company: "LiteLLM"
source_id: "yc-litellm-news-import-8bfeaefbc2ad"
canonical_url: "https://docs.litellm.ai/blog/litellm-microsoft-assert"
published_at: "2026-06-03T10:00:00+00:00"
first_seen_at: "2026-07-22T02:33:29.495129+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:c80fec413b194b13953d08943317abaf4de85b8b2cd74d46ac63a9b914888aca"
---

# Announcing LiteLLM x Microsoft ASSERT

Today we're excited to officially launch **LiteLLM x Microsoft ASSERT** — bringing policy-driven agent evaluation to every model running through the LiteLLM AI Gateway.


## What is ASSERT?​


ASSERT is Microsoft's open-source framework for policy-driven agent evaluation, built on a proven Microsoft Research approach. ASSERT takes your organizational policies and requirements as input, systematically generates targeted evaluation scenarios, and surfaces safety and quality defects before they reach production.


## Why this matters​


As teams ship agents into production, the gap between "it works in a demo" and "it behaves under our policies" is where real risk lives. ASSERT closes that gap by turning your written policies into concrete, testable evaluation scenarios — and now those evaluations run against any of the 100+ LLM providers LiteLLM supports, through a single unified interface.


## How it works with LiteLLM​


- **Bring your policies** — ASSERT ingests your organizational policies and requirements.
- **Generate scenarios** — ASSERT systematically produces targeted evaluation scenarios.
- **Run through LiteLLM** — evaluate any model behind the LiteLLM Gateway with consistent auth, logging, and cost tracking.
- **Surface defects early** — catch safety and quality issues before they reach production.


## Get started​


ASSERT is open source. Point it at your LiteLLM Gateway endpoint and start evaluating your agents against your own policies today.


- [Set up the LiteLLM Gateway](https://docs.litellm.ai/docs/proxy/quick_start) — get a gateway endpoint running in minutes.
- [Microsoft ASSERT on GitHub](https://github.com/microsoft/assert) — install ASSERT and run it against your gateway.
