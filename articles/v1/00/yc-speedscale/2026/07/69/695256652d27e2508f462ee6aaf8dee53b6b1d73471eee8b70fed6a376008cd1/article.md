---
schema_version: "1.0.0"
document_id: "695256652d27e2508f462ee6aaf8dee53b6b1d73471eee8b70fed6a376008cd1"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/flamegraphs-find-it-replay-proves-it/"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-01T01:39:12.021391+00:00"
fetched_at: "2026-08-01T01:39:14.144415+00:00"
content_hash: "sha256:19d567100461bba6c01ee4dd8302f0f28330dff0460dd4df6ee5a9190660c443"
---

# Flamegraphs Find It. Replay Proves It.

I made an API endpoint 13 times faster. Then I realized my first verification only checked the status, headers, and response schema.


I had not checked the totals. I had made the bug faster.


That is the problem with giving an AI coding agent one kind of evidence. A CPU profile can show where the application is slow, but not whether an optimization preserves behavior. A traffic replay can prove that behavior stayed stable, but not explain why the code burns CPU.


This walkthrough gives the agent two independent witnesses:


- [Grafana Pyroscope](https://grafana.com/oss/pyroscope/) identifies the hot function and source line.
- [proxymock](https://speedscale.com/proxymock/) supplies the exact input, repeats the workload, and compares the output.


Together, they turn[AI code verification](https://speedscale.com/features/ai-code-verification/) into an experiment with two independent checks.


That distinction matters. In Stack Overflow’s 2025 Developer Survey,[87% of respondents expressed concern about AI accuracy and 81% about security and privacy](https://survey.stackoverflow.co/2025/ai) . A 2025 METR study also found that 16 experienced open source developers took[19% longer across 246 tasks with early-2025 AI tools](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/) , even though they believed the tools made them faster. Better models help. Better evidence helps more.


## What you will build


The[companion lab](https://github.com/speedscale/mock-lab/tree/main/pyroscope) contains a Go API with` GET /api/catalog/stats` . Its downstream catalog returns 15,120 records, including duplicates and malformed entries. The endpoint is correct but CPU-bound because it deduplicates records with a nested scan.


You will record that traffic, replace the downstream service with a mock, profile a repeatable load, ask an agent to fix the hotspot, and prove that the faster version returns the same stable values.


You need Go 1.23+, Docker with Compose,` curl` , and an installed and activated` proxymock` binary.


## 1. Start Pyroscope and record the contract


Clone the lab, then start Grafana, Pyroscope, and Alloy:


```text
git   clone   https://github.com/speedscale/mock-lab.git
cd   mock-lab/pyroscope
make   observability-up
```


Grafana is available at` http://localhost:3000` with` admin` /` admin` . This credential is only for the disposable local lab. Alloy pulls the Go` /debug/pprof/*` endpoints and forwards profiles to Pyroscope, following Grafana’s[Go pull-mode profiling pattern](https://grafana.com/docs/pyroscope/latest/configure-client/grafana-alloy/go_pull/) .


Start the deterministic catalog dependency:


```text
make   catalog
```


In a second terminal, start recording:


```text
make   record
```


In a third terminal, send one request through proxymock’s recording port:


```text
curl   http://localhost:4143/api/catalog/stats
```


Stop` make record` . The recording now contains the inbound request and response plus the outbound` /v1/catalog` exchange. That downstream response is our traffic contract: an optimization that drops validation or deduplication will change the final totals.


## 2. Establish correctness and performance baselines


The real catalog is no longer needed. Start the app with recorded downstream traffic and no network passthrough:


```text
make   mock
```


In another terminal, run a three-request functional replay followed by the 8-VU, 45-second load replay:


```text
make   functional-replay   RESULTS_DIR=proxymock/results/baseline
date   -u   +"%Y-%m-%dT%H:%M:%SZ"
make   load-replay   RESULTS_DIR=proxymock/results/baseline
date   -u   +"%Y-%m-%dT%H:%M:%SZ"
```


Keep the two UTC timestamps. The functional replay checks correctness and learns which fields, such as` Date` , are volatile. The load replay produces sustained CPU activity. Do not use load mode as your correctness check.


## 3. Give the coding agent both MCP servers


The lab’s[mcp.example.json](https://github.com/speedscale/mock-lab/blob/main/pyroscope/mcp.example.json) defines a local proxymock MCP server and Grafana’s MCP server. Copy those entries into your agent’s MCP configuration. The Grafana container runs read-only and exposes only datasource, Pyroscope, and navigation tools.


For Codex, run these commands from the lab directory:


```text
codex   mcp   add   proxymock   --   proxymock   mcp   run   --work-dir   .
codex   mcp   add   grafana   --   docker   run   --rm   -i   \
--add-host   host.docker.internal:host-gateway   \
-e   GRAFANA_URL=http://host.docker.internal:3000   \
-e   GRAFANA_USERNAME=admin   \
-e   GRAFANA_PASSWORD=admin   \
grafana/mcp-grafana:0.14.0   -t   stdio   --disable-write   \
--enabled-tools   datasource,pyroscope,navigation
codex   mcp   list
```


` codex mcp list` should show both servers. See the official[Codex MCP documentation](https://developers.openai.com/codex/mcp/) for configuration scopes and the desktop or IDE setup. If you use another coding agent, follow the equivalent[Claude Code MCP instructions](https://code.claude.com/docs/en/mcp) or[OpenCode MCP instructions](https://opencode.ai/docs/mcp-servers/) , translating the same two entries from` mcp.example.json` into that tool’s configuration format.


The words “query datasource Pyroscope” do not magically fetch data. The agent uses Grafana’s MCP server to discover the datasource and call` query_pyroscope` . Grafana documents both the[Pyroscope MCP tools and datasource-scoped permissions](https://grafana.com/docs/grafana/latest/developer-resources/mcp/reference/mcp-tools-table/) .


Give the agent this prompt:


```text
Using the Grafana MCP server, find the Pyroscope datasource and
available profile types. Then call query_pyroscope for the Process
CPU profile.


Use matcher {service_name="catalog-api"} and the exact UTC interval
from the baseline load replay.


Identify the hottest application functions and source lines.
Do not propose a fix until you inspect the recorded catalog input
through the proxymock MCP server.
```


The baseline profile points to` deduplicateProjects` and` runtime.memequal` . Inspection confirms the application scans every previously accepted project for every new project: quadratic work on a large catalog.


## 4. Make the smallest safe fix


Now tell the agent to implement the fix it inferred from the profile and recorded traffic:


```text
Implement the smallest maintainable fix for the CPU hotspot.
Preserve validation and first-valid-record deduplication semantics.
Do not hard-code catalog values or change tests to make them pass.
Run make test and explain the algorithmic and behavioral impact.
```


In the reference run, the agent replaced the nested scan with constant-time membership checks. The focused benchmark fell from 83.9 ms/op to 0.69 ms/op. That is encouraging, but a benchmark cannot prove the API still returns the right answer.


## 5. Replay, diff, and profile again


Restart` make mock` so it runs the candidate binary, then create a new functional result:


With the candidate running, paste this prompt into the same agent session:


```text
Validate the candidate using both proxymock and Grafana/Pyroscope.
Do not edit the application code during this validation.


1. Run:
make functional-replay RESULTS_DIR=proxymock/results/candidate
2. Through the proxymock MCP server, call response_diff to compare:
- proxymock/results/baseline/functional
- proxymock/results/candidate/functional
Report every stable-field difference. Do not treat matching status codes
or response schemas as proof of correctness.
3. Record the exact UTC start time, run:
make load-replay RESULTS_DIR=proxymock/results/candidate
Then record the exact UTC end time.
4. Through the Grafana MCP server, query the Pyroscope Process CPU profile
for matcher {service_name="catalog-api"} over that exact candidate interval.
5. Compare the candidate latency, throughput, failed requests, response diff,
and CPU hotspots with the baseline evidence from earlier in this session.


Report the evidence in a before-and-after table. Declare the candidate valid
only if there are no failed functional requests, no stable response changes,
and the original application hotspot is absent or materially reduced.
```


The commands and expected evidence are broken out below if you want to follow the validation manually.


```text
make   functional-replay   RESULTS_DIR=proxymock/results/candidate
```


Ask the agent to call proxymock’s` response_diff` , comparing` proxymock/results/baseline/functional` with` proxymock/results/candidate/functional` . The[proxymock MCP tools](https://docs.speedscale.com/proxymock/how-it-works/mcp-tools/) let the agent search local traffic and compare replay results without translating them by hand.


The expected result is no stable-field differences. A matching` 200` and schema are not enough.


Now repeat the performance run and query its exact UTC interval through Grafana MCP:


```text
date   -u   +"%Y-%m-%dT%H:%M:%SZ"
make   load-replay   RESULTS_DIR=proxymock/results/candidate
date   -u   +"%Y-%m-%dT%H:%M:%SZ"
```


The validated reference run produced these results on one Apple arm64 machine:


Evidence Baseline Candidate


Average latency 168.2 ms 13.0 ms


Throughput 47.3 req/s 591.3 req/s


Failed requests 0 0


Stable response changes Baseline None


Dominant application CPU Deduplication JSON decoding


Treat the numbers as directional, not universal hardware guarantees. Compare both versions on the same machine. Pyroscope explains where CPU moved; proxymock supplies absolute workload results and semantic proof.


## Give the agent evidence that can disagree


Profiles can expose function names, repository structure, and source lines. Recorded traffic can contain credentials and personal data. Outside this local lab, use a scoped read-only Grafana service account, redact sensitive traffic, retain an audit trail, and decide explicitly where agent data may be sent and retained.


The agent in this walkthrough did not become smarter. It stopped guessing. Pyroscope found the expensive code. The recorded input explained why it was expensive. Replay rejected behavior-changing shortcuts. The second profile showed that the hotspot actually moved.


Flamegraphs find the problem. Replay proves the fix.
