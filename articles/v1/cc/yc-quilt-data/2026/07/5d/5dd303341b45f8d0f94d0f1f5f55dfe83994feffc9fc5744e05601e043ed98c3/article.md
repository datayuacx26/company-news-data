---
schema_version: "1.0.0"
document_id: "5dd303341b45f8d0f94d0f1f5f55dfe83994feffc9fc5744e05601e043ed98c3"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/claude-science-quilt-mcp-cell-painting"
published_at: "2026-07-01T18:34:10+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:b35eeeb455e6b60fd25cb6a8339a6c66a24fde4be752f62fa6b82447bce9af6a"
---

# Claude Science Meets the Quilt MCP: From Public Cell Painting Data to 25 Tested Hypotheses

Anthropic just shipped Claude Science, an agentic research workbench for life sciences. The first thing we wanted to know was simple: point it at a real dataset through the Quilt MCP and see how far it gets on its own. It went from a cold start to twenty-five tested hypotheses, six figures, a versioned results package, and a reproducibility bundle with the verified code to regenerate all of it. No human wrote a line of the analysis.


This is a write-up of that first run: what we connected, what we asked for, what came back, and where it tripped. One idea runs through it. A Quilt package is a good unit of context to hand a science agent. You give Claude a self-describing package, tell it what you're looking for, and it gives back results that are versioned and reproducible by construction. Package in, analysis in the middle, package out. The rest of this post is that loop, with the numbers and the setup so you can run it yourself, either against public data on Quilt Open or against your own S3 data through your own Quilt stack.


---


## What Claude Science is, and why data access is the hard part


