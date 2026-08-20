---
schema_version: "1.0.0"
document_id: "7ba95e30c4080263533b0f181c166b2746aeca2dc75e5a097a28067fe73a3cd9"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/3-ways-of-versioning-your-design-system-within-supernova"
published_at: "2025-06-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:9e8e60329810da6fe0debeec91e193a08438729410039a2851921b7ca2903f55"
---

# 3 ways of versioning your design system within Supernova

As your design system grows, so does the complexity of managing its evolution. Whether you’re rolling out new tokens, shipping a major redesign, or maintaining legacy components, versioning becomes critical to keeping your work organized, accurate, and aligned across teams.


In Supernova, versioning isn’t one-size-fits-all. We’ve seen teams succeed with a variety of approaches depending on their system’s scale, release cadence, and stakeholder needs.


Here are three proven ways to version your design system in Supernova, with real-world examples and when to use each.


## **1. Capture moments in time with Supernova’s built-in versioning**


If you’re looking for a straightforward, scalable way to track your system’s evolution, Supernova’s versioning feature is your best starting point.


A[version](https://learn.supernova.io/latest/design-systems/getting-started/versioning-oIUhGTVL) in Supernova is a snapshot of your design system at a specific moment. It captures all tokens, components, and documentation exactly as they were when published. You can continue working in the Current draft — your editable design system documentation — while your published versions remain locked and accessible through the documentation version dropdown.


This approach is ideal for teams that ship regular updates and want to:


- Communicate what changed between versions
- Let developers refer back to stable iterations
- Keep the system moving forward without breaking the past


### **💡 Customer example: MDS Design System (**[MeisterTask](https://www.meistertask.com/) **)**


The team at MeisterTask uses Supernova’s native versioning to mark each meaningful release of their design system. By maintaining a clean version history and exposing the version switcher in documentation, they give consumers a clear timeline of changes and a way to refer back to previous standards when needed.


[Check out MDS design system](https://logical-swallow-violet.supernova-docs.io/latest/home-PsURHVKf)


✅ When to use this method:


- You release regular updates and want to preserve a changelog
- Your users need to access previous versions for backwards compatibility
- You want a minimal-effort, centralized versioning workflow with full traceability


## **2. Use separate design systems for major versions**


Sometimes, your system needs more than a snapshot — it needs a fresh start.


When teams go through major design overhauls (like moving from v9 to v10), it often makes sense to create an entirely new design system in Supernova. This approach gives you a clean slate, separating your legacy tokens, components, and documentation from your new design language without losing access to the past.


To make the transition seamless for your organization, Supernova’s[multi-design system navigation](https://learn.supernova.io/latest/documentation/documentation-settings/advanced-settings-ZFqrVkY7#section-multi-design-system-navigation-d4) lets you publish and display multiple design systems side by side. You can define which systems are available to your viewers in the documentation settings, making it easy for teams to switch between legacy and current versions.


This is especially helpful when you’re:


- Rebranding or changing your design language significantly
- Supporting multiple products or platforms on different timelines
- Avoiding clutter from legacy components in your current system


### **💡 Customer example: Flare + Spark Design System (**[Genesys](https://www.genesys.com/en-gb) **)**


The Genesys team manages versioning by maintaining two distinct instances of their design system: Spark, the legacy system still in use by some teams, and Flare, their newer, unified design language. By keeping both systems live in Supernova, they provide clear, versioned documentation and export paths without mixing legacy content into the current setup. This approach allows them to support migration at each team’s pace while offering a clean, focused experience for Flare users.


[Check out Flare/Spark design system](https://spark.genesys.com/latest/home-N9duape8)


This mirrors the strategy of industry giants like[IBM Carbon](https://carbondesignsystem.com/all-about-carbon/releases/#release-history) , which provides full documentation portals for each major version of its system.


✅ When to use this method:


- You’re rolling out a brand-new version with major changes
- You want clean separation between legacy and current systems
- You support long-lived products where multiple versions must coexist


## **3. Track component-level lifecycles using the Component Manager**


Not every team needs system-wide versioning — sometimes you just want to track where individual components are in their lifecycle. Supernova’s Component Manager gives you the flexibility to do just that.


By adding[custom properties to components](https://learn.supernova.io/latest/design-systems/custom-properties/component-properties-rcueZ3PG) (like “status” or “platform version”) and surfacing them in the documentation using[component overview table block](https://learn.supernova.io/latest/documentation/documentation-blocks/component-blocks-HEBNHdzt#search-d1c79e20-c506-11ee-be2e-694c8efdfb07) , you can indicate which components are in exploration, ready for design, implemented in code, or fully documented. This allows for versioning at the atomic level, giving stakeholders a clear, cross-platform picture of progress.


### **💡 Customer example: Yggdrasil Design System (**[Futurehome](http://futurehome.io/) **)**


Yggdrasil built a custom component lifecycle view using Supernova’s Component Manager. They track each component’s maturity across Figma, documentation, and implementation, making it easy to spot gaps, coordinate handoffs, and plan updates based on real progress.


[Check out Yggdrasil design system](https://steady-weasel-purple.supernova-docs.io/latest/components/overview/status-Ik4oR8DS)


✅ When to use this method:


- You manage a large system with complex platform dependencies
- You want to track adoption and maturity on a per-component basis
- You need visibility into implementation gaps and content freshness


## **Combine methods for complete coverage**


The best versioning strategy? Often, it’s a hybrid. We’ve seen many teams use:


- **Built-in versioning** for incremental releases,
- **Separate design systems** for major redesigns,
- And **component-level versioning** to monitor implementation health and platform readiness.


Supernova gives you the flexibility to adopt the strategy that matches your system’s complexity and evolve it as you grow.


⸻


Design systems aren’t static, and neither is their documentation. The more you scale, the more critical versioning becomes for transparency, trust, and usability.


Whichever strategy (or mix of strategies) you choose, Supernova gives you the tools to keep your design system up to date and your teams on the same page.


👉 And if you’re planning a system update, don’t miss our free guide:[Release your next design system update with a bang](https://www.supernova.io/campaigns/analytics/analytics-guide) 💥 — packed with practical advice, planning tips, and real launch strategies that work.
