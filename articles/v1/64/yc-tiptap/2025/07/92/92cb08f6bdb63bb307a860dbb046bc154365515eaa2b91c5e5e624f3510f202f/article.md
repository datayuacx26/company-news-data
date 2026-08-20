---
schema_version: "1.0.0"
document_id: "92cb08f6bdb63bb307a860dbb046bc154365515eaa2b91c5e5e624f3510f202f"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/tiptap-3-0-is-stable"
published_at: "2025-07-12T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:5634863db4e42e2d69475ef8be12ab7ffd19fc13b5e51f32a0cc5ec5459fb668"
---

# Tiptap 3.0 is stable

## Everything that changed in 3.0


Tiptap 3.0 comes with a bunch of improvements, cleanup, and a few breaking changes you’ll want to be aware of. Here’s what’s new.


### **Highlights**


Area What changed


Meta‑packages We bundled common extensions into @tiptap/extensions, @tiptap/extension‑list, @tiptap/extension‑table, and @tiptap/extension‑text‑style. Less boilerplate, better tree-shaking.


UI / Menus All floating menus now use **Floating UI** . Smaller bundle, better positioning, same API (just rename tippyOptions to options).


Pro → OSS Features like Drag-handle, Emoji, Math, File Handling, and more are now open-source.


Server‑friendly You can now create editors without DOM elements. Great for SSR frameworks like Next.js and Nuxt.


Static Renderer Convert Tiptap content to HTML, Markdown, React components, or your own format with @tiptap/static-renderer.


Starter Kit Now includes **Link** , **ListKeymap** , and **Underline** out of the box. One install, ready to go.


[What’s new →](https://tiptap.dev/docs/resources/whats-new)


### **⚠️ Breaking changes**


What changed What you need to do


Lists Use @tiptap/extension‑list and the new ListKit.


Tables Same deal: @tiptap/extension‑table and TableKit.


Undo/Redo It’s now UndoRedo inside @tiptap/extensions.


Text styling Use the new @tiptap/extension‑text‑style package and TextStyleKit.


Placeholder The considerAnyAsEmpty option is gone.


UMD builds We ship ESM and CJS only now.


Collab Caret Renamed to @tiptap/extension‑collaboration‑caret.


Floating UI Add this peer dep: npm i @floating-ui/dom@^1.6.0


[Upgrade guide →](https://tiptap.dev/docs/guides/upgrade-tiptap-v2)


### Core API improvements


- ` insertContent` and` insertContentAt` no longer split paragraphs into ghost nodes
- ` setContent` and` clearContent` now emit updates
- TypeScript types are stricter and smarter
- ` unmount()` lets you cleanly reuse an editor
- New MarkView system for rendering inline UI (like color pickers)
- New helpers:` delete` ,` canInsertNode` , and attribute validators
- DevTools toggle with` enableDevTools: true`
- Better transaction deduping to save CPU cycles


### Extension updates


Extension Update


HorizontalRule can() now returns correct insertability


Image Now supports width and height


Link Supports multiple triggers like @, #, etc.


Mention Supports multiple triggers like @, #, etc.


TextAlign Added toggleTextAlign command


Task List Optional accessibility labels for checkboxes


Math Caret Now nodes (not marks) for better performance; migrate with migrateMathStrings()


YouTube Better URL matching


Emoji Now outputs alt text for screen readers


### **Fixes and polish**


- Drag-and-drop shows proper previews and doesn’t wipe content from non-editable editors
- Floating menus clean up resize listeners on destroy
- Cut bugs, plugin unregistration bugs, Jest + ESM issues: gone
- Image insert handling is smoother, especially for multiple files


## What’s next for the open-source core


Tiptap 3.0 will slowly make its way into apps over the coming months and years. Meanwhile, we’re already working on the next batch of features for the 3.x line:


### **Markdown support**


You’ll be able to load and export content as Markdown, like` renderMarkDown()` and` parseMarkDown()` . This helps with markdown-based workflows and makes life easier for LLMs.


### **Decorations API**


A Decorations API allows you to influence document presentation without altering its content, providing an intuitive way to add visual enhancements beyond complex ProseMirror internals.


### **Content migrations**


Content migrations will allow you to rewrite your document JSON to align with your current schema, facilitating document updates during schema changes with fully customizable solutions tailored to your needs.


## **UI components and templates now run on 3.0**


The Tiptap UI components and the simple editor template are already using 3.0 under the hood. If you’re building on top of them, you’re already set up for the future.


## **Join the community**


Tiptap is built with and for the open-source community. If you’re using Tiptap or building something cool on top of it, we’d love to hear from you:


- [Tiptap Discord](https://tiptap.dev/discord)
- [Tiptap on X](https://x.com/tiptap_editor)
- [Tiptap on BlueSky](https://bsky.app/profile/tiptap.dev)
- [Tiptap GitHub](https://tiptap.dev/github)


Thanks for building with us,
The Tiptap Team
