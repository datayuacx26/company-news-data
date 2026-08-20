---
schema_version: "1.0.0"
document_id: "57c84dc268f12fcb4e0ef645c6450930d43138947b9670b05c884ae659b08fcd"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2019-01-25-nomadourexperiencesandbestpractices/"
published_at: "2019-01-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:e02db76494a0c6183e0417e743ba27279d21f9781ee1b19e0c3b09ea5ebd3f67"
---

# Nomad - our experiences and best practices

### How we migrated our monitoring stack to the cloud


Hello from trivago’s performance & monitoring team. One important part of our job is to ship more than a terabyte of logs and system metrics per day, from various data sources into elasticsearch, several time series databases and other data sinks. We do so by reading most of the data from multiple Kafka clusters and processing them with nearly 100 Logstashes. Our clusters currently consists of ~30 machines running Debian 7 with bare-metal installations of the aforementioned services. This summer we decided to migrate all of this to an on-premise \[Nomad\]([https://www.nomadproject.io/](https://www.nomadproject.io/) cluster) cluster.


## It’s not hard to say goodbye


Discussions about whether or not to put our systems into a private cloud have been going on for a while in our team. There is something to be said for isolation of services, faster deployment and having a centralized tool for service maintenance. Practical considerations, on the other hand, forbade us from migrating solely for the purpose of technical beauty and fancy tools. We had written our own cluster management scripts after all, and our bare-metal installations served us very well for years. The decision to set up a private cloud infrastructure came on a late August day when it turned out that one of our services had made changes to its installation scripts that had side effects on other services on the same machine. Automated updates were no longer possible, and the desire for isolation of services became a top priority.


## Why Nomad?


… and not[Kubernetes](https://kubernetes.io/)


Simple pragmatism. Our internal cloud service team already runs a large Nomad infrastructure, so we could profit from their experience and tools. Other teams had given Kubernetes a try and found it harder to maintain on-premise. For our team and its technical needs, the additional functionality that Kubernetes had to offer was not worth the extra efforts and human resources required to keep it running. Remote cloud solutions like a managed Kubernetes cluster or[ECS](https://aws.amazon.com/ecs/) are not an option for our I/O intense jobs either. On top of that, we wanted to have our logging platform under our control and not pay for data ingestion with third-party providers.


## Tooling


The goals for our cloud project were these:


- Ability to handle our stateful and our stateless services
- Location on-premise
- Isolation between services
- Jobs can be locally executed and tested
- Easy to start new services
- Secure secrets handling


We selected the following technologies:


- [Docker](https://www.docker.com/) for containerization
- [Nomad](https://www.nomadproject.io/) for orchestration
- [Make](https://www.gnu.org/software/make) for providing a unified interface for common tasks
- [Vault](https://www.vaultproject.io/) for secret management
- [Artifactory](https://jfrog.com/artifactory/) for Docker image storage


Additionally, we’re using[hclfmt](https://github.com/fatih/hclfmt) , a linter for Nomad job files, which keeps our Nomad files neat and well-structured.


We’re also using[envsubst](https://www.gnu.org/software/gettext/manual/html_node/envsubst-Invocation.html) , which is a tool for replacing placeholders in files with the values of environment variables. In our case, those files are templates for Nomad jobs or Dockerfiles, which require the injection of job specific information such as resource allocation, runtime flags and Docker build parameters.


## Cluster separation: Stateless versus Stateful


We have two types of jobs: stateless ones, which can be run anywhere on our Nomad machines, and stateful ones, which require some extra attention. Hence we have divided our cluster into two - one for each job type.


The stateless cluster runs jobs like[Logstash](https://www.elastic.co/products/logstash) .


### The stateful cluster


Everything which looks like a database, talks like a database and walks like a database, goes into the cluster for stateful jobs. Things are a bit different here. The database systems require persistent storage and their Nomad jobs are pinned to individual Nomad client machines on which they mount their data directories permanently. Those jobs cannot float around freely in the cluster and be deployed wherever Nomad sees fit. One might wonder, if this kind of defeats the purpose of using Nomad, but we decided to stick with it for the sake of consistency in the deployment process, and we love it. Examples of jobs in this cluster are Elasticsearch data nodes, \[InfluxDB\]([https://www.influxdata.com/](https://www.influxdata.com/) dashboard) service, and[Grafana](https://grafana.com/) which has a small internal database for dashboard storage and user management.


## Our Nomad conventions


Our team has declared a handful of rules that all of us stick to when setting up new Nomad jobs:


1.


We use` make` for everything. It builds our Docker images, it runs our Nomad deployments, it pulls our logs from Nomad. Every interaction with Nomad or Artifactory or any other system involved goes through a Makefile.


2.


All Makefiles need to enforce the automatic linting of the Nomad job file, using hclfmt.


3.


Secrets go into Vault.


4.


If the software in the image requires passwords, Nomad reads them from Vault at runtime and places them as files inside the container.


Credentials that Nomad itself needs can be placed as Vault placeholders directly in the Nomad job file.


1. The most important convention: every Nomad project needs to provide some predefined make targets.


## make all the things


We love` make` . It is well suited for our needs and a lot of fun to work with. Picking up on the last bullet point above, all Nomad jobs need to have a Makefile with at least the following targets:


make build → docker build
make push → push image to artifactory
make nomad.job → render job file from template (if applicable)
make deploy → nomad run
make stop → nomad stop
make status → nomad status
make logs → nomad logs


This convention has served us well in the last months. It allows us to deploy and work with projects that we might not be familiar with, because every team member knows exactly which functionality to expect from any of our Nomad jobs. Onboarding and shifting responsibilities during vacations is a breeze now. No more wondering how to deploy an alien project or how to check its current status.


## Examples


Let’s go with a simple example. The following Makefile builds one of our InfluxDB images, pushes it to Artifactory and can then be used to tell Nomad to run the job.


```text
VERSION_TAG=$(shell git rev-parse --short HEAD)


.PHONY: build
build:


docker build -t artifactory:9090/perf/influxdb-foobar:$(VERSION_TAG) .


.PHONY: push
push: build


docker push artifactory:9090/perf/influxdb-foobar:$(VERSION_TAG)


.PHONY: nomad.job
nomad.job:
hclfmt -w nomad.job.tpl
export VERSION_TAG=$(VERSION_TAG) &&
envsubst < "nomad.job.tpl" > "nomad.job"


.PHONY: clean
clean:
@rm nomad.job


.PHONY: deploy
deploy: push nomad.job
nomad run -verbose nomad.job
make clean


.PHONY: stop
stop:
nomad stop perf-influxdb-foobar


.PHONY: status
status:
nomad status perf-influxdb-foobar


.PHONY: logs
logs:
nomad logs -f -job perf-influxdb-foobar


```


A simple` make deploy` will get you running. The respective Nomad job template for this particular job looks roughly like this:


```text
job "perf-influxdb-foobar" {
...
type: "service"


group "monitoring" {
count: 1


task "perf-influxdb-foobar" {
driver: "docker"


constraint {
attribute: "${attr.unique.hostname}"
value    : "machine-number-13"       # stateful jobs get pinned to a machine
}


...


config {
image     : "artifactory:9090/perf/influxdb-foobar:$VERSION_TAG"
force_pull: true


port_map: {
http: 8086
}


volumes: [
"/appdata/influxdb:/var/lib/influxdb", # stateful jobs have a data directory mounted on the host machine
]
}


resources {
network {
port "http" {
static: "8086" # all jobs bind their service ports to the default values
}
}
}
}
}
}
```


## Lessons learned


### The flops


There are a few minor things that we don’t love about Nomad.


You need to specify the resource requirements per job. This requires some experience and repeated adjustments. Give a job too much CPU and memory and Nomad cannot allocate any, or at least not many, other jobs on the same host. Give it not enough memory and you might find it dying with an OOM. It would be neat if Nomad had a way of calculating those resource needs on its own. One can dream.


Nomad doesn’t have support for auto-scaling. Yet. If you need this, right now, you might want to look at Kubernetes instead. We don’t, but we might in the future.


Be careful when writing passwords to Vault, and make sure you run a Vault version which has support for versioning. We lost passwords a couple of times by making small mistakes while setting other passwords.


### The incidents


Let’s talk outages.


#### The DNS incident


One day, we found ourselves without a running Consul / Linkerd service due to some technical issues in another department. All name resolution for our Nomad jobs was gone and our services were unreachable.


**Lesson learned** : do not use the Consul DNS for critical services. In our case, anything is considered critical if its unreachability causes a data loss, such as our database systems. For those jobs, we recommend to bind the service’s port in Docker and Nomad and talk to it directly over ip:port instead of relying on consul’s auto generated job URLs.


#### The wifi incident


Another incident happened when we were doing hardware changes in the datacenter and had to shut down and redeploy some jobs from within there. The wifi in our datacenter is not reliable in some corners of the building, so we normally tether up our laptops to our cell phones and use the normal internet. Imagine your delight when you see a lot of docker pulls on your local shell and you wonder why those run into I/O timeouts and it dawns on you that you are pulling all those Docker images over your private phone’s data volume. On top of that, even on the phone, the connection was so bad that it took us five attempts and way too long to get a single database back up running. That’s not something you want to experience when you are under time pressure.


**Lesson learned** : make docker builds on the local machines optional, such as for local testing, but have them be done on a server in the same datacenter for normal deployments. We’ll be using Jenkins for this in the future.


### The Tops


First of all, all our jobs have the same structure now. This makes them easier to understand and quickly accessible to outsiders.


Version upgrades are very easy. Change the version in your Dockerfile,` make build deploy` and you’re done.


We finally have some proper secret management, as opposed to having our passwords in the developer’s environment or on the servers.


Needing to specify and occasionally review the resource specifications for Nomad gives us a nice resource-usage overview as a small benefit.


Deployments are fast now, and a lot of fun. Make makes everything simple and we are very happy about our choice.


## What’s next?


Our migration process is anything but finished. A lot of services need to be migrated still, and with even more lessons learned down the road, there might be a second part to this article in the future. For now, our goals are to wrap up the migration and then


1. switch to Jenkins for building the Docker images
2. use Jenkins to run syntax checks and test cases before deployments
3. experiment with canary deployments (we’re not using those yet)
4. consider alternatives for Vault, such as[Blackbox](https://github.com/StackExchange/blackbox) .
5. we’re putting YAML files with meta information into the directory of each Nomad job and we’re maintaining an internal tool to automatically generate Markdown documentation from those.
