---
schema_version: "1.0.0"
document_id: "d34a3f8c8d7837826c7392da39e48403635533248c3ad45e9d2785f853f9e213"
company_key: "yc-sre-ai"
company: "SRE.ai"
source_id: "yc-sre-ai-news-import-a9b2aa0ab1a6"
canonical_url: "https://www.sre.ai/post/measuring-salesforce-data-quality"
published_at: "2025-09-08T00:00:00+00:00"
first_seen_at: "2026-07-24T02:05:06.902612+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:63209a28faddec864fcd0d9a4551069e09d34cf05627fb369831f10daec157e3"
---

# Measuring Salesforce Data Quality

Your Salesforce org is only as good as the data inside it.


It doesn't matter how elegant your automation is, how well-architected your data model is, or how powerful your integrations are. If the data is bad, everything downstream is compromised.


Bad data shows up everywhere:


- Sales reps waste time calling disconnected numbers
- Marketing sends emails to duplicate contacts
- Reports show inflated pipeline numbers that don't reflect reality
- Automated processes fail because required fields are empty
- AI and analytics produce garbage insights from garbage inputs


The problem is that data quality issues accumulate slowly, invisibly, until one day you realize your org is a mess.


But here's the thing: data quality is measurable.


And once you can measure it, you can fix it.


# Why data quality matters more than ever


Salesforce isn't just a database anymore. It's a platform.


Your data feeds reporting dashboards, powers AI models, triggers automations, and informs business decisions at every level.


When data quality degrades, the impact compounds:


- **Operational inefficiency** : Teams spend more time cleaning data than using it
- **Lost revenue** : Bad contact data means missed opportunities
- **Compliance risk** : Outdated or incorrect data can violate regulations like GDPR or HIPAA
- **Broken automation** : Flows and processes fail when they encounter unexpected data states
- **Poor decision-making** : Leadership makes strategic calls based on flawed reports


47% of teams experienced data or metadata loss in 2024. And that's just the catastrophic stuff, the incidents where data disappeared entirely.


The more insidious problem is data *degradation* : slow rot that happens record by record, field by field, until your org is full of information you can't trust.


# The anatomy of data debt


Data debt comes in many forms:


### 1. Dirty data


- Duplicate records
- Incomplete records (missing required or important fields)
- Inconsistent formatting (phone numbers, addresses, names)
- Outdated information (old job titles, defunct companies, disconnected emails)


### 2. Structural debt


- Overuse of custom objects when standard objects would suffice
- Data stored in the wrong objects or fields
- Bloated picklists with hundreds of unused values
- Text fields used for data that should be structured (dates, numbers, booleans)


### 3. Configuration debt


- Overly complex automation (Flows with 100+ elements, Process Builder stacking)
- Data validation rules that conflict with each other
- Formula fields that reference deprecated fields
- Triggers that haven't been updated in years


### 4. Access debt


- Sharing rules that expose sensitive data
- Overprivileged users with access they don't need
- Old integration users that are still active
- Records owned by deactivated users


Let's look at how to measure each of these.


# Metrics for data quality


### 1. Duplicate record rate


**What it measures** : The percentage of records that have duplicates in your org.


**Why it matters** : Duplicates inflate counts, confuse users, and create data integrity issues. If your sales team is looking at three different Account records for the same company, someone's going to work with outdated information.


**How to measure** :


- Run duplicate detection rules on key objects (Accounts, Contacts, Leads)
- Count matches and calculate: Duplicate records ÷ Total records × 100
- Break down by object and severity (exact duplicates vs. fuzzy matches)


**What good looks like** :


- Duplicate rate below 2% for critical objects
- Automated deduplication processes running regularly


**Red flags** :


- Duplicate rate above 10%
- Multiple records for the same customer with different data
- No duplicate prevention rules in place


**How to fix it** :


- Enable Salesforce's built-in duplicate management
- Implement matching rules and duplicate rules
- Run regular deduplication campaigns
- Block duplicate creation at the point of entry


### 2. Data completeness score


**What it measures** : The percentage of required and important fields that are populated.


**Why it matters** : Incomplete records break automation, skew reporting, and frustrate users. If 30% of your Contact records are missing phone numbers, your calling campaigns are starting with a handicap.


**How to measure** :


- Identify critical fields for each object (required fields + business-critical fields)
- Count records where all critical fields are populated
- Calculate: Complete records ÷ Total records × 100


**What good looks like** :


