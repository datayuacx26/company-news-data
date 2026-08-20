---
schema_version: "1.0.0"
document_id: "e03ae6cb914d50024ee71033ce638d43464152733ab4c956b21bb7fb9594207a"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-rss-c7049147c8d2"
canonical_url: "https://www.certara.com/blog/model-smarter-not-harder-how-phoenix-cloud-puts-the-science-back-at-the-center-of-modeling/"
published_at: "2026-07-16T22:16:37+00:00"
first_seen_at: "2026-07-20T23:17:28.402331+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:c4742d610e640a6aafa1973c44efc07e8e47993942953a8cdb51f2d53b823226"
---

# Model smarter, not harder: how Phoenix Cloud puts the science back at the center of modeling

ShareShareShare


Arjen Bos Principal Product Manager, CertaraJames Craig, MS Principal Software Engineer, PMx


July 16, 2026


Pharmacometric modeling has never demanded more, yet the software many teams rely on was built decades ago and often gets in the way of the science. Certara’s new Phoenix Cloud for modeling aims to change that, starting with two components: Model Designer and the NLME MCP Server. This post looks at what they do, why they matter, and where the platform is heading.


## The tools were supposed to help. Somewhere along the way, they became the obstacle.


Every pharmacometrician recognizes the feeling. You entered the field to do science, to solve hard problems, to move medicine forward. Instead, a growing share of the day goes to managing the software that is supposed to enable that work. The core toolsets many teams depend on were built decades ago. They are powerful, but they are also complex, disconnected, and unforgiving. When gaps open between one tool and the next, modelers close them the only way they can, by writing code: Python scripts, R pipelines, custom integrations, one off bridges from tool A to tool B. Those bridges work until they break, or until the person who built them moves on and takes the institutional knowledge with them.


The friction compounds. When every scientist runs a bespoke environment, validation becomes an almost impossible task, and coding turns into a job requirement when it should be an optional skill. Meanwhile the science does not stand still: datasets keep growing, molecules and nonlinear mixed effects models keep getting more complex, and expectations keep rising. Collaboration adds its own overhead, since handing work to a CRO or asking a colleague at another site to pick up where you left off can mean packaging an entire tool environment, licenses and all, and hoping the setups match well enough to trust the result. That is not collaboration. It is overhead, and the gap between what the science demands and what the tools deliver keeps widening.


This is the reality Certara set out to change with the launch of Phoenix Cloud for modeling, and its first two components, Model Designer and the NLME MCP Server.


On demand webinar


## See Model Designer in action


Watch the on demand webinar for the full demo.


