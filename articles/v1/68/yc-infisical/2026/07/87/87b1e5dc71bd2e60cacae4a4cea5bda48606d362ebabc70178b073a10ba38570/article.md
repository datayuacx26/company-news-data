---
schema_version: "1.0.0"
document_id: "87b1e5dc71bd2e60cacae4a4cea5bda48606d362ebabc70178b073a10ba38570"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/argocd-secrets-management"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-28T06:15:29.255453+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:fd10fd640661491572ba7ee244a7711578792e3b535d65eed488036ccff85bab"
---

# Managing Secrets in ArgoCD Without Fighting It

ArgoCD's premise is that Git is the source of truth. Whatever's committed to your repository is what gets applied to your Kubernetes cluster, and anything ArgoCD finds running that isn't in Git gets reconciled away.


That model works cleanly for deployments, config maps, and services. It breaks down the moment you need an API key, database password, or other credential. A secret is the one thing you specifically don't want sitting in plaintext in Git.


Any secrets management approach needs to get secrets into ArgoCD without storing them in plaintext.


But they often become operationally burdensome because constant encryption and decryption is annoying and makes it hard to rotate secrets or keep them centralized across different apps and environments. ArgoCD's reconciliation model can also quietly delete secrets it doesn't recognize as its own, which can make ArgoCD secrets management feel like a constant chore.


## Why ArgoCD has no native answer for secrets


A Kubernetes` secret` object is just a resource like any other from ArgoCD's point of view: it's either declared in the Application's Git source and applied, or it's not.


There's no special handling that lets a` secret` hold an encrypted or externally-fetched value differently from anything else. Committing it directly means committing a plaintext credential (Kubernetes` secret` objects are base64-encoded, not encrypted, so anyone reading the manifest reads the value).


Not committing the secret means ArgoCD applies the deployment manifest to the cluster without verifying the secret, marks the cluster as synced, and produces downstream errors because pods can never start.


This is what anyone managing secrets in ArgoCD wants to solve.


## The approaches most teams use


Most teams keep secrets out of plaintext in Git with one of two methods: Externally syncing secrets into Kubernetes or encrypting them.


### Encrypting secrets with SOPS and Sealed Secrets


Encryption methods keep plaintext secrets out of Git without keeping secrets *per se* out of Git, which makes GitOps easier by creating no external dependency, but does burden teams with managing encryption, decryption, and their respective keys. These can also be harder to automate and cause more manual work for DevOps or platform teams.


#### Sealed secrets


Bitnami's Sealed Secrets lets you encrypt a Secret into a` SealedSecret` custom resource using a public key, commit the encrypted` SealedSecret` to Git, and let a controller running in your cluster decrypt it back into a real Secret to mount it as a volume or inject it as an environment variable. ArgoCD applies the` SealedSecret` , the controller does the rest.


```text
apiVersion: bitnami.com/v1alpha1
kind: SealedSecret
metadata:
name: db-credentials
namespace: default
spec:
encryptedData:
password: AgBy3i4OJSWK+PiTySYZZA9rO43cGDEqA...


```


This works well for a single cluster, but becomes burdensome fast. Every secret change requires manually running` kubeseal` against the new value, committing the freshly sealed output, and waiting for ArgoCD to sync. If you forget a step, you’re either committing a stale secret or debugging why the cluster still has the old one.


This friction becomes a chore if you’re releasing several times a day and more than one or two people need to touch secrets.


#### SOPS


SOPS encrypts the Secret manifest itself with a tool like` age` or a cloud key management service (KMS) key, commits the encrypted file to Git, and decrypts it at apply time, typically through a Kustomize generator plugin (` ksops` ) that ArgoCD's config management plugin support can invoke during rendering.


The Secret ends up as a normal, ArgoCD-rendered resource in the Application's manifest. This means every engineer who needs to edit a secret needs the decryption key or KMS access before SOPS works, which creates friction.


It also complicates code review: a pull request touching an encrypted file shows an unreadable diff, so reviewing its changes means decrypting it locally before being able to read it.


SOPS also creates key management complexity: whoever holds the decryption key can read every secret encrypted with it, and rotating that key means re-encrypting every file that used it.


### Where secrets encryption falters


Any encryption-based security measure has a structural limit: teams need to manage keys and frequently encrypt and decrypt secrets. These workflows are typically manual, so they don’t scale well because they consume so many engineering resources.


The alternative is to reference secrets from an external secret store and use an operator to reconcile them into Kubernetes` secret` objects.


## Why teams use secrets managers for credentials in ArgoCD


Using a centralized secrets manager means secrets aren’t managed in Git at all, but live in an external encrypted store.


In this case, the manifest references a secret stored in a secrets manager and configures how to pull that secret and where to pull it from. A Kubernetes operator then reconciles it into a Kubernetes-native` secret` object and injects it into pods as mounts or environment variables.


