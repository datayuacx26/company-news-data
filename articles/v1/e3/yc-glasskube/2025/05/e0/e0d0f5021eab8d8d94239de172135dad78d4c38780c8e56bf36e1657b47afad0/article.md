---
schema_version: "1.0.0"
document_id: "e0d0f5021eab8d8d94239de172135dad78d4c38780c8e56bf36e1657b47afad0"
company_key: "yc-glasskube"
company: "Glasskube"
source_id: "yc-glasskube-news-import-f40b83a58804"
canonical_url: "https://distr.sh/blog/distr-registry/"
published_at: "2025-05-22T00:00:00+00:00"
first_seen_at: "2026-07-21T21:44:16.826534+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:ebd0141f96da0c43adb08b074c59a5ad4e7552eb73e375f5f13a72f8a46f7554"
---

# Distr Launch Week: The Distr Artifact Registry is GA

Today, we are happy to announce that the Distr[artifact registry](https://distr.sh/docs/registry/) is now Generally Available (GA).


Manage and distribute container images,[Helm charts](https://distr.sh/glossary/helm-chart/) , and other[OCI artifacts](https://distr.sh/glossary/oci-container-artifact-registry/) with license-based access control, audit logs, and a visual UI that makes[self-hosted](https://distr.sh/glossary/self-hosted-software/) software distribution pretty darn simple if we do say so ourselves, built for both you and your customers. These[software and AI companies](https://distr.sh/glossary/isv-meaning/) need robust distribution solutions.


---


## Why we built a registry into Distr


Distr was originally built to help vendors manage software distribution in agent based[assisted self-managed](https://distr.sh/docs/use-cases/assisted-self-managed/) environments. But as end customers increasingly requested full control over their deployments, vendors needed a way to offer a[fully self-managed model](https://distr.sh/docs/use-cases/fully-self-managed/) as well. Other third-party registries lacked the fine-grained access controls and visibility these scenarios call for, so that’s why we built an OCI-compliant registry directly into Distr, enabling both assisted and fully self-managed distribution, with features like tag-based access control and detailed download logs.


## Why is Self-Hosted artifact distribution hard?


In case you are unsure whether this is a feature you need, consider this.


Let’s say a new customer is ready to deploy your self-hosted solution. Contracts signed. Excitement is high. But then you get a flurry of messages from your end customers along the lines of…


> “We can’t access your registry.” “Is our firewall blocking the pull?” “Which version are we supposed to use?”


Sound familiar?


Vendors often don’t have a standard way to set up credentials, provide pull instructions, and troubleshoot artifact access issues. Customers get frustrated. Deployment stalls.


With Distr’s built-in registry, those problems are largely mitigated and planned for in advance.


## Meet the Distr Artifact Registry


Built for software vendors who serve end-customer with different[deployment appetites](https://distr.sh/docs/use-cases/fully-self-managed/) , our registry brings OCI-compliant artifact distribution natively integrated into Distr.


Here are a few of its key features:


### OCI-compliant & format-flexible


The registry supports any[OCI artifact](https://distr.sh/glossary/oci-container-artifact-registry/) , including:


- Docker images
- [Helm charts](https://distr.sh/glossary/helm-chart/)
- WASM modules
- Anything else that follows the OCI spec


### License-based access control


Vendors can restrict access by using[Artifacts licenses](https://distr.sh/glossary/software-license-management/) . Grant or revoke permissions to one or many artifacts at the tag level. Update instantly when entitlements change.


### Visual management


Use any OCI compliant CLI for pushing and pulling, and use the Distr Registry UI to:


- View artifact versions or tags
- See which customer pulled what, and when
- Track deployment status across environments


**General Artifact UI**


**Deployment tracking UI**


The Downloads page gives granular artifact consumption logs


**Download audit logs UI**


## What’s next for the Registry?


We’re just getting started, here’s what’s coming soon:


- Integrated[CVE scanning](https://distr.sh/glossary/cve-common-vulnerabilities-and-exposures/) for registry artifacts
- Download history query & export
- Visual indicators and download for[SBOM](https://distr.sh/glossary/sbom-software-bill-of-materials/) &[image signature](https://distr.sh/glossary/oci-container-artifact-registry/#image-signing-and-verification) layers


## Give it a try today


The Distr container registry has already distributed thousands of artifact versions to end customers. Often completely white-labeled, with a custom CNAME DNS record, as part of a vendors software supply distribution stack.


The registry is available now in all Distr accounts. Just head to the Artifacts tab in the dashboard to[get started](https://distr.sh/docs/registry/configuration/) .


- Read the[docs](https://distr.sh/docs/registry/)
- Contact us for a[demo](https://cal.glasskube.com/team/gk/demo)


We are excited to hear your thoughts on the registry and open to your feedback to further shape the product to make it as useful as possible for vendors and end-customers alike.
