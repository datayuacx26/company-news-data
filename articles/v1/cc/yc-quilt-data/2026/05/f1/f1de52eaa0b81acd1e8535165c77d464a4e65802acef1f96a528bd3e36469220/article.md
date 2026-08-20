---
schema_version: "1.0.0"
document_id: "f1de52eaa0b81acd1e8535165c77d464a4e65802acef1f96a528bd3e36469220"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/code-data-environment-closing-ml-reproducibility-loop-git-mlflow-quilt"
published_at: "2026-05-13T20:57:56+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:6b3d393a5ae891f76990eec065adaa0c0277c8f5716bc0bdc8da67d0c226d4ff"
---

# Code, Data, Environment: Closing the ML Reproducibility Loop with Git, MLflow, and Quilt

## The reproducibility problem nobody fully solves alone


If you've ever tried to rerun an ML experiment from six months ago and ended up looking at four nested folders named` v1_final` ,` v1_final_REAL` ,` v2_with_fix` , and` v2_with_fix_USE_THIS_ONE` , you already know the problem. Reproducibility in machine learning is a three-legged stool — and most teams confidently nail two of the legs:


1. **Code & parameters** — solved by Git.
2. **Execution environment** — solved by Docker, Conda,` uv` , Nix, or your IaC of choice.
3. **Data** — usually... not solved. Or "solved" by a shared folder and an honor system.


