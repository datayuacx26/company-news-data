---
schema_version: "1.0.0"
document_id: "2a8a4c9c004c1e44db97dd6dfae85c10b86042b8e665423dbef95f87398c132e"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/css-flexbox-complete-guide-2026/"
published_at: "2026-07-30T11:49:24+00:00"
first_seen_at: "2026-07-31T23:11:23.542680+00:00"
fetched_at: "2026-07-31T23:11:25.123786+00:00"
content_hash: "sha256:418c528a7f25112f4e476801297c1b875b32cbef6158903b7130b8b92fb86e9d"
---

# CSS Flexbox: A Complete Guide [2026]

CSS flexbox is a one-dimensional layout mode that arranges elements in a single row or a single column and distributes the leftover space between them. Setting` display: flex` on an element turns it into a *flex container* , and its direct children become *flex items* . Every flexbox property then acts on one of two axes: the main axis or the cross axis.


That last sentence is the whole thing. People who find flexbox unpredictable almost always know the property names and not the axis each one acts on. This guide covers every property with working CSS, plus the layout recipes worth reusing, the gotchas that produce mysterious overflow, and when to reach for Grid instead.


## What Is CSS Flexbox and When Should You Use It?


Flexbox is a CSS layout mode for arranging items along one axis at a time, sizing them from their content, and dividing the remaining space according to rules you set.


