---
schema_version: "1.0.0"
document_id: "0c60bc245b6ab0534fa148b1adc3d07c1542761db2ebbe55c204bd2c9cf0d79e"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-rss-220861d4746e"
canonical_url: "https://embedded.gusto.com/blog/embedded-payroll-sdk/"
published_at: "2026-07-08T16:27:36+00:00"
first_seen_at: "2026-08-05T20:56:11.076330+00:00"
fetched_at: "2026-08-05T20:56:12.554074+00:00"
content_hash: "sha256:8736ba3117e4356a600497148b80309dd05a0aa9894be1f7a43b67d0eae64f98"
---

# Why We Built the Gusto Embedded Payroll SDK (And What We Learned Along the Way)

## Embedded Payroll Should Not Require A ‘Payroll PhD”


Payroll is one of those things that sounds simple until you actually have to build it.


I’ve spent years watching partners come to Gusto Embedded excited about the opportunity: the stickiness, the revenue potential, and the way payroll can transform an application from a nice-to-have into something truly indispensable.


Then, reality sets in. Payroll, by definition, has to cover a


*lot* of ground to be useful. This domain includes new concepts, edge cases, compliance requirements, and data models that you don’t fully appreciate until you’re knee-deep in them. We sometimes joke internally that building payroll could turn you into a “Payroll Ph.D,” and most partners, understandably, didn’t sign up for that degree.


What’s more, our partners want to build – and their customers expect – a payroll solution that


*feels native* ; one that’s intelligently connected to other features and deeply woven into the fabric of their application. For a bank, that might mean offering their SMB customers a real-time look at their cash flow. For a vertical SaaS platform, that might mean handling complex tip and commission structures that are table stakes for the vertical it serves.


Said another way, leading partners in embedded payroll and SMBs themselves tell us they want a solution that communicates


*trust* . And they trust an application that ‘knows’ them and their business.


The data bears this out as well.


