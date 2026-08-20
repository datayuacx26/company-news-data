---
schema_version: "1.0.0"
document_id: "86efcbff594397841a42a3b64d66752d03450a893aff74257c7988000b7c20b7"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/truncate-text-css/"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-08T05:17:41.693999+00:00"
fetched_at: "2026-08-08T05:17:43.878103+00:00"
content_hash: "sha256:e26291924c2634c892b00438defab3f425db48a883c351822a82e1eb6333c214"
---

# How to Truncate Text in CSS

To truncate text in CSS, use` text-overflow: ellipsis` to clip a single line and` line-clamp` to clip to a fixed number of lines — that one distinction decides everything else.


If you’ve ever watched a card title blow straight past its container while` text-overflow: ellipsis` did nothing at all, you already know the frustration. The failure almost always comes down to one of two things: the wrong technique for the number of lines you want, or a flex parent that quietly refuses to shrink. Single-line truncation needs three declarations working together; multi-line truncation needs the` -webkit-box` trio. The parts that actually break in production are narrower: ellipsis silently fails inside flexbox, multi-line clamping fails if the text can’t wrap, and JavaScript-based truncation quietly destroys content that screen readers need. This walks through each working pattern and the gotcha attached to it.


Here’s the decision rule in table form:


Goal Technique Required declarations Ellipsis customizable?


Clip to one line` text-overflow: ellipsis`` white-space: nowrap; overflow: hidden; text-overflow: ellipsis` Via` <string>` value


Clip to N lines` line-clamp`` display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: N; overflow: hidden` No


## Key Takeaways


- Single-line truncation requires three declarations together:` white-space: nowrap` stops wrapping,` overflow: hidden` clips the spill, and` text-overflow: ellipsis` adds the` …` . The element also needs a constraining width, or there is nothing to overflow.
- For multiple lines, set` display: -webkit-box` ,` -webkit-box-orient: vertical` ,` -webkit-line-clamp: N` , and` overflow: hidden` ; this prefixed trio is supported across all major browsers.
- The standardized, unprefixed` line-clamp` is defined in CSS Overflow Module Level 4 but is not yet Baseline as of 2026, so add it alongside the` -webkit-` declarations rather than replacing them.
- Ellipsis silently fails on a flex or grid child because its default` min-width: auto` refuses to shrink below its content width; set` min-width: 0` on that child to fix it.
- CSS truncation is presentational, so the full string stays in the DOM and screen readers read it in full, unlike JavaScript` substring` truncation, which deletes text.


## How do you truncate a single line of text?


To truncate a single line of text, three declarations must work together:` white-space: nowrap` stops the text wrapping,` overflow: hidden` clips what spills out, and` text-overflow: ellipsis` adds the` …` . Drop any one of the three and the effect breaks:` text-overflow` alone does nothing to wrapped text, and without` overflow: hidden` there is no clipping box for the ellipsis to sit against.


```text
.  truncate   {
white-space  :   nowrap  ;
overflow  :   hidden  ;
text-overflow  :   ellipsis  ;
}
```


The element also needs a constraining width. If the container is wide enough to fit the whole string on one line, nothing overflows and no ellipsis appears: the width of the parent is what forces truncation. Apply this to cards, table cells, nav items, or headings that must stay on one row.


