---
schema_version: "1.0.0"
document_id: "52806f7765734cfe47366b4ac6e0b5ee4663769bec9725a34a760fbbeaa6cb94"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/acl-filtering-kubernetes-apis-using-spicedb"
published_at: "2024-04-11T10:44:08.138+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:11e212f85c469ea79521a1121a593d890768c1244f5633c41cf7af9d0b78b412"
---

# ACL Filtering Kubernetes APIs using SpiceDB

Today, we’re open sourcing an internal project: the[SpiceDB KubeAPI Proxy](https://github.com/authzed/spicedb-kubeapi-proxy) .


Normally, when we[announce the release of an internal project as open source](https://authzed.com/blog/open-source-spicedb-operator) , we prefer to have already gained confidence in its quality in production. However, due to significant interest from community members who have learned about the proxy project, we’ve decided to open source it before reaching version 1.0.


## What is it?


The[SpiceDB KubeAPI Proxy](https://github.com/authzed/spicedb-kubeapi-proxy) functions as a network proxy positioned between a Kubernetes client (like[kubectl](https://kubernetes.io/docs/reference/kubectl/) or[client-go](https://github.com/kubernetes/client-go) ) and a Kubernetes cluster. It presents itself as a full-fledged[kube-apiserver](https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver) to clients.


Upon receiving a request, the proxy utilizes SpiceDB to make decisions about how to forward those requests to an upstream kube-apiserver. By querying SpiceDB, the proxy determines whether a request can proceed and how to filter a list of responses from Kubernetes - including live updates via` watch` requests.


Additionally, the proxy supports the passing of write requests to Kubernetes, executing a distributed transaction across SpiceDB and the Kube API. This ensures that permissions data remains synchronized with the backing cluster. The proxy maintains a durable log of operations and can be configured to perform optimistic or pessimistic locking around the dual writes.


No assumptions are made about how you want to store permissions in SpiceDB, or how you want to use that data to authorize requests. Instead, the proxy is configured with a set of rules that determine how to authorize requests and filter responses:


- **Check** rules: These determine whether a request is authorized to proceed at all. For example, a rule might specify that a user can only list pods in a namespace` foo` if they have the permission` namespace:foo#list@user:alice` in SpiceDB.
- **Filter** rules: These are used to filter the response from the kube-apiserver based on SpiceDB data. For example, a rule might say that a user can only see the pods in namespace` foo` if corresponding relationships exist in SpiceDB that enumerate the permitted pods, like` pod:foo/a#view@user:alice` and` pod:foo/b#view@user:alice` . In this example,` alice` would only see pods` a` and` b` in namespace` foo` . Filter rules extend to` watch` requests and can dynamically filter watch events.
- **Write** rules: These are used to write data to SpiceDB based on the request being authorized by the proxy. For example, if` alice` creates a new pod` c` in namespace` foo` , a rule may dictate that a relationship should be written to SpiceDB granting ownership, e.g.` pod:foo/a#view@user:alice` . The dual write process is resilient to failures in SpiceDB, the Kube API, and the proxy itself.


Because the proxy presents as a kube-apiserver, it can be configured with separate user authentication from the backing Kubernetes cluster. Check out the[GitHub repo](https://github.com/authzed/spicedb-kubeapi-proxy) for more comprehensive documentation and examples.


## Why did we write it?


We wrote the proxy to support the multi-tenant version of the control plane managing[Authzed Cloud](https://authzed.com/docs/authzed/guides/picking-a-product#cloud) (which is currently in early access) and to provide fine-grained permissions to the control plane for[Authzed Dedicated](https://authzed.com/docs/authzed/guides/picking-a-product#dedicated) .


All of AuthZed’s user-facing infrastructure operates through interactions with a set of internal Kubernetes controllers. The proxy is the intermediary between the web dashboard and the backing control plane, ensuring that users only access the resources they have been authorized to view.


Before implementing the proxy, all users of the Authzed Dedicated dashboard had full authority over their control plane. Leveraging SpiceDB to provide fine-grained permissions for the control plane allows us to "dog food" the architecture we recommend for customers. This approach enables organization owners to exercise fine-grained control over their dedicated control plane. This feature will be rolling out to Dedicated customers in the coming months.


The Kubernetes authorization layer is[technically](https://kubernetes.io/docs/reference/access-authn-authz/authorization/#authorization-modules) modular, so this project could have been developed as an authorization plugin. However, such an approach would have required a fork of Kubernetes, which we felt was too cumbersome. Our goal is to validate these concepts in a production environment before suggesting methods for deeper integration with Kubernetes. This could potentially involve advocating for a more pluggable Authz architecture (similar to CNI or CRI).


Special shout out to[Lucas Käldström](https://twitter.com/kubernetesonarm) - whose insights and[discussions](https://speakerdeck.com/luxas/beyond-rbac-avoid-broken-acls-in-control-planes-with-declarative-relation-based-access-control) about ReBAC in Kubernetes inspired our decision to open source the proxy.


## Should I use it?


This is the perfect time to jump in, experiment, and provide feedback while the APIs and implementations are still evolving towards a 1.0 release. That said, this is probably not something you should rely on in production just yet. We’re targeting a 1.0 release in a few months, following sufficient community feedback and real-world testing.


## What’s next?


In the future, we may explore a Kubernetes API for defining proxy rules at runtime, or even consider an API that could directly replace RBAC for components that embrace the ReBAC model. For now, we’re focused on refining the solution for production use.


If others find this solution as useful as we do, we may explore ways for more seamless integration into Kubernetes itself, akin to what is possible with networking, storage, and container runtimes.


Additionally, while it may seem like a distant possibility, there's potential for[Kubernetes to run on top of CockroachDB](https://groups.google.com/g/kubernetes-sig-api-machinery/c/SA3E0o9Egdc) , which could enable us to update SpiceDB and Kubernetes in a single transaction.


On this page


- What is it?
- Why did we write it?
- Should I use it?
- What’s next?


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
