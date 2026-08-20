---
schema_version: "1.0.0"
document_id: "873a65f98c6b5c700da80f2bf9a2401161957c2573c8c14ee4be14f15ffb07c2"
company_key: "yc-unthread"
company: "Unthread"
source_id: "yc-unthread-rss-c618c87b9ca4"
canonical_url: "https://unthread.io/blog/migrate-hubspot-service-hub-zendesk/"
published_at: "2026-08-14T13:00:00+00:00"
first_seen_at: "2026-08-15T04:23:32.205337+00:00"
fetched_at: "2026-08-15T04:23:33.757353+00:00"
content_hash: "sha256:97b032b0ad11f307d65da029f5385cfd5d5605d0cc1c429b75fb6c5c5c535cea"
---

# Migrate from HubSpot Service Hub to Zendesk: Step-by-Step HubSpot Service Hub Migration Guide

Moving from HubSpot Service Hub to Zendesk is not a straightforward data transfer. HubSpot's CRM-native architecture stores support operations alongside sales pipelines and marketing automation, meaning ticket data is deeply intertwined with systems that support teams may never touch. For internal IT and HR teams seeking purpose-built helpdesk capabilities, organizations looking to implement[workflow automations](https://unthread.io/products/automations?ref=unthread.io) that learn from resolved tickets, or companies wanting to separate support operations from CRM overhead, this migration requires careful planning that generic guides overlook.


This step-by-step guide covers the architectural decisions, data extraction challenges, and workflow rebuilding processes that determine whether a migration succeeds or stalls.


## **Key Takeaways**


