---
schema_version: "1.0.0"
document_id: "a548e204e09fdd1b86f2e3e6b28360ec6a1598ceff137565bbba81c99859a965"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/post/platform-engineering-at-weave/"
published_at: "2022-05-27T18:26:47+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:1a187ac9f7d14f5a056ec97432c39a1d72e9462b6fe444475a3175be831f647e"
---

# The State of Platform Engineering at Weave

# The State of Platform Engineering at Weave


Platform Engineering has become a major buzzword in the developer community over the last few years. It’s a term that is being thrown around in many ways and there are a lot of different opinions about what Platform Engineering actually is. It is a new-enough concept that it does not have a how-to guide similar to the[SRE Handbook](https://sre.google/sre-book/table-of-contents/) . As of yet, nothing has been published by leaders of the industry to help guide you through the process of building a platform engineering team.


At Weave, our platform engineering team has been around for several years and is known as the Developer Experience team, or DevX. I believe we are in a unique position, as a team that has been at this a long time, to showcase what has worked for us. We have been building, what I believe to be, a world-class Platform Engineering team and infrastructure. As the first member of our DevX team, and current Engineering Manager, I wanted to share some of our findings and the current state of Platform Engineering at Weave.


## A walk down memory lane


After AWS launched in 2006, cloud native engineering teams enjoyed many benefits of running in the cloud such as: better scalability, availability, security and observability to name a few. This also opened the door for a plethora of new tools to aid in the deployment of services. This lead to the birth of Site Reliability Engineers (a.k.a. SREs) who were responsible for maintaining these systems. The idea of DevOps was born a couple of years later as a way to build high performing teams where developers were the ones responsible for deploying, running, and managing their services end to end. The thought was that this would be more efficient and allow teams to have more control of their deployment pipeline instead of just “throwing it over the wall” to the Ops or SRE team. The issue with this is that most developers don’t have the knowledge or experience to do this.


What typically ends up happening is that you have every team doing things a little different. Ultimately, none of them are building expertise in the ops-side of their day-to-day because that is only **part** of their job. They also have to **actually build** the code they are shipping. This usually leads to embedding an SRE or someone with Ops experience on each team. Now, you have a bunch of teams with embedded SREs that are typically not all 100% aligned in how code is deployed. And, if you think about it, that’s not much different from how it was done before. You still have all your SREs managing these systems. It’s just done in a silo by the individual SREs on each team that are, most likely, duplicating work. Why not just have a central SRE team that can deduplicate their work and build better deployment systems for everyone?


This is typically where most cloud native engineering teams fall: a centralized SRE team that manages these systems that developers use for the deployment of their code. The SRE team manages things like Kubernetes, Helm, Terraform, Prometheus and other tools that help developers deploy, monitor, and manage their services. This, however, also presents a problem. Because these systems are all managed outside each engineering team, now your engineers have to learn all of these tools and systems to ensure when they deploy, they aren’t doing something that will cause issues with their deployments.


**Enter Platform Engineering.**


> Platform engineering is the discipline of building tooling and infrastructure that enables engineering teams to be self-sufficient while working faster and more efficiently.


This means that Platform Engineering teams bridge the gap between what SRE/Ops team manage and the entrypoint of those systems by developers. This **does not** mean that DevOps is dead. In fact, Platform Engineering exists to build a simple and sustainable way for developers to actually embrace the DevOps mindset by owning their services from inception to deployment and beyond. So, in my opinion, Platform Engineering enables DevOps to actually work. Developers still write the code, deploy the service, and monitor its performance. But, instead of requiring developers to do all of this by learning how these systems work, they are able to do it by using the Internal Developer Platform built by the Platform Engineering team. An Internal Developer Platform defines golden paths and enables developer self-service through infrastructure and tooling that reduces the cognitive load on developers.


## From SRE to DevOps to SRE to Platform Engineering 😵


Weave followed a similar journey. We initially had SysAdmins that handled the configuration and state of our on-prem hardware. We grew that model to include more traditional SREs that managed our Kubernetes installations, Grafana, Prometheus, Alert Manager, Jaeger, and our CI/CD pipeline, among other technologies. At this point we had not migrated into any cloud and were still running our hardware in a data center located near our office. For several years, the deployment of code to Kubernetes was done manually by each individual developer through Helm charts. All helm charts lived within each service’s repository and every developer had full access to our Kubernetes clusters 😬.


At this point, we had reached the state of having a centralized SRE team that managed the deployment and uptime of our Kubernetes clusters and all other infrastructure-related services. But, the individual developers were still responsible for maintaining their service’s Helm charts and making sure they were deployed correctly. Unfortunately, because most developers weren’t highly experienced in Kubernetes and Helm, this usually meant that when a new service was created, the developer simply found another service’s Helm chart, copied it into their new repository, changed the name of the chart, and deployed it. Obviously, this caused a number of issues. Most of the time, teams had no idea what their Helm charts were actually doing. And, because this was the normal process for deploying a new service, very few actually took the time to build an expertise in Kubernetes and Helm. This also meant that services were deployed in a many ways and there was no standardization around deployments.


About four years ago, this mentality changed. Our former CTO, Clint Berry, had a vision of standardizing the way services were deployed. And thus DevX was born.


## It all started with bart


` bart` is a CLI tool that all developers install as part of their onboarding process. It is a tool that helps automate and simplify tasks that developers do every day. It is the entrypoint for developers into the infrastructure that DevX builds and maintains.


Some features of` bart` include (typically in only one command):


- Tearing down old services
- Viewing build statuses
- Viewing cluster information
- Deploying services
- Introspection into a specific deployed service
- Initializing a new service
- Getting an API token
- Tailing service logs
- Port forwarding services
- Rolling back deployments
- Running a service locally
- Searching for documentation
- Managing secrets and shared configuration
- Setting up a machine’s local development environment
- Validation of a WAML™
- Full UI to interact with services
- The list could go on and on and on…


The first feature that` bart` included was a way to bootstrap a new application by generating the basic skeleton of a typical Weave service. Very shortly after bart’s incarnation, we wanted to standardize the way we deployed our services. We had the desire to abstract away all the Kubernetes concepts so that our developers had one less thing to worry about. We wanted to implement a manifest that described a service and how it was to be deployed. This would live in the root of each service’s repository and needed to be very simple and easily understood. By doing this, we could manage and standardize how Kubernetes manifests were rendered and ensure best practices and security measures were followed.


But, in that current state of things, every single repo had their own interpretation of how a service was supposed to be deployed. So, we needed to figure out all the different ways Helm charts were being used to ensure we could support them all. This was a very tall task. We had to comb through every deployment and curate a list of all possible Kubernetes concepts, configurations and deployment strategies. Once we had this list, we were able to construct the first version of the` .weave.yaml` , or the WAML™. We decided to build a command into` bart` that would parse a Helm chart, and convert it into a populated WAML™. A shortened version of a populated v1 WAML™ looked something like this:


```text
schema  :     "1"
name  :     my service
slug  :     my-service
owner  :     team-devx@getweave.com
slack  :     "#squad-devx"
namespace  :     devx
deploy  :
prod  :
env  :
-  name  :     MY_ENV_VAR
value  :     "my value"
ports  :
-  name  :     grpc
number  :     9000
service  :
-  name  :     my-service
ports  :
-  name  :     grpc
number  :     9000
ingress  :
-  host  :     www.my-service.com
public  :     true
resources  :
scaling  :
replicas  :     3


```


## One WAML™ to rule them all


Once we had a centralized manifest that defined a service, we could start to build tooling and infrastructure around it. The pattern we follow when architecting a new feature to be added to the WAML™ is to make sane defaults, but allow the customization of all parts of that new concept. Each of the following features that are built into the WAML™ could be a full blog post on their own, and probably will be. So, I will just summarize a few of the features that are built into the WAML™:


### Deployments


Once we had the WAML™ in place, we could start using it to generate Kubernetes manifests. We decided to use GitHub Deployments as the event stream for our deployments. We built a template and validation engine,` The Deployer` , that would respond to GitHub deployment events and generate the manifests that were consumed by ArgoCD and propagated to our Kubernetes clusters. By using GitHub as the event stream, we could deploy from virtually anywhere that had permissions to create GitHub deployments.` bart` was the first entrypoint.` bart deploy` was the first widely used command in` bart` and remains one of the most used commands in` bart` today. It simply packages up the WAML™ and attaches it as the payload in the creation of a new GitHub Deployment which is then consumed by` The Deployer` .


Conclusion:


> SRE manages the deployment and uptime of Kubernetes and ArgoCD while DevX manages the Kubernetes manifests and template generation and validation.


### Secret Management


We spun up Vault in each of our clusters and deployed a forked version of Banzai Cloud’s[bank vaults](https://github.com/banzaicloud/bank-vaults) that allowed us to inject secrets as environment variables at deployment time. This allowed us to build tooling into` bart` that enabled developers to easily add secrets into their services.` bart secret add` was added to the toolchain which simply creates pull requests that contain crypt text and when merged into our secrets repo, the decrypted value was pushed into Vault. This paired nicely with the WAML™. Including a simple environment variable that was scoped to the location of the secret in the Git repo, allowed bank vault’s` vault-env` to inject that secret into the deployment.


```text
env  :
-  name  :     MY_SECRET
value  :     "vault:my-secret#my-secret-key#1"
...


```


We were also able to expand this functionality to provide a concept of` secretMounts` that enables developers to mount all keys within a secret as files within a specific directory into the deployed container. Because DevX controls all metadata around these secrets, we are able to validate all secrets specified within a deployment and ensure they actually exist before allowing a deployment to go out.


Conclusion:


> SRE manages the deployment and uptime of Vault while DevX manages all metadata around secrets and how they are stored and consumed.


### Shared Configuration


Our shared configuration system models that of our secret management system. All values are stored in a Git repo that is watched by an init or sidecar container, that injects these values into the container at deployment time. To enable this, it’s a simple add to the WAML™.


```text
configMounts  :
-  source  :     config/path/within/git/repo
path  :     /path/to/mount/in/container
...


```


> SRE ensures the uptime of Kubernetes while DevX builds and manages these init or sidecar containers that provide this functionality.


### Monitoring/Alerting


Because DevX controls what is attached to a deployment, we can add default monitoring and alerting. For every deployment that goes out, we generate a URL to a fully customized Grafana dashboard that shows all metrics for that deployment. We also attach a list of default Prometheus alerts such as CPU throttling, container restarts, high memory usage, etc. that get routed to the appropriate owner as defined in the WAML™. In addition to the set of default alerts, we expose an easy-to-understand interface in the WAML™ that allows developers to add their own custom alerts.


```text
monitoring  :
rules  :
-  alert  :     MyAlertName
expr  :     my-promql-expr
annotations  :
description  :     My alert description
summary  :     My alert summary
...


```


> SRE manages the deployment and uptime of Prometheus, Grafana and AlertManager while DevX manages what alerts are consumed, how they are configured, as well as the easy discovery of these metrics.


### SLOs


Even complicated concepts like SLOs are extremely easy to implement as a developer. DevX built a set of predefined SLO rules that expand into the complicated and verbose Prometheus configuration that they need to be. This allows developers to gain access to SLOs for their services with a one liner in their WAML™.


```text
slo  :
requestClass  :     critical
...


```


We also expose the full data structure of how an SLO should be configured which allows developers to customize these predefined SLO request classes if they need to. In addition, we have added a form within the` bart` service UI that allows developers to easily fill out a form to populate the SLO configuration into the WAML™.


For a more in-depth overview of how we implemented automatic SLOs at scale for virtually no cost, see the presentation that our very own,[Carson Anderson](https://github.com/carsonoid) , gave at the Utah Kubernetes meetup[here](https://www.youtube.com/watch?v=sfwnr5K8oCs) .


> SRE manages the uptime of Prometheus while DevX manages the recording rules, their associated calculations and the interface to configure them.


### HPAs


Horizontal Pod Autoscaling (HPA) can be hard to grok for some engineers. DevX built a simple way for developers to implement autoscaling for their services. We currently support the ability to scale by CPU, NSQ topic/channel depth, and DB connection wait time, with more in the pipeline. Achieving this result is incredibly easy for our developers by specifying a few things in the WAML™.


```text
autoscaling  :
horizontal  :
minReplicas  :     3
maxReplicas  :     10
cpu  :
averageUtilization  :     75
db  :
maxConnectionWaitTime  :     1s
nsq  :
topic  :     MyTopic
maxDepth  :     100
...


```


> SRE manages the uptime of Prometheus and the health of our metrics server while DevX manages the HPA manifests that are generated, the custom metrics that are configured in our Prometheus adapter, and the interface to configure these.


### Feature Flags and Options


We expose the concept of feature flags and options in the WAML™. These allow us to add features into the WAML™ that we don’t want a concrete type for such as enabling auto-deploys on PR merges or changing the Slack alert channel for a specific environment.


```text
featureFlags  :
auto-deploy  :     true
options  :
slack-alert-channel  :     my-slack-channel


```


---


These are just a few of the things that we have been able to implement to make the lives of our developers easier. This doesn’t even cover: our centralized Schema registry that includes generated gRPC Gateway, gRPC client, OpenAPI documentation, and GoDocs, as well as automatic authentication through custom protoc plugins; our rollout strategies for Canary deployments and header-based routing with API integration tests through automated dependency discovery; our custom Slackbot to serve on-demand notifications for the various event streams that we manage; our media manager service that allows any of our software services to upload and fetch per-customer encrypted media files; or our system built to allow fully self-service local development in our environment of over 400 microservices.


We, DevX, get to build the coolest software and infrastructure. We get to design multi-service, complicated systems that help our engineers do their jobs faster, more efficiently, and with full confidence. And the WAML™ is at the center of it all.


## Curating a world-class Platform Engineering team


DevX has a unique hiring process compared to the rest of the Engineering department. We focus on things that best resemble a real-life work environment such as PR reviews. The ability to take constructive feedback and have a discussion around efficiencies and best practices is a huge part of our culture. We prioritize code reviews in our day-to-day so that we can ensure our code is clean, efficient, well-tested, and maintainable. This is why the first part of our hiring process is to do a code review of a take home assignment that we give each interviewee. We can assess their ability to have a productive discussion around why they made certain decisions in their code and how they could improve certain areas.


Weave’s original values were hungry, creative and caring, and I think these values embody the spirit of DevX.


Remaining hungry, and passionate, is essential to the culture of our DevX team. We are extremely passionate about the work that we get to do. It is very common to find members of DevX thinking about, and working on, the current problem they are trying to solve **outside** traditional working hours. While this is far from a requirement, it is a common theme of our team. We are constantly learning and improving our skills so that we can deliver the best possible platform for our developers.


Being creative is an absolute must for our team. We are tackling problems that don’t have clear solutions which is forcing us to constantly think outside the box. It’s not as easy as finding a blog post somewhere, or a Stack Overflow question, that illustrates how to solve the problem. We are required to come up with these solutions ourselves.


Caring about the people we serve is also a big one. We are responsible for the system that developers interact with every day, all day. So, it is imperative that we carry with us empathy for those we serve. Before building a new feature, we will often consult with the members of the Engineering team. We want to know what the best solution for **them** is, and how **they** will be using it. Everything we do is centered around helping our developers do their best work. So, caring about them is a huge part of our culture.


## So, Platform Engineering?


One of the members of our team mentioned to me that after he explains to his colleagues what his job entails, he is often given a followup question, “So, what do you do for your developers?”. He said that he has found it easier to explain what we prevent our developers from having to do than to try and paint a picture of the entire infrastructure we manage.


That is a core part of what Platform Engineering is: building tooling and infrastructure that automates the menial tasks of what developers are often required to do so that they can focus on what they do best, building software. It is bridging the gap of what SREs manage and developers build. We owe a lot to Weave’s co-founder Clint Berry. It was his initial vision that brought DevX, and Platform Engineering, to life at Weave.


If anyone is curious about how to build a killer Platform Engineering team and infrastructure, be on the lookout for our future talks about blog posts. Although we have been at this a long time, DevX is just getting started. Stay tuned for what we have in store. 💪
