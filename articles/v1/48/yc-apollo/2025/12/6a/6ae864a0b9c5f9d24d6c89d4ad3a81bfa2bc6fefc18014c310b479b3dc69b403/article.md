---
schema_version: "1.0.0"
document_id: "6ae864a0b9c5f9d24d6c89d4ad3a81bfa2bc6fefc18014c310b479b3dc69b403"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-delivery-hero-accelerates-ux-experiments-with-server-driven-ui-and-apollo"
published_at: "2025-12-01T11:19:22+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:55:10.614706+00:00"
content_hash: "sha256:c66fab2c32c25eff61019f4bd6aa786b5856dc4c1c0690adc5c619f21743a2c6"
---

# How Delivery Hero Accelerates UX Experiments with Server-Driven UI and Apollo

## At GraphQL Summit 2025, Delivery Hero shared how a two-layer Apollo supergraph enabled faster iteration and better experiences across brands, markets, and devices.


Delivery Hero’s platform powers food and grocery delivery in more than 70 markets around the globe. When the engineering team set out to speed up UX iteration across its mobile apps, their first attempt, a fully server-driven UI, worked on paper but failed in practice. The custom SDKs were heavy, adoption was slow, and the system meant to accelerate releases became another bottleneck.


The breakthrough came when the team split the problem in two: keep business logic in one layer and UI presentation in another. That shift turned months of coordinated releases into a week of configuration work, all powered by Apollo at the core of Delivery Hero’s global supergraph.


## **The cross-platform iteration problem**


Delivery Hero operates multiple brands, including Foodpanda, Yemeksepeti, and Foodora, on a single platform powered by a global graph that processes billions of queries across its brands each year. In markets where up to 90 percent of orders come from mobile apps, long release cycles were slowing innovation.


Product managers wanted the freedom to tweak and experiment with UI without waiting for app releases. Client teams faced duplication and drift across platforms, maintaining separate layout and formatting logic that slowed launches and made consistency hard to achieve.


Behind the scenes, restaurant and menu data lived in multiple aggregator services and backends-for-frontends. Each screen re-aggregated similar data, adding complexity and slowing iteration across the stack.


The engineering cost was clear, but the strategic cost was greater: slow experimentation in markets where competitors moved faster. In retail and delivery, the ability to test and iterate across channels is not a convenience. It is a necessity.


“The goal was to move faster and let product teams run experiments without engineering having to release new apps.” – Arne Wieding, Principal Software Engineer at Delivery Hero


## **The first attempt: when server-driven goes too far**


Before adopting Apollo, Delivery Hero built *Fluid* , a complete server-driven UI platform. Clients became “dumb rendering machines,” interpreting backend configurations that controlled everything down to padding and font sizes.


Product managers could modify screens instantly, but engineers faced a heavy technical load. “It was too heavy,” said Wieding. The platform collapsed data, UI logic, and configuration into one layer. It required substantial SDKs and created new bottlenecks whenever a team needed a new component.


“Fluid taught us what we didn’t want,” Wieding explained: “a too-generic SDUI DSL with a high SDK tax.”


## **How Apollo enabled Delivery Hero’s two-layer architecture**


After retiring their REST-based SDUI system, Delivery Hero rebuilt its platform on Apollo GraphQL, designing the new architecture with Apollo Federation at its core.


“We designed everything for Federation from the beginning,” said Wieding. “We didn’t have a monograph”


With[Apollo Router](https://www.apollographql.com/docs/graphos/routing) orchestrating a single supergraph for Android, iOS, and web, the platform gained a schema-driven foundation that could scale across brands and markets while supporting faster experimentation.


The new architecture centered on two “layers” in one supergraph:


- **UI layer:** subgraphs that define UI-specific types that return render-ready data and describe appearance without computing logic.
- **Domain layer:** subgraphs that own canonical business data such as restaurants, products, discounts, and customers.


Router composes both layers using the[@requires directive](https://www.apollographql.com/docs/graphos/routing/query-planning/query-planning-best-practices#requires-directive) . The UI layer declares its dependencies on domain data, and Router fetches everything needed in about 40 milliseconds per vendor tile. “The UI layer just describes; it does not compute,” Wieding explained.


Router’s performance makes this model practical, resolving billions of queries across subgraphs while maintaining sub-50 millisecond response times, even under peak load.


## **Focus on what changes: the** ***tile*** **strategy**


Instead of making every element configurable, the team focused on what changes most: tiles, the restaurant and product cards users scroll through.


Each tile follows a shared schema connected to Delivery Hero’s design system. It combines media, vendor info, incentives, and metadata, all configurable by product managers without code or new releases.


When a client requests vendor data via a query, Apollo Router pulls the right information from across the system and returns a finished version of each tile ready for the app to render.


“We only need these four fields that is just an array,” said Oluwapelumi Olaoye, Senior Software Engineer at Delivery Hero. Previously, clients requested 80 fields and performed local computations. Now data maps directly to the design system, improving render time by about 100 milliseconds and resolving each tile in roughly 40 milliseconds.


## **From three months to one week**


The outcomes were clear and measurable:


- **Design Cycle Time (3 months → 1 week):** Coordinating design updates across restaurant, cart, and checkout pages now happens in days instead of months.
- **API Payload Size (80+ fields → 4 fields):** Streamlined, render-ready responses mapped to the design system now load 100 ms faster per view.
- **5-minute propagation:** Configuration changes sync to production apps within minutes, enabling real-time experimentation and daily iteration.
- **Adoption by choice:** Teams can still query raw domain data when needed, but most have embraced the model because it provides what they need without adding heavy frameworks or proprietary tools.


Collectively, these gains transformed experimentation from a scheduled release task into an everyday habit, accelerating innovation across Delivery Hero’s brands and markets.


## **Why Apollo Router matters in Delivery Hero’s architecture**


For Delivery Hero, Apollo Router is the piece that keeps the two-layer design fast and reliable at global scale. It composes queries across dozens of subgraphs, resolving each vendor tile in about 40 milliseconds, even under heavy load.


During peak ordering times, that consistency allows teams to keep experimenting safely. Router’s stability and composition logic ensure the graph can evolve without affecting performance, which is why Delivery Hero treats its supergraph as a core product in its platform rather than infrastructure to work around.


## **Architectural principles that scale**


Delivery Hero documented its approach in the[Supergraph SDUI Guidelines](https://docs.google.com/document/d/1I8ijaxwsquze8_-1K3hvztF-YD_lmlMIQA7esJr1Zq0/edit?usp=sharing) , a shared framework that outlines best practices for schema design, governance, and UI layering. The core idea is simple: keep logic and presentation separate, design for resilience, and give teams flexibility within a common structure.


The takeaway from Delivery Hero’s journey is that server-driven UI works when it reduces coordination overhead without replacing engineering judgment. Apollo GraphQL provides the foundation, Apollo Federation powers the supergraph, and Apollo Router delivers the orchestration performance. Together, they enable the experimentation velocity modern retail demands.


Watch how Delivery Hero’s platform team brought this two-layer Apollo GraphQL architecture to life at GraphQL Summit 2025.


Written by


Valeria Gomez


[Read more by Valeria Gomez](https://www.apollographql.com/blog/author/vgomez)