- **The CRM decoupling decision shapes the entire migration** - choosing whether to keep HubSpot CRM alongside Zendesk or leave HubSpot entirely determines scope, timeline, and cost before any data work begins
- **HubSpot's architecture makes ticket extraction complex** - the platform stores ticket conversations as separate engagement objects requiring[3-5 API calls](https://clonepartner.com/blog/how-to-migrate-from-hubspot-service-hub-to-zendesk-complete-guide?ref=unthread.io) per ticket to reconstruct full history, making this the primary bottleneck
- **Property pruning prevents post-migration chaos** - HubSpot instances typically contain[50-200 custom properties](https://clonepartner.com/migrate/hubspot-service-hub-to-zendesk?ref=unthread.io) , and migrating all of them creates cluttered interfaces agents ignore
- **Professional migration services can start around $5,000** , with final costs depending on ticket volume, workflow complexity, integrations, and the scope of CRM replacement planning
- **Timeline varies by scenario** - expect[4-7 weeks](https://clonepartner.com/blog/how-to-migrate-from-hubspot-service-hub-to-zendesk-complete-guide?ref=unthread.io) for decoupled migrations where CRM stays in HubSpot, and 6-10 weeks for full departures requiring CRM replacement
- **Cross-object workflows have no Zendesk equivalent** - workflows touching contacts, companies, and deals alongside tickets require middleware solutions or must be accepted as lost functionality


## **Understanding the Need for Help Desk Migration**


### **Why Move from HubSpot to Zendesk?**


The decision to migrate from HubSpot Service Hub to Zendesk typically stems from operational friction rather than feature checklists. Internal IT teams handling infrastructure requests and HR teams managing employee queries often find themselves navigating CRM complexity when all they need is focused ticket management.


**Common drivers for this migration include:**


### **Support-specific depth**


Zendesk offers granular SLA policies, multi-brand support, and mature automation that HubSpot Service Hub Professional cannot match.


### **Agent experience**


Support teams do not need lifecycle stages, deal pipelines, or marketing automation cluttering their workspace.


### **Cost and platform fit**


HubSpot Service Hub Professional currently starts at about $90 per seat per month when billed annually, so teams should compare the value of its broader CRM-connected feature set with the internal help desk capabilities they actually need.


### **Separation of concerns**


Keeping sales CRM operations distinct from internal service management reduces complexity for both teams.


This is notably a reverse migration. Most content covers Zendesk-to-HubSpot consolidation, not the opposite direction. Teams moving to Zendesk are typically seeking purpose-built helpdesk functionality rather than all-in-one CRM integration.


The architectural difference matters for internal service teams. HubSpot treats tickets as CRM objects alongside contacts, companies, and deals. Zendesk treats tickets as the primary object with purpose-built tools for routing, escalation, and resolution. For[IT service desk](https://unthread.io/solutions/it-service-desk?ref=unthread.io) operations or employee support workflows, this distinction affects daily operations significantly.


## **Strategic Planning for HubSpot to Zendesk Migration**


Before touching any data, a fundamental decision must be made that shapes everything else: will the organization keep HubSpot CRM alongside Zendesk, or leave HubSpot entirely?


### **Scenario A (Decoupled Model)**


- Tickets move to Zendesk
- Contacts, companies, and deals stay in HubSpot CRM
- Sales and marketing teams continue using HubSpot unaffected
- Install Zendesk-HubSpot sidebar app for CRM context
- [Timeline: 4-7 weeks](https://clonepartner.com/blog/how-to-migrate-from-hubspot-service-hub-to-zendesk-complete-guide?ref=unthread.io)


### **Scenario B (Full Departure)**


- Everything moves to Zendesk or replacement CRM
- HubSpot CRM either archived or replaced
- Clean break with no integration dependency
- Requires CRM replacement planning if sales team needs it
- [Timeline: 6-10 weeks](https://clonepartner.com/blog/how-to-migrate-from-hubspot-service-hub-to-zendesk-complete-guide?ref=unthread.io)


This decision stalls more migration projects than any technical challenge. Get stakeholder sign-off on scope during Week 1, not Week 4.


**Planning phase documentation should include:**


- Ticket count by pipeline and stage
- Every custom property on tickets, contacts, and companies
- All workflows touching tickets (event-driven and time-based)
- Third-party integrations (Slack, Jira, phone systems)
- Macros, snippets, and SLA policies currently in use


Expect to find significantly more customization than teams initially estimate. Half of it is undocumented.


## **Mapping Data for CRM Migration from HubSpot to Zendesk**


HubSpot and Zendesk structure data fundamentally differently. Depending on how conversations and activities were created in HubSpot, migration may require combining ticket records, associated conversation threads or CRM activities, contacts, and attachments before rebuilding the history as Zendesk ticket comments.


**Property classification is mandatory:**


### **Must-have**


Fields essential for operations (ticket type, priority, SLA tier).


### **Nice-to-have**


Useful but not critical (secondary categorization).


### **Stay-in-HubSpot**


CRM-specific data irrelevant to support.


### **Drop**


Dead fields from abandoned processes.


Most teams can reduce 50-200 properties by 40-60% through this exercise. Migrating everything creates cluttered Zendesk interfaces that agents ignore.


**Critical data mapping considerations:**


### **Pipeline-to-status mapping**


HubSpot stages must map to Zendesk statuses.


### **Multi-contact associations**


HubSpot supports many tickets-to-contacts; Zendesk requires a single requester.


### **Dropdown values**


Must match exactly between platforms or imports fail.


### **Engagement types**


Emails become public comments, notes become private comments, calls become internal comments with metadata.


Pre-create the required Zendesk custom fields and valid dropdown options before import, then validate field mappings in a test migration so unsupported values can be corrected before the production load.


For teams maintaining a[knowledge base](https://unthread.io/products/knowledge-base?ref=unthread.io) , plan article migration separately. Download embedded images and update internal links before HubSpot decommission, or the organization will face broken documentation.


## **Executing the Migration**


### **Step-by-Step Transfer of Help Desk Software**


The extraction phase is where most DIY migrations stall. HubSpot's API architecture requires multiple endpoints to reconstruct complete ticket records.


**The multi-endpoint extraction process:**


1. Fetch tickets via batch read endpoint (100 per call)
2. Fetch ticket associations and identify linked contacts, companies, conversations, and relevant CRM activities
3. Retrieve associated conversation threads and messages, plus any additional activities that need to become Zendesk comments
4. Download required attachments from HubSpot
5. Fetch contact details for requester attribution
6. Normalize and sort the resulting history chronologically per ticket


Extraction throughput varies substantially with ticket history depth, attachment volume, API batching, and retry behavior. Benchmark your own HubSpot instance with a representative sample before estimating the full extraction window.


**Key technical constraints:**


- HubSpot Search API has a[hard limit of 10,000 results](https://clonepartner.com/blog/how-to-migrate-from-hubspot-service-hub-to-zendesk-complete-guide?ref=unthread.io) , requiring date-range windowing
- Zendesk Ticket Import API allows 700 requests/minute on Enterprise, 400/minute on Professional
- Attachment limits vary by channel and upload method; Zendesk currently allows files up to 50 MB for standard ticket attachments, so validate oversized HubSpot files separately before migration


**Test migration is non-negotiable:**


Run 200-500 representative tickets through Zendesk's sandbox environment before full migration. Validate engagement assembly, author attribution, and attachment handling. Teams that skip this step catch mapping errors on 200,000 production records instead of 500 test records.


Use Zendesk's Ticket Import API because it supports original ticket and comment timestamps and does not run triggers on the initial import of non-closed tickets. Zendesk also notes that normal ticket metrics and SLA calculations are not supported for imported tickets, so tag migrated records and exclude historical imports from current SLA reporting where appropriate. Tag all imported tickets with "migrated-from-hubspot" for easy identification.


## **Configuring Zendesk for Effective Internal Service Management**


With data migrated, configuration determines whether agents can actually work effectively. Zendesk's structure differs enough from HubSpot that direct feature mapping rarely works.


**Workflow decomposition from HubSpot to Zendesk:**


### **Ticket-scoped event-driven workflows**


Become Zendesk Triggers (fire on ticket create/update).


### **Ticket-scoped time-driven workflows**


Become Zendesk Automations (run hourly).


### **Cross-object workflows**


Have no Zendesk equivalent and must move to middleware or be accepted as lost.


This last point catches teams off-guard. HubSpot workflows can touch contacts, companies, deals, and tickets in single automations. Zendesk triggers only affect tickets. Workflows like "When deal closes, notify CSM via Slack" must stay in HubSpot CRM (if decoupled) or move to Zapier/Make.


**SLA policy configuration:**


Zendesk supports multiple SLA policies with condition-based matching, more granular than HubSpot's approach. Define policies based on:


- Employee group, department, service tier, or request type
- Ticket priority and category
- Business hours vs. 24/7 coverage
- Escalation thresholds


**Plan requirements by Zendesk tier:**


Suite Professional ($115/agent/month when billed annually) includes advanced internal support capabilities and supports custom ticket statuses, but does not include Zendesk's standard sandbox environment.


Suite Enterprise adds enterprise governance capabilities such as sandbox environments and custom agent roles, with current pricing available from Zendesk sales.


Teams with complex internal service workflows should compare Zendesk tiers against their specific requirements before purchasing. Custom ticket statuses are available on Professional, while features such as Zendesk's standard sandbox environment and certain enterprise governance controls require higher-tier capabilities.


## **Integrating the New Help Desk with Enterprise Tools**


Zendesk's integration ecosystem differs from HubSpot's CRM-centric approach. Plan the integration architecture before cutover.


**For decoupled model migrations:**


Install the Zendesk-HubSpot marketplace app to maintain CRM visibility. This provides a sidebar showing contact, company, and deal data within Zendesk. Note that integration is one-way from Zendesk to HubSpot, meaning ticket events write to HubSpot, but CRM changes do not push to Zendesk.


**Common integration points:**


### **Task management**


Jira, Asana, Linear for development team escalations.


### **Communication**


Slack for notifications and team collaboration.


### **Identity**


Okta, Microsoft 365 for SSO.


### **Phone systems**


Most major providers have Zendesk apps.


### **Middleware**


Zapier or Make for cross-object workflow bridging.


**What does not transfer:**


- HubSpot Playbooks (guided agent scripts) require recreation as Zendesk macros or use third-party apps like Zingtree
- HubSpot NPS/CES surveys require replacement (Zendesk offers native CSAT only; NPS requires marketplace apps)
- HubSpot unified CRM timeline is lost in full departure, partially mitigated by sidebar in decoupled model


For organizations using[integrations](https://unthread.io/products/integrations?ref=unthread.io) across multiple platforms, document all connection points during planning and verify each works post-migration.


## **Post-Migration Optimization**


### **Enhancing Performance for Internal Teams**


Migration completion is the starting line, not the finish line. The weeks following cutover determine long-term adoption success.


**Validation checklist before declaring success:**


- Record-count reconciliation (tickets exported vs. tickets created)
- Sample 20-50 tickets comparing HubSpot timeline vs. Zendesk comment thread
- Verify timestamps show original dates, not migration date
- Check all attachments download correctly
- Test Zendesk-HubSpot sidebar app functionality (if decoupled)
- Have 2-3 agents work in Zendesk for 1-2 days before full rollout


**Agent training requirements:**


Budget 2-3 days for structured training covering:


- Zendesk workspace navigation
- Macro usage and creation
- Sidebar CRM reference (if decoupled)
- View management and filtering
- SLA tracking and escalation protocols


**Performance monitoring setup:**


Track key metrics from day one to identify optimization opportunities. For teams using[analytics](https://unthread.io/products/analytics?ref=unthread.io) dashboards, establish baselines during parallel operation when both systems run simultaneously.


Keep HubSpot Service Hub active[8-12 weeks post-migration](https://clonepartner.com/blog/how-to-migrate-from-hubspot-service-hub-to-zendesk-complete-guide?ref=unthread.io) as a fallback and reference archive. Run Delta migration to capture tickets created during the cutover window.


**Continuous improvement targets:**


- Agent efficiency gains from a simplified interface and purpose-built tools
- Reduced ticket handling time through mature macro library
- Lower escalation rates through proper SLA configuration
- Improved employee service experience through faster internal response and resolution times


## **Building an Intelligent Internal Help Desk with Unthread**


After completing the HubSpot to Zendesk migration, many organizations discover that traditional helpdesk platforms still leave gaps in internal service delivery. Purpose-built solutions designed specifically for internal teams offer capabilities that legacy ticketing systems cannot match.


Unthread provides[AI-powered automation](https://unthread.io/products/agentic-ai?ref=unthread.io) that learns from resolved tickets to handle routine employee requests automatically, reducing manual workload for IT and HR teams. The platform's[workflow automations](https://unthread.io/products/automations?ref=unthread.io) enable sophisticated routing, escalation, and resolution logic without requiring engineering resources or middleware platforms.


For organizations managing employee knowledge, Unthread's[knowledge base](https://unthread.io/products/knowledge-base?ref=unthread.io) integrates directly into internal communication channels, surfacing relevant answers where employees already work. Combined with[analytics](https://unthread.io/products/analytics?ref=unthread.io) that provide visibility into request patterns, resolution times, and team performance, internal service teams gain the insights needed to continuously optimize operations.


Whether running a dedicated[IT service desk](https://unthread.io/solutions/it-service-desk?ref=unthread.io) or managing cross-functional employee support, the post-migration period offers an opportunity to implement purpose-built tools designed for how internal teams actually work.


## **Frequently Asked Questions**


### **What happens to HubSpot data if the decoupled model is chosen and a later decision is made to leave HubSpot entirely?**


The decoupled model provides flexibility for phased departures. Ticket history lives in Zendesk from migration day forward, while CRM data remains accessible in HubSpot. If a later decision is made to leave HubSpot completely, CRM data (contacts, companies, deals) can be exported via CSV for archive purposes or migrated to a replacement CRM like Salesforce. The Zendesk-HubSpot sidebar app simply stops functioning when HubSpot access ends. Some organizations downgrade to HubSpot's free tools rather than immediately deleting the account, but downgrade limits can affect access to features and historical activity. Export any CRM history required for long-term retention before ending the paid subscription.


### **How should tickets be handled that were created by contacts who no longer exist in HubSpot?**


Orphaned engagements from deleted contacts are a common challenge during extraction. HubSpot retains engagement data even when associated contacts are deleted, but requester attribution becomes impossible. During extraction, identify these tickets through missing contact associations. In Zendesk, create a placeholder user (e.g., "Unknown Requester - Migrated") and assign orphaned tickets to this account. Add an internal note to each affected ticket documenting the original HubSpot ticket ID for reference. This preserves ticket history and conversation content while clearly marking records with incomplete attribution.


### **Can a partial migration be run to test Zendesk with a subset of the team before committing fully?**


Yes, and this approach reduces risk significantly. Create a dedicated Zendesk instance for a pilot team (often IT or a specific support queue). Migrate 60-90 days of historical tickets for that queue only, configure workflows for that team's processes, and run parallel operations. The pilot team works in Zendesk while other teams remain on HubSpot. After 4-6 weeks, evaluate agent satisfaction, resolution metrics, and workflow effectiveness. This approach costs more in temporary parallel licensing but provides validated evidence for migration decisions rather than theoretical comparisons.


### **What compliance considerations should be accounted for when migrating between platforms?**


Data retention obligations vary by industry, record type, and regulatory context. Before deleting HubSpot data, confirm which migrated records are subject to your organization's HIPAA, SOX, FINRA, or other retention requirements and verify that the required history is preserved in an appropriate system. Both platforms offer SOC 2 Type II compliance, and migration services like ClonePartner maintain SOC 2 certification. HIPAA-related deployments require eligible services, appropriate security configuration, and a signed Business Associate Agreement where applicable. Confirm current plan eligibility and covered services with both HubSpot and Zendesk before migrating PHI. For cross-region transfers (e.g., HubSpot EU data center to Zendesk US), verify additional legal basis under GDPR and document data processing agreements accordingly.


### **How should the decision be made between DIY migration, automated tools, and professional services?**


Base the decision on ticket volume, history complexity, custom fields, workflows, integrations, and available engineering capacity. Smaller and relatively standardized migrations may be practical to handle internally or with migration tooling, while environments with multiple pipelines, extensive historical conversations, cross-object workflows, or complex integrations are stronger candidates for specialist migration services. Benchmark a representative sample before estimating engineering effort or cost.
