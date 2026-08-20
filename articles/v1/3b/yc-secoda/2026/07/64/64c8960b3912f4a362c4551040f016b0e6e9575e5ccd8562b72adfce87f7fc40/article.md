---
schema_version: "1.0.0"
document_id: "64c8960b3912f4a362c4551040f016b0e6e9575e5ccd8562b72adfce87f7fc40"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/glean-secoda-integration-data-discovery"
published_at: null
first_seen_at: "2026-07-22T12:57:57.585252+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:c92c32f000f4ea1e3fbca92d1bee6f2379a6eabd9eac18551747f7d7ccd74280"
---

# New Secoda connector in Glean to power seamless data discovery

At Secoda, we believe that trusted data should be easy to find, wherever your employees are searching. As more organizations adopt AI platforms like Glean to unify discovery across their internal knowledge, the need to bring[enterprise-grade data cataloging](https://www.secoda.co/enterprise) and documentation from Secoda into that broader search experience is growing.


In recent months, customers like Super.com, Vanta, Cargurus, and ID.me have all asked us the same thing: *“Can we make our Secoda content appear in Glean so users can find it alongside everything else?”* We’re excited to share that we’ve built a Secoda connector for Glean to do exactly that.


The new connector allows Secoda to push metadata from your workspace (e.g. documents, tables, glossary terms) into Glean’s search index, enabling discoverability of your data assets directly from Glean. This drives greater adoption of Secoda, reduces the dependence on data teams, and supports a more seamless search experience across the tools employees already use.


## **What is Glean?**


Glean is a Work AI platform that connects to over 100+ SaaS applications and tools like Google Drive, Hubspot, Notion, Zoom, and more, to make internal knowledge easily searchable, understandable, and actionable - all from one place. It’s designed to help employees quickly find and take action on the information they need across fragmented systems, without having to know where it lives. By indexing content across your organization, Glean acts as a central hub for knowledge discovery and collaboration.


## **Why did we build a Glean connector?**


For many of our customers, Glean has become the go-to place to search for internal knowledge. But while Glean makes it easy to find documents, messages, and wiki content, **trusted data and metadata about tables, columns, lineage, and definitions live in Secoda** .


While Glean provides powerful tools for indexing structured knowledge, users may still face challenges in quickly finding specific metadata, often leading them to switch tools or reach out to the data team for assistance with queries that could otherwise be easily resolved.


As our customers roll out Glean company-wide, they’ve asked for Secoda’s metadata and documentation to appear directly in Glean to improve discoverability and further embed trusted data into their organization's knowledge ecosystem.


Ultimately, this connector is about meeting users where they already search and helping data teams scale the impact of their work.


## **How does the Glean connector work?**


We’ve enabled customers to push Secoda metadata into Glean’s search index, so it shows up alongside internal content from other sources.


By default, Secoda pushes metadata for the following entity types:


- Documents
- Questions
- Glossary terms
- Columns
- Tables
- Schemas
- Databases
- Charts
- Dashboards


Admins can customize which entities get pushed using **filter rules** . For example, to only push documents, you can create a rule that includes just the “Document” entity type.


Pushes can run automatically on a schedule using a cron expression defined in the connector's Schedule tab. For teams that want more control, admins can also manually push metadata by clicking “Run sync” and then “Push metadata” in the settings.


Once metadata is pushed, Glean begins indexing it. After indexing is complete, Secoda resources will appear in Glean’s search results hyperlinked back to the original resource in Secoda.


## **What does this mean for our customers?**


For business users, this connector makes it dramatically easier to discover and understand trusted data, right in the flow of work. No more digging through yet another tool. Secoda’s documentation and definitions appear wherever employees are already searching.


For data teams, it's a way to extend the impact of their work.[Lineage](https://www.secoda.co/blog/a-complete-guide-to-data-lineage) , definitions, and curated documentation from Secoda can now reach more users, without the friction of switching platforms.


And for those leading[governance](https://www.secoda.co/blog/data-governance-the-ultimate-guide) and data quality initiatives, the connector helps promote a culture of data literacy and self-service by making trusted, governed data more visible across the organization.


## **Conclusion**


At Secoda, we’re focused on making trusted data more accessible and actionable without disrupting how teams work. That’s why we continue to invest in extensions that bring Secoda into the tools employees already use.


Just like our Chrome extension helps surface context in data warehouses and BI tools, and our integrations with Slack, Microsoft Teams, and PagerDuty support collaboration around data workflows, the Glean connector is designed to embed Secoda content directly into daily search behavior.


These extensions help drive adoption by making data easier to find and use in the tools people rely on. When data becomes more approachable,[governance](https://www.secoda.co/data-governance) gains momentum and reach across the business.


This is core to our product philosophy: meet users where they are, and make trusted data easy to discover and use, everywhere they work.


To get access to the Glean connector or learn how it could work in your environment, reach out to your Secoda customer team at[\[email protected\]](https://www.secoda.co/cdn-cgi/l/email-protection) .
