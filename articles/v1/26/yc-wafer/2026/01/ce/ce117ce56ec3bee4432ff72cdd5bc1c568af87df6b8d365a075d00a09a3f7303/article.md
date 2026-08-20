---
schema_version: "1.0.0"
document_id: "ce117ce56ec3bee4432ff72cdd5bc1c568af87df6b8d365a075d00a09a3f7303"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/rocprofiler-compute"
published_at: "2026-01-13T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:23:44.568277+00:00"
content_hash: "sha256:8313e153d34bf76c0683e41632a5619602e8676bfb89cb26f4b02e59e9c0a79f"
---

# Introducing ROCprofiler Compute: AMD GPU Profiling in Your IDE

**TLDR:** ROCprofiler Compute brings AMD GPU profiling into your IDE. View hardware metrics, roofline analysis, and kernel performance data directly in VS Code or Cursor.


- [Book a Demo](https://cal.com/wafer/20min)
- [Contact the team](https://www.wafer.ai/cdn-cgi/l/email-protection#80e8e9c0f7e1e6e5f2aee1e9)


Today we’re launching ROCprofiler Compute support in Wafer.


If you’re profiling AMD GPUs (MI300X, MI250, etc.), you can now view your` rocprof-compute` results directly in your editor with an interactive GUI.


## What’s included


**1. GPU Architecture Overview**


See your kernel’s performance at a glance. The architecture diagram shows every hardware block, including instruction dispatch, compute units, caches, and memory bandwidth, with real metrics from your profiling run.


**2. Roofline Analysis**


Understand where your kernel sits relative to hardware limits. The roofline plot shows arithmetic intensity vs. performance for L1, L2, HBM, and peak compute.


**3. Kernel Statistics**


View top kernels by execution time, dispatch lists, and per-kernel breakdowns. Sort and filter to find the hotspots.


**4. System Info**


Full hardware details: GPU model, compute units, cache sizes, clock speeds, ROCm version, and everything you need to understand your target hardware.


## How to use it


### Option 1: View existing profiling results


If you already have` rocprof-compute` results:


1. Open Wafer in VS Code/Cursor
2. Select “ROCprofiler Compute” from the tools menu
3. Browse to your workload folder (must contain` sysinfo.csv` )
4. Click “Launch GUI”


### Option 2: Profile and analyze with Wafer CLI


Profile your application:


```text
wafer   rocprof-compute   profile   --name   my_kernel   --   './my_hip_app'
```


Analyze results:


```text
wafer   rocprof-compute   analyze   workloads/my_kernel
```


Launch GUI:


```text
wafer   rocprof-compute   analyze   workloads/my_kernel   --gui
```


## Requirements


- ROCm 7.0+ with` rocprof-compute` 3.2+
- AMD GPU with ROCm support (gfx908, gfx90a, gfx940, gfx941, gfx942, gfx950)


The GUI viewer works on any platform; it only reads CSV files from your profiling run. You can profile on a remote AMD machine and view results locally.


## Try it out


If you’d like access for your team, talk to us.


- [Book a Demo](https://cal.com/wafer/20min)
- [Contact the team](https://www.wafer.ai/cdn-cgi/l/email-protection#b1d9d8f1c6d0d7d4c39fd0d8)