- Completeness above 95% for critical fields
- Trend showing improvement over time


**Red flags** :


- Completeness below 80%
- Key fields (like email, phone, or address) frequently empty
- No validation rules enforcing data entry


**How to fix it** :


- Make critical fields required
- Use validation rules to enforce data quality at entry
- Run data enrichment campaigns to backfill missing information
- Integrate with data enrichment services (Clearbit, ZoomInfo, etc.)


### 3. Data accuracy rate


**What it measures** : The percentage of records with accurate, up-to-date information.


**Why it matters** : Old or incorrect data is worse than no data. If your reps are calling disconnected numbers or emailing bounced addresses, they're wasting time.


**How to measure** :


- Email bounce rate for Contact records
- Phone number disconnect rate (from call logs)
- Address validation failures
- Manual spot checks of sample records


**What good looks like** :


- Email bounce rate below 5%
- Phone disconnect rate below 10%
- Regular data hygiene processes


**Red flags** :


- Bounce rates above 15%
- Records that haven't been updated in years
- No process for validating or refreshing data


**How to fix it** :


- Implement email validation tools
- Use address verification services
- Set up automated processes to flag stale records
- Schedule regular data refresh campaigns


### 4. Data consistency score


**What it measures** : How consistently data is formatted across records.


**Why it matters** : Inconsistent formatting breaks reporting, automation, and search. If phone numbers are stored as "(555) 123-4567", "555-123-4567", and "5551234567", you can't reliably match or deduplicate.


**How to measure** :


- Audit key fields for formatting variations
- Check for inconsistent capitalization, spacing, or abbreviations
- Count records that don't match expected patterns


**What good looks like** :


- Standardized formats enforced by validation rules
- Consistent capitalization and formatting across records


**Red flags** :


- Wide variation in how the same type of data is entered
- No validation rules enforcing format
- Users entering free text in fields that should be structured


**How to fix it** :


- Use validation rules to enforce formatting standards
- Implement picklists instead of text fields where possible
- Use formula fields to normalize data automatically
- Run data cleanup scripts to standardize existing records


### 5. Record age and staleness


**What it measures** : How long it's been since records were last updated.


**Why it matters** : Old data is often bad data. If a Contact record hasn't been touched in three years, the person probably doesn't work there anymore.


**How to measure** :


- Calculate average age of records (days since last modified)
- Identify records that haven't been updated in 6, 12, or 24 months
- Track percentage of "stale" records by object


**What good looks like** :


- Active records updated within the last 90 days
- Clear policies for archiving or deleting stale data


**Red flags** :


- Large percentage of records untouched for years
- No process for identifying or retiring old data
- Users creating new records instead of updating old ones


**How to fix it** :


- Implement data aging policies (archive records after X months of inactivity)
- Use automation to flag stale records for review
- Encourage users to update records rather than create duplicates


# Metrics for configuration and metadata quality


### 6. Flow complexity score


**What it measures** : The size and complexity of your automation.


**Why it matters** : Long, complex Flows are hard to maintain, debug, and optimize. A Flow with 200+ elements is tech debt waiting to explode.


**How to measure** :


- Count the number of elements in each Flow
- Track the number of decision branches and loops
- Identify Flows with excessive complexity (100+ elements)


**What good looks like** :


- Most Flows under 50 elements
- Clear, well-documented logic
- Modular design with subflows for reusable logic


**Red flags** :


- Flows with 100+ elements
- Deeply nested decision trees
- No documentation on what the Flow does or why


**How to fix it** :


- Break large Flows into smaller, modular components
- Use subflows to encapsulate reusable logic
- Add descriptions and documentation to every Flow
- Regular audits to identify and refactor complex automation


### 7. Automation overlap and redundancy


**What it measures** : How many automation tools are doing similar things.


**Why it matters** : If you have a Process Builder, a Flow, and a trigger all firing on the same object, you're asking for trouble. Overlapping automation creates unpredictable behavior and performance issues.


**How to measure** :


- Count active automation on each object (Flows, Process Builders, Workflow Rules, Triggers)
- Identify objects with 3+ active automations
- Map automation to understand execution order and potential conflicts


**What good looks like** :


- One automation tool per object (ideally Flows)
- Clear documentation of what each automation does
- No conflicts or race conditions


**Red flags** :


- Multiple automation tools triggering on the same object/action
- Process Builders still in use (Salesforce deprecated them)
- No visibility into automation execution order


