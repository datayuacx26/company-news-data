---
schema_version: "1.0.0"
document_id: "4a9e5d296d8ba935c9ac7c77bac16a668bcf22ea9dedfd41fef3186d5096a5af"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/2025-in-review-metrics-releases-and-enabling-the-agentic-era/"
published_at: "2025-12-18T08:53:11+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:daba4c2956d2465ac283f2763b36d93331590c61bbaf0993771fd2125e0cffab"
---

# 2025 in Review: Metrics, Releases, and Enabling The Agentic Era

2025 was a huge year for Signadot. It was the year that ephemeral environments shifted from being a “nice-to-have” to an essential tool for high-performing engineering teams building microservices and AI agents.


From releasing Operator and CLI 1.0 to enabling the next generation of development in the agentic era, here’s a look at what we accomplished together in 2025.


---


## The Year in Numbers


The most telling story of 2025 is in the usage data. We saw massive acceleration in platform adoption, particularly in the second half of the year, but the numbers also reveal a shift in workflow.


The data shows that Signadot is having the impact it is supposed to, giving teams the ability to iterate in the inner loop. Developers are updating sandboxes regularly and cleaning them up at a nearly 100% rate, treating them with the same fluidity as local environments.


**2025 at a glance:** 82,098 sandboxes created · 73,284 sandbox updates · 99.6% cleanup rate · 4x growth in monthly sandbox creation from January to peak months


### 🚀 82,098 Sandboxes Created


You’ve all been busy! We processed over 82,000 sandbox creation events in 2025. To put that in perspective, monthly sandbox creation grew 4x from January (2,714) to our peak months (10,000+).


### 🔄 73,284 Sandbox Updates


This number tells a deeper story than just sandbox creation. We saw over 73,000 update events, meaning developers are actively iterating within their sandboxes.


They aren’t just spinning up an environment and walking away. They are refining code, pushing changes, and testing in a tight feedback loop. This high volume of updates confirms that Signadot is being used exactly as intended: enabling a high-fidelity inner loop for cloud-native development.


### 🧹 99.6% Cleanup Rate


This might be our favorite metric of the year. Of the 82,098 sandboxes created, 81,744 were deleted.


Why does this matter? It proves that ephemeral environments work. Developers are spinning them up, running their tests, and tearing them down immediately. It shows incredible resource efficiency and platform hygiene. There are no “zombie” environments clogging up clusters.


### 🔥 The Wednesday Peak


If you felt busiest in the middle of the week, you weren’t alone. Wednesday was officially the most active day on Signadot in 2025, accounting for nearly 20% of all traffic.


In fact, our busiest single day of the year was Wednesday, November 26, when developers spun up 726 sandboxes in just 24 hours.


---


## Product Highlights


While the numbers grew, the platform evolved. We launched three major releases this year that delivered a more mature core product experience, powerful local development tools, and enhanced ability to support modern development workflows in the agentic era.


### 1. Operator and CLI 1.0: Enterprise Ready


We officially released Signadot Operator and CLI into 1.0 in July. This release marked a milestone in stability and maturity, but it also introduced native support for Istio Ambient Mesh, which was a huge unlock for many of our users.


By supporting Ambient mode, we’ve made it possible to adopt Istio for request routing without the sidecar tax, making sandboxes lighter, faster, and cheaper to run.


##### [Read the 1.0 announcement](https://www.signadot.com/blog/signadot-operator-cli-hit-1-0-now-with-istio-ambient-support-and-enhanced-local-dev/)


### 2. SmartTest Enhancements


Testing is only useful if it fits into your workflow. We added native git support and new CLI commands to SmartTests, allowing you to define and execute tests based on code changes directly from your terminal.


This means you can now store your SmartTests alongside your application code in your git repository. When you run signadot smart-test run, the CLI automatically discovers the relevant tests for your modified services and executes them against your sandbox or baseline. This makes it much easier to integrate high-fidelity API testing into your CI pipelines and local inner loops.


##### [Read the guide](https://www.signadot.com/docs/guides/smart-tests/smart-tests-git)


### 3. Backstage Plugin


For teams using Backstage as their Internal Developer Portal (IDP), we released a dedicated plugin to bring Signadot directly into your daily dashboard.


The plugin provides a unified view where you can verify the status of your sandboxes, see active preview URLs, and monitor SmartTest execution results without context switching. It automatically surfaces critical details like associated route groups and routing keys, making it easier for platform teams to expose ephemeral environments to the wider engineering organization.


##### [Get the Backstage plugin](https://github.com/signadot/backstage-plugin)


### 4. Multi-Cluster Route Groups


As organizations scale, services often get split across multiple clusters. We introduced multi-cluster support for Route Groups to ensure your testing strategy scales with your infrastructure.


This feature allows you to join sandboxes residing in different clusters into a single cohesive Route Group. Provided header propagation is in place, you can now route requests seamlessly across cluster boundaries, enabling true end-to-end testing even when your dependencies are distributed across the fleet.


##### [Read the docs](https://www.signadot.com/docs/reference/route-groups/usage#multi-cluster-route-groups)


### 5. Signadot Local: Fidelity in the Inner Loop


One of our biggest launches this year was Signadot Local. We spoke to our customers and looked at usage patterns, and the message was clear: devs want the fidelity of the cluster with the speed of their local IDE.


Signadot Local gives users unprecedented traffic visibility and control, along with the ability to write code locally and test changes end-to-end against real services. This means developers can now code, test, and debug locally against a baseline environment before they merge code.


- **Traffic Record & Inspect:** Record live traffic between any services in your cluster and inspect in your local environment for fast, accurate integration debugging in the inner loop.
- **API Override:** Intercept and override any API response to test edge cases, simulate failures, and validate resilience locally in seconds.


All this comes together to deliver a “hot reload” experience for the backend, shifting high-fidelity testing into the inner loop.


##### [Check out the quickstart guide](https://www.signadot.com/docs/tutorials/quickstart/local-development)


### 6. Signadot MCP Server: Ready for the Agentic Era


And last but certainly not least, we unveiled the Signadot MCP (Model Context Protocol) Server.


Developers shouldn’t have to leave their IDE to test and validate their work. The Signadot MCP Server enables you to construct preview and test environments directly within your workflow. It automatically identifies services, ports, and dependencies to build Signadot Sandboxes and Route Groups, allowing you to iterate faster without fighting infrastructure complexity.


At Signadot, we are dedicated to building the infrastructure that will enable developers and teams to fully realize the benefits of the agentic era. The Signadot MCP server is just the beginning!


##### [Read the docs](https://www.signadot.com/docs/integrations/mcp)


---


## Looking Ahead: What’s Coming in 2026


We are entering 2026 with more momentum than ever. Here’s a sneak peek at what will be shipping next:


- **Agent and MCP Development:** We’re working on new tooling to enable teams building agents and MCP servers to ship to production faster and with more confidence. Stay tuned!
- **Developer Pods as CDEs:** We are reimagining the remote development experience. Soon, you will be able to spin up dedicated developer pods that integrate directly with your IDEs and coding agents, giving you a seamless cloud-based dev environment.
- **Self-Hosted Preview Servers:** We’re also working on allowing you to stand up custom preview servers for internal use, giving you more flexibility in how you share and test changes.


---


## Thank You!


To the organizations building with Signadot and the thousands of developers spinning up sandboxes every Wednesday: Thank you!


Your feedback and input have been instrumental in shaping the product in 2025. Please keep sharing your experiences with us as we continue to improve Signadot so you can build, test, and ship reliable software even faster in 2026.
