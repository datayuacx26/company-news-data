---
schema_version: "1.0.0"
document_id: "4f63eb3ebb18d105a9f771d53257cc8feebcac281da939cd947dac8477ccee63"
company_key: "yc-wasp"
company: "Wasp"
source_id: "yc-wasp-rss-5b1984e54864"
canonical_url: "https://wasp.sh/blog/2026/07/13/why-design-matters-for-a-web-framework"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:24.634053+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:e58a8c38d17b6f74d2d8829ee29f6bbe7284443fce3cbea890439ed29ff29b4b"
---

# Why design matters for a web framework: a 7-year evolution

[Wasp](https://wasp.sh/) is a full-stack web framework for React and Node.js (imagine Rails for TypeScript, but with frontend included and wired up out-of-the-box), and we've been building it for a little over five years (as a full-time thing). For almost all of that time, we saw design and "branding" as something we shouldn't spend too much time on.


A framework is mostly code, we thought: you drive it from a CLI and your editor, there is little visual to look at, and these days an agent reads the docs for you half the time anyway. The most loved frameworks kind of prove the point, with Django and Rails famous and famously plain.


Turns out we were wrong, though not in the way you'd expect. What made design start to matter wasn't users or growth - it was us, the people building Wasp and the community around it.


This is the story of how we figured that out, and of the rebrand it led to, which shipped as part of Launch Week #12 - the same week Wasp went[fully TypeScript-native](https://wasp.sh/blog/2026/06/15/wasp-typescript-spec) .


## Five years of not having a design​


Same product, five years apart. Very different vibes.


Wasp has always had a logo, colors and fonts (ok, the fonts were whatever shipped with Docusaurus). So when I say we didn't have a design, I mean we never sat down to think about it as a whole. There was no central story, no backbone holding everything together, just a bunch of pages that happened to be ours.


Here's how it typically went down: with every big release (v0, Alpha, Beta), I'd sit down and convince Martin (my twin brother and co-founder) to let me "rebrand" Wasp. And every single time, I'd do almost the exact same thing: look around for some inspiration (thanks, Supabase), then patch, okay, *steal* , a few sections together into something that looked decent enough, and ship it.


And early on, that was the right call. When you're still proving the product deserves to exist, obsessing over a design system is a great way to burn weeks you don't really have. Plenty of founders do the opposite. They polish pixels on something nobody wants yet (and we witnessed that many times) and we were determined not to be them.


But design debt compounds, just like code debt does. Over five years, Wasp itself grew up *a lot* , and it grew much faster than its design did, until at some point it just overgrew it. The framework got serious, but the brand stayed improvised. You can kind of watch it happen across our old landing pages:


stic (before it was called wasp), 2019


01 / 04


And once our design felt broken, there was little motivation to keep it right, so it just kept getting worse. At one point we needed to show a roadmap on the site, so Martin just screenshotted our GitHub project board and pasted it straight in. Low-res, rounded corners not quite matching, with some random background bleeding out around the edges. It stayed up like that for an embarrassingly long time:


Our roadmap section. Yes, that is just a screenshot of our GitHub project. Not even properly cropped.


Or take the "How does it work?" section right below it. The diagram was originally drawn as a transparent SVG, which broke once the site got a dark mode, so someone slapped a white background behind it to "fix" it. Now it looked fine on the dark version, and noticeably worse everywhere else. Add the orange highlight pills next to the copy, styled in a completely different visual language than anything else on the page, and the whole thing kind of looked like three different sites stitched together:


Our old "How it works" section, broken in a few different ways.


At some point we just started to feel bad about how unpolished it looked. We couldn't really tell you what users made of it (probably not much), but we could feel it ourselves: Wasp had grown into a serious product that people use for[serious things](https://wasp.sh/blog/2026/06/17/made-with-wasp-vol-2) , and it was about time it started looking like one.


## Looking unpolished made us feel unpolished​


There was also another reason for finally taking design seriously, harder to pin down, but looking back it's pretty obvious.


Looking unpolished slowly does something to you. Every time we shipped a great feature next to a website that looked like a slightly abandoned student project, it sent a quiet little signal we never really meant to send: that maybe we don't actually care all that much. And that signal doesn't only go out to users; it also comes back in. A product that looks like nobody sweated the details slowly starts to *feel* like one to the people building it, even when, internally, the opposite is completely true.


You don't really notice it on any given day, but over five years it compounds. You hesitate a bit before posting the screenshot. You add a caveat before the demo ( *"ignore how it looks for a second, just look at what it does"* ). You quietly stop wanting to share Wasp with someone whose opinion you actually care about, because they might judge the wrapper instead of the thing inside. And it gets hard to pour your full energy into something you're a tiny bit embarrassed by.


So in a real sense, a good part of this rebrand wasn't for users at all. It was for us. We wanted to be able to look at every surface Wasp touches and not flinch, and to feel like the way Wasp looks finally matches how much we actually care about it. Looking back, that's not really vanity but basic hygiene, and we should've done it sooner.


## The product changed, so the skin had to​


When we[made the call to go all-in on TypeScript](https://wasp.sh/blog/2026/05/13/new-language-for-web-dev-was-a-mistake) , we knew it wasn't a facelift. It changed what Wasp fundamentally *is* : not a custom language you have to learn, but a framework that meets you where you already are.


That forced an uncomfortable question: if someone landed on wasp.sh today, would they actually see that? Honestly, no. The old look still said "quirky side project with a custom language", while the new Wasp is something else: a serious, full-stack TypeScript framework you'd reach for to build a real product. The caterpillar was getting its wings (ok, wasp, but you get the idea), and it couldn't really keep wearing the old skin.


So instead of asking ourselves "what looks nice?", we ended up asking a much better question: **what is Wasp, actually?**


## Wasp is a spec. So the brand is a blueprint.​


The core idea behind Wasp has always been the same, DSL or no DSL: you declare *what* your app is (routes, pages, auth, jobs, the whole skeleton), and Wasp handles all the plumbing under the hood. The high-level spec you define in TypeScript is essentially an engineering drawing of your application, and the framework just builds it.


(Quick side note: "Wasp" itself stands for **W** eb **A** pplication **SP** ecification - which is basically how we've always thought about it.)


But before we landed there, we explored a *lot* of other directions. Some internal attempts (mostly me prompting Nano Banana), and we also briefly worked with a few designers who came back with their own takes. AI turned out to be genuinely useful in this phase: you can iterate on entire landing-page mockups in a few minutes and quickly see what does and what doesn't feel like Wasp, and build intuition for what you like. Here's one of mine:


One of my early Nano Banana runs. Already leaning hard into the schematic / blueprint angle that ended up shaping the final brand.


And here's a small sample of what the designers sent over:


(Click to zoom): A few of the directions designers came back with. Honestly, none of them really clicked for us, probably because the process happened too much in isolation from us.


After cycling through enough of these, the visual direction we wanted got pretty obvious. Wasp's brand should look like what it is: **a technical spec** . Blueprints, schematics, annotated diagrams: the kind of stuff engineers draw before they go and build.


That single idea ended up driving basically everything in the new identity:


The new Wasp brand system.


**Monospace first.** Our primary typeface is JetBrains Mono, backed by IBM Plex Mono. Monospace is the native typography of our world and we just really like how it looks. We were deliberating about using monospace literally everywhere, but in the end decided to settle on titles and subtitles, while the body text in the docs and the blog is IMB Plex Sans - easier on the eyes, but still feels close to the monospace aesthetic.


**Three colors, and that's it.** Wasp Yellow (#F5C842), Ink Black (#111), and a warm paper tone we call Blueprint Paper (#F7F5F0). Yellow and black because, well, wasp. Paper because real engineering drawings live on paper, not on gradients.


**Sharp corners, visible structure.** 90° angles, 2px strokes, no rounded-blob softness anywhere. Our illustrations aren't there to decorate the page. They're schematics: boxes, connectors, annotations, the architecture of your app drawn like a circuit diagram.


**The mark.** The` =}` symbol, set in a solid square. It's pulled straight from code, works at any size from a favicon to a billboard, and if you squint a bit it still reads as a wasp's wings and antennae. It didn't arrive fully formed, though. The project actually started life as *stick* (yes, there was a literal photo of a wooden stick), then *stic* (Specification to Implementation Compiler), and finally *wasp* . The` =}` itself only showed up around the Alpha launch in 2020, sitting inside a yellow circle. The 2026 rebrand was basically the moment we finally squared it off and stopped fiddling with it:


The Wasp mark, from a stick to a square.


**Light, on purpose.** One thing we very deliberately kept: Wasp stays light theme-first. It would have been the easy, default move to go dark-mode-everything and look like every other devtool out there. But people have always told us that Wasp's look feels *happy* (warm and approachable in a category that mostly reads as cold and serious), and we didn't want to just throw that away to become another Linear clone. The world already has plenty of near-black landing pages with a purple gradient on top. Blueprint paper is bright, optimistic, and it's ours.


## Constraints are the whole point - both in design and code​


Here's the part we honestly didn't expect: the best thing about the new brand isn't really how it looks, but how *fast* it now makes us move.


Two fonts, three colors, one illustration style, and only sharp corners. It sounds restrictive on paper, and it is, but in the best possible way. Before, no one really knew what to reach for, so everyone would just do their own thing each time. Now there's almost nothing to decide. A launch banner, a chart for Twitter, a docs page, a sticker - they all kind of assemble themselves from the same small kit. It's actually the same philosophy as Wasp itself: strong defaults and clear structure don't limit you - they free you up to focus on the parts that actually matter.


We even wrote ourselves a proper brandbook in the end, partly because we genuinely needed one, and partly because after five years of winging it, it just felt really, really good to finally have one.


The color-system page from our brandbook.


## We shipped it like a rolling release​


We didn't flip a switch one morning and unveil a finished new site. We rolled it out the same way we ship software: piece by piece, swapping new parts in while the old ones were still live.


So for a while, wasp.sh was a bit of a Frankenstein (honestly, it kind of still is). A shiny new hero sitting on top of an old footer, new buttons next to old cards, fonts not quite agreeing with each other across the page. It looked pretty rough, but the old site already looked rough, so a half-migrated version was still a step up, not a regression. The bar, luckily for us, was very forgiving.


And honestly, the awkwardness actually helped. Having a half-finished site sitting out in the open was a little embarrassing, which was kind of the whole point. Nobody really wants a Frankenstein page on the internet with their name on it, so we kept sprinting to fix the next piece before too many people saw it in that state. The embarrassment basically became a deadline we couldn't ignore.


And it was the only realistic way we were ever going to get it done. If we'd waited to perfect every pixel and ship it all at once, we'd genuinely still be waiting today. Wasp moves too fast for a big-bang redesign. By the time it was "done", half of it would already be stale. Shipping it in pieces is what actually kept us moving.


## Designing it changed what we think Wasp is​


Another thing we found really valuable about this whole process had nothing to do with how anything looked.


To draw a thing, you first have to know what the thing actually *is* . So every design decision kept bouncing back at us as a product question: who is this really for, what do we stand for, what do we actually want to say. Before we knew it, the rebrand had quietly turned into a rewrite. We reworked our words, our positioning, our whole story, and then the new words changed the design, and the new design changed the words again. The two kept reshaping and reinforcing each other.


That loop is exactly why we're glad we didn't just hand this whole thing off to someone external. An agency can absolutely make you *look* good. What it can't really do is the part that turned out to matter most: the way designing the brand forces you to understand, and sometimes redraw, the product itself. That kind of thing only really happens when the people building the product are also the ones drawing it.


## What do you think?​


The new look is now live on[wasp.sh](https://wasp.sh/) , across our docs, and everywhere else Wasp shows up. Like everything we ship, it'll keep evolving, and we'd love your honest take: the good, the bad, and the "did it really have to be monospace?".


Come tell us on[Discord](https://discord.gg/rzdnErX) ,[Twitter/X](https://twitter.com/WaspLang) , or[GitHub](https://github.com/wasp-lang/wasp) . 🐝