The specification is[CSS Flexible Box Layout Module Level 1](https://www.w3.org/TR/css-flexbox-1/?ref=scrimba.com) , a W3C Candidate Recommendation Draft dated 14 October 2025. Two values create a container:


```text
.container { display: flex; }        /* block-level flex container */
.inline-container { display: inline-flex; }  /* inline-level flex container */


```


The moment you type` display: flex` , you inherit a set of defaults. Knowing them explains most surprising behavior:


Property Initial value Effect


` flex-direction`` row` Left to right


` flex-wrap`` nowrap` Overflows rather than wraps


` justify-content`` normal` (acts as` flex-start` ) Bunched at the start


` align-items`` stretch` Equal heights


` flex-grow`` 0` No absorbing free space


` flex-shrink`` 1` Shrinks under pressure


` flex-basis`` auto` Size from` width` or content


So a bare flex container gives you a row of content-sized, equal-height boxes that refuse to wrap. Reach for flexbox when the layout is a row or a column: navbars, toolbars, form rows, card decks, button groups.


## Main Axis vs Cross Axis: The Concept Everyone Gets Wrong


The main axis is the direction` flex-direction` points. The cross axis is always perpendicular to it. Alignment properties are split between the two and never overlap.


This is the source of the classic confusion. Beginners memorize "` justify-content` is horizontal,` align-items` is vertical." That holds only while` flex-direction` is` row` . Switch to` column` and both swap physical direction, because they were never physical to begin with.


` flex-direction` Main axis Cross axis


` row` (default) Horizontal, left to right Vertical


` row-reverse` Horizontal, right to left Vertical


` column` Vertical, top to bottom Horizontal


` column-reverse` Vertical, bottom to top Horizontal


The rule to memorize instead: **` justify-content` works on the main axis, everything starting with` align-` works on the cross axis.** Sizing follows the same split, since` flex-grow` ,` flex-shrink` , and` flex-basis` all describe main-axis size.


Flexbox is also writing-mode aware. In a right-to-left language,` row` starts on the right, which is why` flex-start` and` flex-end` exist alongside the physical` left` and` right` .


## Flex Container Properties


These seven go on the parent. They govern direction, wrapping, and how the group is positioned.


` **flex-direction**` sets the main axis:` row` ,` row-reverse` ,` column` , or` column-reverse` .


` **flex-wrap**` decides whether a single line can break into several:` nowrap` (default),` wrap` , or` wrap-reverse` . **` flex-flow`** is the shorthand for both.


```text
.container {
display: flex;
flex-flow: row wrap;  /* flex-direction: row; flex-wrap: wrap; */
}


```


` **justify-content**` distributes items along the main axis. It is the only main-axis alignment property, because flexbox has no` justify-self` or` justify-items` .


```text
.container {
justify-content: flex-start;   /* also: flex-end, center, start, end, left, right */
/* space-between: first and last flush to the edges, equal gaps between */
/* space-around:  each item gets equal space on both sides, so edges are half-size */
/* space-evenly:  every gap including the outer two is identical */
}


```


` **align-items**` positions items on the cross axis.` stretch` is the default, which is why flex children come out the same height without anyone asking for it.


```text
.container {
align-items: stretch;  /* also: flex-start, flex-end, center, baseline, start, end */
}


```


` baseline` is the underrated one: it lines up the text baselines of items with different font sizes, which no other value does.


` **align-content**` packs multiple flex lines along the cross axis, using the same distribution values as` justify-content` . One prerequisite catches everyone: per the spec, on a single-line flex container` align-content` has no effect. It needs` flex-wrap: wrap` .


**` gap`** sets spacing between items without the margin arithmetic that preceded it.


```text
.container {
gap: 1rem;             /* row-gap and column-gap together */
gap: 1rem 2rem;        /* row-gap, then column-gap */
column-gap: 2rem;      /* individually */
}


```


Gap in flexbox landed in Chrome 84, Edge 84, Firefox 63, and Safari 14.1, and now sits around 94% global support ([Can I use](https://caniuse.com/flexbox-gap?ref=scrimba.com) ). Safari was the last holdout, which is why older codebases still carry negative-margin hacks.


## Flex Item Properties


These six go on the children. Four control main-axis size, one controls order along it, and one controls cross-axis position.


` **order**` changes visual position without touching the HTML. It defaults to` 0` and sorts items into *ordinal groups* , lowest first, with source order breaking ties. Negative values are legal, so` order: -1` pulls one item to the front without renumbering anything else.


Use it carefully.` order` moves the paint order only. Tab order and screen reader order still follow the source, so a keyboard user moves through your reordered layout in the original sequence ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Ordering_flex_items?ref=scrimba.com) ).


` **flex-grow**` (default` 0` ) is the share of *positive free space* an item absorbs.` **flex-shrink**` (default` 1` ) is its share of the *negative free space* when items overflow.` **flex-basis**` (default` auto` ) is the size each item starts from before any of that happens.


**` flex`** is the shorthand for all three, and the spec is unusually direct about preferring it:


> "Authors are encouraged to control flexibility using the flex shorthand rather than with its longhand properties directly, as the shorthand correctly resets any unspecified components to accommodate common uses."


` **align-self**` overrides` align-items` for one item. It accepts every` align-items` value plus` auto` , which means "go back to whatever the container said."


```text
.item { align-self: flex-end; }


```


Here is the full reference, container and item together:


Property Applies to Common values Initial


` display` container` flex` ,` inline-flex`` inline`


` flex-direction` container` row` ,` row-reverse` ,` column` ,` column-reverse`` row`


` flex-wrap` container` nowrap` ,` wrap` ,` wrap-reverse`` nowrap`


` flex-flow` container direction plus wrap` row nowrap`


` justify-content` container` flex-start` ,` center` ,` space-between` ,` space-evenly`` normal`


` align-items` container` stretch` ,` flex-start` ,` center` ,` baseline`` stretch`


` align-content` container` stretch` ,` center` ,` space-between` ,` space-around`` normal`


` gap` container one or two lengths` normal`


` order` item any integer` 0`


` flex-grow` item non-negative number` 0`


` flex-shrink` item non-negative number` 1`


` flex-basis` item` auto` ,` content` , length, percentage` auto`


` flex` item` initial` ,` auto` ,` none` , a number` 0 1 auto`


` align-self` item` auto` plus any` align-items` value` auto`


## flex-basis vs width, and Why` flex: 1` Behaves the Way It Does


` flex-basis` sets an item's starting size on the main axis before growing or shrinking. When it is` auto` , it reads the item's` width` (or` height` in a column). Give it a length and it overrides` width` entirely.


Flexbox works in two steps. It lays every item out at its flex base size, then measures the difference against the container: extra room is *positive free space* , a shortfall is *negative free space* .` flex-grow` divides the first,` flex-shrink` the second.


One asymmetry matters. Grow factors split space by ratio alone, but shrink factors are multiplied by each item's base size first, so a wide item gives up more pixels than a narrow one at the same` flex-shrink` . That is why small items do not collapse to nothing while a large sibling sits untouched.


The shorthand keywords are where` flex: 1` gets explained:


Shorthand Expands to Result


` flex: initial`` 0 1 auto` Content-sized, can shrink, will not grow. The default.


` flex: auto`` 1 1 auto` Starts from content size, then absorbs free space


` flex: none`` 0 0 auto` Content-sized and completely rigid


` flex: 1`` 1 1 0%` Base size zeroed, so all space is shared equally


So` flex: 1` produces equal-width columns *regardless of content* , because a base size of` 0%` erases content from the calculation before the split.` flex: auto` produces content-proportional columns, because each item keeps its natural width and only the leftover is divided.


```text
.equal   > * { flex: 1; }        /* every column identical */
.natural > * { flex: auto; }     /* wider content stays wider */
.fixed     { flex: 0 0 240px; }  /* a rigid 240px sidebar */


```


One detail worth knowing: percentages on` flex-basis` resolve against the container's inner main size, and an unresolvable percentage falls back to` auto` .


## Five Flexbox Layout Recipes


**Responsive card grid** with no media queries. Each card asks for 300px and wraps when it cannot have it.


```text
.cards { display: flex; flex-wrap: wrap; gap: 1rem; }
.cards > * { flex: 1 1 300px; }


```


**Holy grail layout** : header, footer, fixed sidebars, fluid center column.


```text
.page   { display: flex; flex-direction: column; min-height: 100vh; }
.middle { display: flex; flex: 1; }
.middle nav, .middle aside { flex: 0 0 200px; }
.middle main { flex: 1; }


```


**Navbar with a split** , using an auto margin instead of a spacer element. An auto margin swallows the free space in that direction and pushes everything after it to the far end.


```text
.nav { display: flex; align-items: center; gap: 1.5rem; }
.nav .logo { margin-right: auto; }


```


**Sticky footer** that sits at the bottom on short pages and below content on long ones.


```text
body { display: flex; flex-direction: column; min-height: 100vh; }
main { flex: 1; }


```


**Perfect centering** , horizontally and vertically. The recipe that made flexbox famous.


```text
.center {
display: flex;
justify-content: center;
align-items: center;
min-height: 100vh;
}


```


Be honest about the last one: flexbox stretches the final row's cards to fill the leftover width, while CSS Grid keeps every track the same size. If ragged last rows bother you, that is a Grid job. For layouts that adapt across breakpoints, Scrimba's[complete guide to responsive web design](https://scrimba.com/articles/responsive-web-design-a-complete-guide-2026-2/) picks up where this leaves off.


## Common Flexbox Gotchas


Nearly every "flexbox is broken" moment traces back to one of these, and the first accounts for most of them. The spec is explicit about the cause:


> "To provide a more reasonable default minimum size for flex items, the used value of a main axis automatic minimum size on a flex item whose computed overflow value is non-scrollable is its content-based minimum size."


- **Items refuse to shrink and overflow the container.** The initial value of` min-width` is` auto` , which for flex items resolves to the` min-content` size rather than zero ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/min-width?ref=scrimba.com) ). A long URL or an unbreakable word sets a floor` flex-shrink` cannot cross. Fix it with` min-width: 0` on the item.
- **A nested scroll area will not scroll.** Same cause, block direction. A column flex item wrapping overflowing content needs` min-height: 0` , or an` overflow` value that makes it a scroll container, which sets the automatic minimum to zero.
- **` justify-content` suddenly does nothing.** Check for an auto margin. Auto margins consume free space before the alignment properties get a look, so the two cannot both act in one dimension.
- **A child's` height: 100%` collapses.** Percentage heights need a *definite* size to resolve against. A stretched item in a single-line container with a definite cross size gives you one; a container sized` auto` does not.
- **` align-content` is ignored.** It only acts on multi-line containers. Without` flex-wrap: wrap` , there is one line and nothing to pack.


