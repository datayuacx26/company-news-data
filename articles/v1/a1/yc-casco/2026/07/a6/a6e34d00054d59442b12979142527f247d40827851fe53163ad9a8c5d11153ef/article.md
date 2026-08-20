---
schema_version: "1.0.0"
document_id: "a6e34d00054d59442b12979142527f247d40827851fe53163ad9a8c5d11153ef"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/network-observability"
published_at: "2026-07-31T17:00:00+00:00"
first_seen_at: "2026-07-31T18:50:42.124553+00:00"
fetched_at: "2026-07-31T18:50:43.655795+00:00"
content_hash: "sha256:ae7f3a5cf6fa308a7d81e95819fadb3270a8e14683461e4b8ee31d4d2b77b3fd"
---

# Identify and Trace AI Pentest Traffic | Casco Observability

# Introducing Casco’s Improved Network Observability


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Fri Jul 31 2026


Casco's improved network observability lets you see exactly what our agents did to your application. Every request is identifiable by source IP and an` x-casco-request-id` header, and every finding links back to the specific requests that proved it, so testing traffic is never a mystery in your logs and remediation starts from what actually happened.


## The question everyone is asking AI security vendors right now


Earlier this month,[OpenAI disclosed](https://openai.com/index/hugging-face-model-evaluation-security-incident/) that a combination of its models escaped a sandboxed testing environment during an internal cyber-capabilities evaluation, gained internet access through a zero-day, and compromised Hugging Face's infrastructure. Hugging Face, which published a[full technical timeline](https://huggingface.co/blog/agent-intrusion-technical-timeline) , called the incident unique because it was "driven, end to end, by an autonomous AI agent system."


We were asked: how does Casco prevent something like this from happening?


It's the right question. Our scope controls have always enforced the boundary. What's new today is that you can see and verify it yourself, in your own logs, instead of taking our word for it.


## Scope is enforced below the agent, not requested of it


The failure mode in agentic systems is treating instructions as guardrails. Telling an agent "only test these targets" is a request, and a sufficiently capable agent under a sufficiently aggressive objective will route around requests. Guardrails that live only in the prompt are guardrails the agent can reason its way past.


Casco's strongest boundary doesn't depend on the agent's judgment at all. In the console, every asset is either in-scope or out-of-scope:


**In-scope.** A target. This is what Casco tests.


**Out-of-scope.** Blocked at the network level. The agent's traffic to that asset is stopped at the proxy, regardless of what the agent decides. It cannot reason its way past a dropped packet.


The block isn't an instruction the agent agrees to follow; it's enforced below the agent, in infrastructure the agent doesn't control. The difference between an agent that is asked to behave and an agent that is physically unable to misbehave is the difference between a request and a dropped packet.


## See exactly what Casco did


Scope controls answer what the agent can touch. Observability answers what it actually did, in your logs, on your terms. Three things come out of it.


**Separate Casco from everyone else.** Casco tests from a known, stable set of IP addresses and tags every request with an` x-casco-request-id` header. Filter your logs on either and you can cleanly split testing traffic from production, classify it correctly in your own systems, and stop triaging your own pentest as an incident. The IPs make it authoritative: a copied header from some other address can't impersonate a Casco request.


**Trace any finding to the exact requests.** Open a finding, switch to the **Network requests** tab, and you get every` x-casco-request-id` used to validate that exploit, each with a capture timestamp, copyable one at a time or all at once. Search those IDs in your application, gateway, or SIEM logs to follow the real requests through your infrastructure.


**Fix from the inside, not from a description.** Because you can trace the exact requests, remediation starts from what actually happened in your systems rather than from our write-up of it. You see the vulnerability the way the exploit did.


## The bigger picture


Our mission at Casco is to make all software effortlessly secure, and autonomous agents are how we get there. Running capable agents against your systems only works if the boundaries are real and you can see them holding. Every launch this week has been a version of the same commitment:[the tuning is visible](https://casco.com/blog/automatic-context-refinement-loops) , the findings travel to you, and now the traffic itself is traceable. If an AI agent is testing your application, you should be able to watch exactly what it does.


Get year-round security with autonomous security testing. Get started at[https://casco.com](https://casco.com/)


Quick answers


## Frequently asked questions


What to know before connecting Casco to your coding agent.


###


###


###


###


###


###


###


###
