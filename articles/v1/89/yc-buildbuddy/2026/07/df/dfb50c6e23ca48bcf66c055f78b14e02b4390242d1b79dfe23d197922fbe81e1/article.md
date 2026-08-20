---
schema_version: "1.0.0"
document_id: "dfb50c6e23ca48bcf66c055f78b14e02b4390242d1b79dfe23d197922fbe81e1"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/detect-nondeterminism"
published_at: "2026-07-01T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:97d6ef9d426f8d6f7121b62f374fb5de89d309870c6d1341b6a45fd76e80a808"
---

# bb detect nondeterminism

You can now use the` bb` CLI to detect non-deterministic builds using the` bb detect nondeterminism` command.


The command runs a Bazel command twice with caching disabled, then compares the two compact execution logs with[bb explain](https://www.buildbuddy.io/changelog/bb-explain) . Spans whose outputs differ between the two runs are reported as non-deterministic.


```text
# By default, the command runs "build //...".     bb detect nondeterminism        # You can pass any Bazel command to run.     bb detect nondeterminism   --bazel_command  =  'build //foo:bar --config=linux'
```


## Sending notifications​


To email all BuildBuddy org admins when non-determinism is detected, add` --notify_email` . To post a notification to a Slack channel, add` --notify_slack=<SECRET_NAME>` , where` <SECRET_NAME>` is the name of a[BuildBuddy secret](https://www.buildbuddy.io/docs/secrets) holding a Slack webhook URL.


Sending notifications requires an API key with the **notification** capability. Set it via the` BB_NOTIFY_API_KEY` environment variable:


```text
BB_NOTIFY_API_KEY  =  <  API_KEY  >   bb detect nondeterminism   --notify_email
```


## Running on a schedule​


To schedule a nightly nondeterminism check to catch regressions, you can configure a scheduled Workflow in your buildbuddy.yaml:


buildbuddy.yaml


```text
actions  :         -     name  :   Nondeterminism check          triggers  :             schedule  :               crons  :                 -     "0 8 * * *"     # 8:00 AM UTC every day           steps  :             -     run  :   bb detect nondeterminism   -  -  notify_email   -  -  notify_slack=SLACK_WEBHOOK_SECRET_NAME          platform_properties  :             # Caching is disabled for this check anyway, so recycling adds little.             recycle-runner  :     false
```
