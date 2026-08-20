---
schema_version: "1.0.0"
document_id: "4334e2ee3f38ae0f3bba425bcfdf76f6eef6af0855b870d8a7ed72ec3ec92784"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/launching-synth-managed-research-mcp-first"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-22T15:27:15.216799+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:64940cb10f4012e04465b5612530a4950826d1e8d39b24995f4728d9ad49c6d8"
---

# Launching Managed Research: A Nanoprogram Walkthrough

## What launches today


We are launching one canonical builder path for Managed Research: a repo-backed, MCP-first workflow that produces a real artifact someone can judge locally.


The public example is Nanoprogram because it has a hard contract and an official evaluator. If the walkthrough works, you do not need to trust screenshots or marketing copy. You can pull the workspace, inspect` submission/optimizer.py` , and run the same shell script we run.


The same control-plane loop also applies to NanoHorizon-style work, but Nanoprogram is the launch-day anchor because it gives us a single-file deliverable and a public harness.


## The contract


The demo is successful only if the Managed Research workspace contains:


- ` submission/optimizer.py`


That file needs to implement the Nanoprogram optimizer hooks:


python


```text
def   run_optimizer  (contract) ->   dict  :
...


def   best_candidate  (contract, result  =  None  ) -> dict[  str  ,   str  ]:
...
```


We validate it with Nanoprogram's official evaluator, not with an internal smoke-only shortcut.


## The canonical MCP loop


This is the exact order we want people to follow:


1. ` smr_health_check`
2. ` smr_create_runnable_project` or` smr_list_projects`
3. ` smr_attach_source_repo`
4. optionally` smr_set_project_notes`
5. ` smr_get_project_setup`
6. ` smr_prepare_project_setup`
7. ` smr_get_capacity_lane_preview`
8. ` smr_get_launch_preflight`
9. ` smr_trigger_run`
10. ` smr_get_run` or` smr_get_semantic_progress`
11. ` smr_download_workspace_archive` or` smr_get_project_git`


The important detail is that launch preflight and trigger must use the same launch payload.


## The launch payload


For the public walkthrough, the default shape is:


json


```text
{
"project_id"  :   "proj_123"  ,
"host_kind"  :   "daytona"  ,
"work_mode"  :   "directed_effort"  ,
"agent_kind"  :   "codex"  ,
"agent_model"  :   "gpt-5.4-mini"  ,
"initial_runtime_messages"  : [
{
"body"  :   "Inspect the Nanoprogram contract, improve the optimizer, and leave the workspace ready for local evaluation with evaluate_optimizer.sh. The primary deliverable is submission/optimizer.py."  ,
"mode"  :   "queue"
}
]
}
```


Two rules matter here:


- attach the repo before launch
- put kickoff text in` initial_runtime_messages` , not the removed` prompt` field


## The trigger rule that matters


MCP trigger calls can succeed at the protocol level while still denying launch in the payload. Always branch on` result.get("error")` .


python


```text
result   =   smr_trigger_run(  ...  )
if   result.get(  "error"  ):
raise   RuntimeError  (  f  "  {  result[  'error'  ]  }  :   {  result.get(  'message'  )  }  "  )


run_id   =   result[  "run_id"  ]
```


If you skip this check, you can end up treating a routing or entitlement denial as a real run.


## Semantic progress is the public read model


After trigger, the canonical inspection surface is no longer just raw run state. Use` smr_get_semantic_progress` when you want the run's primary parent, OEQs, DEOs, milestones, experiments, and run progress in one place.


Raw` smr_get_run` is still useful for low-level runtime state, but it is not the best answer to "what is this run trying to achieve?".


## Retrieving the workspace


There are two valid post-run paths:


- use` smr_download_workspace_archive` when you want the safest launch-day path for local inspection and evaluation
- use` smr_get_project_git` only when your deployment actually pushes changes back to the attached repo


The archive is a project-level snapshot, not a per-run bundle, and the download URL is short-lived. That is why the blog walkthrough teaches archive retrieval first.


## Running the official evaluator


After extracting the workspace archive into your local Nanoprogram checkout, run:


bash


```text
bash   evaluate_optimizer.sh   --fast   submission/optimizer.py   banking77
```


For the full confidence pass, run:


bash


```text
bash   evaluate_optimizer.sh   submission/optimizer.py   banking77   hotpotqa   openforecaster
```


Those are the only evaluator commands the launch walkthrough teaches.


## Common failures


- no repo was attached before trigger, so the run has nothing useful to work on
- launch preflight and trigger used different payloads, so preflight did not actually validate the launched run
- the client trusted a successful tool call without checking` result.get("error")`
- kickoff text was sent through the removed` prompt` field instead of` initial_runtime_messages`
- the user expected git output when the deployment only produced a workspace archive


## Where the frontend fits


The hosted app follows the same launch order: setup, lane preview, launch preflight, then trigger. The UI is important, but the launch-day walkthrough is intentionally MCP-first so people can see the real control-plane contract without hidden app behavior.


## What this post is not claiming


- Nemotron is not required for the launch-day demo
- the UI is not a separate product path from MCP
- local evaluation is not optional for submission-style work


Nemotron remains part of the broader product direction, but the default public walkthrough is the OpenAI-backed hedge path because it is the shortest route to a reproducible result today.
