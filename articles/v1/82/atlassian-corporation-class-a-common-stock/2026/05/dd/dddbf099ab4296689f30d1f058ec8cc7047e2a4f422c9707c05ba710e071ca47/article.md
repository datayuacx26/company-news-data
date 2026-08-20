---
schema_version: "1.0.0"
document_id: "dddbf099ab4296689f30d1f058ec8cc7047e2a4f422c9707c05ba710e071ca47"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/ai-at-work/atlassian-design-system-building-the-context-engine-for-the-ai-era"
published_at: "2026-05-28T16:09:04+00:00"
first_seen_at: "2026-07-24T17:38:21.036095+00:00"
fetched_at: "2026-07-28T22:36:36.514477+00:00"
content_hash: "sha256:713ec3aa7de949a915cc3e1a879af77c001887e121c7ebb7aa2c90479f9a0b96"
---

# Atlassian Design System: Building the context engine for the AI era

##### Design systems are no longer ‘just’ about consistent UI and component libraries. They’re strategic capabilities that harness design intent, drive coherence and accelerate a team’s ability to innovate. Here’s how the Atlassian Design System (ADS) is evolving to be a full stack context engine to meet the demands of an AI-native future.


**Maria Christley** is the Head of Design for the Atlassian Design System, leading over 35 designers globally across Design Language, Accessibility, Systems Architecture, and AI. She is a 2025 Women Leading Tech finalist and has spoken at Figma Config and UX Australia.


**Rachel Radford** is a Design Manager on the Atlassian Design System team, where she leads designers working on the systems, components, and practices that power Atlassian’s products at scale. She is a founding leader of Friends of Figma Wellington, New Zealand.


Design Systems with Figma – Sydney


---


## The value of a design system has never been higher


Whether it’s 2016 or 2026, certain truths about working in design persist. Seeking buy-in is hard. Brand coherence is always at risk. Knowing which design decisions have been made, and how to apply them correctly remains elusive at scale.


These aren’t new problems. But the expectations around them are accelerating. Competitive landscapes are shifting faster, and customers notice the gap between products that feel considered and those that feel assembled. Design systems have a leading role to play in how companies close that gap, and we believe the value of the design system has never been higher.


Today, design systems play a leading role in:


- **Front-end infra**
Components are deeply connected to the codebase and engineering practices. They are part of the performance and reliability story, not just the visual layer. This layer is now more agentic than ever in how it show up to designers and developers via the tools they use, ultimately making it easier and faster to produce high quality artefacts.
- **Fluid collaboration, not linear handoffs.** The SDLC workflows are being replaced by fluid, shared workflows where the system becomes the connective tissue between design, product, and engineering—and now, AI agents. All product builders are collectively supported exponentially increasing who the customers of a design system is.
- **Accessibility as culture, not checklist**
Accessibility is treated as a baseline expectation of good design, not a QA step at the end. A healthy and AI native design system encodes accessibility into the context layer via structured content files, agentic skills and as part of AI tooling.
- **Brand expressed as an opinionated and crafted design language**
Tokens, grids, and primitives aren’t endpoints or isolated parts; they’re the DNA of a very deliberate brand expression that customers experience across all app, every day. Design systems of today are the strategic custodian’s of that brand expression and how it is encoded.
- **From isolated components to extensible compositions**
The real value is in how pieces come together: patterns, layouts, and compositions that empower creativity while maintaining coherence. We define and document this intent into the context layer that AI tooling can understand and accurately translate.


atlassian.design combines Atlassian’s design intent, build in public narrative and the design system that powers the context engine.


**Here’s how we’re evolving the system across three areas:**


1. **Strengthening our intelligent source of truth**
2. **Building systems for flexible evolution via Panel**
3. **Defining the infra stack of an AI-native design system**


## Strengthening our intelligent source of truth


Our foundations span color, typography, spacing, iconography, accessibility and more. Each one passes through a quality bar before any team in the company or our marketplace can adopt it. We treat the design system like a product, with a roadmap, priorities, feedback loops and have it prominently featured in company level goals.A snapshot of the Design Foundations we have invested in for humans and AI.


