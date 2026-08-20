---
schema_version: "1.0.0"
document_id: "e0acad08cf79bbef2aa9c1327ff25516607f29a0bfb1515bd452adb4c74171da"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/from-writing-words-to-designing-systems-the-future-role-of-the-content-designer"
published_at: "2026-04-10T00:00:00+00:00"
first_seen_at: "2026-07-23T07:46:02.800033+00:00"
fetched_at: "2026-07-28T22:15:54.285032+00:00"
content_hash: "sha256:40c0ded3de96abdada868111d9cfff47af548154af8d3d0d065af57b4db03d7d"
---

# Ditto Blog | From writing words to designing systems: The future role of the content designer

*This is a guest blog by Chrisi Webster. Chrisi is a Senior Content Designer at Checkout.com. She focuses on the intersection of language and systems design, building the frameworks and AI logic that allow products to communicate at scale.*


-----


For a long time, content design has been about words: writing clear labels, helpful error messages, and intuitive flows.


That hasn’t gone away. But in my work at Checkout.com, I’ve started to notice the same thing happening across very different projects: Writing isn’t enough on its own anymore. I was responsible for thinking about the underlying system that would get us to the right words.


## **Moving from writer to systems thinker**


This shift didn’t come from one project. It showed up in different ways, across completely different work.


While working on our AI analytics assistant – a chat interface that lets merchants query their payment data in natural language and get generated, contextual responses – I couldn’t solve the problem by writing a fixed set of responses. The system needed to generate answers based on different queries and different data sets, so the work became defining how those responses should be produced. This meant working with engineers on prompt structure, tone and how the system adapts to different queries.


In a separate project, I was working on string externalisation – moving content out of the code and into a shared place so it can be reused and updated more easily. Here, the problem wasn’t generation; it was scale. Content lived in different places, naming was inconsistent, and even small updates required engineering time. To make content reusable, we had to rethink how it was stored and structured.


I saw the same pattern again when working on our notifications inbox and merchant emails. Writing copy for a few MVP use cases worked initially, but it didn’t scale. Different events required different combinations of message, tone and action, so we needed to define the inputs structure and guardrails to keep content consistent.


Different problems, but the same underlying shift: designing content as a system.


Across all of this work, the focus shifted in the same way. Instead of just asking what should this *say* , we started asking:


- Where does this content live?
- How is it structured?
- How is it generated?
- How can it be reused?


Writing is still part of the role, but more and more, the work is about designing the conditions that make content work at scale.


## **What this looks like in practice**


I’m noticing that I now spend the majority of my time on the architecture behind the words. This includes:


- **Defining naming conventions** that scale across products so content stays consistent.
- **Structuring content** so it can be reused, localised and indexed.
- **Collaborating with engineers** on how content is stored and retrieved.
- **Shaping prompts and responses** for AI-driven features.
- **Creating frameworks** that ensure consistency across dynamic outputs.


A lot of this work is invisible in the final UI. But it has a huge impact on how coherent, scalable and maintainable a product is.


## **The impact on the role**


The role of content design is expanding. It now involves:


- **Defining delivery:** how content is generated, stored, and moved.
- **Thinking in structure:** focusing on the logic rather than just the phrasing.
- **Designing for scale:** the goal isn’t a perfect sentence, but a system that produces good ones consistently.


That can feel like a big shift, especially if your background is rooted in writing. But it’s also a huge opportunity. The people who understand both language and systems are the ones shaping how products communicate at scale.


## **How to start tomorrow**


You don’t need to become an engineer. But it helps to get closer to how content works beyond the interface. A few practical starting points:


- **Learn how content is stored** and managed in your specific product.
- **Get familiar with structured content** and localisation workflows.
- **Collaborate closely with engineers** on implementation early in the process.
- **Experiment with AI tools** to understand how prompts shape output.
- **Document patterns and systems** , not just copy.


You don’t have to do all of this at once. Even small steps can start to shift how you think about the role and the impact you can have.
