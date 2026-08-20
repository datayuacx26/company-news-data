---
schema_version: "1.0.0"
document_id: "af4f322c3b36c29db03911ee233abab117dfd62417d8fa67a5b134c37c5e40c5"
company_key: "yc-stagewise"
company: "stagewise"
source_id: "yc-stagewise-news-import-0e327623c986"
canonical_url: "https://stagewise.io/news/release-week-april-27-3"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-22T14:44:14.475585+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:aa4941d8db4c3d200ecc4b1c88895242a2204082562d7cd241850e16ab998b5c"
---

# Release Week: April 27–May 3 · stagewise Newsroom

## What Shipped Last Week


During the last week, we shipped **Alpha 73** through **Alpha 78** . Let's take a look at the biggest changes!


### Support for GPT 5.5


Just like you, we waited eagerly for OpenAI to make **GPT 5.5** available on the public API. Right as that happened on April 27, we shipped an update that made OpenAI's newest model available in stagewise. GPT 5.5 is a massive bump in intelligence and agentic capabilities compared to GPT 5.4, but it also costs significantly more. We therefore recommend: for frontier intelligence, give the new model a spin, but for regular coding tasks, GPT 5.4 is still a great companion.


### Agent Preview Panels


Working with many agents at once can quickly become very tedious, and keeping an overview is not that easy. Thus, we shipped Agent Preview Panels - both in the Agents List and in the Active Agents Section - to make it easier to identify the correct agent or simply get a quick overview of what has happened in the agent. Simply hover over the agent card, and you will see the new Preview Panel pop up.


### Always allow mode for Agents


Up until now, you always had to manually approve tool calls for bash execution in stagewise. We shipped a new **Always Allow Mode** that bypasses permission checks. We recommend using this feature only on dedicated dev machines without sensitive secrets or strong sandboxing, as injection attacks may lead to data loss or the leakage of sensitive information.


### Remotion Plugin


We shipped a dedicated Remotion plugin to stagewise, making it **easier than ever to build high quality videos** using your favorite Agent.


The new plugin ships a more efficient and more opinionated version of the well-known Remotion skill, and extends it with design best practices, a clear video creation process, and a standardized way to build and maintain reusable video style guides, making it easy to not just create one video, but create multiple while maintaining brand consistency.


### Native support for DeepSeek and Z.AI models


We shipped native support for the latest DeepSeek and Z.AI models, giving you even easier access to the latest and greatest open source models from the two labs.
