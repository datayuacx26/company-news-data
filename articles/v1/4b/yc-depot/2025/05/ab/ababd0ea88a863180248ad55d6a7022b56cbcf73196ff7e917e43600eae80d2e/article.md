---
schema_version: "1.0.0"
document_id: "ababd0ea88a863180248ad55d6a7022b56cbcf73196ff7e917e43600eae80d2e"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/go-code-to-container-depot-api"
published_at: "2025-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:d2916f1ae7e8bdaf7fe65947aa9183e3d8089f9369e2b2a6664b2572f91fbaab"
---

# From Go Code to Container Image with Depot API

# From Go Code to Container Image with Depot API


In this post we're going to dive into how you can build container images via the[Depot API](https://depot.dev/docs/api/overview) using our Go SDK. Allowing you to build container images directly from your own code using Depot as the underlying build platform.


The Depot API can be used to manage a number of different entities in Depot. But, it can also be used to manage isolated project caches, launch BuildKit machines, and build container images from code. In this first of several posts, we'll start at the lowest level and build container images. In later posts we'll show you how to create and tune isolated caches.


[If you're still writing build scripts in 2026, you're already behind. Use the API instead →](https://depot.dev/sign-up)


## How Depot's Build Flow Works


With the Depot API, there are three steps to build a container.


1. Register a build with Depot.
2. Acquire a Depot build machine.
3. Configure, build, and push a container.


## Before You Start


Before you start, ensure you have a[Depot project setup](https://depot.dev/docs/container-builds/overview#projects) and an[organization API token](https://depot.dev/docs/api/authentication) generated.


## Install the Go Package


To start, let's pull in the[Depot Go package](https://github.com/depot/depot-go) .


```text
go   get   github.com/depot/depot-go
```


Check out the[imports](https://github.com/depot/depot-go/blob/main/examples/simple/main.go) in the example repo for a quick copy/paste.


## Register a Build


First, we request a new build with the Depot API. The request routes a build to a specific project's isolated cache. When registered, the Depot API will respond with the new build's ID and a one-time build token. We defer reporting the build result, buildErr, to the API. Any non-nil error is a build failure.


```text
token   :=   os.  Getenv  (  "DEPOT_TOKEN"  )
project   :=   os.  Getenv  (  "DEPOT_PROJECT_ID"  )


req   :=   &  cliv1  .  CreateBuildRequest  {
ProjectId: project,
}


build, err   :=   build.  NewBuild  (ctx, req, token)
if   err   !=   nil   {
log.  Fatal  (err)
}


var   buildErr   error
defer   build.  Finish  (buildErr)
```


## Acquire a Builder Machine


Next, we use the build ID and build token to request a new ephemeral builder machine. Here I'm requesting an arm64 machine, but amd64 is available as well.


```text
var   buildkit   *  machine  .  Machine
buildkit, buildErr   =   machine.  Acquire  (ctx, build.ID, build.Token,   "arm64"   /* or "amd64" */  )
if   buildErr   !=   nil   {
return
}
```


When Depot starts a container build machine, two things are happening behind the scenes. First, the machine is started with the project's isolated layer cache from previous builds for the build to use automatically. Second, we start our own optimized version of BuildKit on the machine to handle the container build.


The combination of the two allows you to build container images with all the performance of Depot, but still treat the entire build as if it was "just" BuildKit.


Because you're interacting directly with the underlying BuildKit in Depot, you need to remember to release the machine when the build completes. By default, machines stay on for two minutes after a build to handle additional builds.


```text
defer   buildkit.  Release  ()
```


## Connect to BuildKit


Once we have the BuildKit connection from the` Acquire` call, we connect to the machine's BuildKit using mutual TLS. Once connected we can use all the power of BuildKit in Depot. In the future I'll write a bit about other BuildKit patterns, but this example covers the majority of use cases. We'll build and push a simple container.


```text
var   buildkitClient   *  client  .  Client
buildkitClient, buildErr   =   buildkit.  Connect  (ctx)
if   buildErr   !=   nil   {
return
}
```


## Configure the Build


We create a BuildKit "solver" configuration. BuildKit thinks of builds as a graph of operations that are "solved."


```text
dockerfilePath   :=   "./Dockerfile"
workingDir   :=   "."
imageTag   :=   "goller/depot-example:latest"


solverOptions   :=   client  .  SolveOpt  {
Frontend:   "dockerfile.v0"  ,
FrontendAttrs:   map  [  string  ]  string  {
"filename"  : filepath.  Base  (dockerfilePath),
"platform"  :   "linux/arm64"  ,
},
LocalDirs:   map  [  string  ]  string  {
"dockerfile"  : filepath.  Dir  (dockerfilePath),
"context"  :    workingDir,
},
Exports: []  client  .  ExportEntry  {
{
Type:   "image"  ,
Attrs:   map  [  string  ]  string  {
"name"  :           imageTag,
"oci-mediatypes"  :   "true"  ,
"push"  :             "true"  ,
},
},
},
Session: []  session  .  Attachable  {
authprovider.  NewDockerAuthProvider  (config.  LoadDefaultConfigFile  (os.Stderr),   nil  ),
},
}
```


Here we want a "Dockerfile" solution. We provide BuildKit with the Dockerfile and the local working directory. The working directory contains your build context that you want your Dockerfile to build from (i.e. the files referenced by COPY commands).


We specify that we want to export the container as an image and that we want to push the image to a registry. Finally, an image push requires an "auth provider." This auth provider is the local credentials created by` docker login` .


## Stream Build Output


Next, we can run a go routine that prints the output of each step of the build.


```text
buildStatusCh   :=   make  (  chan   *  client  .  SolveStatus  ,   10  )
go   func  () {
enc   :=   json.  NewEncoder  (os.Stdout)
enc.  SetIndent  (  ""  ,   "  "  )
for   status   :=   range   buildStatusCh {
_   =   enc.  Encode  (status)
}
}()
```


BuildKit will return status messages for each step of the build. Here, I'm printing them as JSON.


## Build and Push the Container


Lastly, we ask BuildKit to "solve" our request to build and push the container. The build will reuse cached steps stored in the project cache.


```text
_, buildErr   =   buildkitClient.  Solve  (ctx,   nil  , solverOptions, buildStatusCh)
if   buildErr   !=   nil   {
return
}
```


When it finishes, it will have built and pushed the container.


[Scrolled all the way here without trying it? That’s bold. Start building with Depot →](https://depot.dev/start)


## The fastest way to build and push containers from Go


With Depot, you can build and push container images directly from Go code. No Docker CLI. No CI boilerplate. No messing with your own runners. Just fast, native builds powered by Depot's API.


It's as simple as:


```text
// 1. Create a new build
build, _   :=   build.  NewBuild  (ctx, req, token)
// 2. Acquire a remote builder
buildkit, _   :=   machine.  Acquire  (ctx, build.ID, build.Token,   "arm64"  )
// 3. Connect to BuildKit
client, _   :=   buildkit.  Connect  (ctx)
// 4. Build and push the container
client.  Solve  (ctx,   nil  , solverOptions, buildStatusCh)
```


Once your project and token are set up, you can build production-ready containers from your codebase in seconds. Everything is automated, cache-aware, and easy to plug into dev tools, CI flows, or internal platforms.


If you're ready to ship faster and control your builds from real code,[get started with Depot](https://depot.dev/sign-up) .


## Related posts


- [Accelerating builds: Improving EC2 boot time from 4s to 2.8s](https://depot.dev/blog/accelerating-builds-improve-ec2-boot-time)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Chris Goller


Principal Software Engineer at Depot
