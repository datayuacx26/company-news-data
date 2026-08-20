---
schema_version: "1.0.0"
document_id: "efa639cb37f559e8efd667116a19c75811e0f6fbec4df5e81545a0bfcc1584ca"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/search-and-replace-for-tiptap"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T21:22:41.447671+00:00"
fetched_at: "2026-08-06T21:22:42.659311+00:00"
content_hash: "sha256:4d1301e1af97fc3e7e55f0ebc9275db03d1286c9f65d864d3a5d6043d805c16f"
---

# Search and replace for Tiptap

Search and replace is one of those things every editor needs and nobody wants to write. It's not hard to build, just fiddly. Matching across marks, keeping the current result stable while the document changes, not fighting with the browser's own find bar.


To save you from having to build it with one prompt and then spend weeks fixing all the edge cases, we built for you: a headless extension that does the matching and the commands, and a UI component if you don't want to build the panel yourself. Both are MIT licensed.


---


## The extension


Install the package first, then add the extension to your editor. From there, it's headless and gives you everything you need to build your own search and replace UI.


```text
npm install @tiptap/extension-find-and-replace
```


From there, initialize your editor and start using the command:


```text
import   { Editor }   from     '@tiptap/core'
import   { FindAndReplace }   from     '@tiptap/extension-find-and-replace'
import   { StarterKit }   from     '@tiptap/starter-kit'


const   editor =   new   Editor({
element  :   document  .querySelector(  '#editor'  ),
extensions  : [StarterKit, FindAndReplace],
})


editor.commands.setSearchTerm(  'Tiptap'  )
editor.commands.setReplaceTerm(  'Editor'  )
editor.commands.replaceAll()
```


It supports result navigation, case-sensitive and whole-word matching, and regular expressions.


See the documentation for the full API and details about regex support.


## The UI component


If you don't want to build the panel, install ours:


```text
npx @tiptap/cli@latest add search-and-replace
```


You get a search panel with a result counter, next and previous, match case, whole word, regex, replace and replace all. It owns the search state, but not its position or its open state. So you can dock it in a corner, put it in a popover, or render it inline in a mobile toolbar. There's also a` useSearchAndReplace()` hook if you want the state and the actions with your own markup.


Two behaviors worth calling out.` Mod+F` is captured for the whole page while the panel is mounted, open or closed, so the browser's find-in-page never competes with it. Set` enableShortcut` to` false` if you'd rather leave that to the browser. And navigating results moves the highlight only. The editor selection stays where the user left it, so your toolbar state and floating menus don't react on every next and previous.


---


## Links


- Extension docs:[tiptap.dev/docs/editor/extensions/functionality/find-and-replace](https://tiptap.dev/docs/editor/extensions/functionality/find-and-replace)
- UI component docs:[tiptap.dev/docs/ui-components/components/search-and-replace](https://tiptap.dev/docs/ui-components/components/search-and-replace)
- Custom UI guide:[tiptap.dev/docs/guides/find-and-replace](https://tiptap.dev/docs/guides/find-and-replace)
- GitHub:[packages/extension-find-and-replace](https://github.com/ueberdosis/tiptap/tree/main/packages/extension-find-and-replace/)
