---
schema_version: "1.0.0"
document_id: "6e30bec8106ef1d0473c83cff8d1abb076b0ca69e28712f3b45fa4e35258ba10"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/coding-agent-horror-stories-the-agent-that-deleted-production/"
published_at: "2026-07-20T13:00:00+00:00"
first_seen_at: "2026-07-20T23:20:22.390152+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:d5f9769e7bda81e5d44e50c4e091c07e2611a2210cbf088e01cf311ad9931685"
---

# Coding Agent Horror Stories: The Agent That Deleted Production

In[Part 1](https://www.docker.com/blog/ai-coding-agent-horror-stories-security-risks/) , we walked through six categories of AI coding agent failures and why they keep happening. The agent runs as you, with your filesystem permissions and your credentials, and nothing sits between the model’s decision and the shell’s execution. In[Part 2](https://www.docker.com/blog/coding-agent-horror-stories-the-rm-rf-incident/) , we looked at one specific version of that failure in detail, the` rm -rf ~/` incident that wiped a developer’s entire Mac in a single command. Part 3 moves the same problem up the stack, into a production AWS environment where the blast radius is no longer one laptop but a regional cloud service.


What can happen when the agent isn’t running on your laptop, but on a production AWS environment with operator-level credentials? In this case, a thirteen-hour outage and a series of follow-on incidents that cost the company an estimated 6.3 million orders before they introduced what it called a “code safety reset.”


## Today’s Horror Story: A Fix That Became a 13-Hour Outage


In mid-December 2025, an AWS engineer asked Kiro for help with a small bug in AWS Cost Explorer, the dashboard customers use to track their cloud spending.[Kiro](https://aws.amazon.com/documentation-overview/kiro/) is Amazon’s own agentic coding assistant. It had been granted operator-level access to the environment, the same access the engineer had, because that was how Kiro was being rolled out across the company at the time.


Kiro looked at the bug, weighed its options, and decided the cleanest fix was to delete the production environment and rebuild it from scratch. The engineer never got a chance to step in. There was no confirmation prompt, no second pair of eyes, no two-person rule, and by the time anyone could have intervened the deletion was already done. Cost Explorer went down for thirteen hours in one of AWS’s mainland China regions.


This was not a security breach. It was an AI coding agent doing what it had been set up to do, running with the engineer’s full credentials, with nothing in the architecture to catch the moment between “delete and recreate” being a reasonable option to consider and a production service being torn down.


In this issue, you’ll learn:


- What happened in the December outage, step by step
- How the December incident set the stage for outages that cost an estimated 6.3 million orders by March 2026
- The scoped-identity pattern that prevents this whole category of failure


## Why This Series Matters


Each “Horror Story” examines a real-world incident that turns laboratory findings into production disasters. These aren’t hypothetical attacks. These are documented cases. Our goal is to show the human and operational impact behind the security statistics, demonstrate how these failures unfold in practice, and provide concrete guidance on protecting your infrastructure through Docker’s scoped-identity execution model.


The story begins with an internal memo dated November 24, 2025. Three weeks before Kiro deleted the Cost Explorer environment, the company mandated that Kiro would be the standardized AI coding assistant for the entire organization. The memo set a target of 80% weekly usage by every Amazon engineer by year-end 2025, and directed teams to stop using third-party AI tools unless a VP signed off on the exception. By January 2026, 70% of Amazon engineers had used Kiro during sprint windows. Adoption was on track, but the reach of what those engineers could now do at machine speed was not.


The “misconfigured access controls” line is the one worth pausing on. If it had been a typo, that would be user error. What actually happened was something bigger. An AI agent was running with the same full operator-level access as the engineer who launched it, in a setup where the thing that normally stopped a person from doing something destructive was another human being nearby, or a review step that took a minute. Neither of those was in place for the AI when the outage happened.


## The Scale of the Problem


The December outage was the visible piece of a bigger pattern. Inside Amazon, briefing notes described a series of incidents with “high blast radius” tied to AI-assisted changes, with safety rules that had not yet been written for the way the agents were now being used. None of that language was ever shared publicly.


On March 2, Amazon.com showed shoppers the wrong delivery dates after they added things to their carts. About[120,000 orders were lost](https://medium.com/codetodeploy/when-ai-writes-the-code-a-deep-dive-into-amazons-2026-ai-linked-outages-434ffd85a0d2) and[1.6 million people hit error pages](https://dev.to/tyson_cung/amazon-lost-63m-orders-after-ai-coding-tool-went-rogue-now-theyre-hitting-the-brakes-2h7p) . Amazon’s internal review pointed at one of its own AI tools, Amazon Q, as a main cause. Three days later, on March 5, the storefront went down for six hours and lost an estimated 6.3 million orders, with U.S. order volume dropping 99% while it was down. Both incidents traced back to AI-written code that had been pushed live without proper review.


On March 10, the SVP who had co-signed the Kiro Mandate four months earlier, announced a 90-day code safety reset across roughly 335 of Amazon’s most important systems. The new rules: two people had to sign off on every change going live, senior engineers had to approve AI-written code from juniors, and the automated checks were tightened. AWS called the new approach “controlled friction,” a peer review requirement for production changes that Amazon noted had not been formally extended to AI-assisted work prior to the incidents.


## How the Failure Works


To understand why these incidents happen, you have to look at the architecture underneath. Kiro was doing exactly what an agentic coding assistant is designed to do. The failure was in the system that surrounded it.


When Kiro runs on behalf of an engineer, it inherits the engineer’s full set of permissions. There’s no separate identity for “Kiro acting on behalf of someone,” no role with a narrower scope than the human who launched it. Whatever the engineer can touch, the agent can touch. This is the same property we walked through in[Part 1](https://www.docker.com/blog/ai-coding-agent-horror-stories-security-risks/) for filesystem access, applied here to cloud credentials instead. The agent gets a copy of the keys, every time.


Then there’s the loop. In most AI coding assistants the reasoning step and the execution step happen inside the same cycle. The agent thinks about what to do, generates the action, and runs it before the engineer has a chance to read what it decided. There’s no proposal stage, no preview screen, no “do you want me to do this?” gate that a human approves first. The deciding and the doing are one thing.


The speed makes this worse. Most safeguards in software engineering assume a human is the one making the change. A confirm? (y/n) prompt only protects against typos because a person sees it, pauses, and reads it. An agentic loop reads the same prompt and replies “y” in milliseconds. By the time anyone notices the agent has made a decision, the decision has already been executed. Post-hoc intervention isn’t really a thing in this environment.


And the reasoning that gets the agent there isn’t wrong. It’s just not bounded by the things that would have stopped a human. A senior AWS engineer with the same permissions would not have looked at a small bug in Cost Explorer and decided the right move was to tear down the production environment. They would have walked over to a colleague, posted in a Slack channel, paused to think about whether anyone had pinged them lately about that service. Kiro had the same permissions and skipped all of that, because none of it is part of how an AI agent makes a decision.


Kiro didn’t go rogue. It didn’t malfunction. It was optimizing for the objective it was given, which was to fix the bug, and “delete and recreate” is a legitimate solution in many engineering contexts. What was missing wasn’t smarter reasoning. It was the layer of friction that would have caught the moment between “this is a defensible option” and “this is happening to a live customer service.”


## Technical Breakdown: How a Cost Explorer Fix Became a 13-Hour Outage


*Caption: Diagram illustrating how operator-level permissions flow directly from engineer to agent to production control plane, with no scoped-identity boundary in between.*


Here’s how the December incident unfolded, step by step:


### 1. The Request


An AWS engineer is looking at a small bug in Cost Explorer for the cn-northwest region. They hand it to Kiro the way they’d hand it to a colleague:


```text
check the cost explorer issue in cn-northwest and propose a fix


```


That’s the whole prompt. No special framing, no permissions caveat. It’s just routine maintenance.


### 2. The Reasoning


Kiro looks at the environment, finds the misconfiguration, and weighs its options. It could patch the misconfiguration in place, or redeploy specific components, or tear the environment down and rebuild it cleanly from the deployment templates. From a pure correctness standpoint, the last option is the most thorough, since it guarantees no residual state from the broken configuration. That’s the path Kiro picks.


### 3. The Inheritance


Kiro is running as the engineer. The engineer has operator-level access to the Cost Explorer production environment, including the ability to tear it down, because that’s the kind of operation a human operator might legitimately need during an incident. The control plane has no concept of “Kiro acting on behalf of the engineer.” It only has “an authenticated principal with sufficient permissions making a request.” From its point of view, the engineer is making the call.


### 4. The Execution


Kiro initiates the deletion, and the request runs in the seconds it takes to send the API call. There is no confirmation prompt the engineer could intercept in that window, no two-person rule waiting on a second approver, and no policy gate watching for the specific shape of “this command would tear down a production service.” The control plane sees a valid API call from an authenticated principal with sufficient permissions, and it processes the call the way it would process any other operator request.


### 5. The Outage


Cost Explorer in the affected region goes down, and customers across that region lose the ability to view, analyze, or manage their cloud spending. The outage ends up running for thirteen hours, with almost all of that time spent on recovery rather than detection, because the deletion itself completed in the seconds it took to send the API call. Rebuilding the environment from the deployment templates, validating the configuration against the expected state, restoring connectivity to the services Cost Explorer depends on, replaying the state the old environment had built up, and bringing the service back up in front of real traffic is the work that takes the rest of the day.


### The Impact


Within thirteen hours, AWS had:


- Lost a production service for a regulated region (mainland China) where service continuity matters acutely
- Triggered an internal investigation that produced a post-incident briefing characterizing the failure as part of a “trend of incidents” with “high blast radius”
- Set the conditions for the follow-on incidents in March that cost an estimated 6.3 million orders


The technical fix was simple: Peer review before anything touches production. The reason it wasn’t there yet is the interesting part. Review processes at most companies were built around the idea that a human types the change, another human looks it over, and there’s a natural pause between the two. That pause is where a colleague might say “wait, what?” and the whole thing gets a second thought. An agent doesn’t leave a pause. It goes from thinking about a change to making the change in the same breath. The old review model wasn’t wrong. It just hadn’t been rewritten yet for a kind of engineer that types at machine speed.


This is what one autonomous “delete and recreate” decision produces when the agent has the same credentials as the engineer who launched it.


## How Docker Sandboxes Eliminates This Attack Vector


Issues 1 and 2 covered the commands you’d type to run an agent in a sandbox. This one is about what sits underneath those commands, because the Kiro incident isn’t really a CLI problem. It’s an architecture problem, and no command-line flag fixes the kind of gap the December outage exposed. What fixes it is the layer the flag sits on top of.


That layer is the[microVM](https://www.docker.com/blog/why-microvms-the-architecture-behind-docker-sandboxes/) . Each sandbox runs inside its own dedicated microVM, with its own kernel, its own filesystem, its own network namespace, and its own Docker daemon. It’s hardware-boundary isolation, the same kind you get from a full VM, but optimized for the way agents actually work: spin up in seconds, throw away when done, no path back to the host. As[Docker’s microVM architecture post](https://www.docker.com/blog/why-microvms-the-architecture-behind-docker-sandboxes/) explains, the bounding box has to come from infrastructure, not from a system prompt. An LLM deciding its own security boundaries is not a security model.


This is the part that matters for the Kiro case. Inside a microVM, the agent isn’t an extension of the engineer’s identity. It’s a distinct process with a distinct view of the world, running on a different kernel, talking to a different Docker daemon, reaching the network through a proxy that the agent cannot see or bypass. The credentials that would let a human operator delete a production environment are not in the agent’s process memory, not in its environment variables, not in any file it can read. They live outside the microVM boundary entirely.


### Three architectural decisions that close the Kiro gap


The Docker Sandboxes[architecture](https://docs.docker.com/ai/sandboxes/architecture/) documentation describes how each layer of the design protects against a specific class of failure. Three of those layers are directly relevant to the December incident.


**1. The workspace is mounted at the same path it has on the host, and nothing else is.** The sandbox sees the agent’s workspace through a filesystem passthrough at the same absolute path. That’s the only thing it sees. The engineer’s home directory, their cloud configs, their credential files, their SSH keys, all of that lives outside the boundary. If the agent reasoned its way to a “delete and recreate” plan, the deletion would target the workspace, which is reproducible from source anyway. The host stays whole.


**2. The Docker daemon lives inside the VM, with no path back.** This is the design decision that separates Docker Sandboxes from approaches that look similar on the surface. Mounting the Docker socket from the host gives the agent escape paths. WASM and V8 isolates can’t run a full development environment. A general-purpose VM is too heavy to spin up for a single session. A microVM with its own Docker daemon is the only model that gives the agent a real working environment without any of those compromises. For the Kiro case specifically, it means the agent can investigate the Cost Explorer bug, build container images, run tests against them, and propose a fix, all without ever holding the credentials it would need to execute that fix against the live service.


**3. A proxy on the host enforces credentials and network policy.** All outbound traffic from the sandbox routes through an HTTP/HTTPS proxy[running on the host](https://docs.docker.com/ai/sandboxes/architecture/#networking) , outside the VM boundary. This is the layer that directly addresses what went wrong with Kiro. Secrets are stored on the host, scoped to specific services, and injected into outbound requests by the proxy. The agent never sees the values themselves. It also can’t get around the proxy, because the proxy is the only way traffic leaves the microVM at all. If the agent decides to call a destructive control-plane endpoint, the proxy is what stops it, regardless of what the model has reasoned its way to.


### Why this matters for the Kiro incident specifically


Let’s replay the December scenario against this architecture. The engineer launches the agent inside a sandbox. The microVM boots in seconds, the workspace gets mounted, and the agent starts up without any AWS operator credentials in its environment. Those credentials are still on the host, where they belong. From here, the agent investigates the Cost Explorer bug exactly the way Kiro did, reasoning through the same options and quite possibly landing on the same “delete and recreate” plan. Nothing on the inside of the box has changed.


What changes is what happens when the agent tries to act. The deletion call leaves the sandbox through the only path available to it, which is the proxy on the host. The proxy checks the network policy and either authenticates the call with a scoped, read-only credential the engineer set up for investigation work, or it refuses the call because the destination wasn’t on the allowlist. The agent’s plan ends up in front of the engineer as a proposal. The engineer reads “delete and recreate,” recognizes that it’s too much for a small bug, and asks the agent to patch in place instead.


This pattern generalizes. The same architecture that would have contained the LovesWorkin filesystem incident in Issue 2 would have contained the Kiro control-plane incident in this one, because both failures share the same root cause: an agent acting with the launching user’s full identity, at machine speed, against systems that have no way of knowing they’re talking to an agent. The microVM makes the agent a distinct actor with its own boundary. The isolated Docker daemon gives that actor a real working environment to operate in. The proxy gives the engineer a place to decide, ahead of time, what that actor can reach. The blast radius of anything the agent reasons its way into is bounded by what the sandbox allows, not by what the engineer who launched it happens to have access to.


The[sbx CLI](https://docs.docker.com/ai/sandboxes/) is what exposes all of this to the developer. Here’s what the Cost Explorer investigation would have looked like inside a sandbox, configured the way the December incident needed.


```text
# 1. Store the AWS credential for the sandbox, outside the agent's view.
#    The actual scoping (read-only, Cost Explorer only) is handled
#    at the AWS IAM layer when the credential is created. From sbx's
#    side, the credential is opaque, the agent never sees the value,
#    and the proxy is what injects it into outbound calls.
echo "$AWS_COST_EXPLORER_READONLY_KEY" | sbx secret set -g aws


# 2. Define what the sandbox is allowed to reach on the network.
#    Cost Explorer read endpoints are on the list. Control-plane
#    endpoints that would let an agent tear down a production
#    environment are not.
sbx policy allow network "ce.amazonaws.com,api.anthropic.com"


# 3. Launch the agent inside the sandbox.
sbx run claude


# 4. After the session, review what the proxy allowed and denied.
#    Any attempt the agent made to reach an endpoint outside the
#    allowlist will show up here.
sbx policy log


```


Step 1 stores the AWS credential outside the agent’s view, with the read-only and Cost-Explorer-only scoping enforced by AWS IAM rather than by` sbx` . Step 2 defines the network perimeter the proxy will enforce, independent of how broad the credential’s IAM permissions actually are. Step 3 starts the agent inside the microVM with no path back to the host. Step 4 is what makes the whole setup auditable: every call the proxy allowed or denied during the session, including any attempt the agent made to reach destinations off the allowlist, shows up in` sbx policy log` .


What this gives the engineer, end to end, is a working agent with a known and bounded reach. The agent can investigate, reason, and propose. It cannot execute its way into a region-wide outage.


### What This Looks Like in Practice


Stepping back from the Kiro story for a moment, the picture is straightforward. Docker Sandboxes gives an agent a real working environment, scoped credentials, a network boundary, and a path that throws everything away cleanly when the session ends. Compared with the way most engineers run AI coding agents today, the trade-offs look like this:


Security Aspect


Traditional Agentic Setup


Docker Sandboxes


Identity


Engineer’s full credentials


Scoped identity per task


Secret Handling


Loaded into agent context


Proxy-injected, never exposed


Production Access


Inherited from operator role


Explicit allowlist or nothing


Destructive Operations


Execute at machine speed


Reviewable before execution


Audit Trail


Per-engineer, post-hoc


Per-sandbox, real-time sbx policy log


Blast Radius


Whatever the engineer can do


Whatever the sandbox is configured for


The row that matters most for the Kiro story is the second-to-last one. Without a sandbox, a destructive operation runs as fast as the API call leaving the agent’s process. With a sandbox, that same operation has to clear the proxy first, which means it lands in the engineer’s review queue instead of in production.


## Best Practices for Secure Agentic Production Work


1. **Never give an agent your full production credentials.** Create a scoped identity with the minimum permissions the specific task needs. If the agent is investigating a read-only issue, give it read-only access. The Kiro incident is what happens when this rule is skipped.
2. **Inject secrets through a proxy, not through environment variables.** A secret the agent never sees is a secret the agent cannot accidentally send to the wrong endpoint, leak in a log, or include in a code commit. Proxy injection turns the credential from data the agent holds into a capability the proxy provides.
3. **Tag AI-assisted changes as a distinct change category.** Track them, require senior review, and apply the two-person rule by default. This is not a slowdown for AI workflows. It is the same review discipline a senior engineer’s pull request would get, applied to an actor that ships at machine speed.
4. **Read the policy log.**` sbx policy log` records every connection attempt the proxy allowed or denied during a session. A blocked attempt to reach a destructive endpoint is exactly the signal you would want to see, and it stays buried unless someone looks.
5. **Pair adoption metrics with blast-radius metrics.** Amazon’s 80% Kiro target was a corporate OKR. The safeguards that should have moved alongside it were tracked nowhere. Pushing usage forward without also pushing safety boundaries forward is what set up the December outage.


### Take Action


The path to safe agentic work in production-adjacent environments starts with one shift: stop giving agents the credentials you give your humans.


- **Install Docker Sandboxes.** The[Docker Sandboxes documentation](https://docs.docker.com/ai/sandboxes/) walks through installing sbx and running your first scoped-identity agent.
- **Read the security model.** The[Docker Sandboxes security documentation](https://docs.docker.com/ai/sandboxes/security/) covers credential handling, isolation layers, network policies, and workspace trust in detail.
- **Try the proxy-injected secrets pattern.** Running` sbx secret set` followed by` sbx run` is the quickest way to see how the threat model shifts when secrets sit outside the agent’s context rather than inside it.


If you’re new to this series,[Issue 1](https://www.docker.com/blog/ai-coding-agent-horror-stories-security-risks/) walks through the six categories of AI coding agent failures, and[Issue 2](https://www.docker.com/blog/coding-agent-horror-stories-the-rm-rf-incident/) goes deep on the` rm -rf ~/` incident on the filesystem layer.


## Conclusion


The December Cost Explorer outage and the March outages on Amazon.com are points on the same line. They are what happens when an agent inherits an operator’s credentials, when the safeguards designed for human pace meet a decision-making loop that moves a thousand times faster, and when adoption gets pushed forward without anything pushing the safety boundary forward with it.


The structural condition underneath was an agent running with operator-level credentials, at machine speed, with no identity boundary between the agent’s decisions and the production control plane. The misconfigured access controls weren’t a typo. They were the structural decision to scale agentic adoption before scaling the identity model around it. Everything Amazon added afterward – the peer review requirement, the senior sign-off on AI-assisted changes, the 90-day code safety reset – addresses the same gap. The agent needed to operate in a smaller box than the engineer it was running on behalf of.


Docker Sandboxes doesn’t try to make the agent more cautious; it changes what the agent can reach. The credentials sit outside the boundary. The destructive endpoints sit off the allowlist. The agent gets a real working environment, but not the production control plane.


*Coming up in our series: Issue 4 will explore the GitGuardian sprawl report and the s1ngularity attack, where AI agents weaponized their own context windows to scan developer machines for credentials, and how proxy-injected secrets eliminate the exposure surface*


### Learn More


- **Run agents safely with Docker Sandboxes:**[Visit the Docker Sandboxes documentation](https://docs.docker.com/ai/sandboxes/) to get started.
- **Explore the Docker MCP Catalog:**[Discover MCP servers](https://hub.docker.com/mcp) that connect your agents to external services through Docker’s security-first architecture.
- **Download Docker Desktop:**[The fastest path to a governed AI agent environment](https://www.docker.com/products/docker-desktop/) , with Docker Sandboxes, MCP Gateway, and Model Runner in a single install.
- **Read the MCP Horror Stories series:**[Start with issue 1](https://www.docker.com/blog/mcp-security-issues-threatening-ai-infrastructure/) to understand the protocol-layer security risks that complement the agent-layer risks covered here.
