---
schema_version: "1.0.0"
document_id: "1d7f81662254ae19a87e34d3c4a2262e87d5a00ce9075ff2a9ebc4138eea262a"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/druids-the-design-system-that-powers-datadog/"
published_at: "2022-09-29T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:e3afa8d27d305dae304ced8f8770837f816e089aa4430191c64eadddb892c956"
---

# DRUIDS, the design system that powers Datadog

Derek Howles


Cecilia Watt


In 2018, Datadog was in the midst of rapid expansion. We had recently released[APM](https://www.datadoghq.com/product/apm/) to complement our core[infrastructure monitoring](https://www.datadoghq.com/product/infrastructure-monitoring/) product and, with the addition of[Log Management](https://www.datadoghq.com/product/log-management/) and the development of[Synthetic](https://www.datadoghq.com/product/synthetic-monitoring/) and[Real User Monitoring](https://www.datadoghq.com/product/real-user-monitoring/) , we were becoming a unified data platform.


For an enterprise software platform to be successful, the whole has to be greater than the sum of its parts. In Datadog’s case, it means users must be able to connect different types of data, pivot seamlessly from one context to another, and follow the thread of an investigation wherever it might lead.


Dependable and repeatable UX patterns are key to this. If something works one way in one part of the platform — say, filtering a list of data, scoping to a range of time, or inspecting a graph — users expect it to work the same way elsewhere. To ensure our users would have a familiar experience across Datadog as we expanded to dozens of products — and to ensure these experiences would be efficient for us to produce — we started building a design system in 2018.


We call it DRUIDS, which stands for “Datadog Reusable User Interface Design System.” (Is the name redundant? Yes! Was the acronym — and the accompanying logo — too awesome to pass up? Also, yes!)


Although DRUIDS is not open source, we recently made the docs site public at[druids.datadoghq.com](https://druids.datadoghq.com/) . Just a handful of enterprise software companies have documented their UX patterns publicly, and we wanted to share ours.


This is just the first post in a series. Subsequent ones will explore how we use DRUIDS to make workflows simple, powerful, and flexible for our users — and how the technical decisions on the engineering and product design sides have allowed us to scale while maintaining that focus.


But unless the hundreds of product designers and front-end developers across Datadog trust the design system and actually use it to build our interconnected platform, none of this means much. For that to happen, DRUIDS has to be **easy to understand** , **easy to implement** , and **easy to contribute back to** .


That’s where we want to start.


## Easy to understand


### Cmd+K


We use the quick nav pattern throughout the[Datadog platform](https://www.datadoghq.com/blog/datadog-quick-nav-menu) to let users quickly jump between views, and we’ve implemented it in the DRUIDS docs as well. Anyone looking for something in DRUIDS — a component, value, pattern, icon, logo, you name it — can press` Cmd+K` (` Ctrl+K` on Linux/Windows) and see all matching results, prioritized by relevance. It’s become the only way many Datadog designers and front-end developers navigate the docs. (In a meta twist, the underlying menu itself is a[DRUIDS component](https://druids.datadoghq.com/components/dialogs/SearchModal) . 🤯)


### DRUIDS Loupe


The best documentation isn’t just comprehensive; it’s also contextual. For new designers and developers still familiarizing themselves with DRUIDS and the Datadog platform, the ability to easily discover components and get a feel for how they’re used together is important. To that end, we built the “DRUIDS Loupe.” Datadog employees can press a keyboard shortcut on any Datadog page to see every DRUIDS component. Hovering discloses links to view it in source code, design tools, and docs.


### Links, links, links


To take the idea of inspection a step further, we provide links to source code whenever you view details of a DRUIDS component. For internal users, the[details page](https://druids.datadoghq.com/components/form/Button) for each component has three links: GitHub, Figma, and VS Code (the IDE of choice for most Datadog front-end developers).


This approach cuts the other way, too. We use[JSDoc](https://jsdoc.app/) comments to provide in-context descriptions and links to the DRUIDS docs from VS Code. We also link to components from[Figma](https://www.figma.com/) , our primary design tool. One of the core UX tenets of the Datadog platform is minimizing dead ends for our users: Additional context should always be close at hand. That’s a lesson we carry through to DRUIDS by making sure more context is just a click away any time a DRUIDS component is encountered.


## Easy to implement


### Editable playgrounds


Our design tool, Figma, is super powerful and easy to use. And while we do our best to robustly represent our design system with it, it’s impossible to capture all the states and permutations of[more complex UI components](https://druids.datadoghq.com/components/table/table) .


Because of that — and because what’s defined in code is what a user actually interacts with in the browser — we treat the underlying React, TypeScript, and CSS code as the source of truth for how components can look and function. To support this, we’ve built editable examples, or “playgrounds,” into the DRUIDS docs. They allow developers and designers to quickly check the different properties, or “props,” available to them and even click to copy production code.


We also have a[Code Sandbox](https://druids.datadoghq.com/code-sandbox) page, where arbitrary components can be combined into a live preview, complete with stateful URLs (in our internal version) to make sharing concepts or reproducing bug reports super easy.


As an added bonus, these interactive features are especially helpful for teams building internal apps without dedicated front-end developers or designers. When someone who spends most of their day in Go or Python needs to create a site to manage their microservice, they can use familiar patterns from the Datadog UI and generate production-ready code with little overhead.


### Auto-generated API tables


While thelive playgrounds are helpful in many cases, sometimes designers and developers — especially developers — want to skip to the nuts and bolts of a given component. In a design system, the nuts and bolts are the component’s API: its list of props and their allowable values.


One of DRUIDS’ guidelines is that components must include thoughtfully named and meaningfully described props to maximize predictability (more on thisbelow ). But with 150+ components, it’s easy for the actual props to drift from their documentation. Enter auto-generation.


Each DRUIDS component has an auto-generated API table that pulls the list of props, their values, and their descriptions directly from the component’s source code. This way, we ensure a single source of truth for every component, which developers and designers know they can always rely on.


## Easy to contribute back to


### Contribution guidelines


We believe that DRUIDS should constantly evolve as the needs of Datadog and our customers evolve. To do that, it’s important that contributions back to the design system by developers and designers throughout the company have a clear set of expectations while simultaneously feeling lightweight.


We publish a set of[contribution guidelines](https://druids.datadoghq.com/foundations/about#contribution-guidelines) , which we ask internal contributors to follow. They’re mostly common sense, and the goal isn’t to be intimidating or inefficient. They’re a way to reinforce best practices from the ground up. We break them into a handful of categories: *Core considerations* , *Structure and formatting* , *Prop naming* , *Documentation* , *Styling* , *Responsiveness* , *Accessibility* , and *Testing* . For example:


### CLI tooling


Tooling is the final piece in a smooth road for contributions. All DRUIDS components follow a certain structure — in terms of their code and how their files, like unit tests and documentation examples, are organized. So that internal contributors can remain focused on design, UX, and performance — not on boilerplate — we built a command line wizard. A developer can run a single terminal command to generate all the necessary scaffolding for any new component. For example,` yarn component button` :


## What’s next?


Next up in our series: a deep dive into one of Datadog’s most widespread and unique components: the[DateRangePicker](https://druids.datadoghq.com/components/time/DateRangePicker) . We’ll show how DRUIDS makes it super easy for our customers to use across the platform — and easy for our developers and designers to work with.


Last, but not least, special shoutout to everyone else on the design systems team who work so hard to build DRUIDS: designers Won Choi, Dave Epstein, Jenil Gogari, and Jenn Lebor; and front-end engineers Arnaud Crowther, Benjamin Koltes, Roman Komarov, Vincent Volckaert, and Thibaud Vouillon.


Interested in using DRUIDS to build world-class user experiences? We’re[hiring](https://careers.datadoghq.com/engineering/) !


-
-
-
