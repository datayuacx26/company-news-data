---
schema_version: "1.0.0"
document_id: "0b21cff7602a4c30f242bd0c8b4f3bc1f28a7862a73122acee6adef261b202ef"
company_key: "yc-diffusion-studio"
company: "Diffusion Studio"
source_id: "yc-diffusion-studio-news-import-a88cd81ba245"
canonical_url: "https://diffusion.studio/changelog/text-to-lottie-text-edit/"
published_at: "2026-07-05T00:00:00+00:00"
first_seen_at: "2026-07-21T16:21:29.490873+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:acbeddc78a1f98f2501833557940a0e47886053c20d5c88fc4ca288743b22258"
---

# Native font support and text editing

Text-to-lottie now supports native text rendering and editable text slots. Scenes can include real fonts, text slots appear in the properties panel, and edits update the canvas instantly on commit. Exports bundle fonts alongside the Lottie JSON.


```text
npx   skills   add   diffusionstudio/lottie
```


[GitHub - diffusionstudio/lottie: Generate production-ready Lottie animations with Claude Code or Codex github.com](https://github.com/diffusionstudio/lottie)


## Native text and editing


The player renders font-backed Lottie text natively. Text slots show up in the properties panel with their current values. Change the text and the canvas updates instantly. Colors and numbers work the same way.


One control can drive multiple slots at once using grouped targets. Repeated text like brand names, dates, or CTAs stays in sync from a single field. Grouped slots are hidden from the panel so they do not clutter the UI.


0:00 / 0:00


## Templates


Generate a Lottie template once, then reuse it by changing the content from the properties panel. Swap the headline, description, colors, and export a new version without regenerating the animation.


0:00 / 0:00


Same brand, different composition. Existing scenes give the skill context to produce new ones that stay consistent.


0:00 / 0:00


## What else is new


- Multiline text wrapping using box text for longer copy
- Unicode safe text persistence for accented characters, symbols, and special characters
- Ordered slot controls in the properties panel
- Bug fixes
