---
schema_version: "1.0.0"
document_id: "530fc6b37e37c9896b4940b595cc0eb5c22004fa3caa310bb37ed2560da7b824"
company_key: "figma-inc-class-a-common-stock"
company: "Figma Inc."
source_id: "figma-inc-class-a-common-stock-rss-45373d72e79e"
canonical_url: "https://www.figma.com/blog/supercharge-your-design-system-with-slots/"
published_at: "2026-03-05T05:00:00+00:00"
first_seen_at: "2026-07-25T04:33:00.636596+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:83b7160c1ca59b86b59b621be68a86cb2dfbe7577a773ea5272fbd73c8a65dbe"
---

# How to supercharge your design system with slots

Design systems promise consistency at scale. But as they grow, the guardrails meant to protect the brand can begin to limit expression, so teams find workarounds to bring back individuality. Libraries swell. Variants multiply. Instances get detached. What starts as a system for cohesion becomes a system people circumvent.


Designer Advocate, Brett McMillin, shows you how to get hands-on with our slots[playground file](https://www.figma.com/community/file/1610367877471727305) .


That’s where[slots](https://www.figma.com/community/file/1610367877471727305) come in. They make design systems more flexible and easier to scale. With slots, designers can customize components, like menu options, buttons, or icons without detaching, so content can flex without breaking structure. This mirrors how components are built in code. Developers dynamically inject content and compose within a stable structure rather than duplicating it. Bringing that model into design makes components behave more like they do in production—simplifying implementation, clarifying handoff, and making systems easier for automation and AI to interpret.


Here’s what this unlocks across teams:


- **For design system managers:** fewer variants, less maintenance, and stronger alignment to production
- **For designers:** more creative freedom inside the design system—not outside it
- **For developers:** predictable structures that map directly to code


We introduced slots at[Schema 2025](https://www.figma.com/blog/schema-2025-design-systems-recap/) and it's now available in open beta.


Since we announced slots at Schema, we've been collaborating with early testers to understand how slots can improve their workflows. In this guide, we share five field-tested tips gleaned from building and evolving slots with our users—so you can be more expressive without compromising the integrity of your design system.


## 1. Start with the most-used components for the biggest immediate payoff


###### Field note:


Start with components people detach the most—that’s where slots will deliver immediate value.


Early users consistently saw the biggest wins by starting where teams were already working around the design system. For many teams, that meant focusing on foundational components like dialogs, menus, modals, cards, and panels. Across those teams, two patterns showed up again and again.


First, teams used slots to customize components with repeating elements. **** Menus and lists often contain hidden layers to account for every possible item count. You might ship a list with three visible rows but have seven more hidden just in case. And when someone needs an eleventh item? They detach. With slots, authors can include only what’s needed. Designers can add more items as content grows without bloating the base component or breaking the connection.


Dropdown menus were one of the most common use cases early testers applied slots to.


Second, slots helped manage components with many configurations. Modals and cards are classic variant magnets. Titles, descriptions, media, buttons—every possible combination turns into another property (prop) or variant. Slots let you keep the structure intact while swapping or inserting the content you need, dramatically reducing variant divergence.


Slots create the most impact in places where structure stays consistent but content changes frequently. Instead of rolling them out across your entire system, start by focusing on a few high-traffic components. You should prioritize components that:


- Appear frequently across screens
- Are duplicated across your system
- Have accumulated excessive variants
- Need to support different types of content


From filter lists to dashboards to music players—same structure, different content.


## 2. Use pre-filled slots to clarify next steps


###### Field note:


Default content adds context. Empty slots signal action. Use both intentionally.


When authoring a slot, decide whether it should include default content. This gives designers context and often requires no edits at all. For example, if an icon is always in the top right corner of a card, why make people insert it every time? In early testing, this was the most common setup—especially for components where content is predictable but occasionally changes.


On the other hand, an empty slot makes it explicit that designers need to add something. This mirrors many current system patterns, especially where teams have been simulating similar behavior with instance swap. It’s a clear signal that content is required.


There’s no single right answer. Some systems will default to filled slots. Others will prefer empty ones. Many will use both depending on the component. Choose the setup that matches how your team actually works.


Once that decision is clear, creating a slot is straightforward:


- **Start with a parent component:** Slots can only live inside a main component, not directly on the canvas. Start by creating or opening the component you want to make more flexible.
- **Insert a frame:** Put a frame in the parent component. Slots are built from frames that are morphed into a special layer. You can’t convert a rectangle on the canvas into a slot.
- **Convert the frame into a slot:** Right-click and convert the frame into a slot, or use the shortcut (Shift + Command + S). That’s it. Now you’re ready to configure it.


A slot that is empty (left) and a slot with default content (right). Both work!


## 3. Make it clear what belongs in each slot


###### Field note:


Preferred instances add guidance to open slots—use them by default.


The most successful users paired slots with clear signals about what belongs inside them. Without direction, a slot can feel ambiguous—slowing teams down and leading to inconsistent usage. That’s where preferred instances come in. Think of preferred instances as guardrails, not constraints. They guide usage while preserving the flexibility that makes slots powerful in the first place. If people have to ask what goes in a modal every time, the slot needs guidance.


When authoring a component, you can recommend specific component instances for a slot from the props panel, just like instance swap today. These appear by default when designers click the “+” button inside the slot, giving them a curated set of options to choose from. For most systems, preferred instances should be the default approach because they bring:


- **Faster workflows:** Designers choose from a short, curated set instead of searching the entire library.
- **Stronger consistency:** Approved components get used in predictable ways, building cohesion across design outputs.
- **Easier handoff:** Developers see familiar components in predictable places, reducing questions like, “Is this custom?”


Set up preferred instances so designers know exactly what belongs there without having to guess.


## 4. Think beyond individual components


###### Field note:


If you’re duplicating layouts to handle variation, that layout probably needs slots.


Slots aren’t just for buttons and menus. Early testers got the most leverage when they scaled up slots for larger components, too—like page sections and layout components that define headers, sidebars, content regions, and footers. The overall layout stays intact, while each predefined region remains flexible and swappable. Maybe your product page has a hero region that holds a video, multiple static images, or a combination of both depending on the needs of a launch. Instead of creating separate layout components for every variation or detaching to make structural edits within a page region, you can define a single layout and let slots handle the differences in content. The result is reusable page frameworks that scale across product surfaces without multiplying components. Slots work at every level of your design system, from a single element to larger layout components. The principle stays the same: Keep the structure consistent and the content flexible.


## 5. Align design and code with slots


###### Field note:


When design structure mirrors production structure, handoff gets faster—and ambiguity disappears.


Early testers consistently pointed to structural alignment as one of the biggest advantages of slots. The more closely a component mirrored production, the smoother everything downstream became. In code, container components—like dialogs, menus, and cards—are built to accept content within defined regions. Slots bring that same pattern into design, making it clearer what content belongs where and how components are meant to be used. Instead of approximating flexibility with variants and detaches, designers work with components the way engineers actually build them.


That shared structure reduces interpretation. Engineers can clearly see what content is intended for each region. Designers can build layouts that reflect how components function in production. The gap between mock and implementation narrows. It makes systems easier for automation and AI agents to reason about—because structure is explicit rather than implied.


Learn more about implementing[Code Connect](https://github.com/figma/code-connect) in your design system and stay tuned for upcoming MCP support.


Slots are also supported in[Code Connect](https://www.figma.com/blog/introducing-code-connect/)


[The right code for your design system Today, we’re announcing beta for Code Connect, a feature built to improve design system adoption by making code more accessible and useful for developers.](https://www.figma.com/blog/introducing-code-connect/)


, where they appear directly in code snippets. Engineers can immediately see how a slot-based component maps to production, making handoff more predictable and implementation faster. When structure stays consistent and content stays flexible, teams move with less friction—especially in container components where variation is expected.


To get the most out of this alignment, define slot regions intentionally and mirror how your components are structured in code. The closer those models match, the fewer translation layers your team has to manage.


Slots appear directly in Code Connect snippets, helping engineers see how components map to production and making handoff easier.


[Learn more](https://help.figma.com/hc/en-us/articles/38231200344599) about how slots work—from the smallest elements to entire frameworks. The possibilities are wide open. Where will slots make the biggest impact in your system? The components everyone detaches? The buttons that need flexible icons? The full-page layouts your team starts from scratch every time? We can’t wait to hear where slots fit in your design system.


Zoë is a Product Manager at Figma building features that power modern design systems. Previously, she shaped design system practices at LaunchDarkly and InVision.


[LinkedIn](https://www.linkedin.com/in/zoeade/)


Sarah Kelly is a Product Marketer at Figma, where she works on launching new products and features.


[LinkedIn](https://www.linkedin.com/in/sarahbellkelly/)
