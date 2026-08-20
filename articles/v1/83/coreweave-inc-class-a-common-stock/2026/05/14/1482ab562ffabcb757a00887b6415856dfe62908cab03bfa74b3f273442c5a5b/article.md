---
schema_version: "1.0.0"
document_id: "1482ab562ffabcb757a00887b6415856dfe62908cab03bfa74b3f273442c5a5b"
company_key: "coreweave-inc-class-a-common-stock"
company: "CoreWeave Inc."
source_id: "coreweave-inc-class-a-common-stock-news-import-30332681496c"
canonical_url: "https://www.coreweave.com/news/coreweave-sandboxes-launches-to-accelerate-reinforcement-learning-agent-tool-use-and-model-evaluation"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-25T00:41:32.785334+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:b3a44ca35a127b71838f7588ec8fae68a616438a9b2793ee925a56e4df090aab"
---

# CoreWeave Launches Sandboxes for AI Reinforcement Learning, Agent Tool Use & Model Evaluation

**LIVINGSTON, N.J. — May 14, 2026 —**[CoreWeave](http://coreweave.com/) , Inc. (Nasdaq: CRWV), The Essential Cloud for AI™, today announced[CoreWeave Sandboxes](https://www.coreweave.com/blog/run-agentic-workloads-safely-at-scale-with-coreweave-sandboxes) , an execution layer that gives AI researchers and platform teams secure, isolated environments for running reinforcement learning (RL), agent tool use, and model evaluation. The new offering is available on a customer's own CoreWeave infrastructure or as a serverless runtime through Weights & Biases (W&B).


As AI systems evolve from generating outputs to taking actions, training them requires more than compute alone. Advanced AI workflows such as RL and evaluation require isolated execution environments that run code safely, maintain information across steps, and scale across concurrent workloads.


What’s more, most organizations lack a unified execution layer for RL, agent tool use, and model evaluation. Instead, they rely on custom-built systems, loosely integrated tools, or third-party sandbox products that sit outside their core infrastructure. As scale, concurrency, and workflow complexity increase, those disconnected approaches become harder to manage, less reliable, and more difficult to govern.


CoreWeave Sandboxes provides that unified execution layer through two access models: on-cluster for platform teams running training on[CoreWeave Kubernetes Service](https://www.coreweave.com/products/coreweave-kubernetes-service) (CKS) and serverless through W&B for researchers and applied AI teams who want enterprise-grade isolation without the infrastructure overhead.


**Designed for scale, simplicity, and control**


Available now through the[Cloud Console](https://docs.coreweave.com/security/authn-authz/activate-org) and the Python SDK, CoreWeave Sandboxes runs directly within a customer’s CKS cluster, allowing teams to run RL, agent tool use, and model evaluation workloads alongside their AI jobs without adding a separate execution stack. At launch, it includes a Python SDK **** for creating and managing isolated, secure environments that can handle complex back-and-forth tasks and run multiple jobs at the same time. Built-in session management, storage integration, and monitoring tools help teams run these workflows with less operational overhead.


For teams without an existing CoreWeave cluster, or those looking to extend their current compute, CoreWeave Sandboxes is also available as a serverless runtime through Weights & Biases. Researchers authenticate with an existing W&B API key, install the Python client, and can start running sandboxes in minutes with no cluster provisioning or infrastructure decisions required. Every sandbox runs in its own fully isolated virtual environment by default – meaning a failure, memory spike, or runaway process in one sandbox cannot affect any other. When something does go wrong, teams don't have to hunt across disconnected systems to find out why: sandbox activity is captured directly in the same W&B run view as training metrics, so debugging happens in context rather than across tools.


"CoreWeave Sandboxes solves a real gap in our AI research stack: secure, isolated code execution at scale directly in our existing compute," said Brian Belgodere, senior technical staff member, AI/ML Systems, IBM Research. "Our reinforcement learning workflows spin up thousands of sandboxes in parallel per training step, each with its own container image and resource boundaries. Researchers run sandboxes within minutes of a pip install cwsandbox, with no infrastructure knowledge required."


"As agent tool use and evaluation move to production scale, teams need an execution layer that behaves like the rest of their infrastructure — governed, observable, and close to the workflows already running on CoreWeave," said Chen Goldberg, EVP, Product and Engineering at CoreWeave. "CoreWeave Sandboxes closes the execution gap in reinforcement learning and agent workflows without requiring teams to build custom execution systems around them. And for teams that want these capabilities without managing their own clusters, the serverless path through Weights & Biases makes that same execution layer accessible in minutes."


**Addressing the growing complexity of AI workflows**


“Managing separate clusters and scheduling sandboxes across different node types lacked a unified solution, costing us time and resources. CoreWeave Sandboxes eliminated that issue,” said Roman Soletskyi, AI scientist, Mistral. “We now run hundreds of concurrent sandboxes on CPU nodes and alongside Slurm training jobs on GPU nodes, all through a single setup. The Python SDK let our researchers get started immediately, and the CoreWeave team worked closely with us to adapt the open-source SDK to fit seamlessly into our codebase."


“Enterprises are under pressure to build agentic AI automation as fast as possible, so they're looking for any help to accelerate the time from idea to live agent,” said Holger Mueller, VP and principal analyst, Constellation Research. “As they enter the next stages of agentic AI automation, they need to support reward verification and evaluation without adding custom infrastructure to the environments they already run. Purpose-built execution that stays inside existing training infrastructure reduces operational sprawl and removes the fragility of homegrown sandbox systems, a gap that general-purpose and CPU-only sandbox vendors are not designed to solve."


**Built on proven AI infrastructure**


CoreWeave consistently delivers industry-leading infrastructure performance, demonstrated by record-breaking[MLPerf benchmark results](https://www.coreweave.com/news/coreweave-delivers-leading-inference-performance-in-mlperf-r-benchmark) , its position as the only AI cloud to earn the top[Platinum](https://www.coreweave.com/news/coreweave-achieves-semianalysis-platinum-clustermax-tm-rating-for-the-second-consecutive-ranking-remaining-the-industrys-sole-platinum-provider) ranking in both SemiAnalysis ClusterMAX™ 1.0 and 2.0, and its[#1 ranking for inference speed and price-performance](https://www.coreweave.com/news/coreweave-achieves-1-ranking-for-inference-speed-and-price-performance-for-moonshot-ais-kimi-k2-6-model-in-independent-benchmark) for Moonshot AI’s Kimi K2.6 in independent inference benchmarking conducted by Artificial Analysis.
