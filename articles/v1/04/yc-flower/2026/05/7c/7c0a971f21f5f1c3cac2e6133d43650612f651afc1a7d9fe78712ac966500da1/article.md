---
schema_version: "1.0.0"
document_id: "7c0a971f21f5f1c3cac2e6133d43650612f651afc1a7d9fe78712ac966500da1"
company_key: "yc-flower"
company: "Flower"
source_id: "yc-flower-news-import-15689314081b"
canonical_url: "https://flower.ai/blog/2026-05-21-introducing-fab-format-version"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-21T20:31:29.302072+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:0fbcdaa975d05e9d9a3e844ea4b9e467d3cff9e91b1053c857a8575b18cacad7"
---

# Introducing the First Stable FAB Format for Flower Hub

We’re excited to introduce the first stable version of the Flower App Bundle (FAB) format for Flower Hub.


This is a foundational step for the Flower ecosystem.


Flower Hub is not just a place to upload apps, it’s the center of a growing ecosystem of collaborative AI applications, and for that ecosystem to thrive, apps must remain compatible, reusable, and future-proof.


The stable FAB format makes that possible.


## A Foundation for a Growing Ecosystem


As Flower evolves, so do the requirements for apps:


- Richer metadata
- Improved packaging standards
- Stricter validation rules


But evolution creates a challenge: **How do you introduce new capabilities without breaking existing apps?**


An ecosystem only works if:


- apps published today still work tomorrow
- new features don’t force immediate migration
- old and new versions can coexist seamlessly


The stable FAB format creates a clear compatibility boundary, giving developers confidence today while allowing Flower to innovate tomorrow.


## Why this matters


As a user or publisher of collaborative AI apps, you want:


- Apps that continue to work across platform updates
- A consistent and predictable packaging standard
- The ability to adopt new features on your own timeline


Without a defined app format, even small changes can introduce risk:


- New validation rules might unintentionally break older apps
- Newer apps have no clean way to signal advanced requirements


The stable FAB format solves this.


It provides a structured way to **preserve backward compatibility** while enabling **forward-looking innovation** .


## Powering Flower Hub


Flower Hub depends on a shared understanding of how apps are packaged and validated.


The FAB format is that shared contract.


It ensures that apps from different publishers can:


- Coexist in a single ecosystem
- Follow consistent validation rules
- Be reliably discovered, shared, and executed


This is not just a packaging detail; it’s **core infrastructure for the Flower ecosystem** .


With a stable FAB format, Flower Hub can evolve safely, introducing new requirements without disrupting what already works.


## How It Works


At the center of the FAB format is a simple but powerful field: fab-format-version


.


Defined under \[tool.flwr.app\]


in pyproject.toml


, this field tells Flower which app format and validation rules to apply during the build process.


For example:


```text
[project]
name = "my-federated-app"
version = "0.1.0"
dependencies = [
"flwr>=1.27.0,<=2.0.0",
]


[tool.flwr.app]
publisher = "your-username"
fab-format-version = 1
flwr-version-target = "1.28.0"
```


**Key points** :


- fab-format-version


is not the app version
- it is not the Flower runtime version
- it defines the app format and validation rules
- it governs both metadata and FAB content validation


If omitted, the app defaults to legacy format ( 0


).


## Built for Evolution


The stable FAB format is designed with long-term evolution in mind:


- Older apps continue to work under legacy rules
- Newer apps can adopt stricter validation when ready
- Platform updates no longer risk breaking the ecosystem


This means developers can move forward at their own pace, without sacrificing compatibility.


## For Flower Hub Publishers


If you publish apps on Flower Hub, now is the perfect time to upgrade.


By adopting the latest fab-format-version


, you:


- Make your app’s format explicit
- Align with current validation standards
- Prepare your app for future platform capabilities


Review your app metadata, validate your FAB, and republish using the latest format.


Follow the guide:[How to Publish an App on Flower Hub](https://flower.ai/docs/hub/how-to-publish-app-on-hub.html)


## Learn More


To explore the full details:


- [FAB Format Version Explainer](https://flower.ai/docs/hub/fab-format-version.html)
- [Flower Framework CLI Reference](https://flower.ai/docs/framework/ref-api-cli.html)


## In Summary


The first stable FAB format establishes a **reliable, forward-compatible foundation** for Flower Hub.


It ensures that:


- Today’s apps remain usable tomorrow
- New capabilities can be introduced safely
- The ecosystem can grow without fragmentation


**FAB is more than a file format, it’s the backbone of a scalable collaborative AI ecosystem.**
