---
schema_version: "1.0.0"
document_id: "5109677dd1fbffaded0a0b562d3a5e896bd3468755813348e0ecd19e348b7a4e"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/sentinel-detection-rules-migration"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T17:43:11.677045+00:00"
fetched_at: "2026-07-29T17:43:13.146694+00:00"
content_hash: "sha256:a381c57f2cb0129f2af1f6945bccb4dfe8fa39210d3aac02374cef508aa783ec"
---

# Stop rewriting detection rules by hand: automatic Sentinel-to-Elastic migration is here

29 July 2026 •


[Charles Davison](https://www.elastic.co/security-labs/author/charles-davison)


# Stop rewriting detection rules by hand: automatic Sentinel-to-Elastic migration is here


Elastic's first automatic migration from a modern SIEM. Translate your Sentinel detection rules into Elastic Security without rebuilding them.


4 min read


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering) ,


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


Elastic automatically translates your Microsoft Sentinel detection rules into Elastic Security. Export your Scheduled and Near Real Time (NRT) analytics rules from Sentinel, upload them, and Elastic picks up the mapping and translation from there using an LLM you choose. Watchlists and severity mappings carry over. This is the first automatic migration path off a modern SIEM, available now in Tech Preview in 9.5, and it works across multiple cloud providers and regions so you can deploy closer to where your data lives.


##


Which Microsoft Sentinel rule types can be migrated automatically?


Automatic Migration focuses on the rules that carry your detection logic. In 9.5, it translates Scheduled and Near Real Time (NRT) analytics rules from Microsoft Sentinel, exported from your Sentinel workspace, and handles the translation for you.


