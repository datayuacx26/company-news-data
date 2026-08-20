---
schema_version: "1.0.0"
document_id: "ada5c5e8d6d13767128658bc31499a99605addd25a3b1666900126790939bee0"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/inert-attribute-focus-interactivity/"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T17:38:53.548823+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:e5f282813f35621f89af37ed240bec1e5139171cbe0b37d809b567de3a1fb047"
---

# Managing Focus and Interactivity with the Inert Attribute

Setting` inert` on an element removes its entire subtree from the tab order, blocks all pointer and click events, and hides it from the accessibility tree so screen readers cannot discover or announce it — these three behaviors are spec-mandated by the[WHATWG HTML Living Standard](https://html.spec.whatwg.org/multipage/interaction.html#inert-subtrees) . In addition, current browsers prevent in-page find (Ctrl/Cmd+F) from matching the subtree’s text and disable text selection within it, behaviors the spec leaves to user-agent discretion but which[MDN documents as the implemented norm](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/inert) . That is six interaction channels disabled with a single attribute.


This article resolves a specific problem: how to cleanly disable background content when a modal, drawer, or side-nav opens — without hand-rolling a focus trap that breaks on mobile screen readers or scattering` aria-hidden` across the DOM. It covers what` inert` blocks, the HTML and JavaScript syntax for applying it, a complete worked modal with focus restoration, how to style inert content, and when to reach for` disabled` ,` aria-hidden` , or` hidden` instead.


## Key Takeaways


- ` inert` blocks six interaction channels in one declaration: focus, pointer/click events, Tab order, and accessibility-tree discoverability (all spec-mandated), plus in-page find and text selection (UA discretion, but implemented across current browsers).
- ` inert` became[Baseline Newly available in April 2023](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inert) — Chrome and Edge 102, Firefox 112, Safari 15.5 — and reached Baseline Widely available around October 2025, making the wicg-inert polyfill legacy context rather than a production requirement.
- The mental-model shift is guard-vs-trap: a focus trap locks users inside a component with JavaScript;` inert` guards the rest of the page so the browser enforces the boundary natively.
- Beyond the HTML boolean attribute,` inert` is exposed as the` HTMLElement.inert` IDL property, a boolean you set in JavaScript —` mainEl.inert = true` on open,` mainEl.inert = false` on close.
- ` <dialog>.showModal()` automatically inerts the rest of the page, so manual` inert` management is only necessary for custom dialog patterns built outside the native element.


## What the inert Attribute Blocks


` inert` is a[global HTML attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inert) that makes an element and its entire subtree non-interactive and undiscoverable. Per the WHATWG HTML Living Standard’s[inert subtrees section](https://html.spec.whatwg.org/multipage/interaction.html#inert-subtrees) , a node that is inert does not receive targeted user-interaction events such as click and focus, and user agents must exclude it from the accessibility tree. Three blocks are normative and consistent across every implementation:


1. **Focus** — inert elements cannot receive focus by Tab, click, or programmatic` element.focus()` .
2. **Pointer and click events** — user-initiated clicks and pointer events do not reach inert nodes.
3. **Accessibility-tree discoverability** — assistive technology cannot find or announce the subtree.


Two further blocks are left to user-agent discretion in the spec (it uses the normative *may* ), but MDN documents them as the implemented behavior across current browsers:


1. **In-page find** — Ctrl/Cmd+F does not match text inside an inert subtree.
2. **Text selection** — users cannot select inert text.


Tab order falls out of the focus block: because inert nodes are unfocusable, they are removed from sequential focus navigation entirely. One important boundary:` inert` blocks *user-initiated* events, not programmatic ones. A` dispatchEvent()` call or a timer firing inside an inert subtree still runs —` inert` is not` alert()` and does not freeze JavaScript execution.


The pitfall to internalize: because` inert` removes the subtree from the accessibility tree, never apply it to content a user still needs to read. If you only need to hide something visually while keeping it discoverable, that is a different tool.


## The Two Canonical Use Cases


` inert` exists for two situations, both documented on[web.dev’s inert guide](https://web.dev/articles/inert) : DOM that is present but off-screen or hidden, and DOM that is visible but should not be interactive.


**Off-screen or hidden DOM.** A slide-out navigation drawer or side nav adds focusable links to the DOM before they are visible. Without` inert` , keyboard users can Tab into the closed drawer and land on controls they cannot see. Marking the drawer’s container inert until it opens keeps those links out of the tab order:


```text
<  nav   id  =  "  drawer  "   inert  >
<  a   href  =  "  /dashboard  "  >  Dashboard  </  a  >
<  a   href  =  "  /settings  "  >  Settings  </  a  >
</  nav  >
```


**Visible but non-interactive UI.** When a form is submitting, a page is loading, or a modal overlay sits over backgrounded content, that content is visibly present but should not accept input. Applying` inert` to the form during submission prevents double-submits and stray focus:


```text
<  form   id  =  "  signup  "   inert  >
<!-- fields disabled as a group while the request is in flight -->
</  form  >
```


Both cases share the same logic: the content stays in the DOM (so layout, transitions, and state survive), but the browser refuses to route interaction to it.


## Syntax: The HTML Attribute and the HTMLElement.inert Property


` inert` has two interfaces: the HTML boolean attribute and the` HTMLElement.inert` IDL property. The attribute is for static or server-rendered markup; the property is for toggling state in JavaScript.


As a boolean attribute, its presence is what matters —` inert` and` inert=""` are equivalent, and the[default value is false](https://web.dev/articles/inert) (absent means interactive):


```text
<  main   inert  >
<!-- everything here is non-interactive -->
</  main  >
```


To toggle it at runtime, use the[HTMLElement.inert property](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/inert) , a boolean you can read and set directly — no` setAttribute` /` removeAttribute` dance required:


```text
const   mainEl   =   document  .  querySelector  (  '  main  '  );


// disable interaction with the rest of the page
mainEl  .  inert   =   true  ;


// restore it
mainEl  .  inert   =   false  ;
```


This is the cleanest part of the API and the part missing from most existing write-ups: the open/close toggle is two assignments. Compare that to the eight-step focus-trap procedure below.


## Before inert: Focus Traps and Why They Were Fragile


Before` inert` , the standard way to bound a modal was a JavaScript focus trap — logic that intercepts Tab and Shift+Tab to loop focus inside the dialog. The canonical procedure,[enumerated by CSS-Tricks](https://css-tricks.com/focus-management-and-inert/) , runs to roughly eight steps: find every focusable element on the page, identify the first and last focusable element inside the modal, strip interactivity and discoverability from everything outside it, move focus in, listen for dismissal events, restore everything on close, and return focus to the trigger.


That first step — “find every focusable element” — is itself a source of bugs, because the set of natively focusable elements is larger than most people remember. The elements that take focus in sequential tab order without any` tabindex` are:


- ` <a>` and` <area>` with an` href`
- ` <button>` ,` <input>` ,` <select>` , and` <textarea>` (unless` disabled` )
- ` <iframe>` ,` <embed>` , and` <object>`
- ` <audio>` and` <video>` with a` controls` attribute
- ` <summary>` (the first one inside a` <details>` )
- any element with a non-negative` tabindex` , and any` contenteditable` element


Miss one when building a trap and a keyboard user escapes it; over-manage the list and you fight the browser’s own ordering. The safer default is to leave the document’s natural focus order alone and intervene only when a component genuinely requires it — which is most of the manual work` inert` removes.


The mental-model shift is the point. A focus trap *locks users inside* a component by intercepting keypresses;` inert` *guards the rest of the page* by making everything outside the dialog unreachable — the browser enforces the boundary, not your JavaScript. This guard-vs-trap framing comes from[LogRocket’s treatment of the attribute](https://blog.logrocket.com/using-html-inert-property-manage-user-focus/) .


Hand-rolled traps fail in three recurring ways:


- **Mobile assistive tech.** TalkBack on Android and VoiceOver on iOS navigate via swipe gestures, not Tab keypresses. A JavaScript trap that only intercepts keyboard events provides no boundary at all for swipe-based screen-reader users.` inert` blocks the subtree at the platform level, covering both keyboard and gesture navigation.
- **` aria-hidden` sprawl.** The pre-` inert` workaround was to set` aria-hidden="true"` on every non-modal element. On a page with deep DOM trees this becomes unmaintainable and frequently incomplete.
- **Manual tab loops.** The Tab/Shift+Tab interception logic is brittle and easy to get wrong, especially when the modal’s focusable contents change.


Session replays of modal and drawer implementations frequently surface focus events landing on background content while a dialog is still open — the signature of an incomplete focus boundary, and precisely what` inert` is designed to eliminate.


The references above rebuild a full` trap-focus.js` ; there is no need to repeat that here. The relevant comparison is the line count. The trap is dozens of lines of event interception. The` inert` equivalent is this:


```text
function   openModal  () {
mainEl  .  inert   =   true  ;
}
function   closeModal  () {
mainEl  .  inert   =   false  ;
}
```


## Worked Modal Example with Focus Restoration


The cleanest custom-modal pattern places the dialog as a sibling of` <main inert>` : the modal sits outside the inert subtree, so it stays interactive while everything in` <main>` is sealed off. This` <main inert>` sibling pattern follows the structure[CSS-Tricks documents](https://css-tricks.com/focus-management-and-inert/) . The example below adds the missing piece every reference skips — moving focus into the dialog on open and restoring it to the trigger on close.


```text
<  button   id  =  "  open-modal  "   type  =  "  button  "  >  Save changes…  </  button  >


<  div
id  =  "  modal  "
class  =  "  modal  "
role  =  "  dialog  "
aria-labelledby  =  "  modal-title  "
aria-modal  =  "  true  "
hidden
>
<  h2   id  =  "  modal-title  "  >  Save changes?  </  h2  >
<  p  >  Your unsaved changes will be lost.  </  p  >
<  button   id  =  "  save  "   type  =  "  button  "   autofocus  >  Save  </  button  >
<  button   id  =  "  cancel  "   type  =  "  button  "  >  Discard  </  button  >
</  div  >


<  main   id  =  "  page  "  >
<!-- all page content -->
</  main  >
```


```text
const   triggerEl   =   document  .  getElementById  (  '  open-modal  '  );
const   modalEl   =   document  .  getElementById  (  '  modal  '  );
const   mainEl   =   document  .  getElementById  (  '  page  '  );
const   cancelEl   =   document  .  getElementById  (  '  cancel  '  );


let   lastFocused   =   null  ;


function   openModal  () {
lastFocused   =   document  .  activeElement  ;     // remember the trigger
modalEl  .  hidden   =   false  ;
mainEl  .  inert   =   true  ;                      // guard the rest of the page
// move focus to the dialog's primary action
modalEl  .  querySelector  (  '  [autofocus]  '  ).  focus  ();
}


function   closeModal  () {
mainEl  .  inert   =   false  ;                     // restore the page
modalEl  .  hidden   =   true  ;
if (  lastFocused  )   lastFocused  .  focus  ();     // restore focus to the trigger
}


triggerEl  .  addEventListener  (  '  click  '  ,   openModal  );
cancelEl  .  addEventListener  (  '  click  '  ,   closeModal  );
document  .  addEventListener  (  '  keydown  '  , (  e  )   =>   {
if (  e  .  key   ===   '  Escape  '   &&   !  modalEl  .  hidden  )   closeModal  ();
});
```


A few notes on correctness. The dialog uses` tabindex="-1"` semantics implicitly through its focusable children; you generally do not need a positive` tabindex` anywhere —[positive integers override natural tab order and are a documented anti-pattern](https://css-tricks.com/focus-management-and-inert/) . Use` tabindex="-1"` only when you need to focus a non-interactive container programmatically, and` tabindex="0"` only for genuinely interactive custom elements. The` autofocus` attribute on the primary action is the spec-recommended starting point for focus inside a dialog. This pattern works in Chrome 102+, Firefox 112+, and Safari 15.5+.


## Styling inert Content: There Is No Default


` inert` has no default visual effect — the browser changes behavior, not appearance, so inert content looks identical to active content unless you style it. The standard pattern, shown on[web.dev](https://web.dev/articles/inert) , targets the` \[inert\]` attribute selector and combines three properties that mirror the interaction channels the attribute blocks:


```text
[  inert  ],
[  inert  ]   *   {
opacity  :   0.5  ;           /* visual dimming — signals "not active" */
pointer-events  :   none  ;   /* suppresses hover/cursor affordances */
user-select  :   none  ;      /* prevents text selection */
cursor  :   default  ;
}
```


Each property earns its place:` opacity` communicates the disabled state visually,` pointer-events: none` removes hover states and cursor changes that would otherwise imply interactivity, and` user-select: none` matches the text-selection block the attribute already applies. The behavior is enforced by` inert` itself; the CSS exists so sighted users can see the boundary the browser is enforcing under the hood.


## Choosing Between inert, disabled, aria-hidden, hidden, and pointer-events


Pick the tool by scope and accessibility-tree behavior:` inert` blocks every interaction channel across an entire subtree and removes it from the accessibility tree;` disabled` blocks a single control but keeps it discoverable;` aria-hidden` hides content from assistive tech while leaving clicks and focus intact;` hidden` removes content entirely; and CSS` pointer-events: none` blocks only the mouse. Reach for` inert` whenever you need to seal off background content behind a modal, drawer, or loading overlay.


Tool Blocks interaction In accessibility tree? Scope Use when


[inert](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inert) Yes (focus, pointer, find, selection) No — removed Element + subtree Sealing off background content behind a modal, drawer, or loading overlay


[disabled](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/disabled) Yes (for the control) Yes — announced as unavailable Form control or fieldset group A single button, input, or form section that is temporarily not actionable


[aria-hidden="true"](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-hidden) No — clicks/focus still work No — removed Element + subtree Hiding decorative or duplicate content from AT only


[hidden / display:none](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/hidden) Yes — fully removed No — not rendered Element + subtree Content that should not exist visually or for AT right now


[pointer-events: none](https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events) Mouse only — keyboard/AT unaffected Yes Element + subtree Cosmetic click-through; never a substitute for` inert`


The two common mistakes: using` aria-hidden` on background content while leaving it clickable and focusable (it stays in the tab order), and using` pointer-events: none` and assuming keyboard and screen-reader users are blocked (they are not). For full background sealing,` inert` is the only single tool that covers every channel.


## Do You Still Need inert with dialog.showModal()?


When you open a dialog with[HTMLDialogElement.showModal()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLDialogElement/showModal) , the browser automatically inerts the rest of the page — the[top-layer behavior includes an implicit inert boundary](https://web.dev/articles/inert) , so everything outside the dialog becomes unclickable and untabbable without any attribute management on your part. Manual` inert` is only necessary when you build a custom dialog pattern outside the native` <dialog>` element, as in the worked example above.


```text
<  dialog   id  =  "  confirm  "  >
<  p  >  Delete this item?  </  p  >
<  button  >  Delete  </  button  >
<  button  >  Cancel  </  button  >
</  dialog  >
```


```text
document  .  getElementById  (  '  confirm  '  ).  showModal  ();   // page auto-inerted
```


If you can use` <dialog>` with` showModal()` , you get the inert boundary for free. Reach for manual` inert` when AT support concerns or design constraints push you toward a custom dialog.


## Browser Support and the Emerging CSS interactivity Property


` inert` became Baseline Newly available in April 2023 —[Chrome and Edge shipped it in version 102, Firefox in 112, and Safari in 15.5](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inert) — and reached[Baseline Widely available](https://web.dev/baseline) around October 2025 (30 months after the interoperable date). The[wicg-inert polyfill](https://github.com/WICG/inert) is now legacy context rather than a production requirement; its last release is v3.1.3 (2023) and it is no longer actively maintained. Its own README notes the polyfill is “expensive, performance-wise” because it “requires a fair amount of tree-walking” — a cost the native implementation avoids. For any browser shipped since 2023, you do not need it.


A newer CSS-based alternative is the[interactivity property](https://developer.mozilla.org/en-US/docs/Web/CSS/interactivity) , which accepts` interactivity: inert` to apply inert behavior through a stylesheet rather than an attribute. It is an emerging feature with a narrower support story: per[caniuse data](https://caniuse.com/mdn-css_properties_interactivity_inert) , it is Chromium-only (Chrome/Edge 135+, March 2025) with no Firefox or Safari support as of mid-2026, and it is not Baseline (Limited availability). Treat it as a forward-looking option for Chromium-only contexts, not a cross-browser replacement for the attribute.


## Conclusion


For sealing off background content behind a modal, drawer, or loading overlay,` inert` replaces the entire fragile apparatus of hand-rolled focus traps and` aria-hidden` sprawl with one attribute that the browser enforces across keyboard, pointer, and assistive-technology navigation. Audit your existing dialogs: if a keyboard user can Tab out of an open modal into the page behind it, wrap that background content — or your` <main>` — in` inert` , toggle it with` element.inert` on open and close, and restore focus to the trigger. With the attribute Widely available since 2025, the only remaining decision is whether the native` <dialog>` gives you the boundary for free.


## FAQs


What is the difference between the inert attribute and aria-hidden?


The inert attribute blocks interaction and removes content from the accessibility tree, so the subtree becomes both unreachable and undiscoverable. The aria-hidden attribute only removes content from the accessibility tree; it does not block clicks, focus, or keyboard interaction. Applying aria-hidden to background content while leaving it clickable and focusable is a common mistake, since those elements stay in the tab order. Use inert when you need to block interaction across an entire subtree.


Does inert block JavaScript event listeners and programmatic events?


No. The inert attribute blocks user-initiated events such as clicks, focus, and pointer interaction, but it does not stop programmatic events. A dispatchEvent call, a timer callback, or any script running inside an inert subtree still executes normally. Unlike the alert function, inert does not freeze JavaScript execution; it only changes how the browser routes user interaction and accessibility discovery to the marked subtree.


Do I still need a JavaScript polyfill for inert?


No, for any browser shipped since 2023. The inert attribute became Baseline Newly available in April 2023, with Chrome and Edge 102, Firefox 112, and Safari 15.5, and reached Baseline Widely available around October 2025. The wicg-inert polyfill is now legacy context rather than a production requirement; its last release is v3.1.3 in 2023 and it is no longer actively maintained. Its README also notes it is expensive performance-wise because it requires tree-walking that native implementations avoid.


Why do traditional focus traps fail on mobile screen readers?


Traditional focus traps fail on mobile because TalkBack on Android and VoiceOver on iOS navigate by swipe gestures rather than Tab keypresses. A JavaScript trap that intercepts only keyboard events provides no boundary for swipe-based screen-reader users, so they can escape the dialog into background content. The inert attribute blocks the subtree at the platform level, covering both keyboard navigation and gesture navigation, which is why it replaces hand-rolled focus-trap logic for bounding modals.


Digital experience platform


## Truly understand users experience


See every user interaction, feel every frustration and track all hesitations with **OpenReplay** — the open-source digital experience platform. It can be self-hosted in minutes, giving you complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
