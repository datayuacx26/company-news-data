---
schema_version: "1.0.0"
document_id: "1fc33065b28472701dfadc8dd293d83d4320dcdf56fe4de7e9080ec8e17272e4"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/building-crabtrap-open-source"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-21T11:29:48.523536+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:0fafbed7699b49d8a29549ab594f44db72ff71acea73b7ff86ca39c23ede69e7"
---

# CrabTrap: an LLM-as-a-judge HTTP proxy to secure agents in production

## **Using LLMs to judge the network traffic of an AI agent**


At Brex, our AI agents do real work in production environments. In the journey of deploying harnesses like OpenClaw in production, Brex faced the same wall as the rest of the industry: agents need real credentials (API keys, OAuth tokens, and service accounts), but can hallucinate destructive actions or get prompt-injected. As soon as these requests leave the process, it hits APIs with production consequences.


The obvious fix is guardrails. Much of the early work has coalesced around scoped tools, per-action permissions, and human-in-the-loop approvals. The challenge is that as agents get more capable, every new capability means another hand-tuned API token or surface to audit. The guardrails that actually exist tend to fall into two extremes: either they're so restrictive the agent can't do its job, or they're so bespoke they don't scale.


So we built CrabTrap: an open-source HTTP/HTTPS proxy that intercepts every request an AI agent makes and uses LLM-as-a-judge to determine if the request matches a policy of allowed traffic for that agent. The results are promising; we believe it’s a meaningful step forward in the security of agent harnesses in production environments.


## **Why existing approaches weren’t enough**


Existing approaches solved pieces of the problem. MCP gateways enforce policy at the protocol layer, but only for traffic that uses MCP. Guardrails from the LLM provider are tied to a single model and opaque to customize with your own policies. A solution like NVIDIA OpenShell, while powerful, is more of a per-sandbox egress control. We needed a solution that sat between every agent and every network request, and could make nuanced decisions about what to allow.


The net result is that while OpenClaw is the fastest-growing project on GitHub, there are few successful cases of enterprise deployments. Brex decided to change that.


## **How it works**


A core architectural decision to secure our agents was to operate at the transport layer. This way, the proxy is agnostic to the framework, language, and APIs involved.


You set HTTP_PROXY and HTTPS_PROXY in the agent's environment, and every outbound request routes through CrabTrap before reaching the destination. No SDK, no wrappers, and no per-tool integration. For HTTPS, CrabTrap performs TLS interception: generating a certificate signed by its own CA per host, negotiating TLS with the client, then opening a separate TLS connection upstream. The deployer can also add iptables rules in the agent's container to block direct outbound connections, not destined to the proxy, as an extra layer of enforcement.


Under the hood, every request the agent makes is evaluated in two stages:


First, static rules: deterministic pattern matches against the URL (i.e. prefix, exact, or glob), optionally scoped to specific HTTP methods. Deny rules always take priority. Static rules compile to cached regexps and execute in microseconds.


But pure static rules can't handle the long tail. If no static rule matches, the request goes to the LLM-as-a-judge. The judge receives the full request context along with a natural-language policy assigned to that agent, and returns a structured JSON decision (ALLOW or DENY) with a reason.


This approach enables speed for known patterns and judgment for everything else.


## **Generating policies from network traffic using LLMs**


This raises a natural question: how should you define an effective natural language policy for an agent? As we know from expense policies, writing a good policy is difficult. Something that sounds reasonable often blocks actions it shouldn’t. So how do you predict what an agent's actual policy should be?


We built two systems to close that gap.


First is a policy builder, itself powered by an agentic loop. Instead of writing policy rules first and hoping they match reality, the philosophy is to observe reality and infer an appropriate policy from it. The policy builder analyzes the agent’s historical traffic, samples representative network calls, and drafts a policy matching the agent's real behavior.


The second is an eval system to test policy changes before they go live. CrabTrap can replay historical audit entries against a draft policy and report what would change with any policy updates. Results can be sliced by method, URL, original decision, and agreement status. Evals run with concurrent judge calls, so replaying thousands of requests completes in minutes. All past requests are logged in PostgreSQL, indexed and queryable through the admin API and web dashboard.


## **What the judge actually sees**


Building the LLM judge meant solving a specific prompt engineering problem: giving a model enough context about an HTTP request to make a security decision, without letting the request itself become an attack vector.


CrabTrap sends the full request to the judge as a structured JSON object (method, URL, headers, body) so all user-controlled content is escaped rather than interpolated as raw text. This prevents prompt injection through crafted URLs, headers, or body content. Security-relevant headers are prioritized and total header content is capped at 4KB, preventing prompt inflation attacks that stuff headers with junk to displace the policy from the context window. Request bodies are truncated at 16KB with an explicit warning to the model. Multipart requests are replaced with a structured summary of each part rather than sent raw.


## **Learnings from running CrabTrap in production**


Brex runs CrabTrap with OpenClaw agents doing real work in our corporate environment. A few learnings have stood out so far:


1. **Policies derived from traffic are surprisingly strong.** We expected the policy builder to produce a rough starting point needing heavy manual editing ([watch the demo video](https://www.brex.com/crabtrap#Demo) ) In practice, pointing it at a few days of real traffic produced policies that matched human judgment on the vast majority of held-out requests. Starting from observed behavior and editing down is far more effective than starting from a blank page.
2. **Latency was the first question everyone asked, and it turned out to be a non-issue.** The obvious concern with a proxy between an agent and every API is added latency on every request. In practice, agents settle into predictable traffic patterns quickly, and once you've observed those patterns, the high-volume ones become static rules. The LLM judge only fires on the long tail of unfamiliar endpoints or unusual request shapes. On one production use case, we saw the LLM activate on fewer than 3% of requests.
3. **The** **proxy** **became** **a** **discovery** **tool,** **not** **just** **an** **enforcement** **one.** Deploying CrabTrap on a production agent revealed how much noise agents generate. The audit trail made this visible for the first time. We started using denial logs and traffic analysis not just to tune policies, but to go back and tighten the agents themselves by removing tools and cutting out entire categories of requests that were wasting time and tokens.


## **Why open source**


CrabTrap is experimental. It works well for us today, but the space is young, the attack surface is evolving, and we don't think any single approach is the last word on agent security. We're open-sourcing CrabTrap for three reasons:


First, CrabTrap is useful infrastructure. When we started, we hadn’t found a solution to deploying harnesses like OpenClaw safely. Instead of waiting for the industry to catch up, we decided to own the problem and invent the necessary tools.


Second, CrabTrap gets better with more users. Our agents talk to a specific set of APIs. Teams using and deploying CrabTrap in front of different agents, services, and policy requirements will surface edge cases and patterns we can't hit alone.


Third, we have ambitious plans for where it could go, and we’d rather build in the open alongside you. Improvement areas include deeper authentication functionality (like SSO, fine-grained RBAC), escalation workflows that let agents request additional permissions, and policy recommendations from denial patterns.


## **Try it out**


To deploy CrabTrap or view the repo, check out the[quickstart](https://github.com/brexhq/CrabTrap/blob/main/QUICKSTART.md) . To watch an interactive demo of CrabTrap, visit our landing page[here](https://www.brex.com/crabtrap) .


## **Come build with us**


At Brex, we build the infrastructure AI agents run on, because most of it doesn't exist yet. If you want to work on the hard problems nobody has solved yet, join us at[brex.com/careers](https://www.brex.com/careers) .
