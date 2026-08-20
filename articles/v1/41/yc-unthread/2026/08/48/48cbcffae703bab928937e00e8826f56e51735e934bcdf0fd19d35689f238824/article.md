---
schema_version: "1.0.0"
document_id: "48cbcffae703bab928937e00e8826f56e51735e934bcdf0fd19d35689f238824"
company_key: "yc-unthread"
company: "Unthread"
source_id: "yc-unthread-rss-c618c87b9ca4"
canonical_url: "https://unthread.io/blog/migrate-freshdesk-zendesk/"
published_at: "2026-08-01T15:57:57+00:00"
first_seen_at: "2026-08-01T16:08:13.104986+00:00"
fetched_at: "2026-08-01T16:08:14.021725+00:00"
content_hash: "sha256:ded7b95089861efce5f432cea23433bea2ca0f2269476354741283a1e0534d34"
---

# Migrate from Freshdesk to Zendesk: Step-by-Step Freshdesk Migration Guide

Platform migrations sound like pressing a button. Export tickets from Freshdesk, import into Zendesk, done in a weekend. Then two weeks later, teams discover their SLAs report garbage numbers, not a single automation made the trip, and inline images in historical tickets show broken placeholders.


A Freshdesk to Zendesk migration is a data-model translation problem where Freshdesk's flat, contact-centric structure must be restructured to match Zendesk's layered user-organization-ticket model. The platforms use different terminology, different status values, and different approaches to automation logic. Success requires understanding what actually transfers, what requires manual rebuilding, and how to validate results before going live.


This guide breaks down the complete migration process into actionable steps, realistic timelines, and decision frameworks for choosing between migration methods. Whether handling 5,000 tickets or 500,000, the same fundamental questions apply.


## **Key Takeaways**


