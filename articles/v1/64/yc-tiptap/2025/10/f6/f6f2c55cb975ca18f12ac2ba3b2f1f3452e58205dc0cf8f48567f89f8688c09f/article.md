---
schema_version: "1.0.0"
document_id: "f6f2c55cb975ca18f12ac2ba3b2f1f3452e58205dc0cf8f48567f89f8688c09f"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/introducing-bidirectional-markdown-support-in-tiptap"
published_at: "2025-10-15T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:e525a7c918404544a24ba0ff085f16c3ad74c928152d2ed138694aa7e5d4c54b"
---

# Introducing bidirectional Markdown support in Tiptap

Markdown has become a common bridge between visual editors, developer tools, and AI systems. Many Tiptap users already store or exchange content as Markdown. With the new open-source` @tiptap/markdown` extension, teams can now move content between Markdown and rich text without custom converters or data loss.


“Bidirectional” means you can parse Markdown into Tiptap and serialize editor content back to Markdown — with full support for round-tripping content.


The Markdown system is split into modular layers, so you can use only what you need. From simple Markdown conversion to deep customization.


## How it works


The` @tiptap/markdown` extension integrates[MarkedJS](https://marked.js.org/) for parsing, providing a fast, extensible, and CommonMark-compliant foundation for handling Markdown content.


The new Markdown API is centered around a` MarkdownManager` , which handles parsing and serializing Markdown to and from Tiptap’s ProseMirror document structure. It’s fully modular, so you can register custom extensions, tokenizers, or serialization logic.


Markdown coverage follows CommonMark. If you use highly custom extensions, you may need to define parsing and serialization rules manually.


You can use Markdown in a few ways:


- **Direct integration:** Pass Markdown input and output through the` Editor` instance using the new Markdown API.
- **Custom extensions:** Build extensions that parse or serialize specific Markdown patterns, like highlight syntax, emoji, or admonitions.
- **Custom tokenizers:** Extend Markdown’s behavior to support nonstandard syntax or internal logic.


## Key parts of the API


- [Editor Markdown API](https://tiptap.dev/docs/editor/markdown/api/editor) : Provides Markdown-specific methods on the Tiptap editor instance.
- [MarkdownManager](https://tiptap.dev/docs/editor/markdown/api/markdown-manager) : Controls parsing and serialization, with full access to tokens and node mappings.
- [Extension API](https://tiptap.dev/docs/editor/markdown/api/extension) : Lets extensions define how they’re represented in Markdown.
- [Utilities](https://tiptap.dev/docs/editor/markdown/api/utilities) : Helper functions for working with tokens, nodes, and Markdown text.
- [Types](https://tiptap.dev/docs/editor/markdown/api/types) : TypeScript definitions for consistent usage and extension development.


## Installation


To add the Markdown extension to a project, install the package:


```text
npm install @tiptap/markdown
```


## Example usage


Here’s a minimal setup:


```text
import   { Editor }   from     '@tiptap/core
import { Markdown } from '  @tiptap/markdown  '


const editor = new Editor({
extensions: [Markdown],
content: '  # Hello Markdown!  ',
contentType: '  markdown  ',
})


// Get Markdown output
console.log(editor.storage.markdown.getMarkdown())
```


` ‍` You can find more examples in the[Markdown documentation](https://tiptap.dev/docs/editor/markdown/examples) , including custom syntax for highlighting, admonitions, and inline emoji.


The Markdown extension is compatible with any existing Tiptap setup and supports custom extensions. Developers can configure their extensions to define how they are represented.


## Significance for Developers & LLM Workflows


Markdown is still the most common text format for developers and LLMs. With native Markdown support, Tiptap can now serve both structured editing and lightweight text workflows. You can load Markdown, edit it visually, and export it back to Markdown without losing fidelity.


If you’re building documentation tools, note-taking apps, or AI-driven editors, this makes Tiptap an even more flexible foundation.


## **‍We’d Love Your Feedback!**


With bidirectional Markdown support, Tiptap now connects structured editing with the Markdown ecosystem used by developers, AI models, and documentation tools.


If you have any feedback or suggestions, reach out athumans@tiptap.dev or join our[Discord Community](https://tiptap.dev/discord) .


Thanks for building with Tiptap.
The Tiptap team
