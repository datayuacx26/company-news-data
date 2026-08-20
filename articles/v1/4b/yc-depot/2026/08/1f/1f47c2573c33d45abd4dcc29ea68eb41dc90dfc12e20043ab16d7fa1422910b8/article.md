---
schema_version: "1.0.0"
document_id: "1f47c2573c33d45abd4dcc29ea68eb41dc90dfc12e20043ab16d7fa1422910b8"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/why-soci-belongs-in-the-build"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T16:25:42.347718+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:f3d832129a8ab510fd176602164824501f380a2e5ea9f84fa711716e7a9b60f8"
---

# Why SOCI belongs in the build

I think container startup should be a build artifact.


Fast builds are only half the story. At Depot we work to make container builds faster; however, a container that builds quickly can still make the deployment slow. Specifically, if a container is really large, every node still has to pull and decompress gigabytes before the container starts.


Modern containers are not tiny bundles of application code (well, maybe they never were). We see many containers often include many gigabytes of language runtimes and shared libraries like CUDA, PyTorch, and more. It's enough to make "pull the container" a real deployment phase.


There's tech to help speed up deployment by lazy-loading the container.


SOCI , or Seekable OCI, gives a container runtime a map, or index, of what files are inside the container. The SOCI containerd snapshotter uses that index to download just a file's specific bytes when a file is requested. In other words, it gets what it needs first and loads the rest later.


We've just added SOCI support in Depot container builds, so you can make SOCI v2 metadata during your builds.


## When container startup gets slow


The traditional container startup path is straightforward. containerd downloads the container layers, decompresses each layer, and unpacks the layer into a snapshotter. After that it starts the container entrypoint.


That works fine when container images are small, however it gets really slow when the container is many gigabytes.


The container layer format we've all inherited is part of the reason. Traditional layers are filesystems archived with tar and compressed with gzip. However, tar is sequential and gzip is also stream-oriented. The practical implication is that to get a file in the middle of the layer, you must unpack all previous files. containerd does not have a good way to request some file in a random place in the layer.


So, the default pull thinks of layers like big sequential blobs: download the entire blob, verify it all, inflate it all, unpack it all, and repeat for all layers.


## Loading only what you need


Most containers actually do not need the entire filesystem to start executing. A large amount of the files in the container might not be read until later, or perhaps not even read at all. It needs the entrypoint, application code, and whatever the startup path actually reads.


SOCI, or Seekable OCI, was built by AWS to make "seeking" to random files possible and thus speed up starting containers.


```text
Traditional pull — wait on the whole image before anything runs


┌──────────┐
│┌─────────┴┐
││┌─────────┴┐        ┌──────────┐       ┌──────────┐        ┌──────────┐
│││          │        │          │       │          │        │          │
│││  fetch   │        │decompress│       │  unpack  │        │  start   │
└┤│  layers  │───────▶│   all    │──────▶│all files │───────▶│container │
└┤          │        │          │       │          │        │          │
└──────────┘        └──────────┘       └──────────┘        └──────────┘


SOCI lazy-load — start almost immediately, stream the rest


┌──────────┐                          ┌──────────┐
│  fetch   │                          │          │
│  small   │                          │  start   │
│  index   ├─────────────────────────▶│container │
│          │                          │          │
└──────────┘                          └──────────┘
│
│
▼
┌──────────┐       ┌──────────┐       ┌──────────┐
│          │       │          │       │          │
│fetch file│       │   FUSE   │       │read file │
│on demand │◀──────│          │◀──────│on demand │
│          │       │          │       │          │
└──────────┘       └──────────┘       └──────────┘
```


## SOCI v2 format


A SOCI v2 container has an OCI artifact next to the container. It is another index that points to zTOCs for each layer.


A zTOC has two kinds of information: metadata from the tar and metadata from the gzip. The tar metadata is a copy of the layer tar's file paths, sizes, offsets, and so on. The gzip metadata are checkpoints and spans that let the runtime seek into compressed gzip to skip decompressing the whole layer.


When starting a container, the SOCI snapshotter gets all the metadata first. When the container asks for a file the snapshotter can look up where that file is, get the compressed spans that have it, and decompress.


Therefore, in the end the runtime can download specific files rather than treating the layer as one indivisible blob.


## Different ways to make container images seekable


SOCI is not the first attempt to make container images start faster.


