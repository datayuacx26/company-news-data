---
schema_version: "1.0.0"
document_id: "080c6798b69dc862efdc3a5e8b0f4f857022878e4af73c23ff5da2492a0255e4"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/new-capabilities-security-developer-tools-gradient-ai-platform"
published_at: "2025-10-02T07:12:50.004+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:0633c7f7112716838270863339615ad22bfd2093d5bfdd764770a202c15ad877"
---

# Build Smarter Agents with Image Generation, Auto-Indexing, VPC Security, and new AI Tools on DigitalOcean Gradient™ AI Platform

At Deploy London 2025, we shared the next chapter of the[Gradient AI Platform](https://www.digitalocean.com/products/gradient/platform) . We’re making it easier for developers and businesses to build production-ready AI applications, whether you’re experimenting with your first agent or scaling an enterprise workload.


Today, in that spirit, we’re introducing a new wave of features that expand what you can build with the Gradient AI Platform, give you greater security and control, and accelerate your development workflow.


## Expanding capabilities for Gradient AI Platform


AI applications are becoming more multimodal and data-driven, able to work with text, images, audio, and other formats. With Image Model Support and Knowledge Base Auto-Indexing, these new Gradient AI Platform features make it easier than ever to give your agents a wide range of inputs and knowledge sources.


### Image Model Support


You can now generate images programmatically using text prompts through Gradient AI Platform’s Serverless Inference API, powered by OpenAI’s gpt-image-1 model. This is the platform’s first non-text modality (with more coming soon), expanding our capabilities from text-only to include text-to-image generation.


- **Text-to-image generation –** Generate images directly via the Serverless Inference API.
- **API integration –** OpenAI compatible using the image generation endpoint. For code examples and setup instructions, check out the[DigitalOcean Gradient AI starter kit on GitHub](https://github.com/digitalocean-labs/gradient-starter-kit) .
- **Unified billing –** Image generation charges appear on your DigitalOcean account alongside other services.
- **Developer-first workflow –** Direct API access without requiring UI-based tools or self-hosting.
- **Enterprise authentication –** Use Model Access Keys for secure access.


With this capability, you can create images using natural language prompts for projects such as content generation, marketing assets, product imagery, and more.


[Get started with Image Model Support →](https://docs.digitalocean.com/products/gradient-ai-platform/details/models/)


### Knowledge Base Auto-Indexing


Keep your agents up to date without the manual work. With auto-indexing, new and updated documents from connected sources are automatically detected, fetched, and re-indexed into your OpenSearch database to help keep your agent’s knowledge current.


- **Source connection –** Connect integrations such as Google Drive, Amazon S3, or Dropbox.
- **Auto-indexing toggle –** Turn on auto-indexing and set a default schedule.
- **Customizable frequency –** Choose from daily, weekly, or manual indexing.
- **Ingestion logic –** Automatically handle new, updated, or deleted documents.
- **Sync logs –** Review the last sync time, status, and errors for each source.


With auto-indexing, you can keep your knowledge bases up to date automatically, saving time and reducing manual maintenance.


[Try Auto-Indexing →](https://cloud.digitalocean.com/registrations/new?activation_redirect=%2Fgen-ai&redirect_url=%2Fgen-ai)


## Enterprise-ready infrastructure


As adoption of AI scales, so do requirements for privacy, compliance, and security. We’re making it easier and more secure for enterprises to run AI reliably in production.


### VPC Integration


The new Gradient AI Platform virtual private cloud (VPC) feature enables you to establish more secure, private network connections between services and resources in your own VPC. This helps eliminate exposure to the public internet while keeping deployments in DigitalOcean’s managed infrastructure.


- **Agent VPC connectivity –** Connect agents running in the platform to private customer databases.
- **Egress IP visibility –** See outbound IP addresses to configure your firewall.
- **Cross-VPC support –** Enable connectivity to multiple VPCs as needed.


With the new VPC integration, you can more securely run agents and indexing jobs with private network access, supporting security requirements without managing infrastructure yourself.


[Learn more about VPC Integration →](https://cloud.digitalocean.com/registrations/new?activation_redirect=%2Fgen-ai&redirect_url=%2Fgen-ai)


## Accelerating development


Every team builds differently. Some developers want full control in code; others prefer visual tools to prototype quickly. With this release, we’re supporting both.


### Gradient AI Agent Development Kit (private preview)


The Gradient AI Agent Development Kit (ADK) is a code-first toolkit that helps you build, test, and deploy AI agent workflows directly from your development environment. It provides a code-first framework to define agent workflows, integrate models, and connect tools or APIs, with traces and insights viewable in your workspace.


- **Expressive APIs –** Define agent workflows and manage deployments entirely through code.
- **Model integration –** Connect open-source or commercial models to your agents.
- **Custom tools and connectors –** Wrap APIs or functions for use by your agents.
- **Traces and insights –** View agent sessions and workflow traces via your workspace interface or through code.


The Gradient AI Agent Development Kit will be available soon,[sign up for the private preview](https://forms.gle/1XigKYeMAVmp7KpFA) to get early access and start building smarter, production-ready agents.


### Gradient AI Genie (private preview)


Also launching soon, Genie is a new IDE-integrated experience in Visual Studio Code. It lets you interact with your agents in natural language from inside VS Code, helping you design, configure, and connect multi-agent systems.


- **Get started –** Configure agents in VS Code with the Gradient AI ADK.
- **Get help and recommendations –** Receive workflow guidance, on-demand tips, and improved output.
- **Automate –** Orchestrate agents, build guardrails, and generate synthetic datasets.


With Gradient AI Genie, you can start building and orchestrating multi-agent systems without leaving your IDE.[Sign up for the private preview](https://forms.gle/1XigKYeMAVmp7KpFA) to get early access to the new feature.


## And much more to come


With these new features, you can streamline how you build, manage, and secure AI agents—whether you’re generating images, keeping knowledge bases up to date, or connecting your workloads via private networks. And with the upcoming Gradient AI Agent Development Kit and Gradient AI Genie, you’ll soon have more powerful tools to develop, test, and orchestrate multi-agent systems directly from your development environment. These updates are designed to help you move faster, work more efficiently, and tackle increasingly complex AI workflows with confidence.


Want to explore these new features in depth?[Join our webinar on October 8](https://streamyard.com/watch/ruj77ahiTYPY) , where we’ll walk through the updates and share practical tips for building with the Gradient AI Platform.
