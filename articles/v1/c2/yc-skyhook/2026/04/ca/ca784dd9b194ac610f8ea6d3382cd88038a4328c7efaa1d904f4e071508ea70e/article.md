---
schema_version: "1.0.0"
document_id: "ca784dd9b194ac610f8ea6d3382cd88038a4328c7efaa1d904f4e071508ea70e"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/trivy-compromise-audit-checklist"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:39:47.906327+00:00"
fetched_at: "2026-07-28T22:16:19.189952+00:00"
content_hash: "sha256:d11d653e7d2974e38e4e955393d1cc6628e13e56b90ce8a48faf27b3bd038a75"
---

# After the Trivy Compromise: What Every K8s Pipeline Should Audit Right Now

TL;DR: On March 19, 2026, attackers took over the official Trivy GitHub Action and the official Trivy Docker image. Anyone whose CI pipeline pulled the latest Trivy that day got a backdoored binary that quietly stole cloud credentials and shipped them to the attackers. Same crew (TeamPCP) then used those stolen credentials to attack npm, PyPI, and Docker Hub the following weeks. This post is the audit checklist - the same controls work whether you have three engineers or a hundred, you just have less inventory to walk through. Working YAML included, jargon explained inline. At the end, we cover what Skyhook does for the clusters we manage.


## What Made This Attack Different


Most CVEs are about a flaw in a piece of software you are running. The Trivy attack was different because **the attacker became the trusted thing** . Three details made it nasty:


1. **The scanner became the malware.** Trivy is the tool teams use to decide whether *other* images are safe. When the scanner itself is the threat, your image scanner cannot save you. The malicious payload ran silently before the real scan, so CI logs looked normal.
2. **They moved version tags, not commits.** 76 of 77 version tags of` aquasecurity/trivy-action` (` v0` ,` v1` , semver tags) were rewritten to point at malicious code. The underlying commit IDs (the long` abc123...` SHAs) were untouched. So anyone who wrote` uses: aquasecurity/trivy-action@v0` got the malware. Anyone who wrote` uses: aquasecurity/trivy-action@<long-sha>` was fine.
3. **They came back three days later.** After the cleanup, the same stolen credentials were used to push two new malicious Trivy Docker images on March 22. If your CI was pulling` aquasec/trivy:latest` , you might have grabbed a clean one Monday and a malicious one Wednesday and never noticed.


