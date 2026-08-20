---
schema_version: "1.0.0"
document_id: "f1f780a6c20cae45aced9ff3083506331749a13c099ff4df5f735cb07e837048"
company_key: "yc-lamin"
company: "Lamin"
source_id: "yc-lamin-news-import-1018c6ad32c5"
canonical_url: "https://blog.lamin.ai/annbatch"
published_at: null
first_seen_at: "2026-07-24T09:27:44.861145+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:738424dc3600fb878f3cbc3bde57002a31d9bf69fab8e40653719656b07dcde4"
---

# Scaling anndata training to the terabyte scale with annbatch ¶

- [LinkedIn](https://www.linkedin.com/posts/fabian-theis-4b4b10173_annbatch-unlocks-terabyte-scale-training-share-7449504033716899840-oVW8/) ·[Tweet](https://x.com/fabian_theis/status/2043739617787093016)
- ⸻ 2026-07-10


# Scaling anndata training to the terabyte scale with annbatch¶


- [Felix Fischer](https://github.com/felix0097) ,[Ilan Gold](https://github.com/ilan-gold) ,[Fabian Theis](https://scholar.google.com/citations?user=sqWpn2AAAAAJ&hl=en) ,[Alex Wolf](https://falexwolf.com/)


The demand for AI in omics has grown at an unprecedented rate, with state-of-the-art models now routinely trained on datasets exceeding the terabyte scale. To make that process more efficient, we developed` annbatch


` ,\[ 1 \] a high-performance data loader built on` anndata


` that enables loading speeds of 60k samples/second and more, at least a factor of 3 higher than the fastest recent alternatives.


Since` anndata


`\[ 2 \] released a first disk-backed data loader (` AnnCollection


` ) in 2019, better implementations have been developed, for instance, as used in SCimilarity\[ 3 \] , Cellarium\[ 4 \] or BioNEMO.\[ 5 \] Based on these improvements, some of us helped develop` MappedCollection


`\[ 6 \] to address the need for true weighted random sampling in 2023. This came, however, at a significant performance cost compared to approaches that load contiguous chunks, such as NVIDIA Merlin\[ 7 \] or the` tiledbsoma


` loader of CELLxGENE.\[ 8 \] In 2025,` scDataset


`\[ 9 \] and SLAF\[ 10 \] have been introduced with significant performance improvements.


With` annbatch


` ,\[ 1 \] we developed an` anndata


` -based loader that optimizes loading contiguous chunks, assumes pre-shuffling, and uses the popular` .zarr


` array format.\[ 11 \] It reaches 60k samples/second and more\[ 1 \] on the Tahoe-100M dataset,\[ 12 \] which stores transcriptional profiles of 100M cells ( **Figure 1** ). For reproducibility and to showcase the conversion of the original collection of` .h5ad


` files to a collection of` .zarr


` stores, benchmarks were tracked with data lineage ( **Figure 2** ).


**Figure 1 ([source](https://lamin.ai/laminlabs/arrayloader-benchmarks/artifact/AYfx4Nm2j0lpkkwK0000) )** : Dataloader throughput on the Tahoe-100M dataset across three loaders, with` scDataset


`\[ 9 \] shown both with a matched block/chunk size and with its recommended settings. By clicking on` source


` , you can navigate to the runs that produced the results. For example, the run producing the` annbatch


` results is[here](https://lamin.ai/laminlabs/arrayloader-benchmarks/run/ZSuaqX3BWwLzwduW) with information about parameters, environment, and hardware (` ml.m5.24xlarge


` on AWS).


**Figure 2 ([explore](https://lamin.ai/laminlabs/arrayloader-benchmarks/artifact/AYfx4Nm2j0lpkkwK0000) )** : Processing pipeline from the originally published Tahoe-100M, over pre-shuffled datasets, to running the data loader, to plotting Figure 1.


While the benchmarks here cache arrays from their cloud storage location to local disk, streaming data directly from S3 is a relevant use case, albeit resulting in lower loading speeds. A recent benchmark by Ryan Conrad\[ 13 \] found` annbatch


` to perform well in this case, too.


## Data & code availability¶


-


Paper:[arXiv:2604.01949](https://arxiv.org/abs/2604.01949)


-


Repo:[github.com/scverse/annbatch](https://github.com/scverse/annbatch)


-


Benchmarking database:[lamin.ai/laminlabs/arrayloader-benchmarks](https://github.com/laminlabs/arrayloader-benchmarks)


-


Benchmarking repo:[github.com/laminlabs/arrayloader-benchmarks](https://github.com/laminlabs/arrayloader-benchmarks)


## Acknowledgements¶


We are grateful to Raaghav Pillai for re-running benchmarks with the latest versions after a long development process. We thank Sergei Rybakov for early discussions, following the development of` MappedCollection


` . We thank Pavan Ramkumar for feedback and for validating early benchmarks. We thank Davide D’Ascenzo and Sebastiano Cultrera di Montesano for discussions related to` scDataset


` .


## References¶


---


[previous Simpler queries for the 2.5B transcriptional profiles of the Arc Virtual Cell Atlas](https://blog.lamin.ai/arc-virtual-cell-atlas)
