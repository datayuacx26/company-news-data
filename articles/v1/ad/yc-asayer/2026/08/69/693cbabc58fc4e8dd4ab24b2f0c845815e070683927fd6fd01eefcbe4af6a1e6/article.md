---
schema_version: "1.0.0"
document_id: "693cbabc58fc4e8dd4ab24b2f0c845815e070683927fd6fd01eefcbe4af6a1e6"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/browser-editing-contenteditable/"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T08:48:12.350603+00:00"
fetched_at: "2026-08-13T08:48:14.609766+00:00"
content_hash: "sha256:81be2d71f5f47da8f191bdf21f319fafa482f56777fc002f71aae4cb02cab623"
---

# In-Browser Editing with contentEditable

Any HTML element becomes editable in place when you add the` contenteditable` attribute — no form control, no library, no dependencies.


If you’ve ever built a whole form just to let someone rename one heading, the first time you try this it feels like a cheat code.


The browser turns the element into an editing host, drops a caret in, and lets the user type directly into the rendered DOM. That makes` contenteditable` the fastest way to ship an editable heading, a click-to-edit field, or a lightweight notes area. It also comes with sharp edges the attribute reference never mentions: there’s no native` change` event, the markup it generates varies across browsers, the old formatting API is deprecated, and rendering the result back to other users is a textbook XSS sink. This article covers how to turn it on, capture and persist edits correctly, handle those edge cases, and decide when to reach for something else.


## Key Takeaways


- The` contenteditable` attribute takes three values:` true` (or an empty string) makes an element editable,` false` disables it, and` plaintext-only` allows editing raw text while stripping rich-text formatting.
- ` contentEditable` has no native` change` event. Listen for the` input` event, which fires on every modification to the editing host.
- ` document.execCommand()` for bold, italic, and links is deprecated and non-standard; use the Selection and Range APIs with` beforeinput` /` input` , or a dedicated editor library, for real rich text.
- Never write user-entered` contenteditable` HTML back into the page without sanitizing — use DOMPurify, or the browser’s` setHTML()` where available with a DOMPurify fallback.
- ` contenteditable="plaintext-only"` is now cross-browser, having shipped in Firefox 136 (March 2025) alongside long-standing Chromium and WebKit support.


## Turn it on: the contenteditable attribute


The` contenteditable` global attribute takes three values, and picking the right one is most of the battle.` true` (or an empty string) makes an element editable;` false` disables it; and` plaintext-only` makes the raw text editable while disabling rich-text formatting. Per[MDN’s contenteditable reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/contenteditable) , it is an enumerated attribute, not a boolean: a missing or invalid value inherits editability from the parent.


The one-line version:


```text
<  h1   contenteditable  =  "  true  "  >  Edit this heading  </  h1  >
```


For a text-only field (a renameable title, a tag input, a single-line note), prefer` plaintext-only` . It blocks pasted rich formatting at the source: content pasted into an element with` contenteditable="true"` retains all formatting, while content pasted into` contenteditable="plaintext-only"` has all formatting removed.


Toggle editing from JavaScript through the` contentEditable` property (camelCase):


```text
const   el   =   document  .  querySelector  (  '  #note  '  );
el  .  contentEditable   =   '  plaintext-only  '  ;   // or 'true' / 'false'
```


## How do you capture and persist contenteditable edits?


