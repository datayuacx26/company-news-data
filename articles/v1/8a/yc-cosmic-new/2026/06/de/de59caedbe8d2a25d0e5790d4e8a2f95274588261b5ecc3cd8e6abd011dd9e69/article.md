---
schema_version: "1.0.0"
document_id: "de59caedbe8d2a25d0e5790d4e8a2f95274588261b5ecc3cd8e6abd011dd9e69"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/rich-text-metafield-a-modern-editor-with-reusable-content-blocks"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:74946d10a72ec23a1535269f89d8388e9b8904d78e6886be24e08f66e622ad17"
---

# Rich Text Metafield: A Modern Editor with Reusable Content Blocks

Cosmic now has a new **rich-text** metafield: a modern writing experience for long-form content, backed by portable Markdown and a control-first rendering model. It ships with **Content Blocks** (reusable snippets you define per Bucket) and **inline Object embeds** (drop a live Object into your prose), so editors get a clean editor and developers keep full control of the output.


This is the recommended field for all new body and article content. The previous HTML editor still works, but rich-text is where we are investing going forward.


## What's New


- **A modern rich-text editor.** Format with a toolbar or a slash menu: headings, lists, task lists, tables, links, inline code, and full-screen writing. Paste images straight from your clipboard or drag and drop them in, and they upload to your Media library automatically.
- **Reusable Content Blocks.** Define blocks once in your Bucket settings and reuse them anywhere. Each block can be rich text, raw HTML, or plain text, and authors insert it with a simple token from the slash menu.
- **Inline Object embeds.** Reference any existing Object directly inside your content with . Embeds stay live, so the rendered content always reflects the current Object.
- **Portable Markdown storage.** Content is stored as Markdown, not locked HTML. It stays clean, diff-friendly, and easy to move, query, and reuse across channels.
- **Control-first rendering.** A new endpoint returns your block definitions, and the new React package renders the content. You decide exactly how every block and embed looks in your front end. Nothing is baked into a locked HTML string.
- **AI that knows the field.** Autopilot and Team Agents now prefer rich-text when generating content models and writing long-form content.
- **Backward compatible.** The legacy HTML editor () is now marked as legacy but remains fully functional. No migration is required, and existing fields keep working exactly as before.


## Why This Matters


Long-form content has always sat awkwardly between two extremes: a rigid HTML blob that is hard to restyle and reuse, or plain Markdown with no way to drop in reusable, structured pieces like a call to action or a product card.


The rich-text metafield closes that gap. Editors get a fast, familiar editor. Developers get clean Markdown plus structured tokens for blocks and embeds, and a rendering model where the front end stays in charge. You can change how a block looks across every post by updating one component, instead of editing locked markup inside hundreds of records.


## How It Works


1. Add a **Rich Text** field to any Object Type, or switch a long-form field to it.
2. Write content in the editor. Use the slash menu to add headings, media, AI-generated text, Content Blocks, and Object embeds.
3. Define reusable **Content Blocks** in your Bucket settings (rich text, HTML, or plain text). They become available in the slash menu as tokens.
4. In your front end, fetch the Object, fetch block definitions from the blocks endpoint, and render with :


```text


```


Because the content is Markdown and the blocks resolve at render time, you control the markup, styling, and behavior of every block and embedded Object.


## Try It


Open any Object Type, add a **Rich Text** field, and start writing. Create a Content Block in your Bucket settings, then insert it with the slash menu to see reusable content in action. When you are ready to render, install and pass your blocks.


New to Cosmic?[Create your free account](https://app.cosmicjs.com/signup) , model your content with rich-text, and publish in minutes.


## Packages & Resources


Everything you need to render rich-text content is available now on npm:


- **[@cosmicjs/rich-text](https://www.npmjs.com/package/@cosmicjs/rich-text)** (v0.1.0): React renderer that expands and tokens into components you control.
- **[@cosmicjs/sdk](https://www.npmjs.com/package/@cosmicjs/sdk)** (v2.1.0): adds to fetch your Bucket's block definitions.
- **[@cosmicjs/cli](https://www.npmjs.com/package/@cosmicjs/cli)** (v1.10.0): rich-text-aware content modeling from the terminal.
- **[@cosmicjs/mcp](https://www.npmjs.com/package/@cosmicjs/mcp)** (v1.4.0): rich-text support for AI tools and agents.
- **Blocks API** : returns your block definitions.
