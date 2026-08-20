---
schema_version: "1.0.0"
document_id: "ffc026ce36d83850553c3f01b42250af838630f81c316a4ff59fe553e07f99a8"
company_key: "yc-sim"
company: "Sim"
source_id: "yc-sim-rss-f4bc2e85e2b3"
canonical_url: "https://www.sim.ai/blog/enterprise"
published_at: "2026-02-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:11.483682+00:00"
fetched_at: "2026-07-28T22:20:57.996558+00:00"
content_hash: "sha256:5c7e28a3ec581b7a4c89e754fcd2e94cef53288e077b068530da374835528a5b"
---

# Sim for Enterprise

We've been working with security teams at larger organizations to bring Sim into environments with strict compliance and data handling requirements. This post covers the enterprise capabilities we've built: granular access control, bring-your-own-keys, self-hosted deployments, on-prem Copilot, SSO & SAML, whitelabeling, compliance, and programmatic management via the Admin API.


## Access Control


Permission groups let administrators control what features and integrations are available to different teams within an organization. This isn't just UI filtering—restrictions are enforced at the execution layer.


### Model Provider Restrictions


Allowlist specific providers while blocking others. Users in a restricted group see only approved providers in the model selector. A workflow that tries to use an unapproved provider won't execute.


This is useful when you've approved certain providers for production use, negotiated enterprise agreements with specific vendors, or need to comply with data residency requirements that only certain providers meet.


### Integration Controls


Restrict which workflow blocks appear in the editor. Disable the HTTP block to prevent arbitrary external API calls. Block access to integrations that haven't completed your security review.


### Platform Feature Toggles


Control access to platform capabilities per permission group:


