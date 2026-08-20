---
schema_version: "1.0.0"
document_id: "6d89c08f0eeb4f0438a10e6632245f457ee0ed458c6310240bb7bfc8f26df707"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/rich-text-editable-blocks-and-html-rendering"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:3bf551a17e098a1b3b8a1b10e1e6c4e8e89ee52b5247b8a726bb3aa35450874f"
---

# Rich Text: Edit Blocks on the Page, Plus Framework-Agnostic Rendering

When we[launched the rich-text metafield](https://www.cosmicjs.com/changelog/rich-text-metafield-a-modern-editor-with-reusable-content-blocks) , reusable Content Blocks were global: edit a block in settings and every instance updates. That is exactly what you want for a shared CTA or footer. But sometimes you want to drop in a block as a starting point and tweak that one instance, like a callout with copy unique to a single article, without changing it everywhere else.


Now you can. This release adds **editable blocks** in the editor and a **framework-agnostic HTML renderer** for developers, two of the most requested follow-ups since launch.


## What's New


- **Edit a block on the page.** Every block now has an "Edit on page" action. Insert a reusable block, then detach this one instance to edit its content inline. The saved block and all its other instances stay untouched. It is the "drop in a snippet and customize it here" workflow, built right into the editor.
- **Synced or per-instance, clearly labeled.** Each block shows whether it is **Synced** (resolved from the saved block, updates everywhere) or **Edit on page** (content lives inline, unique to this page). Changed your mind? "Reset to synced" discards the on-page edits and reconnects the instance to the saved block, with a confirmation so it never happens by surprise.
- **The right editor for the content.** A detached block opens the same editor its type uses: the full rich-text editor for rich text, a code editor for HTML, and a plain text area for plain text.
- **Render anywhere, not just React.** The new function turns a rich-text value into a plain HTML string with no React dependency. Use it in Vue, Svelte, Astro, server templates, or any non-React stack. Import it from the entry point.
- **Backward compatible.** Existing content and synced blocks work exactly as before. There is no migration, and the stored format is unchanged for synced references.


## Why This Matters


Reusable blocks and one-off, editable blocks solve different problems, and good content tooling needs both. A global block keeps shared content consistent. An editable instance lets a writer adapt a pattern (a callout, a pull quote, a stat highlight) to a single piece without leaving the post or creating a separate Object to link to.


On the developer side, rich-text was already stored as portable Markdown with tokens you resolve at render time. The missing piece for non-React teams was a simple way to get an HTML string. closes that gap while keeping the same control-first model: you decide the markup for every block and embed.


## How It Works


In the editor, insert a block from the slash menu as usual. To customize a single instance, click **Edit on page** on the block. It detaches from the saved block and becomes editable right there in your content. Synced instances keep tracking the saved block. To undo a detach, click **Reset to synced** and confirm: the inline edits are discarded and the instance references the saved block again.


For non-React front ends, render the value to an HTML string:


```text


```


You can customize the markup for each block with , and resolve inline Object embeds to your own HTML with . React apps keep using the component and as before.


## Packages & Resources


- **[@cosmicjs/rich-text](https://www.npmjs.com/package/@cosmicjs/rich-text)** (v0.2.0): adds the framework-agnostic renderer via the new entry point (no React dependency), and renders editable, per-instance blocks.
- **Docs** :[Rich text and blocks](https://www.cosmicjs.com/docs/api/rich-text) , including the new "Render to an HTML string (no React)" section.


New to Cosmic? Create your free account, add a Rich Text field, and start writing.
