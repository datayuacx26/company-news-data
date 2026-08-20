---
schema_version: "1.0.0"
document_id: "22918ecbd7df82cae7b210a1fe613182d4dab1415248a3488d8060fff906f96b"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/introducing-extend-templates-ready-made-document-processing-recipes-for-agents"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T21:18:00.165090+00:00"
fetched_at: "2026-08-06T21:18:00.715803+00:00"
content_hash: "sha256:2fc4a0f49824ceb726b9accbbede845d0d146727bbf50c10404c86a8b9bea92a"
---

# Introducing Extend Templates: ready-made document processing recipes for agents

**TLDR:** Today, we’re launching[Extend Templates](https://www.extend.ai/templates) , a library of ready-made recipes for agents to build production-ready document pipelines. Each template recipe gives you a starting point grounded in best practice configurations so you can choose the exact one for your business needs, give your agents context, adapt as needed, and keep control of every artifact created.


If your document is missing from the library,[submit a request](https://www.extend.ai/templates) .


---


You know what data your business needs from documents and what should happen once that data reaches your system. But building a production-ready pipeline that turns unstructured documents into that data is a separate technical problem.


Extend Templates solves the cold-start problem by giving your agent the document-specific context and instructions to provision a working pipeline. With processor settings, schema, sample, and expected output already in place, you can adapt the workflow to your requirements and move from idea to production faster.


## Document-specific recipes for agents


Being user-first means being agent-first to match how users prefer to build. To that end, Templates provides agents the artifacts they need to be immediately useful for a user’s document processing goal: a sample document, pipeline, processor settings, schema, expected output, and setup instructions. You can inspect and edit those artifacts, adapt them to your documents, and manage the resulting workflows as code.


Consider a[bill of lading](https://www.extend.ai/templates/bill-of-lading) . You know the shipment details your system needs to automate freight intake, tracking, and reconciliation. Your agent still needs a pipeline, processor settings, a schema, a sample, expected output, and instructions. This is a document processing cold-start problem.


To solve it, each template delivers:


- A real sample document and live pipeline
- Processor settings and a document-specific schema
- Expected markdown and structured output
- A[provisioning script](https://www.extend.ai/templates/QVRCE/provision.ts) that can safely run more than once
- A portable[workflow.json](https://www.extend.ai/templates/QVRCE/workflow.json)
- A[skill.md](https://www.extend.ai/templates/QVRCE/skill.md) that explains the document type and configuration


Select “Send to OpenAI Codex” or “Send to Claude Code” to generate a prompt that you can paste directly into that agent. The handoff includes the provisioning script, skill, and workflow definition. Other coding agents can use the same artifacts.


This matters because an agent does not need to guess at or infer the schemas, processor settings, or definition of correct output for your unstructured documents. Templates make those decisions explicit. The agent can then provision the pipeline in your Extend account and adapt it to your business requirements.


## Inspect, change, and own the workflow


For production workflows, agent-first works best if the user stays in control.


After the agent provisions the workflow, the pipeline, settings, schema, code, and output remain visible. You can then test the workflow with a representative document, add or remove fields, adjust processor settings, and compare the result with the expected output.


For example, your transportation management system may require a purchase order number. In this case, you can add that field to the Bill of Lading schema, remove fields that your system does not use, and define how PO number items appear in the final JSON.


The workflow artifacts are available as code. Fork the recipe on GitHub, review each change, and version the workflow with the rest of your application. The user and the agent work from the same files.


Ready-made means ready to adapt. The recipe supplies the initial document-processing decisions. Your documents and business requirements determine the final workflow.


## Start with a recipe or submit your document


Open the[Extend Templates library](https://www.extend.ai/templates) and search for the document you need to process.


If the library has a matching recipe:


1. Inspect the sample and expected output.
2. Give the recipe to your coding agent.
3. Provision the workflow in your Extend account.
4. Test it with your own documents.


If the library does not cover your document type, submit your document to request a generated template. You can inspect, adapt, and give that template to your agent through the same workflow.


[Browse Extend Templates](https://www.extend.ai/templates) , choose the document job you need, and give your agent a real starting point.
