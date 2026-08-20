---
schema_version: "1.0.0"
document_id: "f313f9acd21ab6932c6f504c81b1d6362e91ea70f88387655bca771e352ca585"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/a-notion-style-editor-ready-to-use-in-tiptap-cloud"
published_at: "2025-07-24T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:15530f0fa672a149dae5411c6f9f090f9a5130dd9c08bdb974e77b9e3cdbb10b"
---

# A Notion-style editor, ready to use in Tiptap Cloud

It includes a familiar block-based interface, real-time collaboration, AI-assisted writing, and version history — all integrated with the Tiptap framework and UI component system. The goal is to offer a reliable starting point for teams building collaborative, structured writing tools.


## Background


Many teams using Tiptap end up building editors that resemble modern document tools like Notion. These editors have become the expected baseline: block-based layout, multiplayer collaboration, slash commands, undo history, dark mode.


Recreating that from scratch takes time — not just for the frontend, but for syncing logic, persistence, AI integrations, and design consistency. We’ve done this work internally to validate our own stack, and we’re now making that template available so others can build on it.


## What the template includes


This is a React-based implementation that combines Tiptap’s editor engine, our growing library of UI components, and several Cloud features.


**Out of the box, it supports:**


- Block-based editing with drag-and-drop reordering
- Slash and context menus for quick actions
- Real-time collaboration with presence indicators
- Integrated AI tools (rewrite, autocomplete, generate)
- Responsive layout with light/dark mode
- Support for image uploads, and YouTube embeds.


The design system is simple and neutral, aiming to be useful as-is or easy to adapt.


## Requirements


To use the template, you’ll need:


- React
- [Tiptap Editor](https://tiptap.dev/product/editor)
- [Tiptap UI Components](https://tiptap.dev/product/ui-components)
- [Tiptap Cloud account](https://tiptap.dev/archive/pricing-q2-25) (Start plan or higher)
- [Tiptap CLI](https://tiptap.dev/docs/ui-components/getting-started/cli)


If you don’t need[collaboration](https://tiptap.dev/docs/collaboration/getting-started/overview) ,[versioning](https://tiptap.dev/docs/collaboration/documents/history) or[AI](https://tiptap.dev/docs/content-ai/getting-started/overview) , the[Simple Editor](https://tiptap.dev/docs/ui-components/templates/simple-editor) is available as a lightweight open source alternative.


## Feature overview


Feature OSS (Simple Editor) Cloud (Notion Template)


Core Editing ✅ ✅


UI Pro Components ❌ ✅


Real-time Collaboration ❌ ✅


AI Commands ❌ ✅


Drag-and-Drop Blocks ❌ ✅


## How to try it


👉[Live preview](https://template.tiptap.dev/preview/templates/notion-like/)


👉[Start building](https://tiptap.dev/docs/ui-components/templates/notion-like-editor)


If you're already using Tiptap Cloud, you have everything you need.


The code is structured to be used as a starting point — not a final product. You can extend it, or strip it down, depending on your use case.


## What’s next


We're continuing to build UI components that match the structure and patterns in this template. The next releases will include:


- Comments and threaded discussions
- Table of contents blocks
- Column layouts
- Table/grid support


All will integrate with the same Cloud-backed architecture.


## We’d appreciate your input


If there are components or features you find yourself looking for repeatedly, we’d love to hear about them. **Drop us a message, open an issue, or start a discussion. We’re listening!**


Our goal is to make it easier to ship structured editors that feel modern, collaborative, and stable out of the box.


Thanks for building with Tiptap.


— The Tiptap team
