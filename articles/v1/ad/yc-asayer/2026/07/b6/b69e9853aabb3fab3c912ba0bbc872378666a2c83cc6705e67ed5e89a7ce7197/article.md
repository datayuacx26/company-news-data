---
schema_version: "1.0.0"
document_id: "b69e9853aabb3fab3c912ba0bbc872378666a2c83cc6705e67ed5e89a7ce7197"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/use-css-if-vs-container-style-queries/"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:2998862862f4ce86232a675debea1a1f4d8c2cb572fd64b25548f834b7521cdb"
---

# When to Use CSS if() vs Container Style Queries

Use the CSS` if()` function when a single element should decide its own value inline — theming tokens, component states, a responsive property tweak. Reach for a container style query (` @container style(...)` ) when one parent’s style should drive a *set* of rules across multiple descendants. The one-line mental model: **` if()` produces an element-local conditional value; a container style query establishes context, applying conditional rule-blocks from a parent down to its children.**


Both are conditional-CSS mechanisms that read custom properties, both use the same` style()` query primitive, and their capabilities overlap enough that picking the wrong one leads to awkward code. This article resolves the overlap: what each does, the mechanical difference that decides which to use, the two syntax gotchas that bite people (semicolons and the colon-vs-equals notation), the range syntax now in both, a scenario-to-tool decision table, and how to ship either one given that browser support is split three ways as of mid-2026.


## Key Takeaways


- ` if()` reads custom properties declared in the *same* rule on the *same* element and returns one value;` @container style()` only ever looks *upward* through the cascade and applies whole rule-blocks to descendants.
- Inside` if()` , semicolons (not commas) separate condition–value pairs, and` else` provides the fallback; with no matching condition and no` else` , the function returns a guaranteed-invalid value that resolves to the property’s initial value.
- Inside` style()` , the colon notation (` style(--n: 3)` ) does token/string matching with no math, while the comparison-operator notation (` style(--n = 3)` ) parses both sides numerically and evaluates` calc()` first.
- As of mid-2026 the support stories diverge: basic container style queries ship in all four major engines (Chrome 111+, Edge 111+, Safari 18+, Firefox 151+), while` if()` and the enabled-by-default range syntax remain Chromium-only.
- Because` if()` is Chromium-only, every` if()` declaration needs a same-property fallback ahead of it or an` @supports` guard.


## What each mechanism is, minimally


