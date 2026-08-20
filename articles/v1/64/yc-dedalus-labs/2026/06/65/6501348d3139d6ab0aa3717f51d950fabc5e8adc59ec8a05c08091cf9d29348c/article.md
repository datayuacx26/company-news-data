---
schema_version: "1.0.0"
document_id: "6501348d3139d6ab0aa3717f51d950fabc5e8adc59ec8a05c08091cf9d29348c"
company_key: "yc-dedalus-labs"
company: "Dedalus Labs"
source_id: "yc-dedalus-labs-news-import-c6fd8a49c5f3"
canonical_url: "https://www.dedaluslabs.ai/blog/dedalus-machines-journey"
published_at: "2026-06-29T20:00:00+00:00"
first_seen_at: "2026-07-21T15:59:30.110359+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:ff6fb1eaca36926c16b20c120c14793b478d1e3915864b50f0af8997a384aa8e"
---

# Why we are building virtual machines for AI agents

When we first started building agents with[our Agents SDK](https://github.com/dedalus-labs#dedalus-sdks) , we needed something to host them on. We tried several “agent sandbox” providers, and they all faced the same issues: non-negotiable tradeoffs that made agent development messy.


Providers that promised speed compromised security and persistence. Providers that promised persistence offered machines that were slow and more expensive. And if you wanted to save on both speed and cost, then you’re stuck with limited runtime hacks, like V8 isolates or Web Assembly.


Everyone claimed to be selling a sandbox for agents. But no one had everything an agent *needs* .


Our users were facing the same challenges. Most developers started hosting and testing agents locally, which is pretty straightforward: your agent lives on your computer, often with full access to your files, your context, and your life (more so the case after the OpenClaw hype).


But that’s just the tip of the iceberg.


When you start building agents at scale, with thousands of people using it everyday, you need to host your agents in the cloud. Suddenly, you need to start worrying about the scope of each agent: what files and credentials should this agent be allowed access to? How do I stop one user’s agent from sharing context with another user’s agent? How can I dynamically scale the amount of compute I am using for each task?


Using a single machine just won’t cut it. You need separation.


There are a few ways developers achieve separation today. One option is containers, a common way to isolate software from other applications on the same host machine. Containers worked great for SaaS code and seemed like the obvious solution to agent use cases too. They’re quick to start, lightweight, and enable several agents to run on the same host machine. However, containers have a fundamental problem: they lack kernel-level isolation.


Things were more controlled and pre-defined in the old days of running SaaS apps, but once one or more AI models enters the equation, you introduce unpredictability and non-determinism. Couple that with your agents executing untested code or touching unverified inputs, and you have a[ticking time bomb](https://emirb.github.io/blog/microvm-2026/) with a blast radius of every single container that shares the same host kernel.


Containers don’t work for AI agents; you need a virtual machine (VM). VMs offer strong isolation: every VM has its own kernel and can run workloads of any size. However, the problem with most VMs today is they are slow and bulky, with start times measured in seconds. For agents that need on-demand compute in an instant, a few seconds to minutes is an eternity, delaying responses and increasing latency.


Neither containers nor traditional VMs satisfied us or our customers. We were being forced to cram agents into slow, expensive machines that could only run for a few hours at a time, or make tradeoffs in security for container start times.


We have high standards for the software we make and use. When something doesn’t live up to those standards, we build the version everyone deserves. That’s why we built[Dedalus Machines](https://www.dedaluslabs.ai/about) .


Dedalus Machines is the first agent hosting platform that doesn't make you compromise. Each machine offers the full flexibility of a Linux computer with VM-level isolation, true persistence that lasts for as long as you want, sub-second start times, and pricing that doesn't charge for idle time.


We achieved this through a few engineering miracles. We developed our own world-class filesystem, built our own custom virtual machine monitor (VMM) forked from the Cloud Hypervisor[project](https://github.com/cloud-hypervisor/cloud-hypervisor) , and engineered numerous other optimizations that allowed us to do the seemingly impossible: provide agents with full Linux machines that are fast, persistent, and flexible.


The cherry on top is that Dedalus Machines were designed to be agent-native. You can build an entire agent platform on top of Dedalus Machines that agents can access natively through a CLI interface and programmatic API access. If you really wanted to, you could even build Dedalus Machines on a Dedalus Machine.


We built our machines with a first-class agent interface, zero cold starts, and billing that doesn’t charge for idle time because we think a large portion of our users will be agents themselves. We believe these design choices will help enable a robust agent-to-agent economy.


As part of our commitment to giving back to the developer community, we will be publishing research and open sourcing parts of our stack, starting with our new filesystem that powers Dedalus Machines.


Dedalus Machines represents the culmination of months of hard work in the core infra layer. It was a difficult problem to solve, and one that still excites our team. If you also enjoy solving hard problems,[join us](https://www.dedaluslabs.ai/careers?gad_campaignid=23741175255&gbraid=0AAAABDF6n92gJbTYJVI5udHRbiPdRmWFw) .


We hope you are as excited as we are.
