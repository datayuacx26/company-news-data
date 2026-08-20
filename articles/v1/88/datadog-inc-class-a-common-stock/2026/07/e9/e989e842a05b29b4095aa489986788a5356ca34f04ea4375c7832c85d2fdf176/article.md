---
schema_version: "1.0.0"
document_id: "e989e842a05b29b4095aa489986788a5356ca34f04ea4375c7832c85d2fdf176"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/how-datadog-uses-ai-to-build-internal-software-delivery-tools-and-improve-system/"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:6f39456ddbfd1ce360364f994619169d575b2a1e481160eabb00e9d4862252d7"
---

# Three internal tools Datadog built with AI that improved developer experience and system performance

Joe McGovern


Alex Zvorygin


Alex Bianchi


Colin Nichols


At Datadog, we want our developers to become better at using AI tools with the end goal of **building quality software, faster, that generates real value** . **** This includes not only the products and features that our customers use, but also the internal tools that help keep our workflows running smoothly behind the scenes.


In this blog post, we’ll cover a few examples of internal tools that Datadog developers have built using AI and the impact they’ve had on our software development life cycle (SDLC). These projects do the following:


-


Help our support staff service more internal deployment tickets


-


Build a unified shadowing platform to test deployments


-


Increase the performance of our backend systems


## Automating our response to internal deployment assistance


Datadog has a platform engineering team responsible for developing internal deployment tools and the release infrastructure that engineers rely on to deploy and troubleshoot changes across environments. One of their core responsibilities (among many others) is helping engineers from other teams resolve their deployment issues. These requests are posted in a Slack channel that is constantly monitored by a rotating schedule of platform engineers, who troubleshoot these issues during their shift. The channel receives between 120 to 240 support requests weekly.


This volume, combined with the wide breadth and complexity of issues, requires multiple responders to handle requests at all times, and even then, not every request can be answered within a timely manner. Responders need to prioritize which issues require immediate attention, and the remaining requests get logged as Jira tickets. At times, lower-priority tickets could take days to be resolved or be buried in the backlog of requests.