## Flexbox vs CSS Grid: Which Should You Use?


Use flexbox for one-dimensional layouts where content decides the sizes, and CSS Grid for two-dimensional layouts where you decide the structure first.


Flexbox CSS Grid


Dimensions One axis at a time Rows and columns together


Direction of control Content out Layout in


Item placement Sequential along the line Any cell, by line or named area


Best for Navbars, toolbars, button rows, card decks Page skeletons, dashboards, galleries


The dichotomy is real but overstated. Production layouts use both: Grid for the page skeleton, flexbox inside each region for the component-level rows.


The short test: if you are aligning things in a line, use flexbox. If you are aligning things in a line *and* a column at once, use Grid. Scrimba's[practical guide to CSS Grid](https://scrimba.com/articles/how-to-learn-css-grid/) covers the second case in the depth this guide covers the first.


## How to Actually Learn Flexbox


Reading property tables gets you a third of the way. Flexbox is a system with feedback: you change` flex-basis` from` auto` to` 0` , the boxes move, and the model clicks. That is a practice problem, not a reading problem.


Scrimba's[Learn Flexbox](https://scrimba.com/learn-flexbox-c0k?ref=scrimba.com) is a 52-minute Pro course taught by Per Borgen, built around the two axes, the alignment properties, and the flex shorthand, closing with a responsive navbar and a flexbox image grid. Because Scrimba's scrims let you pause the screencast and edit the instructor's CSS in place, changing a value and watching the layout react is the default interaction rather than a separate exercise. The course assumes working HTML and CSS, and does not cover CSS Grid or media queries in depth.


