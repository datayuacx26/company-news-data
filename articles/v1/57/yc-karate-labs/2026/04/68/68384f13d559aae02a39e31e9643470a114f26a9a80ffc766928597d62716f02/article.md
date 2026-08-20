---
schema_version: "1.0.0"
document_id: "68384f13d559aae02a39e31e9643470a114f26a9a80ffc766928597d62716f02"
company_key: "yc-karate-labs"
company: "Karate Labs"
source_id: "yc-karate-labs-news-import-eb3f9bfe6418"
canonical_url: "https://karatelabs.io/blog/enterprise-computer-use-on-premises"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-22T01:15:21.098373+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:88091f2dc4fa2dc5120139811066e488381cc743c8bfafe55f3bb3e1a7d4ab4d"
---

# Enterprise Computer Use On-Premises | Karate Labs

Cloud-based AI browser agents are impressive. Claude computer use, OpenAI Operator, Google’s experimental agents — all can drive browsers to accomplish meaningful work. Demos are compelling. Proof-of-concepts are fast to set up.


Enterprise production deployment is a different story. Three hard constraints stop cloud agents at the procurement gate: data residency, cost at scale, and vendor lock-in. This post is about how enterprises are running AI browser agents on their own infrastructure in 2026, and what that deployment actually looks like.


## Why cloud AI agents don’t work for regulated workloads


When a cloud-based browser agent operates against your web application, it sends screenshots or DOM extracts of that application to the vendor’s inference servers. Those screenshots and DOM extracts often contain:


- Personally identifiable information (PII) — names, emails, addresses in form fields
- Financial data — account numbers, transactions, portfolio positions
- Healthcare data — patient records, appointment information
- Business-sensitive data — pricing, deal flow, strategic documents
- Session credentials — authentication tokens, cookies, session state


For regulated industries — banking, insurance, healthcare, government, defense — this is a non-starter. Compliance frameworks (GDPR, HIPAA, PCI DSS, SOX, industry-specific rules) require that sensitive data stay inside the organization’s control plane.


Even for unregulated enterprises, the risk calculus is unfavorable. Why expose your application’s UI — which is your business logic, your user experience, your competitive position — to a third party when you don’t have to?


## The on-premises AI agent stack


Running AI browser agents on your own infrastructure requires three components: the agent server, the browser runtime, and the LLM. Modern tools let you deploy all three inside your network.


### Component 1: the agent server


[Karate Agent](https://karatelabs.io/agent) is the reference implementation for enterprise on-premises deployment. It ships as a Docker image. One container runs the agent server; additional containers run browser sessions on demand. Kubernetes orchestrates horizontal scale.


### Component 2: the browser runtime


Headless Chrome inside Docker. Each test session runs in its own container — complete isolation, no state leakage. The agent server communicates with browsers via Chrome DevTools Protocol.


### Component 3: the LLM


This is the part that cloud-based agents dictate. Self-hosted AI agents let you choose.


- **Cloud LLM (hybrid):** keep the agent on-prem, but call out to Anthropic/OpenAI/Google for inference. Acceptable for non-sensitive workloads under existing enterprise LLM agreements.
- **Self-hosted LLM (fully on-prem):** run Llama, Qwen, DeepSeek, or Mistral on your own GPU infrastructure via Ollama or vLLM. No data leaves.


## Reference architecture: fully on-premises


```text
# Kubernetes deployment (excerpt)


# Agent server
apiVersion: apps/v1
kind: Deployment
metadata:
name: karate-agent
spec:
replicas: 3
template:
spec:
containers:
- name: agent
image: karatelabs/agent:latest
env:
- name: LLM_PROVIDER
value: ollama
- name: LLM_ENDPOINT
value: http://ollama:11434
- name: LLM_MODEL
value: llama3.3:70b
---
# LLM runtime (GPU node)
apiVersion: apps/v1
kind: StatefulSet
metadata:
name: ollama
spec:
serviceName: ollama
replicas: 1
template:
spec:
nodeSelector:
node.kubernetes.io/gpu: "true"
containers:
- name: ollama
image: ollama/ollama:latest
resources:
limits:
nvidia.com/gpu: 1
volumeMounts:
- name: models
mountPath: /root/.ollama


```


This is running enterprise AI browser testing with zero external dependencies. No internet required after deployment. No telemetry. No model calls phoning home.


## Hardware planning


Sizing depends on which LLM you run and how many concurrent sessions you need.


### LLM hardware (the expensive part)


- **Llama 3.1 8B (Q4):** runs on a single RTX 4070 Ti (~12GB VRAM). Fine for routine workloads.
- **Llama 3.3 70B (Q4):** runs on an A100 40GB with spill, or 2×RTX 4090. Good for most enterprise workloads.
- **Qwen 2.5 72B (FP16):** needs 144GB VRAM, multi-GPU (2×A100 80GB or 4×A100 40GB).
- **DeepSeek V3:** even larger, typically needs H100 nodes.


A single H100 node serves dozens of concurrent test sessions. A smaller RTX 4090 workstation handles departmental-scale QA.


### Agent + browser containers (the cheap part)


- Agent server: 2 vCPU, 4GB RAM for hundreds of concurrent sessions
- Browser containers: ~1GB RAM each, roughly 0.5 vCPU under load
- Typical enterprise worker (16GB RAM, 8 vCPU): 10-20 concurrent sessions


## Operations and observability


Self-hosted AI agents integrate with standard enterprise ops tooling:


- **Logs:** structured JSON to stdout → Fluentd → Elastic / Splunk / Loki
- **Metrics:** Prometheus scrape endpoints → Grafana dashboards
- **Traces:** OpenTelemetry-compatible for distributed tracing
- **Reports:** archived to S3 / Azure Blob / GCS with retention policies
- **Alerts:** on failure-rate spikes, session pool exhaustion, GPU utilization, LLM latency


## Security posture


Standard enterprise security hygiene applies:


- Image provenance verified via signatures (Cosign, Notary)
- Containers run as non-root with minimum capabilities
- Network policies restrict egress (critical for air-gapped deployments)
- Single-tenant and unauthenticated by design — reached over your VPN, SSH tunnel, or an authenticating reverse proxy, never exposed directly
- Secrets (API keys, SSL certs) managed via enterprise vault integration


## Who’s running this in production


Financial services customers run the pattern we’ve described, typically with Llama 3.3 70B self-hosted, for trading platform and core banking regression. Insurance customers do the same for Guidewire deployments. Healthcare customers run against EHR front-ends. In each case, the combination of on-premises deployment + self-hosted LLM + Docker-native architecture removes all the regulatory blockers that stop cloud agents.


## Where to start


1. Read the[self-hosted AI testing](https://karatelabs.io/self-hosted-ai-testing) landing page for deployment architectures
2. Review the[enterprise evaluation](https://karatelabs.io/enterprise-evaluation) page for procurement answers
3. Pilot with[Karate Agent](https://karatelabs.io/agent) using a cloud LLM first, then swap to self-hosted in week two
4. Scale to production over a quarter


Enterprise AI browser testing doesn’t require the cloud. It requires the right architecture. And by late 2026, the on-premises pattern is the dominant one for teams that care about data, cost, and control.