- **[Knowledge Base](https://docs.sim.ai/integrations/knowledge)** — Disable document uploads if RAG workflows aren't approved
- **[MCP Tools](https://docs.sim.ai/agents/mcp)** — Block deployment of workflows as external tool endpoints
- **Custom Tools** — Prevent creation of arbitrary HTTP integrations
- **Invitations** — Disable self-service team invitations to maintain centralized control


Users not assigned to any permission group have full access, so restrictions are opt-in per team rather than requiring you to grant permissions to everyone.


---


## Bring Your Own Keys


When you configure your own API keys for model providers—OpenAI, Anthropic, Google, Azure OpenAI, AWS Bedrock, or any supported provider—your prompts and completions route directly between Sim and that provider. The traffic doesn't pass through our infrastructure.


This matters because LLM requests contain the context you've assembled: customer data, internal documents, proprietary business logic. With your own keys, you maintain a direct relationship with your model provider. Their data handling policies and compliance certifications apply to your usage without an intermediary.


BYOK is available to everyone, not just enterprise plans. Connect your credentials in workspace settings, and all model calls use your keys. For self-hosted deployments, this is the default—there are no Sim-managed keys involved.


A healthcare organization can use Azure OpenAI with their BAA-covered subscription. A financial services firm can route through their approved API gateway with additional logging controls. The workflow builder stays the same; only the underlying data flow changes.


---


## Self-Hosted Deployments


Run Sim entirely on your infrastructure. Deploy with[Docker Compose](https://docs.sim.ai/platform/self-hosting/docker) or[Helm charts](https://docs.sim.ai/platform/self-hosting/kubernetes) for Kubernetes—the application, WebSocket server, and PostgreSQL database all stay within your network.


**Single-node** — Docker Compose setup for smaller teams getting started.


**High availability** — Multi-replica Kubernetes deployments with horizontal pod autoscaling.


**Air-gapped** — No external network access required. Pair with[Ollama](https://docs.sim.ai/platform/self-hosting/ollama) or[vLLM](https://docs.sim.ai/platform/self-hosting/vllm) for local model inference.


Enterprise features like access control, SSO, and organization management are enabled through environment variables—no connection to our billing infrastructure required.


---


## On-Prem Copilot


Copilot—our context-aware AI assistant for building and debugging workflows—can run entirely within your self-hosted deployment using your own LLM keys.


When you configure Copilot with your API credentials, all assistant interactions route directly to your chosen provider. The prompts Copilot generates—which include context from your workflows, execution logs, and workspace configuration—never leave your network. You get the same capabilities as the hosted version: natural language workflow generation, error diagnosis, documentation lookup, and iterative editing through diffs.


This is particularly relevant for organizations where the context Copilot needs to be helpful is also the context that can't leave the building. Your workflow definitions, block configurations, and execution traces stay within your infrastructure even when you're asking Copilot for help debugging a failure or generating a new integration.


---


## SSO & SAML


Integrate with your existing identity provider through SAML 2.0 or OIDC. We support Okta, Azure AD (Entra ID), Google Workspace, OneLogin, Auth0, JumpCloud, Ping Identity, ADFS, and any compliant identity provider.


Once enabled, users authenticate through your IdP instead of Sim credentials. Your MFA policies apply automatically. Session management ties to your IdP—logout there terminates Sim sessions. Account deprovisioning immediately revokes access.


New users are provisioned on first SSO login based on IdP attributes. No invitation emails, no password setup, no manual account creation required.


This centralizes your authentication and audit trail. Your security team's policies apply to Sim access through the same system that tracks everything else.


---


## Whitelabeling


Customize Sim's appearance to match your brand. For self-hosted deployments, whitelabeling is configured through environment variables—no code changes required.


**Brand name & logo** — Replace "Sim" with your company name and logo throughout the interface.


**Theme colors** — Set primary, accent, and background colors to align with your brand palette.


**Support & documentation links** — Point help links to your internal documentation and support channels instead of ours.


**Legal pages** — Redirect terms of service and privacy policy links to your own policies.


This is useful for internal platforms, customer-facing deployments, or any scenario where you want Sim to feel like a native part of your product rather than a third-party tool.


---


## Compliance & Data Retention


Sim maintains **SOC 2 Type II** certification with annual audits covering security, availability, and confidentiality controls. We share our SOC 2 report directly with prospective customers under NDA.


**Data Retention** — Configure how long workflow execution traces, inputs, and outputs are stored before automatic deletion. We work with enterprise customers to set retention policies that match their compliance requirements.


We provide penetration test reports, architecture documentation, and completed security questionnaires (SIG, CAIQ, and custom formats) for your vendor review process.


---


## Admin API


Manage Sim programmatically through the Admin API. Every operation available in the UI has a corresponding API endpoint, enabling infrastructure-as-code workflows and integration with your existing tooling.


**User & Organization Management** — Provision users, create organizations, assign roles, and manage team membership. Integrate with your HR systems to automatically onboard and offboard employees.


**Workspace Administration** — Create workspaces, configure settings, and manage access. Useful for setting up isolated environments for different teams or clients.


**Workflow Lifecycle** — Deploy, undeploy, and manage workflow versions programmatically. Build CI/CD pipelines that promote workflows from development to staging to production.


The API uses standard REST conventions with JSON payloads. Authentication is via API keys scoped to your organization.


---


## Import & Export


Move workflows between environments, create backups, and maintain version control inside or outside of Sim.


**Workflow Export** — Export individual workflows or entire folders as JSON. The export includes block configurations, connections, environment variable references, and metadata. Use this to back up critical workflows or move them between Sim instances.


**Workspace Export** — Export an entire workspace as a ZIP archive containing all workflows, folder structure, and configuration. Useful for disaster recovery or migrating to a self-hosted deployment.


**Import** — Import workflows into any workspace. Sim handles ID remapping and validates the structure before import. This enables workflow templates, sharing between teams, and restoring from backups.


**Version History** — Each deployment creates a version snapshot. Roll back to previous versions if a deployment causes issues. The Admin API exposes version history for integration with your change management processes.


For teams practicing GitOps, export workflows to your repository and use the Admin API to deploy from CI/CD pipelines.


---


## Get Started


Enterprise features are available now. Check out our[self-hosting](https://docs.sim.ai/platform/self-hosting) and[enterprise](https://docs.sim.ai/platform/enterprise) docs to get started.


*Questions about enterprise deployments?*


[Contact Us](https://form.typeform.com/to/jqCO12pF)


## FAQ


### Can Sim be deployed fully on-premises or in an air-gapped environment?


Yes. Sim can run entirely on your infrastructure via Docker Compose or Helm charts for Kubernetes, with the application, WebSocket server, and PostgreSQL database staying inside your network. Air-gapped setups are supported by pairing self-hosting with Ollama or vLLM for local model inference, so no external network access is required.


### Does using our own LLM API keys (BYOK) keep our data from passing through Sim's servers?


Yes. When you configure your own API keys for providers like OpenAI, Anthropic, Google, Azure OpenAI, or AWS Bedrock, prompts and completions route directly between Sim and that provider without passing through Sim's infrastructure. BYOK is available to everyone, not just enterprise plans, and is the default with no Sim-managed keys involved in self-hosted deployments.


### Can Copilot be used without sending workflow data to an external AI service?


Yes. Copilot can run entirely within a self-hosted deployment using your own LLM keys, so prompts containing context from your workflows, execution logs, and workspace configuration route directly to your chosen provider and never leave your network.


### What identity providers does Sim support for SSO, and what happens when an employee is deprovisioned?


Sim integrates with Okta, Azure AD (Entra ID), Google Workspace, OneLogin, Auth0, JumpCloud, Ping Identity, ADFS, and any SAML 2.0 or OIDC compliant identity provider. Session management ties to your IdP, so logging out there terminates Sim sessions, and account deprovisioning immediately revokes access.


### Is Sim SOC 2 certified, and can we get a copy of the report for vendor review?


Sim maintains SOC 2 Type II certification with annual audits covering security, availability, and confidentiality controls, and shares the report directly with prospective customers under NDA. Sim also provides penetration test reports, architecture documentation, and completed security questionnaires (SIG, CAIQ, and custom formats) for vendor review.


### Can we restrict which model providers or integrations are available to certain teams?


Yes, through permission groups that are enforced at the execution layer, not just hidden in the UI. Administrators can allowlist approved model providers so unapproved ones fail to execute, disable specific workflow blocks like HTTP calls, and block integrations that haven't cleared security review — while users outside any permission group keep full access by default.
