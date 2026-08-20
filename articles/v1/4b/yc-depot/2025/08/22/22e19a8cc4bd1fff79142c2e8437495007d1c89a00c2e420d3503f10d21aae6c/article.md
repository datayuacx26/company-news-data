---
schema_version: "1.0.0"
document_id: "22e19a8cc4bd1fff79142c2e8437495007d1c89a00c2e420d3503f10d21aae6c"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/from-go-code-to-container-image-with-depot-api"
published_at: "2025-08-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:a1c8145680d1c232fc770ed2e593c08d083891c85c272d0a9f98674bfa6350df"
---

# From Go code to container image with Depot API

In this post, we're going to create a more specialized container building system using the lower-level[Depot API](https://depot.dev/docs/api/overview) Go SDK. This is a more advanced use case that builds on our previous[blog post](https://depot.dev/blog/go-code-to-container-depot-api) . Here, we are going to create a tool that gets tar content and builds a container.


## How Depot's build flow works


With the Depot API, there are three steps to build a container.


1. Register a build with Depot.
2. Acquire a Depot build machine.
3. Configure, build, and push a container.


## Before you start


Before you start, ensure you have a[Depot project set up](https://depot.dev/docs/container-builds/overview#projects) and an[organization API token](https://depot.dev/docs/api/authentication) generated.


## Install the Go package


To start, let's add these imports and run **go mod tidy** :


```text
import   (
"  context  "
"  encoding/json  "
"  log  "
"  os  "
"  time  "


"  github.com/depot/depot-go/build  "
"  github.com/depot/depot-go/machine  "
cliv1   "  github.com/depot/depot-go/proto/depot/cli/v1  "
"  github.com/moby/buildkit/client  "
"  github.com/moby/buildkit/session  "
"  github.com/moby/buildkit/session/upload/uploadprovider  "
)
```


## Register a build


First, we request a new build with the Depot API. This request associates a build with a specific project's isolated cache. When registered, the Depot API will respond with the new build's ID and a one-time build token. We defer reporting the build result, buildErr, to the API. Any non-nil error is a build failure.


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


## Acquire a builder machine


Next, we use the build ID and build token to request a new ephemeral builder machine.


```text
var   buildkit   *  machine  .  Machine
buildkit, buildErr   =   machine.  Acquire  (ctx, build.ID, build.Token,   "arm64"  )
if   buildErr   !=   nil   {
return
}
```


Since you're interacting directly with the underlying BuildKit in Depot, you must remember to release the machine when the build completes. By default, machines stay on for two minutes after a build to handle additional builds.


```text
defer   buildkit.  Release  ()
```


## Connect to BuildKit


Once we have the BuildKit connection from the` Acquire` call, we connect to the machine’s BuildKit using mutual TLS. Once connected we can use all the power of BuildKit in Depot.


```text
var   buildkitClient   *  client  .  Client
buildkitClient, buildErr   =   buildkit.  Connect  (ctx)
if   buildErr   !=   nil   {
return
}
```


## Configure the build


We create a BuildKit solver configuration. BuildKit conceptualizes builds as a graph of operations that are solved. In this case, we are going to configure the solver to receive a tar that contains the Dockerfile and all the needed content to run the build.


```text
/*
* howdy.tar.gz is a compressed tar archive that contains the Dockerfile and
* any other files needed to build the image.
*/
r, _   :=   os.  Open  (  "howdy.tar.gz"  )


uploader   :=   uploadprovider.  New  ()
// Special buildkit URL for HTTP over gRPC over gRPC.
contextURL   :=   uploader.  Add  (r)


solverOptions   :=   client  .  SolveOpt  {
Frontend:   "dockerfile.v0"  ,
FrontendAttrs:   map  [  string  ]  string  {
"platform"  :   "linux/arm64"  ,
"context"  :  contextURL,   // The tar file
},
Session: []  session  .  Attachable  {
uploader,
},
}
```


We provide BuildKit with a session attachable uploader. We need to explain BuildKit's rather unique solutions to client/server communication. The reasons for this design choice are unclear. The "what" is a method for a server to request content from a client.


An attachable is a BuildKit concept for a gRPC service hosted on the ***client*** on top of a bi-directional gRPC stream. This client-hosted service can be called by the server. In other words, the server tunnels gRPC requests back to the client.


BuildKit's uploadprovider takes this tunneling approach further by layering HTTP/1.1 requests on top of the gRPC-over-gRPC connection. As you can imagine, this layering of networking can be brittle to network drops and thus build retries are not uncommon.


```text
┌────────────────┐                                                             ┌────────────────┐
│Buildkit Client │                                                             │ Buildkit Server│
└────────────────┘                                                             └────────────────┘
│                                                                              │
┌┴─┐   Layer 1: Client calls Server Session Service                          ┌──┤
│  │───(Bidirectional gRPC)────────────────────────────────────────────────▶ │  │
│  │                                                                         │  │
│ ┌┴─┐                   Layer 2: Server calls Client hosted Upload Service  │  │
│ │  │ ◀────────────────────────────────────(Bi-Di gRPC tunneled over gRPC)──│  │
│ │  │                                                                       │  │
│ │  │                                      Layer 3: Server asks for upload  │  │
│ │ ┌┴─┐ ◀───────────────────────────────────(HTTP/1.1 over gRPC over gRPC)──│  │
│ │ │  │                                                                     │  │
│ │ │  │ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ▶ │  │
│ │ └┬─┘                                                                     │  │
│ │  │─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─▶ │  │
│ └┬─┘                                                                       │  │
│  │ ◀ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ └─┬┘
└┬─┘                                                                           │
│                                                                             │
▼                                                                             ▼
```


Nevertheless, we serve the tar file from the client to the BuildKit server over this protocol.


## Stream build output


Next, we can run a goroutine that prints the output of each step of the build.


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


BuildKit will return status messages for each step of the build. Here, we're printing them as JSON to stdout.


## Build the container


Lastly, we ask BuildKit to solve our request to build the container. The build will reuse cached steps stored in the project cache.


```text
_, buildErr   =   buildkitClient.  Solve  (ctx,   nil  , solverOptions, buildStatusCh)
if   buildErr   !=   nil   {
return
}
```


With this example, you could build a service that receives a tar and creates an image.


## Conclusion


With Depot's Go SDK, you can build sophisticated container building services without managing BuildKit infrastructure yourself. This approach gives you the performance benefits of Depot's optimized build machines—including native ARM64 support, persistent caches, and sub-second machine provisioning—while maintaining full control over the build process.


The pattern we've explored here powers many production services that need programmatic container builds: CI/CD platforms, development environments, and SaaS applications that containerize user code. By leveraging Depot's API, you get enterprise-grade build performance without the operational overhead.


Ready to get started? Generate an[organization API token](https://depot.dev/docs/api/authentication) and explore our[API documentation](https://depot.dev/docs/api/overview) to begin building your own container services.


## FAQ


What is Depot's Go SDK?


Depot's Go SDK provides low-level access to BuildKit infrastructure for programmatic container builds. It allows you to build Docker images from your applications without managing BuildKit servers yourself.


When should I use the API instead of the CLI?


Use Depot's API when you need to build container images programmatically, such as in SaaS platforms that package user code or CI/CD systems that need custom build logic. The CLI is better for interactive development workflows.


Do I need to manage BuildKit infrastructure?


No, Depot handles all BuildKit infrastructure including machine provisioning, scaling, and maintenance. You get ephemeral build machines that spin up in seconds and automatically clean up after builds.


How does caching work with the API?


Each Depot project gets isolated cache storage that persists across builds. The BuildKit solver automatically reuses cached layers, dramatically speeding up subsequent builds with similar content.


## Related posts


- [Now available: Depot API](https://depot.dev/blog/depot-api)
- [How to use BuildKit cache mounts in CI](https://depot.dev/blog/how-to-use-buildkit-cache-mounts-in-ci)
- [BuildKit in-depth: Docker's build engine explained](https://depot.dev/blog/buildkit-in-depth)


Chris Goller


Principal Software Engineer at Depot