If the prerequisites are the gap, the free[Learn HTML and CSS](https://scrimba.com/learn-html-and-css-c0p?ref=scrimba.com) course runs 5.7 hours with a completion certificate. Afterwards,[CSS Challenges](https://scrimba.com/css-challenges-c02p?ref=scrimba.com) is 2.6 hours of rebuilding real UI components, and[Learn Responsive Web Design](https://scrimba.com/learn-responsive-web-design-c029?ref=scrimba.com) with Kevin Powell, one of the most widely followed CSS educators working today, runs 15.1 hours including advanced flexbox layouts. Scrimba's roundups of[CSS courses](https://scrimba.com/articles/best-css-courses-and-tutorials-2026/) and[free HTML and CSS courses](https://scrimba.com/articles/best-free-html-and-css-courses/) compare the alternatives. Pro is $24.50/mo annually, with student and location-based discounts available.


## Frequently Asked Questions


### What is the difference between flexbox and CSS Grid?


Flexbox is one-dimensional and lays items out along a single row or column, sizing them from their content. CSS Grid is two-dimensional and controls rows and columns at once from a structure you define upfront. Most layouts use Grid for the page skeleton and flexbox inside components.


### What does` flex: 1` actually mean?


` flex: 1` expands to` flex: 1 1 0%` . The item can grow, can shrink, and starts from a base size of zero. Because content is erased from the size calculation, all items with` flex: 1` end up equal width no matter how much text they hold. Use` flex: auto` for content-proportional widths.


### Why is my flex item overflowing its container?


The initial value of` min-width` is` auto` , which resolves to the` min-content` size for flex items. A long word, a URL, or a wide image sets a minimum that` flex-shrink` cannot cross. Setting` min-width: 0` on the item, or` min-height: 0` in a column container, removes the floor.


### Is` gap` supported in flexbox?


Yes. Gap in flexbox works in Chrome 84 and later, Edge 84, Firefox 63, and Safari 14.1, putting global support at roughly 94%. Safari was the last major browser to ship it, so the negative-margin workarounds in older codebases are no longer necessary.


### How long does it take to learn flexbox?


The core model, meaning the two axes plus the alignment and sizing properties, takes an afternoon of focused practice. Scrimba's dedicated flexbox course runs 52 minutes end to end. Fluency, the point where you stop guessing which property to reach for, comes from building real layouts over a few weeks.


## Key Takeaways


- Flexbox is one-dimensional: it lays out a single row or column and distributes the leftover space along it.
- ` flex-direction` defines the main axis, and the cross axis is always perpendicular.` justify-content` acts on the main axis; every` align-` property acts on the cross axis.
- ` flex: 1` is` 1 1 0%` and produces equal-width items.` flex: auto` is` 1 1 auto` and produces content-proportional items.
- Shrink factors are weighted by base size, so wide items give up more pixels than narrow ones at the same` flex-shrink` .
- Unexplained overflow is almost always` min-width: auto` resolving to` min-content` . The fix is` min-width: 0` , or` min-height: 0` in a column.
- Use flexbox for components and CSS Grid for page structure. Real layouts use both.
- Scrimba's Learn Flexbox is a 52-minute course from Per Borgen, and the free Learn HTML and CSS course covers the prerequisites it assumes, certificate included.


## Sources


- W3C. "CSS Flexible Box Layout Module Level 1." Candidate Recommendation Draft, 14 October 2025.[https://www.w3.org/TR/css-flexbox-1/](https://www.w3.org/TR/css-flexbox-1/?ref=scrimba.com)
- MDN Web Docs. "Basic concepts of flexbox."[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox?ref=scrimba.com)
- MDN Web Docs. "Aligning items in a flex container."[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Aligning_items_in_a_flex_container](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Aligning_items_in_a_flex_container?ref=scrimba.com)
- MDN Web Docs. "Controlling ratios of flex items along the main axis."[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Controlling_ratios_of_flex_items_along_the_main_axis](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Controlling_ratios_of_flex_items_along_the_main_axis?ref=scrimba.com)
- MDN Web Docs. "Ordering flex items."[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Ordering_flex_items](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Ordering_flex_items?ref=scrimba.com)
- MDN Web Docs. "min-width."[https://developer.mozilla.org/en-US/docs/Web/CSS/min-width](https://developer.mozilla.org/en-US/docs/Web/CSS/min-width?ref=scrimba.com)
- Can I use. "gap property for flexbox." Accessed July 2026.[https://caniuse.com/flexbox-gap](https://caniuse.com/flexbox-gap?ref=scrimba.com)
