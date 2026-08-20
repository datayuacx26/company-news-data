---
schema_version: "1.0.0"
document_id: "2ace8c868e3ca97179945dc4500240f9778709b3e58bba1ccfaa58e7ad63bb3b"
company_key: "rackspace-technology-inc-common-stock"
company: "Rackspace Technology Inc."
source_id: "rackspace-technology-inc-common-stock-news-import-038771c82b17"
canonical_url: "https://spot.rackspace.com/blog/kind-kubernetes"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-28T05:17:18.529675+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:8370a34176fc5942e66766747ab821fea5ca86d2a9fc866627e7898850a8b71e"
---

# What Is kind in Kubernetes? The Complete Guide to Local Clusters with Docker

A developer wants to test a manifest or a Helm chart against a real Kubernetes cluster. Spinning up a cloud cluster costs money and takes minutes to provision. Waiting for a VM to boot burns even more time. kind solves this: it runs a real, conformant Kubernetes cluster locally, using Docker containers instead of virtual machines, ready in under a minute.


## What is kind? (Kubernetes IN Docker)


[kind](https://kind.sigs.k8s.io/) is a tool for running local Kubernetes clusters using Docker (or Podman, or nerdctl) containers as the "nodes." The name is literal: **K** ubernetes **IN** **D** ocker. Each node in the cluster is not a VM, it is a container, running a full Kubernetes control plane or worker components inside it.


kind was built primarily to test Kubernetes itself and is a CNCF-certified conformant Kubernetes installer, meaning it passes the same conformance suite as any production distribution. That conformance is what makes it trustworthy for local development and Continuous Integration: a kind cluster behaves like a real one because it has to pass the same tests a real one does.


Every node bootstraps through` kubeadm` , the same tool that stands up production clusters.


That conformance stops at the laptop, though. When a workload is ready to run somewhere real,[Rackspace Spot](https://spot.rackspace.com/ui/signin) is a managed Kubernetes option worth signing up for.


## How kind works


kind runs each Kubernetes node as a Docker container rather than a virtual machine. When it creates a cluster, it uses a Docker network to connect those node containers together, the same way any multi-container Docker application communicates. Each node container runs a purpose-built` kindest/node` image with systemd and the Kubernetes components already staged inside it, and` kubeadm` bootstraps the cluster the same way it would on bare metal or a VM.


Nodes follow a predictable naming convention: the control plane container is named` <cluster-name>-control-plane` , and worker containers are named` <cluster-name>-worker` ,` <cluster-name>-worker2` , and so on. Running` docker ps` against an active kind cluster proves the architecture directly:


```text
NAMES                IMAGE                  STATUS         PORTS
kind-control-plane   kindest/node:v1.36.1   Up 5 minutes   127.0.0.1:59339->6443/tcp
kind-worker          kindest/node:v1.36.1   Up 5 minutes
kind-worker2         kindest/node:v1.36.1   Up 5 minutes
```


Notice that only the control plane exposes a port to the host,` 6443` , the Kubernetes API server. The worker nodes expose nothing. This single detail explains the most common friction point kind users hit, and it gets its own section later.


The framing is mind-bending the first time it clicks: Kubernetes, a container orchestrator, running its own nodes as containers, orchestrated by Docker. It works because Kubernetes only needs a Linux environment and a container runtime to bootstrap onto, it does not care whether that environment is a bare-metal server, a VM, or another container.


## Why use kind? Key features and benefits


- **Multi-node and HA clusters** : kind supports multiple worker nodes and high-availability control planes, something most local Kubernetes tools do not handle as cleanly.
- **Build Kubernetes from source** : kind can build and run a Kubernetes release directly from source using` make` or a Docker-based build.
- **Cross-platform** : kind runs on Linux, macOS, and Windows, wherever Docker, Podman, or nerdctl runs.
- **CNCF-certified conformant** : it is real, standards-compliant Kubernetes.
- **Fast and self-contained** : the first cluster pulls a node image, while later clusters benefit from the cached image.
- **CI-native** : ephemeral clusters can spin up, run a test suite, and disappear.


## kind vs. minikube vs. Docker Desktop


All three tools answer the same question, "how do I get Kubernetes on my laptop," but they answer it differently enough that the choice matters.


kind minikube Docker Desktop


Node architecture Docker containers VM or its own Docker driver Single built-in node


Multi-node / HA support Yes Limited No


Startup speed Under a minute after first pull Slower, VM boot overhead Fast, always-on


Add-ons and dashboard Minimal Rich ecosystem (dashboard, ingress, metrics addons) None built in


Service exposure Port-forward, Ingress` minikube tunnel` , built-in LoadBalancer Direct localhost binding


Best fit CI pipelines, multi-node testing Exploration, batteries-included local dev Simplest single-node local cluster


Pick kind for fast, disposable, multi-node clusters, especially inside CI. Pick minikube when the add-on ecosystem and a dashboard matter more than raw speed. Pick Docker Desktop's built-in cluster when a single node is genuinely enough and simplicity wins over configurability.


## How to install kind


### Prerequisites


kind needs a container runtime, Docker, Podman, or nerdctl, and` kubectl` to talk to the cluster. Go is only required if installing through` go install` ; binary downloads and Homebrew need no Go toolchain.


‍


### Install on macOS


Homebrew is the fastest path:


```text
brew install kind
```


No Go toolchain or manual binary handling required. Confirm it worked:


```text
kind version


```


```text
kind v0  .32  .0   go1  .26  .3   darwin/arm64


```


A direct binary download also works for Apple Silicon:


```text
curl -Lo ./kind https:  //kind.sigs.k8s.io/dl/v0.32.0/kind-darwin-arm64chmod +x ./kindsudo mv ./kind /usr/local/bin/kind
```


‍


### Install on Windows


Chocolatey and winget both package kind:


```text
choco install kind
```


```text
winget install Kubernetes.kind


```


A direct binary download is also available from the[kind releases page](https://github.com/kubernetes-sigs/kind/releases) . Run kind's Docker requirement through WSL2; Docker Desktop's WSL2 backend is the officially recommended environment for kind on Windows.


‍


### Install on Linux


Download the binary directly:


```text
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.32.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/  local  /bin/kind
```


Or, with Go installed:


```text
go install sigs.k8s.io/kind@v0  .32  .0
```


With a recent Go and Docker already in place:


```text
go install sigs.k8s.io/kind@v0  .32  .0   && kind create cluster


```


## How to create your first cluster


### Create a cluster


```text
kind create cluster
```


Output:


```text
Creating cluster   "kind"   ...
• Ensuring node image (kindest/node:v1  .36  .1  ) 🖼  ...
✓ Ensuring node image (kindest/node:v1  .36  .1  ) 🖼
• Preparing nodes 📦   ...
✓ Preparing nodes 📦
• Writing configuration 📜  ...
✓ Writing configuration 📜
• Starting control-plane 🕹️  ...
✓ Starting control-plane 🕹️
• Installing CNI 🔌  ...
✓ Installing CNI 🔌
• Installing StorageClass 💾  ...
✓ Installing StorageClass 💾
Set   kubectl context to   "kind-kind"


You can now use your cluster   with  :


kubectl cluster-info --context kind-kind
```


Start to finish, including the one-time node image pull, that run took 43 seconds. Every cluster after the first is faster, since the node image is already cached locally.


Give the cluster a name instead of the default:


```text
kind create cluster --name mycluster


```


‍


### Verify and interact with the cluster


kind writes a new context directly into` ~/.kube/config` :


```text
kubectl cluster-info --context kind-kind


```


```text
Kubernetes control plane is running at https:  //127.0.0.1:59084
CoreDNS is running at https:  //127.0.0.1:59084/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```


```text
kubectl get nodes


```


```text
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   10m   v1.36.1


```


List every kind cluster running on the machine, and every node within a named one:


```text
kind get clusters
kind get nodes --name mycluster
```


‍


### Create a multi-node (or HA) cluster with a config file


A single node is fine for a quick check, but most real testing wants something closer to production topology. Define one with a config file:


```text
kind: Cluster
apiVersion  : kind.x-k8s.io/v1alpha4
nodes  :
- role: control-plane
- role: worker
- role: worker
```


Apply it:


```text
kind create cluster --config kind-config.yaml


```


Output:


```text
Creating cluster   "kind"   ...
• Ensuring node image (kindest/node:v1  .36  .1  ) 🖼  ...
✓ Ensuring node image (kindest/node:v1  .36  .1  ) 🖼
• Preparing nodes 📦 📦 📦   ...
✓ Preparing nodes 📦 📦 📦
• Writing configuration 📜  ...
✓ Writing configuration 📜
• Starting control-plane 🕹️  ...
✓ Starting control-plane 🕹️
• Installing CNI 🔌  ...
✓ Installing CNI 🔌
• Installing StorageClass 💾  ...
✓ Installing StorageClass 💾
• Joining worker nodes 🚜  ...
✓ Joining worker nodes 🚜
Set   kubectl context to   "kind-kind"
```


Confirm all three nodes:


```text
kubectl get nodes


```


```text
NAME                 STATUS   ROLES           AGE     VERSION
kind-control-plane   Ready    control-plane   5m45s   v1  .36  .1
kind-worker          Ready    <none>          5m31s   v1  .36  .1
kind-worker2         Ready    <none>          5m31s   v1  .36  .1
```


‍


### Delete a cluster


```text
kind   delete   cluster --name mycluster


```


```text
Deleting cluster   "mycluster"   ...
Deleted nodes: [  "mycluster-control-plane"  ]


```


## Accessing services running in a kind cluster


By default, kind only exposes the Kubernetes API server to the host. A service running inside the cluster is not reachable at` localhost` without an extra step.


The simplest option is` kubectl port-forward` :` ‍`


```text
kubectl create deployment demo-nginx --image=nginx:alpine
kubectl expose deployment demo-nginx --port=  80
kubectl port-forward svc/demo-nginx   8080  :  80
```


```text
Forwarding   from     127.0  .0  .1  :  8080   ->   80
Forwarding   from   [::  1  ]:  8080   ->   80
```


```text
curl -o /dev/null -w   "HTTP status: %{http_code}\n"   http://localhost:8080
HTTP status: 200
```


That confirms the whole chain works: Docker container node, to Kubernetes service, to pod, to an HTTP response on the host.


Other options include` kubectl proxy` , an Ingress controller with` extraPortMappings` , and Cloud Provider KIND for` LoadBalancer` service support.


## Common use cases for kind


- **Local development** : testing a manifest, Helm chart, or operator against a real API server.
- **CI/CD pipelines** : create a disposable cluster, run integration tests, and tear it down.
- **Kubernetes conformance and version testing** : the reason kind exists in the first place.
- **Testing operators and CRDs** :` kind load docker-image` loads a locally built image directly into the cluster.
- **Learning Kubernetes without cloud cost** : a real, conformant cluster with no cloud bill attached.


The CI use case has real numbers behind it. Linkerd's project reported cutting its CI pipeline from roughly 45 minutes down to under 10 by running tests against parallel kind clusters.


Loading a freshly built, single-platform image works cleanly:


```text
kind load docker-image local-test-image:v2


```


```text
Image:   "local-test-image:v2"     with   ID   "sha256:bf79859b2..."   not yet present on node   "kind-control-plane"  , loading...
Image  :   "local-test-image:v2"     with   ID   "sha256:bf79859b2..."   not yet present on node   "kind-worker"  , loading...
Image  :   "local-test-image:v2"     with   ID   "sha256:bf79859b2..."   not yet present on node   "kind-worker2"  , loading...
```


But loading an image that was` docker pull` -ed as a multi-platform manifest and then re-tagged failed in the same test session with a digest lookup error. Building locally or pulling a single-platform image avoids that issue.


## kind's limitations and when you need production Kubernetes


kind is not designed for production. Its limitations are structural:


- **Ephemeral by design** : clusters live with the Docker environment they run on.
- **Tied to a single host** : every node is a container on one Docker daemon.
- **No durable storage or real load balancing out of the box** : production-grade persistence and load balancing require other infrastructure.
- **No cross-machine HA** : multiple control-plane containers are still on one physical host.


Once a team outgrows local testing, the question becomes where the workload actually runs day to day.[Rackspace Spot](https://spot.rackspace.com/) runs production Kubernetes clusters on an open-market auction model, with bids starting as low as $0.001 per hour. Workloads that make sense for kind locally, including CI jobs and batch processing, are also natural candidates for Rackspace Spot.


## Troubleshooting common kind issues


- **Cluster won't start** : confirm Docker is running with` docker ps` and has enough resources.
- **` kind: command not found` after` go install`** : add` $GOPATH/bin` or` $HOME/go/bin` to` $PATH` .
- **Can't reach a service** : expected by default; only the API server is exposed.
- **` kind load docker-image` digest errors** : build the image locally or pull a single-platform image explicitly.
- **Version mismatches** : pin the node image with` --image kindest/node:v<version>` .
- **Port conflicts** : check with` lsof -i :<port>` and remap the host port if needed.
- **Increasing node resources** : raise Docker's resource allocation.


kind's[Known Issues page](https://kind.sigs.k8s.io/docs/user/known-issues/) tracks platform-specific problems.


## Quick reference: essential kind commands


```text
kind create cluster                            # default cluster, named "kind"
kind create cluster --name mycluster           # named cluster
kind create cluster --config kind-config.yaml   # multi-node, from a config file
kind get clusters                              # list all kind clusters
kind get nodes --name mycluster                # list nodes in a named cluster
kind delete cluster --name mycluster           # delete a named cluster
kind load docker-image <image>:<tag>           # load a local image, no registry needed
kind build node-image                          # build a node image from Kubernetes source
kubectl cluster-info --context kind-<name>     # verify a cluster is reachable
```


## Frequently asked questions


### What is a kind in Kubernetes?


kind most commonly refers to the tool for running local Kubernetes clusters inside Docker containers, Kubernetes IN Docker. The same word also names the` kind:` field in every Kubernetes YAML manifest.


‍


### What does kind stand for in Kubernetes?


Kubernetes IN Docker.


‍


### What is the kind tool for Kubernetes?


A CNCF-certified conformant tool that runs a full local Kubernetes cluster using Docker containers as nodes instead of virtual machines.


‍


### What is the difference between kind and minikube?


kind favors fast, disposable, multi-node clusters, especially in CI. minikube offers a richer add-on ecosystem at the cost of more overhead.


‍


### Is kind suitable for production Kubernetes?


No. kind clusters are ephemeral, tied to a single Docker host, and lack durable storage or real load balancing without extra setup. For production workloads, look into managed Kubernetes offerings like[Rackspace Spot](https://spot.rackspace.com/) , which runs clusters on an open-market auction model with bids starting as low as $0.001 per hour


‍


### How do I install kind on Windows or macOS?


On macOS, use` brew install kind` or a direct binary download. On Windows, use` choco install kind` ,` winget install Kubernetes.kind` , or a direct binary download.


‍


### Can I run a multi-node cluster with kind?


Yes. Define the node roles in a config file and pass it to` kind create cluster --config` .


‍


### How do I increase resources for a node in a kind cluster?


Raise Docker's own resource allocation. kind does not have a separate per-node resource setting.