**How to fix it** :


- Consolidate automation into Flows
- Migrate from Process Builder and Workflow Rules
- Document all automation and their trigger conditions
- Use a tool to visualize automation dependencies


### 8. Picklist health


**What it measures** : The quality and usability of your picklists.


**Why it matters** : Picklists are supposed to standardize data entry. But over time, they accumulate cruft (duplicate values, outdated options, inconsistent naming) that defeats the purpose.


**How to measure** :


- Count the number of values in each picklist
- Identify unused or rarely-used values
- Check for duplicates or near-duplicates ("Active" vs "active")
- Measure how many values have been added in the last year


**What good looks like** :


- Picklists with 10-20 values (not 100+)
- Regular audits to remove unused values
- Consistent naming conventions


**Red flags** :


- Picklists with 50+ values
- Values that haven't been used in years
- Inconsistent capitalization or formatting


**How to fix it** :


- Audit picklists annually
- Remove or archive unused values
- Standardize naming conventions
- Consider dependent picklists to reduce complexity


### 9. Custom object and field sprawl


**What it measures** : How many custom objects and fields you've created.


**Why it matters** : Every custom object and field adds complexity. If you're not careful, your data model becomes an unmaintainable maze.


**How to measure** :


- Count total custom objects and fields
- Identify objects/fields that are rarely or never used
- Calculate field usage rate: Records with data in field ÷ Total records × 100


**What good looks like** :


- Custom objects and fields created with purpose
- Usage rates above 70% for custom fields
- Regular audits to identify and remove unused customizations


**Red flags** :


- Hundreds of custom fields on standard objects
- Custom objects with no records
- Fields that are populated in less than 10% of records


**How to fix it** :


- Document the purpose of every custom object/field before creating it
- Audit usage regularly and deprecate unused customizations
- Use standard objects and fields when possible
- Implement a governance process for new customizations


### 10. Metadata vs. database configuration split


**What it measures** : How much of your configuration lives in metadata (fields, objects, automation) vs. data (custom settings, custom metadata types).


**Why it matters** : Metadata is version-controlled and can be deployed with your release pipeline. Database configuration (stored as records) is harder to track, backup, and deploy.


**How to measure** :


- Identify configuration stored as data (Custom Settings, lookup tables, config objects)
- Count the number of config records vs. metadata components
- Assess how much of your automation relies on database config


**What good looks like** :


- Critical configuration stored as metadata (Custom Metadata Types, not Custom Settings)
- Clear separation between data and configuration
- Configuration changes deployed through your DevOps pipeline


**Red flags** :


- Heavy reliance on Custom Settings for config
- Configuration stored in standard objects (Accounts, Contacts, etc.)
- No process for backing up or versioning config data


**How to fix it** :


- Migrate Custom Settings to Custom Metadata Types
- Use hierarchy custom settings only when necessary
- Include custom metadata in your version control and deployment process
- Document what config lives where


# Metrics for data security and access


### 11. Overprivileged user percentage


**What it measures** : How many users have more access than they need.


**Why it matters** : Following the principle of least privilege reduces security risks. But over time, permissions accumulate. People get promoted, change roles, or join new projects, and nobody revokes their old access.


**How to measure** :


- Audit user permissions against their current role
- Count users with "View All" or "Modify All" permissions
- Identify users with admin privileges who don't need them


**What good looks like** :


- Users have only the access required for their role
- Regular access reviews (quarterly or bi-annually)
- Admin access limited to 5-10% of users


**Red flags** :


- Large percentage of users with admin or elevated permissions
- No process for reviewing or revoking access
- Shared accounts or generic user logins


**How to fix it** :


- Conduct quarterly access reviews
- Implement role-based access control (RBAC)
- Remove permissions when users change roles
- Use permission sets for temporary access needs


### 12. Orphaned record ownership


**What it measures** : How many records are owned by deactivated users.


**Why it matters** : When a user is deactivated, their records don't disappear. But if those records aren't reassigned, they can become invisible to the rest of the team.


**How to measure** :


- Count records owned by inactive users
- Calculate percentage of total records affected


**What good looks like** :


- All records reassigned when users are deactivated
- Automated process for handling ownership transfers


**Red flags** :


- Thousands of records owned by ex-employees
- No process for reassigning records during offboarding


**How to fix it** :


- Build an offboarding checklist that includes record reassignment
- Use automation to flag orphaned records
- Implement a policy for mass reassignment before deactivation


