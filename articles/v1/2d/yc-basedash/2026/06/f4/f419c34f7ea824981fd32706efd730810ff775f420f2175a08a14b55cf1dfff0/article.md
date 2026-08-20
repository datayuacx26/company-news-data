---
schema_version: "1.0.0"
document_id: "f419c34f7ea824981fd32706efd730810ff775f420f2175a08a14b55cf1dfff0"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/data-masking-in-bi-tools-how-to-protect-pii-in-dashboards/"
published_at: "2026-06-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:994c91ee0f7040dd579d672979705cfc6603e563ca0df127f6fa635f0451545d"
---

# Data masking in BI tools: how to protect PII in dashboards and AI queries

Data masking hides or obscures sensitive values, like email addresses, card numbers, or salaries, so people can use a dashboard without seeing the real data underneath. In a business intelligence tool, masking sits next to two other controls people often confuse it with: row-level security decides which rows you can see, encryption protects data at rest and in transit, and masking changes what a specific column shows to a specific user.


This guide is for anyone responsible for putting sensitive data into dashboards: a founder wiring up a customer-facing analytics view, an analyst who has to share a revenue dashboard with a vendor, or a data lead who needs PII to stop leaking into screenshots and CSV exports. It covers the main masking techniques, the difference between static and dynamic masking, a framework for deciding where to enforce it, how masking interacts with AI-generated queries, and how the major data platforms implement it.


## What data masking actually means


Masking transforms a value before it reaches the person looking at it. The underlying record can stay intact in the database, but the version that lands in the chart, table, or export is altered. A support agent sees` ****1111` instead of a full card number. A marketing analyst sees` j****@acme.com` instead of a customer’s real email. A manager outside the finance team sees a salary band instead of an exact figure.


The point is to let people do useful analysis without handing them data they have no business reading. You still get accurate counts, trends, and segments. You just remove the raw identifiers and sensitive details that create risk if they end up in a screenshot, a shared link, or an AI-generated answer.


### Masking vs encryption vs row-level security


These three controls solve different problems and you usually need more than one.


Control What it protects Who it targets What the user sees


Encryption Data at rest and in transit Anyone without the key (attackers, infrastructure) Nothing, until decrypted by the system


Row-level security Which records you can access Authorized users with partial access A filtered subset of rows


Data masking Which values inside a column you can read Authorized users who can see the row but not the field A transformed value (partial, hashed, redacted)