[A survey of SMB owners for the Harvard Kennedy School](https://www.hks.harvard.edu/sites/default/files/centers/mrcbg/Final_AWP_247.pdf) found that 56% of respondents said “managing multiple platforms was a challenge” and 63% saw “embedded finance and its one-stop-shop proposition as a solution to that problem.” But that only holds true if the application has customized the solution to meet the business’s needs. With “ease of use” the top-ranked reason SMBs use embedded finance services, it stands that those platforms “that leverage the vast amounts of data to pull insights on the needs of small businesses and create customized solutions will likely outperform competitors who are unable to look at the complete customer profile.”


## Speed vs Quality in Embedded Payroll


Understandably, partners want to build a great payroll solution. We all know quality takes time. For product leaders, quality means sweating the details, sticking to a vision, pushing back when other functions ask ‘does menu layout really matter?’


*And* in today’s AI-driven tech landscape, Product leaders need to move quickly. Other roadmap commitments, investment thresholds, or even just deadlines – often in time for the end-of-year switching season in payroll – drive urgency for nearly every partner.


Every product leader feels the inherent tradeoff between quality and speed. When it comes to embedded payroll, the product leaders we work with have their own frameworks and decision criteria to address this tradeoff. But it doesn’t go away.


Partners who put a high premium on a solution that feels deeply woven into the rest of their application could build their own frontend and write directly to our APIs. Partners who went the fully custom route got complete control, but that often meant longer build times and the need to learn and build for payroll edge cases, which drive risk concerns.


Partners prioritizing speed, customer feedback, and iterative learning could opt to use more of our pre-built frontend components. Our Flows package 15+ years of Gusto’s cumulative UI/UX learnings, cover key edge cases (e.g., onboarding data collection for state taxes), and offer a reasonable amount of customization across a product space that’s larger than most realize. However, partners still tell me they felt the tradeoff.


So we asked ‘Why does it have to be a tradeoff?’ Why can’t we give partners the business logic and the guardrails, let them build something that looks and feels completely their own, and help them build as quickly as they would with Flows?


That question informed our approach to our embedded payroll SDK.


## Accelerating Customization in Embedded Payroll


We didn’t get here in a straight line. A year of work has already gone into our SDKs, and we continue to invest. One iteration of the SDK offered partners maximum flexibility, but by focusing on flexibility, it wasn’t opinionated enough to dramatically accelerate a payroll build. Another iteration took the opposite approach by building tightly integrated subcomponents that abstracted away most of the complexity, but this approach left partners with too few degrees of freedom to deliver the quality they wanted.


Candid feedback from current and prospective partners drove our calibration and iteration. Working through the problem with them pushed us toward


[React Hooks](https://react.dev/reference/react/hooks) as the architectural foundation for embedded payroll customization. When we showed partners the SDK with React Hooks, something clicked. We saw real acceleration of roadmaps alongside meaningful customization coexist in the same build.


That was the moment we knew we were onto something.


Hooks give development teams optionality as they combine SDK capabilities with their own application logic; they’re familiar to anyone already working with React, and they make SDK state easier to consume. From a Product lens, Hooks simplify the build, and they


*feel* more natural in a React frontend, which means higher quality UI for less cost.


We don’t push every partner toward one approach. The SDK allows you to implement at the level you want: entire workflows, blocks you can embed, and complete customization with hooks. The SDK provides it all. We’ll share more about these build methods in a future article.


## A Payroll API For The AI Coding Era


We’re working to build the premier embedded payroll platform in the eyes of the developer. One that offers software engineers (and their agents) the agility to build a payroll solution that feels native to the user while also leaning on Gusto’s proven expertise and reliability at scale. The embedded payroll SDK is a foundational upgrade in that ongoing effort.


That’s also why we recently launched the Gusto Embedded Dev Assistant MCP server into open beta. If you’re building with AI coding tools, like Cursor, VS Code, Claude Code, or others, you can now point them directly at Gusto’s API reference and documentation through the


[Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro) . That means your AI assistant gets accurate, up-to-date context about our endpoints, schemas, and best practices instead of relying on stale training data. Ask it things like “generate a Python snippet for running payroll” or “what webhooks are available for payroll events?” and get answers grounded in our current API.


The SDK and the Dev Assistant MCP offer different tools to solve different parts of the same problem: getting a partner from zero to a working, native payroll experience as fast as possible. You can read more about the


[Developer Assistant MCP server on our blog](https://embedded.gusto.com/blog/payroll-developer-mcp-server/) , but if you want to try it now, you can connect at


https://embedded-payroll.readme.io/mcp


.


## What I’d Tell A CPO Considering This


Here’s something I genuinely believe: when you’re building your first embedded payroll product, you should get something in front of real users


*quickly* . Not your full vision. But functional and polished enough to communicate trust and test your hypotheses about product connections that make payroll valuable within your specific application.


You can refine the UI. You can tighten the positioning. But you can’t skip the step of learning what actually resonates with your users. And every week you spend perfecting the wrong thing is a week lost.


The SDK aims to help you move through that first phase faster than anything else available today while also setting you up for smart interaction and refinement. For most partners, it offers you and your users a payroll product that feels at home in your application and, ideally, highlights why embedded payroll can save them time. We’re one of the first embedded payroll providers to offer this kind of build option: one that gives you the business logic and guardrails of a pre-built solution without locking you into someone else’s design decisions. And when you’re ready to make it feel truly native, the flexibility is already there. Using our SDKs, you’ve already done the work to build a payroll product your customers will love.


## What’s Next For The Embedded Payroll SDK


We built this through direct collaboration with partners, and we’ll keep building it that way. If you’re evaluating embedded payroll and don’t see something you need, reach out. We’d happily have that conversation early.


If you’re ready to dive in and start building with the Gusto Embedded Payroll SDK,


[check out our dedicated SDK docs site with quickstart guides](http://sdk.gusto.com/) . And take a look at the next post in the series, where we’ll get into the specifics of


*how* to implement the SDK.