### 13. Sensitive data exposure


**What it measures** : How much sensitive data is accessible to users who shouldn't have it.


**Why it matters** : PII, financial data, and health records need to be protected. If your sharing rules are too permissive, you're risking compliance violations and data breaches.


**How to measure** :


- Identify fields containing sensitive data (SSN, credit cards, medical info)
- Audit who has access to those fields (via profiles, permission sets, sharing rules)
- Check field-level security settings


**What good looks like** :


- Sensitive data restricted to authorized users only
- Field-level security enforced for PII
- Regular audits of data access


**Red flags** :


- Sensitive fields visible to all users
- No field-level security in place
- Overly permissive sharing rules


**How to fix it** :


- Implement field-level security on sensitive fields
- Use Salesforce Shield Platform Encryption for highly sensitive data
- Audit sharing rules and restrict access
- Use tools like Data Detect to identify exposed PII


# Metrics for data operations


### 14. Data load and backup frequency


**What it measures** : How often you're backing up your data and how fresh those backups are.


**Why it matters** : Data loss happens. Whether it's a bad deployment, a user error, or a malicious deletion, you need to be able to restore. But backups are only useful if they're recent.


**How to measure** :


- Frequency of backups (daily, weekly, monthly)
- Time since last successful backup
- Coverage: What percentage of your data is backed up


**What good looks like** :


- Daily backups of critical data
- Backups tested regularly to ensure they work
- Clear recovery process documented


**Red flags** :


- Backups running less than weekly
- No test restores performed
- Large gaps in backup coverage


**How to fix it** :


- Implement automated daily backups
- Test your restore process quarterly
- Use third-party backup solutions if native tools aren't sufficient


### 15. Data migration success rate


**What it measures** : The percentage of data migrations (imports, updates, deletions) that succeed without errors.


**Why it matters** : Failed data loads corrupt your org and create cleanup work. High failure rates indicate poor data quality in source systems or inadequate validation.


**How to measure** :


- Successful records ÷ Total records attempted × 100
- Track by data source and object type


**What good looks like** :


- Success rate above 95%
- Errors logged and addressed immediately


**Red flags** :


- Success rate below 80%
- Same errors recurring across migrations
- No process for handling failed records


**How to fix it** :


- Validate data before attempting to load it
- Use staging environments to test migrations
- Implement better error handling and logging
- Address root causes of failures in source systems


# How to implement data quality metrics


### 1. Start with a baseline audit


You can't improve what you don't measure. Run a comprehensive audit of your org to establish baseline metrics for the areas that matter most.


### 2. Prioritize based on business impact


Not all data quality issues are created equal. Focus first on the data that:


- Powers critical business processes
- Drives revenue (leads, opportunities, accounts)
- Is subject to compliance requirements
- Is used in AI or analytics


### 3. Set targets and timelines


Once you have a baseline, set realistic improvement targets:


- Reduce duplicate rate from 15% to 5% within six months
- Increase data completeness from 70% to 90% within a quarter
- Audit and clean picklists annually


### 4. Automate measurement


Manual audits are useful, but they don't scale. Use tools to continuously monitor data quality:


- Salesforce's built-in reports and dashboards
- Third-party data quality tools (Validity DemandTools, Cloudingo, etc.)
- Custom automation to flag issues in real time


### 5. Make metrics visible


Share data quality metrics with stakeholders:


- Weekly or monthly data quality dashboards
- Reports showing trends over time
- Visibility into the cost of poor data quality (wasted time, lost opportunities)


### 6. Build a data governance program


Data quality isn't a one-time cleanup. It's an ongoing discipline.


Establish:


- Clear ownership for data quality (who's responsible?)
- Processes for preventing bad data (validation rules, duplicate management)
- Regular audits and cleanup campaigns
- Training for users on data entry best practices


# Conclusion


Your Salesforce org is a living system. Data flows in from dozens of sources, gets transformed by automation, and powers decisions across your business.


If that data is bad, everything downstream suffers.


The good news? Data quality is measurable. And once you can measure it, you can manage it.


Start with the metrics that matter most to your business. Track them over time. Make the problem visible.


Because clean data isn't a luxury. It's the foundation everything else is built on.


**Need help measuring and improving data quality in your Salesforce org?**


SRE.ai provides visibility into environment drift, metadata health, and automation complexity, helping you identify and fix data quality issues before they become problems.


Let's talk.


‍
