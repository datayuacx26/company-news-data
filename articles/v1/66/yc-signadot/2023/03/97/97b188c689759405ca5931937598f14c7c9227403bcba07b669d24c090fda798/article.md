---
schema_version: "1.0.0"
document_id: "97b188c689759405ca5931937598f14c7c9227403bcba07b669d24c090fda798"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/new-framework-for-ephemeral-resources/"
published_at: "2023-03-30T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:4c007e249d0f4ff3d02d54fce5b2cf6db3bbabc5e1140e40fe005754b046c56b"
---

# A New Framework for Ephemeral Resources

## Sandboxes and Ephemeral Resources


We’ve seen already how[sandboxes](https://www.signadot.com/blog/creating-sandboxes-in-kubernetes-at-scale/) in Kubernetes offer a scalable way of testing microservices using request routing. As sandboxes start to get used for more complex forms of testing - ranging from manual preview environments to executing e2e tests, one of the most important questions that needs to be addressed is that of data isolation.


While requests between microservices are isolated through having request tenancy information and request routing, this is often not possible when using data stores, or any internal or external services that don’t have support for tenancy-based isolation. In these cases, we need a robust approach to isolate these dependencies, so as to use the sandbox safely without breaking the isolation boundary of a sandbox and affecting the underlying baseline environment.


Sandbox resources are managed using the notion of a **resource plugin** which is an abstraction that can be invoked by a sandbox (with some input arguments) to create/get an instance of an ephemeral resource, bind that to the sandboxed workloads, and also perform deletion/cleanup logic for that resource when the sandbox is deleted. One thing to note is that this entire workflow is ideally run within Kubernetes itself because the resource plugin often needs access to credentials/secrets in order to set up the resource.


In the following sections, we will explain the reasoning behind the previously built resource framework, discuss what we learned and some of the notable features of the new framework, and show how it incorporates those learnings.


## Where We Started


Brainstorming about Resources back in 2022


When we first implemented a framework to handle ephemeral resources associated with sandboxes, some of the design decisions we made were:


1. All resource plugins will be directly installed into each Kubernetes cluster as CRDs and configuration.
2. Each resource plugin will be packaged using[helm](https://helm.sh/) and expose all the configurations necessary for our users to stand up and configure that ephemeral resource via **values.yaml** .
3. Building a custom resource plugin will require creating a new docker image and deploying that into the Kubernetes cluster.


## What we Learned from User Feedback


Once we launched an alpha version of the above[last year](https://www.signadot.com/blog/microservices-test-isolation-with-resource-plugins/) , we learned several things from early users.


First, the amount of custom logic that is required - especially around the configuration of the resource - say seeding a MySQL database is rather bespoke and it is very difficult to provide sufficient flexibility via helm **values.yaml** to accommodate all the diverse use-cases across different organizations.


Second, and related to the above, the need for custom plugins was far higher than anticipated. Custom plugins required the creation of a custom docker image that adheres to a specific contract which makes it error-prone and quite time-consuming for our users to build and test.


Third, we heard a lot of asks for supporting IAC providers (Terraform, etc) for cases where infrastructure isolation was desirable, and accommodating this under the helm/values paradigm proved difficult given how much variance there is in managing IAC configuration.


Finally, several of our users have many Kubernetes clusters, and installing these resource plugins and managing them across these individual clusters proved quite challenging as well - given that the plugins were installed on a per-cluster basis.


Overall, we learned that we had underestimated just how much bespoke logic would need to live in a resource plugin. While the underlying framework we had built was quite versatile, it also required a lot of effort to extend and build something custom.


## New Resources Framework


Earlier this year, we decided to improve on the previous resource management framework for sandboxes, with the intent of resolving some of the difficulties that we had uncovered based on user feedback. We’re pleased to make this new framework available with our[v0.12 release](https://www.signadot.com/) .


The newly minted resource plugin configuration looks like this:


There are several improvements in this new framework that address the shortcomings of what we had built previously.


1. An underlying full-fledged workflow execution engine that allows for a lot of flexibility - including being able to execute multi-step workflows.
2. Separation of the runner (image, pod, etc) and the script (business logic) to make it easier to extend and iterate on the plugin without having to rebuild images.
3. Explicit passing of inputs and outputs in the workflow to make the interfaces less error-prone for users building custom plugins.
4. CLI tooling to embed and manage scripts & pod parameters as separate files in source control.
5. Creation and management of the plugin at the Signadot API (as opposed to being at each individual cluster), while still providing flexibility for in-cluster configuration for entities like secrets.


The execution of the plugin (when sandboxes are created) continues to be entirely within the user’s connected Kubernetes cluster. The workflow now looks as shown below.


Check out our documentation on[resource plugins](https://www.signadot.com/docs/reference/resource-plugins) to get a more in-depth perspective on how these plugins work and how they are written. Additionally, in recognition of the need for making it easy to build custom plugins, we’re writing several[sample plugins](https://github.com/signadot/plugins) that are meant to provide a good starting points for users looking to make use of these resource plugins within their infrastructure.


## What’s Next?


We’re excited about this new framework and its potential for bringing down the time to create a resource plugin from being a long-involved process to something that takes minutes. We’re just getting started and would love to learn more about how we can make sandboxes and resources more versatile and robust for testing microservices in a scalable fashion. If you have any thoughts or feedback, come talk to us on our[slack channel](https://join.slack.com/t/signadotcommunity/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ) . If you’d like to try sandboxes out, you can sign up at[https://www.signadot.com/](https://www.signadot.com/) and give it a whirl with our quickstart guide.


‍
