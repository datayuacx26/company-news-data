---
schema_version: "1.0.0"
document_id: "1b806f7546962beabe0afcff31fdb56f765314140b2c52f33c535d7e35d7f99b"
company_key: "yc-font-awesome"
company: "Font Awesome"
source_id: "yc-font-awesome-atom-991752a735d6"
canonical_url: "https://blog.fontawesome.com/how-does-web-awesome-stack-up-against-shoelace/"
published_at: "2026-08-17T16:37:18+00:00"
first_seen_at: "2026-08-17T18:30:03.330314+00:00"
fetched_at: "2026-08-17T21:37:21.506005+00:00"
content_hash: "sha256:3f28d19886febe5e468aa0e1f66c6b0dd9211c0b0cb63ba412c4c4e2fd2ebc8a"
---

# How Does Web Awesome Stack Up Against Shoelace?

The latest release of Shoelace shipped on March 11, 2025. It’s still on npm, it’s still two lines away from any HTML page, and it still works as well today as it did the day it went out.


It’s also the last Shoelace release there’s ever going to be.


None of that is news.[We launched Web Awesome back in October](https://blog.fontawesome.com/web-awesome-component-library/) , and the banner has been sitting on the Shoelace docs ever since. So I’m not here to tell you it happened. I’m here to answer the question everyone keeps asking:


*Why should I switch to Web Awesome?*


“Because it’s the successor” doesn’t cut it when you’ve got an app full of` sl-` tags and a manager asking why you want to spend a week on this.


So let’s put them side by side. Nothing breaks tomorrow if you stay put, but the case is better than most folks realize, and it’s a lot more than a rename.


## What Actually Changed


Shoelace was a component library. Web Awesome is a design system.


That sounds like something a marketing page says, so let me be specific. Shoelace gave you 58 components, a set of` --sl-*` tokens, and a light and dark theme. Everything else was your problem: layout, typography, spacing, the styling of your plain old` <table>` and` <h2>` .


Web Awesome 3.11 gives you 70 free components *plus* the rest of the page. Native element styles. Layout and typography utilities. Eleven themes. Nine color palettes. A theming API that goes deeper than swapping two stylesheets.


And it’s still free and open source under the MIT license. That part was never negotiable.


## What Folks (Probably) Care About the Most


There are components you couldn’t build with Shoelace without writing your own.` <wa-callout>` for inline messaging.`[<wa-popover>](https://webawesome.com/docs/components/popover/)` for anchored content that isn’t a tooltip and isn’t a dropdown.`[<wa-scroller>](https://webawesome.com/docs/components/scroller/)` for overflow containers with real scroll affordances. Then`[<wa-accordion>](https://webawesome.com/docs/components/accordion/)` ,`[<wa-page>](https://webawesome.com/docs/components/page/)` ,`[<wa-number-input>](https://webawesome.com/docs/components/number-input/)` ,`[<wa-time-input>](https://webawesome.com/docs/components/time-input/)` ,`[<wa-zoomable-frame>](https://webawesome.com/docs/components/zoomable-frame/)` ,`[<wa-intersection-observer>](https://webawesome.com/docs/components/intersection-observer/)` , and more.


Your plain HTML gets styled, too. Web Awesome ships a` native.css` layer that styles real elements: headings, tables, form controls,` <figcaption>` , the works. If you were around for the[original Shoelace](https://www.abeautifulsite.net/posts/shoelacecss-a-back-to-the-basics-css-starter-kit/) in 2017, that might feel a little familiar. That project was a CSS starter kit before it was ever a component library. It took me about nine years, but the idea made it back in


You get CSS utilities, too. Layout primitives like` wa-stack` ,` wa-cluster` , and` wa-split` . Gap, alignment, border radius, and text utilities. A` wa-prose` class for long-form content. Nothing resembling a build step.


Component styles live in` @layer wa-component` , which means your unlayered CSS wins by default. Go delete your` !important` declarations. I’ll wait.


Icons are Font Awesome by default instead of Bootstrap Icons, and if you have a Font Awesome Pro Kit, that works too.


There’s server-side rendering. As of 3.8, components render on the server via Declarative Shadow DOM and hydrate when your JavaScript arrives. Less layout shift, better first paint. It’s experimental, but it’s real and people are using it.


And there’s a theme called Shoelace. There’s a built-in` shoelace` theme and a matching` shoelace` color palette, so you can migrate the markup without explaining to your designer why every button looks different on Monday.


It installs the way it always has. Paste a stylesheet and a module script into your` <head>` , served from our own CDN, and start writing tags. Or` npm install @awesome.me/webawesome` and bundle it like anything else. No framework, no wrapper, no build step you weren’t already running.


## Side by Side


**Shoelace 2** **Web Awesome Core** **Web Awesome Pro**


**Status** Sunset, 2.20.1, March 2025 Active, 3.10.0, June 2026 Active


**License** MIT MIT Commercial, alongside the MIT core


**Components** 58 66 Everything free, plus the list below


**Icons** Bootstrap Icons Font Awesome Free Font Awesome Pro via Kit


**Themes** Light + dark Default, Awesome, Shoelace 8 more (Active, Brutalist, Glossy, Matter, Mellow, Playful, Premium, Tailspin)


**Color palettes** 1 3 9


**Native element styles** – ✅ ✅


**CSS utilities** – ✅ ✅


**Cascade layers** – ✅ ✅


**Server-side rendering** – ✅ ✅


**Theme Builder** – – ✅


**Pattern library** – – ✅


**Hosted projects + CDN** – – ✅


**Figma design kit** – – ✅


**Support** Community Community + us Priority support


**Price** Free Free[Commercial license](https://webawesome.com/purchase)


Pro components, for the record:`[<wa-combobox>](https://webawesome.com/docs/components/combobox/)` ,`[<wa-file-input>](https://webawesome.com/docs/components/file-input/)` ,`[<wa-date-picker>](https://webawesome.com/docs/components/date-picker/)` ,[<wa-date-input> ,](https://webawesome.com/docs/components/date-input/)`[<wa-video>](https://webawesome.com/docs/components/video/)` ,`[<wa-video-playlist>](https://webawesome.com/docs/components/video-playlist/)` ,`[<wa-sparkline>](https://webawesome.com/docs/components/sparkline/)` , and the Chart.js-backed chart family (bar, line, pie, doughnut, bubble, radar, scatter, and polar area). A data grid is on the way.


## A Word About Pro


I’d rather you hear this from me than figure it out halfway through a migration.


Web Awesome Pro exists so Web Awesome Core can keep existing. Every subscription pays for the bug fixes and the new components that everyone gets, free users included. Pro just moves you to the front of the line when you need a hand. Over 7,000 people backed the Kickstarter to make that model work, and I’m still a bit stunned by it.


Almost none of your migration touches Pro. The components you built your Shoelace app with are in Core, and that’s where they’re staying. The one place Pro comes up is if you had a second library bolted onto Shoelace for charts, comboboxes, or the like. Those live in Pro now, and moving to a first-party version is optional, not required.


## How Hard Is This, Really?


Less hard than you’re expecting.


Most components migrate on a` sl-` to` wa-` rename and nothing else, which covers roughly two thirds of the library. Tokens go from` --sl-*` to` --wa-*` .` variant="primary"` becomes` variant="brand"` . The` outline` and` filled` booleans became` appearance="outlined"` and` appearance="filled"` . Slots named` prefix` and` suffix` are now` start` and` end` .` help-text` is now` hint` .


A handful of things changed shape rather than name.` <sl-menu>` became` <wa-dropdown>` ,` <sl-range>` became` <wa-slider>` ,` <sl-image-comparer>` became` <wa-comparison>` , and` <sl-visually-hidden>` is now a` wa-visually-hidden` utility class you can put on anything.


The parts that bite quietly are event listener strings,` ::part()` selectors, and icon names. None of those throw an error. They just stop doing what you expected. Color scales also inverted, so` --sl-color-primary-600` doesn’t map to the number you’d guess.


All of it is written down in the[migration guide](https://webawesome.com/docs/resources/migrating-from-shoelace/) , along with a[checklist](https://webawesome.com/docs/resources/migration-checklist) you can work through file by file. The guide also includes a prompt you can hand to a coding agent, and it’ll do most of the mechanical work for you. I was skeptical about including that. Then I watched it migrate a real project in an afternoon.


## Should You Migrate?


If your Shoelace app is stable and shipped and you’re not touching it, no, you don’t need to do anything this week. It’ll keep working.


But there won’t be another Shoelace release. No new components, no fixes, no answers on the issue tracker. Every hour of work is going into Web Awesome now. If you’re actively building on Shoelace, every new feature you ship is more code you’ll eventually have to migrate. The sooner you switch, the less of it there is.


Start with the[migration guide](https://webawesome.com/docs/resources/migrating-from-shoelace/) . Pick one page. Rename one prefix. It’s usually much easier than it sounds.


## Thank You


More than a year after the last release, Shoelace is still serving[over two million requests a month](https://www.jsdelivr.com/package/npm/@shoelace-style/shoelace) on jsDelivr alone. That’s all of you, still building things. Every issue, every PR, every “hey, this is broken” in Discord got the project further than I could have taken it alone, and Web Awesome is that same project with more people and more hours behind it.


Come see what we’ve built. And keep building awesome stuff!
