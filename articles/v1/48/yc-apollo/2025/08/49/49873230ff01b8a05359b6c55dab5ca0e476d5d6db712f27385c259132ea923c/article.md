---
schema_version: "1.0.0"
document_id: "49873230ff01b8a05359b6c55dab5ca0e476d5d6db712f27385c259132ea923c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/getting-started-with-apollo-graphos-operator"
published_at: "2025-08-05T06:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:b4a890d65a782749925e4dd9f88ea46effe7ee0bb675858ff34dadea647f0362"
---

# Manage your subgraphs and supergraphs as Kubernetes-native resources

The Kubernetes ecosystem has evolved to provide powerful declarative resource management for applications, services, and infrastructure. Today, we’re excited to announce the public preview of the[Apollo GraphOS Operator](https://apollographql.com/docs/apollo-operator) , bringing the best way to manage your subgraphs and supergraphs in Kubernetes environments.


With the operator, you can define your subgraphs, compose them into supergraphs, and deploy them using native Kubernetes resources and workflows. This solves key challenges like keeping subgraph implementations and schemas in sync, providing end-to-end declarative control over both subgraphs and supergraphs, and enabling controlled deployments supergraphs, all within a GitOps-friendly, Kubernetes-native environment.


Let’s explore how the Apollo GraphOS Operator brings Kubernetes-native resource management to your GraphQL supergraphs.


## What is the Apollo GraphOS Operator?


The Apollo GraphOS Operator introduces three new Kubernetes Custom Resource Definitions (CRDs) that work together to manage your supergraphs declaratively.


The **Subgraph** resource defines your subgraphs with their schemas and endpoints. You can specify your schema inline, load it from a container image, or reference it as an OCI artifact. The operator automatically discovers these subgraphs and makes them available for composition.


The **SupergraphSchema** resource defines supergraph schemas from your Subgraph resources. It uses Kubernetes label selectors to dynamically find relevant subgraphs, then automatically publishes them to Apollo GraphOS Studio for composition. This gives you flexible, declarative control over which subgraphs participate in each supergraph.


The **Supergraph** resource deploys GraphOS Routers. It automatically provisions all the necessary Kubernetes resources for your supergraphs: Deployments, Services, ConfigMaps, and Secrets. Because it creates standard Kubernetes deployments, you get all the native Kubernetes capabilities like rolling updates, automatic rollbacks, and integration with your existing monitoring and scaling tools.


## Declare your first Subgraph


Once you have[installed](https://apollographql.com/docs/apollo-operator/get-started/install-operator) the operator in your cluster, you can use the Subgraph resource to define your subgraphs declaratively. This ensures that your subgraph schemas and implementations are always in sync and stay versioned together.


While you can specify your schema inline, we recommend storing your schema in the same container image as your subgraph implementation. Here’s an example of a Subgraph resource that loads its schema from a container image:


```text
apiVersion: apollographql.com/v1alpha1
kind: Subgraph
metadata:
name: users-subgraph
namespace: users
labels:
domain: retail
service: users
spec:
name: users
endpoint: http://users-service.users.svc.cluster.local/graphql
schema:
ociImage:
# Reference to your container image
reference: my-registry/users-service:v1.0.0
# Subgraph schema file in your image
path: /schema.graphql
```


You can also use the Subgraph resource with Apollo Connectors to create GraphQL APIs from your existing REST services. As you might not have an implementation server, you can also define your subgraph schema directly in the Subgraph specification:


```text
apiVersion: apollographql.com/v1alpha1
kind: Subgraph
metadata:
name: products-subgraph
namespace: products
labels:
domain: retail
service: products
spec:
name: products
schema:
sdl: |
extend schema
@link(
url: "https://specs.apollo.dev/federation/v2.10"
import: ["@key"]
)
@link(
url: "https://specs.apollo.dev/connect/v0.2"
import: ["@connect", "@source"]
)
@source(
name: "ecomm"
http: { baseURL: "http://products-service.products.svc.cluster.local/" }
)


type Product {
id: ID!
name: String
description: String
}


type Query {
products: [Product]
@connect(
source: "ecomm"
http: { GET: "/products" }
selection: """
$.products {
id
name
description
}
"""
)
}
```


The operator will automatically discover these subgraphs and make them available for composition. You can check the status with:


```text
kubectl get subgraph users -o yaml
kubectl get subgraph products -o yaml
```


## Automatically compose a SupergraphSchema


Now that you have your subgraphs defined, you can compose them into a supergraph schema using the SupergraphSchema resource. It automatically keeps track of the matching Subgraph resources, triggering launches when it detects changes to your subgraph schemas. It also performs automatic change debouncing and batching on your behalf.


```text
apiVersion: apollographql.com/v1alpha1
kind: SupergraphSchema
metadata:
name: retail
namespace: apollo-schemas
spec:
selectors:
- matchLabels:
domain: retail
graphRef: retail@current
```


This SupergraphSchema will automatically find all subgraphs with the` domain: retail` label across namespaces and compose them into a supergraph. The operator discovers subgraphs using the label selector, publishes schemas to the Apollo GraphOS Platform for composition, and continuously monitors the composition status to keep your resource status updated.


**Note:** While the operator reconciles resources across namespaces, it can be finely tuned to monitor specific namespaces for each resource type. For example, you could allow service owners to manage Subgraphs in their own namespaces, while managing SupergraphSchemas in a dedicated namespace.


You can monitor the composition process with:


```text
kubectl get supergraphschema retail -o yaml
```


Once the composition is successful, you’ll see an **Available** condition indicating that your supergraph schema is ready for deployment.


## Deploy your Supergraph


Now that you have a composed supergraph schema, you can deploy it using a Supergraph resource:


```text
apiVersion: apollographql.com/v1alpha1
kind: Supergraph
metadata:
name: retail
namespace: apollo-supergraphs
spec:
replicas: 2
schema:
resource:
name: retail
namespace: apollo-schemas
podTemplate:
routerVersion: 2.4.0
routerConfig:
sandbox:
enabled: true
supergraph:
introspection: true
```


The operator takes care of all the complexity behind the scenes. It securely manages graph API key as Kubernetes secrets, sets up ConfigMaps with your router configuration, fetches the latest schema and deploys it to the router. Furthermore, it applies schema changes by triggering a standard rolling update, ensuring a progressive deployment that can be rolled back in case of problems.


**Remark** : If you are not using SupergraphSchema resources, for example because your subgraphs are not all living in Kubernetes, or you deploy subgraphs across mutliple clusters, you can also use a graph variant reference directly. Check out the[operator workflow documentation](https://apollographql.com/docs/apollo-operator/workflows) to learn more about advanced workflows.


Once you have created your Supergraph, you can check its status with the following command:


```text
kubectl get supergraph retail -o yaml
```


Once the deployment is ready, you’ll see a **Ready** condition and your supergraph will be available through the automatically created Kubernetes service.


## What’s next for the operator?


We’re only getting started bringing Kubernetes-native subgraphs and supergraphs management with the Apollo GraphOS Operator. In addition to our current capabilities, we will be working on new features that will enhance the developer experience even further.


This includes the introduction of ephemeral supergraphs, allowing you to replace specific subgraphs while developping new features. Furthermore, we are focusing on implementing safe deployment strategies for your supergraphs, such as canary releases.


## Get started today


The Apollo GraphOS Operator brings the power of Kubernetes-native resource management to your GraphQL supergraphs. With declarative subgraph definitions, automatic composition, and seamless deployment, you can manage your supergraphs using the same Kubernetes workflows and tooling you already know.


Ready to simplify your supergraph management?[Install](https://apollographql.com/docs/apollo-operator/get-started/install-operator) the Apollo GraphOS Operator and start defining your subgraphs, composing supergraphs, and deploying routers using the Kubernetes-native approach we’ve explored in this post.


For advanced deployment patterns and workflow options, visit our operator[documentation](https://apollographql.com/docs/apollo-operator) .


Written by


Nicolas Moutschen


[Read more by Nicolas Moutschen](https://www.apollographql.com/blog/author/nmoutschen)