The[if() function](https://developer.mozilla.org/en-US/docs/Web/CSS/if) is a value-level conditional you place inside any property. Its argument is a semicolon-separated list of condition–value pairs, where each condition is separated from its value by a colon, and an optional` else` supplies the default:


```text
.  badge   {
color  : if(
style  (--variant: danger):   white  ;
style  (--variant: muted): #  555  ;
else:   black  ;
);
}
```


Each condition is one of three query types:


- ` style()` tests a custom property on the current element;
- ` media()` runs an inline media query;
- ` supports()` runs a feature query.


The MDN reference documents all three with the same semicolon grammar — for example,` flex-direction: if(media(orientation: landscape): row; else: column;)` flips a single property on a media result.


A **container style query** is a` @container` at-rule whose condition is one or more` style()` notations reading a *containing element’s* custom properties. It wraps a block of rules rather than producing a single value:


```text
.  card   { --status:   active  ; }


@container style(--status: active) {
.  card-title   {   font-weight  :   700  ; }
.  card-icon   {   opacity  :   1  ; }
.  card-meta   {   color  :   green  ; }
}
```


Per[MDN’s container queries guide](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Containment/Container_size_and_style_queries) , every element is a style container by default — unlike size queries, **setting a` container-type` is not required** for style queries. That removes the main setup friction people expect from container queries.


## The core distinction that decides usage


The mechanical difference is scope of lookup.` if()` reads custom properties declared in the *same* rule on the *same* element and is self-contained, while` @container style()` only ever looks *upward* through the cascade to an ancestor container. MDN states this directly: using a style query inside` if()` lets you target an element based on whether a custom property is set on it, rather than checking styles on a parent. The two are explicitly framed as complementary, not redundant.


That scope-of-lookup difference has three practical consequences:


1.


**Output granularity.**` if()` sets exactly one value.` @container` applies a whole rule-block — multiple declarations across multiple selectors — when its condition is true. As MDN puts it, you can only set single property values with` if()` style queries, whereas` @container` queries conditionally apply whole sets of rules.


2.


**Direction.**` if()` can read a property declared on the very element it’s styling.` @container` cannot read its own element’s value to style that element; it reads an ancestor’s and styles descendants.


3.


**Scoping.** Only container style queries can scope by[container-name](https://developer.mozilla.org/en-US/docs/Web/CSS/container-name) . Name a container and the query evaluates only against that container; if the named container doesn’t define the queried property, the block simply doesn’t run.` if()` has no equivalent scoping control — it always resolves against the element it sits on.


So the tiebreaker in the overlap zone is: if *one* property on *one* element should flip,` if()` is less indirection. If *several* descendants must react to *one* parent value,` @container` expresses that with a single condition instead of repeating` if()` across every child.


## The syntax gotchas that bite people


**Semicolons, not commas, separate the pairs in` if()` .** This trips up everyone coming from JavaScript or from comma-separated CSS functions like` rgb()` . The grammar is` if(condition-1: value-1; condition-2: value-2; else: fallback)` , with the semicolon after the final pair optional. There must also be no space between` if` and the opening parenthesis, or the whole declaration is invalid. And when no condition matches and you omitted` else` ,` if()` returns a guaranteed-invalid value — in a normal property context that resolves to the property’s **initial** value, which is rarely what you want. Always include` else` .


**The colon-vs-equals distinction inside` style()` is the subtle, high-value detail almost nobody explains.** Both` if()` and container style queries accept two notations with genuinely different behavior, now documented on MDN.


The **plain (colon) notation** does token/string matching of computed values and performs no math. The **range (operator) notation** — using` =` ,` <` ,` <=` ,` >` , or` >=` — resolves each side, parses both as a numeric type, evaluates any` calc()` , and compares numerically. The consequence, straight from MDN’s own example:


```text
.  box   { --n: calc(  6   /   2  ); }


/* FALSE: computed value of --n is the string "calc(6/2)",
which is not the token "3" */
@container style(--n: 3) {   /* … */   }


/* TRUE: both sides parse as <number>, calc(6/2) computes to 3,
and 3 = 3 */
@container style(--n = 3) {   /* … */   }
```


So` style(--n: 3)` fails against` --n: calc(6/2)` , while` style(--n = 3)` succeeds. Two more rules matter. With the plain notation, the custom-property name must be on the left without` var()` —` style(var(--n): 3)` is invalid — whereas the range notation accepts a name,` var()` , literal, or` calc()` on either side in any order. And equivalent values (like` blue` and` #0000ff` ) only match under the colon notation if the property is registered with[@property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property) and a` syntax` descriptor. The takeaway: use the colon notation for keyword/string matching (` style(--theme: dark)` ), and the operator notation for numeric comparison (` style(--columns >= 3)` ).


## Range syntax now lives in both


Range syntax extends both mechanisms beyond exact matching to numeric comparison, drawing operands from custom properties,` attr()` , or literal values. Per the[Chrome 142 release notes](https://developer.chrome.com/release-notes/142) , a comparison only resolves when both sides resolve to the same data type, limited to seven numeric types:` <length>` ,` <number>` ,` <percentage>` ,` <angle>` ,` <time>` ,` <frequency>` , and` <resolution>` . The three-value interval form works too —` @container style(0 < --n < 10)` — with both comparators pointing the same way.


Range syntax is what makes patterns like a notification badge clean. Read a count off an attribute and branch on thresholds:


```text
@container style(--count > 99) {
.  badge  ::  after   {   content  :   "  99+  "  ; }
}
```


Because the range notation parses numerically,` style(--s = new)` is false for a keyword like` new` , while` style(--s: new)` is true — another reason the notation choice is load-bearing.


## Which to use, by scenario


Scenario Reach for Why


Theme token → one property (e.g.` color` from` --theme` )` if()` One element decides its own value inline


Component state flips several child styles (active/error)` @container style()` One parent value drives a rule-block across descendants


Responsive tweak to a single property` if()` with` media()` Localized per-property logic; no` @media` block needed


Notification-count badge thresholds Either, via range syntax` @container` if siblings also react;` if()` if only the badge changes


Contrast-aware text on a themed parent` @container style()` Children read the parent’s theme and restyle together


Feature-gated value (e.g.` lch()` with fallback)` if()` with` supports()` Single value swap on support


Multi-property layout change at a breakpoint` @media` (neither) Both are the wrong tool for large layout shifts


The rule of thumb: a single conditional *value* is an` if()` job; a conditional *context* that restyles a subtree is a` @container` job.


## Shipping it in 2026


Browser support splits three ways, and the old “conditional CSS is Chromium-only” framing is no longer accurate. **Browser support as of June 2026:**


Feature Chrome / Edge Safari Firefox


Container style queries (custom properties, plain syntax) 111+ 18+ 151+


` if()` function 137+ Not shipped Not shipped


Range syntax in` style()` /` if()` 142+ (on by default) Not shipped 151, behind` layout.css.attr.enabled`


Basic container style queries are now genuinely cross-engine. They shipped in Chrome and Edge 111,[reached WebKit in Safari 18.0](https://caniuse.com/css-container-queries-style) , and arrived in[Firefox 151, released May 19, 2026](https://www.firefox.com/en-US/firefox/151.0/releasenotes/) , which added` @container`` style()` query support based on a container’s custom properties. The` if()` function, by contrast, remains Chromium-only — MDN flags it as experimental with limited availability — and range syntax is enabled by default only in Chromium. Firefox 151 ships` @container`` style()` range syntax but disabled by default, behind the` layout.css.attr.enabled` preference.


The practical upshot:` if()` does not gracefully degrade, so write a default declaration first and let supporting browsers override it, or wrap the rule in an` @supports` guard. One pattern covers both mechanisms:


```text
/* fallback first — every browser applies this */
.  card-title   {   font-weight  :   400  ; }


/* progressive enhancement: only style-query-capable engines apply this */
@supports (  container-type  :   normal  ) {
@container style(--status: active) {
.  card-title   {   font-weight  :   700  ; }
}
}


/* if(): static value first, then the conditional override */
.  badge   {   padding  :   0.25em  ; }
.  badge   {   padding  : if(  style  (--size: lg):   0.5em  ; else:   0.25em  ); }
```


Because` if()` and default-on range syntax still render down their fallback path in Firefox and Safari — browsers a Chrome-using developer rarely opens during development — cross-browser session replay is a useful technique for actually watching the fallback branch render on a real user’s browser rather than assuming it looks right. For the deeper` calc()` and nesting patterns inside` if()` , LogRocket’s[decision-focused walkthrough](https://blog.logrocket.com/css-if-function-conditional-styling-2025/) goes further than there’s room for here.


Pick by intent, not novelty: a conditional value on one element is` if()` ; a conditional context that restyles a subtree from a parent is a container style query. Ship either behind a fallback today, re-check the support matrix before you rely on` if()` in production, and the two will increasingly cover the conditional-CSS space that once required custom-property hacks and JavaScript.


## FAQs


Can container style queries read the same element's own custom property, like if() can?


No. A container style query always looks upward through the cascade to an ancestor container and applies its rules to descendants, so it cannot read a custom property on the very element it would style. The if() function is the opposite: it reads custom properties declared in the same rule on the same element and resolves a value inline. If you need an element to branch on its own property, use if(); if a parent's value should restyle its children, use a container style query.


Do container style queries require setting a container-type, the way size queries do?


No. Every element is a style container by default, so style queries on custom properties work without declaring container-type. This differs from container size queries, which do require container-type (such as inline-size) on the queried ancestor before any size condition can resolve. This is documented on MDN's container queries guide, and it removes the main setup step developers expect when first reaching for the @container at-rule.


Why does my style query with calc() never match?


Because the plain colon notation does token and string matching with no math. A property set to calc(6 / 2) computes to the string 'calc(6/2)', which never matches the token 3, so style(--n: 3) returns false. To compare numerically, use the comparison-operator notation, style(--n = 3), which resolves each side, evaluates calc(), parses both as a numeric type, and compares numerically, so it returns true. Use the colon form for keyword matching and the operator form for numeric comparison.


Does if() degrade gracefully in Safari and Firefox?


No. As of mid-2026 if() ships only in Chromium browsers (Chrome and Edge 137+), so Safari and Firefox ignore any declaration containing it and fall back to whatever value applied before. Write a static default declaration first and let supporting browsers override it, or wrap the conditional rule in an @supports guard. Basic container style queries are safer cross-engine, shipping in Chrome 111+, Edge 111+, Safari 18+, and Firefox 151+, but the enabled-by-default range syntax remains Chromium-only.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
