---
schema_version: "1.0.0"
document_id: "2e186892962c803418f81d5075d6dbbe4a0bdc035a44b7924f0c0224ca43c5e0"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/inside-data-engineering-at-doximity-building-for-impact"
published_at: "2025-08-20T15:29:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:58e4473c83206adfb36bf01dc1e92bd90b4760346d31153dd852659bdca7a481"
---

# Inside Data Engineering at Doximity: Building for Impact

IBM defines Data Engineering as "the practice of designing and building systems for the aggregation, storage, and analysis of data at scale, empowering organizations to get insights in real time from large datasets."


While this definition generally captures what data engineering is, it blends together distinct specializations that enable scalable transformation of raw data to actionable insights. At Doximity, we break this down into two distinct roles: product-focused data engineers and platform-focused data engineers, each playing a crucial role to fuel our organization with timely, reliable data. Our platform-focused data engineers sit on a centralized team that owns two core areas: (1) providing a high-velocity, low-risk data platform and (2) providing reliable and accessible services, standards, and foundational datasets. Our product-focused data engineers are embedded directly on product teams and leverage our data platform, its services, and data to shape our products and deliver value directly to our users. Let's take a closer look at a day in the life of our product team data engineers.


## Finding Flow


As a member of a cross-functional product team, mornings typically start with a quick scrum (sometimes live, sometimes asynchronously in Slack), providing a shared space to align priorities and highlight blockers before diving into focused work. Meetings are minimal; most of the day is spent deep in the code, refining data pipelines, optimizing queries, reviewing code, and working closely with the other members on your team. We are first and foremost engineers, and take pride in spending as much time as possible doing what we love: coding. This includes everything from small snowflake SQL logic tweaks to systemic architecture designs. The goal isn’t only to move data but also to ensure that every system we build remains maintainable for fellow engineers and intuitive for analysts. It’s no wonder that one of our guiding principles is “Always leave the code better than you found it.”


## Driving Impact


At Doximity, data engineering for products is more than just ETL; it’s about shaping the product itself. Every system and pipeline we build directly serves an end product, allowing engineers to see the immediate impact of their work. Enhance a ranking algorithm and watch engagement rise? That was you. Incorporate a new data source and see user behavior shift? Incredible! Unlike traditional data engineering roles, where pipelines often feed into more pipelines, our work on product teams stays tightly connected to the user experience. To achieve this, cross-functional collaboration is a must. One product may be seeking real-time adjustments based on user interactions while still maintaining a low-latency experience for our users. Another product may be supporting multiple, concurrent split tests while balancing cross-product dependencies and potential user impact. Our software developers, data analysts, data engineers and product managers all sit on the same team, collaborating closely together (albeit remotely) to execute on these complex data challenges.


## Balancing Cutting-Edge with Stability


One of the challenges in the field of data engineering is adopting new technologies without constantly switching tech stacks. At Doximity, we focus on technology that measurably improves performance, maintainability, and developer experience, helping us avoid falling for every shiny new tool. The goal is always pragmatic: build stable, scalable systems that support rapid iteration without unnecessary churn. In the fast-evolving world of technology, adaptability is key, but at Doximity, two core data technologies remain constant: Python and Snowflake. These tools are deeply embedded in our workflows, shaping how we process, analyze, and leverage data every day. Whether building scalable data pipelines, optimizing queries, or unlocking new insights, we continually explore innovative ways to maximize their potential and drive our products forward.


## Crafting Code, Culture, and Change


Growth at Doximity isn’t just about becoming a better coder. Our product-focused data engineers are encouraged to take ownership, identifying and driving changes that not only improve the product but also the broader data engineering team and data organization as a whole. Additionally, mentorship is a key part of our culture. Whether it’s helping analysts navigate a complex dataset or guiding newer data engineers through difficult architectural decisions, there are numerous opportunities to lead and grow.


## One Team, One Data Mission


A day in the life of a product-focused data engineer means partnering with your cross-functional product team to deliver high-impact data solutions that drive value for our physicians. If you’re looking for a role where you can build, optimize, and have a direct impact on the product, this might just be the team for you. For anyone working in data, though, you know that the ability to achieve this relies heavily on being able to access the data you need when you need it with the tools you need to solve the problem at hand. Stay tuned for part two of Data Engineering at Doximity, a deep dive into how we’re building scalable, high-performance systems.


*Be sure to follow[@doximity_tech](https://x.com/doximity_tech) if you'd like to be notified about new blog posts.*
