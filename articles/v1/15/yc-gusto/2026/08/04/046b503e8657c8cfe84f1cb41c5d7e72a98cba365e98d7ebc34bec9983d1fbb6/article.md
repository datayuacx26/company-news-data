---
schema_version: "1.0.0"
document_id: "046b503e8657c8cfe84f1cb41c5d7e72a98cba365e98d7ebc34bec9983d1fbb6"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-rss-220861d4746e"
canonical_url: "https://embedded.gusto.com/blog/react-payroll-sdk/"
published_at: "2026-08-10T20:28:18+00:00"
first_seen_at: "2026-08-11T01:08:15.268981+00:00"
fetched_at: "2026-08-11T01:08:16.149499+00:00"
content_hash: "sha256:60dd45d961b82c2cabf63b662facd8e9deb5b78a432cfaebdfa8983856b709a9"
---

# How to Build Payroll with the Gusto Embedded React SDK

*This is the second post in our series on the Gusto Embedded Payroll SDK.*[The first post](https://embedded.gusto.com/blog/embedded-payroll-sdk/) *covered why we built it. This one is for the engineers and PMs who are actively evaluating or starting an SDK build. By the end, you should know where the SDK fits relative to your other options, what it actually gives you out of the box, how to customize it, and what a realistic first few weeks of building look like.*


## Where the SDK Sits


Gusto Embedded gives you three ways to build payroll: direct API calls, our pre-built embeddable Flows, or the SDK. This isn’t a single decision, but one you can revisit for different parts of payroll as you build. We usually recommend new partners get to market with a pilot rapidly to get early signal from real users. The SDK is the best starting point in most parts of the build, allowing you to move quickly without giving up the customization that makes payroll feel native to your application. If your dev team is proficient in coding with AI, you should be able to use the SDK and our enablement materials to build a comprehensive payroll demo application—or proof of concept that looks native to your application—in a matter of days.


Within the SDK portfolio, we have a spectrum of options that allow you to choose your desired level of customization and level of investment. This was built on the insight that our partners often consider some parts of payroll to be more critical to customize than others, so optionality allows them to optimize for both speed and quality. The nature of the customization may also vary: wanting to bring your own navigation patterns, ensuring consistency in look and feel, or omitting certain data field collection or experiences entirely.


So given this, how do you decide what to use within the SDK? Where a payroll-related workflow is standard and you want to move fast, use the SDK’s pre-built components. Where a workflow touches something core to your product’s identity, like how you present cash flow or handle vertical-specific pay structures, use the SDK’s Hooks to build it yourself on top of our business logic—a headless path that abstracts the API complexity away from your code. The API is always there if you need it, but our goal is to make direct API calls unnecessary for most areas of a complete payroll build.


## What the SDK Actually Is


The


[Gusto Embedded React SDK](https://sdk.gusto.com/) (


@gusto/embedded-react-sdk


on npm) is a React component library that wraps our payroll APIs with the business logic, validation, and edge-case handling we’ve built over 15+ years. It ships three ways to consume that logic, and you can mix all three in the same app:


- **Workflows** : a single component that encapsulates an entire multi-step experience (for example,


Payroll.PayrollFlow


). Best for: areas where you’re happy with the experience and structure as-is and just want to apply your own theming.


- **Blocks** : smaller, embeddable components for a specific step or piece of UI you want to drop into your own layout. Particularly useful when you already have some of the data that the Workflow captures, or want to take parts of it piecemeal.


- **Hooks** : headless access to the underlying state and logic, with no UI at all, for when you want to build the interface entirely yourself. Useful for maximum customization.


## The Workflows You Get Out of the Box


Today the SDK ships pre-built workflows for the moments most partners need first: company onboarding, employee onboarding, contractor onboarding, and running payroll (


Payroll.PayrollFlow


and


Payroll.PayrollList


, among others). Each one already handles the state-specific tax setup, compliance requirements, and validation that make payroll hard to get right, so you’re not re-solving problems we’ve already solved. A complete inventory is


[available here](https://sdk.gusto.com/docs/guides/workflows-overview) .


## The Customization Levers, and What Each One Unlocks


There are two main customization levers we want to highlight. In rough order of how much control they hand you:


*Theming* gets you visual consistency (colors, spacing, typography) with minimal effort. It’s the first thing most partners reach for, and often the only thing they need for a workflow that doesn’t need to feel bespoke.


*The Component Adapter* lets you register your own components (a button, an input, a card) and have the SDK render those instead of its defaults. What might not be immediately clear is that this isn’t just styling: if you already have a mature design system, the Component Adapter means every payroll screen can be built from your actual components, not skinned versions of ours. That’s the difference between “themed to look like you” and “built by you.”


## React Hooks for the Most Customization


And underneath the Component Adapter and headless building sits the real unlock:


**React Hooks** . Hooks give your team optionality. Where our pre-built UI already works well, you get the acceleration. Where you want full control over the experience, you can go headless and build your own UI on top of the hook, without ever having to navigate our APIs directly. Hooks are also a pattern your React developers already know, so the learning curve is minimal.


These aspects are the major cornerstones of our SDK. To see some of our other smaller customization capabilities on the SDK as well as some useful reference for topics we often get asked about, see


[this reference section](https://sdk.gusto.com/docs/reference/#configuration) .


## Mixing SDK, Flows, and API


Remember, none of these approaches are mutually exclusive or binding. A partner might use a Flow for contractor onboarding because it’s not core to their product, an SDK workflow for company onboarding because they want it to feel native but don’t need heavy customization, and Hooks for running payroll because tip and commission handling is central to their app. You can also revisit these choices: a Flow can become a Hooks build as you learn where deeper customization pays off with your users.


## A Note on Staying Current


We follow


[semantic versioning](https://sdk.gusto.com/docs/guides/integration-guide/versioning/) on the SDK, and breaking changes are called out in the


[changelog on GitHub](https://github.com/Gusto/embedded-react-sdk/blob/main/CHANGELOG.md) . If you’re building on the SDK, it’s worth setting up a lightweight process now, even just a recurring check of the changelog before you upgrade, so integration maintenance doesn’t become a surprise later.


## Building Payroll in One Week


For most dev teams, the first day breaks down roughly like this: get sandbox credentials and authenticate against the API, install the SDK and then follow our


[Quick Start process](https://sdk.gusto.com/docs/getting-started/quick-start) to get up and running. If you’re using AI, you can even point it at our


[example application](https://sdk.gusto.com/docs/getting-started/example-app) so it has a working reference to help you stand up your own build within a couple of days. By the end of the first week, you’re already fine-tuning the payroll application to look more like your own, without getting bogged down in the setup or having to learn everything involved with payroll to get started.


## QA and the Path to Pilot


Before any partner goes live, Gusto’s Technical Solutions team reviews the integration against a standard checklist, covering things like test payroll runs across different pay schedules, webhook handling, error states, and tax edge cases. The ones who struggle tend to treat QA as a final gate rather than something to build alongside development. The partners who launch smoothly are the ones who loop in Technical Solutions early, not right before go-live, and who test with realistic (not just happy-path) data during their pilot.


## Docs, Code, and Getting in Touch


Everything above is covered in more depth in the docs:


- SDK Quickstart and guides:


[sdk.gusto.com](http://sdk.gusto.com/)
- Source and examples:


[github.com/Gusto/embedded-react-sdk](https://github.com/Gusto/embedded-react-sdk)
- Install:


npm add @gusto/embedded-react-sdk


If you hit something the SDK doesn’t cover, reach out to us directly via your partner and technical solutions contacts rather than working around it. We’re shaping this SDK based on partner feedback, and are happy to work with you to make it even more valuable.


## What’s Next


We’re also exploring an SDK-specific Dev Assistant MCP server, in the same spirit as the


[API-focused MCP we launched](https://embedded.gusto.com/blog/payroll-developer-mcp-server/) , so AI coding tools can get accurate, up-to-date context on SDK components, Hooks, and patterns directly. It’s early days, and we’ll share more once it’s ready.
