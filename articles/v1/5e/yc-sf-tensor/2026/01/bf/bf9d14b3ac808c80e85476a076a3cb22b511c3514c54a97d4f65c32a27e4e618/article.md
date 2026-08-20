---
schema_version: "1.0.0"
document_id: "bf9d14b3ac808c80e85476a076a3cb22b511c3514c54a97d4f65c32a27e4e618"
company_key: "yc-sf-tensor"
company: "SF Tensor"
source_id: "yc-sf-tensor-news-import-73b0de6f3d55"
canonical_url: "https://sf-tensor.com/engineering/infrastructure-tack"
published_at: "2026-01-15T00:00:00+00:00"
first_seen_at: "2026-07-27T05:05:23.755525+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:408e003b4dbf708f0a61dd12d232bd3102bbc77d2b9132cdbd200fcd7fde1fb6"
---

# We Got Sick of "Works on My Machine"

Local development gets complicated when you have multiple services that depend on each other.


At SF Tensor, we have numerous services that depend on each other. The cloud backend downloads code from GitHub and stores it in a bucket. The optimizer pulls from that bucket, processes it, and writes results back. The scheduler reads that state when spinning up training environments.


When you try to run this locally, you end up with` if (process.env.NODE_ENV === 'development')` scattered through your codebase. You mock S3 calls that don't actually test your S3 code. You write path handling that works against your filesystem but breaks against object storage.


We've all shipped code that passed every local test and then broke in staging because S3 returns keys differently than` fs.readdir` .


The usual solutions each have problems. Docker Compose doesn't match Kubernetes, so production bugs don't reproduce locally. Remote development clusters add latency, require a stable internet connection and explode costs. Mocking everything means you're testing different code than what you ship. And maintaining separate code paths for local versus production guarantees they'll drift apart.


So we built Tack.


## The Core Idea


Environment differences should live in the infrastructure layer, not your application code.


Your code shouldn't know whether it's talking to MinIO or S3. It shouldn't care if the database is a local container or RDS. Write once, run anywhere.


Tack is a Pulumi-based framework that gives you polymorphic resources. A bucket is a bucket. Locally, it's MinIO running in Minikube. In production, it's S3 with IAM roles. The interface is identical.


```text
typescript  const   dataBucket  =    createBucket  (  {
name :    "training-data"
}  )


```


On your laptop, that's MinIO with access keys injected as environment variables. On AWS, it's S3 with IRSA. No credentials in your code. The application sees the same interface either way.


The same pattern works for databases, secrets, queues, and load balancers. Tack's` Resource<ProductionBacking, LocalBacking>` abstraction means you define what you need once, and the infrastructure layer figures out how to provide it.


## Four Stacks, Same Code


We run four stack types, all executing the same Pulumi code:


**Development** has hot-reloading. Applications run as DevPods with file synchronization, port forwarding, and real-time logs. This is where we spend most of our time.


**Local-staging** uses the same local infrastructure (Minikube, MinIO, in-cluster Postgres) but deploys as regular Kubernetes Deployments instead of DevPods. Useful when we want to test production-like behavior without development tooling.


**Staging** is full AWS infrastructure with smaller instance sizes. EKS, S3, RDS. The real thing, just scaled down.


**Production** is staging with larger instances, multi-AZ databases, deletion protection, and stricter backup policies.


The application code running in all four is identical. Only the infrastructure differs.


## DevPods


Docker Compose doesn't match Kubernetes. Your local environment is different from production, so production bugs often don't reproduce locally.


DevPods move your development environment into Minikube. Hot reloading, file sync, real-time logs, but running inside the same orchestration layer you deploy to. Your machine syncs files and displays output. The execution happens in Kubernetes.


How it works:


1. A control server runs inside the pod, managing the dev server lifecycle
2. A client on your machine watches files and syncs changes via HTTP
3. Server-Sent Events stream logs back to your terminal


When you save a file, it's in the container within 100ms. The dev server hot-reloads. No rebuilding containers. No waiting for images to push and pull.


The control server handles graceful process shutdown, automatic` bun install` when` package.json` changes, log aggregation, and health checks for Kubernetes probes. It's about 600 lines of TypeScript.


## Local Domains


One annoying difference between local and production is URLs. Locally you get` http://localhost:3000` , but production has` https://app.example.com` . This causes real problems with cookie domains, CORS, OAuth redirects, and relative URLs.


