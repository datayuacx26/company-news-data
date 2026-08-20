---
schema_version: "1.0.0"
document_id: "d42cba0fddcb1b4075c2ea9ef2bdd226e8032668f3fbb3a306dc9e3c958df2d4"
company_key: "yc-lamin"
company: "Lamin"
source_id: "yc-lamin-news-import-1018c6ad32c5"
canonical_url: "https://blog.lamin.ai/intro-to-laminr"
published_at: null
first_seen_at: "2026-07-24T09:27:52.013300+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:481ecfdae9d26fbe80d407bfe39491c99eaae83633df86a91a3058c14cdbfbf6"
---

# An introduction to LaminR

- [LinkedIn](https://www.linkedin.com/posts/tylerjburns_hi-friends-i-wrote-a-guest-post-for-lamin-share-7445459710889336832-bNxF/) ·[Tweet](https://x.com/tjburns08/status/2039700022522974646) ·[BlueSky](https://bsky.app/profile/lazappi.bsky.social/post/3miv5ydk7m22l)
- ⸻ 2026-04-02


# An introduction to LaminR¶


- [Tyler Burns](https://github.com/tjburns08)


Any data scientist will tell you that a key to a successful project is strong data management. If your data are disorganized, you don’t know who did what, or you can’t reproduce results, it will come back to bite you and your team. Thus, teams should carefully plan how they handle, store, modify, and track data throughout a project. Here, we’ll be using an example from single-cell analysis to illustrate how the open-source LaminR package helps with traceability and reproducibility of data analyses in R.


The devil is in the details, in particular when working with many complex datasets. You might have multiple data scientists and agents manipulating the data in multiple ways. Did we do a log1p transform or an asinh transform? Did we center and scale the data? I see some clusters. How did we cluster? Did we use the default parameters or change something? And so forth. Furthermore, revisiting older projects can be a nightmare when the original authors have moved on. Yet, these projects can be treasure troves that provide crucial context and training data for AI agents.


It doesn’t matter how good your AI foundation model is if your datasets and infrastructure are problematic. It boils down to the classic adage: ‘garbage in, garbage out.’ So how do we handle all of this, aside from hiring a team of data engineers? This is where LaminR helps. LaminR is an open-source package designed to solve the data infrastructure challenges that naturally arise when large teams and AI agents collaborate on massive datasets. In particular, LaminR manages metadata to allow querying and finding data, and it tracks every line of code that modified a data object, including who made the change and when. So if a data scientist has to revisit an old dataset or one they did not work on, they’ll have the information they need.


## PBMC 3k¶


To illustrate this, we use the well-known PBMC 3k dataset. This dataset has been featured in Seurat’s[guided clustering tutorial](https://satijalab.org/seurat/articles/pbmc3k_tutorial.html) for a decade, and is still the common entry point for single-cell RNA sequencing analysis.


If you have access to a hosted LaminDB instance on[lamin.ai](https://lamin.ai/) , you can log in and connect to it:


CLI


```text
lamin    login    username
lamin    connect    instance_owner/your_instance


```


R


```text
lc     <-     laminr  ::  import_module  (  "lamin_cli"  )
lc  $  login  (  user     =     "username"  )
lc  $  connect  (  "instance_owner/your_instance"  )


```


If you want to initialize your own database instead, use:


CLI


```text
lamin    init    --storage    ./mydata    --modules    bionty


```


R


```text
lc     <-     laminr  ::  import_module  (  "lamin_cli"  )
lc  $  init  (  storage     =     "./mydata"  ,     modules     =     "bionty"  )


```


LaminR is based on the Python package LaminDB and` reticulate


` . To set things up in your script or Rmd notebook, run the following:


```text
library  (  laminr  )
ln     <-     laminr  ::  import_module  (  "lamindb"  )      # `ln` is the central object, equivalent to Python's `lamindb`


```


Importantly, you typically want LaminR to track what you do, so all datasets will be linked to the generating code:


```text
ln  $  track  ()      # start a tracked run of your script or notebook


```


From here, you load the PBMC 3k dataset and take it through whatever analysis you’re going to do, e.g., a standard pre-processing → PCA → clustering → nonlinear dimensionality reduction setup:


```text
library  (  Seurat  )
library  (  SeuratData  )


cells     <-     SeuratData  ::  LoadData  (  "pbmc3k"  )
cells     <-     NormalizeData  (  cells  ,     normalization.method     =     "LogNormalize"  ,     scale.factor     =     10000  )
cells     <-     FindVariableFeatures  (  cells  ,     selection.method     =     "vst"  ,     nfeatures     =     2000  )
all.genes     <-     rownames  (  cells  )
cells     <-     ScaleData  (  cells  ,     features     =     all.genes  )
cells     <-     RunPCA  (  cells  ,     features     =     VariableFeatures  (  object     =     cells  ))
cells     <-     FindNeighbors  (  cells  ,     dims     =     1  :  10  )
cells     <-     FindClusters  (  cells  ,     resolution     =     0.5  )
cells     <-     RunUMAP  (  cells  ,     dims     =     1  :  10  )
FeaturePlot  (  cells  ,     features     =     c  (  "MS4A1"  ,     "GNLY"  ,     "CD3E"  ,     "CD14"  ,     "FCER1A"  ,     "FCGR3A"  ,     "LYZ"  ,     "PPBP"  ,     "CD8A"  ))


```


Let’s save this Seurat object as an artifact in LaminR:


```text
saveRDS  (  cells  ,     "pbmc3k_processed.rds"  )
ln  $  Artifact  (  "pbmc3k_processed.rds"  ,     key     =     "pbmc3k/pbmc3k_processed.rds"  )  $  save  ()


```


And end the compute session:


```text
ln  $  finish  ()


```


If you use notebook mode in RStudio, this will automatically upload a run report. Otherwise, you can knit your notebook and run one of the following to save the knitted HTML:


CLI


```text
lamin    save    pbmc3k.Rmd


```


R


```text
lc  $  save  (  "pbmc3k.Rmd"  )


```


The above saves the` pbmc3k.Rmd


` notebook as a “transform” (shorthand for data transformation), linking it against the output file` pbmc3k/pbmc3k_processed.rds


` . This is also visible on the LaminHub GUI on the artifact page:[lamin.ai/laminlabs/training/artifact/VugfUMiwR8OtlnIU](https://lamin.ai/laminlabs/training/artifact/VugfUMiwR8OtlnIU)


Clicking on the` pbmc3k.Rmd


` notebook gives us the run report, which contains all plots and results:


To share this notebook, you can send colleagues a persistent link to the transform page:[lamin.ai/laminlabs/training/transform/KFtlfbCiP9Bm](https://lamin.ai/laminlabs/training/transform/KFtlfbCiP9Bm) .


Beyond inputs and outputs, this page also shows the environment—specifically, the packages that were loaded at the time of running the script:


Every unit of work that you do can now be easily shared and communicated, both with colleagues and your future self. For a given piece of data, every manipulation and its surrounding code are now recorded and stored, making it easier to understand and reproduce.


To learn more, take a look at the docs or the source code:


-


The docs:[https://docs.lamin.ai/laminr](https://docs.lamin.ai/laminr)


-


The LaminR source code:[laminlabs/laminr](https://github.com/laminlabs/laminr)


-


The LaminDB source code:[laminlabs/lamindb](https://github.com/laminlabs/lamindb)


## Acknowledgements¶


Thanks to Luke Zappia and Robrecht Cannoodt for creating LaminR and to Alex Wolf for editing this post.


## Background¶


Lamin engaged Tyler to illustrate a simple introductory use case for LaminR. This blog post is the result of that engagement.


[previous A data lakehouse for biology's sparse measurements](https://blog.lamin.ai/sparse-measurements)[next Managing spatial omics datasets with SpatialData & LaminDB](https://blog.lamin.ai/spatialdata)
