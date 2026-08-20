---
schema_version: "1.0.0"
document_id: "b76abcf461f7a7db133ceea1a8489d96fc7c2785e3bce90815d27e61fe72a809"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/3-challenges-in-ai-antibody-rd-and-how-to-tackle-them"
published_at: "2025-06-25T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:0f9527ed52ac606dc95530472fc0020683bf17479ca46c9048861978c544e55a"
---

# 3 challenges in AI-driven antibody R&D and how to tackle them

Antibody development has long been a slow, iterative grind — start with a candidate, optimize, test, repeat. But in December 2024, advancements with[AI-driven de novo antibody desig](https://arxiv.org/pdf/2412.07763) n signaled a turning point: designing entirely new antibodies from scratch, not just tweaking existing ones.


Antibody therapeutics are already the[fastest-growing drug class](https://pmc.ncbi.nlm.nih.gov/articles/PMC10535987/#:~:text=In%20recent%20years%2C%20antibodies%20have,antibodies%20as%20it%20stands%20today.) . With AI, we could unlock novel structures, accelerate development, and bring life-changing therapies to patients faster. But getting there requires more than smarter models, it demands new workflows, data strategies, and coordination across R&D.


Here are the three biggest challenges standing in the way of AI-powered antibody R&D and how to chip away at them.


## Challenge #1: Data is still the bottleneck


### 1a. Siloed data, inconsistent methods


**The problem:** AI models can’t predict what they can’t see. Data is increasingly digitized, but most biopharma organizations still treat data like private property, stored in isolated research, process development (PD), and clinical teams. Teams have their own data lakes, but few bridges between them.


AI needs continental-scale data integration to work, but today’s R&D organizations operate like independent countries. Without a unified system, critical multi-stage data — such as manufacturability considerations from PD — aren’t fed back into research, leading to little or no learnings from prior efforts.


**The fix:**


-


Unify data models and software platforms across research, PD, CMC (Chemistry, Manufacturing, Controls), and clinical teams to enable seamless data sharing.


-


Standardize experimental methods for key assays, and track the metadata detailing how those measurements were made. Reduce the noise AI models have to work through.


-


Automate data access and interoperability between stages, ensuring insights from clinical and PD feedback into upstream research decisions. Fail fast and design better molecules sooner.


**Example:** Model accuracy for manufacturability assessments such as expression, aggregation, melting temperature, and viscosity are far more effective when late-stage data is fed into early-stage R&D (example:[TAP: Therapeutic Antibody Profiler](https://opig.stats.ox.ac.uk/webapps/sabdab-sabpred/sabpred/tap) ). Predicting manufacturability early in the screening process could save millions of dollars in wasted development costs.


### 1b: Missing negative data and sparse coverage for complex formats


**The problem:** AI models don’t just need lots of data — they need the right balance of data. When the goal is to get one or two lead molecules to move forward in the campaign, scientists tend to record only successful candidates, leaving negative data points either unmeasured or unrecorded. Selective recording results in models with reduced predictive power, and bias. It’s like training a self-driving car on only successful trips. Without crash data, how can it learn what *not* to do?


Furthermore, the training data we have for one type of therapeutic (e.g. monoclonal antibodies) often doesn’t generalize well to nanobodies or complex formats like bispecifics and multi-specific antibodies.


**The fix:**


-


Promote a mindset and create protocol for “every data point counts,” including failures and negative results.


-


Miniaturize and automate assays to reduce the cost of capturing negative data.


-


Standardize experimental conditions and metadata collection across R&D and PD teams, and capture them in consistent data formats.


-


When possible, work with external partnerships (e.g. national labs, academic institutions, and cloud labs) to systematically recreate and measure past candidates.


**Future outlook**


3D-printed organoids are starting to replace animal models for immunogenicity testing – better data, closer to real human biology. With the[recent FDA plan](https://www.fda.gov/news-events/press-announcements/fda-announces-plan-phase-out-animal-testing-requirement-monoclonal-antibodies-and-other-drugs) to phase out animal testing, beginning with antibodies, we’ll see an uptick in organoids.


Federated learning offers a path for AI collaboration without sharing sensitive data, but a major hurdle remains: data heterogeneity. Without standardized formats, models can’t learn effectively across companies. Industry-wide adoption of frameworks like[Allotrope (ASM)](https://www.allotrope.org/asm#:~:text=The%20Allotrope%20Simple%20Model%20(ASM,by%20any%20modern%20software%20system.) and platforms like Benchling is critical to making federated learning truly interoperable and impactful.


One promising effort comes from the[AI Structural Biology Consortium](https://www.statnews.com/2025/03/27/abbvie-johnson-and-johnson-pharma-data-sharing-openfold3-ai-drug-discovery/) and OpenFold3 — an open-source AlphaFold3 alternative: companies including AbbVie and Johnson & Johnson are contributing structural data to train models on drug-relevant interactions, such as small molecule —protein and antibody —antigen structures. It’s a step toward unlocking powerful models without compromising IP.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Challenge #2: AI models need to fit into an automated, end-to-end loop


**The problem** : R&D generates millions of sequences in antibody discovery campaigns, and deciding which leads to advance is still a slow, manual process. Developability models are becoming more common, but running them is bottlenecked by manual handoffs between computational scientists and research teams. It’s an inefficiency akin to emailing an engineer for every Google search.


Additionally, many developability predictions and models are trained on specific antibody types (e.g. IgGs or nanobodies) and have limited applicability to new modalities and scaffolds. Consequently, candidate selection is frequently based on incomplete information, leading to wasted cycles on suboptimal choices and problems at CMC stage. The challenge lies in knowing which predictive models to use and when, a process that is still highly artisanal, relying on human-driven exploratory analysis.


**The fix**


-


Invest in throughput in the build and test steps of your workflow to accelerate AI/ML model development.


-


Track all tiers of predictions and measurements in standard data models.


-


Go beyond bespoke models run by experts, and invest in dashboards, visualization layers, and democratized inference. Work toward:


-


An AI model hub with versioning, training metadata, and performance benchmarks; make it easy to assess model relevancy and drift.


-


Data pipelines that track and refresh model performance over time.


-


Dashboards/visualization layer where scientists can view sequence data, experimental results, and model predictions—side by side.


-


Implement agents where AI not only predicts but also suggests the next round of experiments, reducing manual intervention.


-


Semiautonomous workflows are beginning to emerge as part of a[lab-in–the-loop approach](https://www.biorxiv.org/content/10.1101/2025.02.19.639050v1.full.pdf) , including deep learning, in vitro experimentation, and generative building of new antibody molecules. Get on board so you aren’t left in the dust when it’s common in five years. Simultaneously invest in seamless hand-off to the robots to execute.


**Future outlook:**


Plug-and-play systems for all of computational biology, where scientists can run, mix, and match models with other computational steps, without needing constant support from informatics teams. This means connecting generative models with traditional bioinformatics tools, using both to engineer features, and feeding those into predictive systems that can learn from diverse data sources.


We’re already seeing hints of this: models like viscosity predictors are now used routinely by protein chemists without modification. The next step is making it just as easy to compose entire pipelines — feature engineering, prediction, and design — into flexible, interoperable systems.


AI in biologics R&D will operate at 80% accuracy thresholds ,not perfect, but good enough to guide decision-making and improve over time with more data.


## Challenge #3: AI can’t stop at discovery, it must extend to bioprocess development


**The problem:** Once an antibody is discovered, there is still a long and expensive road to manufacture it. Bioprocess and downstream recovery choices have huge effects on the titer and quality characteristics. AI can help make the iterative refinement of these process parameters faster, saving a lot of time and material costs, in addition to shortening the time to clinic.


Traditional Design of Experiment approaches date back a century and have been a staple of this process, but ML/AI is poised to upend the old process by making better use of experimental data and transferring learnings from historical projects to the specific and current project focus.


Despite all this potential, computational teams in biopharma are disproportionately focused on discovery. A typical research team may have 40 computational scientists compared to just two in PD.


**The fix:**


-


Expand ML/AI applications into bioprocess optimization like:


-


Cell growth optimization: adjusting bioreactor conditions for maximum yield


-


Purification protocol refinement: reducing processing time and improving purity


-


Assay condition tuning: enhancing signal-to-noise ratios in characterization assays


-


"Productize" AI models by not only offering predictions but also explainability. It gives domain scientists a chance to contribute to the modeling process, catch errors in the input data, and understand caveats to its generalizability. It also builds their trust in the computational systems.


-


Integrate AI directly into experimental workflows, so models continuously learn and improve as more data is generated.


-


Have computational teams use or build tooling that spans both Research and PD. Rather than only building bespoke ML systems for discovery, identify the common engines that can drive model fitting, explanations, and recommendations across domains.


**Example:** AI based on Bayesian optimization/active learning can improve purification and formulation, just as it improves candidate selection. Biopharma teams need to bring PD into AI conversations earlier.


**Future outlook:** Hybrid models (combining data and mechanistic understanding) show promise when we understand the first principles of a unit operation to make mathematical models of it, and have the data to specify the parameters that define its behavior.
