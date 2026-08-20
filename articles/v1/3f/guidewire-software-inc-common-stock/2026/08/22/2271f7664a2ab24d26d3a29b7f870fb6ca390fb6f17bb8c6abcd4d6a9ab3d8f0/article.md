---
schema_version: "1.0.0"
document_id: "2271f7664a2ab24d26d3a29b7f870fb6ca390fb6f17bb8c6abcd4d6a9ab3d8f0"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/accelerating-p-c-delivery-with-developer-assistants-from-guidewire"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T23:13:26.906989+00:00"
fetched_at: "2026-08-10T23:13:36.606004+00:00"
content_hash: "sha256:5348d91e4af03259d1b408d1692a0b84aaf186cee378d1a0c18e4618619f1ffc"
---

# Accelerating P&C Delivery with Developer Assistants from Guidewire

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Intelligent Experience](https://www.guidewire.com/resources/blog/intelligent-experience)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Accelerating P&C Delivery with Developer Assistants from Guidewire


AI is rapidly reshaping how enterprise software is built, tested, and maintained. Development teams are increasingly adopting an intent-driven model, where developers describe the outcome they want and AI-powered coding tools help generate code, answer questions, automate repetitive tasks, and accelerate software delivery. For Guidewire teams, the opportunity is not just to work faster, but to combine those productivity gains with AI that understands the Guidewire platform, its applications, and its development patterns.


In this post, we explore how our domain-specific Guidewire Developer Assistants fit into that picture: why they exist, what they are, the results we’ve seen, how they support day-to-day Guidewire development, connect to your existing AI stack, and where we’re taking them next.


## Why is There a Need for Guidewire Developer Assistants


Senior Guidewire developers bring years of product knowledge, architectural understanding, and development experience to every change they make. They understand the code in its broader application context, including how Gosu, product models, effective-dating rules, integration patterns, and UX conventions work together within Guidewire. They know where the critical business logic lives, which patterns are cloud-friendly, and which anti-patterns create long-term risk. That expertise is incredibly valuable, but it doesn’t automatically scale as organizations take on more projects, more teams, and more frequent cloud releases.


A Guidewire-native layer makes that deep platform context easier for teams to access and apply in their everyday development work. It brings the expertise and patterns of these experienced Guidewire developers directly into development workflows, helping teams move faster while staying aligned with platform best practices. The Developer Assistants for Gosu, Integrations, and Jutro, now Generally Available (GA) with the[Qusar release](https://www.guidewire.com/sitecore/service/notfound.aspx?item=master%3a%7b53ED8F07-B3F5-4ECA-A472-886A7C17B490%7d%40en) , provide this Guidewire-native layer, bringing domain-specific context directly into the tools developers use every day.


## What Being Guidewire‑Native Really Means and What We’ve Seen in Practice


So what does it actually mean for a Developer Assistant to be “Guidewire-native”? It means each assistant is designed to operate with the context and awareness needed to support the relevant Guidewire development workflows. A Developer Assistant is an AI coding companion that turns a developer’s intent into concrete, platform-safe code. Four design choices make that possible:


- **Purpose-built for InsuranceSuite** , with deep understanding of code syntax, Page Configuration Format (PCF) structures, Guidewire Cloud APIs, and best practices
- **Grounded in Guidewire’s internal documentation** , including cookbooks and curated embeddings, rather than just publicly available web data
- **Version-aware** , providing guidances aligned with the Guidewire cloud release and project setup in use
- **Delivered as part of your Guidewire toolkit through Model Context Protocol (MCP)** , rather than a one‑off chatbot


Those design choices aren’t just architectural nice-to-haves; they show up clearly in the results.


In our internal experiments, we took the same AI coding tool and ran a suite of 33 Gosu tasks twice, once with the Developer Assistant connected via MCP and once without. Without the Developer Assistant, the AI tool failed all 33 tests and, in one instance, produced code that compiled successfully but failed to start the server because it violated a Guidewire-specific rule. With the Developer Assistant for Gosu connected, it passed all 33 tests and produced clean, Guidewire-compliant code that was ready for review.


Additionally, the Developer Assistant for Gosu completed tasks 41% faster by pulling the correct Guidewire patterns from cookbooks in a single step instead of relying on trial and error. Early Access customers and partners have also reported similar reductions in development effort.


## How Guidewire Developer Assistants Fit in Your Daily Workflow


Here are three real Guidewire workflows that show how an AI coding tool, such as Claude Code in our demos, and Developer Assistants work together to streamline multi-step Guidewire tasks into a single intent:


**i. Understanding and Refactoring Gosu Code**


### Developer Assistant: Gosu


In the Gosu demo, the starting point is a long, unfamiliar ClaimCenter class that would normally require a senior developer to modify safely. With the Developer Assistant for Gosu, the developer provides a single prompt describing the new search field needed. Instead of manually stitching together entity, configuration, display key, and PCF updates, a cohesive set of changes proposed by the assistant is reviewed. In practice, this turns complex changes that normally require a senior developer into reviewable proposals that more of the team can safely progress.


**ii. Building Resilient Integration Gateway Flows**


### Developer Assistant: Integrations


In the Integrations demo, the Developer Assistant for Integrations builds an Integration Gateway route that sends open claims to an external fraud scoring system. The Developer Assistant, with access to Integration Gateway knowledge and documentation, generates a starting route. The route is then pushed further with multi-step prompts, generating Java classes from an API file, creating mapping functions, and modifying the existing route. This makes it faster to stand up new, resilient routes without inventing unsupported patterns that create future upgrade or maintenance risk.


**iii. Designing Jutro-Idiomatic Digital Experiences**


### Developer Assistant: Jutro


In the Jutro demo, the Developer Assistant for Jutro helps build a functional personal auto quote experience starting from a blank template. The assistant has access to Jutro development skills, SDK guides, and UX rules so that the generated code uses the right design system components and entity fields. The result is a functional app that feels native to the Jutro Digital Platform, with code, layout, and UX choices aligned to its recommended patterns and practices.


## How Open Platform Connectivity Brings Guidewire Developer Assistants to Your AI Ecosystem


Most of the insurers we work with aren’t starting from zero on AI. You may already have standardized on a primary AI coding assistant, such as Claude Code, Copilot, Cursor, or Amazon Kiro. You want Guidewire capabilities to plug into the AI stack you already have. This is exactly what our MCP-based Developer Assistants are designed to support in an open, tool-agnostic way.


At a high level, each Developer Assistant is exposed as an MCP server. Existing MCP-aware tools and agentic environments can treat these as “Guidewire tools” that can be invoked whenever a workflow touches Guidewire code, configuration or APIs. This open design delivers three practical benefits:


- **Primary AI environment stays the same** - Developers continue working in the tools they already use, which routes Guidewire sub-tasks directly to the relevant Developer Assistant
- **Governance remains centralized in one AI stack** - You continue to control models, logging, and guardrails, while Guidewire provides safe, version-aware tools that plug into that environment
- **No need to build a separate Guidewire knowledge layer** - Instead of hand-maintaining prompts and retrieval over Guidewire documentation, teams rely on assistants that are maintained and updated by Guidewire


In short, you bring the AI foundation that fits your enterprise. We make it Guidewire-aware by plugging in Developer Assistants via MCP.


## What’s Next for Guidewire Developer Assistants


With the Qusar release, we're expanding our AI assistant capabilities to serve more personas across the platform. The[new Developer Assistant for Functions, Product Design Assistant for APD, and Data Curation Assistant for Data Studio](https://www.guidewire.com/sitecore/service/notfound.aspx?item=master%3a%7b53ED8F07-B3F5-4ECA-A472-886A7C17B490%7d%40en) are all available in Early Access (EA). This means more of your product design, configuration, and curation work can benefit from the same velocity gains you see today with Gosu, Integrations, and Jutro Developer Assistants.


Our direction is toward a future where development feels less like assembling individual tools and more like expressing intent. Developers describe the outcome they want, and a fabric of MCP-connected, platform-aware capabilities interprets that intent, breaks down the required work, and proposes a coherent set of changes.


***For IT leaders, the destination is clear. You want your teams focused on solving the insurance problems that matter, rather than memorizing every nuance of the Guidewire stack. With our open platform MCP-connected Developer Assistants, we handle the domain-specific complexities so your teams can stay focused on delivering better outcomes for your business and policyholders.***


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