To address this issue, our platform engineers built an automated AI support application using[Gas City](https://github.com/gastownhall/gascity) , an agent orchestration SDK, to improve their throughput during each shift. By orchestrating a network of agents, the application is able to automate an end-to-end workflow in which it processes incoming requests, investigates them to identify the issue, drafts a message to respond to the initial support request (this needs to be approved by the on-call responder), and if a bug or potential fix is identified, opens a PR in a feature branch that the team can review and merge.


This workflow includes multiple agents that are each responsible for the specific tasks, in chronological order:


-


` **dispatcher**` polls the \`deployments-help\` Slack channel on a regular basis for new requests. It filters out requests that don’t require investigation, as well as requests that are being handled by a human responder.


-


` **first-responder**` gathers context from Slack threads and then launches an investigation by reading source code, documentation, and Datadog telemetry. Before it can finalize claims to send to the end user, it has to complete two adversarial loops with \`support-staff\`.


-


` **support-staff**` **** demands concrete evidence for each of \`first-responder\`’s claims. This can include links to documentation, Datadog context, or specific lines of source code. The agent then validates the evidence provided for each claim and approves the final summary and the list of changes proposed before these are sent to the user.


-


` **fixer**` makes code changes and opens a PR if the previous agents surface a bug or agree to implement a feature improvement.


-


` **boss-man**` continuously improves the application by analyzing agent sessions. It identifies when agents use tools incorrectly, adjusts prompts, and recommends skills for better performance.


Our platform engineers reported that using this application enabled them to resolve significantly more support requests per shift. Before we rolled the app, engineers working on particularly complex issues might not be able to resolve even one incident per shift. After testing the application, engineers reported resolving up to eight requests per shift. Improving the time to resolution on these requests frees up responders’ bandwidth and also unblocks the developer originally submitting the request, since their deployment relies on the issue being resolved.


## Building a shadowing platform to validate query changes at scale


Making changes to our backend query systems that handle data retrieval is difficult. At Datadog’s scale of operations, it is not feasible to predict and test every edge case a query creates. Unlike a traditional database, we don’t require customers to define their schemas before they submit data; while this gives customers greater flexibility, it makes our systems more challenging to test and validate. Without a comprehensive validation workflow, subtle differences in output nullability, type inference, or default sort orders can go undetected. For example, changes to one service can introduce regressions in downstream services that aren’t immediately obvious. Testing these changes against synthetics data in our staging environment wasn’t foolproof enough; our teams needed a way to test against real production traffic.


To solve this, our engineers built a shadowing platform using Claude Code and Cursor. A shadowing platform (or the act of shadow-testing) is a system that receives production traffic, duplicates these requests, and runs these duplicate requests through production services with read-only access. This enables developers to test their changes against real-world conditions and compare outputs against the production environment. To build this, we gathered context from existing shadow deployment implementations used by different teams to inform an initial design, and then heavily iterated on it using Claude’s plan mode. Planning was done by one agent, executed in another agent thread, and then reviewed by the initial planning agent. The end result was a self-service model where teams only needed to implement a handler layer on top of the unified platform that handled requests to the team’s area of product ownership rather than developing a bespoke implementation on a per-team or per-service basis.


When a service receives a gRPC request, the shadowing platform duplicates the request so that it can be processed by different handlers. Developers are then able to modify the request using their handler to create different variants of the request depending on the changes they’d like to test. For example, a team validating a new experimental feature might define a variant that takes the production request and adds` enable_experimental_feature=true` to it. The platform executes each variant, compares the results against the original production query, and surfaces any discrepancies in a centralized dashboard for engineers to review. Variant requests propagate through the full request path, allowing teams to validate behavior across downstream services. As a result, even if only a handful of services are directly integrated with the shadowing platform, each test provides visibility into a much larger portion of the request flow.


Once the shadowing platform was deployed, we migrated traffic of over hundreds of thousands of queries per second to its query stack. Teams are able to see how their changes impact real traffic and identify the queries most relevant to their testing, eliminating much of the previous guesswork involved. The platform also enables broader AI-assisted development workflows. While AI tools can accelerate implementation, shadowing provides the production validation engineers need to confidently verify and deploy those changes.


Using AI tools to create the platform also helped establish a shared ownership model. Prior to the shadowing platform, teams were responsible for creating their own validation tools. This fragmented approach siloed valuable features behind implementations designed for a specific service. However, after adopting the shadowing platform, engineers can easily use features deployed by other teams as long as they build a handler layer. Our engineers are now able to focus on building new testing features that any team can access, and they no longer need to waste bandwidth rebuilding service-specific implementations of existing functionality.


## Creating an efficient parser to optimize memory allocation


Across Datadog’s Go services, one out of every three memory allocations occurs in our metrics intake service, which processes all metrics submitted via the[Agent](https://www.datadoghq.com/blog/datadog-agent/) ,[DogStatsD](https://docs.datadoghq.com/extend/dogstatsd/?tab=hostagent) , and our API endpoints.


A significant portion of these allocations come from tag handling. Metrics payloads encode tags as arrays of strings (e.g.,` env:prod` ,` service:web` ). During protobuf deserialization, each of these strings requires its own memory allocation in Go. Given that each metric can carry multiple tags and ingestion operates at very high throughput, this results in a large number of small, repeated allocations, making tag parsing a dominant source of memory pressure.


We had previously explored alternative solutions for reducing this pressure, such as new payload formats and[vtprotobuf](https://github.com/planetscale/vtprotobuf) . However, these options came with issues of their own, and we relied too much on the interoperability and reliability of protocol buffers. Our developers decided to revisit this issue by asking Claude to come up with new ways to optimize this part of our codebase. Based on the query, Claude scanned other relevant areas within our codebase and surfaced an[arena allocation](https://protobuf.dev/reference/cpp/arenas/) approach that we had used for a custom deserialization use case. Although this existing implementation did not use protocol buffers, we knew that the approach could be abstracted and applied to our problem statement.


With the help of Claude Code, developers were able to create, review, and deploy the new parser in under two weeks—in contrast, the parser that Claude surfaced and referenced took months to deploy. The new implementation replaces the per-string allocation model with a single contiguous memory arena. On the first pass, the parser scans the payload to calculate the total number of bytes required across all tags and strings. On the second pass, the parser carves up the pre-allocated arena sequentially, eliminating the resource overhead of repeatedly allocating small chunks of memory for each string during deserialization.


We benchmarked both the pre-existing parser and the new arena allocation implementation with 1,000 points, as shown in the table below. Since deploying the new parser to our staging environment, we’ve observed a 10% increase in packets per second per core. Assuming the same increase in network capacity once deployed to production, this would translate to 2 million dollars in annual savings.


Metric Protobuf parser Arena parser Improvement


Time/operation 1,032 µs 599 µs 1.7x faster


Throughput 277 MB/s 478 MB/s 1.7x


Allocs/operation 30,680 9 3,409x fewer


Bytes/operation 1,295,002 881,985 32% less


## Learn more about AI at Datadog


The examples we showcased in this blog post are only a small fraction of the internal projects made possible with AI-assisted development. These new workflows have enabled developers to surface and deploy performance wins in shorter delivery cycles and work on projects that enhance or reduce friction in software delivery workflows across individual teams and our entire engineering organization.


You can learn more about newly released Bits AI features in[our Monitor blog posts](https://www.datadoghq.com/blog/search/?tag=bits%20ai) or get a deeper look into the AI research that we’re conducting at Datadog in[our AI blog](https://www.datadoghq.com/blog/ai/) . If you’re new to Datadog,sign up for a free 14-day trial to get started.


-
-
-