This is well-trodden territory. Both the lakeFS team ([ML Reproducibility Pillars](https://lakefs.io/blog/ml-reproducibility-pillars/) ) and Databricks ([Reproduce Anything](https://databricks.com/blog/2021/04/26/reproduce-anything-machine-learning-meets-data-lakehouse.html) ) have written about it well. MLflow's own documentation acknowledges the gap: its` mlflow.data` module records a *digest and a pointer* to a dataset, but it doesn't *version the bytes* . ([MLflow Dataset Tracking](https://mlflow.org/docs/latest/ml/dataset/) )


So the question isn't *whether* you need data versioning alongside Git and MLflow. It's *which tool* you bolt on, and *how* it integrates with the workflow you already have. We get the same handful of questions in nearly every introductory call with an ML or data-science team:


- *How does this idea of a "package" relate to an ML experiment?*
- *How does it link to Git code and to MLflow runs?*
- *What does the diff between two versions actually show me?*
- *What automations are available?*
- *Why Quilt vs. DVC, DataChain, or LakeFS?*


This post is our answer.


---


## The mental model: a package *is* the experiment unit


A common framing question we hear is: *"So the package is basically an ML experiment, right?"* That's the right intuition.


A Quilt **package** is an immutable, versioned, content-addressed manifest of files in S3 — plus arbitrary structured metadata. It's a logical grouping that points at physical S3 objects (it doesn't duplicate them). A package version is identified by a top-level hash that summarizes everything inside it — every byte of every file, the directory structure, the metadata, the schemas. Change one byte, you get a new hash and a new version.


That makes the package a natural unit of an *experiment* :


- The **raw inputs** (image stacks, sequencing reads, sensor recordings, scraped HTML — whatever your domain) live as one package.
- The **processed dataset** going into training lives as another.
- The **trained model artifacts + evaluation outputs** live as a third.


Each is independently versioned, each is hash-pinned, and each can be cross-referenced from a Git commit, an MLflow run, a Benchling entry, or a Slack message — by URL.


This is the same packaging philosophy Quilt grew out of in computational biology, where regulatory submissions demand bit-exact provenance of every dataset that informed a model decision. (See[Tessera's 1 PB / 3x faster NGS](https://www.quilt.bio/case-studies/tessera) and[Resilience's audit-trail rollout](https://www.quilt.bio/case-studies/sustainably-scaling-biomanufacturing) for two production examples.)


---


## How Quilt links to Git and MLflow — the "three deep links"


This is the question we get most often: *how exactly do packages tie into Git and MLflow?*


The answer in Quilt is mechanical and boring in the best way: **deep links** . Every package version in Quilt has a stable, hash-pinned URL that resolves to the exact state of that data — forever. So the integration is "just URLs," and it works in both directions.


### Direction 1: From your code to the data (Quilt URI in your repo)


In your training code, you reference data not by path but by package + tag/hash:


> ```text
> import quilt3
>
>
> p = quilt3.Package.browse(
> "myteam/curated-training-set",
> top_hash="3a7c...e1",
> registry="s3://myteam-quilt-prod",
> )
> df = p["features/train.parquet"]()
>
>
> ```


That` top_hash` lives in your repo, in your training script. Now the Git commit fully describes the experiment's data, not just its code. (Quilt also supports` latest` / named tags if you want a moving target during development.)


### Direction 2: From MLflow to the data (Quilt URI as an MLflow input)


When you start an MLflow run, log the Quilt deep link as an input dataset and as a tag:


> ```text
> import mlflow
> from mlflow.data.meta_dataset import MetaDataset
> from mlflow.data.http_dataset_source import HTTPDatasetSource
>
>
> quilt_uri = "https://catalog.example.com/b/myteam-quilt-prod/packages/myteam/curated-training-set/tree/3a7c...e1/"
>
>
> with mlflow.start_run():
> mlflow.log_input(MetaDataset(HTTPDatasetSource(quilt_uri), name="curated@3a7c"))
> mlflow.set_tag("quilt.package", "myteam/curated-training-set")
> mlflow.set_tag("quilt.top_hash", "3a7c...e1")
> # ... train, log metrics, log model ...
>
>
> ```


Now every MLflow run links straight back to the exact dataset version in the Quilt Catalog — clickable, browsable, diffable. MLflow's own docs cover the` mlflow.data` patterns ([here](https://mlflow.org/docs/latest/ml/dataset/) ), and they compose cleanly with Quilt URIs because Quilt URIs are stable forever.


### Direction 3: From the data back to the run (MLflow run ID in package metadata)


This is the leg most teams forget. When you *produce* a new package version (e.g., a training output, a model card, an evaluation report), stamp the MLflow run ID into the package metadata:


> ```text
> quilt3.Package() \
> .set_dir(".", "outputs/") \
> .set_meta({
> "mlflow_run_id": run.info.run_id,
> "mlflow_experiment": "my-model-experiment",
> "git_commit": git_sha,
> "model_metric_auc": 0.93,
> }) \
> .push("myteam/model-outputs-v3", registry="s3://myteam-quilt-prod")
>
>
> ```


Now the loop closes: a Quilt package points at the MLflow run that produced it, and the MLflow run points at the Quilt packages it consumed. You can walk the graph from either side.


That's what we mean by the "three-legged stool" being *complete* : Git pins the code, your container pins the environment, MLflow pins the run, and the Quilt hash pins the data — and the three are connected by clickable URLs and queryable metadata, not tribal knowledge.


---


## What` diff` actually shows you between versions


A reasonable question once you have versioned packages: *"What's actually different between version seven and version six?"* In Quilt, the answer is a structured, file-by-file diff, not "well, the folder is 2 GB bigger now."


In the Catalog UI, two package revisions render side-by-side with files added, removed, and modified — including the underlying hash changes — plus a metadata diff (so changes to schema, labels, or experiment parameters in metadata are visible too). You can also drive it programmatically:


> ```text
> old = quilt3.Package.browse("myteam/curated-training-set", top_hash="...v6...")
> new = quilt3.Package.browse("myteam/curated-training-set", top_hash="...v7...")
> # Compare manifests, metadata, schemas, etc.
>
>
> ```


For tabular data inside packages,[Quilt Tabulator](https://docs.quilt.bio/) exposes Parquet/CSV files as queryable tables in Athena, so "diff between v6 and v7" can also mean a SQL diff over row counts, label distributions, or feature stats — which is what you actually care about when validating training data.


---


## Automations: workflows, the Python API, and MCP


The other question that comes up early: *"What automation is available on top of all this?"* Three layers, depending on how much control you want.


**1. Quilt Workflows.** Schema-based gates on package push. Require certain metadata fields, validate file structures, enforce naming conventions, or run JSON-Schema checks before a push is allowed to land. Used as a CI-style policy layer — think "you cannot publish a training-ready dataset without` labeling_protocol_version` and` qc_status: passed` ."


**2.` quilt3` Python library + CLI.** Everything in the UI is also a Python call. This is what plugs into Nextflow ([nf-quilt](https://github.com/quiltdata/nf-quilt) ), Airflow, Prefect, Dagster, and GitHub Actions. The Tessera deployment runs Quilt as the output sink for every Nextflow pipeline run, automatically.


**3. Quilt MCP Server.** This is the newer one. Quilt now exposes its primitives (search packages, browse, diff, create, query Athena) over the[Model Context Protocol](https://modelcontextprotocol.io/) , so Cursor, Claude, ChatGPT, or your in-house agent can interact with packages directly. "Build me a curated dataset from these three packages, run QC, and publish as v8" becomes a sentence.


For life-science teams specifically, the[Benchling integration](https://blog.quilt.bio/quilt-benchling-integration-seamless-data-packaging-for-life-science-labs) auto-creates packages from ELN entries, so the data, the experimental protocol, and the run all stay linked.


---


## Why Quilt vs. DVC, DataChain, and LakeFS


This is the question every team eventually asks, and it deserves an honest answer. The lakeFS team published a thoughtful post on this exact comparison —[Git-Like Data Versioning Meets MLOps: lakeFS with MLflow, DataChain, Neptune & Quilt](https://lakefs.io/blog/git-like-data-versioning-meets-mlops-lakefs-with-mlflow-datachain-neptune-quilt/) . It's worth reading. Their framing — that data versioning is a layer underneath MLOps tools — is correct, and we agree with most of it. Where we differ is on **what the right abstraction is** for ML teams, and especially for teams working with very large, heterogeneous files.


Rather than rehash their post, here's how we'd describe the actual tradeoffs.


### DVC — Git-LFS on steroids


**Best at:** Small-to-mid datasets, tight Git coupling, single-developer or small-team ML projects where the data conveniently fits the Git mental model.


**Where it strains:** When file sizes go up (multi-GB-per-file scientific data, hundreds of TB total), DVC's per-file pointer model and pull-then-train pattern starts to hurt. The community has been candid about this for years — see the[DagsHub comparison](https://dagshub.com/blog/data-version-control-tools/) , and the[r/mlops thread](https://www.reddit.com/r/mlops/comments/1j0w0qk/lakefs_or_dvc/) lakeFS themselves cite, where the consensus is roughly: *"DVC is great for low-scale; for big data, look elsewhere."*


**Vs. Quilt:** DVC versions *files* ; Quilt versions *named, metadata-rich packages* . DVC requires a Git repo to be the source of truth for data state; Quilt does not — the package registry in S3 is the source of truth and the Git repo just references hashes. For teams whose data is bigger than their code, that inversion matters.


### LakeFS — Git-like branches on the object store


**Best at:** Pure data-engineering use cases where you want branch/merge semantics over a whole bucket — think ETL pipelines, parallel feature engineering, "shadow" production data branches for backfills. Their zero-copy branching is genuinely good engineering.


**Where it strains:** LakeFS is an *infrastructure layer* . It's an S3-compatible gateway you put your data behind. That's a meaningful operational commitment — you're now routing reads/writes through lakeFS for everything in that repo. It also doesn't carry the *human-facing* affordances that data scientists ask for: README rendering, schema previews, longitudinal package-level metadata, package-level search, a notion of "this collection of files is the v3 dataset that ships."


**Vs. Quilt:** This is the most interesting comparison. LakeFS's blog post correctly observes that Quilt is "less focused on the process of getting to" a published version. That's true *by design* . We don't think most ML teams want a branch/merge workflow on their data lake — they want **versioned, named, metadata-rich datasets that humans and pipelines both understand** , with diff and rollback when needed. Branch/merge is powerful but heavyweight; the cognitive cost of "is this on` main` or on` feature-update` ?" for a 5-person data-science team is non-trivial, and the failure mode (forgotten branches, abandoned merges) is real. Quilt's model — publish a package, get a hash, deep-link it — fits how ML researchers actually work day-to-day.


The two tools are also not mutually exclusive. You *can* run Quilt over data that lakeFS is managing underneath; the package manifest just points at the (lakeFS-presented) S3 paths. We've seen teams do exactly that. For most teams, though, this is overkill, and you can pick one.


A few specific places we'd push back on the lakeFS post:


- It characterizes Quilt's data versioning as "partial — via S3 object versioning and package snapshots; limited support for branching or rollback." Rollback in Quilt is a one-line call —` quilt3.Package.rollback(name, registry, top_hash)` resets` latest` to any prior version, and` Package.install(... top_hash=...)` fetches the exact files from that version. Every prior hash is permanently addressable. We don't have first-class branches, by design (see above), but rollback is not the limitation.
- It places governance squarely in lakeFS's column. In practice, Quilt's governance story for ML teams is **Workflows + IAM + audit logs + package-level metadata-as-policy** , plus integration with whatever identity provider you already run. For audit-conscious and GxP-aligned workflows, that pattern is in production at companies preparing data for regulatory submission ([Resilience case study](https://www.quilt.bio/case-studies/sustainably-scaling-biomanufacturing) ).
- Garbage collection is real and useful — we'd just note that in Quilt's model, "GC" is usually expressed as **package retention policies** + S3 lifecycle rules, which composes well with existing S3 cost controls.


### DataChain — Pythonic dataset curation


**Best at:** Heavy unstructured-data curation pipelines (images, audio, video, PDFs) where you want to apply ML models and LLMs *as part of the pipeline* and persist the structured outputs in an embedded database. Iterative.ai's tooling is genuinely good here.


**Where it strains:** Versioning is delegated to the storage layer, which means your "data version" story is "whatever S3 object versions plus DataChain's internal database says." For regulated teams or for cross-team data sharing, that's often not enough.


**Vs. Quilt:** Different layer. DataChain is great at *generating* curated datasets; Quilt is great at *publishing, distributing, and pinning* them. We see teams use both: DataChain to run the curation pipeline, Quilt to publish the resulting package with metadata, schemas, and a deep link for MLflow.


### So, when *do* you pick Quilt?


Roughly: pick Quilt when **(a) your data is big and heterogeneous** , **(b) more than one team needs to find, understand, and reuse it** , and **(c) you need provenance you can hand to an auditor, a collaborator, or your future self.** That's the niche we've optimized for since the company's bioinformatics roots, and it's why the deployment pattern at companies like[Tessera](https://www.quilt.bio/case-studies/tessera) ,[Entact Bio](https://www.quilt.bio/case-studies/entact) , and Resilience looks the way it does.


---


## A reference pattern: Git + MLflow + Quilt, end-to-end


For a typical small-to-mid ML team — a handful of data scientists, raw data arriving from instruments or vendor feeds, a regulated or audit-conscious downstream consumer, and tens-to-hundreds of TB of processed data — here's the pattern we'd recommend, and the one we'd run a 60-day POC against:


1. **Ingest.** Raw drops land in an S3 prefix. A scheduled Quilt push creates an immutable` myteam/raw-<source>` package per drop, with metadata extracted from filenames and sidecar JSON. A Quilt Workflow enforces required metadata fields at push time.
2. **Curation.** A Nextflow pipeline (via` nf-quilt` ) or a DataChain job reads` myteam/raw-<source>@<hash>` , applies QC and filtering, and writes a new` myteam/curated-training-set` package. The output package metadata records the input package hash and the pipeline Git commit.
3. **Training.** Training code lives in a Git repo. Each training script references` myteam/curated-training-set@<hash>` . The script starts an MLflow run, logs the Quilt URI as an input dataset, logs` quilt.top_hash` and` git_commit` as tags, trains, logs metrics, and finally publishes outputs as` myteam/model-outputs-v<n>` — with` mlflow_run_id` and` git_commit` in the package metadata.
4. **Review.** In the Catalog, diff` v6` vs.` v7` of` curated-training-set` to see what changed. From any MLflow run, click through to the exact data. From any Quilt package, click through to the MLflow run that produced it.
5. **Promotion.** A "production" tag on Quilt packages is what triggers downstream consumers. Only packages that pass a Workflow gate (e.g., metric thresholds, QC fields) can be tagged.


Three URLs, one hash apiece, one auditable chain.


---


## Closing


MLflow is excellent at tracking *runs* . Git is excellent at tracking *code* . Neither was designed to version your 200 TB of imaging, sequencing, sensor, or document data — and they don't have to be, as long as you bolt on a data layer that speaks the same language.


The lakeFS team's framing of data versioning as a layer underneath MLOps tools is right. We just think the right shape of that layer, for most ML teams, isn't a branchable virtual filesystem — it's a **package** : a hash-pinned, metadata-rich, human-and-machine-readable unit that you can deep-link from anywhere and rollback at will.


If you're an ML or data-science team wrestling with the all-too-familiar story — unversioned copies of files everywhere, experiments you can't replicate, a hot mess you need to clean up before the next phase of work — we'd love to talk. We offer a 60-day, no-cost POC on your own S3 bucket. Reach out at[quilt.bio](https://quilt.bio/) .


---


### Further reading


- [Git-Like Data Versioning Meets MLOps: lakeFS with MLflow, DataChain, Neptune & Quilt](https://lakefs.io/blog/git-like-data-versioning-meets-mlops-lakefs-with-mlflow-datachain-neptune-quilt/) — the lakeFS post this article responds to.
- [ML Reproducibility Pillars: The Holy Trinity](https://lakefs.io/blog/ml-reproducibility-pillars/) — the three-legged stool framing.
- [MLflow Dataset Tracking docs](https://mlflow.org/docs/latest/ml/dataset/) — the` mlflow.data` API.
- [Reproduce Anything: Machine Learning Meets Data Lakehouse](https://databricks.com/blog/2021/04/26/reproduce-anything-machine-learning-meets-data-lakehouse.html) — Databricks' take.
- [DVC vs. Git-LFS vs. Dolt vs. lakeFS: Data Versioning Compared](https://lakefs.io/blog/dvc-vs-git-vs-dolt-vs-lakefs/)
- [Quilt Mental Model](https://docs.quilt.bio/mentalmodel) — what a package is and isn't.
- [nf-quilt](https://github.com/quiltdata/nf-quilt) — Nextflow + Quilt integration.
- [Tessera Therapeutics case study](https://www.quilt.bio/case-studies/tessera) — 1 PB of genomics on Quilt.
- [Resilience: Sustainably Scaling Biomanufacturing](https://www.quilt.bio/case-studies/sustainably-scaling-biomanufacturing) — FDA-aligned data infrastructure.
- [Quilt + Benchling Integration](https://blog.quilt.bio/quilt-benchling-integration-seamless-data-packaging-for-life-science-labs) — auto-packaging from ELN entries.
