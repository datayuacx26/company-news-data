---
schema_version: "1.0.0"
document_id: "84d26789a7c9b8dc5c720b8cc97a9b473a5316b496ce183b79c83d33afcf6aa4"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/kubernetes-operator-rebuild"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:d4bd44aed1b34063c17dc7ef4e6ec31faa839ad9fb85c99f039731d303408a3f"
---

# Our Kubernetes Operator Didn’t Scale, So We Rebuilt It

Security is often at odds with convenience, but the human brain prefers convenience (and makes mistakes, even with the best of intentions). Most identity security tools reconcile this by making security as convenient as possible.


Any friction increases the risk that people build convenient workarounds or sidestep security tools and thus soften the organization’s security posture.


This is why we rearchitected[our Kubernetes operator](https://infisical.com/docs/integrations/platforms/kubernetes/overview#kubernetes-operator) to improve performance and developer experience when we realized it struggled at scale.


## Why we built a Kubernetes operator


One of the maxims of good secrets management is centralization. Uniting all secrets provides one place to store, manage, and audit secrets across your infrastructure.


Centralization requires syncing secrets into every type of infrastructure and deployment model. This means delivering secrets from the secret store into the user’s infrastructure to the service that consumes the secret. Ideally, this happens without workarounds or custom logic to avoid creating security gaps or placing maintenance burden on users.


Our first Kubernetes operator extended native secret syncs into distributed deployments. It worked, but didn’t scale well. As resources in a cluster or deployment proliferated, its memory footprint ballooned and performance degraded.


This is why we redesigned our operator with a new reference-based architecture.


## Where our first Kubernetes operator faltered


Syncing secrets into Kubernetes at scale required our operator to do a few things:


- **Connect and authenticate to Infisical:** Where the API is hosted, how to connect and authenticate to it.
- **Find the correct secrets in Infisical:** Know the correct secret path within Infisical: which project, environment, folder, etc.
- **Enable pods and deployments to use them:** Reconcile the secrets into Kubernetes-native` secret` objects so deployments and pods can get secrets into the correct environments.


Our initial design checked those boxes, but faltered as workloads increased.


### Why v1’s architecture struggled at scale


In v1, users wrote monolithic custom` InfisicalSecret` resources that pointed at Infisical secrets. Each` InfisicalSecret` resource on the` v1alpha1` API contained:


- The address of the Infisical instance
- The authentication credentials
- The scope to pull from
- The managed Kubernetes secret to write to


A resource looked like this:


```text
apiVersion: secrets.infisical.com/v1alpha1
kind: InfisicalSecret
metadata:
name: service-a-secrets
spec:
hostAPI: https://app.infisical.com/api
authentication:
universalAuth:
credentialsRef:
secretName: universal-auth-credentials
secretNamespace: default
secretsScope:
projectSlug: my-project
envSlug: prod
secretsPath: "/service-a"
managedSecretReference:
secretName: service-a-managed
secretNamespace: default


```


Scalability suffered because each resource replicated authentication and connection. That architecture works on persistent infrastructure. A VPS or VM is one identity that only reauthenticates on restart or config changes. Kubernetes clusters, however, contain dozens or hundreds of these resources and frequently redeploy and restart. Because each carried its own auth and connection, each held its own independent client. This created three problems:


- Resources consumed outsized memory because each resource held its own client in memory. At scale, it created out-of-memory issues that required raising Helm memory limits. Each time pods went OOM, each resource reconciled and authenticated at the same time.
- Restarts produced a burst of simultaneous authentication calls, which ran into rate limits. The operator would succeed after backoffs and retries, but it created latency in getting clusters to a steady state.
- Engineering teams had to do extra work. Rotating a machine identity or changing the Infisical host meant editing the authentication block on every single resource.


The fundamental issue was the overloaded CRD architecture, not missing logic. We evaluated event handlers, jitter, and other logic. Those may have helped, but added more complexity to an already overloaded CRD.


We could only solve the underlying issue with a new architecture.


## How reference-based architecture fixed the replication


The new design separates connection, authentication, and sync. Secrets reference authentication and connection resources as shared objects. This fixes the performance issues and improves the developer experience.


We modeled the new architecture roughly on[External Secrets Operator](https://external-secrets.io/) ’s (ESO) resource split, which separates` provider` ,` store` , and` externalsecret` CRDs. Infisical[integrates with ESO](https://external-secrets.io/latest/provider/infisical/) , but we build our own operator for two reasons:


- ESO has previously paused development. It has since resumed, but it’s not ideal to create a dependency that may stop being developed.
- We want to offer native UX, minimize moving parts, and eventually support[dynamic secrets](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview) and[push secrets](https://infisical.com/docs/integrations/platforms/kubernetes/infisical-push-secret-crd) (secrets that originate in Kubernetes and find their way to Infisical), which ESO doesn’t support.


V2 of the Infisical Kubernetes Operator,` v1beta1` , introduces three CRDs:


- ` InfisicalConnection` defines the address of an Infisical instance and optional TLS settings.
- ` InfisicalAuth` defines the authentication details for a[machine identity](https://infisical.com/docs/documentation/platform/identities/machine-identities) and references a connection.
- ` InfisicalStaticSecret` defines a sync and references an auth resource. It replaces` InfisicalSecret` .


*Note: We’re still working on CRDs for` InfisicalDynamicSecret` and` InfisicalPushSecret` .*


` InfisicalConnection` and` InfisicalAuth` are defined once, and secret resources point at them. This ensures` InfisicalStaticSecret` can find, pull, and reconcile secrets without running its own client.


Here’s an example of a connection resource:


```text
apiVersion: secrets.infisical.com/v1beta1
kind: InfisicalConnection
metadata:
name: my-infisical-connection
spec:
address: https://app.infisical.com


```


And here’s how you can define authentication:


```text
apiVersion: secrets.infisical.com/v1beta1
kind: InfisicalAuth
metadata:
name: prod-auth
spec:
infisicalConnectionRef:
name: my-infisical-connection
namespace: default
method: universal
universal:
clientIdRef:
name: universal-auth-credentials
namespace: default
key: clientId
clientSecretRef:
name: universal-auth-credentials
namespace: default
key: clientSecret


```


A final` InfisicalStaticSecret` resource looks like this:


```text
apiVersion: secrets.infisical.com/v1beta1
kind: InfisicalStaticSecret
metadata:
name: service-a-secrets
spec:
infisicalAuthRef:
name: prod-auth
namespace: default
sources:
- projectId: <your-project-id>
environmentSlug: prod
secretPath: /service-a
targets:
- name: service-a-managed
namespace: default
kind: Secret
creationPolicy: Owner


```


` InfisicalAuth` is now part of configuration. The controller resolves each` InfisicalStaticSecret` auth reference to client cached by identity, which creates one client per identity, not one per resource. Any` InfisicalStaticSecret` pointing at the same identity reuses authentication and connection.


This fixes the previous issues:


- A restart produces one authentication call per identity, not one per resource.
- Changing the Infisical host or machine identity only requires editing one resource.
- Clients aren’t replicated across every resource, which shrinks the memory footprint.


The cache is lazy, but invalidates on config changes or when a call comes back with a 401 or 403. The operator then drops the cached client and reauthenticates.


To solve memory, authentication, and connection, we could’ve deduplicated clients internally by identity. We split the monolithic CRD into three instead to improve the developer experience. Our operator now mirrors the well-known pattern from ESO and obviates mass-editing CRDs when configuration changes.


Besides the rearchitected CRDs, we also made three smaller upgrades in v2:


- ` InfisicalStaticSecret` can pull from more than one source path. This allows workloads to consume secrets from different paths. If a platform team owns auth credentials at` /shared/auth` while app teams own integration keys at` /app/integrations` , the CRD supports this natively.
- It can write to more than one target, where a target can be a Kubernetes` secret` or a` ConfigMap` . This enables pulling sensitive values into a` Secret` and non-sensitive ones into a` ConfigMap` . This is also useful for fetching values to sidecars that only accept` ConfigMap` mounts.
- Every resource now reports a readiness status, so` kubectl get` shows the health of each resource.


### Upgrading to Infisical Kubernetes operator v2


We currently maintain both` v1alpha1` and` v1beta1` , but plan to deprecate the` v1alpha1` API eventually. Our documentation contains a[migration guide](https://infisical.com/docs/integrations/platforms/kubernetes/overview#migrating-from-v1alpha1-to-v1beta1) as well as detailed instructions for installing and using our Kubernetes operator more generally.


## Why Kubernetes matters to us


Software categories dictate engineering priorities. Product managers and engineers spend hours in issue trackers, so Linear invested heavily in low latency, a crisp UI, and UX affordances like keyboard shortcuts and cmd+k navigation.


In the security category, products need to work on any infrastructure, deployment model, and workload size. Gaps or parallel systems introduce the friction that automation and centralization were supposed to prevent. Any workaround, duplicate secrets manager, or “let’s just use plaintext secrets here, this one time” is a potential vulnerability.
In an ideal world, Infisical should be able to secure secrets for a company that simultaneously uses us on all three major clouds, a dedicated VPS, and the server basement they’ve been maintaining since the 1990s.


Kubernetes is becoming more popular across all organization sizes, so managing Kubernetes secrets on Infisical needs to be seamless. This makes seemingly small things like the scalability of a Kubernetes operator important.
