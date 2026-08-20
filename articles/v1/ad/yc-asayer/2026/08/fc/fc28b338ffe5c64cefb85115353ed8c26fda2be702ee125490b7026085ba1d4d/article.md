---
schema_version: "1.0.0"
document_id: "fc28b338ffe5c64cefb85115353ed8c26fda2be702ee125490b7026085ba1d4d"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/animate-display-none/"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T15:07:39.607497+00:00"
fetched_at: "2026-08-03T16:10:47.640296+00:00"
content_hash: "sha256:b7c72f9958444a64c596723360f827ce57002e3d9067c888ab841cc55d999140"
---

# How to Animate display: none Without JavaScript Hacks

To animate an element to or from` display: none` with CSS alone, add` display` to your` transition` with the` allow-discrete` keyword and supply the entry state in a` @starting-style` rule — no JavaScript, no` setTimeout` , no` transitionend` listener. Two features shipped to make this work:[transition-behavior: allow-discrete](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-behavior) lets discrete properties like` display` participate in a transition, and[@starting-style](https://developer.mozilla.org/en-US/docs/Web/CSS/@starting-style) gives the browser a “before-open” state to animate in from. This guide gives you copy-pasteable exit and entry recipes, the dialog/popover top-layer case with its` overlay` gotcha, and an honest read on current browser support.


## Key Takeaways


- Animate` display` by including it in the` transition` shorthand with` allow-discrete` , and define the entry state in a` @starting-style` block — no JavaScript required.
- ` display` is a discrete property: with` allow-discrete` the browser flips it to visible at 0% on entry and to` none` at 100% on exit, so content stays visible for the whole duration.
- If you use the standalone` transition-behavior: allow-discrete` , it must come **after** the` transition` shorthand or the browser ignores it.
- For dialogs and popovers, also transition` overlay ... allow-discrete` , but treat it as progressive enhancement because` overlay` is not yet Baseline.
- ` @starting-style` and` allow-discrete` have been Baseline Newly available since Firefox 129 shipped on August 6, 2024; with no support the element just appears and disappears instantly.


## Why display: none couldn’t be transitioned


A CSS transition needs a before-change style to animate from. An element at` display: none` — or one that was just inserted into the DOM — has no rendered previous state, so historically the browser had nothing to interpolate against and the transition never fired. That is why developers reached for` setTimeout` class-toggling or double` requestAnimationFrame` reflow hacks.


The common shortcut — fading with` opacity: 0` and leaving the element in the DOM — is not a substitute.` opacity: 0` leaves the element in layout, in the tab order, and in the accessibility tree, so keyboard and screen-reader users can still land on content that looks gone.` display: none` removes it from all three. In practice, session replays of half-migrated components surface exactly this bug class: a user tabbing or clicking into a panel that looks dismissed but was only faded and never removed from layout. Animating real` display: none` eliminates it.


## The two CSS features that fix it: allow-discrete and @starting-style


` display` is a[discrete property](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-behavior) — it can’t interpolate between values, it’s an on/off switch. Discrete-animated properties generally flip between two values 50% through animating between the two; the exception is when animating to or from` display: none` or` content-visibility: hidden` , in which case the browser flips between the two values so that the transitioned content is shown for the entire animation duration. Direction matters: when animating` display` from` none` to` block` , the value flips to` block` at 0% of the duration so it is visible throughout; when animating from` block` to` none` , it flips to` none` at 100% of the duration so it is visible throughout — which is what[keeps the fade actually visible in both directions](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Transitions/Using) .


You enable this by setting` transition-behavior: allow-discrete` on the` display` transition, and you supply the entry state with` @starting-style` .


## The exit recipe: transition to display: none


To fade an element out and then remove it from layout, include` display` in the` transition` with` allow-discrete` and set the hidden state:


```text
.  panel   {
opacity  :   1  ;
transition  : opacity   0.3s   ease  , display   0.3s   allow-discrete;
}


.  panel  .  is-hidden   {
opacity  :   0  ;
display  :   none  ;
}
```


Opacity animates to` 0` over 300ms;` display` holds at its visible value until 100%, then flips to` none` . Forget` allow-discrete` on` display` and the element vanishes instantly — the most common mistake with this technique.


## The entry recipe: animating from display: none


For an element that starts hidden, put the “before-open” values in a` @starting-style` rule. With CSS nesting, the whole thing lives in one block:


```text
.  panel   {
display  :   none  ;
opacity  :   0  ;
transition  : opacity   0.3s   ease  , display   0.3s   allow-discrete;
}


.  panel  .  is-open   {
display  :   block  ;
opacity  :   1  ;


@starting-style {
opacity  :   0  ;
}
}
```


Order matters.` @starting-style` has the same specificity as the rule it targets, so it must come after the open-state declaration to win the cascade. And there’s a one-line trap: if you apply` transition-behavior: allow-discrete` before the transition shorthand,[the browser will ignore the transition-behavior](https://web.dev/blog/baseline-entry-animations) . Written standalone, it goes last:


```text
.  panel   {
transition  : opacity   0.3s  , display   0.3s  ;
transition-behavior: allow-discrete;   /* must come AFTER the shorthand */
}
```


For older engines, MDN’s cross-browser pattern declares` transition` twice — the first instance without` allow-discrete` provides[cross-browser support](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition-behavior) , ensuring the other properties still transition in browsers that don’t support` transition-behavior` .


## Dialogs and popovers: the overlay gotcha


Top-layer elements —` <dialog>` and anything using the` popover` attribute — are the highest-value use case, and they have an extra requirement. For dialogs and popovers you must also transition` overlay ... allow-discrete` , or the element drops out of the top layer instantly and the exit animation never shows:


```text
dialog   {
translate  :   0   100vh  ;
transition  :
translate   0.4s   ease-out  ,
display   0.4s   allow-discrete,
overlay   0.4s   allow-discrete;
}


dialog  [  open  ] {
translate  :   0   0  ;


@starting-style {
translate  :   0   100vh  ;
}
}
```


For a popover, swap` dialog\[open\]` for the` :popover-open` pseudo-class. The` overlay` property is what[defers the drop from the top layer](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition-behavior) : it makes sure that the removal of the element from the top layer is deferred until the animation has completed; in more complex cases, not doing this can result in the element being removed from the overlay too quickly, meaning the animation is not smooth or effective.


One honest caveat competitors skip:[overlay is not Baseline](https://developer.mozilla.org/en-US/docs/Web/CSS/overlay) . MDN marks it experimental — this feature is not Baseline because it does not work in some of the most widely-used browsers. Treat it as progressive enhancement; where it’s unsupported, the dialog still opens and closes, just without the deferred exit.


## Browser support and graceful degradation


` @starting-style` and` transition-behavior: allow-discrete` both became[Baseline Newly available](https://web.dev/blog/baseline-entry-animations) with Firefox 129,[released on August 6, 2024](https://github.com/mdn/content/blob/main/files/en-us/mozilla/firefox/releases/129/index.md?plain=1) . But animating` display` itself needs more than those two features shipping. It works in Chrome and Edge 117+ and in Safari 18+ — Safari 17.4 added` transition-behavior` and 17.5 added` @starting-style` , but transitioning` display` with them only works from Safari 18. Firefox 129+ supports both features, yet as of mid-2026 it still doesn’t transition the` display` property at all, so in Firefox the element just shows and hides instantly.` display` in` @keyframes` has worked since Chrome 116.


Feature Status Fallback


` transition-behavior: allow-discrete` Baseline (Aug 2024) Instant show/hide


` @starting-style` Baseline (Aug 2024) No entry animation


` overlay` Not Baseline Dialog still opens/closes


This is pure progressive enhancement. Without these features, elements animating into the top layer or from a` display: none` style will simply appear on your page without the transition, as they do today. No polyfill, no JavaScript fallback. Gate the enhancement with feature detection if you want an explicit boundary:


```text
@supports (transition-behavior: allow-discrete) {
/* modern entry/exit animations */
}
```


## When to reach for View Transitions instead


Use these transitions when you’re toggling an existing element’s visibility. Reach for the[View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/ViewTransition) instead when you’re adding or removing DOM nodes. Same-document view transitions have been[Baseline Newly available since October 14, 2025](https://web.dev/blog/same-document-view-transitions-are-now-baseline-newly-available) , following the release of[Firefox 144](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases/144) on the same day — supported in Chrome 111+, Edge 111+, Safari 18+, and Firefox 144+. Wrap the DOM mutation in` document.startViewTransition()` with a plain fallback:


```text
if (  document  .  startViewTransition  ) {
document  .  startViewTransition  (()   =>   card  .  remove  ());
} else {
card  .  remove  ();
}
```


The modern recipe retires the old JavaScript choreography: include` display` in your` transition` with` allow-discrete` , define the entry state in` @starting-style` , add` overlay` for top-layer elements, and let unsupported browsers fall back to an instant swap. Drop the` setTimeout` and ship the CSS.


## FAQs


Why does my exit animation not fire even after I added allow-discrete to the display transition?


The most common cause is that the standalone transition-behavior: allow-discrete declaration comes before the transition shorthand, so the browser silently ignores it. When written as a separate property, transition-behavior must appear after the transition shorthand or the shorthand resets it. If you inline allow-discrete inside the transition value itself, ordering within the shorthand is fine and this trap does not apply.


Do I still need @starting-style if I only want to fade an element out to display none?


No. @starting-style is only required for entry animations, where the element goes from display: none or is freshly inserted into the DOM and needs a before-open state to animate from. A pure exit — animating an already-visible element to display: none — needs only display included in the transition with allow-discrete and the hidden state set. Add @starting-style only when you also animate the element in.


What happens in browsers that do not support transition-behavior or @starting-style?


The element simply appears and disappears instantly, exactly as it would without any animation. This is progressive enhancement, so no polyfill and no JavaScript fallback are needed. For dialogs and popovers, browsers lacking the non-Baseline overlay property still open and close the element correctly; they just skip the deferred top-layer exit. You can gate the enhancement explicitly with an @supports (transition-behavior: allow-discrete) rule.


When should I use the View Transitions API instead of animating display none?


Use View Transitions when you are adding or removing DOM nodes, and use display transitions when you are toggling the visibility of an element that already exists in the DOM. Same-document view transitions became Baseline Newly available on October 14, 2025, supported in Chrome 111+, Edge 111+, Safari 18+, and Firefox 144+. Wrap the DOM mutation in document.startViewTransition() with a plain fallback for unsupported engines.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
