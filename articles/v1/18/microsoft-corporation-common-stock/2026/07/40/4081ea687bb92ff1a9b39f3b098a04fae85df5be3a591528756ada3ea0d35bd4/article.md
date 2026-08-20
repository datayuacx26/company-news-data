---
schema_version: "1.0.0"
document_id: "4081ea687bb92ff1a9b39f3b098a04fae85df5be3a591528756ada3ea0d35bd4"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-news-import-bfb1f574a3a6"
canonical_url: "https://azure.microsoft.com/en-us/blog/att-and-microsoft-scale-trillion-token-workloads-with-microsoft-foundry-and-amd/"
published_at: "2026-07-23T18:30:00+00:00"
first_seen_at: "2026-07-25T02:59:23.665583+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:2f775d8b782cc8e69f86f29765f653c2aa931d8a634f65e838eba182cb3ea017"
---

# AT&T and Microsoft scale trillion-token workloads with Microsoft Foundry and AMD

## First-of-its-kind telecom AI deployment


Telecommunications organizations are increasingly looking to AI to help teams navigate highly specialized domains, but generic models often lack the industry-specific knowledge needed to understand telecom networks, standards, and operations. To address that gap, AT&T created their Open Telco (OTel) models, the next generation of telecom-focused AI designed to bring deeper telecommunications expertise into AI systems. Building OTel2.0 required more than training a large language model, it reflected a broader issue many organizations face: how to build domain-specific AI systems at scale while balancing cost, performance, and operational complexity. Cost management quickly became a key consideration. To continue advancing telecom-focused AI, AT&T needed a platform capable of supporting OTel2.0 development at an entirely new scale.