The[text-overflow property](https://developer.mozilla.org/en-US/docs/Web/CSS/text-overflow) accepts` clip` (the default hard cut),` ellipsis` , and a custom` <string>` . There is also a` fade` keyword and a` fade()` function, added to the standard in CSS Overflow Level 4 and now listed in MDN’s formal syntax, but effectively no browser implements them, so don’t rely on either in production.


## How do you truncate text to multiple lines?


For multi-line truncation, set` display: -webkit-box` ,` -webkit-box-orient: vertical` ,` -webkit-line-clamp: N` , and` overflow: hidden` . Despite the vendor prefixes, this combination is supported across all major browsers and is the production-safe approach today.


```text
.  clamp   {
display  :   -webkit-box  ;
-webkit-box-orient  :   vertical  ;
-webkit-line-clamp  :   3  ;
overflow  :   hidden  ;
}
```


Keep the` overflow: hidden` . As[MDN notes](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/line-clamp) , without it the text isn’t actually clipped, yet the ellipsis still appears after your line count, so you end up with the ellipsis and the overflowing text both visible below it. These prefixed properties are also co-dependent:` -webkit-line-clamp` only does anything when` display` is` -webkit-box` (or` -webkit-inline-box` ) and` -webkit-box-orient` is` vertical` . The prefixed properties are formally deprecated, but that three-way dependency is itself a specified behavior that browsers will keep supporting, so shipping it carries no real risk.


One failure mode to watch: multi-line clamping only works if the text is allowed to wrap. An inherited` white-space: nowrap` will quietly break it, collapsing everything onto one line. Keep` white-space` at` normal` or` pre-wrap` .


There is also an unprefixed standard` line-clamp` property defined in[CSS Overflow Module Level 4](https://drafts.csswg.org/css-overflow-4/#propdef-line-clamp) , which limits a block’s contents to a set number of lines. As of 2026, though, it is still marked as not Baseline, since some widely used browsers don’t support it yet. Treat it as progressive enhancement. Add it alongside the prefixed declarations, not as a replacement:


```text
.  clamp   {
display  :   -webkit-box  ;
-webkit-box-orient  :   vertical  ;
-webkit-line-clamp  :   3  ;
line-clamp  :   3  ;   /* standardized, add alongside — not a replacement yet */
overflow  :   hidden  ;
}
```


## Why ellipsis fails inside flexbox (and grid)


Ellipsis silently fails on a flex child because a flex item’s default` min-width: auto` refuses to shrink below its content’s width, so the element never gets narrow enough to overflow, and` text-overflow` has nothing to clip. The child blows past its container instead of truncating. The fix is one declaration on the child:


```text
.  card   {   display  :   flex  ; }


.  card__label   {
min-width  :   0  ;   /* overrides the auto minimum so the item can shrink */
white-space  :   nowrap  ;
overflow  :   hidden  ;
text-overflow  :   ellipsis  ;
}
```


The[min-width: auto automatic minimum sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/min-width) applies to grid items too, so truncation inside a CSS Grid track fails for the identical reason. The fix is the same: set` min-width: 0` on the child (or` overflow: hidden` , which also lifts the automatic minimum). This is the single most common real-world truncation bug: the CSS looks correct in isolation and only breaks once the element is a flex or grid child.


## Tailwind equivalents


Tailwind ships both patterns as utilities, no plugin required. The[truncate utility](https://tailwindcss.com/docs/text-overflow) is a composite that sets` overflow: hidden` ,` text-overflow: ellipsis` , and` white-space: nowrap` : the full single-line recipe in one class. For multiple lines,[line-clamp-<n>](https://tailwindcss.com/docs/line-clamp) (for example` line-clamp-3` , or` line-clamp-none` to disable) emits the` -webkit-box` trio. Line-clamp utilities have been part of Tailwind core since v3.3; the current stable release is v4.3.3 (July 2026), so there is nothing extra to install.


```text
<  p   class  =  "  truncate  "  >  Single line, clipped with an ellipsis…  </  p  >
<  p   class  =  "  line-clamp-3  "  >  Multiple lines, clamped to three…  </  p  >
```


Inside a flex or grid child, add` min-w-0` : the utility equivalent of the` min-width: 0` fix.


## Accessibility: CSS truncation vs. JS truncation


Because CSS truncation is purely presentational, the full string stays in the DOM and screen readers still read it in full, with no` aria-label` needed. This is the decisive difference from JavaScript truncation: a` text.substring(0, 50)` approach rewrites the DOM node, so the clipped-off text is gone for everyone, including assistive tech, unless you separately preserve it with a` title` or` aria-label` attribute.


Even with CSS, truncation still hides information visually. Session replays of production UIs regularly surface this class of problem: a user hovering or squinting at a clipped table cell or nav item that hides the one detail they needed. Pair truncation on information-bearing content with a` title` attribute or an expand affordance:


```text
<  span   class  =  "  truncate  "   title  =  "  Quarterly revenue report — EMEA region, Q3 2026  "  >
Quarterly revenue report — EMEA region, Q3 2026
</  span  >
```


One limitation to know: the` -webkit-line-clamp` ellipsis can’t be customized and` text-overflow` does not apply to it, since` text-overflow` only affects inline overflow. If you need a custom truncation indicator on multi-line text, that’s a signal to reach for a different pattern rather than fight the clamp.


Reach for` text-overflow: ellipsis` when you need one line and the` line-clamp` trio when you need several, remember` min-width: 0` the moment the element lives inside flex or grid, and add a` title` wherever the hidden text carries meaning. That covers the patterns that ship correctly and the three that quietly don’t.


## FAQs


What is the difference between text-overflow: ellipsis and line-clamp?


Use text-overflow: ellipsis to clip a single line and line-clamp to clip to a fixed number of lines. Single-line truncation needs three declarations working together: white-space: nowrap, overflow: hidden, and text-overflow: ellipsis. Multi-line clamping needs the -webkit-box trio plus overflow: hidden and does not respond to text-overflow at all, since that property only affects inline overflow.


Why does my ellipsis truncation not work inside a flexbox or grid?


A flex or grid item's default min-width: auto refuses to shrink below its content width, so the element never gets narrow enough to overflow and text-overflow has nothing to clip. Setting min-width: 0 on that child overrides the automatic minimum and restores truncation. Applying overflow: hidden to the child also lifts the automatic minimum. This is the most common real-world truncation bug because the CSS looks correct in isolation.


Is the unprefixed line-clamp property safe to use alone in 2026?


No. The standardized, unprefixed line-clamp is defined in CSS Overflow Module Level 4, but as of 2026 MDN flags it as not Baseline because it does not work in some of the most widely-used browsers. Add it alongside the -webkit-box, -webkit-box-orient, and -webkit-line-clamp declarations as progressive enhancement rather than replacing them. The prefixed trio remains the production-safe approach today.


Does CSS truncation hurt accessibility for screen readers?


No. CSS truncation is purely presentational, so the full string stays in the DOM and screen readers read it in full without any aria-label. The problem is JavaScript truncation: a substring approach rewrites the DOM node, deleting the clipped text for everyone including assistive tech unless you preserve it with a title or aria-label. For information-bearing content, pair CSS truncation with a title attribute or an expand affordance regardless.


Digital experience platform


## Truly understand users experience


See every user interaction, feel every frustration and track all hesitations with **OpenReplay** — the open-source digital experience platform. It can be self-hosted in minutes, giving you complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