A snapshot of the Design Foundations we have invested in for humans and AI.


These foundations are a critical part of our our AI Native System. Not only are they robust, scalable, and enterprise-grade quality but they are built with intentional structure to enable AI to easily consume and generate with it. Since our talk we have conducted evals to confidently report on this impact.


- 52% accuracy improvement in AI calls
- 34% faster on average across ADS specific tasks
- 26% reduction in AI tooling calls, with 16% reduction in AI token usage


This makes business ‘cents’ and has a positive impact on the way we build experiences our customers love.


The Atlassian Design System continues to build the context and maturity needed to scale AI tooling and over the last 12 months has shipped over 58 updates to thousands of product builders.


### Accessibility at the core of good design


One example is our date time picker. The previous version confused screen readers by auto-opening a calendar on focus. The component had been optimised for sighted, mouse-based interaction – a design bias that created friction for everyone else.


Our redesigned version reduced keyboard inputs, introduced semantic structure with proper labels, and adjusted border colours and focus rings to achieve at least 3:1 contrast across themes. The result works better for people using assistive technologies, and it works even better for everyone else too.


[Read ‘behind the screens’ on accessibility.](https://www.atlassian.com/blog/design/behind-the-screens-accessible-design-is-good-design)


### Iconography with deep intent and purpose


Our icon system uses a 1.5-pixel stroke that matches the 1.5-pixel stroke in Atlassian Sans, our custom typeface. When you squint, the result is beautifully uniform type and icons that share the same visual weight, which aids wayfinding and familiarity across apps. To support this at scale, we built an icon contribution plugin used by over 550 designers globally, which guides contributors through the process, including whether a new icon is needed at all.


[Read ‘behind the screens’ on iconography.](https://atlassian.design/whats-new/building-atlassians-new-icon-system)


## Evolving our next wave of foundations


Everything we publish on atlassian.design is our commitment to build in public and share as much as possible with the community. At Figma’s design system event and at Team’26 we gave a sneak peek into what we are shipping next across a range of exciting topics – Motion System, Labelling System and Panel.


### Motion System


We’re currently developing a motion system that applies the same semantic rigour as our other foundations. Our token structure spans five groups across slide, fade, scale, rotate, and content so that both humans and AI can understand the intent behind each animation. We prototyped the entire system through an interactive UX motion guideline site, vibe coded to communicate the guardrails before the tooling is fully in place. Not only this but we are setting the expression and tone for how motion feels by applying it to distinctly Atlassian experiences like Rovo, Confluence Remix and our Spotlights and ensuring we democratise motion via motion tokens and motion baked into ADS components as well as leveraging AI skills and AI tooling integration for seamless usage.


**Labelling System**


We’ve also released a new labelling system to beta, overhauling lozenges, tags, badges, and labels to align with our design language. The previous state was years of accumulated design and tech debt – hacks, customisations, and siloed solutions that had diverged across apps. The new system introduces clear visual hierarchy, native interactivity, semantic, accessible defaults. As well as this it integrates and supports


Next up we take a deep dive into Panel from the lens of building systems for flexible evolution.


## Building systems for flexible evolution via Panel


Scaling a design system across 20 apps means the central team cannot be involved in every feature or product decision. We needed a model that provides structure where it matters and freedom where teams need it.


> *Constraints foster creativity. These design concepts were created to illustrate what you could do if you were only using the Atlassian Design System – cohesive design that still feels unique, not cookie-cutter sameness.*


### A three-tier composition model


We developed a three-tier model that lets product builders compose quickly without reinventing the basics:


- **Core (Atlassian Design System):** Tokens, components, and guidelines used everywhere. High quality, accessible, and performant.
- **Platform:** Cross-product components and patterns used in multiple apps and contexts.
- **App:** Domain-specific components used in a single product, deeply aware of their contextual situation.


Adoption flows down from the core into platform and app layers, but innovation also flows upward. App-level patterns that prove their value can graduate into higher layers where they become useful across different contexts.How design decisions at different levels flow between each other.


How design decisions at different levels flow between each other.


### Composition in practice: the panel component


Our new panel component demonstrates this approach in action. The page layout shell is an intentional constraint set by the design system, ensuring overall layout behaviour stays consistent and responsive. The outer panel component is built with primitives and tokens so teams can scaffold their content without breaking accessibility or visual rhythm.


Sub-header layouts vary by context: a configuration screen needs a different header than a work-item detail view. We collaborated with teams across the company to systemise these patterns while catering for the variety of use cases.


The body of the panel is where freedom lives. Using slots, we provide purposeful regions within the panel so that a designer on the Teams app can make their panel suitable for varied rich content, while an admin-config panel can be a simple form flow. Both are coherent and on-brand because they are composed from the same foundations.


### From layers to a web of decisions


As we have progressed our use of this model, the neat three-tier model evolved. In practice, the system behaves more like a constellation – a web of interconnected systems where designers, engineers, and AI can approach it as a set of decision trees rather than a rigid hierarchy. A designer in Jira might use the Jira UI design kit, dip into platform-level patterns, or pull directly from the core design system, arriving at the combination that best fits their UI.


Through these organic workflows, facilitated increasingly by AI tooling, adoption becomes easier, construction is faster, and product builders are better supported.


## Defining the infra stack of an AI-native design system


For the past few months, we’ve been exploring what it means for a design system to be AI-native. For us, this means four things:


1. **AI can understand it.** Strong semantics allow AI to read and reason about the system’s structure, tokens, and intent.
2. **AI can build with it.** Structured content – context files that guide decision-making – enables AI to compose interfaces using our system correctly.
3. **AI patterns are part of the system.** We’ve introduced Rovo and AI-specific patterns into the design system, clarifying which patterns should be used across Atlassian.
4. **AI maintains system health.** AI handles burdens that slow teams down—migration tooling, testing, content updates—freeing designers and engineers for higher-order work.


### The expanded design system infra stack


Until recently, our design system stack was the familiar set of foundations, tokens, and components. Now, we’re building the context layer on top of it and expanding the range of tooling it can support. This includes structured content files that guide AI in better quality decision-making using fewer AI tokens, our Atlassian Design System MCP server, unified templates for code generation, building ADS skills and other markdown files for portability. This infrastructure enables us to plug into AI agents like Rovo, AI prototyping tools like Figma Make and Replit, and even to update content on our atlassian.design website.


> *To identify the rules that help LLMs, you also uncover the rules that help explain these concepts to humans—and that’s a good thing.*


The discipline of making a design system legible to AI forces clarity about what the system actually means: its constraints, its flexibilities, its intent. That clarity doesn’t just serve AI. It serves every designer, engineer, and product builder who needs to make confident decisions with the system.


## The design system’s leading role


The Atlassian Design System has been on a 15-year journey: from static style guides to coded systems, to powering scale and coherence across a branded house of over 20 apps. Over the past few years, we began augmenting with AI via migration tooling and automated testing but now we’re building toward a system where AI is a core collaborator with strong semantics it can use to understand and build with our design language.


Companies that have deeply invested in their design systems are able to integrate with AI tooling far more quickly than those that haven’t. **The strategic value of design systems in driving brand coherence, empowering creative execution, and enabling confident decision-making has never been higher.**


Design clarifies intent. And when you build a system that can communicate that intent to both humans and AI, you don’t just make products more consistent. You make the entire act of building them faster, more creative, and more inclusive.


## Learn More:


[How we design at Atlassian by checking out Atlassian.Design.](https://atlassian.design/)


Watch Charlie Sutton, Chief Design Officer, speak at Team26 on ‘Designing for the beautiful mess of modern work’. This talk further highlights how we are intentionally designing the systems that shift all product builders towards clarity and how we are building this system in a way AI can consume.
