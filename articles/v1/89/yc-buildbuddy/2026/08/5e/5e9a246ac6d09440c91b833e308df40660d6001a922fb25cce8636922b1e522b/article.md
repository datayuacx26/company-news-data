---
schema_version: "1.0.0"
document_id: "5e9a246ac6d09440c91b833e308df40660d6001a922fb25cce8636922b1e522b"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/bb-explain-profile"
published_at: "2026-08-12T10:00:00+00:00"
first_seen_at: "2026-08-13T21:05:38.102607+00:00"
fetched_at: "2026-08-13T21:05:38.835450+00:00"
content_hash: "sha256:75703e08216e11f5566ca9074bf207beee0ad54064fa73dea9b6cb232c8aaed4"
---

# Analyze slow builds with bb explain profile

You can now use the` bb` CLI to analyze a Bazel timing profile with an AI coding agent using` bb explain profile` .


The command downloads the timing profile for an invocation and returns a detailed report with actionable recommendations for speeding up the build.


The command accepts an invocation ID or URL:


```text
bb explain profile   <  INVOCATION_ID  >     bb explain profile https://app.buildbuddy.io/invocation/  <  INVOCATION_ID  >
```


The command supports interpreting the timing profile with` --agent=codex` or` --agent=claude` . If available, it uses your locally authenticated Claude Code or Codex subscription. Otherwise you can set the environment variable` ANTHROPIC_API_KEY` or` OPENAI_API_KEY` to authorize API requests. You can also specify` --model` and` --effort` to control the agent's behavior.


```text
bb explain profile   --agent  =  codex   --model  =  gpt-5.4   --effort  =  high   <  INVOCATION_ID  >
```
