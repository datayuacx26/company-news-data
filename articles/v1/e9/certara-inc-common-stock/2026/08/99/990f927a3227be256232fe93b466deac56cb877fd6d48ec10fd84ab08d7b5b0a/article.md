---
schema_version: "1.0.0"
document_id: "990f927a3227be256232fe93b466deac56cb877fd6d48ec10fd84ab08d7b5b0a"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-rss-c7049147c8d2"
canonical_url: "https://www.certara.com/blog/rsnlme-meets-mcp-the-pharmacometric-ai-assistant-scientists-have-been-asking-for/"
published_at: "2026-08-03T16:40:42+00:00"
first_seen_at: "2026-08-03T16:45:01.581586+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:3a2123da9d813c928b1ba18afccfb0acbe3eca7fa297961a521635311ddaf159"
---

# RsNLME meets MCP: the pharmacometric AI assistant scientists have been asking for

ShareShareShare


Arjen Bos Principal Product Manager, CertaraJames Craig, MS Principal Software Engineer, PMx


August 3, 2026


For decades, a steep technical wall has stood between a pharmacometrician’s scientific theory and the software required to run it. Scientists got into the field to push medicine forward, only to find themselves acting as amateur software administrators and programmers. We built this new MCP server to tear that wall down. By loading purpose-built tools and a curated PML knowledge base directly into your coding assistant, we created a system that understands both the science and the software simultaneously. You bring the scientific problem, and the tool handles the translation.


## What we built


