---
schema_version: "1.0.0"
document_id: "45541d1c24e91471c34c64a9e1f6d943c56d8bf0bfc8b0a2e8428884a7dd0b4e"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/css-shorthand-properties/"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T15:29:32.412738+00:00"
fetched_at: "2026-08-11T15:29:34.394512+00:00"
content_hash: "sha256:dc70842c2465920e019a9553c7b4ef8d430edb2b6305d31e6c66aacc47bb772d"
---

# Common CSS Shorthand Properties

A CSS shorthand sets several longhand properties at once, and any longhand you leave out is reset to the shorthand’s default — which silently overrides anything you set for it earlier in the cascade.


If you’ve ever set` background-size: cover` and watched it vanish the moment a later` background:` line shipped, you’ve hit this the hard way. Nothing errors out, which is what makes it such a time-sink to track down. That single behavior, not syntax, is the source of most shorthand bugs. This reference covers the six shorthands you write daily (` margin` ,` padding` ,` border` ,` font` ,` background` , and` flex` ) with the value orders, required values, and reset gotchas that make each one fail silently.


## Key Takeaways


- Omitting a longhand in a shorthand resets it to a default defined by the shorthand (usually its initial value), overriding any earlier declaration, so` background: red` also wipes` background-image` ,` background-position` , and` background-size` .
- ` margin` and` padding` read clockwise from the top: top, right, bottom, left (TRBL).` margin: 0 auto` centers a block-level element horizontally.
- ` border-style` is required; with no style the default` none` wins and nothing renders, regardless of width or color.
- The` font` shorthand requires both` font-size` and` font-family` ; omit either and the whole declaration is ignored.
- In` background` ,` background-size` is only valid immediately after` background-position` , separated by a slash:` center / cover` .


## Why does a CSS shorthand reset other properties?


Every CSS shorthand expands into a fixed set of longhand properties, and[a value you don’t specify is set to a default defined by the shorthand](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Shorthand_properties) , which overrides anything you set for that longhand earlier. Writing` background: red` doesn’t just set the color; it resets` background-image` ,` background-position` ,` background-size` , and every other background longhand back to its initial value.


This is why the most common shorthand bug is never a syntax error. It’s a later shorthand quietly resetting a longhand set correctly somewhere else:


```text
/* Bug: the shorthand resets background-size to auto */
.  hero   {
background-size  :   cover  ;
background  : url(  '  hero.jpg  '  )   no-repeat   center  ;
}


/* Fix: keep size inside the shorthand */
.  hero   {
background  : url(  '  hero.jpg  '  )   no-repeat   center   /   cover  ;
}
```


This failure mode is invisible in the declaration itself and often surfaces only in the rendered page. Replaying a production session reconstructs the real computed styles, which surfaces the collapsed` background-size` that a local check with different assets can miss.


## margin and padding: the clockwise rule