[Watch now](https://www.certara.com/on-demand-webinar/model-smarter-not-harder-introducing-model-designer/)


## A different picture: tools that get out of the way


The premise is straightforward. What if, instead of fighting your tool, you could bring your scientific problem and let the tool guide you through it? That means an interface that is intuitive by design and focused on the science rather than the software, with AI assistance layered on top that understands both the modeling task and the underlying tool. The learning curve shrinks, and the science moves to the front. Both newcomers and experienced modelers benefit: beginners get a guided path, and experts get power without the setup tax.


Certara’s argument is that the cloud is central to solving two very specific problems named above, setup and collaboration. A cloud platform removes local installation, configuration, and version drift between users, so everyone works in the same validated environment. Sharing data and working together become native to how the tool behaves rather than something bolted on afterward. The cloud also lifts a ceiling that most teams hit sooner or later: compute. Pharmacometric problems can be extraordinarily demanding as datasets grow and study designs get more intricate, yet most teams are limited by whatever hardware sits on a desk or in an organizational grid. Elastic cloud compute makes that constraint optional, available when it is needed without the cost of carrying full time capacity.


## Model Designer: pharmacometric modeling software in the browser


Model Designer is the first component of the platform, built from the ground up for pharmacometricians rather than software engineers. There is no installation and no barrier to entry; you open a browser and you are working. The experience combines a clean interface, built in pharmacometrics guidance at every step, and a context aware AI assistant that helps you get where you are going, whether you are new to modeling or writing PML from scratch.


In practice, the tool meets users through guided wizards for pharmacokinetic, pharmacodynamic, or combined PK/PD models. Each wizard walks through the modeling decisions one at a time, framed in familiar terms. On the PK side, the choices map naturally to the ADME sequence: how the drug is administered and absorbed, how many compartments describe distribution, whether elimination is linear or saturable (Michaelis-Menten), and whether to model excretion explicitly. On the PD side, the wizard asks whether the drug acts through a direct or indirect response, which indirect response mechanism applies, whether to include a sigmoid Hill coefficient, and whether an effect compartment is needed to capture a delay between concentration and effect. Small curve previews accompany each choice, and the list of remaining steps updates dynamically as selections change.


The model detail page is where the selections assemble into a complete, working model across three connected panes: a live structural view where every wizard decision becomes an editable control, a central plot that updates in near real time as parameters, error models, random effects, or dosing change, and the underlying PML, the Pharmacometric Modeling Language, fully annotated so users can learn to read and write the syntax as they go. Every structural choice, parameter value, and dosing option is captured in the URL, so sharing a model is as simple as copying a link; a colleague opens the exact same configuration with no export step. For users who prefer to search rather than configure, a model library offers a catalog spanning PK, PD, and PK/PD, filterable and searchable by synonyms familiar to NONMEM users. Alongside configurable templates, it includes author defined models built by domain experts, often taken directly from the literature or tailored to a specific drug, disease, or therapeutic area, with references back to the original sources. Model Designer builds on Phoenix, the PK/PD platform many pharmacometricians already use.


## The NLME MCP Server: modeling inside your AI coding environment


The second component extends the same philosophy into the agentic coding tools many scientists already use. The NLME MCP Server is a Model Context Protocol connector that sits between an AI coding environment, such as Cursor, OpenAI Codex, or Claude Code, and Certara’s trusted modeling tools, connecting the Phoenix NLME engine and the RsNLME packages to the assistant. Crucially, these are purpose built tools for executing NLME fits, running visual predictive checks, comparing jobs, and validating PML, not a generic assistant guessing its way through R. In a demonstration, an agent took a Warfarin dataset and performed a full population PK analysis end to end: exploratory data analysis, comparison of four structural models, residual error model selection, a stepwise covariate search that identified weight on volume, final model fitting, goodness of fit plots, and both binned and binless VPCs. It then generated a reproducible set of R scripts, a QC package with pass or fail checks, and a rendered report following regulatory guidance, all from a single dataset.


This reflects a broader movement in the field. According to PubMed, a 2025 review by Tosca and colleagues concluded that large language models are unlikely to replace mechanistic pharmacometric models but hold strong potential as assistive tools across the workflow, provided they are paired with domain specific tooling and rigorous validation (DOI). A companion perspective by Androulakis and colleagues argued that such approaches can lower barriers to entry and democratize modeling for researchers without deep coding expertise (DOI). A purpose built connector that keeps trusted engines behind the assistant, and produces reproducible, inspectable scripts, is a concrete step in exactly that direction.


## A proven foundation, and a phased roadmap


None of this would work without a solid foundation. Model Designer and the NLME MCP Server sit on top of tools many teams already know: Phoenix NLME, described as the fastest, best converging NLME engine on the market, whose new ADPO fast optimization option can cut run times by 20 to 50 percent in some cases without sacrificing accuracy; Certara RsNLME, which brings the same engine into the R environment with libraries for exploratory analysis, execution, and output generation; and Pirana with Darwin, the pharmacometrician’s workbench paired with machine learning driven model search. The engines are proven; the new tools make them accessible.


The value of getting more modelers to that starting line is well established. According to PubMed, an industry and regulatory survey by Marshall and colleagues documented broad and growing adoption of model informed drug development, along with the expectation of wider future impact (DOI), while a regulatory review by Pan and colleagues detailed how these approaches now support dose selection, trial design, and regulatory decisions in populations that are otherwise difficult to study (DOI). A systematic comparative review by Mao and colleagues similarly found that AI based methods can strengthen model informed decision-making across development stages, while noting the need for standardized evaluation and clearer regulatory guidance (DOI). Lowering the barrier to competent modeling is not a convenience; it is a way to extend the reach of a discipline that already supports the majority of novel drug approvals.


Certara frames the platform in three phases. Now delivers the smart front end: Model Designer and the NLME MCP Server. Next adds scale, with elastic cloud compute for intensive jobs and a cloud Model Workbench for managing, comparing, and organizing nonlinear mixed effects models within a single project. Later brings intelligence, with cloud based Model Discovery powered by Darwin and a Simulation Studio that connects models directly to trial design decisions. Both current tools are targeted for general availability in Q3 2026, with early access to Model Designer open now, and both are free for existing Phoenix Cloud NLME and RsNLME users, with academic programs available and no separate installation required for the MCP server.


## Bringing the science back to the front


For too long, fragmented workflows, steep learning curves, and infrastructure overhead have turned pharmacometricians into administrators and programmers instead of scientists. Phoenix Cloud for modeling is an attempt to reverse that: to make coding an optional skill rather than a prerequisite, to make collaboration and reproducibility native, and to keep scientists in the driver’s seat while the tools recede into the background. Model Designer and the NLME MCP Server are the opening moves, and they are available sooner than many expect.


> “Coding should be an optional skill, not a job requirement. When the tools get out of the way, the science moves to the front.”


On demand webinar


## See Model Designer in action


Want the full picture? Watch the on demand webinar to see Model Designer and the NLME MCP Server demonstrated live, from building a model in the browser through a complete AI assisted population PK analysis. See every step from a raw dataset to a final, reproducible report.


[Watch the webinar on demand](https://www.certara.com/on-demand-webinar/model-smarter-not-harder-introducing-model-designer/)


## FAQs


### What is Certara Model Designer?


Model Designer is a cloud based pharmacometric modeling tool that runs in the browser with no installation. It lets scientists browse a curated model library, configure PK, PD, and PK/PD models through guided wizards, author PML directly, and simulate and visualize model behavior in real time, with AI guidance at every step.


### Do I need to know how to code to use Model Designer?


No. Model Designer is built for pharmacometricians rather than software engineers. Guided wizards frame each modeling decision in familiar terms, while the annotated PML and context aware AI assistant help you learn the syntax as you go. Experienced users can still drop straight into PML authoring for full control.


### What is the NLME MCP Server, and which tools does it work with?


The NLME MCP Server is a Model Context Protocol connector that links AI coding environments such as Cursor, OpenAI Codex, and Claude Code to the Phoenix NLME engine and the RsNLME packages. It provides purpose built tools for tasks like NLME fits, visual predictive checks, and PML validation, and it produces reproducible R scripts you can keep, share, and rerun.


### How does Model Designer fit into the wider Phoenix Cloud roadmap?


Model Designer and the NLME MCP Server are the now phase, the smart front end on top of the NLME engine and RsNLME. The next phase adds elastic cloud compute and a cloud Model Workbench, and a later phase adds Model Discovery powered by Darwin and a Simulation Studio for trial design, all designed to connect into one workflow.


## Authors


[Arjen Bos](https://www.certara.com/teams/arjen-bos/) Principal Product Manager, Certara


Arjen is a product manager with more than 20 years of experience in the life sciences industry. ​He joined Certara two years ago and is involved with a variety of Phoenix Cloud modules.


[James Craig, MS](https://www.certara.com/teams/james-craig-ms/) Principal Software Engineer, PMx


James is a Staff Software Engineer with 14 years of experience developing scientific and pharmacometric software in highly collaborative, cross-functional environments. He specializes in statistical computing, modeling workflow systems, and scientific application development, with deep expertise in R, Shiny, Python, and domain-specific languages for Non-Linear Mixed-Effects Modeling, including PML for Phoenix NLME and NMTRAN for NONMEM. James also has strong experience in enterprise and cloud-native web application development, including Java, JavaScript/TypeScript, React, database systems, APIs, and containerized execution environments. His work bridges pharmacometrics, software engineering, applied statistics, and machine learning, and he has coauthored seven scientific publications in leading journals.


## Explore the engine behind Model Designer


Model Designer and the NLME MCP Server are built on Phoenix, the fastest, best converging NLME engine on the market. See how Phoenix supports PK, PD, popPK, and toxicokinetic analysis across your programs.


[Explore Phoenix](https://www.certara.com/software/phoenix-pkpd/)[Contact the Certara team](https://www.certara.com/software/phoenix-pkpd/phoenix-cloud/#Contact-us-CTA)


## References


1. Tosca EM, Aiello L, De Carlo A, Magni P. Pharmacometrics in the age of large language models: a vision of the future. Pharmaceutics. 2025;17(10):1274.[https://doi.org/10.3390/pharmaceutics17101274](https://doi.org/10.3390/pharmaceutics17101274)


2. Androulakis IP, Cucurull-Sanchez L, Kondic A, et al. The dawn of a new era: can machine learning and large language models reshape QSP modeling? J Pharmacokinet Pharmacodyn. 2025;52(4):36.[https://doi.org/10.1007/s10928-025-09984-5](https://doi.org/10.1007/s10928-025-09984-5)


3. Marshall S, Madabushi R, Manolis E, et al. Model informed drug discovery and development: current industry good practice and regulatory expectations and future perspectives. CPT Pharmacometrics Syst Pharmacol. 2019;8(2):87-96.[https://doi.org/10.1002/psp4.12372](https://doi.org/10.1002/psp4.12372)


4. Pan X, Wang L, Liu J, et al. Model informed approaches to support drug development for patients with obesity: a regulatory perspective. J Clin Pharmacol. 2023;63 Suppl 2:S65-S77.[https://doi.org/10.1002/jcph.2349](https://doi.org/10.1002/jcph.2349)


5. Mao B, Gao Y, Xu C, Macha S, Shao S, Ahamadi M. Evaluating the impact of AI based model informed drug development (MIDD): a comparative review. AAPS J. 2025;27(4):102.[https://doi.org/10.1208/s12248-025-01075-0](https://doi.org/10.1208/s12248-025-01075-0)


## You May Also Like


AllCertara Products - Phoenix


[Phoenix Pharmacometrics Day – Korea​](https://www.certara.com/live-events/pmx-day-korea-2026/)


[Phoenix Pharmacometrics Day – Korea​](https://www.certara.com/live-events/pmx-day-korea-2026/)[Live Events](https://www.certara.com/category/live-events/)


### [Phoenix Pharmacometrics Day – Korea​](https://www.certara.com/live-events/pmx-day-korea-2026/)


[Phoenix Roadshow 2026 – Tokyo​](https://www.certara.com/live-events/phoenix-roadshow-tokyo-2026/)


[Phoenix Roadshow 2026 – Tokyo​](https://www.certara.com/live-events/phoenix-roadshow-tokyo-2026/)[Live Events](https://www.certara.com/category/live-events/)


### [Phoenix Roadshow 2026 – Tokyo​](https://www.certara.com/live-events/phoenix-roadshow-tokyo-2026/)


[5 Key Principles of Modern PK/PD in 2026](https://www.certara.com/guide/5-key-principles-of-modern-pk-pd-in-2026/)


[5 Key Principles of Modern PK/PD in 2026](https://www.certara.com/guide/5-key-principles-of-modern-pk-pd-in-2026/)[Guide](https://www.certara.com/category/guide/)


### [5 Key Principles of Modern PK/PD in 2026](https://www.certara.com/guide/5-key-principles-of-modern-pk-pd-in-2026/)
