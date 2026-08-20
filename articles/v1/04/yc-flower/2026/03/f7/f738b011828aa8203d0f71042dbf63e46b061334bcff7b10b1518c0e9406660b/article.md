---
schema_version: "1.0.0"
document_id: "f738b011828aa8203d0f71042dbf63e46b061334bcff7b10b1518c0e9406660b"
company_key: "yc-flower"
company: "Flower"
source_id: "yc-flower-news-import-15689314081b"
canonical_url: "https://flower.ai/blog/2026-03-10-announcing-flower-hub"
published_at: "2026-03-10T00:00:00+00:00"
first_seen_at: "2026-07-21T20:31:29.302072+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:c290a2c9f2d7446906c7dacce564937dfe6e926bef5deb8f3ed8380a3cf83ea0"
---

# Announcing Flower Hub: The App Hub for Collaborative AI

Today, alongside our launch partners, we’re proud to launch **Flower Hub** — an app hub for publishing, discovering, and running Flower apps across heterogeneous environments.


Flower Hub allows developers to focus on what matters most: building decentralized and federated AI apps. The underlying infrastructure — from distribution to execution in both simulation and real-world deployments — is handled by Flower.


This launch represents a major step forward for Collaborative AI. It marks the transition from a world of isolated projects to an open community platform of shared, reusable and trusted apps.


The era of collaborative AI starts now.


## Why Flower Hub?


Over the past few years, decentralized and federated AI have advanced rapidly. Yet one challenge remained surprisingly persistent: sharing, reusing and building on the work of others in the field has been difficult.


One of our launch partners said it best:


> We built a federated system that worked beautifully — but sharing it with another team felt like handing over our entire infrastructure stack. Flower Hub lets us focus on the application itself, not the surrounding complexity.


Federated projects are often tightly coupled to specific infrastructure, deployment setups, and local environments. Reproducing results across teams typically requires manual configuration and deep system knowledge.


That friction slows innovation. Flower Hub removes it.


### Reproducibility Across Environments


> From prototype to production shouldn’t mean starting over. Flower Hub lets the same federated app run everywhere.


Flower Hub enforces a clean separation between application logic and infrastructure. The same app can run:


- In **Simulation Runtime** for development and experimentation
- In **Deployment Runtime** across distributed nodes


No code changes required.


This dramatically reduces the friction between experimentation and production which enables faster iteration and more reliable deployment.


### Shareable Federated Apps


> Federated learning across healthcare institutions using data from millions of patients in different countries — each with their own trusted research environments, data governance frameworks, and regulatory requirements — demands a combination of mathematics, bioinformatics, DevOps, and IT infrastructure expertise that few research groups have in one place. Flower Hub lets teams focus on the important clinical questions rather than the infrastructure.


With Flower Hub, AI researchers and engineers can publish federated applications in a standardized, reusable format.


Other users can:


1. Create new projects from them
2. Use them as references
3. Extend and customize them
4. Benchmark against them


Federated AI becomes modular and shareable — not siloed.


### One Command to Run


> Federated AI adoption depends on operational simplicity. One command to run an app can make the difference between experimentation and real-world deployment. Flower Hub simplifies execution and removes unnecessary friction.


Flower Hub apps can be executed with a single command:


```text
flwr run @flwrlabs/demo --stream
```


Note: This command requires flwr


version 1.27.0


or newer.


Simple execution lowers the barrier to entry and accelerates adoption across research and industry teams.


## How to Use Flower Hub


Flower Hub revolves around two core commands: one for creation, one for execution of[Flower Apps](https://flower.ai/apps) .


### Create a Local Project From an App on Flower Hub


```text
flwr new @flwrlabs/demo
```


This generates a local project based on a published app from the Flower Hub. Just like any other Flower app, it can be run locally using the Flower Simulation Runtime ( flwr run . local


) or in a distributed enterprise deployment using the Flower Deployment Runtime ( flwr run . supergrid


).


### Execute an App From Flower Hub


```text
flwr run @flwrlabs/demo --stream
```


Apps on the Flower Hub can be directly executed using flwr run


. Instead of pointing flwr run


at a local app project (e.g., flwr run .


), you point it at an app on the Flower Hub ( flwr run @flwrlabs/demo


).


For step-by-step instructions, see the[How to Use an App from Flower Hub](https://flower.ai/docs/hub/how-to-use-app-from-hub.html) guide.


## Call for Contributions


Flower Hub is not just a platform — it’s a community ecosystem!


We invite researchers, engineers, and organizations to publish:


- New federated learning algorithms
- Real-world federated applications
- Benchmarks
- Reproducible research implementations
- Tools and extensions for the federated ecosystem


Publishing is straightforward:


```text
flwr app publish .
```


This command uploads your app’s source files and metadata from the current project directory. Flower Hub then builds the FAB server-side and publishes the app version for others to discover and run.


For full setup and requirements, follow the[How to Publish an App on Flower Hub](https://flower.ai/docs/hub/how-to-publish-app-on-hub.html) guide.


Your work becomes discoverable, reusable, and verifiable by the entire Flower community.


Collaborative AI advances fastest when we build together.


## The Beginning of a Collaborative App Ecosystem


Flower Hub represents the next step for federated AI: From individual projects → to reusable applications → to a collaborative ecosystem


We can’t wait to see what you build.
🌐 Explore Flower Hub.
▶️ Run an app.
📦 Publish your own.


The future of AI is collaborative — and it starts today.


## Acknowledgement


We would like to sincerely thank our Launch Partners — BloodCounts!, Fraunhofer IMS, Gachon University, Mozilla AI, Owkin, Sony AI, SYNTHEMA, the University of Cambridge, the University of Melbourne, and the Vector Institute — for their invaluable support and collaboration in bringing Flower Hub to life.


We are equally grateful to all Early Contributors whose feedback, experimentation, and dedication helped shape Flower Hub into what it is today.