We use Minikube's ingress-dns addon. Add a resolver file to` /etc/resolver/` , and` elastic-cloud.sft` resolves to your Minikube cluster. The nginx ingress controller routes traffic like ALB does in production.


```text
typescript  const   lb  =    createLoadBalancer  (  {
name :    "main-lb"  ,
cluster ,
healthCheckPath :    "/api/health"  ,
rules :    [  {
host :    isLocalStack  (  currentStack )    ?    "elastic-cloud.sft"    :    "cloud.sf-tensor.com"  ,
routes :    [
{   path :    "/api"  ,   service :   api .  service .  metadata .  name ,   port :    4000    }  ,
{   path :    "/"  ,   service :   frontend .  service .  metadata .  name ,   port :    3000    }
]
}  ]
}  )  ;


```


The only difference is the hostname. Path-based routing, health checks, and service resolution all work identically.


## What We're Open-Sourcing


**Tack** is the Pulumi framework for polymorphic infrastructure. Buckets, databases, clusters, and secrets with identical interfaces across local and production stacks.


```text
typescript  const   instanceType  =    stackSwitch  (
{   production :    "m6gd.xlarge"  ,   staging :    "m6gd.large"    }  ,
"invalid_type"
)  ;


```


That` invalid_type` default is intentional. If someone accidentally runs` pulumi up` on a development stack with AWS credentials, it fails fast instead of spinning up resources.


**DevPod Server** is the control server that runs inside your pods. File uploads with path traversal protection, process lifecycle management, automatic dependency installation, log streaming.


**DevPod Client** is the local watcher that syncs your code and manages port forwarding. Debounced file batching, automatic reconnection, deterministic port allocation.


## Results


Before Tack, we had conditional logic everywhere.` isLocal` checks in service code. Separate configuration files for each environment. Mock implementations that drifted from reality.


We had a bug during a training run where code worked locally but failed in staging. The local storage service correctly dropped double file extensions, but the S3 service didn't. We couldn't reproduce it locally because the mock worked fine. We only found it when we tried manually downloading files from the bucket and noticing they had wrong extensions.


After migrating:


We deleted over 5,000 lines of code. No more` if (isLocal)` . No more mock implementations. No more environment-specific configuration files.


If something breaks in staging, it breaks locally. Same bucket interface means same path handling.


New developers can get a full development environment running in about 8 minutes.` git clone` , run the setup script,` pulumi up` . All services running, talking to each other, using real infrastructure.


## How It Works In Practice


Our training pipeline receives code, validates it, and stores it:


```text
typescript  await   s3 .  putObject  (  {
Bucket :   process .  env .  DATA_BUCKET  ,
Key :    `  submissions/  ${  jobId }   /source.tar.gz  `   ,
Body :   archive
}  )  ;


```


This code runs identically in development and production.` DATA_BUCKET` points to MinIO locally, S3 in AWS. The application doesn't branch or check.


The optimizer pulls from the same bucket:


```text
typescript  const    {   Body  }    =    await   s3 .  getObject  (  {
Bucket :   process .  env .  DATA_BUCKET  ,
Key :    `  submissions/  ${  jobId }   /source.tar.gz  `
}  )  ;


```


Same interface. Same code. Different backing implementation.


The infrastructure decides what "bucket" means:


```text
typescript  const   bucket  =    createBucket  (  {   name :    "training-data"    }  )  ;


// bucket.endpoint returns:
// "http://minio.default.svc:9000" locally
// "https://s3.us-east-1.amazonaws.com" in production


```


The application layer doesn't know which it's talking to.


## Tradeoffs


This approach has costs.


Minikube needs resources. We allocate 6 CPUs and 16GB RAM. Your laptop will feel it.


Developers need basic Kubernetes literacy. They need to understand pods, services, and ingresses at a conceptual level.


Pulumi is a real dependency. Our infrastructure code is deeply integrated. If Pulumi disappeared, we'd have work to do.


First-time setup takes longer than Docker Compose. Getting Minikube, DNS resolution, and the tooling configured takes effort upfront.


For us, the tradeoffs are worth it. The velocity gains from identical environments, the confidence from testing what we ship, the hours saved debugging environment-specific issues.


## Getting Started


Check out our open source release of[tack](https://github.com/sf-tensor/tack) today.


If you've been fighting the same problems we were, we'd like to hear how it works for you.
