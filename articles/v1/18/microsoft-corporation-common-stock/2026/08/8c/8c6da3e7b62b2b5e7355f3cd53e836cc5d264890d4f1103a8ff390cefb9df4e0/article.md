---
schema_version: "1.0.0"
document_id: "8c6da3e7b62b2b5e7355f3cd53e836cc5d264890d4f1103a8ff390cefb9df4e0"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0524baf4a828"
canonical_url: "https://www.microsoft.com/en-us/startups/blog/how-to-deploy-fireworks-ai-on-microsoft-foundry-a-startup-architecture-blueprint/"
published_at: "2026-08-06T20:11:40+00:00"
first_seen_at: "2026-08-06T22:13:37.730572+00:00"
fetched_at: "2026-08-06T22:13:38.505053+00:00"
content_hash: "sha256:71e0b026a8dd66cdb3fa948291430d953a370efbaaf32c3317be57a95b0925f5"
---

# Microsoft expands AI model choice for startups

Many startups begin building AI products with a single closed model as a fast way to prototype. But model selection for application workloads is a decision that compounds across a product’s lifecycle and shapes future costs and product differentiation.


Open models let you make that call more deliberately: you pick the model, tune it to your use case, and shape the behavior that sets your product apart. With[Fireworks AI on Microsoft Foundry](https://azure.microsoft.com/blog/introducing-fireworks-ai-on-microsoft-foundry-bringing-high-performance-low-latency-open-model-inference-to-azure/?wt.mc_id=fireworksarchitecture_foundry_blog_mfsmktg) now generally available, you can serve high-performance, low-latency open model inference *directly in Azure* . You don’t need to build your own inference infrastructure to run open models here; Fireworks serves them on[Foundry](https://azure.microsoft.com/products/ai-foundry) , so you can start quickly and scale that footprint as you go.


To help, we’re introducing new resources for AI-native startups on how to[deploy and serve Fireworks models on Foundry](https://learn.microsoft.com/startups/build/ai/deploy-fireworks?wt.mc_id=fireworksarchitecture_deploy_blog_mfsmktg) and scale from prototype to production.


## Implementation blueprint designed for AI-native startups


The[implementation blueprint for deploying Fireworks AI models](https://learn.microsoft.com/startups/build/ai/deploy-fireworks?wt.mc_id=fireworksarchitecture_deploy_blog_mfsmktg) shows how founding engineers and small teams can move from idea to MVP to product-market fit (PMF) using a repeatable, Azure-native approach. The experience starts simple and grows with your needs, so you own your intelligence from the beginning.


The stack runs entirely inside your Azure environment and only requires a model endpoint for your application infrastructure or harness. Start by deploying a single model, routing traffic through[API Management](https://azure.microsoft.com/products/api-management?wt.mc_id=fireworksarchitecture_api_blog_mfsmktg) , and track latency, usage, and cost metrics along the way. When ready, you can scale by using[Azure Cache for Redis](https://azure.microsoft.com/products/cache/?wt.mc_id=fireworksarchitecture_redis_blog_mfsmktg) to reduce redundant inference, introducing performance tuning based on workload and deploying multiple model variants for A/B testing.


Fireworks models are deployed through Foundry within your Azure subscription, so model discovery, governance, and billing all remain within a single control plane.


**Component**


**Purpose**


[Microsoft Foundry + Fireworks AI models](https://learn.microsoft.com/azure/foundry/how-to/fireworks/enable-fireworks-models#available-catalog-models?wt.mc_id=fireworksarchitecture_modelcatalog_blog_mfsmktg)


Foundry provides the Azure-native platform for deployment, governance, and billing; Fireworks is the inference layer serving the open models behind them


[Azure Container Apps](https://azure.microsoft.com/products/container-apps?wt.mc_id=fireworksarchitecture_containerapps_blog_mfsmktg)


Hosts the application or API that sends inference requests


[Azure Container Registry](https://azure.microsoft.com/products/container-registry?wt.mc_id=fireworksarchitecture_containerregistry_blog_mfsmktg)


Stores container images


[Azure Key Vault](https://azure.microsoft.com/products/key-vault?wt.mc_id=fireworksarchitecture_keyvault_blog_mfsmktg)


Stores credentials and endpoint information securely


[Azure Monitor](https://azure.microsoft.com/products/monitor?wt.mc_id=fireworksarchitecture_monitor_blog_mfsmktg) (optional)


Provides observability and performance insight


## Why startups need flexible AI inference architecture


Inference is one of the largest controllable cost drivers for AI-native companies. Early decisions about how models are served can create long-term constraints in cost, latency, and flexibility. This architecture is designed to address these challenges upfront. Serving open models this way keeps those decisions in your hands, so you can choose, optimize, and switch the models behind your product as your cost and performance needs change.


### Optimize cost from day one


- Use serverless, pay-per-token inference through Foundry with a[selection of open models](https://learn.microsoft.com/en-us/azure/foundry/how-to/fireworks/enable-fireworks-models#available-catalog-models)
- Match workloads to the most cost-effective model, and avoid being tied to one model provider
- Cache repeated requests with[Azure Cache for Redis](https://azure.microsoft.com/products/cache/?wt.mc_id=fireworksarchitecture_redis_blog_mfsmktg) to reduce compute usage
- Track cost per million tokens as a core engineering metric


### Eliminate infrastructure overhead


- No need to stand up or manage GPU clusters
- Fireworks provides high-throughput inference, while Foundry provides governance, security, and lifecycle management


### Maintain flexibility as you scale


- Experiment with and switch models through consistent APIs and deployment workflows, so swapping takes less rework
- Support custom or bring-your-own model weights where needed
- Move from experimentation to production on the same platform
- Once a workload is well understood, your eval suites, prompt libraries, and graded production traffic are training data: teams can fine-tune and optimize a model via Fireworks Training and then import to Azure via[bring your own weights](https://learn.microsoft.com/azure/foundry/how-to/fireworks/import-custom-models?tabs=rest-api) .


## Build and test AI applications with less upfront cost pressure


For teams in the Microsoft for Startups program, this architecture unlocks a significant advantage. You can apply your Startup credits to Fireworks model deployments using Data Zone Standard (provisioned throughput units, or PTUs, are reserved capacity and not covered by Startup credits), as well as the supporting Azure infrastructure.


This means you can build and test production-grade AI applications, experiment with multiple models to find where open models give you the right cost and performance advantage before scale, and iterate quickly toward product-market fit, without introducing immediate infrastructure cost pressure.


## Build with Microsoft for Startups


If you’re building AI applications on Azure, we’d love to learn more about your vision and help accelerate your journey.


Microsoft for Startups helps founders build fast, scale smart, and sell more with Startup credits,[Azure AI](https://azure.microsoft.com/solutions/ai?wt.mc_id=fireworksarchitecture_azureai_blog_mfsmktg) infrastructure, technical guidance, and go-to-market resources designed to help startups move from prototype to enterprise deployment faster.[Get started with Microsoft for Startups today](https://www.microsoft.com/startups?wt.mc_id=fireworksarchitecture_getstarted_blog_mfsmktg) .


## Access your startups benefits today


Microsoft for Startups helps founders build fast, scale smart, and sell more. Apply today to unlock up to $150,000 in Startup credits to start building immediately.


[Apply today](https://www.microsoft.com/startups?wt.mc_id=fireworksarchitecture_getstarted_blog_mfsmktg)