The downstream damage is still being counted.[CERT-EU has attributed](https://cert.europa.eu/blog/european-commission-cloud-breach-trivy-supply-chain) a 340 GB Europa.eu data leak to credentials stolen this way, and[Datadog traced](https://securitylabs.datadoghq.com/articles/litellm-compromised-pypi-teampcp-supply-chain-campaign/) PyPI compromises in the following weeks (LiteLLM, Telnyx) back to tokens harvested from CI runs that pulled the bad binary.


## The Pattern Underneath


Every supply chain attack of the last five years rhymes: SolarWinds, Codecov,` tj-actions/changed-files` , Trivy. A trusted thing gets compromised, the references that point to it (` @latest` ,` @v0` ,` :latest` tag) are mutable, and downstream consumers pull the new version without noticing.


A "mutable reference" is anything that can change underneath you. Tags can be moved. Branches can be rewritten. A friendly name like` :latest` always points at whoever pushed last. The opposite is an "immutable reference" - a digest like` sha256:6d3c...` or a Git commit ID - which is content-addressed: change one byte and the ID changes too. The fix for this whole class of attack is structural: stop using mutable references in the places where it matters, and put one place in your stack that decides what is allowed to run.


## The Audit Order That Actually Ships


You cannot do everything in one week. You can do these three things, in this order, and the next attack like Trivy is a non-event for you.


### 1. Pin GitHub Actions to commit SHAs, not tags


If only one thing on this list gets done, do this. The Trivy attack was harmless to anyone pinning by SHA, because the SHAs never moved.


```text
# BAD: tag is mutable - it can be re-pointed at malicious code
-   uses  :   aquasecurity/trivy-action@v0


# BAD: even semver tags can be force-pushed
-   uses  :   aquasecurity/trivy-action@0.28.0


# GOOD: full commit SHA - cannot be changed
-   uses  :   aquasecurity/trivy-action@915b19bbe73b92a6cf82a1bc12b087c9a19a5fe2    # v0.28.0
```


In plain English: GitHub Actions are referenced by either a tag (a friendly name someone in the project picks and can move) or a commit SHA (the long string that uniquely identifies a snapshot of the code). Always use the SHA. Add the friendly version as a comment so humans can read it.


You do not have to keep these SHAs current by hand. Dependabot does it for you, opens PRs when actions release new versions, and updates the comment automatically:


```text
# .github/dependabot.yml
version  :   2
updates  :
-   package-ecosystem  :   "github-actions"
directory  :   "/"
schedule  :
interval  :   "weekly"
```


This costs you one PR and zero buy-in from anyone else. Open it.


### 2. Block` :latest` and pin every container image to a digest


The same idea, one layer up: a Docker image tag (` myapp:v1.2` ,` aquasec/trivy:latest` ) can be re-pointed at a different image at any time. A digest (` @sha256:...` ) cannot. To enforce this across a Kubernetes cluster, you use a tool called **Kyverno** . The policies in this section assume **Kyverno 1.10+** (for` mutateDigest` and the` subjectRegExp` field used later in the Cosign policy).


Quick definitions before the YAML:


- **Kyverno** is an open-source policy engine for Kubernetes. You write policies as YAML, install them in your cluster, and they get checked every time anyone tries to create a pod.
- **Admission control** is the moment Kubernetes asks "is this allowed?" before creating a resource. Kyverno plugs into that moment.
- **Image digest** is the immutable fingerprint of an image:` myapp:v1@sha256:abc123...` . The tag is decorative; the digest is what gets pulled.


Here is the policy. It does two things: rejects any pod that uses` :latest` , and automatically rewrites` image: foo:v1` into` image: foo:v1@sha256:...` at the moment the pod is created.


```text
apiVersion  :   kyverno.io/v1
kind  :   ClusterPolicy
metadata  :
name  :   disallow-mutable-tags
spec  :
validationFailureAction  :   Enforce    # actually block, not just warn
rules  :
-   name  :   block-latest
match  :
any  :
-   resources  :
kinds  : [  Pod  ]
validate  :
message  :   "Image tag :latest is forbidden. Pin to a specific version or digest."
pattern  :
spec  :
=(initContainers)  :
-   image  :   "!*:latest"
containers  :
-   image  :   "!*:latest"
-   name  :   rewrite-tag-to-digest
match  :
any  :
-   resources  :
kinds  : [  Pod  ]
verifyImages  :
-   imageReferences  :
-   "*"
mutateDigest  :   true     # the magic line: rewrite tag to digest at admission
required  :   false        # set to true once you also add signing
```


In plain English: the first rule rejects pods that use` :latest` . The second rule tells Kyverno "for any image, look up its current digest and rewrite the manifest so the cluster pulls by digest." Even sloppy` image: foo:v1` references get pinned automatically. If somebody force-pushes a tag tomorrow, you keep running the version you actually deployed.


Pair it with a registry allowlist so workloads can only pull from registries you trust.` anyPattern` is the OR construct in Kyverno - one of the patterns must match:


```text
-   name  :   registry-allowlist
match  :
any  :
-   resources  :
kinds  : [  Pod  ]
validate  :
message  :   "Pulls are only allowed from ghcr.io/myorg or harbor.internal."
anyPattern  :
-   spec  :
containers  :
-   image  :   "ghcr.io/myorg/*"
-   spec  :
containers  :
-   image  :   "harbor.internal/*"
```


The hard part of all this for most teams is not writing the policy - it is owning Kyverno itself: installing it, keeping it in sync with the Kubernetes version, upgrading it when the cluster upgrades, and making sure someone is still on call for it after the engineer who shipped it changes teams. This is the chassis Skyhook handles for the clusters we manage. Kyverno is a one-click addon. Policies are managed from the Skyhook UI: you pick from a baseline, paste in custom YAML for the ones you want, and toggle each policy between` Audit` (warn only) and` Enforce` (actually block) per cluster. The selection ends up in your GitOps repo so it is reviewable like any other change.


If you cannot run Kyverno at all, the next-best thing is to pin digests directly in your deployment manifests using Kustomize:


```text
# kustomization.yaml
images  :
-   name  :   aquasec/trivy
newName  :   aquasec/trivy
digest  :   sha256:6d3c1e2b7a8f4c5d9e0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e
```


You can find the digest of any image with` crane digest aquasec/trivy:0.69.3` (install` crane` from the` go-containerregistry` project) or with` docker buildx imagetools inspect aquasec/trivy:0.69.3` .


### 3. Run a "pull-through" registry as a choke point


A pull-through registry sits between your cluster and the public registries (Docker Hub, GHCR, etc.). When something asks for` docker.io/library/redis:7` , the request goes to your registry first. If your registry has it cached, it serves the cached copy. If not, it fetches it from Docker Hub once, caches it, and serves it. Every subsequent pull uses your copy. **Harbor** is the most common open-source choice.


This is the unsexy part of supply chain security, but it is what lets you actually do something when the next attack happens:


- **Audit log** : every image that ever entered your cluster is in one place.
- **Quarantine in one place** : if` aquasec/trivy:0.69.4` turns out to be malicious, you delete it from your registry once and it stops pulling everywhere.
- **Layer retention** : when a maintainer rewrites history (or a registry deletes an image), you still have the version you were running.


For most teams, the cloud bill for a self-hosted Harbor is in the low tens of dollars a month. The day it earns its keep, it pays for the next decade. (If you are very small - say, fewer than ten engineers and one production cluster - you can defer this and rely on the digest-pinning policy until the registry choke point becomes worth the operational overhead.)


The same logic applies one layer up at the deployment level. If every team deploys differently, "pin all images by digest" is a 50-place change. If every deploy goes through one GitOps plane, it is a one-place change. This is structurally what Skyhook gives you: one Kustomize layer, one ArgoCD plane, one set of overrides. When you decide no image without` @sha256:` is allowed in prod, the policy goes in one place and applies everywhere.


## Cosign Verification: Smaller Than People Think


The next step up from "block` :latest` and pin digests" is **image signing** : when you build an image, you cryptographically sign it. When the cluster tries to pull it, you verify the signature. If the signature is missing or wrong, the pull is rejected. The dominant open-source standard for this is **Cosign** , part of the **Sigstore** project.


Cosign has a reputation for being a months-long project. It is not. Signing your own builds is two extra lines in your CI workflow:


```text
cosign   sign   --yes   ghcr.io/myorg/app@  ${DIGEST}
```


Verifying it at the cluster is one Kyverno policy. Here is the version that proves the image was built by your repo, on your branch, by your CI:


```text
apiVersion  :   kyverno.io/v1
kind  :   ClusterPolicy
metadata  :
name  :   verify-org-signatures
spec  :
validationFailureAction  :   Enforce
rules  :
-   name  :   cosign-keyless-org-images
match  :
any  :
-   resources  :
kinds  : [  Pod  ]
verifyImages  :
-   imageReferences  :
-   "ghcr.io/myorg/*"
attestors  :
-   entries  :
-   keyless  :
subjectRegExp  :   "^https://github  \\  .com/myorg/[^/]+/  \\  .github/workflows/release  \\  .yml@refs/heads/main$"
issuerRegExp  :   "^https://token  \\  .actions  \\  .githubusercontent  \\  .com$"
rekor  :
url  :   "https://rekor.sigstore.dev"
mutateDigest  :   true
required  :   true
```


A focused engineer can have signing + this policy in` Audit` mode across their own org's images in about a week. The reason it sometimes stretches longer is *coverage* : third-party images (operators, sidecars, base images) are not signed by your CI, so this policy will not cover them. Most teams accept narrower scope and rely on the digest-pinning policy from above for everything they did not build themselves.


One small prerequisite people skip: Kyverno's webhook needs TLS, and the easiest way to handle that is to install **cert-manager** (a separate Kubernetes addon that issues certificates automatically). On Skyhook clusters cert-manager is already installed as a one-click addon, which removes one of the more common yak shaves on the path here.


## What People Miss


The audit fails silently in the same places every time. Three to actually check:


**Helm charts you adopted years ago.** Some` values.yaml` from a 2022 tutorial still has` image: redis:latest` . ArgoCD reconciles it on every cycle and nobody looks. Run` helm template` over your repo and grep for any image reference without` @sha256:` .


**Sidecars and init containers.** Service mesh proxies, log shippers (` fluent-bit:latest` is everywhere), wait-for-db init scripts, secrets injectors. People audit the main app container and stop. The Trivy attack landed in a CI sidecar pattern; the Codecov attack in 2021 landed in a CI script. Same shape.


**Operator-managed images.** When you install an operator (ArgoCD, cert-manager, Vault), it pulls *its own* images based on its source code. Those are often pinned to mutable tags that you do not control. List what is actually running with:


```text
kubectl   get   pods   -A   -o   jsonpath='{range .items[*]}{.spec.initContainers[*].image}{"\n"}{.spec.containers[*].image}{"\n"}{end}'   |   sort   -u
```


This is also where having a single addon catalog matters. If every operator was installed by a different engineer over three years from a different Helm command, you have no inventory. If they were all installed through one addon system, you have one list to grep. Skyhook's addon catalog is structured this way - one place to see what is running, one place to override an image source, one place to pin a version.


## The Triage Pass: 30 Minutes


If you have been ingesting Trivy in CI, do this first:


```text
# Look for the attacker's exfil endpoints in your repos
grep   -r   "scan.aquasecurtiy.org"   .            # note the typo - that's the malicious one
grep   -r   "tpcp-docs"   .


# Find every GitHub Action pinned by tag instead of SHA
grep   -rE   "uses:\s+\S+@v[0-9]"   .github/workflows/


# Find every Kubernetes manifest using :latest
grep   -rE   "image:\s+\S+:latest"   .
```


If any of those return hits, those are your weekend.


## A One-Week Plan


The work scales with your inventory, not with your team size. Three engineers and one cluster will finish faster than one hundred engineers and twenty clusters, but the order is the same:


1. **Day 1:** PR pinning every GitHub Action to a SHA. Add Dependabot to keep them current.
2. **Day 2-3:** Install Kyverno (or enable it from your platform's addon catalog). Apply the` block-latest` and` mutateDigest` policies in` Audit` mode for a week to see what would fail.
3. **Day 4-5:** Stand up Harbor as a pull-through cache. Update the registry allowlist policy.
4. **Day 6:** Flip Kyverno from` Audit` to` Enforce` . Triage the inevitable surprises.
5. **Week 2-3:** Add Cosign signing to your own builds. Add the verifyImages policy in` Audit` , then` Enforce` for your own images.


The first three items neutralize most of this attack class for a fraction of the work. Cosign is a focused week after that, not a quarter.


## Where Skyhook Fits


If you are running on Skyhook, most of the chassis for this work is already in place. Kyverno is a first-class addon, installed in one click and kept in sync with the cluster version through the same upgrade flow as everything else. cert-manager ships the same way.


Policies are managed from the Skyhook UI per cluster. A baseline of opinionated security policies is included with the addon (block host paths, block privileged containers, require non-root, require resource limits, require probes, require labels). You can add the` block-latest` and` mutateDigest` policies from this post by pasting them into the custom-policy editor, and you can flip each policy between` Audit` and` Enforce` from the same screen. The selection lives in your GitOps repo, so it is reviewable, reversible, and applied identically to every cluster.


That is the structural piece that matters here. Every Skyhook deploy goes through one Kustomize layer applied through one ArgoCD plane. When you decide tomorrow that no image without` @sha256:` is allowed in prod, the change goes in one place and applies everywhere. This works the same way whether you have one cluster or twenty, and whether your team is three engineers or a hundred - the leverage actually scales up the more inventory you have. The controls in this post are the ones we are continuously adding to the default bundle so customers do not have to think about them at all.


The next supply chain attack is already being planted. The good news: the controls that stopped this one will stop the next one. Pin the SHAs. Block the tags. Enforce the digests. Then go to bed.


## Further Reading


- [How to Manage Secrets in Kubernetes](https://www.skyhook.io/blog/how-to-manage-secrets-in-kubernetes) - The other half of the supply chain story: keeping the credentials your CI does need from leaking the same way Trivy's victims did.
- [ArgoCD in Production: Patterns That Actually Matter](https://www.skyhook.io/blog/argocd-in-production-patterns-that-actually-matter) - How a single GitOps plane turns "pin every image by digest" from a 50-place change into a one-place change.
- [Managing Kubernetes Add-ons: Argo CD or Terraform?](https://www.skyhook.io/blog/argo-cd-vs-terraform-for-kubernetes-add-ons) - Where Kyverno, cert-manager, and the rest of this audit's chassis actually live.
- [CERT-EU advisory on the European Commission cloud breach](https://cert.europa.eu/blog/european-commission-cloud-breach-trivy-supply-chain) - Primary source for the Europa.eu impact.
- [Aqua Security's incident write-up](https://www.aquasec.com/blog/trivy-supply-chain-attack-what-you-need-to-know/) and[Wiz's TeamPCP analysis](https://www.wiz.io/blog/trivy-compromised-teampcp-supply-chain-attack) - Original technical breakdowns of the attack chain.