Where teams previously had to own and manage deployments, infrastructure, and the associated operational overhead,[Foundry Managed Compute](https://learn.microsoft.com/en-us/azure/foundry/concepts/managed-compute-overview) provided a more streamlined way to access dedicated graphics processing unit (GPU) capacity. This transformation requires more than powerful models; it requires the ability to scale without compromising cost, flexibility, or performance.


[Learn how OTel2.0 scales telecom AI](https://www.gsma.com/newsroom/press-release/gsma-launches-open-telco-ai-to-accelerate-development-of-telco%E2%80%91grade-ai/)


Using[Microsoft Foundry Managed Compute](https://devblogs.microsoft.com/foundry/announcing-foundry-managed-compute/) , AT&T was able to experiment across multiple open models, optimize workloads across different GPU architectures, and process massive volumes of telecom data all within a unified platform. The result was an AI development environment capable of supporting trillions of tokens while giving teams the flexibility to iterate, optimize, and innovate faster.


## Model choice meets infrastructure flexibility


Building OTel2.0 required flexibility across both models and infrastructure. Rather than standardizing on a single model, AT&T adopted a multi open-model strategy. Open models were central to AT&T’s approach because they provided the flexibility to work with approved telecom data, tailor the workflow for domain-specific model development, and support large-scale experimentation with greater control over cost and deployment strategy. Through[Microsoft Foundry](https://azure.microsoft.com/en-us/products/ai-foundry) , the team deployed several models from the Hugging Face collection, including[Phi-4](https://azure.microsoft.com/en-us/products/phi) , OSS-120B, and Gemma-4, to support different stages of development, from synthetic data generation and data preparation to reasoning-intensive workloads and broader model development efforts. Phi-4 played a significant role in this process, processing more than 700 billion tokens a month as part of the broader data preparation and training workflow for OTel2.0.


> Every company in the world needs to build its own AI, and that is only possible with open models and open source. AT&T is championing this vision, building on open models like Phi-4 and Gemma, and giving OTel back to the community as a telecom AI foundation others can build upon. Microsoft Foundry makes this practical at scale, bringing the latest open models from the Hugging Face collection together with AMD and NVIDIA GPUs in one place, so teams can pick the right model and the right hardware, then deploy in hours instead of weeks.
>
>
> —Jeff Boudier, Vice President of Product, Hugging Face


[Developing OTel2.0](https://www.gsma.com/newsroom/press-release/gsma-launches-open-telco-ai-to-accelerate-development-of-telco%E2%80%91grade-ai/) also required infrastructure capable of operating at telecom scale. AT&T used approximately **530 GPUs** through Microsoft Foundry Managed Compute spanning multiple GPU architectures including **430 AMD Instinct™ MI300X GPUs** . This heterogenous approach gave AT&T more flexibility in how models were deployed and optimized as requirements evolved.


Model Example workload


Phi-4 Around 700B tokens a month for
data preparation and synthetic
data generation


OSS 120B Higher-reasoning workloads


Gemma 4 OTel2.0 development workflows


Table 1: Explains what open source models were used and how


This flexibility illustrates a broader trend across AI development. Organizations increasingly need platforms that allow them to choose the right model for the job, optimize for cost and performance, and scale workloads without rebuilding operational environments.[Microsoft Foundry](https://azure.microsoft.com/en-us/products/ai-foundry) brings model choice, infrastructure flexibility, governance, and operational scale together in a unified platform that supports those requirements.


[Start building with Microsoft Foundry](https://azure.microsoft.com/en-us/products/ai-foundry)


Beyond flexibility and cost, deployment speed is a critical factor for many AI initiatives. As workloads expand and new models are evaluated, the ability to access GPU capacity quickly enables teams to move from experimentation to execution faster without lengthy provisioning cycles. With Foundry Managed Compute, AT&T could deploy and scale models in days rather than waiting weeks for infrastructure to become available, helping accelerate development timelines and maintain momentum across OTel2.0 development.


## Optimizing cost without limiting innovation


As AI workloads grow, economics become as important as model performance. For AT&T, one of the primary objectives was to lower AI model consumption costs while continuing to drive meaningful business value through AI-powered innovation. By using open models on[Microsoft Foundry Managed Compute](https://learn.microsoft.com/en-us/azure/foundry/concepts/managed-compute-overview) , AT&T was able to support large-scale data preparation and model development using a different economic model built around dedicated GPU infrastructure and open-model flexibility.


The impact became clear at scale. In support of OTel2.0, AT&T processed approximately 1T tokens, consisting of raw documents from GSMA supplemented by synthetic data generated. Generating the data using open-source models like Phi-4, served by Microsoft’s Foundry Managed Compute, saved tens of millions of dollars versus using frontier models. This allowed teams to invest in larger-scale experimentation and development while maintaining a focus on business value and operational efficiency.


Metric Value


OTel 1.0 Downloads Over 25M


GPUs Used Through Foundry Managed Compute About 530


Tokens Processed for OTel2.0 About 1T


Tokens Trained for OTel2.0 About 400 B


Models used to train OTel Phi-4, OSS 120B, Gemma 4


Table 2: Quick facts about the OTel model family and metrics around what was used to build OTel2.0


> When you are processing hundreds of billions of tokens, infrastructure becomes part of the problem you solve. Foundry Managed Compute gave us access to GPU capacity at scale so our teams could focus on advancing OTel2.0 instead of managing infrastructure.
>
>
> —Mark Austin, Vice President, Data Science and AI at AT&T


At this scale, infrastructure is no longer simply a deployment consideration. It becomes a strategic component of AI development.


## Accelerating the next wave of production-scale AI


OTel 2.0 demonstrates how organizations can combine open models, scalable infrastructure, and domain expertise to build production-ready AI systems. By matching different models to different workloads and optimizing infrastructure for cost and performance, AT&T was able to process trillions of tokens while maintaining operational efficiency.


As organizations move from AI experimentation to production deployment, they increasingly need the flexibility to choose the right models, optimize infrastructure, and scale efficiently.[Microsoft Foundry](https://azure.microsoft.com/en-us/products/ai-foundry) and[Foundry Managed Compute](https://learn.microsoft.com/en-us/azure/foundry/concepts/managed-compute-overview) help support that transition by bringing those capabilities together in a unified platform.


### Learn more


- [Read Scott Guthrie’s blog](https://blogs.microsoft.com/blog/2026/07/20/microsoft-expands-azure-ai-and-hpc-infrastructure-with-amd/) about Azure AI and HPC infrastructure.
- Learn more about[OTel2.0](https://www.gsma.com/newsroom/press-release/gsma-launches-open-telco-ai-to-accelerate-development-of-telco%E2%80%91grade-ai/) .


**Explore session topics from[AMD’s Advancing AI](https://www.amd.com/en/corporate/events/advancing-ai.html)** :


- [From GPUs to CPUs: Optimizing Every AI Workload with Azure and AMD](https://www.amd.com/en/corporate/events/advancing-ai/sessions-catalog/sovereign-ai-at-scale--enterprise-ready-ai-with-microsoft-and-amd.html)
- [What’s Next for AI Infrastructure in the Cloud?](https://www.amd.com/en/corporate/events/advancing-ai/sessions-catalog/whats-next-for-ai-infrastructure-in-the-cloud.html)


## Powering the Future of AI on Azure


Discover how Microsoft and AMD are expanding Azure AI and HPC infrastructure.


[Read more](https://blogs.microsoft.com/blog/2026/07/20/microsoft-expands-azure-ai-and-hpc-infrastructure-with-amd/)
