---
schema_version: "1.0.0"
document_id: "133b4caf11cee6fdb8e87992e88581da616f16458044fe9012e33f0ee4a7c4e4"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/shipwright-v-0-1-0/"
published_at: "2024-04-16T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:3e7297d6d5cf70e2a2e3f1e0c95a76d81849cae89947f39be1bf141dbd4f2ffe"
---

# Shipwright 0.1.0

In this release of Sails Shipwright, the much-requested DX of HMR (Hot Module Replacement) and live reload has finally been shipped 🚀.


As an old Sails user, I’ve grown accustomed to hitting refresh like a caveman, so HMR and live reload weren’t necessities for me 😅. However, for newer Sails users joining via The Boring JavaScript Stack, HMR is a must-have.


Sails Shipwright, built on[Rsbuild](https://rsbuild.dev/) as the modern asset pipeline for Sails, now offers HMR and live reload for full-stack Sails apps, courtesy of the custom dev server feature introduced in[Rsbuild v0.5.0](https://rsbuild.dev/community/releases/v0-5#-support-for-custom-server) .


Also see the[full changelog](https://github.com/sailshq/sails-hook-shipwright/releases/tag/v0.1.0) on GitHub.


## ✅ Upgrading


To start enjoying the DX of HMR and live reload in your Sails applications and in The Boring JavaScript Stack, simply upgrade` sails-hook-shipwright` in your project.


```text
npm   i   sails-hook-shipwright@latest   -D
```