The biggest benefit is that values can be centrally updated in a secrets manager you also use elsewhere. Managing secrets in ArgoCD becomes the same as managing secrets anywhere else, which gives you a central control plane for secrets management.


### External Secrets Operator (ESO)


ESO is an open-source project that takes the opposite approach: it doesn't encrypt anything for Git. Instead, an` ExternalSecret` resource declares which key to fetch from an external secrets manager like Infisical, and the operator writes the fetched value into a real Kubernetes Secret on a sync interval.


```text
apiVersion: external-secrets.io/v1
kind: ExternalSecret
metadata:
name: db-credentials
namespace: default
spec:
secretStoreRef:
name: my-secret-store
kind: ClusterSecretStore
target:
name: db-credentials
creationPolicy: Owner
data:
- secretKey: password
remoteRef:
key: prod/db
property: password


```


Nothing sensitive lives in Git at all. A secret automatically updates when it’s changed in the external[secrets manager](https://infisical.com/blog/secrets-management-complete-guide) without touching Git, which means secrets live in a centralized store and can be rotated or updated without touching ArgoCD’s source of truth.


ESO is a great tool, but does come with tradeoffs. If a secret isn't showing up, debugging now runs through three systems: ArgoCD, ESO, and whatever external secret store you use. It also requires maintaining a connection with your external secret store.


## When to use Infisical for ArgoCD secrets


[Infisical](https://infisical.com/) is an open-source secrets manager that centrally stores, rotates, access-controls, and delivers secrets, with a native Kubernetes Operator that pulls them into your cluster as native` secret` resources. The core pattern is similar to ESO, but specific to Infisical.


If your team is already using Infisical to manage secrets elsewhere (in CI, in other environments, across other clouds), the[Infisical Kubernetes Operator](https://infisical.com/docs/integrations/platforms/kubernetes/overview) lets ArgoCD-managed clusters pull from that same source instead of adding a fourth secrets store to keep in sync.


The` InfisicalStaticSecret` custom resource declares what to fetch and where to put it:


```text
apiVersion: secrets.infisical.com/v1beta1
kind: InfisicalStaticSecret
metadata:
name: db-credentials
namespace: default
spec:
infisicalAuthRef:
name: my-infisical-auth
namespace: default
syncOptions:
refreshInterval: 60s
sources:
- projectId: <your-project-id>
environmentSlug: prod
secretPath: /
targets:
- name: db-credentials
namespace: default
kind: Secret
creationPolicy: Owner


```


The` infisicalAuthRef` points at a separate` InfisicalAuth` resource, which in turn points at an` InfisicalConnection` resource declaring which Infisical instance to talk to. Both are one-time setup per cluster.


This means that if you configure Infisical for your pipeline once, you only need to manage secrets in Infisical through the dashboard or CLI. There’s no local encryption tool to install and no command to remember before a change reaches the cluster.


This can sometimes be tricky because ArgoCD is relatively ruthless.


## How ArgoCD can prune secrets out from under you


ESO and the Infisical Operator both create the real Kubernetes` secret` independently, after ArgoCD has already applied the` ExternalSecret` or` InfisicalStaticSecret` resource that describes it.


From ArgoCD's perspective, the generated` secret` in your cluster isn't part of what is directly rendered from Git. Every resource requires a Kubernetes owner reference back to the resource that created it, because ArgoCD will otherwise prune it.


This can make debugging confusing because a` secret` might reappear every sync interval only to vanish again when you deploy. The` secret` itself gives no indication of why it disappeared.


Infisical’s Kubernetes operator solves this with an owner-reference mechanism. A` creationPolicy` field controls whether the generated` secret` carries an owner reference back to the resource that created it to make sure it persists into the deployment.


On the current` InfisicalStaticSecret` CRD,` creationPolicy` is a required field. It should be set to` Owner` to attach that owner reference so Kubernetes' own garbage collection (and ArgoCD's resource tracking) can associate the` secret` with its source.


Anything a controller creates out-of-band, after ArgoCD's own sync step, needs an explicit owner reference back to the resource ArgoCD does track, or ArgoCD's next reconciliation can remove it. It's a one-line fix once you know to look for it. Before that, it looks like ArgoCD randomly breaking your secrets management setup.


## Choosing an approach


If you don't already have a secrets manager and want the simplest possible setup for a single cluster, you can try an encryption method like Sealed Secrets or SOPS. For anything beyond that, encryption methods falter as multiple environments, scaling teams, and complex processes make them burdensome.


To centralize secrets everywhere (including ArgoCD), centralize secrets in[Infisical](https://infisical.com/talk-to-us) across other parts of your infrastructure, the[Kubernetes Operator](https://infisical.com/docs/integrations/platforms/kubernetes/overview) extends that same source into your ArgoCD-managed clusters rather than adding a separate one.
