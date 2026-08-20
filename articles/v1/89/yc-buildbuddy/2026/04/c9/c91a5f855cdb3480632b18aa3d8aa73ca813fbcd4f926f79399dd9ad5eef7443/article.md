---
schema_version: "1.0.0"
document_id: "c91a5f855cdb3480632b18aa3d8aa73ca813fbcd4f926f79399dd9ad5eef7443"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/secret-env-overrides"
published_at: "2026-04-10T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T22:15:54.285032+00:00"
content_hash: "sha256:8041351520c83f5510579c3e3b32772a6995c66519231c6febe7d66eddd1d2f3"
---

# Improved handling of short-lived secrets in remote execution

BuildBuddy has supported passing short-lived secrets to remote actions via the` env-overrides` platform property, which redacts values from action cache entries. The new` secret-env-overrides` and` secret-env-overrides-base64` properties extend this protection by also **redacting values from workflow logs** .


Pass secrets via remote exec headers so they're injected at invocation time without affecting the action hash:


```text
bazel build //my:target   \         --remote_exec_header  =  x-buildbuddy-platform.secret-env-overrides  =  API_KEY  =  sk-abc123,OTHER_KEY  =  val
```


For values containing commas or special characters, base64-encode each` KEY=VALUE` pair:


```text
bazel build //my:target   \         --remote_exec_header  =  x-buildbuddy-platform.secret-env-overrides-base64  =  $(  echo     -n     'CREDS={"token": "abc"}'     |   base64  )
```


See the[Secrets docs](https://www.buildbuddy.io/docs/secrets) and[RBE platform properties reference](https://www.buildbuddy.io/docs/rbe-platforms) for more details.
