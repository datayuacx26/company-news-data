---
schema_version: "1.0.0"
document_id: "e067501989f19b94b18161f3aaefce2dd73d20da3b8587867dfbcbdefce11727"
company_key: "yc-bloom-4"
company: "Bloom"
source_id: "yc-bloom-4-rss-ddbb04375eab"
canonical_url: "https://bloom.diy/changelog/tx7egaxpcq121556pav6synd69875qpc"
published_at: "2026-01-01T04:00:00+00:00"
first_seen_at: "2026-07-25T01:09:15.209381+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:5088df134b950b7dcbfa43f9204585e17657031ef57a768a792cf468147816c5"
---

# The Agent Now Asks Before It Builds

[Changelog](https://bloom.diy/changelog) January 1, 2026


# The Agent Now Asks Before It Builds


We've added a new step to how Bloom handles prompts. When you describe something and the agent needs more context to get it right, it now asks you a short set of clarifying questions before building.


You answer them, the agent uses your answers, and the result is closer to what you actually wanted on the first try.


**How it works**


The questions are short and specific. Multiple choice where possible, short text input when needed. It takes 20 to 30 seconds to answer a typical set.


The agent only asks when it genuinely needs to. Simple changes and clear prompts go straight to building as normal.


**Why we built it**


The biggest source of wasted credits and frustration was the agent guessing wrong and having to redo work. This fixes the root cause: giving the agent the information it needs before it starts.


**Keyboard shortcuts**


On web, you can use number keys to select multiple choice answers and Tab to move between questions without touching the mouse.


Human in the Loop is available to all users on web and iOS.
