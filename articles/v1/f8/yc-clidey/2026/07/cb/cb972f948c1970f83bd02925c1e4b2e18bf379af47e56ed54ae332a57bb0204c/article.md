---
schema_version: "1.0.0"
document_id: "cb972f948c1970f83bd02925c1e4b2e18bf379af47e56ed54ae332a57bb0204c"
company_key: "yc-clidey"
company: "Clidey"
source_id: "yc-clidey-rss-03eb479b7595"
canonical_url: "https://blog.clidey.com/what-sovereign-ai-actually-requires-and-why-most-companies-dont-have-it/"
published_at: "2026-07-22T15:56:23+00:00"
first_seen_at: "2026-07-24T22:35:58.719229+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:416159fd8a49ccf1f068b4fa8f87a2132e047aef995a3bc124959d6ac7685708"
---

# What Sovereign AI Actually Requires (And Why Most Companies Don't Have It)

Everyone's talking about sovereign AI. Governments want it. Enterprises say they're building it. Vendors are selling it.


Most of them are describing something that isn't actually sovereign.


I've been thinking about this for a while, not as a political question but as a technical and organisational one. What does it actually take for a company or institution to own its AI stack? Not rent it. Not route it through someone else's infrastructure. Actually own it.


Here's where I land.


## **The First Requirement: Zero Data Retention**


If you're sending prompts to a model provider and trusting that nothing is logged, you're operating on hope, not trust.


Zero data retention agreements exist, and they matter. But they're only meaningful if the infrastructure underneath them supports that promise. If your data has to leave your environment to reach the model, you've already given something up. The query, the schema, and the patterns in how your team works all travel with the request.


Most companies don't think about this until something goes wrong. By then, the value in those patterns may already be accumulating somewhere else.


The only real answer is to run models where your data already lives, instead of sending your data to where the model lives.


## **The Second Requirement: You Have to Be Able to Switch Models**


This one sounds obvious. Almost nobody does it well.


Model quality is moving fast. The best option today won't be the best option six months from now. If your workflows are tightly coupled to one provider, you're not operating independently. You're locked into a single ecosystem.


Tools like OpenRouter make switching models at the API level easier. But that's only part of the problem. The pipelines, prompts, integrations, and access controls built around the model matter just as much. If those depend on one model's behaviour or one provider's infrastructure, switching becomes a rebuild instead of a simple change.


Sovereign AI requires model liquidity. Your system should be able to route requests to OpenAI, Anthropic, AWS Bedrock, Google Vertex, or a locally hosted open-weight model without the rest of your stack needing to change. That's the idea behind the WhoDB Agent. The model is swappable. The infrastructure underneath stays yours.


## **The Third Requirement: Own the Infrastructure Layer**


This is the hardest part.


Owning the hardware and model weights removes your dependence on external providers. There are no contracts to negotiate, policy changes to adapt to, or uptime dependencies on someone else's systems. But it also raises the barrier significantly, especially for smaller organisations. Building and maintaining that architecture is expensive and requires deep technical expertise.


There's a middle ground that most organisations can realistically reach.


You don't necessarily need to own the physical hardware. You do need to own the layer where your data and AI workflows live. That means self-hosting inside your own VPC, with your pipelines, access controls, audit logs, and ontology all running within your environment. The model can be external. Everything that interacts with your data shouldn't be.


This is the layer that creates long-term value. Every query your team runs, every pipeline that executes, and every decision made through your data contributes to a record of how your organisation works. If that record stays inside infrastructure you control, the value stays with you. If it lives somewhere else, the value does too.


## **What Gets in the Way**


I want to be honest about the parts that don't have clear answers yet.


The talent problem is real. The people who can build and maintain sovereign AI infrastructure are concentrated in a small number of places. For most organisations, especially governments in smaller or less wealthy countries, building this from scratch isn't realistic. The architecture requires deep expertise that still isn't widely available.


There's also a political dimension that I find difficult. Owning AI infrastructure at a government level means that the government has full control over what gets logged, what gets intercepted, and what gets used for "safety." The same infrastructure that protects citizens from external surveillance can also enable internal surveillance. There's no technical solution to that tension. It's ultimately a governance question.


I don't think these challenges mean sovereign AI isn't worth pursuing. They do mean it should be approached carefully, with open source foundations and community oversight wherever possible, rather than as a closed system controlled by a single government or vendor.


## **What We're Actually Building**


WhoDB sits at the infrastructure layer. It connects your data sources, builds your pipelines, governs access, and surfaces decisions, all within your own environment.


The source code is open. You can deploy it in your VPC using Docker or Kubernetes. Air-gapped deployments are supported for classified environments. WhoDB Agent supports OpenAI, Anthropic, AWS Bedrock, Google Vertex, Ollama, and any OpenAI-compatible API. The model is your choice. The infrastructure stays yours.


We're not trying to own your AI stack. We're building the infrastructure around it so you can own yours.


## **The Shift That's Already Happening**


The organisations that move ahead won't be the ones with access to the best models. Capable models will be available to everyone. The difference will come from what they build around those models: the data layer, the governance layer, and the record of how their organisation works, all running inside infrastructure they control.


Most companies think they own their data. They own access to it. That distinction is going to matter more than most people realise.


WhoDB is the AI-native data platform built by Clidey. Self-hosted, open source, governance-first.


**Check**[whodb.com](https://whodb.com/?ref=blog.clidey.com)
