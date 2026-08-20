---
schema_version: "1.0.0"
document_id: "9483c84e491d438179f7dd62f7201c3b863eef9fda8bf4dd680c6fc5d90f1f59"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/kubectl-artillery-plugin-demo"
published_at: "2022-05-12T00:00:00+00:00"
first_seen_at: "2026-07-21T22:43:25.559968+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:15f8f54d7b37191d7e67eb564aec4b1a71340f5fc5aa1b83d8956660889fa999"
---

# Kubectl Artillery Plugin demo

May 12th, 2022[How to](https://www.artillery.io/blog/tag/howto)


# Kubectl Artillery Plugin demo


Ezo Saleh


Our new[kubectl-artillery](https://github.com/artilleryio/kubectl-artillery) plugin helps developers and testers boostrap Artillery testing on Kubernetes.


See it in action in the video below. This video walks through:


- The[k8s-testing-with-kubectl-artillery](https://github.com/artilleryio/artillery-examples/tree/main/k8s-testing-with-kubectl-artillery) example.
- Scaffolding Artillery test-scripts from running Kubernetes services.
- Generating tests that target Kubernetes services.
- Running the generated tests.


To use the plugin we assume you have[kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) installed and some familiarity with Kubernetes in general.


Install the plugin by following the[instructions here](https://github.com/artilleryio/kubectl-artillery#installation) for your target OS.


Here’s an example of installing it on macOS (Darwin) amd64:


```text
curl -L -o kubectl-artillery.tar.gz https://github.com/artilleryio/kubectl-artillery/releases/download/v0.2.2/kubectl-artillery_0.2.2_darwin_amd64_2022-05-11T16.49.36Z.tar.gz
tar -xvf kubectl-artillery.tar.gz
sudo mv kubectl-artillery /usr/local/bin
```


Cloud native testing for the win! 🔌☁️
