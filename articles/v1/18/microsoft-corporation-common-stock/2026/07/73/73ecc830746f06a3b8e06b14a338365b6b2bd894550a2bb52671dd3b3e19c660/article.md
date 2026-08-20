---
schema_version: "1.0.0"
document_id: "73ecc830746f06a3b8e06b14a338365b6b2bd894550a2bb52671dd3b3e19c660"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-5eeb9d1ed7ff"
canonical_url: "https://devblogs.microsoft.com/blog/dont-rewrite-your-cli-for-agents/"
published_at: "2026-07-07T13:52:06+00:00"
first_seen_at: "2026-07-20T04:34:25.944233+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:00d996cc33d8513ebf38606ef764dfc88fdb15859416d7d816eb9effdfa98589"
---

# Don’t rewrite your CLI for agents

There’s advice making the rounds: replace your CLI args with a single` --json` payload so agents can use your tool more effectively. The thinking being, that agents already think in structured formats, and nested data maps cleanly to JSON. Flat args on the other hand, force awkward conventions like repeating` --service-name` to delimit multi-value groups, which is inherently ambiguous. Not to mention, that the agent needs to get the types of all values right.


It’s a reasonable hypothesis, and we wanted to know if it holds up under measurement. The data we collected, showed something interesting.


## What we tested


We built a synthetic CLI called` podctl` that creates multi-service deployments. The scenario was deliberately complex: two services with independent configuration, three levels of nesting (` services\[\].resources.cpu.request` ), arrays, mixed types, and cross-references between services. Over 30 distinct values across the deployment spec. Here’s what the args invocation looks like:


```text
podctl create \
--name "api-gateway-prod" \
--env production \
--region us-east-1 \
--replicas 3 \
--service-name auth \
--service-image ghcr.io/acme/auth:v2.4.1 \
--service-port 8080 \
--service-protocol grpc \
--service-cpu-request 250m \
--service-cpu-limit 500m \
--service-mem-request 128Mi \
--service-mem-limit 256Mi \
--service-health-path /healthz \
--service-health-interval 10s \
--service-health-timeout 3s \
--service-health-retries 3 \
--service-env DB_HOST=db.internal \
--service-env DB_PORT=5432 \
--service-name gateway \
--service-image ghcr.io/acme/gateway:v1.8.0 \
--service-port 443 \
--service-protocol https \
--service-cpu-request 500m \
--service-cpu-limit 1000m \
--service-mem-request 256Mi \
--service-mem-limit 512Mi \
--service-health-path /ready \
--service-health-interval 15s \
--service-health-timeout 5s \
--service-health-retries 5 \
--service-env UPSTREAM=http://auth:8080 \
--service-depends-on auth \
--scaling-min 2 \
--scaling-max 10 \
--scaling-metric cpu \
--scaling-target 70 \
--rollout-strategy canary \
--rollout-canary-percent 10 \
--rollout-canary-pause 300


```


Notice how` --service-name` appears twice to start a new service block. The agent has to figure out that flags after the second` --service-name` belong to the gateway, not to auth. And here’s the same deployment as a JSON payload:


```text
podctl create --json '{
"name": "api-gateway-prod",
"environment": "production",
"region": "us-east-1",
"replicas": 3,
"services": [
{
"name": "auth",
"image": "ghcr.io/acme/auth:v2.4.1",
"port": 8080,
"protocol": "grpc",
"resources": {
"cpu": {"request": "250m", "limit": "500m"},
"memory": {"request": "128Mi", "limit": "256Mi"}
},
"healthCheck": {
"path": "/healthz",
"interval": "10s",
"timeout": "3s",
"retries": 3
},
"env": {"DB_HOST": "db.internal", "DB_PORT": "5432"}
},
{
"name": "gateway",
"image": "ghcr.io/acme/gateway:v1.8.0",
"port": 443,
"protocol": "https",
"resources": {
"cpu": {"request": "500m", "limit": "1000m"},
"memory": {"request": "256Mi", "limit": "512Mi"}
},
"healthCheck": {
"path": "/ready",
"interval": "15s",
"timeout": "5s",
"retries": 5
},
"env": {"UPSTREAM": "http://auth:8080"},
"dependsOn": ["auth"]
}
],
"scaling": {
"min": 2,
"max": 10,
"metric": "cpu",
"targetPercent": 70
},
"rollout": {
"strategy": "canary",
"canaryPercent": 10,
"canaryPauseSec": 300
}
}'


```


You can see the appeal. The JSON version makes the hierarchy explicit. Each service is a distinct object, resources nest naturally, and there’s no ambiguity about which field belongs where. On paper, this should be easier for an agent to get right.


We used two separate CLIs as the test subjects. One accepts only individual args, the other accepts only a` --json` payload. Both share the same validation backend and normalize to the same canonical structure. At test time, whichever variant is active gets renamed to` podctl` so the agent sees a consistent binary name. Since the CLI is unknown to the agent, it would have to discover the input model from scratch. For args, it means invoking the CLI’s help. For JSON, it also means invoking help but also calling an extra command that returns the JSON schema.


We used two separate binaries because a single CLI supporting both modes would let the agent pick its preferred approach, likely skewing the results.


We ran each model five times per input mode with identical prompts and evaluation criteria, using GitHub Copilot Chat as the harness. For the models we picked Claude Haiku 4.5, Claude Sonnet 4.6, Claude Sonnet 5, GPT-5.3-Codex, and MAI-Code-1-Flash, which are all commonly used for coding.


## Correctness: args swept


Every args profile achieved perfect correctness across all five runs, regardless of model. Haiku 4.5, the smallest model in the test, produced correct deployments as reliably as Sonnet 5.


