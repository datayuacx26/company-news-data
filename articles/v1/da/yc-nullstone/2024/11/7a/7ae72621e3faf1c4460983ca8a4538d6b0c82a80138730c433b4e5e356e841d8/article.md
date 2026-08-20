---
schema_version: "1.0.0"
document_id: "7ae72621e3faf1c4460983ca8a4538d6b0c82a80138730c433b4e5e356e841d8"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/gitops-launch-week-day-4"
published_at: "2024-11-14T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:f2c099131e447cbbb1178c34e1584fc16c6767363e02f725e3b64cbe65b0d17c"
---

# GitOps Launch Week, Day 4

## Overview


At Nullstone, we believe developers should be able to submit a single pull request that contains everything needed to deliver a feature or bug fix. No manual configuration in another system. No manual steps at deploy time. Instead, fully automated and identical across environments.


All week, we’re launching Nullstone GitOps to achieving this goal. Nullstone GitOps is not just an infrastructure management tool and is not a replacement for ArgoCD/FluxCD. Stay tuned all week to learn more.


## IaC Auto-completion/Validation


Yesterday, we launched CLI commands to generate and test your IaC files. This helps developers tremendously, but sometimes changing a variable or connection leads to confusion for the developer. Instead of bouncing back and forth between the terminal, it would be better to see immediate feedback in the editor. Today, we released auto-complete and validation for Nullstone IaC files that is built-in to your favorite editor. If you’re using a supported editor, invalid syntax or invalid input will add visuals to indicate errors or warnings accordingly. Also, as you’re typing, your editor will reveal a set of valid choices.


The following editors are supported:
- Android Studio
- CLion
- Emacs via \[eglot\](https://github.com/joaotavora/eglot)
- IntelliJ IDEA
- JSONBuddy
- Neovim via \[SchemaStore.nvim\](https://github.com/b0o/SchemaStore.nvim)
- PhpStorm
- PyCharm
- ReSharper
- Rider
- RubyMine
- SublimeText via \[LSP-json\](https://packagecontrol.io/packages/LSP-json),\[LSP-yaml\](https://packagecontrol.io/packages/LSP-yaml)
- Visual Studio
- Visual Studio Code (\[YAML\](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml),\[TOML\](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml),\[JSON\](https://marketplace.visualstudio.com/items?itemName=remcohaszing.schemastore))
- Visual Studio for Mac
- WebStorm


## GitOps/IaC Docs


If you’re a fan of classic reference material or need to read more about GitOps or IaC format/attributes, we have launched a resource for you. We published a new section to the Nullstone docs site, “GitOps”. In this section, we detail the ins and out of GitOps workflows, examples, how it works, and best practices. Also in this new section, we detail examples, attributes, and reference for IaC files.


Check it out at \[GitOps\](https://docs.nullstone.io/gitops/overview.html).


‍