- **Migration is a three-part project, not a one-click export** . Historical data may transfer in hours, but rebuilding or validating automations, SLA policies, and business rules can add several weeks. Some migration services can transfer selected macros and triggers, while complex workflows and SLA configurations usually require manual review or recreation.
- **What migrates versus what requires manual rebuild determines timeline** . Tickets, conversations, contacts, and[knowledge base](https://unthread.io/products/knowledge-base?ref=unthread.io) articles transfer automatically, while automations, triggers, macros, and SLA policies may require migration add-ons or manual rebuilding.
- **Field mapping errors discovered after full migration cost days to fix** . Running a[demo migration](https://help-desk-migration.com/freshdesk-to-zendesk-migration/?ref=unthread.io) of 20 tickets before committing reveals inline image breaks, custom field mismatches, and conversation truncation issues.
- **Migration method choice depends on volume, complexity, and risk tolerance** . Automated SaaS tools work for most standard migrations, specialist-led services handle complex transformations and compliance requirements, and DIY scripts require 2-6 weeks of engineering time.
- **Teams often discover migration is the perfect moment to evaluate simpler alternatives** . Rebuilding complex automation logic in a new platform reveals opportunities to consolidate workflows in tools like[Slack-native help desks](https://unthread.io/products/slack-support?ref=unthread.io) that eliminate context-switching entirely.


## **Understanding What Actually Transfers During Migration**


Migration services move historical internal-support data while preserving employee request threads, attachments, requester relationships, team assignments, and knowledge base content. The process handles data extraction, field mapping, format transformation, and import. But the critical distinction is between records that transfer and configuration that requires rebuilding.


### **Data that transfers automatically with migration tools**


- Tickets with full conversation threads (public replies and private notes)
- Contacts mapped to Zendesk users
- Companies mapped to Zendesk organizations
- Agents and group assignments
- Custom field values (with proper mapping configuration)
- Knowledge base articles, categories, and folders
- Attachments (with some size limitations)


### **Configuration that may require migration add-ons, manual rebuilding, or post-migration validation**


- Automations, triggers, and business rules
- SLA policies and escalation paths
- Macros and canned responses
- Business hours and holiday schedules
- Ticket forms and conditional fields
- Routing rules and assignment logic
- [Integrations](https://unthread.io/products/integrations?ref=unthread.io) with third-party tools
- Satisfaction ratings (must be stored as tags or custom fields)


Some migration providers support selected macros and triggers, but availability varies by migration route. Complex conditions, SLA policies, and platform-specific actions should still be reviewed and tested manually.


This distinction matters because teams consistently underestimate the rebuild phase. A 2-day data migration still requires 5-10 days to recreate business logic. Budget these as separate workstreams with dedicated resources.


## **Choosing Your Migration Method**


Four approaches exist for moving data from Freshdesk to Zendesk, each with distinct trade-offs between cost, control, and complexity.


### **Zendesk Native CSV Import (Free, Limited)**


Zendesk's current user Data Importer supports files containing up to 500,000 rows and 200 columns. Limits vary by record type and importer, so confirm the applicable requirements before preparing user, organization, or custom-object files. However, basic CSV import is not designed to preserve complete ticket histories. This method works only for user and organization lists, not for preserving support history.


### **Automated SaaS Migration Tools**


Services like Help Desk Migration provide wizard-driven interfaces that connect both platforms via API tokens. These tools handle tickets with[full conversation threads](https://help-desk-migration.com/freshdesk-to-zendesk-migration/?ref=unthread.io) , timestamp preservation, attachments, and knowledge base content. Pricing generally scales with the number of records and any optional services, such as delta migration, custom transformations, or managed implementation.


Automated tools work well for 5,000-100,000 tickets with standard field structures. Limitations include potential inline image breaks (Freshdesk-hosted URLs expire after migration) and attachment size caps around 15-20MB.


### **Specialist-Led Migration Services**


Companies like ClonePartner assign dedicated engineers who handle custom scripting, rate-limit management, inline image re-hosting, and large attachment handling. This approach suits 50,000+ ticket migrations, regulated industries requiring HIPAA or GDPR compliance, and organizations with complex custom field transformations.


Specialist-led services include sandbox testing, post-migration support (typically 48 hours), and explicit handling of edge cases that automated tools might skip silently.


### **DIY API Scripts (Free, 2-6 Weeks Engineering Time)**


Building custom migration scripts using the Ticket Import API provides complete control over transformation logic. Zendesk's Ticket Import API allows migration tools and custom scripts to preserve supported ticket and comment timestamps, including creation and update dates.


This approach requires handling rate limits, pagination for tickets with many conversations, retry logic for failed requests, and error recovery procedures. Freshdesk API limits depend on the account's plan and rate-limit model. Trial accounts default to 50 calls per minute, while some older plans use per-minute limits and newer accounts may use hourly account-wide limits. Check the account's response headers and current Freshdesk documentation before setting migration concurrency.


DIY migrations make sense when in-house engineering capacity exists, unique edge cases require custom handling, and comfort maintaining migration code through the entire project.


## **Step-by-Step Migration Process**


### **Step 1: Audit Your Freshdesk Data (2-5 Days)**


Export sample tickets (100-500) and document the complete data inventory. Count total tickets (open versus closed), list all custom fields (ticket, contact, and company levels), identify agents and groups, and screenshot automation rules.


**Key audit deliverables:**


- Spreadsheet inventory of what needs to migrate
- Documentation of custom fields and their values
- Screenshots of SLA policies and business rules
- Identification of dead custom fields to prune
- Decision on which historical data matters versus what can be archived


This audit prevents discovering missing field mappings mid-migration. Use Freshdesk's native export (Admin → Export) to get accurate counts.


### **Step 2: Provision Accounts and Select Method (1-3 Days)**


Create a Zendesk sandbox or trial instance for testing. If using a third-party migration service, sign up and connect both platforms via OAuth or API tokens.


Avoid testing against a Freshdesk trial account, which has a 50 requests per minute hard cap. Test against paid sandbox environments or request temporary rate-limit increases.


### **Step 3: Pre-Configure Zendesk Environment (2-5 Days)**


Before importing any data, manually create all reference objects in Zendesk:


- Custom fields (ticket, user, and organization levels)
- Groups matching Freshdesk team structure
- Agent accounts with correct permissions
- Ticket forms (if using multiple forms)


Disable all triggers and automations during import to prevent welcome emails and auto-assignment from firing on historical tickets. Disable welcome emails specifically. Set ticket form conditions to "Never required" temporarily.


Import order matters critically: fields, groups, organizations, users, then tickets. Importing tickets before agents and groups exist creates orphaned records that require manual cleanup.


### **Step 4: Map Fields Carefully (1-2 Days)**


Build a complete field mapping specification. The platforms use different terminology and status values:


**Status mapping:**


- Freshdesk Open → Zendesk Open
- Freshdesk Pending → Zendesk Pending
- Freshdesk Resolved → Zendesk Solved (different terminology)
- Freshdesk Closed → Zendesk Closed


**Priority mapping:**


- Freshdesk Medium → Zendesk Normal


**Entity mapping:**


- Freshdesk Contact → Zendesk User
- Freshdesk Company → Zendesk Organization


Custom dropdown values require transformation. Freshdesk display values like "Hardware Issue" must become Zendesk tag format like "hardware_issue" during import.


### **Step 5: Run Demo Migration and Verify (2-3 Days)**


Launch a[free demo migration](https://help-desk-migration.com/freshdesk-to-zendesk-migration/?ref=unthread.io) (typically 20 tickets plus 20 knowledge base articles). Manually inspect results in Zendesk:


**Verification checklist:**


- Open tickets and check conversation threads display correctly
- Verify attachments download without errors
- Confirm inline images render (not broken placeholder icons)
- Check custom field values populated in correct locations
- Validate timestamp preservation (creation and update dates)
- Test that private notes remain private


Finding issues in a 20-ticket demo costs hours to fix. Finding the same issues after importing 75,000 tickets costs days or requires complete re-migration.


### **Step 6: Execute Full Migration with Delta Sync**


Communicate a clear freeze point or cutover date to the team. Run the full migration overnight or over a weekend. Migration speed varies by API limits, ticket complexity, attachment volume, conversation length, and migration method. Use the demo or provider estimate to calculate an expected runtime for the specific dataset rather than relying on a universal tickets-per-hour benchmark.


After the bulk import completes, run a delta migration to capture tickets created or updated since the freeze point. This prevents losing conversations that agents handled during the migration window.


Monitor progress throughout. A migration may take anywhere from several hours to several days depending on record volume and source-platform constraints. Ask the migration provider to estimate runtime after it has counted records, comments, attachments, and knowledge base content.


### **Step 7: Rebuild Configuration (5-10 Days)**


This phase requires the most effort and receives the least planning. Migration tools transfer data, but business logic requires manual recreation.


**What requires manual recreation:**


- Convert Freshdesk Dispatch'r rules to Zendesk triggers (event-based)
- Convert Freshdesk Scenario Automations to Zendesk automations (time-based)
- Recreate canned responses as Zendesk macros
- Rebuild SLA policies from scratch
- Reconfigure business hours and holiday schedules
- Set up routing rules and assignment logic


Test each recreated trigger with sample tickets before going live. A single misconfigured automation can generate thousands of incorrect notifications.


This rebuild phase is where many IT teams realize the opportunity to simplify. Instead of recreating complex rule chains, teams increasingly consolidate on[Slack-native internal help desks](https://unthread.io/solutions/it-service-desk?ref=unthread.io) that handle[workflow automation](https://unthread.io/products/automations?ref=unthread.io) through natural language rather than trigger conditions.


### **Step 8: Cutover and Go-Live (1-2 Days)**


Execute the final cutover:


- Run one more delta sync to capture stragglers
- Update the relevant employee-support mailbox, forwarding rule, or mail connector so new internal requests enter Zendesk
- Replace or reconfigure employee portal links, web forms, and embedded widgets as applicable
- Disable Freshdesk portal (or set read-only)
- Send test emails to confirm Zendesk ticket creation
- Brief agents (30-minute orientation on Agent Workspace, views, macros)


Keep Freshdesk read-only for 2-4 weeks as a safety net. If the cutover requires DNS changes, review the existing TTL in advance and temporarily lower it where appropriate. Continue monitoring Freshdesk during the transition because cached DNS records, forwarding rules, or unchanged employee bookmarks can still send requests to the old system.


## **Common Migration Pitfalls and How to Avoid Them**


**Inline images break post-migration.** Freshdesk stores pasted screenshots as Freshdesk-hosted URLs. After migration, these URLs expire and images show broken placeholders. Enable "migrate inline images as attachments" if the migration tool offers it, or use specialist-led services that re-host images.


**Attachments over 15-20MB get silently skipped.** Some migration tools impose lower practical limits than the platform APIs allow. Verify attachment size distribution during the audit and handle large files separately if needed.


**Conversation threads truncate at 10 messages.** Freshdesk's API returns only 10 conversations inline by default. Migration tools must use separate API calls for tickets with extensive thread history. Verify conversation-count parity during QA.


**Rate limit exhaustion stalls migration.** Monitor rate-limit headers and throttle proactively. For large migrations, consider purchasing Zendesk's High Volume API add-on (2,500 requests per minute) or run migrations during off-hours when other integrations are quiet.


## **Real-World Migration Outcomes**


**Lleida.net (63,000+ tickets):** The global e-communication provider completed their migration with zero errors, achieving "very efficient and fast" results according to their Helpdesk IT Director.


**PillDrill (~1,500 tickets, healthcare):** This HIPAA-regulated company migrated internal support data including approximately 600 contacts with highly secure handling aligned with healthcare privacy requirements. Their VP of Operations noted "superb service, very secure and great attention to detail."


**Mid-market SaaS (75,000 tickets, 40 agents):** A 28-day project achieved 99.8% ticket parity and 99.6% conversation parity, with zero downtime through phased cutover.


## **Simplify Internal Support with Unthread**


The rebuild phase forces teams to document every automation, trigger, and workflow accumulated over years. This documentation often reveals duplicated rules, contradictory logic, and processes nobody remembers creating.


Rather than recreating this complexity in a new platform, many internal IT and HR teams use migration as an opportunity to consolidate on simpler tools. A[Slack-native help desk](https://unthread.io/products/slack-support?ref=unthread.io) that turns a #it-help channel into a full ticketing system eliminates the separate platform entirely. Employees submit requests without leaving Slack, tickets route automatically based on content, and the[knowledge base](https://unthread.io/products/knowledge-base?ref=unthread.io) learns from resolved conversations.


This approach particularly suits organizations where most internal requests already happen in Slack. Rather than training employees on a new external portal, meet them where they already work.[Unthread's integrations](https://unthread.io/products/integrations?ref=unthread.io) connect existing tools, while[workflow automations](https://unthread.io/products/automations?ref=unthread.io) replace complex trigger chains with natural language rules.


## **Frequently Asked Questions**


### **How do I handle tickets containing sensitive employee information, especially HR, payroll, benefits, or legal requests?**


Migration services transfer tickets as-is, including private notes and sensitive content. For HIPAA, GDPR, or other regulated migrations, verify the provider's current security documentation, subprocessors, encryption controls, data-retention terms, and independent audit reports. When protected health information is involved, confirm that the provider will sign an appropriate Business Associate Agreement before granting access. After migration, audit which historical tickets contain sensitive data and apply appropriate access controls in Zendesk. For ongoing privacy needs, consider platforms that support private ticketing flows where sensitive HR requests move to DMs rather than shared channels.


### **What happens to historical SLA metrics and performance reports after migration?**


Zendesk does not support reliable native metrics or SLA calculations for imported tickets. Reporting on imported historical tickets can therefore produce incomplete or inaccurate results. Tag migrated tickets, exclude them from Zendesk SLA reporting, and preserve historical performance data separately before decommissioning Freshdesk. Historical performance data exists only in Freshdesk exports or documentation created before decommissioning.


### **Can both Freshdesk and Zendesk run simultaneously during transition, or must there be a hard cutover?**


Phased parallel operation is possible but requires careful coordination. Route new tickets to Zendesk while continuing to resolve open tickets in Freshdesk. This extends the transition period and doubles licensing costs temporarily, but reduces go-live risk. Most teams prefer a defined cutover date with delta migration to capture stragglers, keeping Freshdesk read-only for 2-4 weeks as a fallback reference.


### **How do integrations with tools like Slack, Jira, or Salesforce that connect to the current Freshdesk instance get handled?**


Integrations require separate reconfiguration in Zendesk. Document all current Freshdesk integrations during the audit, then rebuild each connection in Zendesk using native apps or API configurations. Some integrations may work differently between platforms. Many teams discover this is a good moment to evaluate whether a Slack-first approach with[built-in integrations](https://unthread.io/products/integrations?ref=unthread.io) simplifies their stack compared to maintaining multiple point-to-point connections.


### **What's the minimum viable migration if only recent tickets matter and historical data can be archived?**


Significantly reduce migration complexity by limiting scope to the past 90-180 days of tickets. Export older data to CSV or archive storage for compliance purposes without migrating it to the live system. This approach cuts migration costs, speeds timeline, and avoids dealing with outdated custom field values or defunct agent accounts. Define retention requirements before deciding what truly needs to exist in the new platform versus what needs to be accessible in archives.
