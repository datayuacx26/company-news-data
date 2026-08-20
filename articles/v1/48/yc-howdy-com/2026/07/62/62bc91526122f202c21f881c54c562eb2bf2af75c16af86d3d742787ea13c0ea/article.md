---
schema_version: "1.0.0"
document_id: "62bc91526122f202c21f881c54c562eb2bf2af75c16af86d3d742787ea13c0ea"
company_key: "yc-howdy-com"
company: "Howdy.com"
source_id: "yc-howdy-com-news-import-7de6c48bfb23"
canonical_url: "https://www.howdylatam.com/blog/frontend-development-stopped-being-about-css"
published_at: "2026-07-30T03:00:00+00:00"
first_seen_at: "2026-07-31T23:44:21.659856+00:00"
fetched_at: "2026-07-31T23:44:22.904459+00:00"
content_hash: "sha256:ac648e8b3ea79cdf0ef5d232d88ed14d1719c849cd23d6a638a6e5f30d3c32b7"
---

# Frontend development stopped being about CSS a long time ago

You're in a planning meeting and the PM says the usual line: "the backend is the hard part, we'll throw the frontend together quickly at the end." Nobody pushes back. The tech lead nods. And right there, if you've spent more than five years building real interfaces, something tightens in your stomach, because you know exactly how that story ends: with a global state nobody fully understands, with a rendering decision made without much thought that's now brutally expensive to reverse, with a UI that collapses under its own weight three sprints later. Frontend development stopped being about arranging boxes and picking a color palette a long time ago, and yet we keep planning as if it were still 2014.


## **Frontend development was never about layout, even though we still treat it that way**


There's an idea baked into the process, almost never said out loud but present in every sprint estimate: the frontend executes, the backend designs. One thinks, the other builds what it's told to build. That idea worked when a page was HTML with a bit of jQuery on top. It stops working when your application has to sync state between the server, the client cache, the browser history, and three data sources that sometimes disagree with each other.


When a team treats the frontend as the finishing step, what it's actually doing is postponing architecture decisions until there's no room left to make them well. The API got designed around the data model, not around how it would actually be consumed on screen. The contract between client and server got negotiated without anyone thinking through what happens when the user loses connection mid-action. And by the time the frontend gets "thrown together quickly at the end," it's already inherited every problem nobody wanted to solve earlier.


This isn't a turf complaint. It's a technical observation: most product bugs that end up as a support ticket don't come from broken business logic, they come from a frontend that had to improvise on top of a foundation that never accounted for it.


## **State architecture is the real work, not margin tweaking**


If you had to explain to someone from another discipline what a senior frontend engineer actually does day to day, the honest answer has nothing to do with CSS. It has to do with deciding what counts as the source of truth and what's just a derived copy. What lives on the server, what lives on the client, what gets cached, when that cache gets invalidated, and what happens when two tabs from the same user fall out of sync.


React Query, Zustand, Redux, signals, whatever your stack uses, these are tools for solving that problem, not the problem itself. Using Redux for absolutely everything is just as wrong as having no state strategy at all and letting each component fight for its own version of the truth. The question that separates well thought out frontend development from something that collapses in production isn't which library you picked. It's whether you understood, before writing the first line, what needs to be synced in real time, what can live a few seconds out of date, and what shouldn't be on the client at all.


This shows up most in teams that grow fast. State that was manageable across four screens turns into a tangled mess across forty, and that's where a frontend that scaled parts ways from one that starts needing a rewrite every six months.


## **SSR, CSR, or streaming: the question isn't which is better, it's what you're willing to give up**


Server-side rendering, client-side rendering, streaming with React Server Components. Every technical talk presents this like there's a correct answer waiting to be discovered. There isn't. There are trade-offs, and choosing without understanding them is where scalability problems start, the kind nobody can quite explain later.


SSR gives you a fast first render and better indexing, but it shifts compute cost to your server and complicates caching when the content is personalized. CSR simplifies your infrastructure and works well for highly interactive applications, but it hurts time to visible content, especially on slow connections. Streaming promises the best of both worlds, progressive content, selective hydration, but it adds a layer of complexity that a team without prior experience will consistently underestimate.


The right call depends on how personalized your content is, how critical SEO is for that specific product, and how much engineering capacity the team has to maintain whatever solution gets picked. A team that copies the rendering architecture of a company with a hundred dedicated platform engineers, without having that same capacity, isn't making a technical decision. It's making an internal marketing decision.


## **Real performance is measured on a user's mid-range phone, not on your laptop**


A green Lighthouse score means nothing if your actual user, on a shaky 4G connection with a two year old phone, is staring at a blank screen for eight seconds. The gap between lab performance and real performance is exactly the gap between measuring under ideal conditions and measuring what actually happens to the people using your product.


Core Web Vitals matter because Google uses them for ranking, sure, but they matter more because they're reasonably good proxies for real frustration: how long it takes for something to show up, how much the screen shifts while it loads, how long it takes to respond to an interaction. Teams that only look at synthetic metrics, generated in a controlled environment, end up optimizing for a user who doesn't exist.


Measuring with real user data, not just automated runs, changes your priorities. Sometimes the bottleneck isn't the JavaScript bundle, it's an unoptimized image in the homepage hero. Sometimes it's not rendering at all, it's a third-party API blocking the main thread for seconds at a time. Without that visibility, a frontend team ends up optimizing whatever's easy to measure, not what actually matters.


## **Every frontend decision is a product decision, even if nobody says so in the meeting**


Using a skeleton screen instead of a spinner isn't a visual detail, it's a bet on how an anxious user perceives time. Deciding whether an action shows as successful before the server confirms it (optimistic updates) or whether you wait for the real response directly changes how many people complete a checkout flow. A poorly designed error boundary can turn a minor failure into a blank screen that makes someone abandon the app for good.


These aren't aesthetic choices. They're product decisions with a direct impact on conversion, on retention, on how many people get frustrated enough to never come back. The problem is they're almost never discussed in those terms. They get treated as implementation, something frontend "handles" after product has already defined what matters. When in reality, each one of these decisions shapes the experience just as much as any new feature on the roadmap.


## **Treating frontend development as the step that comes after backend is why the UI doesn't scale**


Here's the pattern that repeats company after company: the frontend grows in fits and starts because it never had an architecture thought out ahead of time, it just kept absorbing features until every new change breaks three old ones. Nobody planned the state architecture. Nobody made a deliberate call on the rendering strategy. Nobody measured performance with real data until the complaints started rolling in. All of it got pushed off because, in the team's head, the frontend was the part you throw together quickly at the end.


And when it finally blows up, the proposed fix is almost always a full rewrite. Not because the frontend was impossible to fix, but because it was never treated as a system with its own architecture, with decisions that deserved the same level of discussion as any backend design choice. Technical debt piled up in a place nobody was watching closely enough.


## **The hierarchy is wrong, not the technology**


The underlying problem isn't technical. It's that we keep organizing teams and conversations as if the frontend were a later step, when it's actually where you decide whether a product feels fast, reliable, and coherent, or whether it feels broken even when the backend responds perfectly. A senior frontend engineer isn't someone who's better than anyone else at CSS selectors. It's someone who understands that every line of client-side code is a bet on how an entire distributed system behaves, with all its network failures, its intermediate states, and its real users on real hardware. As long as companies keep assigning that level of responsibility to whoever "makes it look nice," they'll keep paying, in production, for a hierarchy that never made sense.
