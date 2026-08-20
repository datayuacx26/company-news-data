---
schema_version: "1.0.0"
document_id: "5168287977b7f923ffb5405c719c1656b9fbdb7eee6184d8a5484678f8a4b86f"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/synth-tag-v1"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-22T15:27:15.216799+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:f411adb057113f7350a9ad476f39939b568f96911a1bb9be51057c94f41b09bd"
---

# Synth Tag: delegate research runs from your SDK

## What Tag Is


Synth Tag is a session-shaped delegate API on top of Managed Research.


The core loop is small:


1. create a Tag session with a task
2. receive a` session_id` and bound` run_id`
3. send a steering message without restarting
4. poll for a terminal receipt


That receipt is the important difference. Tag is not just a chat transcript: it returns the run id, terminal state, run URL, and artifact pointers or an explicit empty-artifact reason.


## SDK Example


python


```text
from   synth_ai   import   SynthClient


client   =   SynthClient()


session   =   client.research.tag.create_session(
"Investigate the failing benchmark and summarize the smallest fix."  ,
definition_of_done  =  "Return a root-cause note with evidence and next action."  ,
)


session   =   client.research.tag.send_message(
session.session_id,
"Keep the final answer under 100 words and include the receipt."  ,
)


while   session.status.value   not   in   {  "done"  ,   "failed"  }:
session   =   client.research.tag.get_session(session.session_id)


print  (session.run_id)
print  (session.receipt.state)
print  (session.receipt.artifact_urls   or   session.receipt.artifact_empty_reason)
```


## MCP Tools


Tag also ships as three MCP tools for agent workflows:


- ` tag_create_session`
- ` tag_get_session`
- ` tag_send_message`


The tools use` tag_*` names so a calling agent can choose task mode without mixing it up with lower-level Factory and run-control tools.


## What It Is Not Yet


This beta is intentionally narrow:


- not Slack or` @Synth` in channels
- not access bundles or Claude Tag-style agent identity
- not team memory, routines, or scheduled digests
- not Factory automatic linking or Gardener/Seraph UX


Those are later layers. v1 proves the delegate, steer, receipt loop first.


## Tag vs Factory


Use Tag when you have one task to delegate now. Use Research Factory when you need a durable program with Efforts, scheduling, decisions, and repeated runs.


Tag is task mode. Factory is program mode.


## Release Proof


This release is backed by the production Tag smoke on backend` b00cd9d1f73153067c62a6807d533cb61aef7889` :


- session` b5cd5bf9-c1a7-46db-a5a2-799ca1816c8b`
- run` ba255a34-0da3-4007-8ac3-0c677d836d85`
- receipt` state=done`
- artifact` /smr/work-products/5eae8c7a-6bf4-54e8-93a5-96401fb237f7/content`
- package` synth-ai\[research\]==0.12.0`


The returned artifact URL served the Synth Tag summary paragraph from the production API.