For JSON, the picture looked differently. The three strongest models matched args with 5/5 correctness. The two smaller models didn’t. Haiku 4.5 managed 2 out of 5 correct deployments in JSON mode. MAI-Code-1-Flash managed 3 out of 5.


Model args JSON


Claude Sonnet 5 5/5 5/5


Claude Sonnet 4.6 5/5 5/5


GPT-5.3-Codex 5/5 5/5


MAI-Code-1-Flash 5/5 3/5


Claude Haiku 4.5 5/5 2/5


Args constrain the input space enough that even smaller models produce correct output consistently. JSON requires the model to manage its own structure, and that’s a capability tax not every model can pay.


## The cost


Developers pay for tokens, not attempts. So we broke down the token consumption into input, cached, and output tokens and calculated cost per task using GitHub Models pricing.


Model args JSON Ratio


GPT-5.3-Codex $0.05 $0.54 11x


MAI-Code-1-Flash $0.01 $0.08 10x


Claude Sonnet 4.6 $0.05 $0.47 9x


Claude Haiku 4.5 $0.03 $0.23 8x


Claude Sonnet 5 $0.08 $0.32 4x


JSON mode cost 4x to 11x more per task across every model. These are per-invocation numbers, and for a team running such tasks many times it would quickly compound.


The raw token gap was even larger (4x to 14x) but caching softens the blow. In JSON mode, the agent retries many times, and each retry’s input is mostly cached at the lower rate. What caching can’t offset though is the output tokens. Each retry means the agent reasons about the error and produces a new workaround, and those workarounds get creative: piping JSON through variables, writing to temp files, trying different escaping strategies. All of that is output, and output tokens are the most expensive category. JSON retries generated 7x to 14x more output tokens than the args path. That’s where the cost comes from.


Where do those extra tokens go? Retries. When a model constructs a JSON payload and the CLI rejects it, the agent reads the error, attempts a fix, and tries again. JSON has more ways to fail (syntax errors and wrong nesting are the obvious ones) and as we’ll see in the next section, shell escaping adds another layer of failures on top. More failure modes mean more retries, and more retries mean more money.


## The shell escaping tax


Retries aren’t the whole story though. There’s a factor the “rewrite for JSON” advice doesn’t account for: shell escaping.


When the agent passes a JSON payload on the command line, it has to escape quotes for the shell. Different shells have different escaping rules for nested quoted strings, and none of them are simple. Models routinely produced valid JSON that broke on shell escaping: double-quoted strings inside single-quoted blocks, nested escaping that the shell interpreted differently than intended.


The primary experiment ran on Windows, where agents default to using PowerShell. The Haiku 4.5 JSON failures illustrate the escaping problem clearly. Three of the five runs never reached a successful invocation. The agent produced correct JSON content but couldn’t get it past the shell. It tried base64 encoding, file piping, here-strings, and` cmd.exe` passthrough, burning tokens on each attempt, and still failed.


We ran the same experiment with Sonnet 4.6 on macOS, where agents default to Bash, to see whether shell choice affected the results. On PowerShell, the cost gap between args and JSON was 9x. On Bash, it collapsed to 1.5x. Same model, same payload, same correctness. The difference was entirely in how each shell handles quoted JSON strings. Args on the other hand were stable across both shells: $0.05 on PowerShell vs. $0.07 on Bash. That’s the point: JSON introduces a dependency on shell quoting behavior that varies wildly across environments. Args don’t.


## Why constraints help agents


The deeper finding is about what constraints do for models. Args constrain the agent’s output in ways that JSON doesn’t.


With args, the` --help` text defines the exact set of valid inputs. Each option has a name and a type, sometimes with an enum of valid values. The model maps from the task description to specific option names and values.


With` --json` , the model gets a schema (or infers one from` --help` ) and constructs a free-form object that must satisfy it. The syntax must be valid, and the nesting must be correct. The field names and types must match. And the whole thing must survive shell escaping. Each of those is a failure mode that args eliminate by design.


Strong models eventually get JSON right, but “eventually” is doing a lot of work. They still hit shell escaping failures, they just recover from them. That recovery costs 4x to 11x more than the args path, even when correctness is identical. Smaller models hit the same escaping failures but can’t recover, which is where correctness drops. For smaller models, the constraints args provide are the difference between consistent success and frequent failure. It turns out, that narrowing the valid input space compensates for gaps in model capability. The model doesn’t need to get JSON nesting right because there’s no JSON nesting to get right.


## What this means for your CLI


If you’re building a CLI that agents will use, the data points in a different direction than “rewrite it with` --json` .” Keep your args. They work across the model capability spectrum and don’t have environment-dependent failure modes. They also cost fewer tokens and money.


But what if your input data is genuinely complex? Our test scenario had 30+ values across nested services, and args handled it perfectly across every model. If you want to offer a` --json` option for programmatic use or batch operations, that’s fine. But don’t remove args in favor of JSON, and don’t expect JSON to improve agent outcomes. In this experiment, JSON never improved correctness and always increased cost.


The intuition that JSON *should* work better makes sense on paper. But “should” doesn’t survive contact with actual models running in actual shells on actual operating systems. Notice, that args are constrained and predictable across shells. JSON is expressive but fragile. For agent-driven CLI usage, constrained wins.


## Measure before you rewrite


This is the same lesson that runs through the rest of the[AX series](https://developer.microsoft.com/blog/the-ax-stack-whats-fixed-where-you-can-win) : plausible advice and measured outcomes are different things, and when they disagree, the measurements win. If someone recommends restructuring your CLI’s input model, run the experiment first. Pick a scenario, define evaluation criteria, select relevant harnesses, models and OS for your audience and do a couple of runs on both approaches. You’ll have your answer in a day without rewriting anything you didn’t need to rewrite.
