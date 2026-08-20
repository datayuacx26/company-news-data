---
schema_version: "1.0.0"
document_id: "863be787fc081e4834d8558c6c989181555f7928f1a3061c403d8732896a3064"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/scheduled-workflows"
published_at: "2026-05-28T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:8e56e8dfc5059c374751269493d087348f85e2f9a5e34827668917b1c78f4426"
---

# Schedule Workflows with cron expressions

You can now run BuildBuddy Workflows on a recurring schedule using standard cron expressions. This is useful for nightly builds, periodic integration tests, or any job that should run independently of code pushes or pull requests.


To schedule an action, add a` schedule` trigger with one or more cron expressions under` triggers` :


buildbuddy.yaml


```text
actions  :         -     name  :     "Nightly tests"           triggers  :             schedule  :               crons  :                 -     "0 2 * * *"     # 2:00 AM UTC every day           steps  :             -     run  :     "bazel test //..."
```


Scheduled runs always execute against the latest commit on your repo's default branch. All cron times are interpreted as UTC, and the minimum supported interval between triggers is 15 minutes.


See the[Workflows configuration docs](https://www.buildbuddy.io/docs/workflows-config#scheduling-workflows-with-cron-expressions) for format details and more examples.
