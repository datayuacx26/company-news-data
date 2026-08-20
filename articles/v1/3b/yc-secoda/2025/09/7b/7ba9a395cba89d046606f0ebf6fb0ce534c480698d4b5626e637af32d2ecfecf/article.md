---
schema_version: "1.0.0"
document_id: "7ba9a395cba89d046606f0ebf6fb0ce534c480698d4b5626e637af32d2ecfecf"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/ai-automation-block-metadata-updates"
published_at: "2025-09-16T00:00:00+00:00"
first_seen_at: "2026-07-24T00:59:20.364488+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:f72efb7ea134dff11d0d9b10d4353e43c5a4e0f860ece3ec8d56c3801fce5115"
---

# Introducing the AI Automation Block: Automate metadata updates with AI in Secoda

Managing metadata should be easy to maintain and easy to scale. With Secoda’s new AI Automation Block, data teams can automate repetitive tasks like generating missing descriptions or tagging undocumented tables. These AI-powered workflows run directly in your existing processes, keeping your catalog accurate and useful without the manual upkeep.


This new block gives you full control over how AI is applied across your catalog. It combines targeted inputs, pre-configured output types, and previewable results. You can review, test, and approve changes before publishing, or let them run on a schedule to keep documentation fresh automatically.


“We wanted to remove the bottleneck around updating metadata and let users scale workflows automatically. The AI Automation Block does exactly that by combining flexible inputs with smart AI and clear, testable outputs.” — Etai Mizrahi, Co-founder & CEO, Secoda


## **What is the AI Automation Block?**


The AI Automation Block is a new block type in Secoda Automations that applies AI-generated metadata updates in bulk and with human in the loop approval. It’s purpose-built for keeping documentation consistent and accurate at scale.


Here’s how it works:


### Step 1: Define your input


Start by selecting the metadata you want to target. For example, you might filter for all tables that are missing descriptions or columns that do not have an assigned owner.


### Step 2: Select your output type


Choose the field you want to update, such as “Description”. This pre-populates the prompt with best-practice instructions that you have defined in your AI settings, and you can further customize the prompt to fit your business-specific requirements and language.


### Step 3: Preview and apply


You can preview the changes on up to three resources by clicking "Generate preview". To review outputs before publishing after the Automation is run, toggle on "Review AI generated output before it publishes." Approve updates in the Automations' Run History section.


Once scheduled, new descriptions or tags are generated every time the automation runs.


## **Why it matters**


The AI Automation Block transforms metadata management:


- **Hours become minutes** : bulk updates that used to take hours are now completed in a few clicks.
- **Consistency improves** : outputs follow the same structure and tone, standardizing documentation for downstream users.
- **Scalability without headcount** : by embedding AI into repeatable workflows, teams can expand coverage without adding manual effort.
- **Control remains with you** : every block includes guardrails like preview and approval before changes go live.


## **Key use cases**


Organizations are already putting the AI Automation Block to work in a variety of ways. Below are a few practical examples, each designed to reduce manual effort and improve data clarity.


**1. Auto-fill missing descriptions** Goal: Keep documentation accurate and current without requiring manual updates.
Setup: Filter for tables or columns with missing descriptions, then use "Generate AI output" with the description field. Use the out of the box template or create a custom prompt like that generates a brief, business-friendly description for this data resource based on its name, schema, and column information.


**2. Auto-tag resources** Goal: Improve data discoverability and strengthen governance practices.
Setup: Filter resources by criteria such as data source or naming patterns, use "Generate AI output" with tags field. Design prompts that consider table purpose, data types, and business domains to suggest appropriate classification tags.


**3. Flag stale assets** Goal: Proactively surface data assets that may need cleanup or deprecation.
Setup: Filter for tables with low[Popularity](https://docs.secoda.co/features/popularity) or Quality, use "Generate AI output" with tags field. Add a tag like “Needs Review” to the prompt, if necessary.


**4. Detect and label PII** Goal **:** Strengthen compliance and governance by automatically surfacing sensitive data.
Setup: Filter for unclassified columns, use "Generate AI output" with the PII field. Create prompts that analyze column names, sample data patterns, and metadata to detect personal information like names, emails, addresses, or phone numbers.


**5. Enrich documentation** Goal: Keep metadata complete and business-friendly without manual effort.
Setup: Filter for resources that need in-depth documentation, based on your organization's needs. Use "Generate AI output" with the documentation field. Use the out of the box template documentation or create customize prompts that request comprehensive explanations including business context, technical details, and usage guidelines.


Beyond these use cases, teams can also build custom prompts to update Descriptions, Documentation, Owners, Tags, and PII fields across resources as needed. This flexibility ensures the automation adapts to your organization’s unique standards and language while scaling governance and quality efforts across the entire data stack.


## **Test, preview, and control the output**


Automation only works if teams can trust the outcome. That’s why every AI Automation Block includes built-in guardrails to give you full visibility before changes are applied.


- **Preview before applying** Run the block on a sample set of resources to generate AI outputs and confirm the results look right before applying them at scale.
- **Review or auto-apply** Decide whether to manually approve each proposed change or let the automation run on a schedule with bulk apply. You’re in control of when and how updates are published.
- **Stay consistent** Because Automation Blocks execute in the background, once you approve the setup you can rely on them to apply metadata changes consistently every time they run.


## **Built to scale**


The AI Automation Block is designed for repeatable, bulk background workflows that grow with your organization. Once configured, these automations reliably apply metadata updates and governance rules without additional manual effort.


**Run on your schedule:** Set workflows to run daily, weekly, or at custom intervals. For example, auto-generate descriptions for new tables every Friday or tag new resources before your Monday team kickoff.


**Configure multiple fields at once:** Use AI Blocks alongside other automation blocks to update several metadata fields in one go. Teams can populate descriptions, assign owners, apply glossary terms, and tag resources, all within a single workflow.


**Enforce consistency automatically** : Once approved, Automations continue to run in the background, ensuring standards are applied consistently across your catalog.


**Scale documentation without increasing headcount:** By embedding AI into routine workflows, you expand coverage without increasing manual work, freeing data teams to focus on higher-value initiatives.


## **Get started today**


The AI Automation Block keeps your catalog accurate by automating the repetitive tasks that slow data teams down. From filling missing descriptions to tagging new resources, it helps you scale governance without adding extra work.


Start building your first AI Block in Secoda Automations today to streamline metadata cleanup, improve documentation quality, and free up your team for more impactful work.


Learn more about AI Automations[here](https://docs.secoda.co/features/automations/ai-powered-automations) .
