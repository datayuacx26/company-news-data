---
schema_version: "1.0.0"
document_id: "ca79f0322317eafda19aed3abab65a494e179367d12211ec62b8a20862a26f20"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/container-security-at-scale-building-untrusted-images-safely"
published_at: "2025-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:3afd96b8ebff9c773b439fc8b2bb421b1ccc4a22300071307590ab319b1e34ec"
---

# Container security at scale: Building untrusted images safely

A lot of our customers run into the same problem: they need to run code on behalf of their customers. Whether you're hosting user-generated Python scripts, processing custom containers, or running code in isolated environments, you end up needing fast, reliable container builds that don't become a bottleneck.


Rather than managing all the container orchestration complexity in-house, many of our customers outsource the container building to us and use our API for the heavy lifting. In this post, we'll walk through how to use the Depot API to set up and administer isolated[project](https://depot.dev/docs/container-builds/overview#projects) cache, report build metrics, and get build logs for your customer workloads.


We'll use Go to build tooling that creates and manages container builds for a multi-tenant SaaS platform.


[Tired of managing build infrastructure? See how our API handles the complexity for you. Check the docs →](https://depot.dev/docs)


## Depot core API


The Depot core API uses[buf.build](https://buf.build/depot/api/docs/main:depot.core.v1) , so it supports both Connect and gRPC protocols.


Thanks to[Buf](https://buf.build/docs/bsr/generated-sdks/#supported-languages) , we can automatically generate client libraries for many languages. In this example, we'll use Go as the backend language, but Buf can be used in many other languages.


## Architecture overview


We'll build some tools to create isolated build environments for users. For a new user, we'll create a new project and a new project-scoped token. Next, we'll get container build metrics including durations. Finally, we'll retrieve the container's steps.


## Getting started with the Go Client


First, let's create a new go program.


```text
go   mod   init   github.com/depot/saas
```


Next, we'll add the Connect Depot API clients.


```text
go   get   connectrpc.com/connect
go   get   buf.build/gen/go/depot/api/connectrpc/go
go   get   buf.build/gen/go/depot/api/protocolbuffers/go
```


You can find the complete documentation for the Go client at the[Buf registry](https://buf.build/gen/doc/go/depot/api/connectrpc/go:main:v1.18.1-1/~/buf.build/gen/go/depot/api/connectrpc/go/depot/core/v1/corev1connect/) .


## Creating projects for customer isolation


We recommend mapping an individual user to a single Depot project. We'll build a simple command-line tool that creates projects.


```text
mkdir   -p   ./cmd/project
```


Add this to the file cmd/project/main.go:


```text
package   main


import   (
"  context  "
"  flag  "
"  fmt  "
"  log  "
"  net/http  "
"  os  "
"  time  "


"  buf.build/gen/go/depot/api/connectrpc/go/depot/core/v1/corev1connect  "
buildv1   "  buf.build/gen/go/depot/api/protocolbuffers/go/depot/build/v1  "
corev1   "  buf.build/gen/go/depot/api/protocolbuffers/go/depot/core/v1  "
"  connectrpc.com/connect  "
)


func   main  () {
var   customerName   string
flag.  StringVar  (  &  customerName,   "name"  ,   ""  ,   "customer project name"  )
flag.  Parse  ()


if   customerName   ==   ""   {
flag.  Usage  ()
return
}


depotToken   :=   os.  Getenv  (  "DEPOT_TOKEN"  )
if   depotToken   ==   ""   {
fmt.  Fprintln  (os.Stderr,   "DEPOT_TOKEN required"  )
return
}


ctx   :=   context.  Background  ()
id   :=   createProject  (ctx, customerName, depotToken)
fmt.  Printf  (  "Created Project ID %s for %s\n"  , id, customerName)
}


func   createProject  (  ctx   context  .  Context  ,   customerName  ,   depotToken   string  )   string   {
depotClient   :=   corev1connect.  NewProjectServiceClient  (
http.DefaultClient,
"https://api.depot.dev"  ,
)


hardware   :=   corev1.Hardware_HARDWARE_32X64
req   :=   connect.  NewRequest  (  &  corev1  .  CreateProjectRequest  {
Name:     customerName,
RegionId:   "eu-central-1"  ,   // or us-east-1
CachePolicy:   &  corev1  .  CachePolicy  {
KeepGb:   30  ,   // Keep 30 GB of cache
},
Hardware:   &  hardware,
})


req.  Header  ().  Add  (  "Authorization"  ,   "Bearer "  +  depotToken)
res, err   :=   depotClient.  CreateProject  (ctx, req)
if   err   !=   nil   {
log.  Fatal  (err)
}


project   :=   res.Msg.  GetProject  ()
return   project.ProjectId
}
```


This program creates a named project with a Depot API client. It expects the environment variable,` DEPOT_TOKEN` to be set to an[API token](https://depot.dev/docs/api/authentication#generating-an-api-token) . The token is used as a Bearer token. It sets the project's region to` eu-central-1` and gives the project a 30GB cache quota. Additionally, it uses a non-default larger builder sized machine with 32 CPUs and 64GB of RAM. Those values are all configurable to give flexibility in build performance.


Here is how to run the program and its unique project ID output:


```text
go   build   ./cmd/project   &&   ./project   -name   my_customer


Created   Project   ID   n9548n2qqx   for   my_customer
```


## Managing customer projects


Deleting projects removes all project cache and project tokens, preventing any further builds:


```text
func   deleteProject  (  ctx   context  .  Context  ,   projectID  ,   depotToken   string  )   error   {
depotClient   :=   corev1connect.  NewProjectServiceClient  (
http.DefaultClient,
"https://api.depot.dev"  ,
)


req   :=   connect.  NewRequest  (  &  corev1  .  DeleteProjectRequest  {
ProjectId: projectID,
})


req.  Header  ().  Add  (  "Authorization"  ,   "Bearer "  +  depotToken)
_, err   :=   depotClient.  DeleteProject  (ctx, req)
return   err
}
```


Similar to creating a project, we create a client and request with bearer auth.


While managing projects, it is very useful to be able reset a project's build cache in case a customer wishes to start fresh. Here is how to do so:


```text
func   resetProject  (  ctx   context  .  Context  ,   projectID  ,   depotToken   string  )   error   {
depotClient   :=   corev1connect.  NewProjectServiceClient  (
http.DefaultClient,
"https://api.depot.dev"  ,
)


req   :=   connect.  NewRequest  (  &  corev1  .  ResetProjectRequest  {
ProjectId: projectID,
})


req.  Header  ().  Add  (  "Authorization"  ,   "Bearer "  +  depotToken)
_, err   :=   depotClient.  ResetProject  (ctx, req)
return   err
}
```


When a project has been reset, its cache will be reset and all currently running jobs will be canceled.


## Getting build metrics for customer analytics


Ok, great, now that we can administer projects we can build containers for our customers. Check out the[blog](https://depot.dev/blog/go-code-to-container-depot-api) on how to build a container using a project id.


Let's assume several container builds have finished. We can list all those builds using the paginated` ListBuilds` API request. This example shows how to paginate through all of a project's builds. Likely, you'll need to add log when to stop paging when there are hundreds of builds.


Each build has an ID and some coarse timing metrics. The duration is the time it took the build to complete. The "saved duration" is the estimated time the Depot cache saved the customer for that step.


```text
func   listBuilds  (  ctx   context  .  Context  ,   projectID  ,   depotToken   string  ) {
depotClient   :=   corev1connect.  NewBuildServiceClient  (
http.DefaultClient,
"https://api.depot.dev"  ,
)
listBuilds   :=   &  corev1  .  ListBuildsRequest  {
ProjectId: projectID,
}


for   {
req   :=   connect.  NewRequest  (listBuilds)
req.  Header  ().  Add  (  "Authorization"  ,   "Bearer "  +  depotToken)


res, err   :=   depotClient.  ListBuilds  (ctx, req)
if   err   !=   nil   {
log.  Fatal  (err)
}


for   _, b   :=   range   res.Msg.  GetBuilds  () {
fmt.  Printf  (  "Build ID: %s\n"  , b.BuildId)
fmt.  Printf  (  "\tStatus: %s\n"  , b.Status)
fmt.  Printf  (  "\tCreated At: %s\n"  , b.CreatedAt.  AsTime  ())
fmt.  Printf  (  "\tBuild Duration: %s\n"  , time.  Duration  (b.  GetBuildDurationSeconds  ())  *  time.Second)
fmt.  Printf  (  "\tSaved Duration: %s\n"  , time.  Duration  (b.  GetSavedDurationSeconds  ())  *  time.Second)
fmt.  Printf  (  "\tCached Steps: %d\n"  , b.  GetCachedSteps  ())
fmt.  Printf  (  "\tTotal Steps: %d\n"  , b.  GetTotalSteps  ())
}


nextPageToken   :=   res.Msg.  GetNextPageToken  ()
if   nextPageToken   ==   ""   {
break
}


listBuilds.PageToken   =   &  nextPageToken
}
}
```


[Sick of slow customer builds killing your platform's performance? See how fast Depot can make them. Try it free →](https://depot.dev/sign-up)


## Getting detailed build steps


Each container build has multiple steps such as transferring build context and running programs. The Depot API also provides a breakdown of each step including its name, timings, and if the step errored or not. This is useful to visualize the entire container build process.


```text
func   buildSteps  (  ctx   context  .  Context  ,   projectID  ,   buildID  ,   depotToken   string  ) {
buildClient   :=   buildv1connect.  NewBuildServiceClient  (http.DefaultClient,   "https://api.depot.dev"  )
req   :=   connect.  NewRequest  (  &  buildv1  .  GetBuildStepsRequest  {
ProjectId: projectID,
BuildId:   buildID,
})
req.  Header  ().  Add  (  "Authorization"  ,   "Bearer "  +  depotToken)


res, err   :=   buildClient.  GetBuildSteps  (ctx, req)
if   err   !=   nil   {
log.  Fatal  (err)
}


for   _, step   :=   range   res.Msg.  GetBuildSteps  () {
fmt.  Printf  (  "Step Name: %s\n"  , step.Name)
fmt.  Printf  (  "\tStarted At: %s\n"  , step.StartedAt.  AsTime  ())
fmt.  Printf  (  "\tFinished At: %s\n"  , step.  GetCompletedAt  ().  AsTime  ())
fmt.  Printf  (  "\tHad Error: %t\n"  , step.  HasError  ())
}
}
```


## Building container infrastructure that scales


With these building blocks, you can create solid container infrastructure for your customers. The isolated projects ensure security and performance isolation, while Depot's caching dramatically reduces build times across your customer base.


This approach works well for platforms that need to:


- Execute customer code in isolated environments
- Provide fast feedback loops for development workflows
- Scale container builds without managing infrastructure complexity
- Offer detailed build analytics to customers


Whether you're building a platform that runs customer Python code, or any other service that needs to execute user-generated containers, Depot's API provides the performance and isolation you need without the operational overhead.


## Get started today


Ready to build container infrastructure for your customers?[Sign up for Depot](https://depot.dev/sign-up) and start with a 7-day free trial. Our Go client libraries make it easy to integrate Depot into your existing infrastructure.


Have questions about implementing customer container builds? Join our[Community Discord](https://discord.gg/XpTfcVrr46) to chat with our team and other developers building similar solutions.


## Related posts


- [Depot Build API: build Docker images as a service](https://depot.dev/blog/docker-build-api)
- [From Go Code to Container Image with Depot API](https://depot.dev/blog/go-code-to-container-depot-api)
- [Now available: Depot API](https://depot.dev/blog/depot-api)
- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)
- [How to speed up your Docker builds](https://depot.dev/blog/speed-up-docker-builds)


Chris Goller


Principal Software Engineer at Depot