Encryption stops someone who steals the disk or intercepts traffic. Row-level security stops a sales rep from seeing another region’s accounts. Masking handles the in-between case: the user is allowed to see the row, and even needs to, but should not read one specific column in the clear. For a deeper look at how row and column controls fit together, see the guide on[data governance for AI-powered BI](https://www.basedash.com/blog/data-governance-for-ai-powered-bi-row-level-security-access-controls-and-compliance) .


## The main data masking techniques


Not all masking is equal. Each technique trades readability for analytical usefulness in a different way.


Technique What it does Example Reversible Keeps analytical value


Redaction / nulling Replaces the value with` NULL` or a fixed token` salary` shows` ***` No Low


Partial masking Reveals only part of the value` 4111 1111 1111 1111` shows` **** 1111` No Medium (last four for support)


Hashing One-way hash of the value`[\[email protected\]](https://www.basedash.com/cdn-cgi/l/email-protection)` becomes a` sha256` digest No Grouping and joins only


Tokenization Swaps the value for a token mapped in a secure vault`[\[email protected\]](https://www.basedash.com/cdn-cgi/l/email-protection)` becomes` tok_9f2a` Yes, with vault access Referential integrity preserved


Generalization Reduces precision into buckets Date of birth becomes birth year; ZIP becomes region No High for aggregates


Format-preserving Replaces with a realistic fake value of the same shape Real email becomes a synthetic but valid-looking email No High for demos and lower environments


A few practical notes. Hashing is useful when you need to count distinct users or join two tables on the same identifier without exposing the identifier itself, but a deterministic hash is still a stable pseudonym, so treat it as personal data under most privacy frameworks. Tokenization is the only common technique that is reversible by design, which is what you want when an authorized downstream system has to recover the original value. Generalization is the unsung hero of analytics masking: bucketing ages, salaries, or locations keeps cohorts and distributions useful while removing the identifying precision.


## Static vs dynamic masking


This distinction matters more than any single technique, because it determines whether the real data still exists where people are querying.


**Static masking** permanently transforms data when you copy it somewhere, usually from production into a staging, development, or analytics environment. The copy contains no real values, so anyone with access to that environment is safe by construction. Use static masking to build realistic test databases and demo datasets without cloning live PII.


**Dynamic masking** leaves the real data in place and applies the transformation at query time, based on who is asking. The same column returns the true value to an authorized role and a masked value to everyone else. This is what you want inside a live BI tool, because the dashboard reads from the real source and you still need authorized users (a fraud analyst, a billing admin) to see the unmasked field while everyone else does not.


Most regulated workflows use both: static masking to keep non-production environments clean, and dynamic masking to govern the live BI layer.


## Where to enforce masking: a four-layer decision


The most common masking mistake is enforcing it in the wrong place. A value masked only in the dashboard UI reappears the moment someone exports a CSV, hits the API, or asks an AI to write the query. Here is a simple framework. There are four layers where masking can live, and the rule is to enforce it as close to the data as the field’s sensitivity demands.


**Layer 1: the warehouse or database.** Masking policies defined in Snowflake, BigQuery, or Databricks apply to every query path, including direct SQL, exports, and AI-generated queries. This is the strongest option and the right default for true regulated PII (payment data, health data, government identifiers), because nothing that reads the table can bypass it.


**Layer 2: the view or semantic layer.** Define masked columns once in a governed view or semantic model, and every tool that queries through that layer inherits the masking. This is a good fit when your team consistently queries models rather than raw tables. The weakness is obvious: it only protects people who go through the layer, so it has to be paired with locking down raw table access.


**Layer 3: the BI tool.** Most BI tools can hide or mask columns by role in their own permission model. This is convenient and fast to configure, but it only protects data flowing through that tool’s UI. If the masking is not enforced at the query layer, it can leak through exports, scheduled emails, embedded views, or AI features. Only rely on this layer alone for fields that are noisy or mildly sensitive rather than regulated.


**Layer 4: the delivery or export layer.** Redacting values in a generated PDF, a CSV, or an embedded dashboard is a last resort. Treat it as defense in depth, not your primary control.


The practical heuristic: regulated PII gets masked at Layer 1 or 2 so it is safe regardless of access path; internal-but-noisy fields can be masked at Layer 3 for convenience. If you only remember one thing, it is that hiding a column in the UI is not the same as masking the data.


## How AI BI tools change the masking problem


AI-assisted analytics, where anyone can ask a question in plain English and get a generated SQL query and chart, widens the surface area for leaks in three specific ways.


**Generated SQL can select anything.** If a user asks “show me our customers and their emails,” a naive AI BI tool will happily write` SELECT name, email FROM customers` . If masking is only configured in the dashboard layer, the generated query bypasses it entirely. Masking has to be enforced at the query layer (Layer 1 or 2) so the AI literally cannot retrieve the raw value.


**Natural-language answers can leak even when results are masked.** An AI might summarize a result set as “the three highest-paid employees are in engineering, earning over $200K,” surfacing compensation in prose even if the salary column was masked in the table. A governed setup applies the same access policies to AI-generated text, not just to query results.


**Exports and schedules inherit whatever the layer below them allows.** Scheduled emails, CSV downloads, and embedded tiles only stay masked if masking lives beneath the UI. The safest pattern is to exclude truly sensitive columns from the set the AI is allowed to reference at all, so they never appear in a query, a chart, or an explanation. The same principle applies when you let autonomous agents touch your data, which is covered in[how to give AI agents safe access to your business data](https://www.basedash.com/blog/how-to-give-ai-agents-safe-access-to-your-business-data) .


## How the major platforms implement masking


Most masking should be enforced where your data lives. Here is how the common platforms approach it.


Platform Masking mechanism Where it runs Notes


Snowflake Dynamic Data Masking and column-level masking policies Query time, schema-level policy Role-based; applies across every query path


BigQuery Dynamic data masking with policy tags, plus column-level security Query time Masking rule attached to a policy tag and a principal


Databricks (Unity Catalog) Column masks and row filters via SQL UDFs Query time Applied with` ALTER TABLE ... ALTER COLUMN ... SET MASK`


PostgreSQL No native dynamic masking; use views or the PostgreSQL Anonymizer extension View or extension Largely do-it-yourself


BI tool layer (for example, Basedash) Governed SQL models with role-based column controls Query layer of the tool Define masked or excluded columns once so the rule applies to ad hoc and AI-generated queries and to exports


Snowflake’s[Dynamic Data Masking](https://docs.snowflake.com/en/user-guide/security-column-ddm-intro) lets you attach a masking policy to a column so the value is masked or shown based on the querying role at runtime. BigQuery applies[dynamic data masking](https://cloud.google.com/bigquery/docs/column-data-masking-intro) through policy tags managed in Dataplex, so a principal sees a masked value while authorized users see the real one. Databricks supports[column masks](https://docs.databricks.com/aws/en/tables/row-and-column-filters) on Unity Catalog tables using SQL functions. PostgreSQL has no built-in dynamic masking, so teams typically expose masked views or use the open-source[PostgreSQL Anonymizer](https://postgresql-anonymizer.readthedocs.io/) extension.


The takeaway: if your warehouse can enforce masking, do it there, and pick a BI tool that respects those policies rather than one that tries to re-implement security in its own UI. If you are still deciding which tool to trust with sensitive data, the comparisons of[BI tools for row-level security](https://www.basedash.com/blog/best-bi-tools-for-row-level-security-compared-2026) and[BI tools for regulated industries](https://www.basedash.com/blog/best-bi-tools-for-regulated-industries-hipaa-sox-gdpr-compliance-compared-2026) are a useful starting point.


## A checklist for masking PII in dashboards


- Inventory the sensitive columns: identifiers, contact details, financials, health data, government IDs.
- Classify each column by sensitivity and the regulation that applies (GDPR, HIPAA, PCI DSS).
- Decide the technique per column: partial masking for support fields, generalization for analytics, hashing for joins, tokenization where the value must be recoverable.
- Enforce masking at the warehouse or view layer for regulated PII, not only in the BI tool’s UI.
- Confirm masking survives exports, scheduled emails, API access, and embedded views.
- Verify AI-generated SQL cannot select masked columns, and that natural-language answers respect the same policy.
- Define which roles see unmasked values, and review that list on a schedule.
- Log access to sensitive columns so you can answer “who saw this” during an audit.


## Common mistakes


**Masking only in the dashboard UI.** The single most common error. The value reappears in CSV exports, scheduled reports, and AI answers. Enforce it at the query layer.


**Treating hashed identifiers as anonymous.** A deterministic hash is a stable pseudonym, so it is still personal data under GDPR, which defines[pseudonymization](https://gdpr-info.eu/art-4-gdpr/) as a distinct concept from true anonymization. Hashing reduces exposure; it does not remove the data from scope.


**Cloning production into staging unmasked.** Lower environments usually have looser access and weaker monitoring, so an unmasked copy is often the biggest real risk. Use static masking when you copy.


**Masking the column but exposing it elsewhere.** A masked` email` column is pointless if the same address sits in a free-text notes field or a URL parameter. Mask every place the value appears.


**Over-masking analytical fields.** Nulling out a date or location when bucketing would do destroys cohort and trend analysis for no extra protection. Match the technique to the actual risk.


## When you do not need data masking


Masking adds configuration and can degrade analysis, so do not apply it reflexively. You probably do not need it when the dashboard contains no personal or regulated data (aggregate revenue, system metrics, anonymous product usage), when every viewer is already cleared to see the underlying records, or when row-level security alone fully solves the access question. Reserve masking for the specific case where someone can legitimately see the row but should not read a particular field, or where data leaves a trusted boundary, such as a[dashboard shared outside your company](https://www.basedash.com/blog/how-to-share-dashboards-outside-your-company-securely) .


## Frequently asked questions


**Is data masking the same as anonymization?** No. Masking obscures values for display while the real data still exists in the source and can be shown to authorized users. Anonymization irreversibly removes the ability to identify a person. Most BI masking is closer to pseudonymization, which keeps the data in scope for privacy regulations.


**Does masking slow down queries?** Dynamic masking adds a small amount of work at query time because the engine applies the policy per column, but on Snowflake, BigQuery, and Databricks the overhead is generally negligible for typical dashboard queries. Static masking has no query-time cost because the transformation already happened.


**Can I mask data in PostgreSQL or MySQL directly?** Neither has full native dynamic masking the way Snowflake or BigQuery does. The common approaches are exposing masked views, applying column permissions, or using an extension such as PostgreSQL Anonymizer. Many teams enforce masking in a governed view or semantic layer instead.


**How does masking work with AI-generated queries?** It only works reliably if it is enforced below the AI, at the warehouse or view layer, so the generated SQL cannot retrieve the raw value. A well-governed AI BI tool also excludes sensitive columns from what the AI can reference and applies the same policy to the natural-language explanation, not just the table of results.


**What is the difference between column-level security and data masking?** Column-level security controls whether a user can access a column at all, often hiding it entirely. Masking lets the user access the column but shows a transformed value. They are frequently combined: hide the columns a role never needs, and mask the ones it needs in partial or generalized form.