[eStargz](https://depot.dev/blog/booting-containers-faster-with-estargz) makes gzip layers seekable by changing how the layer is built. It broadly keeps tar+gzip, but makes layers that include extra structure and a table of contents.


[Nydus](https://github.com/dragonflyoss/nydus) takes a different approach with its own service and filesystem format optimized for lazy pulling.


SOCI is similar to eStargz but does not change the layer. The part I like is that SOCI v2 keeps the existing compressed layers the same and creates metadata alongside. The layer bytes are still the same layer bytes. The SOCI index metadata says where files and compressed spans are, instead of rewriting the layer into a different format.


It turns out that creating the metadata index at build time is efficient because the extra work is relatively small. We read the compressed layers to create SOCI's metadata index; SOCI calls it the zTOC, or zipped table of contents. After the build, Depot exports metadata to the registry right next to the image.


## Benches


AWS has been working on SOCI because deploying ML container images has this problem. In AWS'[Deep Learning AMI and Deep Learning Containers](https://aws.amazon.com/blogs/machine-learning/reducing-container-cold-start-times-using-soci-index-on-dlami-and-dlc/) post, AWS showed SOCI helped their container start time decrease from seven minutes to twenty-one seconds.


SOCI has a bonus improvement I won't dig into here. AWS covers it in detail in their blog about[parallel pull for Amazon EKS](https://aws.amazon.com/blogs/containers/introducing-seekable-oci-parallel-pull-mode-for-amazon-eks/) . The authors found that a large container pull dropped from about five minutes to about two minutes. This is because the SOCI snapshotter has additional optimizations for parallel pulling.


## SOCI at build time


Historically, the awkward part of SOCI has been building it.


The typical flow is post-push indexing. This means that you build the container, push it to the registry, and then have another process pull and create SOCI metadata. Finally, you push that additional metadata back to the registry.


My instinct is that the build is the better place to do it.


The build already has the layers and the build is already pushing the artifact to the registry. So, we updated BuildKit to create the SOCI metadata at build time. Now you don't need a post-push SOCI indexing pipeline. As soon as the image is pushed to the registry it is already deployable.


```text
Post-push SOCI indexing needs a second CI pipeline.


┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
│          │   │          │   │pull image│   │          │   │          │   │          │
│  build   │   │ push #1  │   │          │   │  create  │   │ push #2  │   │  deploy  │
│          │──▶│          │──▶│decompress│──▶│SOCI index│──▶│          │──▶│          │
│          │   │          │   │and unpack│   │          │   │          │   │          │
└──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘


(not deployable until extra pull and indexing finishes)


Depot's build-time indexing so we can ship with the container.


┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│          │  │          │  │          │  │          │
│  build   │  │  create  │  │   push   │  │  deploy  │
│          │─▶│SOCI index│─▶│          │─▶│          │
│          │  │          │  │          │  │          │
└──────────┘  └──────────┘  └──────────┘  └──────────┘
```


## Building a SOCI-enabled container with Depot


We do not turn SOCI on for every build. This is an opt-in feature, just like so many BuildKit features.


```text
depot   build   -t   repo/image:tag   --push   .   \
-o   type=image,soci=true,compression=gzip,force-compression=true,oci-mediatypes=true
```


` soci=true` turns on the SOCI metadata creation.


` force-compression=true` is worth adding because it re-encodes every layer, including base-image layers pulled from elsewhere, as gzip so the whole image gets indexed. SOCI only indexes gzip layers.


At the end of the day, the pushed container contains the normal layers plus the zTOC map the soci-snapshotter can use.


To get the startup benefit the platform running the container has to understand SOCI. On AWS, Fargate does it automatically. On Amazon EKS or ECS with EC2 nodes, or your own hosts, you install the soci-snapshotter as a containerd plugin and voila.


## Conclusion


A fast build doesn't always mean a fast deploy. You can build a container in seconds and still watch every node pull and decompress gigabytes before it starts. So the wait just moves to deploy time.


This is why I think container startup should be a build artifact. SOCI lets the runtime fetch only the bytes it needs to start, and because Depot builds the SOCI index during the build instead of in a separate post-push pipeline, that fast startup ships with the image. The moment it's pushed to the registry it's already deployable.


Flip on` soci=true` , push, and let the runtime pull the rest lazily.


## FAQ


Does enabling SOCI change my container image layers?


No. SOCI v2 keeps your existing compressed layers exactly as they are and writes the index metadata alongside them as a separate OCI artifact. The layer bytes stay the same layer bytes, so a runtime that doesn't understand SOCI just ignores the extra metadata and pulls the image the normal way.


How do I build a SOCI-enabled image with Depot?


Push with the SOCI output options on your` depot build` : set` soci=true` alongside` compression=gzip` in the` -o type=image,...` flags, since SOCI only indexes gzip layers. Adding` force-compression=true` re-encodes every layer, including base-image layers pulled from elsewhere, as gzip so the whole image can be indexed. Once the image is pushed it carries the zTOC metadata right next to the normal layers.


Do I need to change anything on the runtime to get the startup benefit?


Yes. The platform running the container has to understand SOCI. On AWS, Fargate does it automatically. Everywhere else, including Amazon EKS or ECS on EC2 nodes and your own hosts, you install the soci-snapshotter as a containerd plugin. Without a SOCI-aware runtime the image still runs fine, you just don't get the lazy-loading, since it pulls the whole thing the old way.


Does creating the SOCI index at build time slow down my build?


Not by much. Building the index means reading the compressed layers to record where files and compressed spans live, and that extra work is relatively small next to the build itself. The build already has the layers in hand and is already pushing to the registry, so indexing there is cheaper than a separate post-push pipeline that pulls the image back down just to index it.


## Related posts


- [Now available: SOCI v2 support for Depot container builds](https://depot.dev/blog/now-available-soci-support-for-container-builds)
- [Pulling containers faster with eStargz](https://depot.dev/blog/booting-containers-faster-with-estargz)
- [Now available: OCI-compliant Depot Registry](https://depot.dev/blog/now-available-depot-registry-v2)


Chris Goller


Principal Software Engineer at Depot
