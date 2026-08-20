---
schema_version: "1.0.0"
document_id: "c6d50eb86c3a5cbe2deee73dc8b69e5829560a538c974313063a044b2f83c34f"
company_key: "yc-theneo"
company: "Theneo"
source_id: "yc-theneo-news-import-36ab7290964e"
canonical_url: "https://www.theneo.io/blog/how-to-create-user-guides-and-tutorials-in-theneo"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-13T01:12:05.546140+00:00"
fetched_at: "2026-08-13T01:12:07.113059+00:00"
content_hash: "sha256:967d1e19f9cf5610f718c92538fe2d2aebca6ea6132f34e006f5ef95afe35738"
---

# How to Create User Guides and Tutorials in Theneo

**A user guide explains how to complete a task with a product, while a tutorial teaches a workflow through a sequence of steps.** In Theneo, both can live beside your API reference in one developer portal.


## What can you document beyond an API reference?


API reference answers which endpoint to call, which parameters are required, and what a response contains. Product documentation also needs to help someone get started, understand a concept, complete a workflow, solve a problem, and learn what changed.


A Theneo portal can combine getting-started guides, tutorials, conceptual explanations, troubleshooting, generated API references, and changelogs. This matters because a technically complete reference can still leave a developer unsure about the order of operations.


## How do you create a user guide in Theneo?


Start with one reader outcome. Prefer an action such as "Configure authentication" over a vague label such as "Platform overview."


1. **Create the structure.** Break the outcome into prerequisites, setup, core steps, verification, and troubleshooting.
2. **Write in the visual editor.** Product managers and technical writers can build the narrative without working directly in code.
3. **Add technical detail.** Include examples, code blocks, endpoint links, and decision callouts.
4. **Choose a guide-friendly layout.** Theneo's Single Page Template is designed for modular guides, tutorials, onboarding, and help-center content.
5. **Review as a reader.** Test every step and answer the likely next question.


Theneo's[Themes and Templates guide](https://docs.theneo.io/get-started/themes-and-templates) recommends the Single Page Template for guides and tutorials. It supports modular sections, focused navigation, and deep links.


## How should you structure a tutorial?


- **Outcome:** State what the reader will have at the end.
- **Prerequisites:** List required access, files, or prior setup.
- **Steps:** Put actions in the required order.
- **Expected result:** Show how to confirm each important step worked.
- **Troubleshooting:** Cover likely failures and how to identify them.
- **Next step:** Link to the next guide or relevant reference.


Keep one idea per section and answer the question before adding context. This improves scanning and makes the guide easier for search and AI answer systems to extract accurately.


## Can developers manage guides in Markdown?


Yes. Teams can work with Markdown and the Theneo CLI while other contributors use the editor. The current[Theneo documentation](https://docs.theneo.io/) is the source of truth for supported workflows and setup guidance.


Markdown also provides a practical response to vendor-lock-in concerns because narrative content remains in a widely used text format. Keep source files in your own repository and test import and export workflows.


## How customizable are Theneo user guides?


Teams can choose between the Single Page Template and Continuous Scrolling, then configure colors, logos, fonts, code-block styling, and advanced custom CSS or JavaScript. Use Single Page Template for tutorials and onboarding. Use Continuous Scrolling for shorter references.


## What should you review before publishing?


- Does the opening define the task and outcome?
- Are prerequisites explicit?
- Can a new user complete every step?
- Do examples match current product behavior?
- Do links point to the correct reference version?
- Is there an owner and a review trigger?


AI can accelerate a first draft, but it should not be the final reviewer. Product behavior, permissions, and edge cases require human verification.


## Build one portal around the reader's job


When guides and API documentation share one platform, a reader can learn the concept, follow the workflow, inspect the endpoint, and check recent changes without losing context. Start with one high-friction customer task and use support questions to decide what to write next.