Claude Science (in beta for macOS and Linux) is not a chatbot that talks about biology. It runs Python and R, keeps a separate compute environment per analysis specialist, searches the literature with traceable citations, and saves provenance on every result. It ships with specialists for genomics, single-cell, proteomics, structural biology, and cheminformatics, can hand work to sub-agents that run in parallel, and connects to outside tools through the Model Context Protocol (MCP). It is the agentic counterpart to[Claude for Life Sciences](https://www.anthropic.com/news/claude-for-life-sciences) and Claude Code.


That design has one consequence worth dwelling on. An agent this capable is only as good as its access to your data and to the context around it. A bucket full of nameless CSVs is a dead end. The agent needs to know what a file is, where it came from, what license governs it, which columns are the labels, and how this dataset relates to the last one. That is the gap a Quilt package fills, and it is why the MCP connector is the part of this story that matters.


*Claude Science working through the first pass inside the app: it pulled the LINCS Cell Painting profiles from Quilt Open, ran the analysis, and wrote a short report that ties the result back to the source package.*


---


## The setup: one MCP connector, two ways to point it


The Quilt MCP server gives any MCP-compatible client (Claude Science, Claude Code, Cursor, ChatGPT) a set of tools for working with Quilt: search packages, browse contents, read objects, run Athena and Iceberg queries, generate presigned URLs, and create new packages. It runs as a hosted endpoint behind[Quilt Connect](https://docs.quilt.bio/quilt-platform-administrator/connect) , so you connect to it by URL with nothing to install or run locally. Same connector, same tools, whichever catalog you point it at.


### Option A: Quilt Open (public data, no infrastructure)


[open.quiltdata.com](https://open.quiltdata.com/) is our public catalog. It indexes large open datasets that live in S3, including the Cell Painting Gallery and CCLE, as Quilt packages with their metadata and provenance attached. A hosted MCP endpoint serves it through Quilt Connect, so anyone can connect by URL. In a client that takes an MCP config, add Quilt as a custom connector pointed at the Quilt Open endpoint:


```text
{
"mcpServers": {
"quilt-open": {
"url": "https://open.quiltdata.com/mcp/platform/mcp"
}
}
}
```


In a client with a connector UI (Claude Science, Claude.ai, ChatGPT, Cursor), add it under Settings, then Connectors, then Add custom connector, and paste the same URL. Nothing to install, nothing to run locally, and public packages are read-only. That is the whole setup for public exploration.


### Option B: your own data, your Quilt stack


The same approach points at your own catalog by swapping the host. Your Quilt stack runs a Connect Server inside your AWS environment; add it as a custom connector using its MCP URL:


```text
{
"mcpServers": {
"quilt": {
"url": "https://<connect-host>/mcp/platform/mcp"
}
}
}
```


Clients authenticate over OAuth, or with a Quilt API key as a bearer token for non-interactive automation such as AWS-side services. Every request runs inside your AWS environment, under the caller's IAM role and bucket permissions, with no data egress. An administrator enables Connect and adds the allowed client hosts. The Claude, Cursor, and ChatGPT setup flows are in the[Platform MCP Server docs](https://docs.quilt.bio/quilt-platform-catalog-user/mcp-server) .


Worth underlining: the analysis below ran against public Quilt Open data, but nothing about it is specific to public data. Repoint the connector at your own stack and the same workflow runs against your proprietary screens, with your governance intact and no data leaving your VPC.


---


## The task: run a first-pass analysis on a dataset you already have


We gave Claude a deliberately open brief and a connected Quilt Open MCP, and let it choose. It settled on the **LINCS Cell Painting dataset (cpg0004)** from the Cell Painting Gallery.


A word on what that data is, because it matters for the findings. Cell Painting is a high-throughput microscopy assay. Cells are treated with a compound, stained with six fluorescent dyes imaged across five channels (DNA, RNA and nucleoli, endoplasmic reticulum, actin and Golgi and plasma membrane, and mitochondria), and then software measures hundreds of morphological features per well: shape, intensity, texture, and spatial distribution across the nucleus, the cytoplasm, and the whole cell. The result is a numeric profile of how a compound changes what cells look like. The premise of the field is that compounds acting through the same biological mechanism push cells toward similar appearances, so morphology becomes a cheap, unbiased readout of pharmacology. This dataset profiles A549 lung-cancer cells exposed for 48 hours. The package Claude found,` lincs-cpg0004/cp_profiles` , carries normalized CellProfiler profiles with the perturbation annotations (compound, mechanism, target, dose, clinical phase) already joined in.


What it did with the first five plates, unprompted:


- Loaded **1,920 wells** (1,800 compound-treated, 120 DMSO controls), 294 shared morphological features, 111 compounds across 91 mechanisms of action.
- Confirmed that treated cells separate from controls in morphology space (mean distance from the DMSO centroid 14.3 versus 7.2).
- Found a monotonic dose response (Spearman ρ = 0.40).
- Showed that compounds sharing a mechanism of action are about three times more morphologically similar than unrelated compounds (cosine 0.46 versus 0.15, p ≈ 3×10⁻⁷). The tightest clusters were biologically coherent: HCV inhibitors, EGFR inhibitors, and serotonin receptor agonists and antagonists.
- Flagged the most active compounds, all known potent cytotoxics: amsacrine, MG-132, bortezomib, volasertib.


*The first-pass figure Claude produced from five plates. (a) Treated cells fan out from the tight DMSO control cluster. (b) Morphological strength rises with dose. (c) Compounds that share a mechanism are more similar than unrelated pairs. (d) The strongest hits are known potent cytotoxics. The figure travels inside the result package.*


None of this required hunting down a separate annotation file. The mechanism labels, targets, doses, and clinical phases were already inside the same versioned package as the morphology data. That is the point, and it comes back later.


---


## Closing the loop: writing results back as a package


We asked it to save the analysis somewhere clean. It used the MCP's` package_create` tool to write a new derived package,` claude-science/lincs-cellpainting-analysis` , to a sandbox bucket (` quilt-sandbox-bucket` ) on Quilt Open, with the figures, the annotated tables, and a README. The metadata points back at the exact source revision (` lincs-cpg0004/cp_profiles@ad9aba7d…` ), the CC0 license, the citation and DOI (Way GP et al., *Cell Systems* 13, 911-923, 2022), and the headline statistics. The result of the analysis is now itself a versioned package with a pointer to where it came from.


---


## Scaling up, then twenty-five hypotheses in parallel


Then we let it stretch. It expanded from 5 plates to **25** (9,600 wells, 381 compounds, 245 mechanisms of action) and the signal held: treatment versus control strength 9.8 versus 5.8, dose ρ = 0.38 with a p-value that underflowed to zero. It built a UMAP of the compound profiles and, after a useful moment of self-correction, highlighted the mechanisms that were actually morphologically coherent rather than just the most frequent ones.


*UMAP of 381 compound consensus profiles across 25 plates. Grey is every compound; colored points are the most morphologically coherent mechanism classes (EGFR, topoisomerase, and tubulin inhibitors, plus several receptor-antagonist families). Compounds with a shared mechanism settle into local neighborhoods rather than scattering.*


The next request went further. We asked for twenty-five testable hypotheses grounded in the recent Cell Painting literature, run start to finish. Claude searched the literature, organized twenty-five falsifiable hypotheses into five themes, and dispatched five analysis sub-agents in parallel, one per theme, each producing a five-panel figure and a structured results table. A reviewer agent checked the numbers against the underlying tool outputs.


*Inside the app: the five sub-agents finish, and Claude gathers their cluster figures to publish as version 2 of the Quilt package.*


The headline was **19 supported, 3 partial, 3 not supported.** A few of the findings, and why a working scientist would care:


- Mechanism is recoverable from morphology. Compounds sharing a mechanism label (cosine 0.34 versus 0.17) or a target gene (0.36 versus 0.15) are more similar than random pairs, and a leave-one-out nearest-neighbor classifier ran about 9 times above shuffled chance across 245 mechanisms. That is the assay's whole premise, measured: you can infer how an unknown compound works from how it makes cells look.
- Most compounds are quiet. Only about 19% of compounds were morphologically active above the control noise band, which leaves roughly 81% of the library dark. That number is a planning input. It tells you how much of a screen will actually produce signal before you spend on it.
- A clean null result. Clinical-development phase did not predict morphological strength. Launched drugs are not phenotypically stronger than preclinical ones. The agent reported the negative result plainly instead of forcing a story, which is the behavior you want from a research partner.
- Reproducibility held. 88% of treatments replicated, and treatment explained about 110 times more variance than plate-of-origin, so batch effects were small relative to biology. It also caught a real plate-edge artifact, the kind of nuisance signal that quietly poisons screens.
- Polypharmacology candidates. Ten compound pairs with disjoint annotations but near-identical morphology surfaced as off-target leads, the kind of result that turns into a follow-up experiment.


*The verdict map for all 25 hypotheses across five themes: 19 supported, 3 partial, 3 not supported. Each row was tested by an independent sub-agent against the same 9,600-well matrix, then checked by a reviewer agent.*


The full run (the 9,600-well matrix, the feature dictionary, all 25 verdicts, and six figures) was written back as a second revision of the derived package.


---


## From results to a verified reproducibility bundle


The third revision is the part we are most pleased with. We asked Claude to make the whole analysis reproducible. Instead of hand-waving at it, the agent went back through its own execution history, pulled out the actual code that produced every artifact (including each of the five sub-agents' cluster analyses, verbatim), made the scripts run standalone, and published them as a third revision of the package. To prove the bundle was self-contained and not a code dump, it re-ran one cluster analysis from scratch in a clean environment and reproduced the figure pixel for pixel.


That gives the package a clean version history, which is its own small lesson in how versioned data is supposed to work:


Revision What it added Entries


` v1 · 93c89254` First-pass analysis, 5 plates 5


` v2 · a709e9fe` 25-hypothesis study, 25 plates 19


` v3 · baed2340` Reproducibility bundle (code plus pinned environment) 38


The bundle is 1,674 lines of extracted code: a fetch script that pulls the raw plates back from Quilt Open with the source revision hash baked in, the seven pipeline steps (matrix build, feature dictionary, compound UMAP, mechanism coherence, figures, results consolidation), the five cluster analyses exactly as the sub-agents ran them, a pinned` environment_cellpaint.yml` and` requirements.txt` (Python 3.11, scikit-learn 1.9.0, umap-learn 0.5.12), and a` REPRODUCE.md` with the run order, a provenance table, and the one real gotcha (a numba and UMAP cache-directory issue) written down. Every script's lineage points back to the same source revision,` lincs-cpg0004/cp_profiles@ad9aba7d…` . You can browse all three revisions in the[catalog](https://open.quiltdata.com/b/quilt-sandbox-bucket/packages/claude-science/lincs-cellpainting-analysis) .


---


## Why the packaging model is what makes this work


Set the biology aside and the lesson is about data infrastructure. Every annotation that made these tests possible (compound identity, mechanism, target gene, dose, clinical phase, plate, well) lived in the same versioned object as the morphology measurements. The agent never had to locate a side-car CSV, guess at a join key, or trust an undocumented file. It read a package, and the metadata was there.


That is what a Quilt package is: an immutable, content-addressed bundle of S3 objects plus structured metadata, identified by a hash that covers every byte inside it. For an autonomous agent that does two things. It makes data self-describing, so the agent can reason about what it is looking at. And it makes every output reproducible, so when the agent writes results back, the path from raw screen to final figure is a chain of hashes anyone can re-trace. By the third revision, the data, the metadata, the analysis, and the code that produced it were all addressable under one hash. It is the same model our customers use for regulated work, where bit-exact provenance is not optional (see[Tessera's 1 PB and 3x faster NGS](https://www.quilt.bio/case-studies/tessera) and[Resilience's audit-trail rollout](https://www.quilt.bio/case-studies/sustainably-scaling-biomanufacturing) ).


---


## Why this matters for the teams we work with


The bottleneck in most labs is no longer model intelligence. It is getting an agent reliable, governed access to the right data with enough context to act on it. That is a data problem, and it is the one we work on.


When data and metadata travel together in a package, the agent stops guessing. It reads the license, the citation, the schema, and the provenance directly, which is much of why Claude Science could go from raw plates to defensible statistics without a person stitching files together. The same holds for your internal screens. A well-described package is the difference between an agent that explores and an agent that stalls.


The governance does not move when you do this. Repoint the connector at your stack and the agent runs under your existing IAM roles and bucket permissions, inside your VPC, with no data egress. An individual scientist gets self-serve analysis while the organization keeps its access controls and audit trail. The caveat below, where Athena was blocked for the public read-only role, is that model working as intended, not a bug.


And the outputs last. Every result is a versioned package with its provenance attached, which is what regulated and quality-controlled environments require anyway. The analysis a scientist runs on a Tuesday is still reproducible after they have left the company, still traceable in an audit, and still usable as the input to the next study. That is the difference between a session that evaporates and one that becomes part of the institutional record. Tessera, Resilience, and Inari already run their S3 data this way; the MCP just lets an agent take part in it.


---


## How this changes who gets to drive a hypothesis


Here is the shift we think is coming, and that this run is an early sign of. Generating twenty-five literature-grounded hypotheses, testing each with the right statistic, producing publication-quality figures, and packaging the whole thing so it reproduces is, today, weeks of coordinated work. It usually takes a scientist who has the question, a bioinformatician who can run the analysis, and a data engineer who can find and wire up the data. The question travels down that chain and the answer travels back up, slowly, and most questions never make the trip because the overhead is not worth it.


Collapse that chain to one scientist and one session and the cost of asking drops sharply. The expensive step is no longer running the analysis. It is deciding what to ask and judging whether the evidence holds. A scientist can pose a question against a dataset in the morning and have twenty-five tested, figure-backed, reproducible verdicts by the afternoon, including the inconvenient null results. Their time goes where their training matters: framing sharp hypotheses and interrogating the answers, not minding a pipeline.


This does not take the scientist out of the loop, and it should not. The agent reported its null result plainly, a reviewer agent checked the numbers, and a person still owns the call about what is real and what is worth a wet-lab follow-up. What changes is how much ground one curious person can cover. When testing an idea gets cheap enough, you test more ideas, including the speculative ones that were never worth a multi-week project. The package-in, package-out loop is what keeps all that extra exploration from turning into a mess: every answer is itself a citable, versioned starting point for the next question.


---


## An honest caveat


It was not all green checks, and that is worth saying. The agent found Tabulator tables (Quilt's package-to-Iceberg query layer) on six buckets and could inspect their schemas, but the public role it was running under did not have Athena permissions, so it could not query them, and it said so plainly rather than papering over it. On your own stack, where you control the role, that path is open. One more note on the reproducibility bundle: the fetch script documents two ways to pull the raw plates (the` quilt3` client against the public AWS Open Data bucket, and the MCP-connector path used inside the session), but only the in-session path actually ran here, so test the client path before you trust those fetch instructions. The analysis itself is also a first pass on a single cell line and batch, not a definitive profiling result. We would rather show the real run, caveats included, than a polished demo.


---


## Try it on your own data


To reproduce the public version, add the Quilt Open connector from the config above to Claude or Cursor and ask it to explore the Cell Painting Gallery. To run the same thing against your own screens, point the connector at your Quilt catalog and let it work behind your existing permissions. The agent, the tools, and the workflow are identical. The only thing that changes is whose data is in the catalog.


The setup docs are[here](https://docs.quilt.bio/quilt-platform-catalog-user/mcp-server) , and the public catalog is at[open.quiltdata.com](https://open.quiltdata.com/) . If you want help wiring this up against your own data,[get in touch](https://www.quilt.bio/contact) .


The takeaway from our first run is the one we opened with. The package is the unit of context going in and the unit of provenance coming out. Specify what you want against a self-describing package, and capture what you get as another. That loop is what turns a one-off agent session into work someone else can build on.