It uses the same[mapping and translation](https://www.elastic.co/blog/automatic-migration-ai-rule-translation) as our existing rule migrations, now extended to Microsoft Sentinel. The following are supported:


- Integration identification with just rule export
- Support for the following rule types:


- Near-real-time (NRT) detection analytics rules
- Scheduled Analytic Rules


- Support for Watchlists to ES|QL Lookups
- Severity Mapping


##


**How to migrate Microsoft Sentinel detection rules to Elastic**


The migration runs in a few steps, from exporting your rules in Sentinel to reviewing the translated versions in Elastic. Once you've decided which rules and data to migrate, follow these steps:


1. On the Security Launchpad, open Manage Automatic Migrations, select your AI provider, and expand Migrate your existing SIEM rules to Elastic.


1. Select the drop-down on the top right for Microsoft Sentinel. Let Elastic guide you through exporting your rules from Sentinel and uploading them into Elastic Security. Elastic handles the finer details by scanning for watchlists and then prompts you to upload them when found.


1. Once the rules are uploaded, you can view their status.


- Installed: Already added to Elastic SIEM. Click View to manage and enable it.
- Translated: Ready to install. This rule was mapped to an Elastic-authored rule, or translated by[Automatic Import](https://www.elastic.co/docs/explore-analyze/ai-features/automatic-import) . Click Install to install it.
- Partially translated: Part of the query could not be translated. You may need to specify an index pattern for the rule query, upload missing files, or fix broken rule syntax.
- Not translated: None of the original query could be translated.
- Failed: Translation failed. Refer to the error for details.


For more information, refer to the[technical documentation](https://www.elastic.co/docs/solutions/security/get-started/automatic-migration) .


1. After clicking View Rules, you will have the ability to edit and install rules.


##


Should you migrate rules first or data first?


One of the first decisions in a migration is sequencing: data or rules first. Elastic supports both paths, so you can start wherever makes sense for your team.


Path When to use What happens


Rules first You do not know exactly which data sources to prioritise before moving any logs. Translate your Sentinel rules first. Elastic identifies which integrations those rules need, so you can plan data onboarding around what your detections actually require.


Data first Your log sources are already being onboarded, or you want detections to work the moment they're installed. Onboarding data beforehand improves the translation quality. Onboard your log sources into Elastic, then migrate your Sentinel rules to match. Rules can be installed and enabled immediately against data that's already flowing.


Custom data You have proprietary or non-standard log sources that don't map to a prebuilt Elastic integration. Use[Automatic Import](https://www.elastic.co/blog/elasticsearch-custom-integrations-automatic-import) to ingest custom data sources in minutes, then migrate or write rules against them.


By identifying exactly which integrations are needed before moving a single log, teams can build a precise, risk-aware roadmap for their migration project. This transparency eliminates the guesswork and helps ensure that critical visibility gaps are addressed long before you fully decommission your environment.


##


What happens after your Sentinel rules are running in Elastic


Once your rules are running in Elastic,[Workflows](https://www.elastic.co/docs/explore-analyze/workflows) lets you build automation around them. The moment a rule fires, a workflow can kick off multi-step remediation, enrichment, and notification automatically. And building these automations in[Agent Builder](https://www.elastic.co/docs/solutions/security/ai/agent-builder/agent-builder) lowers the barrier, so you can create a workflow in natural language.


##


How Elastic AI fits into a Sentinel-to-Elastic migration


Elastic Security brings generative AI into the SOC with[retrieval augmented generation (RAG)](https://www.elastic.co/docs/solutions/search/rag) and open agentic frameworks. Automatic Migration joins the lineup of Elastic Security’s AI features, helping SOC teams strengthen defenses across the IT environment:


- [Automatic Migration for Detection Rules](https://www.elastic.co/docs/solutions/security/get-started/automatic-migration) complements Elastic’s deep library of prebuilt rules to broaden detection use case coverage.
- [Automatic Import](https://www.elastic.co/blog/elasticsearch-custom-integrations-automatic-import) extends visibility *and powers detection rules* by onboarding custom data sources in minutes.
- [Agent Skills](https://www.elastic.co/security-labs/skills-elastic-security-9-4) assist in the response process and less time context switching.


Elastic’s SIEM and XDR solution helps analysts detect earlier and respond faster.


##


Try automatic detection rule migration


Migrating a SIEM has always meant rebuilding your detection rules by hand, and that cost is what keeps teams on a platform long after they've decided to leave. Automatic Migration simplifies that process, providing mapping to existing Elastic rules and helping to translate the rest. Your watchlists and severity levels carry over as well, and you move on your own terms, with your data and your tooling under your control. For further details check out our[documentation](https://www.elastic.co/docs/solutions/security/get-started/automatic-migration) .


[Try it free](https://www.elastic.co/cloud/cloud-trial-overview) , or[get in touch](https://www.elastic.co/splunk-interest?elektra=organic&storm=CLP&rogue=splunkobs-gic) . Have feedback? Tell us what you think in the[Elastic Community Slack channel](https://elasticstack.slack.com/) or on the[Elastic Security forum](https://discuss.elastic.co/c/security/83) .


#### Jump to section


- [Which Microsoft Sentinel rule types can be migrated automatically?](https://www.elastic.co/security-labs/sentinel-detection-rules-migration#which-microsoft-sentinel-rule-types-can-be-migrated-automatically)
- [**How to migrate Microsoft Sentinel detection rules to Elastic**](https://www.elastic.co/security-labs/sentinel-detection-rules-migration#how-to-migrate-microsoft-sentinel-detection-rules-to-elastic)
- [Should you migrate rules first or data first?](https://www.elastic.co/security-labs/sentinel-detection-rules-migration#should-you-migrate-rules-first-or-data-first)
- [What happens after your Sentinel rules are running in Elastic](https://www.elastic.co/security-labs/sentinel-detection-rules-migration#what-happens-after-your-sentinel-rules-are-running-in-elastic)
- [How Elastic AI fits into a Sentinel-to-Elastic migration](https://www.elastic.co/security-labs/sentinel-detection-rules-migration#how-elastic-ai-fits-into-a-sentinel-to-elastic-migration)
- [Try automatic detection rule migration](https://www.elastic.co/security-labs/sentinel-detection-rules-migration#try-automatic-detection-rule-migration)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Stop%20rewriting%20detection%20rules%20by%20hand:%20automatic%20Sentinel-to-Elastic%20migration%20is%20here&url=https://www.elastic.co/security-labs/sentinel-detection-rules-migration)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/sentinel-detection-rules-migration)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/sentinel-detection-rules-migration&title=Stop%20rewriting%20detection%20rules%20by%20hand:%20automatic%20Sentinel-to-Elastic%20migration%20is%20here)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/sentinel-detection-rules-migration&title=Stop%20rewriting%20detection%20rules%20by%20hand:%20automatic%20Sentinel-to-Elastic%20migration%20is%20here)
