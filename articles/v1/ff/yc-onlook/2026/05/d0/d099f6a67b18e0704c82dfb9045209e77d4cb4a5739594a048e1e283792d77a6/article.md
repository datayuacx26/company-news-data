---
schema_version: "1.0.0"
document_id: "d099f6a67b18e0704c82dfb9045209e77d4cb4a5739594a048e1e283792d77a6"
company_key: "yc-onlook"
company: "Onlook"
source_id: "yc-onlook-rss-a3426f90edb2"
canonical_url: "https://onlook.substack.com/p/state-of-design-in-2026"
published_at: "2026-05-19T20:00:50+00:00"
first_seen_at: "2026-07-27T04:07:26.124598+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:3495a24986bc7876fd9f587581cc0f0fc86acfa4bf378b746d4d09b507c78d43"
---

# The future of design tooling in the age of AI

# The future of design tooling in the age of AI


### Everyone Coded. Nobody Curated.


[Daniel Farrell](https://substack.com/@danielfarrell)


May 19, 2026


Open ten landing pages that shipped in the past few months. Same gradient hero. Same dot-grid background. Same testimonial card with the same drop shadow. This is what a commoditized medium looks like, and it’s the best news designers have had in a decade.


For the past few months, a lot of people who are


*not* designers have been claiming AI will replace designers. The take has gotten loud enough that I want to push back on it — not because it’s threatening, but because it’s wrong in a specific and interesting way.


What’s actually happening is that the entry into design got a lot broader and a lot easier. The floor of web design has been raised across the board. Anyone with a frontier model open can now produce something that, a year ago, would have been gated behind a junior designer or an agency. That is real.


But raising the floor produced a second effect almost nobody is talking about: AI has pulled more people into design and multiplied the volume of work shipping — and in doing so, it has raised, not lowered, the value of differentiation. When everyone can ship competent, competent is no longer the moat.


So with this new level of competence, what does it mean to be a designer in 2026?


## **Code finally became a design medium**


A year ago, it was hard to believe AI would play a major role in design, and it’s hard to blame the skeptics. You’d prompt something and, if you were lucky, get something usable. Layouts would drift, text would overflow, or the result would come back as a big red “Error” you’d have to paste back into your agent to debug. This was a huge blocker for our second Onlook product – Sonnet 3.5 was just not good enough to allow you to just prompt.


These error cases have now


*almost* entirely stopped happening. Agent workflows have gotten more sophisticated with the introduction of Claude Code and Codex, AI models have provided more reliable output, and everyone has gotten a little more familiar with how to ask for something reasonable from an AI (”build me a snippet of an app, rather than the entire app”). Harnesses (the layer around a model that shepherds it along to something useful) have started to emerge as a real category. The frontier coding products are now genuinely capable. They build things that are stable, and because the work is stable, we can finally start generating interesting artifacts on top of it. The latest generation of image models has been an excellent lever on top of that.


We are at the very beginning of a new medium. The medium is designing directly in code, through an LLM.


Designing in code has been around for a while. People have anointed themselves design engineers in the context of producing with code, and historically those people could hand-craft things line-by-line in code. That’s still a real and excellent role, but the prerequisite for “Design Engineering” has changed. Now that prompting is the input modality, anyone can really be a design engineer, it just depends on your appetite for prompting and your tolerance for the iteration loop (which sometimes puts a big “Error” screen in front of you!).


## **Everyone coded. Nobody curated.**


The challenge for the design career, for the design professional, is now to have enough range in expression (~”taste”) to produce something that is actually unique. Given the proliferation of the same handful of fonts (primarily ones served for free on Google Fonts), the same default color palettes (pretty much default Tailwind Styles), and the same standard web design vocabulary, there has been an explosion in creative work for the web represented in code.


The most popular fonts on Google Fonts today


And yet there’s a massive vacuum in the polish that is required to actually finish something.


Look at early-stage brands today – they’re all suffering from homogeneity. Any startup building a landing page right now has to grapple with the fact that, by default, it just feels boring. It’s just where things are. Polish is missing from the design process partly because a single prompt can get you somewhere acceptable, partly because budgets are limited, partly because everyone needs to move faster than ever. There is less internal appetite for things that are very well put together, and less time for things that are incredibly well curated.


What’s actually happened is that the entire web / interface design industry has exploded in volume of work. And very soon, it will follow with an explosion in demand for high-quality work. There will always be a need — and an excitement — around the higher end. The floor for creative implementation of design in code has risen substantially, and soon to follow is a demand for designers who really understand the medium AND can implement aritifacts that can be used in the medium.


## **Systemization becomes a primitive**


There’s an underlying theme of systemization happening with design, which has always existed in some form. Brand guidelines are the core primitive of it, and are essentially a higher abstraction of a “Design System”. What’s different now is that those guidelines are being represented in code, which means they can be translated into usable artifacts that agents can build on top of.


Whether it’s a design.md file, a set of references, an image library, or something else, the agents are now able to play and remix with the resources that are out there. Which, if we’re being honest, is what design has always done: remix what already exists. The difference is that anything code-native is now up for grabs. There are extensions that let you extract code from any site and paste it into your design environment. Or you can just delegate the task to your agent and tell it to go and pick something up and work with it.


From getdesign.md – the “Apple Design System.” Technically Apple’s colors but... not quite Apple...


Tools like


[Mobbin’s MCP](https://mobbin.com/mcp) and


[Refero’s MCP](https://refero.design/mcp) give you access to a huge library of other people’s extremely thought-out design decisions to borrow and remix.


Two things also seem to be true that pull in opposite directions.


The first is that there is a heavy, heavy subsidization happening underneath all of this. The code-production machine that the frontier labs are running is, functionally, a (subsidized) drug and it is absolutely flooding the market with work. It is unclear how long this stays cheap, and unclear what the experience feels like when the dosage costs catch up.


The second is that cost is already starting to become apparent at the margin. People are realizing they need to get more bang for buck. They’re realizing that smaller models can inhibit certain tasks, or that 3 prompts to a cheap model is equivalent to 1 prompt to a more expensive model (and way less headache). They’re realizing that, much like delegating to a person who needs to ramp on a project, the agent’s inherent memory and context on a given decision matters a lot. Persistent context is going to be one of the most undervalued primitives of the next year, especially as we get into teams optimizing harnesses we talked about a little earlier from temporary fixtures to temporal systems (i.e. with Memory).


Thanks for reading Onlook! Subscribe for free to follow-along with our journey of building a next generation design tool for the AI era


## **What it means for designers**


The baseline expectation for design has been lifted. If your design skills are not at the same bar as an LLM’s ability to suggest something very basic, you need to upskill quickly. I’m talking mostly about UX work here — the kind of “obvious” decisions that used to differentiate a competent product designer from a junior one are now one prompt away. To stay ahead of that, you need a sharp sense of what is possible and what can be done, so you actually know what to ask for.


Crafting some header ideas in Onlook for this article – How can you know what you can create if you don’t try to find out?


The other thing is that designers are, more than ever, stewards of the overall experience. And thankfully, the remixability of code makes that easier, not harder. It is now easier than ever to bring systems into the work you’re creating. You can pull in packages that immediately upskill what you can produce. You can bring in resources and elements from across the open web because, if you’re working in code, everything is interoperable.


The expansion of model context protocols extends this further: ideas now translate across mediums. You can pull a spec out of one tool, cross-reference the engineering work happening on it, and have a visual editor represent it for you on a canvas — a canvas that is in truth sourced in code, exportable, and responsive by default, just like what we’re building with


[Onlook](https://onlook.com/) .


The phone is a useful analogy here. The phone in all of its conveniences opened up so many different opportunities — browsing the web on a small screen, apps as a category, design that has to work in fundamentally new contexts.


Code-as-design-medium has the same kind of expanding surface. The more your work is represented in code, the more use cases and form factors you can serve, and the more boundaries of what technology can offer you can actually push. The best design solves for the most cases. Code is the substrate that lets you do that.


## **Design tooling has not caught up**


As of right now, it seems clearly true that design tooling has not actually caught up with dev tooling. This is a sentiment I’ve heard echoed by senior design leaders at the largest organizations. (


[@hannah_hearth](https://x.com/@hannah_hearth) , Head of Design at Vercel, had an excellent


[writeup on this on her website](https://www.hannahhearth.com/posts/tools-the-vercel-product-design-team-actually-uses) )


A lot of designers have started to build their own tooling, which is great. But that tooling tends to be reserved for a very specific niche — a precise comment-and-feedback flow, a particular kind of input control, one slice of the process at a time. It is uncertain how many different services you’re going to need stitched together in order to have the full experience of a former design tool while generating things visually via code. Right now it feels like a duct-taped pile of disparate tools, and the opportunity for a centralized solution is still ripe.


Lots of teams have been chasing the original sin — the failure to build a code-native canvas. The incumbent design tool has shipped a large number of product improvements that, again, are pseudo to the real code. Their AI generation product is probably the best bet they have at maintaining a foothold, but their recently introduced web product seems ill-timed in a way no one really could have known.


One way to see the future is to look at what founders in San Francisco are doing, and an enormous portion of the recent YC batch companies have vibe-coded their websites. Tragically, most of these websites look pretty boring, but they do get the company up-and-running in a medium (code) that offers maximum flexibility.


A lot of teams building in the design tooling space have also been chasing after the HTML and CSS representation of code while still dragging along the old tooling assumption of the prior era — direct manipulation as the primary (and sometimes only) input. That assumption is the thing I’m most convinced is going to break.


## **The harness question**


It is still very much yet to be seen how creative tools — and the agent harnesses underneath them — operate outside of the developer harness category. There is a very large need for tooling to understand what a


*design* harness even is and how it’s supposed to operate. What does state look like inside it? What does context look like? What gets persisted across sessions? What is a “skill” that is supposed to express something visual?


The optimism here is strong. There are lots of great attempts at bringing in the ability for models to generate things visually. The divide between UI generation and image generation is closing a little with the latest image models (GPT Image 2 has been surprisingly good at rendering images of designs), but there is still a real disconnect (severance?) between the medium of flat images and code.


Sound Picker Modal generated in Onlook on the left, and an image produced by GPT Image 2 of four variants of the Modal on the right. The prompt was to take the upper left Concept 01 from the generated Image and translate it into a usable design.


As randoms on the internet continue to rant and rave about how design is dead, or how a new tool has been revealed that supposedly ends the discipline, I want to emphasize that most of this is happening from people who are not really designers. Yes, anyone may now be able to produce something with a modicum of creativity higher than what they have done before. But at the end of the day, designers are always looking to push the limits. We will probably never be satisfied. We are always looking for something bolder, more creative, something that pushes farther than the default.


Actually, I’ll take that back. Designers will


*never* be satisfied. Because our responsibility is to stretch the creative limits of what we can do with what we have.


It can also be true that designers themselves have been eager and excited by this new medium, and the ones on the bleeding edge have embraced new tooling and effectively graduated themselves into a kind of front-end engineering. What’s left, then, is a very open and not-yet-well-understood space: working with creativity inside the harness category, and inside the model category. The people doing the most innovative work right now are either at the model labs or very much on the fringe in a tiny startup, and it’s been impressive to hear the lengths of experimentation teams are going through in order to codify “taste” into their systems.


## **A new medium, not a better version of the old one**


It seems like the industry has been chasing the old model of what design is, at exactly the moment when we’re just beginning to discover the expression of a new medium — which is the visual representation of language.


Onlook’s positioning is that the next design tool of the future will not look like the design tools of the past.


We’ve seen this ourselves, in the most expensive way possible. We invested deeply, months in, into building styling tooling — an egregiously difficult problem to solve across all the different surfaces a designer touches. We barely satisfied it for one CSS framework, and even then, we didn’t get it quite right. Code is expressive


*temporally* . You’d change the color of a button, and it would actually change in the codebase — but only for that one state of the button. Buttons have multiple states. Hover, active, focus, disabled, the dark variant, the small variant, the loading variant. Solving that across the matrix of frameworks and conventions is its own multi-quarter problem.


The color picker in Onlook, allowing you to change the value of a Tailwind CSS variable of a selected element.


What I keep seeing across this space is people investing into parts of the product surface that


*seem* like they would be important, when the numbers say otherwise. Our own stats showed that barely anyone used the styling features we’d invested so deeply in.


What we found is that, in the age of AI, the biggest lift comes from how good the agent is and how effective it is at translating what you actually need. To get an excellent result out of the model, you have to be able to find it with the most relevant context up front. Any tool that falls short of context curation and context execution is not going to be a tool that succeeds in this new future.


The industry is expanding quite rapidly. That’s not to say the older tools don’t have space in it — they do. But the real opportunity, the one we’re building toward at Onlook, is a legible way for people to interact with this new medium: AI in partnership with code, producing work that is both more useful and more creative than what any one of those layers could produce on its own.


## **Where this is by year-end**


Here’s where I think this is by December.


The static design file stops being the default artifact for new product teams. Not gone — design files don’t disappear — but for teams starting work after the summer, the default canvas is the codebase itself, and the design file becomes an exploration surface rather than the source of truth of discussion. I know lots of people have preferences about the process of drawing rectangles to process and explore things (


[@karrisaarinen](https://x.com/@karrisaarinen)


[posted here](https://x.com/karrisaarinen/status/2039553534837117151) ), and there will be room for that, but I know there’s already a sense of tension between designers who


*code* something vs. designers who draw something during design reviews.


Brand-as-code becomes a real practice. design.md, brand.json, whatever the convention ends up being — teams will start versioning their guidelines the same way they version their components, because that’s the only form the agents can actually consume reliably, and...


Agents will become first-class users of design tools. They already are playing an enormous role in the generation of concepts, but a space will emerge where agents will be able to drive autonomously to do creative work, and that space will not look like a static experience.


As a toolmaker, we made the decision to not invest deeply in the old format of direct manipulation. Instead, we’re looking to the exciting, dynamic, and meta world that is emerging in the space of agents and crafting the verification layer for their creative work.


[signüll @signulll the future interface is probably three layers: 1. ambient intent capture voice, location, calendar, screen context, messages, habits, biometrics, etc. the system understands what you’re trying to do before you explicitly “open” anything or augments your intent deeply. 2. 10:56 PM · Apr 26, 2026 · 180K Views 144 Replies · 234 Reposts · 2.45K Likes](https://x.com/signulll/status/2048536648804155434?s=20)


Of course, if the next 90 days is anything like the last 90 days there will be plenty of disruption and excitement and claims and prophecies about the future of design, but I’m confident in the designer’s role and am eager to experience the crazy cool and creative things that designers will make with the medium of code.


If you’re interested in trying out the agent verification layer for design we’ve been building at


[@onlookdev](https://x.com/@onlookdev) , email me or join our waitlist at


[Onlook.com](https://onlook.com/) !


Daniel


---


Some of our other posts you may enjoy:


[Our August 2024 Update](https://onlook.substack.com/p/august-2024-update)[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell) and[Kiet Ho](https://substack.com/profile/46760661-kiet-ho)


·


July 15, 2025


[Read full story](https://onlook.substack.com/p/august-2024-update)


[Our July 2024 Update](https://onlook.substack.com/p/july-2024-update)[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell) and[Kiet Ho](https://substack.com/profile/46760661-kiet-ho)


·


April 23, 2025


[Read full story](https://onlook.substack.com/p/july-2024-update)