` margin` and` padding` take one to four values[clockwise from the top](https://developer.mozilla.org/en-US/docs/Web/CSS/margin) : top, right, bottom, left (TRBL, the consonants in “trouble”). The count controls which sides mirror:


Values Applies to


` margin: 10px` all four sides


` margin: 10px 20px` top/bottom` 10px` , left/right` 20px`


` margin: 10px 20px 30px` top` 10px` , left/right` 20px` , bottom` 30px`


` margin: 10px 20px 30px 40px` top, right, bottom, left


```text
.  button    {   padding  :   12px   24px  ; }     /* vertical 12px, horizontal 24px */
.  section   {   margin  :   40px   auto  ; }       /* vertical 40px, horizontal auto */
```


` margin: 0 auto` centers a block-level element horizontally by splitting the leftover inline space equally. It has no effect on inline elements, and flex or grid items center through their own alignment properties instead.


## border: style is mandatory


In the` border` shorthand the width, style, and color values are order-independent because they’re type-distinct:` 2px solid red` and` solid red 2px` are equivalent. But` border-style` is required: its[initial value is none](https://developer.mozilla.org/en-US/docs/Web/CSS/border-style) , so with no style the border renders nothing regardless of width or color.


```text
.  card   {   border  :   1px   solid   #  e0e0e0  ; }    /* renders */
.  card   {   border  :   1px   #  e0e0e0  ; }          /* no style → nothing renders */
```


Width and color are optional and fall back to their defaults (` medium` and` currentcolor` ). Set style first out of habit and this class of bug disappears.


## font: size and family are required


The` font` shorthand combines` font-style` ,` font-variant` ,` font-weight` ,` font-size` ,` line-height` , and` font-family` , and[both font-size and font-family are required](https://developer.mozilla.org/en-US/docs/Web/CSS/font) . Omit either and the entire declaration is ignored. Order is strict: style, variant, and weight must come before` font-size` , and` line-height` is only valid immediately after` font-size` , joined by a slash.


```text
/* full: style variant weight size/line-height family */
.  text   {   font  :   italic   small-caps   bold   16px  /  1.5   '  Segoe UI  '  ,   sans-serif  ; }


/* minimum valid declaration */
.  text   {   font  :   16px   Arial  ,   sans-serif  ; }


/* invalid — family before size, silently dropped */
.  text   {   font  :   Arial   16px  ; }
```


Because` font` resets every omitted component to its initial value, applying it after you’ve set, say,` font-weight` elsewhere will flatten that weight back to` normal` .


## background: the position/size slash


` background` is the most error-prone shorthand. The one rule that trips up most developers:` background-size` is only valid immediately after` background-position` , separated by a slash (` center / cover` ), so a size with no position before the slash is invalid and drops. This corrects older guides that treat` background-size` as needing a separate declaration; it[belongs in the shorthand](https://developer.mozilla.org/en-US/docs/Web/CSS/background) .


The canonical value sequence is: image, position` /` size, repeat, attachment, origin, clip, color.


```text
/* correct: position / size */
.  panel   {   background  : #  000   url(  '  bg.jpg  '  )   center   /   cover   no-repeat   fixed  ; }


/* wrong: size with no position before the slash → invalid */
.  panel   {   background  : url(  '  bg.jpg  '  ) /   cover  ; }


/* wrong: no slash, so 'cover' is read as a second position value */
.  panel   {   background  : url(  '  bg.jpg  '  )   center   cover  ; }
```


Color can sit anywhere in the single-layer syntax, but keeping it last matches MDN’s ordering and reads consistently.


## flex:` flex: 1` is not` 1 1 auto`


Browsers compute` flex: 1` as` flex: 1 1 0%` , not` 1 1 auto` , so the item ignores its own width and content size when distributing space.[Browsers use a flex-basis of 0% when a single number is given](https://developer.mozilla.org/en-US/docs/Web/CSS/flex) , even though the specification text says` 0` . You can confirm this in DevTools by opening the Computed tab and reading` flex-basis` .


```text
.  grow    {   flex  :   1  ; }            /* 1 1 0%  — grows, ignores content size */
.  auto    {   flex  :   auto  ; }         /* 1 1 auto — grows from content size */
.  fixed   {   flex  :   0   0   200px  ; }    /* fixed 200px track, no grow/shrink */
.  none    {   flex  :   none  ; }         /* 0 0 auto — sizes to content, rigid */
```


Reach for` flex: auto` when you want items sized from their content, and` flex: 0 0 <size>` for a fixed track.


## What are the most common CSS shorthand mistakes?


Four failure patterns account for most shorthand bugs. Each is a direct consequence of a rule above:


1. **A later shorthand resets a longhand.** A theme override shipping` background:` wipes an earlier` background-size: cover` . Keep the value in the shorthand or apply the longhand *after* it.
2. **Missing` font-family` or` font-size` .** The whole` font` declaration is discarded, with no partial application.
3. **Missing` border-style` .** No style means the default` none` , so nothing draws.
4. **` background-size` without a position and slash.** Write` center / cover` , never a bare` cover` .


Rarely-used shorthands like` list-style` ,` outline` , and` place-items` follow the same reset rules but appear far less often; reach for them the same way once the daily six are second nature.


The one habit that prevents nearly all of these: treat every shorthand as a full reset of its property group, not an additive tweak. When a style you set elsewhere goes missing, open the Computed panel and check whether a later shorthand overwrote it. That’s almost always the culprit.


## FAQs


When should I use CSS shorthand versus longhand properties?


Use shorthand when you want to set most or all of a property group at once and are fine resetting the omitted values to their defaults. Use longhand when you need to change a single property without touching the others, such as adjusting only background-size while leaving an existing background-image intact. Shorthand always resets omitted longhands, so a targeted longhand tweak avoids that side effect.


Why does adding a background shorthand break my background-size: cover?


Because a background shorthand resets every background longhand you do not specify back to its initial value, and background-size resets to auto. If background-size: cover is set in an earlier rule and a later background: declaration omits the size, that later shorthand overwrites it. Fix it by writing the size inside the shorthand after the position, as center / cover, or by placing the background-size longhand after the shorthand.


What is the difference between flex: 1 and flex: auto?


Browsers compute flex: 1 as 1 1 0%, giving a flex-basis of 0% so the item ignores its own content size and grows purely from available space. flex: auto expands to 1 1 auto, so the item starts from its content size and then grows. Use flex: 1 for equal columns regardless of content, and flex: auto when items should be sized relative to what they contain.


Does the order of values matter in the border shorthand?


No, the width, style, and color values in the border shorthand are order-independent because they are type-distinct, so 2px solid red and solid red 2px are equivalent. The only firm requirement is that border-style must be present. Its initial value is none, so without a style keyword the border draws nothing regardless of the width or color you supply.


Open-source session replay


## Gain control over your UX


See how users are using your site as if you were sitting next to them, learn and iterate faster with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
