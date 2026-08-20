---
schema_version: "1.0.0"
document_id: "f6a73e68611dfd01968f9ced8eec94837d9d0c54b35d75da561e9639082aa1a4"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds"
published_at: "2024-09-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:7f76f906b7cdcbed9e76e3e0605eceac52495098dbe4ceef16e0948a3115abe2"
---

# Comparing GitHub Actions and Depot runners for 2x faster builds

GitHub Actions is a powerful platform, but some quick testing can show that it's not always the most performant or cost-effective solution. Let's look at the differences between the default GitHub Action runners and Depot GitHub Action runners to show how changing one line in our workflow can speed up our builds by 2x and simultaneously cut our costs in half.


## The test workflow


To benchmark the performance, we’ll write a GitHub Actions workflow that runs parallel builds across both GitHub and Depot runners. The parallel setup allows us to directly compare build times under identical conditions. We chose the[Next.js](https://github.com/vercel/next.js) project as our target because of its heavy dependency load, and popular adoption among developers, making it an ideal candidate for testing CI/CD runner performance under real-world conditions.


We took the` build` job from the Next.js workflow and modified it to run on both` ubuntu-22.04` and` depot-ubuntu-22.04` runners in parallel. You can find the full benchmark workflow file[here](https://github.com/depot/Compare-Runners-Cache-Test/blob/main/.github/workflows/workflow.yaml) . We ran the tests while the repository was private to compare like-for-like performance, as GitHub gives an additional 2 cores to open-source repositories.


```text
name  :   CompareCache


on  :   push


jobs  :
build  :
name  :   'Build: ${{ matrix.OS }}'
runs-on  :   ${{ matrix.OS }}
strategy  :
matrix  :
OS  : [  ubuntu-22.04  ,   depot-ubuntu-22.04  ]
NODE_VERSION  : [  20  ]
steps  :
-   name  :   Disable git crlf
run  :   git config --global core.autocrlf false


-   name  :   Checkout
uses  :   actions/checkout@v4
with  :
repository  :   vercel/next.js
ref  :   v14.2.12
fetch-depth  :   0


-   name  :   Setup PNPM
uses  :   pnpm/action-setup@v3


-   name  :   Setup node@${{ matrix.NODE_VERSION }}
uses  :   actions/setup-node@v4
with  :
node-version  :   ${{ matrix.NODE_VERSION }}
cache  :   'pnpm'


-   name  :   Install dependencies
run  :   pnpm install


-   name  :   Build Packages
run  :   pnpm run build
```


## Not all cores are created equal


Both GitHub Actions and Depot offer a range of labels to choose from when selecting a runner, allowing you to customize the number of vCPU cores and memory for your workflow. However, not all vCPU cores are created equal.


In our tests, Depot runners were consistently 2x faster than GitHub runners. This performance boost is largely due to our faster CPUs, additional memory, and[significantly increased disk I/O throughput](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10928602824/job/30337393246) .


Runner Host vCPU(s) Sysbench CPU Speed


ubuntu-22.04 github 2 4119.42


depot-ubuntu-22.04 depot 2 8917.15


In the example above, we benchmarked the CPUs using[sysbench](https://github.com/akopytov/sysbench) to compare the CPU performance of the two runners. Generally speaking, the default CPU on the Depot runner is around 2x faster than GitHub.


## Network speed: Depot vs GitHub


Caching in these tests was performed using the` actions/setup-node@v4` action, which implements the` actions/cache` action internally to cache dependencies by tar-balling the appropriate cache directory (configured for pnpm in this test) and uploading it to blob storage.


The cache is downloaded and extracted to the appropriate directory on subsequent runs. On the hosted GitHub Actions runner, this will typically interface with Azure Blob Storage under the hood. The same action when running on a Depot runner will use our own accelerated cache storage.


Aspect GitHub Actions default Depot


Cache download throughput 100-150 MB/s 1 GB/s


Cache upload throughput 100-150 MB/s 1 GB/s


Cache size limit 10 GB Unlimited


Retention 7 days 30 days


Previously, we shared how[we reverse-engineered GitHub's Cache Action to enhance performance](https://depot.dev/blog/github-actions-cache) for our runners. By boosting network throughput and increasing parallelism during uploads and downloads, we've significantly sped up dependency retrieval, both on the initial run and when leveraging the cache.


## Real-world CI time and cost comparison


To better understand the real-world performance and cost differences, let’s compare the build times and per-minute costs of the two CI runners.


### Runners


Runner Host vCPU(s) Memory Storage Cache Size Cost per Minute


ubuntu-22.04 github 2 7Gb 14Gb 10Gb $0.008 USD


depot-ubuntu-22.04 depot 2 8Gb 100Gb 100Gb $0.004 USD


Note: GitHub Actions bills per minute, rounding up to the next full minute. On Depot all builds are billed on a per-minute basis, tracked per second. These cost estimates account for that difference.


### Test Results


The test will checkout a recent tag of the[Next.js](https://github.com/vercel/next.js/tags) project, install dependencies, and build the project using the[actions/setup-node](https://github.com/actions/setup-node) with caching enabled for` pnpm` .


#### Run 1: Without cache


The first run will not contain any cache, so dependencies will be pulled from the network externally.


Runner Install Time (s) Build Time (s) Total Time (m) Cost (USD)


ubuntu-22.04 53.8s 105.641 4m 49s $0.04


depot-ubuntu-22.04 40.5 61.658 3m 32s $0.014


**Key Takeaways:**


- Depot runner finished 24% faster when installing dependencies.
- Build time was over 60% faster on Depot’s runner.
- The Depot runner reduced costs by 65%.


#### Run 2: With cache


The second run will pull the dependencies from the cache created in the first run.


Runner Install Time (s) Build Time (s) Total Time (m) Cost (USD)


ubuntu-22.04 16.8 109.457 4m 6s $0.04


depot-ubuntu-22.04 10.7 59.139 3m 1s $0.012


**Key Takeaways:**


- With caching, both runners improved dramatically, but Depot was still 57% faster than GitHub’s runner.
- Although the GitHub Actions runner completed the job in just over 4 minutes, GitHub rounds up to the nearest minute, billing for 5 full minutes.
- Depot once again reduced the cost by 70%, coming in at just $0.012 for the run, compared to GitHub’s $0.04.


Additional runs of the workflow are fairly consistent with the results of the second run.


### Monthly savings estimate


To get a better idea of how this scales, let’s run the numbers over a month of continuous builds. Assuming you’re running about 100 builds a day, we can estimate the potential time and cost savings over 30 days.


Runner Total Time (h) Billed Time (h) Total Cost (USD)


ubuntu-22.04 205h 250h $120


depot-ubuntu-22.04 150h 50m 150h 50m $36.2


**Key Takeaways:**


- Over 50 hours of build time saved per month with Depot.
- Over $80 saved per month with Depot.
- GitHub's round up model adds an additional 45 hours of build time over a month.


## Compare your own workflows


If you’re curious to see how Depot performs with your own CI pipelines, you can easily set up a direct comparison. By adding a matrix strategy to your GitHub Actions workflow, you can run jobs on both GitHub and Depot runners side-by-side, no other changes required. Start a[7-day free trial](https://depot.dev/sign-up) and try your own experiments.


```text
name  :   your-workflow-name
on  :   push


jobs  :
build  :
runs-on  :   ${{ matrix.runner }}
strategy  :
matrix  :
runner  : [  ubuntu-22.04  ,   depot-ubuntu-22.04  ]
```


Depot offers a range of[Intel and Arm runners](https://depot.dev/docs/github-actions/runner-types) with up to 64 vCPUs and 256GB of memory. You can select a runner that matches your current setup for a direct comparison. Given Depot’s lower costs, it’s worth experimenting with higher-spec runners to identify the optimal balance between speed and cost for your workflow.


## Summary


From our tests, we can see that Depot runners outperformed Github Actions runners in both build times and cost. Build times for a Next.js project were consistently twice as fast on Depot runners and well over 60% cheaper.


The repository where we ran these tests is open and[available for you to clone](https://github.com/depot/Compare-Runners-Cache-Test/) and run your own tests, or you can add a matrix strategy to your existing workflows to compare the performance of Depot runners with your current setup.


Our friends at OpenMeter found similar results when they switched to Depot runners,[reducing their build costs by 50%](https://openmeter.io/blog/how-we-made-our-ci-pipeline-5x-faster) .


Kyle Tryon
