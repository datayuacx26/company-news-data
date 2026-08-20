---
schema_version: "1.0.0"
document_id: "2538ef21227fd15a6877c14fccb0dce08010a8ce38f8b29e14997da1d2770676"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/how-to-build-multi-arch-docker-images/"
published_at: "2022-07-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:5593d8c6be89097543b13b009403cc3b4385db3920ae4cfea6f9904228893d9b"
---

# How to Build Multi-Arch Docker Images

With ARM based dev machines and servers becoming more common, it is become increasingly important to build Docker images that support multiple architectures. This guide will show you how to build these Docker images on any machine of your choosing.


This is the graph that changed the landscape for dev machines. This graph opened thousands of issues on Github that said “M1 support when?”.[This website](https://isapplesiliconready.com/) was created in the immediate aftermath and surged with traffic. So here we are over a year later in a developer ecosystem where every engineer on a team might be in a completely different environment, whether that be Linux or Apple Silicon Macs or Intel Macs or[Github Codespaces](https://github.com/features/codespaces) or a custom built cloud dev box.


Meanwhile AWS has been slowly building up its ARM based instances with[Graviton 1](https://aws.amazon.com/blogs/aws/new-ec2-instances-a1-powered-by-arm-based-aws-graviton-processors/) and more recently,[Graviton 2](https://aws.amazon.com/about-aws/whats-new/2021/11/aws-graviton2-based-instances-amazon-neptune/) which have significant cost savings. Azure has joined the club with their[Ampere Altra](https://azure.microsoft.com/en-us/blog/now-in-preview-azure-virtual-machines-with-ampere-altra-armbased-processors/) offerings and[GCP just announced its ARM offerings](https://cloud.google.com/blog/products/compute/tau-t2a-is-first-compute-engine-vm-on-an-arm-chip) . At[Speedscale](https://speedscale.com/) , we’ve decided to ride this wave and take advantage of Graviton instances for our Kubernetes clusters. We build all of our Docker images for multiple architectures and deploy to ARM instances which has saved us about 30% in compute costs alone.


Given that the industry is embracing ARM and has mostly moved to containerized applications, surely all these architectures have been abstracted away and everything just works, right? Right?


## Multi-architecture images


Kind of. Take this simple Go image where we build a binary and put it in a final alpine release image.


```text
FROM   golang:1.18   as   build
WORKDIR   /go/src/github.com/kush/hello-world
COPY   . .
RUN   go build -o /hello-world
FROM   alpine
ENTRYPOINT   [  "/hello-world"  ]
COPY   --from=build /hello-world /hello-world
```


We can build this with` docker build -t hello-world .` .


To build the multi-arch version (linux amd64 and arm in this case) of this we do the following


```text
$   docker   buildx   create   --use   # only needed the first time
$   docker   buildx   build   --platform   linux/amd64,linux/arm64   -t   hello-world   .
=  > [linux/arm64   internal]   load   metadata   for   docker.io/library/golang:1.18     1.3s
=  > [linux/amd64   internal]   load   metadata   for   docker.io/library/golang:1.18
```


Easy enough. You’ll notice that two builds are happening in parallel for each platform you specified. You might also notice this warning when you build your image


` WARNING: No output specified for docker-container driver. Build result will only remain in the build cache. To push result image into registry use --push or to load image into docker use --load`


If you run the command again with the` --load` option, you’ll see` error: docker exporter does not currently support exporting manifest lists` . Furthermore if you run` docker image ls grep hello-world` you won’t see the image we just built.


### So what’s going on here?


Container images with support for multiple architectures are part of the[OCI specification](https://github.com/opencontainers/image-spec/blob/main/image-index.md) and Docker supports creating these as well. The image index (more commonly referred to as the image manifest) contains some metadata about the image itself and an array of actual manifests which specify the platform and the image layer references. Docker supports creating these but only through the experimental new builder, buildx.


Buildx has some nice new features like support for better caching between images as well as cleaner output during builds. However, it runs completely independently of your standard local docker registry. If you run` docker ps` , you’ll see a buildx builder running as a container on your local machine. This is a virtual builder that we created using` docker buildx create` . There are ways to create builders that run as Kubernetes pods or remote Docker machines as well but these require a lot more setup.


Unfortunately because of this there’s no way to manipulate the images created with buildx after the fact. You can no longer do` docker push` since the image doesn’t exist in your registry so you must add` docker buildx build --push` to your build command which will push to a remote registry right after the build is finished.


## Can we go faster?


Functionally, we’ve achieved all that we needed to in the above example but on my machine, that build takes about 9 seconds. Part of this slowness comes from the fact that I’m emulating a different architecture meaning that instructions are constantly being converted from one CPU architecture to another, more details[here](https://www.docker.com/blog/faster-multi-platform-builds-dockerfile-cross-compilation-guide/) . Since my project is in Go and I know Go natively supports cross compilation, I can leverage this to make my builds even faster. Here is the cross compilation version of my Docker image.


```text
FROM   --platform=$BUILDPLATFORM golang:1.18   as   build
WORKDIR   /go/src/github.com/kush/hello-world
COPY   . .
ARG   TARGETOS TARGETARCH
ENV   GOOS $TARGETOS
ENV   GOARCH $TARGETARCH
RUN   go build -o /hello-world
FROM   alpine
ENTRYPOINT   [  "/hello-world"  ]
COPY   --from=build /hello-world /hello-world
```


This takes about 7 seconds on my machine and would be a more significant difference for a non trivial example.


Docker populates the variable` BUILDPLATFORM` which is used to avoid using an emulated base image. It also populates` TARGETOS` and` TARGETARCH` which I use to tell my compiler which architecture to build for. Note that we don’t change anything in the final release image, we use the architecture specific` alpine` image with our cross compiled binaries.


For more details on how this actually works, check out[this blog post by Docker](https://www.docker.com/blog/faster-multi-platform-builds-dockerfile-cross-compilation-guide/) which visually represents the difference between emulation and cross compilation.


## Summary


1. Create and use a docker buildx builder with` docker buildx create --use`
2. Build and push with` docker buildx build --push --platform linux/amd64,linux/arm64 -t {REGISTRY}:{IMAGE}:{tag} .`
3. If you can cross compile, do that because it will be much faster.


1. If you’re using Go and specifically depend on cgo, you can still cross compile but you’ll need to install a C cross compiler for eg.` gcc-aarch64-linux-gnu libc6-dev-arm64-cross`


4. If you’re using any sort of other binaries during your build steps such as a` yum install` or` apt-get` , you may need to run this magic line in order for the emulation to work correctly` docker run --rm --privileged multiarch/qemu-user-static --reset -p yes`


## What next?


You can now successfully produce multi-architecture Docker ARM images so that you can run them on whatever machine you want. But what about actually deploying these to production? In all likelihood, you depend on projects that do not publish multi-architecture images yet but you still want to reduce costs by using ARM instances in your Kubernetes cluster. In the next blog post, we’ll guide you through setting up a Kubernetes cluster with multiple architecture nodes and how to guarantee that your existing workloads will not be disrupted in the process.
