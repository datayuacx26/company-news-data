---
schema_version: "1.0.0"
document_id: "65cdbc521a4f311ae1e8dbe0deeb6b5f1a78b08a7dfb3dea0ed34dd1cab925a8"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-soci-support-for-container-builds"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:959d2ba17eeb5f323bc2fa6b11e6f3b20a283c7fe8264d7c196151cbdf2e8d29"
---

# Now available: SOCI v2 support for Depot container builds

Most of our work at Depot goes into making the build fast. But the build is only half of how quickly you ship: once the image exists, every machine that runs it still has to download and unpack the whole thing before your process starts, and for the multi-gigabyte images that come with ML and GPU workloads, that pull can dwarf the build it came from.


Depot[container builds](https://depot.dev/docs/container-builds/overview) now support[SOCI](https://github.com/awslabs/soci-snapshotter) (Seekable OCI) v2. With one flag, your build produces a SOCI index that lets a compatible runtime stream the image on demand and start the container before the full image has arrived.


## How SOCI speeds up startup


A normal pull grabs each layer whole, decompresses it, and unpacks it before your entrypoint runs, even though a starting container usually reads only a fraction of those files. SOCI, a project from AWS, builds an index of where every file lives inside the compressed layers, so a compatible runtime can pull just what startup needs and fetch the rest later. In one[AWS benchmark](https://aws.amazon.com/blogs/machine-learning/reducing-container-cold-start-times-using-soci-index-on-dlami-and-dlc/) , this cut the time to start a multi-gigabyte vLLM inference container from about 7 minutes (nearly all of it spent pulling the image) to 21 seconds with the SOCI snapshotter. And unlike[eStargz](https://depot.dev/blog/booting-containers-faster-with-estargz) , SOCI v2 doesn't rewrite your layers: it ships the same bytes plus a small sidecar index (its zTOC), so images stay identical for runtimes that don't use it.


## Why we build the index for you


Normally a SOCI index is added after the fact: push the image, then run a separate job to pull it back down, compute the index, and push it up again. Images are deployable but, until that pipeline completes, very slow. A Depot build already has every layer in hand and is already pushing to your registry, so we generate the index as part of the build itself. When your push completes, the image and its index are ready to deploy together.


## How to use it


SOCI is opt-in, the same way Depot's other lazy-pull formats are. Add` soci=true` to your build's` --output` and push as usual:


```text
depot   build   -t   repo/image:tag   --push   \
--output   type=image,soci=true,compression=gzip,force-compression=true,oci-mediatypes=true   \
.
```


The` soci=true` attribute is what produces the index. SOCI indexes gzip layers, so` compression=gzip` is required, and` force-compression=true` makes sure every layer, including base-image layers pulled from elsewhere, is gzip so the whole image gets indexed. The result is an ordinary image with the index attached, ready for any runtime that knows how to use it.


That last part matters: the speedup only shows up when the platform pulling the image is set up to use SOCI. AWS Fargate does this automatically. On Amazon EKS with EC2 nodes, or your own hosts, you enable the soci-snapshotter in containerd (recent EKS AMIs ship it, but it's still off by default).


## Pricing


There's no extra charge for SOCI. Generating the index happens as part of your build, which is billed the same as any other Depot container build under our usage-based[pricing](https://depot.dev/pricing) .


## Conclusion


SOCI support is available now for every Depot container build, no setup required beyond the flag above. We'd love to hear how it works for your images:[reach out](https://depot.dev/help) if you have any questions, or come tell us what you're building in our[Discord](https://discord.gg/MMPqYSgDCg) .


## Related posts


- [Booting containers faster with eStargz](https://depot.dev/blog/booting-containers-faster-with-estargz)
- [Now available: OCI-compliant Depot Registry](https://depot.dev/blog/now-available-depot-registry-v2)
- [Optimize your Dockerfile for 5x faster builds](https://depot.dev/blog/optimize-your-dockerfile-for-5x-faster-builds)


Chris Goller


Principal Software Engineer at Depot
