---
schema_version: "1.0.0"
document_id: "81c82e2a908042b0d0a51ff25cfb7a06931a2b6329a4bb6cfdf197ab9bcf23c6"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/microservices-test-isolation-with-resource-plugins/"
published_at: "2022-06-09T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:2fc46ddff931bc1a8bb64366c3e4539862301bbb656d25b3f4c354640d0bd641"
---

# Microservice Test Isolation with Resource Plugins

## Introduction


We think of microservices on Kubernetes as a collection of workloads which are more often than not *stateless* . REST and gRPC are the go-to protocols with which these workloads communicate, and Kubernetes Deployments and Argo Rollouts are the go-to workload lifecycle controllers. However, most real scenarios depend on persistent state and there is a large multiverse of associated tools. In practice most microservice testing scenarios are impractical without a clear mechanism for handling state and shared resources.


[Signadot Sandboxes](https://www.signadot.com/docs/concepts/sandbox) provide an efficient mechanism for managing development and testing lifecycles of workloads. They spin up in seconds, synchronise automatically with a reference baseline, and offer various forms of smart routing including context propagation, configuration based routing and integration with Istio. These workloads are typically *stateless* .


Until recently, they also did not play well with shared resources such as databases and message queues. For example, the figure below considers testing a service **A** in a sandbox which consumes a message queue **Q** . With no mechanism to handle the coordination of **Q** between **A** and the test version **A-test** any other service which depends on **A** will function as though **Q** simply dropped messages without explanation when in fact these messages were consumed by **A-test** .


Testing isolation when using message queues


This post presents Signadot Resource Plugins, a result of several design iterations targeting the problem of efficient and effective isolation for testing microservices in the presence of shared resources.


## Sandbox Resources Are Logical, Not Physical


Resource management in the context of sandboxes is quite distinct from general resource management, because sandboxes are ephemeral: they come and go and are often expected to spin up in seconds. The resources tied to sandboxes are thus ephemeral as well. At the same time, the idea of “resources” in this context can refer to almost anything. As a result, we have developed resource management for sandboxes using[a system of plugins](https://www.signadot.com/docs/concepts/resources) which abstracts resource management into a few operations for allocating or provisioning resources, making almost no assumptions about the resources themselves.


So resources can be almost anything. Let’s consider the case of working on a microservice **M** that consumes a database **D** , but the database is also consumed by many other microservices. In this case, working on a development version of **M** risks polluting **D** . Our[plugin repository](https://github.com/signadot/plugins) a MariaDB database plugin that could be used to isolate **D** within the test version of **M** . However, there are many possible alternatives as to how to achieve this isolation, each with its own tradeoffs.


Perhaps **M** only writes to one or a few tables in **D** . In this case, it could make sense to provision those tables as resources rather than the whole database if we are optimising for spin-up time. To consider even more lightweight solutions, if **M** only modifies information about a single test-user profile in **D** , we may consider that profile to be the resource allocated for the test version of **M** .


Signadot resources can provide this, as well as many other alternatives, because the plugin system and associated operator support considers resources as logical rather than physical entities.


## How Resource Plugins Work


Resource Plugins are fundamentally tied to Signadot Sandboxes. They provide a logical abstraction of an external entity which needs to be made available to a sandbox for its workloads to run. Here, we provide a breakdown of how we make this abstraction concrete.


### Lifecycle Management


#### Developer’s perspective


From a developer’s perspective, resource lifecycle management is dead simple. A request to create a sandbox includes a list of resources:


Each resource has a name, a plugin, and some key-value parameters to send to the plugin. The plugin value is a name of a resource plugin installed on the cluster. With such declarations in place, the resources will be available to the sandbox when it is running. From a developer’s perspective that’s all there is to it.


#### Operator


The Signadot operator defines and coordinates several CRDs in order to supply this functionality for the developer. It ensures that


- The sandbox workloads do not spin up until the resources have been provisioned.
- No resource is ever consumed by the sandbox once it has been de-provisioned.
- The resource plugin’s interface is executed with the parameters supplied in the sandbox creation request.
- The output of the resource plugin is securely made available to all workloads in the sandbox via Secrets.


### Plugin CRDs


The Plugin CRD describes the resource plugin, giving it a name and a specification for executing an image:


The Pod template spec is a standard Kubernetes Pod template, making the plugin easily adaptable to the cluster. For example, one can specify a service account or labels conforming to the host cluster configuration.


However, the pod spec in a resource plugin does not accept specification of command line arguments, because the image conforms to an interface with multiple entry points.


### Plugin Images


The image interface for a Resource plugin is simple. The Signadot operator assumes that


- There is an executable **/signadot/plugin/bin/provision** and likewise **/signadot/plugin/bin/deprovision** .
- These executables accept two arguments: a sandbox id and a resource name.
- The executables will exit with code 0 if provisioning (or deprovisioning) succeeded.
- The executables will accept input and output via the filesystem as described below.


#### Parameterized Resources


When **provision** is executed, the parameters supplied in the sandbox request are provided to it. They are made available in the directory **/signadot/plugin/input** in the same form as mounted ConfigMaps or mounted Secrets. The plugin can use this information to either customize the resulting resource or modify how it goes about managing it. The operator determines the object name of the resulting resource, using the sandbox ID and resource name to guarantee uniqueness.


#### Secrets and Credentials


When **provision** is successfully completed, it provides outputs to the sandbox, in the same format as the inputs, under the directory **/signadot/plugin/output** . This information is then placed in a Secret wherever the sandbox needs it, under the corresponding resource object name.


## Alternative Designs and Related Work


We considered many different approaches to this problem during the design phase and we’ve seen lots of attempts at similar functionality, from[Terraform](https://terraform.io/) to[Google Config Connector](https://cloud.google.com/config-connector/docs/overview) , with a great many products and projects in between.


### Webhooks


One of our first designs was similar to mutating web hooks in Kubernetes, giving a synchronous response to resource allocation in our SaaS. We envisioned a system whereby upon creation of a sandbox via our SaaS, remote webhooks could be triggered which would modify the sandbox object in a way that reported resource allocation results. A similar mechanism was in place for sandbox deletion.


The major drawbacks of this approach are timing and lifecycle management. We couldn’t guarantee our API or UI would be reactive and at the same time provide enough time for provisioning any kind of resource. On the lifecycle side, placing this primarily in our SaaS gave us limited visibility into the lifecycle of sandboxes, complicating the coordination of sandboxes between our SaaS and our customer’s clusters.


We then tried making the webhooks asynchronous, having our SaaS respond to resource webhook responses whenever they might arrive. This solved the timing problem with respect to the reactivity of our service. However, coordinating the events between our SaaS and our customer’s clusters would have become even more complicated.


One additional problem with both webhook approaches is that customers would need to set up a service for serving the webhooks, which could introduce friction, depending on how a customer organises cluster management.


### In-Cluster Image Based Hooks


The design we settled on, described above, is based on an image interface. Images are much easier to deploy in clusters than full services. They are also objects which our operator, naturally, knows how to work with. By placing the invocation of hooks inside the image, coordinated by our operator, we were able to provide relatively strong guarantees about lifecycle safety. This design also allows for using standard, built in Kubernetes tools to configure, set up, and manage resource plugins.


## Conclusion


In this space, there is a need for a great deal of flexibility to make different kinds of resources available and there is also fundamental event based reactivity for lifecycle management. Hooks and webhooks seem a natural fit.


However, sandboxes should by default reliably spin up for testing and development, but to do so they would need to depend on the availability of resources. By contrast, webhooks traditionally are either informative or provide less fundamental functionality. For example, GitHub repositories work without web hooks and can work even when the linked third party web hooks are broken. Sandboxes, on the other hand will not work if the resources on which they depend are not available. This led us to re-think web hooks and provide operator level support with an image interface and lifecycle guarantees.


We are excited that resource plugins have already unblocked customers, we’ve published a few of them, and we are working on more. We welcome contributions and feedback in our[GitHub plugin repository](https://github.com/signadot/plugins) .


‍