The result is a local MCP server, installed as part of the[RsNLME](https://www.certara.com/software/r-speaks-nlme-rsnlme/) package, that gives any MCP compatible AI assistant structured, purpose built tools for the full NLME modeling workflow. It sits between a coding assistant and the same NLME engine that powers Phoenix, and it comes loaded with a curated PML and NLME knowledge base, so recommendations reflect current pharmacometric practice rather than a generic guess at what R code might work.


This isn’t a passive chatbot that simply talks about pharmacometrics. Think of it as a highly trained personal assistant for your modeling workflow, one that executes the work while you remain responsible for the science. Rather than losing time to manual scripting, a modeler can delegate specific workflow steps while retaining review and control:


- Inspect a PK dataset and propose a column mapping
- Build and fit an NLME model, and judge whether it converged cleanly
- Run a VPC and produce a stratified plot by study
- Generate goodness of fit and covariate diagnostic plots
- Translate legacy model code into PML, then fit it


That last one is worth explaining, because it shows how the assistant actually reasons. It builds an abstract syntax tree of the original model code, reasons over our PML knowledge base to reconstruct the equivalent model, and then sends the result back through RsNLME to validate that it compiles and fits before it is ever handed back to the user. Our focus here is on the built-in expertise. The MCP server bridges the gap between software environments automatically, meaning you don’t need to learn how to connect package A and package B manually, the system orchestrates the integration for you


## Proving it against the literature


We did not want to hand this to modelers on faith, so we validated it against real published models first. Working with Mark Sale, a longtime academic collaborator, we compared the MCP driven workflow against a well known population pharmacokinetic dataset with a published reference model. The assistant approached the problem the way an experienced modeler does: structural model first, covariate testing second, decisions made afterward using[likelihood ratio](https://www.certara.com/knowledge-base/what-is-the-2ll-or-the-log-likelihood-ratio/) tests and prediction corrected visual predictive checks, the same criteria a human would apply. The[model it converged](https://www.certara.com/blog/63-runs-and-no-convergence-when-rsnlme-succeeded-where-others-couldnt/) on had a meaningfully better objective function than the original published model, not because it cheated the process, but because it followed the same process a careful modeler follows, consistently and without fatigue.


We also asked it to go further than a single fit. In one demonstration, we gave it a single instruction: perform a full population PK analysis, including exploratory data analysis, base model development, stepwise covariate modeling, a visual predictive check, and goodness of fit plots, then write it up in an R Markdown report following[EMA guidance](https://www.ema.europa.eu/en/reporting-results-population-pharmacokinetic-analyses-scientific-guideline) , and build a QC package a colleague could use to reproduce the whole thing. It did exactly that. This level of traceability is woven into the entire workflow because the assistant explains its reasoning and documents each step in real time. It ran the exploratory analysis first, compared four structural models on minus twice log likelihood and AIC, tested residual error structures, ran a stepwise covariate search, and saved an independent, inspectable R script at every stage. That last detail mattered to us as much as the science did. It showed its work, the way we would want any collaborator to, instead of handing back a black box result.


## Keeping the Modeler in the Driver’s Seat


No tool gets to decide whether a model is scientifically sound; that judgment stays with the modeler. The assistant proposes, validates, and executes. The modeler still decides what a good model is, what a defensible covariate relationship looks like, and when a numerically better fit is not actually a scientifically meaningful one.


What it does change is where a[modeler’s time goes](https://www.certara.com/blog/model-smarter-not-harder-how-phoenix-cloud-puts-the-science-back-at-the-center-of-modeling/) . Someone who knows exactly what they want can give the assistant precise, step by step instructions and it will execute them exactly, in R, using RsNLME, backed by the NLME engine. Someone less certain can describe the objective in plain language, closer to how they would brief a colleague, and let the assistant plan the workflow, confirm the plan before committing to anything, and explain its reasoning whenever asked why it made a particular decision.


One of our colleagues described it well after watching it run for the first time: it behaves like a senior pharmacometrician sitting next to you, one who happens to also know R, RsNLME, and[PML](https://onlinehelp.certara.com/phoenix/8.3/topics/intropml.htm) fluently, and who never gets tired of writing the twelfth version of a goodness of fit script.


## Bridging the Gap Between Modeling and Code


That is the version of this tool we set out to build, not because we do the science ourselves, but because we have spent our careers close enough to the people who do to know what was missing.


RsNLME’s new MCP server runs on the same engine behind[Phoenix NLME](https://www.certara.com/software/phoenix-nlme/) , and it works today with Claude, Cursor, Codex, and any other MCP compatible coding assistant, right where pharmacometricians already do local, script based work. We built what pharmacometricians have always wanted: a tool that understands both the science and the software at once, so they no longer have to translate between the two themselves.


On demand webinar


## Model Smarter, Not Harder: Introducing Model Designer


If you work in R and NLME today, the RsNLME MCP server is designed to bring this seamless integration straight to your workflow.


[Learn more in our webinar](https://www.certara.com/on-demand-webinar/model-smarter-not-harder-introducing-model-designer/)


"We built what pharmacometricians have always wanted: a tool that understands both the science and the software, so they no longer have to translate between the two themselves."


## Scientific references


Li X, Sale M, Nieforth K, Bigos KL, Craig J, Wang F, Feng K, Hu M, Bies R, Zhao L. pyDarwin: A Machine Learning Enhanced Automated Nonlinear Mixed-Effect Model Selection Toolbox. Clin Pharmacol Ther. 2024;115(4):758-773.


Anthropic. Introducing the Model Context Protocol. Published November 2024.


Pharmacometrics in the Age of Large Language Models: A Vision of the Future. Pharmaceutics. 2025;17(10):1274.


## FAQs


### What is an MCP server?


MCP stands for Model Context Protocol, an open standard that enables developers to build secure, two-way connections between their data sources and AI-powered tools, introduced by Anthropic in November 2024. It’s often compared to USB for AI integrations, before a standard like this existed, connecting an AI assistant to each new tool or dataset required a custom, one-off integration. An MCP server exposes a specific set of tools and knowledge to any MCP-compatible AI assistant in a standardized way, so the assistant can use them directly instead of guessing how to interact with the underlying software.


### What are the use cases for an MCP server in pharmacometrics?


In the context of RsNLME, the MCP server gives an AI coding assistant structured, purpose-built tools for real modeling tasks: inspecting a PK dataset and proposing a column mapping, building and fitting an NLME model and judging convergence, running a VPC and producing stratified plots, generating goodness-of-fit and covariate diagnostics, and translating legacy model code into PML before fitting it. Because it’s loaded with a curated PML and NLME knowledge base, its recommendations reflect current pharmacometric practice rather than a generic guess at what R code might work.


### Are there AI components in Phoenix NLME?


The new RsNLME MCP server runs on the same NLME engine that powers Phoenix, connecting it to AI coding assistants like Claude, Cursor, and Codex. Rather than changing the underlying science engine itself, it adds an AI layer on top that handles the manual scripting, translation, and diagnostic work that used to consume a modeler’s time — while validated model runs are checked against literature benchmarks and standard model-building criteria (structural model first, covariate testing second, likelihood ratio tests, prediction-corrected VPCs) before being handed back.


### What is PML, and how can I learn it?


PML (Pharmacometric Modeling Language, sometimes called Phoenix Modeling Language) is the scripting language used inside Phoenix and RsNLME to build custom PK and PK/PD models that go beyond what the graphical interface supports. Although most models can be built using the graphical user interface in Phoenix, some models require custom coding with PML. To learn it, Certara offers[PML School](https://www.certara.com/training/pml-school/) , a free, interactive series of hands-on educational webinars that guide you through building complex, custom PK and PK/PD models, as well as a paid, self-paced Certara University course, Getting Started Learning PML Code, aimed specifically at RsNLME users.


## Authors


[Arjen Bos](https://www.certara.com/teams/arjen-bos/) Principal Product Manager, Certara


Arjen is a product manager with more than 20 years of experience in the life sciences industry. ​He joined Certara two years ago and is involved with a variety of Phoenix Cloud modules.


[James Craig, MS](https://www.certara.com/teams/james-craig-ms/) Principal Software Engineer, PMx


James is a Staff Software Engineer with 14 years of experience developing scientific and pharmacometric software in highly collaborative, cross-functional environments. He specializes in statistical computing, modeling workflow systems, and scientific application development, with deep expertise in R, Shiny, Python, and domain-specific languages for Non-Linear Mixed-Effects Modeling, including PML for Phoenix NLME and NMTRAN for NONMEM. James also has strong experience in enterprise and cloud-native web application development, including Java, JavaScript/TypeScript, React, database systems, APIs, and containerized execution environments. His work bridges pharmacometrics, software engineering, applied statistics, and machine learning, and he has coauthored seven scientific publications in leading journals.


## Go from zero to model


Model Designer is included as part of Phoenix Cloud for those users that have NLME as part of their package. Trial licenses are available for users of Phoenix NLME (desktop version) and/or Pirana. An academic license is provided at no charge for students and faculty at academic institutions.


Not a user? Let us show you firsthand the combined power of Model Designer and these proven pharmacometrics solutions. Submit this short form to request a demo.


### Send an Inquiry


## You May Also Like


AllPharmacometrics


[How Certara Enabled Fruquintinib FDA Approval for Metastatic Colorectal Cancer](https://www.certara.com/case-study/expert-guidance-streamlined-the-timely-fda-approval-of-fruquintinib/)


[How Certara Enabled Fruquintinib FDA Approval for Metastatic Colorectal Cancer](https://www.certara.com/case-study/expert-guidance-streamlined-the-timely-fda-approval-of-fruquintinib/)[Case Study](https://www.certara.com/category/case-study/)


### [How Certara Enabled Fruquintinib FDA Approval for Metastatic Colorectal Cancer](https://www.certara.com/case-study/expert-guidance-streamlined-the-timely-fda-approval-of-fruquintinib/)


[ICH M15 guideline: What the finalized guideline means for model-informed drug development](https://www.certara.com/blog/redefining-regulatory-strategy-with-midd-insights-from-the-draft-ich-m15-guidance/)


[ICH M15 guideline: What the finalized guideline means for model-informed drug development](https://www.certara.com/blog/redefining-regulatory-strategy-with-midd-insights-from-the-draft-ich-m15-guidance/)[Blog](https://www.certara.com/category/blog/)


### [ICH M15 guideline: What the finalized guideline means for model-informed drug development](https://www.certara.com/blog/redefining-regulatory-strategy-with-midd-insights-from-the-draft-ich-m15-guidance/)


[Guided by Evidence: Implementing the ICH M15 Guideline for Model-Informed Drug Development (MIDD)](https://www.certara.com/guide/guided-by-evidence-implementing-the-ich-m15-guideline-for-model-informed-drug-development-midd/)


[Guided by Evidence: Implementing the ICH M15 Guideline for Model-Informed Drug Development (MIDD)](https://www.certara.com/guide/guided-by-evidence-implementing-the-ich-m15-guideline-for-model-informed-drug-development-midd/)[Guide](https://www.certara.com/category/guide/)


### [Guided by Evidence: Implementing the ICH M15 Guideline for Model-Informed Drug Development (MIDD)](https://www.certara.com/guide/guided-by-evidence-implementing-the-ich-m15-guideline-for-model-informed-drug-development-midd/)