` contentEditable` has no native` change` event. To capture edits, listen for the[input event](https://developer.mozilla.org/en-US/docs/Web/API/Element/input_event) , which fires on every modification to the editing host. This is the single most common mistake in older tutorials, which reach for` keypress` or` keyup` and miss pastes, drag-and-drop, and IME input. Read` element.innerHTML` when you need to preserve formatting, or` element.textContent` when you want plain text, then persist and restore on load.


```text
const   el   =   document  .  querySelector  (  '  #note  '  );


// Restore on load
el  .  textContent   =   localStorage  .  getItem  (  '  note  '  )   ??   ''  ;


// Debounced persistence on every edit
let   t  ;
el  .  addEventListener  (  '  input  '  , ()   =>   {
clearTimeout  (  t  );
t   =   setTimeout  (()   =>   {
localStorage  .  setItem  (  '  note  '  ,   el  .  textContent  );
// or: fetch('/api/note', { method: 'POST', body: el.textContent })
},   400  );
});
```


Swap` textContent` for` innerHTML` if you’re storing rich markup, but read the security section first, because that choice is what turns a notes field into an attack surface. For finer control, the[beforeinput event](https://developer.mozilla.org/en-US/docs/Web/API/Element/beforeinput_event) fires before the DOM mutates and lets you inspect or cancel an edit; it applies to` contenteditable` elements and to any element in` designMode` .


## The sharp edges


This is where` contenteditable` earns its reputation. Three problems bite in production.


**Messy, inconsistent markup.** Browsers disagree on the HTML a` contenteditable` region produces, so the saved output is rarely as clean as you expect. As[Scott O’Hara documented](https://www.scottohara.me/blog/2014/05/08/contenteditable.html) , Safari has historically wrapped line breaks in` <div>` elements while Firefox inserts` <br>` elements, and` <div>` is an invalid child of` <p>` , which causes rendering quirks if you made a paragraph editable. A common production failure mode is a user pasting from Word or Google Docs and dragging in a soup of` <span>` wrappers and inline styles; session replays of these editing sessions are one way to actually watch that malformed output get generated rather than reverse-engineering it from a corrupted database row. The craft-level fix is to prefer` plaintext-only` , or sanitize on` input` /paste.


**` execCommand` is deprecated.**` document.execCommand()` , long used for bold, italic, and link formatting, is now both[deprecated and non-standard per MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand) , so don’t build new rich-text features on it. It survives in legacy code because no full turnkey replacement exists. MDN notes it still uniquely preserves the undo buffer. For new work, reach for the[Selection](https://developer.mozilla.org/en-US/docs/Web/API/Selection) and[Range](https://developer.mozilla.org/en-US/docs/Web/API/Range) APIs together with` beforeinput` /` input` . Be honest about the cost: those are low-level primitives, not a drop-in, and Range behavior differs across browsers. For anything non-trivial, use a purpose-built editor framework.


**XSS.** Never render user-entered` contenteditable` HTML back to other users without sanitizing it first. An unsanitized` innerHTML` write is a direct injection vector. Sanitize with[DOMPurify](https://github.com/cure53/dompurify) (actively maintained, current 3.x), or use the browser’s native Sanitizer API where available with a DOMPurify fallback:


```text
function   safeRender  (  el  ,   html  ) {
if (  '  setHTML  '   in   Element  .  prototype  ) {
el  .  setHTML  (  html  );                // native, strips scripts/handlers
} else {
el  .  innerHTML   =   DOMPurify  .  sanitize  (  html  );
}
}
```


The native path is genuinely new.[Firefox 148](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases/148) , released February 24, 2026, added support for the HTML Sanitizer API along with methods like` setHTML()` , which sanitizes HTML before inserting it into the DOM to reduce the risk of XSS attacks. Chrome and Edge have followed, but[setHTML() is not yet Baseline](https://developer.mozilla.org/en-US/docs/Web/API/Element/setHTML) , so keep the fallback. OpenReplay’s[first look at the HTML Sanitizer API](https://blog.openreplay.com/html-sanitizer-api-overview/) covers the mechanics in depth.


## Accessibility


An editable region has to behave like a real control. Add visible` :focus` styling so keyboard users can see where the caret is, and label the region.` contenteditable` elements have no implicit accessible name, so attach an` aria-label` or an associated label:


```text
[  contenteditable  ]  :  focus   {
outline  :   2px   solid   #  2563eb  ;
outline-offset  :   2px  ;
}
```


```text
<  div   contenteditable  =  "  plaintext-only  "   aria-label  =  "  Note body  "   role  =  "  textbox  "  ></  div  >
```


Editable elements are focusable and participate in sequential keyboard navigation, though nested editable elements aren’t added to the tab order by default. Manage focus when your UI changes: if a button disappears after the user activates it (an undo control that toggles to redo, say), move focus back to a visible element with` .focus()` so keyboard users don’t get dropped, a point Scott O’Hara makes in his undo/redo implementation.


## When should you use contenteditable, and when not?


Use` contenteditable` for lightweight inline edits: an editable heading, a click-to-edit field, a live code/preview toy. Reach for a plain form control when you need reliable, predictable input, and a dedicated editor framework when you need structured rich text with clean output.


Need Best tool


Single/multi-line plain text, form submission` <input>` /` <textarea>`


Inline edit of displayed content, plain text` contenteditable="plaintext-only"`


Live in-browser code/preview toy` contenteditable`


Reliable rich text, structured/collaborative content Editor library (ProseMirror, Lexical, Tiptap)


The decision hinges on output predictability. A` <textarea>` gives you a clean string and a real` change` event;` contenteditable` gives you rendered HTML whose exact shape depends on the browser and what the user pasted. A mature editor library exists precisely because taming that output (normalized markup, a document model, undo history, sanitization) is a large problem someone has already solved.


Reach for` contenteditable` when the editing surface is small and the output is plain text or throwaway. The moment you need trustworthy structured HTML, either constrain the input hard with` plaintext-only` and sanitization, or hand the job to a tool built for it.


## FAQs


Does contenteditable fire a change event when the user finishes editing?


No. A contenteditable element has no native change event, which is why older tutorials using keypress or keyup miss pastes, drag-and-drop, and IME input. Listen for the input event instead, which fires on every modification to the editing host regardless of how the change was made. If you need to intercept or cancel an edit before the DOM mutates, use the beforeinput event, which also applies to contenteditable elements.


Should I use contenteditable or a textarea for a multi-line text field?


Use a textarea for plain text you plan to submit or store, because it returns a clean string and fires a real change event. Reach for contenteditable only when you need inline editing of displayed content in place rather than a separate form control. If the field is text-only, contenteditable='plaintext-only' is the closest match, since it strips pasted rich formatting at the source while still editing the rendered content directly.


Is execCommand still safe to use for bold and italic formatting?


Do not build new rich-text features on execCommand; MDN marks it both deprecated and non-standard. It survives in legacy code because no full turnkey replacement exists and it uniquely preserves the browser undo buffer. For new work, use the Selection and Range APIs together with the beforeinput and input events, though these are low-level primitives with Range behavior that differs across browsers. For anything non-trivial, use a dedicated editor library.


Does contenteditable plaintext-only work in Firefox?


Yes. The plaintext-only value shipped in Firefox 136 (March 2025), making it cross-browser alongside long-standing support in Chromium and WebKit. It makes the raw text editable while disabling rich-text formatting, so content pasted into a plaintext-only element has all formatting removed. This makes it the cleanest choice for text-only fields, since it blocks messy pasted markup at the source rather than requiring you to sanitize it after the fact.


Open-source session replay


## Gain control over your UX


See how users are using your site as if you were sitting next to them, learn and iterate faster with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
