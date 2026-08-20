---
schema_version: "1.0.0"
document_id: "37fe71b267bb61085d824dd8e17edc4e686d9fb4ebddde562fbc479382a9547b"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/building-images-gzip-vs-zstd"
published_at: "2024-10-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:845471ef9506c05b2b8467c2064ad04ef246c28a1dfd447efbf6f84b28e67668"
---

# Building Images: Gzip vs Zstd

When you pull (or push) a Docker image from a registry, what really happens behind the scenes? When you request an image, Docker first retrieves a[manifest](https://docs.docker.com/reference/cli/docker/manifest/) from the registry. This manifest contains crucial information about the image, such as its architecture, configuration, and most importantly, a list of layers (individual file system changes) that need to be downloaded.


By default, Docker checks the manifest for a build that matches your system’s architecture (e.g.,` linux/amd64` ). If it finds a match, Docker retrieves a manifest specific to that architecture, which includes the layers that make up the image.


```text
docker   manifest   inspect   hello-world@sha256:e2fc4e5012d16e7fe466f5291c476431beaa1f9b90a5c2125b493ed28e2aba57
```


```text
{
"schemaVersion"  :   2  ,
"mediaType"  :   "application/vnd.oci.image.manifest.v1+json"  ,
"config"  : {
"mediaType"  :   "application/vnd.oci.image.config.v1+json"  ,
"digest"  :   "sha256:d2c94e258dcb3c5ac2798d32e1249e42ef01cba4841c2234249495f87264ac5a"  ,
"size"  :   581
},
"layers"  : [
{
"mediaType"  :   "application/vnd.oci.image.layer.v1.tar+gzip"  ,
"digest"  :   "sha256:c1ec31eb59444d78df06a974d155e597c894ab4cda84f08294145e845394988e"  ,
"size"  :   2459
}
],
"annotations"  : {
"org.opencontainers.image.revision"  :   "3fb6ebca4163bf5b9cc496ac3e8f11cb1e754aee"  ,
"org.opencontainers.image.source"  :   "https://github.com/docker-library/hello-world.git#3fb6ebca4163bf5b9cc496ac3e8f11cb1e754aee:amd64/hello-world"  ,
"org.opencontainers.image.url"  :   "https://hub.docker.com/_/hello-world"  ,
"org.opencontainers.image.version"  :   "linux"
}
}
```


You can see under` layers.MediaType` , the MIME type of the layer ends in` tar+gzip` . As you pull the image, Docker downloads these compressed layers and then extracts them into a filesystem on your machine. This is why you see messages like downloading , followed by extracting during the pull process:


```text
Pulling   from   library/rust
c1e0ef7b956a:   Pull   complete
143d0f027108:   Extracting   [========================>        ]    31.75MB/64.35MB
```


When you push an image, Docker will automatically compress the layers with` gzip` before sending them to the registry. However, the` buildx build` or` depot build` commands give us much more control over our build process. We can specify the compression method we want to use while building and pushing using the` --output` flag.


```text
depot   build   .   --output   type=image,name=  <  registr  y  >  /  <  namespac  e  >  /  <  repositor  y  >  :  <  ta  g  >  ,compression=  <  compression   metho  d  >  ,oci-mediatypes=true,platform=linux/amd64
```


You can see in the[BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#output) the` compression` flag can be set to` uncompressed` ,` gzip` (default),` zstd` , or` estargz` . We'll do a deeper drive on` estargz` in an upcoming post, as that is a bit of a special case. But what about the other options?


## What is Gzip?


Gzip is the default compression method used by BuildKit and Docker (for instance, when you run the` docker push` command). Gzip is a wrapper around the` DEFLATE` algorithm, and has been the standard for general compression since the 90's.


Gzip (unlike` zip` ) concatenates files together as a` tar` before compressing them, allowing for better compression than individually compressing files. The final compressed collection of files is called a` tarball` and has the extension` .tar.gz` .


```text
# The `-z` tells `tar` to use `gzip` to compress the files.
tar   -czvf   myfiles.tar.gz   myfiles
```


The` -z` tells` tar` to use` gzip` to compress the files.


One common complaint of` gzip` is that it is still a single-threaded application. For one reason or another, probably for compatibility due to how ubiquitous and long-standing` gzip` is,` gzip` has not been updated to take advantage of modern multi-core processors. There is however, a parallel implementation of` gzip` called[pigz](https://zlib.net/pigz/) that can take advantage of multiple cores when compressing or decompressing files.


We've talked before about how[compression isn't free](https://depot.dev/blog/uncovering-disk-io-bottlenecks-github-actions-ci#monitoring-disk-during-untar-of-nextjs-dependencies) , but it is usually made up for in the time saved by transferring smaller files over the network. Depending on your specific use-case and data, you may want to consider using a different compression method, or maybe even no compression at all.


If you were dealing with a machine that does not have a lot of spare CPU cycles, but does have a strong network connection, it would potentially be faster to pull uncompressed layers from your registry. However, there might be a best-of-both-worlds scenario with a newer compression algorithm like` zstd` .


## What is Zstd?


[Zstandard (zstd)](https://facebook.github.io/zstd/) was developed by Facebook in 2015 and open-sourced a year later. Since then it has been rapidly gaining support due to a number of modern features, but most significantly its strong decompression performance. Zstandard was designed with the intent of providing similar compression ratios to` gzip` , but with much faster decompression speeds.


One of the largest differences out of the box, is` zstd` is a multi-threaded tool, while the time-tested` gzip` is still a single-threaded application. At it's core, Zstandard shares some similarities with` gzip` . Just like` gzip` is a wrapper around` DEFLATE` , Zstandard uses a combination of LZ77 dictionary-matching (used in` DEFLATE` ) with a different entropy encoding scheme.


## Benchmark Gzip vs Zstd


This is not a comprehensive real-world benchmark, but this should give us a rough simulation of how the different compression methods will compare in the context of building and pushing a container image.


In this test, we will build the[PostHog/posthog](https://github.com/PostHog/posthog) Docker image as an uncompressed tarball and then compress and uncompress it using both` gzip` and` zstd` . In actuality, each layer would be compressed individually and there would be a little additional overhead in making multiple verification checks, but we will be running these all on the same machine (` depot-ubuntu-24.04-16` ) with 16 CPU cores, so the relative performance should be similar.


### Comparison of Compression Ratios


By default,` zstd` is designed to perform similarly in compression ratio to` gzip` , though we can see it still has a slight edge.


Both` gzip` and` zstd` have tunable compression levels, with` zstd` having a much larger range of options. In this test, we used the default compression levels for both` gzip` and` zstd` . Additionally, I've included` zstd` level 9 as it is a common choice for high compression without sacrificing too much speed.


### Comparison of Compression Times


Clearly, being limited to a single thread is extremely detrimental to our build times. Interestingly,` pigz` (the parallel implementation of` gzip` ) will be used by` containerd` for *decompression* if it is installed on the host machine, **but not for *compression*** .


Single-stream` gzip` considers the whole file as a single chunk, meaning excellent compression ratios but limited by the speed of a single thread. Gzip has an alternate mode that supports chunked compression, where the file is subdivided and compressed separately, and then finally concatenated together to form a tarball. This is ultimately the property that` pigz` leans on, compressing each chunk in parallel in a separate thread, completing work an order of magnitude faster.


This sounds like a win all around. However, even with` pigz` present on the machine, Docker will still by default compress image layers using single-stream` gzip` for ecosystem compatibility. Although both` gzip` and` pigz` produce correct layers, they arrive at their tarballs slightly differently and thus produce layers of different sizes and hashes. When those layers are ultimately pushed to a registry, they'll be recognized as different images despite having identical uncompressed content. Docker as a platform is making a tradeoff here, preferring canonical images in Docker Hub at the expense of slower compression speeds.


Depot, on the other hand, aims to be the fastest place to build software, and so we've placed a higher premium on compression speed. We've modified our builders to use` pigz` for faster compression out of the box. So if you are currently building with the` depot build` command, you are already taking advantage of multi-threaded compression.


Zstandard is natively supported by` containerd` and can be used for both compression and decompression. Standard` zstd` compression is within a margin of error for compression time compared to` pigz` , and will theoretically be faster for decompression.


In this test, using` zstd` at level 9 significantly increased the compression time while only providing a modest improvement in the compression ratio. It's a good idea to run similar benchmark tests with your own container builds to see what will ultimately be the best choice for you over the long term.


### Comparison of Decompression Times


Often the most pertinent metric is our decompression time. If we were thinking of a Kubernetes deployment that needed to scale up quickly, we need to ensure that our images can be decompressed quickly.


Zstandard is significantly faster in our test than` pigz` , and interestingly, decompressing` zstd-9` was roughly the same (actually slightly faster in this run) as the default` zstd` compression level. So with a little more compute up front, some bandwidth can be saved in the pull process without affecting the decompression time.


Luckily as we said before,` pigz` will be used by` containerd` for decompression if it is installed on the host machine, so we can still take advantage of parallel decompression in *most* situations. If you currently are using gzipped images, which is all images by default, you should definitely consider installing` pigz` on your host machine.


## Conclusion


Determining the best option often comes down to various factors, such as data type, core count, and network speed. But in general, adding` zstd` to your builds is an easy "one-liner" improvement that can save time, bandwidth, and potentially money.


Where` zstd` truly shines is in its decompression speed, which in this test was **nearly 60% faster** than` pigz` while producing a smaller file than` gzip` in the compression stage. That is an easy way to get your pods starting roughly twice as fast or save on your CI/CD bill.


If you are building images with` depot build` , you are already benefiting from multi-threaded compression in your build process via` pigz` . Whether you are building images with` depot build` or` docker buildx build` , you can still benefit from faster decompression times by using` zstd` for your images by setting the` --output` flag to` compression=zstd` .


```text
-   uses  :   depot/build-push-action@v1
with  :
context  :   .
tags  :   '<org>/<image>:<tag>'
outputs  :   'compression=zstd,oci-mediatypes=true'
```


## Related posts


- [Uncovering Disk I/O Bottlenecks in GitHub Actions](https://depot.dev/blog/uncovering-disk-io-bottlenecks-github-actions-ci)
- [BuildKit in depth: Docker's build engine explained](https://depot.dev/blog/buildkit-in-depth)
- [How Depot speeds up Docker builds](https://depot.dev/blog/depot-magic-explained)


Kyle Tryon
