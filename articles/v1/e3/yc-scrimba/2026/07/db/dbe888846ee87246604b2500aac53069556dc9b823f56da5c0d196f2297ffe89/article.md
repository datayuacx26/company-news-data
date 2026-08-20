---
schema_version: "1.0.0"
document_id: "dbe888846ee87246604b2500aac53069556dc9b823f56da5c0d196f2297ffe89"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/how-to-learn-css-animations-2026/"
published_at: "2026-07-31T11:04:14+00:00"
first_seen_at: "2026-07-31T23:11:23.542680+00:00"
fetched_at: "2026-07-31T23:11:25.123786+00:00"
content_hash: "sha256:8c856b1e22f474ac415b402b442abd182f35a45422fad6c43a0452e884d12d1b"
---

# How to Learn CSS Animations [2026]

CSS animations come in two forms. **Transitions** move a property between two states when something changes, like a hover or a class toggle. **Keyframe animations** run a sequence you define with` @keyframes` , on their own schedule, with as many steps as you want. Learn transitions first, then keyframes, then transforms, then the performance rules, then accessibility.


Most people learn the syntax in an afternoon and still ship motion that feels cheap. The property names are not the hard part. *Timing* and *restraint* are: 200 milliseconds reads as responsive where 600 reads as sluggish, easing out feels like arrival and easing in like departure, and half the animations on your page should not exist. Everything below is checked against current MDN documentation, not 2018 blog posts.


## Transitions vs Animations: Which One Should You Use?


*Transitions* animate a property between two states triggered by a change. *Animations* run a defined keyframe sequence independently, looping or holding as instructed.


Transition Keyframe animation


Trigger A state change (hover, focus, class toggle) Runs on its own once applied


States Two, start and end As many as you define


Looping No Yes, via` animation-iteration-count`


Best for Hover effects, focus rings, open and close Spinners, pulses, entrances, sequences


Rule of thumb: a user action with one destination means a transition, while anything that runs without input or passes through intermediate states means` @keyframes` .


The` transition` shorthand covers five longhands:` transition-property` ,` transition-duration` ,` transition-timing-function` ,` transition-delay` , and` transition-behavior` , defaulting to` all` ,` 0s` ,` ease` ,` 0s` , and` normal` respectively, per[MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transition?ref=scrimba.com) . That` all` default is a trap: it watches every animatable property, including ones you change for unrelated reasons. Name the properties you mean.


` transition-behavior: allow-discrete` is the newer addition worth knowing. It transitions properties that normally snap between values, including` display` , so you can fade a menu out and drop it from layout in one declaration.


```text
.menu {
opacity: 0;
display: none;
transition: opacity 200ms ease-out, display 200ms allow-discrete;
}


.menu.is-open {
opacity: 1;
display: block;
}


```


## How @keyframes Animations Actually Work


A` @keyframes` rule names an animation and defines its states as percentages. An element then runs it through the` animation` property.


```text
@keyframes pulse {
from { scale: 1; }
50%  { scale: 1.08; }
to   { scale: 1; }
}


.badge {
animation: pulse 1.2s ease-in-out infinite;
}


```


