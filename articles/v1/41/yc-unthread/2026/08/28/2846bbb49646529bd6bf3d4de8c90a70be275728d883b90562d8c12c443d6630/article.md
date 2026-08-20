---
schema_version: "1.0.0"
document_id: "2846bbb49646529bd6bf3d4de8c90a70be275728d883b90562d8c12c443d6630"
company_key: "yc-unthread"
company: "Unthread"
source_id: "yc-unthread-rss-c618c87b9ca4"
canonical_url: "https://unthread.io/blog/migrate-zendesk-intercom/"
published_at: "2026-08-06T12:00:00+00:00"
first_seen_at: "2026-08-07T11:10:58.659265+00:00"
fetched_at: "2026-08-07T11:10:59.292488+00:00"
content_hash: "sha256:4c6c6474f0813f86b198c268629bd86c922429cb377823a77cccd3d6518ef28c"
---

# Migrate from Zendesk to Intercom: Step-by-Step Zendesk Migration Guide

Migrating from Zendesk to Intercom represents more than a platform switch. It fundamentally changes how[internal support teams](https://unthread.io/solutions/it-service-desk?ref=unthread.io) handle requests, shifting from structured ticket queues to conversation-based messaging. This transformation affects everything from agent workflows to how employees interact with IT,[HR](https://unthread.io/solutions/hr-service-desk?ref=unthread.io) , and finance support channels.


For teams managing internal operations across departments, this migration complexity raises an important question: should organizations invest weeks migrating between traditional help desk platforms, or adopt a[Slack-native support solution](https://unthread.io/products/slack-support?ref=unthread.io) that eliminates the need for separate ticket systems entirely? Many IT and HR teams find that keeping[support inside Slack](https://unthread.io/products/slack-support?ref=unthread.io) reduces friction for both employees and support staff while avoiding the data migration challenges outlined below.


## **Key Takeaways**


- Migration complexity extends far beyond data transfer. Moving from Zendesk to Intercom requires understanding a fundamental shift from ticket-centric support to conversation-first messaging, with[migration timelines](https://clonepartner.com/blog/zendesk-to-intercom-migration-guide?ref=unthread.io) spanning 1-8 weeks depending on ticket volume and customization complexity
- Workflow automation requires complete rebuild. Zendesk triggers, automations, and macros do not transfer to Intercom, requiring significant internal time to recreate business logic as Intercom Workflows and Saved Replies
- Data volume determines migration approach. Intercom's native importer handles[fewer than 150,000 tickets](https://help-desk-migration.com/intercom-to-zendesk-migration?ref=unthread.io) , pushing larger organizations toward third-party migration services
- Object mapping requires careful planning. Zendesk tickets become Intercom conversations, organizations become companies, and custom fields require pre-created Data Attributes before migration begins
- Teams increasingly choose Slack-native platforms. Rather than migrating between traditional help desks, organizations are adopting solutions that eliminate the need for separate ticket systems by operating where employees already work


## **Understanding the Landscape**


Zendesk and Intercom represent two different philosophies in help desk software. Zendesk operates as a ticket-centric platform where every interaction becomes a numbered ticket with defined status workflows. Intercom positions itself as a conversation-first platform designed for proactive messaging and in-app engagement.


### **Why Consider Alternatives to Zendesk**


Internal support teams migrate away from Zendesk for several reasons. Cost escalation affects many organizations as their support volume grows. Platform complexity frustrates administrators who need simpler configuration options. The rigid ticket model feels outdated compared to modern conversational interfaces.


However, migrating to Intercom addresses some concerns while introducing others. Intercom's strength lies in proactive engagement and in-app messaging, but its conversation model requires teams to adapt their workflows significantly.


### **The Evolution of Internal Support Platforms**


The help desk landscape has evolved beyond the Zendesk-versus-Intercom comparison. Modern[purpose-built AI agents](https://unthread.io/products/agentic-ai?ref=unthread.io) can[automatically resolve common requests](https://unthread.io/products/agentic-ai?ref=unthread.io) , route tickets to appropriate teams, and generate documentation from resolved conversations. These capabilities exist natively in platforms designed for AI from the ground up, rather than bolted onto legacy ticket systems.


For internal support teams handling IT requests, HR inquiries, and employee service management, Slack-native platforms offer an alternative path. Rather than forcing employees to submit tickets through external portals, these solutions turn existing Slack channels into structured help desks with full ticketing, routing, and[automation capabilities](https://unthread.io/products/automations?ref=unthread.io) .


## **Phase 1: Planning the Migration Strategy**


Successful migration begins with thorough planning. The[preparation phase](https://clonepartner.com/blog/zendesk-to-intercom-migration-guide?ref=unthread.io) typically requires 3-5 days to document scope, categorize data, and align stakeholders on migration objectives.


### **Assessing the Current Zendesk Setup**


Start by auditing the existing Zendesk configuration:


- **Ticket volume and age** (how many total tickets exist, and how far back does history extend)
- **Custom fields** (document every custom ticket field, their data types, and which workflows depend on them)
- **Automations and triggers** (list every automation rule, understanding that none will transfer automatically)
- **Macros** (catalog saved responses agents use daily)
- **Knowledge base structure** (map categories, sections, and article relationships)
- **Integrations** (identify every third-party connection that will need reconfiguration)


### **Defining Intercom Requirements**


Before migrating data, prepare Intercom to receive it. This requires:


- **Manual agent creation** (all Admins must be created in Intercom before migration, as historical ticket assignments fail if target agent IDs don't exist)
- **Team structure setup** (configure Teams to match Zendesk group organization)
- **Data Attribute creation** (Intercom requires custom Data Attributes pre-created before migration; custom field data disappears if attributes aren't ready)
- **Ticket type configuration** (establish ticket types that will receive migrated conversations)


### **Building the Migration Team**


For organizations with[fewer than 10,000 tickets](https://clonepartner.com/blog/zendesk-alternatives-2026-platforms-pricing-migration?ref=unthread.io) and minimal customization, internal teams can often manage migration independently. Mid-complexity migrations with 10,000-100,000 tickets typically benefit from migration specialist support. Organizations with 100,000+ tickets, extensive custom objects, or strict compliance requirements should engage professional migration consultants.


## **Phase 2: Extracting Data from Zendesk**


Data extraction follows a specific sequence to maintain relationships between records. Rushing this phase creates downstream problems that are difficult to correct.


### **Leveraging Zendesk Export Features**


Zendesk provides multiple export methods:


- **CSV exports** (useful for smaller datasets and manual verification)
- **API extraction** (required for large-scale migrations with[rate limits](https://clonepartner.com/blog/zendesk-to-intercom-migration-guide?ref=unthread.io) of 200-700 requests per minute depending on the plan)
- **Incremental Export API** (limited to 10 requests per minute but captures ongoing changes during migration windows)


### **Using the Zendesk API**


Extract data in this order to preserve relationships:


1. **Organizations first** (these become Intercom Companies)
2. **End Users second** (map organization_id to Company associations)
3. **Tickets third** (including all custom field values)
4. **Comments fourth** (maintain chronological ordering and author attribution)
5. **Attachments last** (download locally before re-uploading to Intercom)


**Critical warning:** API-created comments default their author to the API token owner. The system must[explicitly map author_id](https://clonepartner.com/blog/how-to-migrate-from-intercom-to-zendesk-step-by-step?ref=unthread.io) in API calls to preserve original attribution. Failing to do this destroys the audit trail of who said what.


### **Ensuring Data Consistency**


Build safeguards into the extraction process:


- **Use idempotency keys** (unique Zendesk ticket IDs prevent duplicate creation if scripts crash mid-migration)
- **Respect rate limits** (honor Retry-After headers to avoid blocks and data gaps)
- **Validate before proceeding** (compare record counts after each extraction phase)


## **Phase 3: Importing Data into Intercom**


Data transformation sits between extraction and import. Zendesk's data model differs significantly from Intercom's conversation-based structure.


### **Intercom Data Import Best Practices**


The import sequence matters:


1. **Companies** (import Organizations as Companies first)
2. **Contacts** (import Users with Company associations)
3. **Knowledge Base** (migrate articles with collection structure)
4. **Tickets** (create Conversations with proper contact and assignment mappings)
5. **Comments** (add conversation parts in chronological order)
6. **Tags** (apply after primary records exist)


### **Mapping Zendesk Fields to Intercom**


Standard fields achieve[approximately 95% auto-mapping](https://help-desk-migration.com/intercom-to-zendesk-migration?ref=unthread.io) success when using migration tools. Custom fields require manual mapping decisions:


- **Zendesk Ticket Fields** to Intercom Ticket Type Attributes or Data Attributes
- **Zendesk Organizations** to Intercom Companies
- **Zendesk Users** to Intercom Contacts
- **Zendesk Ticket Status** to Intercom Ticket State (requires explicit mapping document)


Document field mappings before importing. Status mapping creates particular challenges since Zendesk's six statuses (New, Open, Pending, On-hold, Solved, Closed) must translate to Intercom's different state model.


### **Handling Attachments and Rich Text**


Attachments require special handling:


- **Download all Zendesk attachments** (internal Zendesk URLs will break after migration)
- **Re-upload via multipart form data** (Intercom requires direct upload, not URL references)
- **Strip complex HTML** (Zendesk comment HTML must be sanitized to basics to avoid breaking Intercom's UI)
- **Test sample articles** (verify formatting before bulk import)


Be aware that Intercom[caps conversation parts](https://clonepartner.com/blog/zendesk-alternatives-2026-platforms-pricing-migration?ref=unthread.io) at 500 per conversation. Long Zendesk tickets with more than 500 comments will be truncated. Identify these tickets before migration and plan appropriate handling.


## **Configuring Intercom for Internal Operations**


After data migration, operational configuration rebuilds support capabilities.


### **Setting Up Inbox and Teams**


Configure the workspace to match the operational model:


- **Inbox views** (create filters matching former Zendesk Views)
- **Team assignments** (configure routing rules for ticket distribution)
- **SLA targets** (establish response and resolution time goals)
- **Operating hours** (set business hours for SLA calculations)


### **Building Out the Knowledge Base**


Knowledge base migration involves structure and content:


- **Collections** (map Zendesk Categories and Sections to Intercom Collections)
- **Article import** (transfer content with formatting review)
- **301 redirects** ([configure redirects](https://helpando.it/zendesk-ticket-and-data-migration?ref=unthread.io) from old Zendesk article URLs to maintain SEO value and prevent broken links)
- **Internal article updates** (update any documentation referencing old Zendesk URLs)


### **Automating Workflows**


This phase consumes the most post-migration effort. Since workflows and automations require complete rebuild, budget significant time:


- **Triggers** to Intercom Workflows and Rules
- **Automations** to Intercom Workflows with scheduled triggers
- **Macros** to Saved Replies
- **SLA policies** to Intercom SLA configuration


For organizations with complex[workflow automation](https://unthread.io/products/automations?ref=unthread.io) requirements, this rebuild phase often exceeds 40 hours. Each Zendesk trigger requires 30-90 minutes to recreate as an Intercom Workflow.


## **Transitioning the Support Team**


Technology migration means little without team adoption. The shift from ticket-based to conversation-based support requires mindset changes.


### **Developing Training Programs**


Structure agent training around key differences:


- **Conversation model** (help agents understand how Intercom's approach differs from Zendesk's ticket queues)
- **Interface navigation** (familiarize teams with inbox, keyboard shortcuts, and productivity features)
- **Workflow changes** (document new processes for routing, escalation, and collaboration)
- **Reporting differences** (train managers on Intercom's analytics and how metrics translate from Zendesk)


Budget adequate time for platform familiarity. This represents real productivity cost during the transition period.


### **Managing the Cutover**


Execute the final switch with minimal disruption:


- **Keep Zendesk read-only** (maintain access for 30-60 days post-cutover as a reference safety net)
- **Run delta syncs** (capture tickets created during the migration window)
- **Reroute channels carefully** (switch MX records, chat widgets, and phone routing to Intercom)
- **Monitor closely** (track first-day metrics against baseline expectations)


## **When Migration Makes Sense**


Migration preserves historical context but carries significant costs. For some organizations, the migration investment exceeds the value of historical data.


Consider whether migration makes sense for the specific situation:


**Migration makes sense when:**


- Regulatory requirements mandate historical data retention
- Employee relationships depend on accessible conversation history
- The organization has invested heavily in Zendesk customizations worth preserving


**Starting fresh makes sense when:**


- Historical tickets rarely get referenced
- There's a fundamental change in support model
- Migration costs exceed the value of historical data


For internal support teams, the calculation often favors fresh starts. Internal IT and HR requests rarely require years of historical context. A[modern IT service desk](https://unthread.io/solutions/it-service-desk?ref=unthread.io) solution that operates inside Slack eliminates migration complexity entirely while providing structured ticketing, routing, and automation where employees already work.


## **Why Unthread Eliminates the Migration Question Entirely**


Organizations using Slack for internal communication can turn channels like #it-help into full help desks with ticket tracking, SLA management, and[self-learning knowledge bases](https://unthread.io/products/knowledge-base?ref=unthread.io) that generate documentation from resolved conversations. This approach skips the migration question entirely by meeting employees where they already communicate.


Rather than forcing teams through weeks of complex migration between traditional help desk platforms,[Unthread's Slack-native approach](https://unthread.io/products/slack-support?ref=unthread.io) provides structured ticketing, intelligent routing, and AI-powered automation without requiring employees to leave their primary workspace. Internal support teams gain full help desk capabilities while eliminating the friction of external portals and avoiding the complexity of platform migrations altogether.


## **Frequently Asked Questions**


### **What happens to Zendesk integrations after migrating to Intercom?**


Every third-party[integration](https://unthread.io/products/integrations?ref=unthread.io) requires reconfiguration in Intercom's app ecosystem. Some Zendesk marketplace apps have Intercom equivalents; others do not. Plan 1-3 hours per integration for reconnection and testing. Create a complete inventory of active integrations before migration, and verify Intercom alternatives exist for critical tools. Custom integrations built on Zendesk's API will need complete rebuilding for Intercom's API structure.


### **How are side conversations handled during migration?**


Zendesk side conversations have no direct Intercom equivalent. Migration services typically convert side conversations to private notes attached to the main conversation. This preserves the information but changes how agents access it. If the team relies heavily on side conversations for internal collaboration, evaluate whether Intercom's internal note system meets needs before committing to migration.


### **Can organizations migrate only specific tickets?**


Intercom's native importer requires importing all tickets without subset selection. Third-party migration services like Help Desk Migration offer filtering options to migrate specific date ranges, ticket types, or status categories. This selectivity allows migrating active and recent tickets while archiving older data in Zendesk's export format for compliance purposes.


### **What security certifications do migration services provide?**


Professional migration services maintain SOC 2 Type II certification, use AES-256 encryption at rest, and TLS 1.2+ encryption in transit. GDPR-compliant data processing agreements are standard. HIPAA compliance with Business Associate Agreements is available upon request for healthcare organizations. Verify specific certifications match compliance requirements before engaging any migration service.


### **How does Intercom's EU data hosting affect migration?**


If choosing Intercom's EU data hosting, be aware that no migration path exists between US and EU Intercom regions after workspace creation. Make this decision before migration begins. Organizations with GDPR-strict requirements operating from US Zendesk instances must carefully plan their Intercom region selection since changing regions later requires starting over completely.
