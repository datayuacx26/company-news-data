---
schema_version: "1.0.0"
document_id: "28b74f87b693a1fd65ac862e539d49c3dd437217d05881cc3c8c0bb153c1d9c2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/open-source-spicedb-operator"
published_at: "2022-09-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:c18e49187590e919d76c06ee391903b386eac5a9d0d34f778ae638cc91709787"
---

# SpiceDB Operator is Open Source

Today we’re announcing the open sourcing of[spicedb-operator](https://github.com/authzed/spicedb-operator/) - a[Kubernetes operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) for installing, upgrading, and maintaining[SpiceDB](https://github.com/authzed/spicedb) clusters on Kubernetes. Through[previous work at CoreOS](https://techcrunch.com/2017/12/05/coreos-tectonic-1-8-makes-it-easy-to-plug-external-services-into-kubernetes/) and[Red Hat](https://operatorframework.io/) , our team developed many of the early ideas around operators. As we began to scale out the deployment of SpiceDB clusters across our fleet to address the needs of our customers, an operator was the natural choice.


## Running in production for 4 months


The operator has been quietly managing SpiceDB clusters for[Authzed’s Serverless and Dedicated](https://authzed.com/pricing) offerings for the past four months. It's production-ready and available today!


To get started yourself, install the operator:


sh


1


```text
kubectl apply --server-side -k github.com/authzed/spicedb-operator/config


```


sh


1


```text
kubectl apply --server-side -k github.com/authzed/spicedb-operator/config


```


And then create a cluster:


sh


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


```text
kubectl apply --server-side -f - << EOF
apiVersion: authzed.com/v1alpha1
kind: SpiceDBCluster
metadata:
name: dev
spec:
config:
replicas: 2
datastoreEngine: postgres
secretName: dev-spicedb-config
---
apiVersion: v1
kind: Secret
metadata:
name: dev-spicedb-config
stringData:
datastore_uri: "postgresql:///the-url-of-your-datastore"
preshared_key: "averysecretpresharedkey"
EOF


```


sh


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


```text
kubectl apply --server-side -f - << EOF
apiVersion: authzed.com/v1alpha1
kind: SpiceDBCluster
metadata:
name: dev
spec:
config:
replicas: 2
datastoreEngine: postgres
secretName: dev-spicedb-config
---
apiVersion: v1
kind: Secret
metadata:
name: dev-spicedb-config
stringData:
datastore_uri: "postgresql:///the-url-of-your-datastore"
preshared_key: "averysecretpresharedkey"
EOF


```


Once a cluster is running, the operator will automatically run migrations and roll out new SpiceDB releases as they become available. Or, by pinning a cluster to a specific release, the operator can be used to coordinate the rollout of migrations in git-ops workflows.


The operator also simplifies the configuration of multi-node clusters and TLS - for more information, check out the[examples](https://github.com/authzed/spicedb-operator/tree/main/examples) and the[docs](https://docs.authzed.com/operator/installing) .


## Listen up, idioms!


We’ve also open-sourced[controller-idioms](https://github.com/authzed/controller-idioms) , the library we developed to support SpiceDB Operator and other operators that we use to run[Authzed Dedicated](https://authzed.com/pricing) . We found ourselves solving some of the same problems in every operator we wrote, and built this microframework to standardize common patterns across our stack.


This new library:


- Works with other (golang-based) tooling like[client-go](https://github.com/kubernetes/client-go) and[controller-runtime](https://github.com/kubernetes-sigs/controller-runtime) .
- Uses golang[generics](https://gobyexample.com/generics) to simplify dealing with Kubernetes APIs, informers, and indexes.
- Implements common patterns for resource adoption, ownership, status, and metrics.
- Provides a framework for breaking down large controller statemachines into small, testable pieces.


If you’re developing an operator of your own, please check out controller-idioms and let us know what’s missing in the[GitHub issues](https://github.com/authzed/controller-idioms/issues) .


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


## Join the Conversation


Don’t see something you want? Let us know in the[GitHub issues](https://github.com/authzed/spicedb-operator/issues) what you’d like to see from the operator in the future, and don’t hesitate to reach out on[discord](https://authzed.com/discord) with any questions or feedback.


*Header image generated with DALL·E 2 with prompt "a robot putting a database icon in a kubernetes cluster"*


On this page


- Running in production for 4 months
- Listen up, idioms!
- Additional Reading
- Join the Conversation


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