Four syntax facts are worth memorizing, all on[MDN's @keyframes page](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes?ref=scrimba.com) :


1. ` from` and` to` are aliases for` 0%` and` 100%` .
2. Selectors can be comma-separated, so` 0%, 40%` applies one block to both points.
3. ` !important` inside a keyframe is ignored entirely.
4. If you omit` 0%` or` 100%` , the browser uses the element's existing styles for that end. A spinner only needs a` to` block.


The` animation` shorthand expands to nine longhands:` animation-name` ,` animation-duration` ,` animation-timing-function` ,` animation-delay` ,` animation-direction` ,` animation-iteration-count` ,` animation-fill-mode` ,` animation-play-state` , and` animation-timeline` .


Two gotchas follow. Put` animation-name` **last** , because any value that can be parsed as another animation property will be, so a keyframe set named` infinite` breaks in surprising ways. And omitting a timeline resets any previously declared` animation-timeline` to` auto` .


` animation-fill-mode` is the one beginners miss. Without it an element snaps back to its pre-animation styles the moment the animation ends.` forwards` holds the final frame,` backwards` applies the first during any delay,` both` does both.


You can also set[animation-timing-function inside a keyframe](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timing-function?ref=scrimba.com) . It applies until the next keyframe touching that property, so a value on` 100%` never runs.


## Timing Functions Are Where Animations Get Their Personality


An easing function maps elapsed time to animation progress, which is what makes motion feel mechanical, snappy, or heavy.


The four keywords are shorthands for Bézier curves. Per[MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function?ref=scrimba.com) ,` ease` equals` cubic-bezier(0.25, 0.1, 0.25, 1)` ,` ease-in` equals` cubic-bezier(0.42, 0, 1, 1)` ,` ease-out` equals` cubic-bezier(0, 0, 0.58, 1)` , and` ease-in-out` equals` cubic-bezier(0.42, 0, 0.58, 1)` .


The practical mapping is simple:` ease-out` for anything entering or responding to a click, because a fast start feels immediate;` ease-in` for anything leaving;` linear` only for continuous motion like spinners, where acceleration would look broken.


` cubic-bezier()` takes four numbers, the two control points of the curve. Y values can exceed the 0 to 1 range, which is how you get overshoot:` cubic-bezier(0.34, 1.56, 0.64, 1)` pushes past the target and settles back, playful without a line of JavaScript.


Two specialized functions remain.` steps()` jumps in discrete increments instead of interpolating, driving sprite-sheet and typewriter effects, with jump terms including` jump-start` ,` jump-end` ,` jump-none` , and` jump-both` .` linear()` takes a list of progress stops, approximating a spring as a piecewise curve.


## Transforms Are the Performant Choice


Transforms reposition, scale, or rotate an element visually without changing its box in the layout, which is why they animate cheaply.


The[2D functions](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function?ref=scrimba.com) you will use constantly are` translate()` and its axis variants (length units),` scale()` (unitless multipliers),` rotate()` (angle units like` deg` or` turn` ), and` skew()` (angles).


There are also standalone` translate` ,` rotate` , and` scale` properties, separate from the` transform` shorthand and[Baseline widely available since August 2022](https://developer.mozilla.org/en-US/docs/Web/CSS/translate?ref=scrimba.com) . They remove a real annoyance: with` transform` you must restate every function every time, so changing rotation on hover means repeating the translation.


```text
.card {
translate: 0 0;
rotate: 0deg;
transition: translate 200ms ease-out, rotate 200ms ease-out;
}


.card:hover {
translate: 0 -6px;
rotate: -1deg;
}


```


The reason to prefer transforms is not aesthetic: moving an element with` left` forces layout recalculation on every frame, and` translate` does not.


## Layout, Paint, and Composite: The Performance Rules That Follow


Every frame the browser may recalculate styles, run layout, paint pixels, and composite layers. The fewer stages your animation triggers, the smoother it runs. At 60 frames per second it has roughly **16.7 milliseconds** per frame for all of it, according to[MDN's animation performance guide](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Animation_performance_and_frame_rate?ref=scrimba.com) . Miss that budget and the frame drops.


Properties that change geometry or position, like` left` ,` width` , or` margin` , trigger style recalculation, then layout, then paint. Properties that only change appearance, like` color` , skip layout but still repaint. Properties in their own layer, notably` transform` and` opacity` , are handled in composition and skip repaint entirely.


Technique Best use case Cost per frame


` transform` and` opacity` Movement, scaling, fades, entrances Composite only, cheapest


` filter` Hover treatments, focus states Usually composited, heavy blur is expensive


` color` ,` background-color` State changes, theming Paint, cheap on small elements


` background-position` Shimmer and gradient sweeps Paint on every frame


` box-shadow` Elevation on hover Paint on every frame, worse at large blur


` grid-template-rows` Accordions with unknown height Layout every frame, fine for short one-offs


` width` ,` height` ,` top` ,` left` Almost nothing, prefer transforms Layout, paint, and composite


When something feels janky, the usual cause is animating a layout property on many elements at once.


Which brings us to` will-change` , the most misused property in CSS. It hints that a property is about to change so the browser can promote the element to its own layer.[MDN's guidance](https://developer.mozilla.org/en-US/docs/Web/CSS/will-change?ref=scrimba.com) is unambiguous: last resort only, for problems you have already measured, never on many elements because the optimizations consume real memory, and set shortly before the change rather than permanently. A blanket` .card { will-change: transform; }` across a hundred cards is not an optimization. It is a memory leak with good intentions.


## Motion Accessibility Is Not Optional


Animation can cause physical harm. People with vestibular disorders experience nausea, migraine, and dizziness from motion effects that everyone else finds pleasant.


> [WCAG's Understanding document for Success Criterion 2.3.3](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html?ref=scrimba.com) is direct about the stakes: reactions to unnecessary motion can include "nausea, migraine headaches, and potentially needing bed rest to recover."


This is not a rare edge case. A[national survey analysis in Archives of Internal Medicine](https://pubmed.ncbi.nlm.nih.gov/19468085/?ref=scrimba.com) found that 35% of US adults aged 40 and over showed some form of vestibular dysfunction on testing. Large scaling and panning effects, parallax, and full-screen slides are the common triggers.


Operating systems expose a setting for this, and CSS can read it.[prefers-reduced-motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion?ref=scrimba.com) evaluates to` reduce` when the user has enabled reduced motion on macOS, Windows, iOS, Android, or Linux.


The instinct is to nuke everything:


```text
/* Safety net, not a strategy */
@media (prefers-reduced-motion: reduce) {
*, *::before, *::after {
animation-duration: 0.01ms !important;
animation-iteration-count: 1 !important;
transition-duration: 0.01ms !important;
}
}


```


That reset is a reasonable backstop for a codebase you inherited, but the word in the media feature is *reduce* , not *remove* . Motion often carries meaning: a drawer sliding in from the right tells you where it came from, and killing it outright costs the user information. The better pattern replaces movement with a fade, because opacity changes do not trigger vestibular responses the way translation and scaling do.


```text
.panel {
animation: rise 320ms ease-out both;
}


@media (prefers-reduced-motion: reduce) {
.panel {
animation: fade-in 160ms ease-out both;
}
}


@keyframes rise {
from { opacity: 0; translate: 0 12px; }
}


@keyframes fade-in {
from { opacity: 0; }
}


```


Success Criterion 2.3.3 sits at Level AAA, so most compliance programs do not require it. Treat the operating system signal as the floor, and build the reduced variant alongside the animation rather than as a cleanup pass six months later.


## Five CSS Animation Recipes You Can Ship Today


These cover most of what production interfaces need, each with its rendering cost noted.


### 1. Hover lift


Composite-only on the transform, paint on the shadow, so keep the blur modest.


```text
.card {
transition: translate 180ms ease-out, box-shadow 180ms ease-out;
}


.card:hover {
translate: 0 -4px;
box-shadow: 0 8px 24px rgb(0 0 0 / 0.15);
}


```


### 2. Loading spinner


Note the missing` from` block. The browser fills it from the element's current styles, exactly as the implicit-keyframe rule promises. Composite-only.


```text
@keyframes spin {
to { rotate: 1turn; }
}


.spinner {
width: 2rem;
aspect-ratio: 1;
border: 3px solid currentColor;
border-top-color: transparent;
border-radius: 50%;
animation: spin 800ms linear infinite;
}


```


### 3. Fade and slide entrance


` both` is` animation-fill-mode` , and it stops the element flashing at full opacity before the animation starts.


```text
@keyframes rise {
from { opacity: 0; translate: 0 12px; }
to   { opacity: 1; translate: 0 0; }
}


.panel {
animation: rise 320ms ease-out both;
}


```


### 4. Skeleton shimmer


This repaints every frame because it animates` background-position` . Fine for a handful of placeholders, wasteful for fifty.


```text
@keyframes shimmer {
to { background-position: -200% 0; }
}


.skeleton {
background: linear-gradient(90deg, #e6e6e6 25%, #f2f2f2 37%, #e6e6e6 63%);
background-size: 200% 100%;
animation: shimmer 1.4s linear infinite;
}


```


### 5. Accordion with unknown content height


You cannot transition to` height: auto` . The grid trick works because` 0fr` and` 1fr` are both numbers the browser can interpolate. The newer answer,` interpolate-size: allow-keywords` , is still[limited availability and experimental](https://developer.mozilla.org/en-US/docs/Web/CSS/interpolate-size?ref=scrimba.com) , so grid remains the portable choice.


```text
.accordion-body {
display: grid;
grid-template-rows: 0fr;
transition: grid-template-rows 250ms ease-out;
}


.accordion-body > .inner {
overflow: hidden;
}


.accordion[open] .accordion-body {
grid-template-rows: 1fr;
}


```


## What Is Worth Learning in 2026


Three newer features are changing how CSS animation gets written, at genuinely different levels of readiness.


**Scroll-driven animations** tie an animation's progress to scroll position instead of time, using` animation-timeline` with the` scroll()` or` view()` functions. No scroll listener, no layout thrashing, no library.


```text
@keyframes reveal {
from { opacity: 0; scale: 0.96; }
}


.card {
animation: reveal linear both;
animation-timeline: view();
animation-range: entry 10% cover 40%;
}


```


The caveat is real. MDN labels` animation-timeline` as[limited availability](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timeline?ref=scrimba.com) , not Baseline, because it does not work in some widely used browsers. Chrome, Edge, and Safari ship it; Firefox still keeps it behind a flag in stable. The saving grace is the failure mode: unsupported browsers show the element without animation, so this is safe as progressive enhancement.


**` @starting-style`** fixes the problem where transitions do not fire on an element's first style update, which is why popovers and dialogs used to need a JavaScript nudge. It reached[Baseline newly available in August 2024](https://developer.mozilla.org/en-US/docs/Web/CSS/@starting-style?ref=scrimba.com) . One rule matters: place the block *after* the original rule, since they share the same specificity.


```text
.popover {
opacity: 1;
transition: opacity 200ms ease-out;
}


@starting-style {
.popover { opacity: 0; }
}


```


**View transitions** animate between two states of a page rather than of an element. Same-document transitions, driven by` document.startViewTransition()` , work in current Chrome, Edge, Firefox, and Safari. Cross-document transitions via` @view-transition` are the uneven part, with Firefox trailing, so check the[MDN compatibility tables](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API?ref=scrimba.com) before relying on them for a multi-page site.


## How to Practice CSS Animations


Rebuild one small component repeatedly, changing only duration and easing. Reading about easing does nothing; feeling the difference does.


A practice loop that works:


- Pick one real component: a toast, a dropdown, a tab switch.
- Build it with a transition first, and ship the simplest version.
- Halve the duration, then double it, and notice where it stops feeling right.
- Swap` ease-out` for` ease-in` and confirm why it feels wrong.
- Add the` prefers-reduced-motion` variant before you move on.


For structured practice, Scrimba's[Learn CSS Animations](https://scrimba.com/learn-css-animations-c02g?ref=scrimba.com) is a two-hour Pro course from Jad Khalili covering transitions, keyframe animations, transforms, vendor prefixes, and CSS variables with custom timing functions, closing on three real builds: an animated menu, a landing page, and a logo. It does not cover scroll-driven animations, JavaScript animation libraries, or SVG animation, so treat those as separate study.


Format matters more than usual here. Scrimba courses are *scrims* , interactive screencasts where you pause the instructor's recording and edit the code in place. For a feel-it-to-learn-it skill, changing a duration mid-lesson and seeing the result immediately is the whole point.


Want volume rather than instruction?[CSS Challenges](https://scrimba.com/css-challenges-c02p?ref=scrimba.com) is 2.6 hours of challenge and solution pairs from Treasure Porth, including loading animations, progress bars, and card-flip effects. If your fundamentals are shaky,[Learn HTML and CSS](https://scrimba.com/learn-html-and-css-c0p?ref=scrimba.com) is free, 5.7 hours, and taught by Per Borgen, with a wider roundup in the[best HTML and CSS courses for beginners](https://scrimba.com/articles/best-html-and-css-courses-for-beginners-2026/) . Layout pairs naturally with motion, so[Learn CSS Grid](https://scrimba.com/learn-css-grid-c02k?ref=scrimba.com) and Kevin Powell's 15.1-hour[Learn Responsive Web Design](https://scrimba.com/learn-responsive-web-design-c029?ref=scrimba.com) are the usual next steps, alongside the[CSS Grid guide](https://scrimba.com/articles/how-to-learn-css-grid/) and[responsive design guide](https://scrimba.com/articles/responsive-web-design-a-complete-guide-2026-2/) .


Scrimba Pro is $24.50 per month on the annual plan ($294 per year), with regional pricing, student discounts, and promotions available, and free courses include completion certificates. For broader options, see the[best CSS courses and tutorials](https://scrimba.com/articles/best-css-courses-and-tutorials-2026/) and the[frontend developer roadmap](https://scrimba.com/articles/how-to-become-a-frontend-developer-in-2026-complete-roadmap/) .


## Frequently Asked Questions


### What is the difference between CSS transitions and CSS animations?


A transition animates a property between two states when something changes, such as a hover or a class toggle. A keyframe animation runs a sequence you define with` @keyframes` , on its own, with any number of intermediate steps, and it can loop.


### How long does it take to learn CSS animations?


The syntax takes a few hours. A focused course like Scrimba's two-hour Learn CSS Animations covers transitions, keyframes, and transforms in a weekend. Developing good judgment about duration, easing, and when not to animate takes a few months of building real components.


### Why is my CSS animation janky?


Almost always because you are animating a property that forces layout, such as` width` ,` height` ,` top` , or` left` , or because too many elements animate at once. Switch to` transform` and` opacity` , which are handled on the compositor and skip repaint entirely.


### Do I still need vendor prefixes for CSS animations in 2026?


No. Transitions, keyframe animations, and 2D transforms have worked unprefixed in every current browser for years. You will still see` -webkit-` prefixes in older codebases and in autoprefixer output, but you do not need to write them by hand for new work.


### Should I use CSS animations or a JavaScript animation library?


Use CSS for state changes, hovers, entrances, and loops, which is the large majority of interface motion. Reach for JavaScript when you need physics, gesture-driven interaction, complex sequencing with runtime control, or SVG path animation that CSS cannot express.


## Key Takeaways


- Transitions animate between two states on a trigger;` @keyframes` animations run a defined sequence independently and can loop.
- The` transition` shorthand has five longhands and` animation` has nine; always put` animation-name` last.
- Use` ease-out` for entrances,` ease-in` for exits, and` linear` only for continuous motion.
- ` transform` and` opacity` are composited without repainting, making them the default choice for smooth animation.
- Treat` will-change` as a last resort for measured problems, never as a blanket optimization.
- Motion can cause nausea and migraine for people with vestibular disorders, so ship a` prefers-reduced-motion` variant that reduces motion rather than deleting all feedback.
- Scroll-driven animations are not yet Baseline,` @starting-style` has been since August 2024, and cross-document view transitions still lag in Firefox.


## Sources


- MDN. "transition."[https://developer.mozilla.org/en-US/docs/Web/CSS/transition](https://developer.mozilla.org/en-US/docs/Web/CSS/transition?ref=scrimba.com)
- MDN. "animation."[https://developer.mozilla.org/en-US/docs/Web/CSS/animation](https://developer.mozilla.org/en-US/docs/Web/CSS/animation?ref=scrimba.com)
- MDN. "@keyframes."[https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes?ref=scrimba.com)
- MDN. "animation-timing-function."[https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timing-function](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timing-function?ref=scrimba.com)
- MDN. "easing-function."[https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function](https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function?ref=scrimba.com)
- MDN. "transform-function."[https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function?ref=scrimba.com)
- MDN. "translate."[https://developer.mozilla.org/en-US/docs/Web/CSS/translate](https://developer.mozilla.org/en-US/docs/Web/CSS/translate?ref=scrimba.com)
- MDN. "Animation performance and frame rate."[https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Animation_performance_and_frame_rate](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Animation_performance_and_frame_rate?ref=scrimba.com)
- MDN. "will-change."[https://developer.mozilla.org/en-US/docs/Web/CSS/will-change](https://developer.mozilla.org/en-US/docs/Web/CSS/will-change?ref=scrimba.com)
- MDN. "prefers-reduced-motion."[https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion?ref=scrimba.com)
- MDN. "animation-timeline."[https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timeline](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timeline?ref=scrimba.com)
- MDN. "@starting-style."[https://developer.mozilla.org/en-US/docs/Web/CSS/@starting-style](https://developer.mozilla.org/en-US/docs/Web/CSS/@starting-style?ref=scrimba.com)
- MDN. "View Transition API."[https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API?ref=scrimba.com)
- MDN. "interpolate-size."[https://developer.mozilla.org/en-US/docs/Web/CSS/interpolate-size](https://developer.mozilla.org/en-US/docs/Web/CSS/interpolate-size?ref=scrimba.com)
- W3C. "Understanding Success Criterion 2.3.3: Animation from Interactions." WCAG 2.2.[https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html?ref=scrimba.com)
- Agrawal Y, et al. "Disorders of balance and vestibular function in US adults." Archives of Internal Medicine, 2009.[https://pubmed.ncbi.nlm.nih.gov/19468085/](https://pubmed.ncbi.nlm.nih.gov/19468085/?ref=scrimba.com)
- Scrimba. Course durations, instructors, tiers, pricing. Self-reported, scrimba.com. Accessed July 2026.
