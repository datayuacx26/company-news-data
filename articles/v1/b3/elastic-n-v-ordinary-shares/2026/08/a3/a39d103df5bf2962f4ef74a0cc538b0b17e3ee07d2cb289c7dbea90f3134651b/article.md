---
schema_version: "1.0.0"
document_id: "a39d103df5bf2962f4ef74a0cc538b0b17e3ee07d2cb289c7dbea90f3134651b"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/soc-case-management-detection-rule-history"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T17:38:59.274611+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:f99078b7530b566809b5d8da55407f7216f72ad6976b53b08d8044c2996d5395"
---

# SOC case management and detection rule history in Elastic Security

3 August 2026 •


[Kseniia Ignatovych](https://www.elastic.co/security-labs/author/kseniia-ignatovych) •


[Melissa Burpo](https://www.elastic.co/security-labs/author/melissa-burpo)


# SOC case management and detection rule history in Elastic Security


Elastic Security now tracks every detection rule change with one-click rollback and makes case data queryable out of the box, so SOC teams get audit trails and reporting without configuring anything.


3 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


Elastic Security now tracks every change to a detection rule and lets you roll back to any previous version with one click. The same history log gives compliance teams a timestamped audit trail that's immutable and append-only. Case data is queryable across 3 global indices (down from 12 per space), so SOC managers can build dashboards on closure rates, assignment load, and case volume without configuring anything. A rebuilt template system gives analysts structured, investigation-specific fields at case creation, so the data feeding those dashboards is consistent from the start.


##


Detection rule change history with one-click rollback


Detection rules change constantly. Analysts add exceptions, engineers tune them, detection logic shifts to adapt to new threats. Until now, that history was gone the moment it happened. If a reliable rule stopped firing, there was no built-in way to see what changed, who changed it, or when, which is a debugging problem and a compliance problem in one.


In 9.5, Detection Rules History Management ships as GA. A History section on the rule details page shows a complete, chronological log of every saved rule state: who made the change, when, and the revision number. From there, you can preview any historical revision, compare it to the previous version, and restore it with a single click.


The log is immutable and append-only, and it captures changes made through the UI or the API. Compliance teams get a defensible, timestamped audit trail for ISO 27001, SOC 2, and DORA standards without any manual export or configuration. Detection engineers get a real undo button: no custom scripts, no digging through audit logs.


##


SOC case management: templates and case analytics


Cases are where investigations land, but the data inside them has rarely been reliable enough to learn from. Custom fields were limited in type and count. The same fields appeared on every case regardless of what the analyst was investigating. Building dashboards required manual index configuration that most teams never completed, so case data stayed useful in the moment and hard to aggregate at scale.


In 9.5, we rebuilt the template system to fix how data goes in and made cases queryable out of the box for everything downstream.


###


Investigation-specific case templates with custom fields


Admins can now define templates for specific investigation types. A "Compromised Account" case collects different information than a "Service Outage" case. Admins build templates using a YAML editor with an Actions menu helper and a live preview panel. Analysts pick the right template for their investigation, see only the fields that apply, and fill in what's actually relevant.


The previous cap of 10 templates was a ceiling for enterprise SOCs managing phishing, malware, insider threat, compliance audits, and more. That limit is gone, along with the cap on custom fields. Seven new field types are also now available, including:


- Checkboxes
- Radio buttons
- A user picker
- A date/time picker


Any field can be marked required before case closure, so regulated teams can enforce that fields like "Root Cause" or "Closing Reason" get filled in before a case closes. A Field Library lets admins define reusable fields once and apply them across templates.


###


Queryable case analytics on every deployment


Cases as Data, Elastic Security's case analytics feature, exposes case activity in dedicated analytics indices so teams can build dashboards tracking case volume, closure rates, time to close, and assignment load, rather than relying on the case UI alone.


We shipped Cases as Data as a tech preview in 9.2, but it required manual configuration, wasn't available on Serverless, and had a complex index structure that wasn't ready for broad adoption. In 9.5, it's GA, on by default, and available across every deployment type including Serverless.


The architecture is simpler, too:


Before 9.5 9.5 GA


Index architecture 12 indices per space 3 global indices


Configuration Manual setup required Auto-provisioned


Serverless Not available Available


Data views Manual creation Pre-built Case Analytics view per space


SOC managers, IR leads, and SRE teams can build case reporting on any deployment without manual index configuration.


##


Get started with detection rule history and case analytics


Detection engineers have been working without rule change history. SOC managers have been working without case data they can report on. Both change in 9.5.


Reliable change history and queryable case data are not the loudest features in a release. They are the groundwork the next wave of SOC automation depends on.


See the documentation for[Detection rule change history](https://www.elastic.co/docs/solutions/security/detect-and-alert/view-rule-changes-history) ,[Case Templates](https://www.elastic.co/docs/explore-analyze/cases/manage-case-templates) , and[Case Analytics](https://www.elastic.co/docs/explore-analyze/cases/case-analytics) to get started. Try the new capabilities on your deployment, or[start a free trial](https://www.elastic.co/cloud/cloud-trial-overview/security) . Connect with us on[Elastic's community Slack](https://join.slack.com/t/elasticstack/shared_invite/zt-2sgssfr0n-NhTOlSwHbaGH85tYfx6kGg) to share feedback or tell us what you are building and how we can help.


#### Jump to section


- [Detection rule change history with one-click rollback](https://www.elastic.co/security-labs/soc-case-management-detection-rule-history#detection-rule-change-history-with-one-click-rollback)
- [SOC case management: templates and case analytics](https://www.elastic.co/security-labs/soc-case-management-detection-rule-history#soc-case-management-templates-and-case-analytics)
- [Investigation-specific case templates with custom fields](https://www.elastic.co/security-labs/soc-case-management-detection-rule-history#investigation-specific-case-templates-with-custom-fields)
- [Queryable case analytics on every deployment](https://www.elastic.co/security-labs/soc-case-management-detection-rule-history#queryable-case-analytics-on-every-deployment)
- [Get started with detection rule history and case analytics](https://www.elastic.co/security-labs/soc-case-management-detection-rule-history#get-started-with-detection-rule-history-and-case-analytics)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=SOC%20case%20management%20and%20detection%20rule%20history%20in%20Elastic%20Security&url=https://www.elastic.co/security-labs/soc-case-management-detection-rule-history)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/soc-case-management-detection-rule-history)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/soc-case-management-detection-rule-history&title=SOC%20case%20management%20and%20detection%20rule%20history%20in%20Elastic%20Security)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/soc-case-management-detection-rule-history&title=SOC%20case%20management%20and%20detection%20rule%20history%20in%20Elastic%20Security)
