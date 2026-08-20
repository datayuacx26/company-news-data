---
schema_version: "1.0.0"
document_id: "7b4c75e374674b92ce288009b5a7094b62b55d9d9a54c70c1552c36d7caf4940"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/the-wix-engineering-2025-review-ai-platforms-data-and-deep-technical-lessons-learned"
published_at: "2025-12-28T18:29:04+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:24:49.305726+00:00"
content_hash: "sha256:9e414941bf5b43e5ab523a02d59fe71753ce78fdd0dabe2436c57b7238275e46"
---

# The Wix Engineering 2025 Review: AI, Platforms, Data, and Deep Technical Lessons-Learned

###


A Year of Building at Scale: Inside Wix Engineering


During 2025, Wix Engineering shared deep dives, playbooks, and real-world lessons from building and operating one of the largest web platforms in the world.


These stories don't start as content. They start as engineering challenges - some of the most complex, high-scale, and unforgiving challenges modern software teams face.


Problems that emerge only when thousands of developers, hundreds of teams, and hundreds of millions of users meet production reality.


From AI-first development to platform engineering, data systems at massive scale, and battle-tested reliability practices, each insight shared here was born from hands-on work: from systems breaking, scaling limits being hit, costs spiraling, latency spiking, and teams being forced to rethink assumptions that no longer held.


This blog post is a curated journey through some of the most impactful lessons we learned this year - not in theory, but by building, operating, and evolving complex systems at scale.


###


AI at Wix: From Strategy to Production


AI wasn't "added" at Wix - it reshaped how we think about development, tooling, and decision-making:


####


AI-Driven Leadership: Navigating Change & Innovation


Wix's CTO


**Yaniv Even-Haim** and VP Engineering


**Aviran Mordo** discuss leadership, uncertainty, and scaling teams through the AI shift. Watch their full talk and read this article:


[Leading Through Uncertainty: How Wix Engineering Navigates Growth, AI, and the Unknown](https://www.wix.engineering/post/leading-through-uncertainty-how-wix-engineering-navigates-growth-ai-and-the-unknown) .


####


How AI Is Reshaping R&D at Wix: A Journey, Not Just an Upgrade


A candid look at the technical, organizational, and human challenges of becoming an AI-first engineering org - from architecture choices to developer education and production automation. Get a deeper understanding with


[this article by Asaf Yonay](https://www.wix.engineering/post/how-ai-is-reshaping-r-d-at-wix-a-journey-not-just-an-upgrade) and watch his insightful podcast video with


**Gefen Mittelman:**


####


The GenAI Transformation at Wix


**Gilad Barkan** and Dr.


**Eli Brosh** share how Wix moved from early AI experimentation to building real AI products at scale - and what comes next. Watch:


###


Building Better AI Systems (Not Just Prompts)


As AI moved into production, engineering discipline became the differentiator.


####


Building AI Systems vs. Software Engineering / Yuval Nissan


Why building AI systems breaks classic software engineering assumptions - from testing and evaluation to ownership, research, and non-deterministic behavior:


####


How Wix’s AI Site-Chat Redefines Chatbots / Ran Geler


Combining LLMs with classic ML techniques, live feedback loops, and dynamic knowledge updates - to deliver smarter, adaptive conversations.


[How Wix's AI Site-Chat Redefines Chatbots with Adaptive Feedback and Dynamic Knowledge](https://www.wix.engineering/post/how-wix-s-ai-site-chat-redefines-chatbots-with-adaptive-feedback-and-dynamic-knowledge)


####


From Hype to Reality: Review of Current AI Practices at Wix / Olga Atzmon & Olha Shainoha


A reality check on building LLM-based systems at scale - exposing gaps, challenges, and best practices beyond the GenAI hype:


####


The Art Behind Better AI: 46% Faster & 23× Cheaper / Idan Dagan


Why context engineering matters more than prompt engineering - and how a few structural changes transformed performance, cost, and accuracy.


[The Art Behind Better AI: How We Achieved a 46% Speed Boost and 23× Cost Reduction](https://www.wix.engineering/post/the-art-behind-better-ai-how-we-achieved-a-46-speed-boost-and-23-cost-reduction)


####


Advancing Enterprise AI: Democratizing RAG Evaluation / Dvir Cohen


In 2025 we also introduced RAGXplain and WixQA - practical tools for diagnosing, debugging, and improving real-world RAG systems. Read here:


[Advancing Enterprise AI - How Wix is Democratizing RAG Evaluation](https://www.wix.engineering/post/advancing-enterprise-ai-how-wix-is-democratizing-rag-evaluation) .


####


Words Matter: How Prompting Shapes Accuracy, Speed, and Cost / Yossi Goldstein


A practical benchmark showing how small prompt changes can swing accuracy and latency dramatically - and how to tune prompts for the metric you actually care about.


[Read here](https://medium.com/wix-engineering/words-matter-how-prompting-shapes-accuracy-speed-and-cost-6626465e1929)


####


A 9-Step Guide to Building an AI Agent You Can Trust / Omer Dodi


A clear 9-step checklist for building reliable agents - from defining tasks and choosing models to evaluation criteria and iterative improvement.


[Read here](https://medium.com/wix-engineering/a-9-step-guide-to-building-an-ai-agent-you-can-trust-69c21ca6c7a1)


###


Vision, Architecture & the Future of Engineering


Here's the big picture - how modern engineering is evolving, and what it takes to stay ahead.


####


The Future of Web Frameworks: A Look at Emerging Trends /


**Yoav Abrahami**


Web frameworks are at a tipping point. Performance, security, accessibility, regulation - and now AI - are redefining what frameworks must handle by default.


**Yoav Abrahami** explores what's changing, why classic assumptions no longer hold, and what the next generation of frameworks must get right. Read


[The Future of Web Frameworks: A Look at Emerging Trends](https://www.wix.engineering/post/the-future-of-web-frameworks-a-look-at-emerging-trends)


####


From Chaos to Control: Rebuilding Reliability in a World of Microservices /


**Yoav Abrahami**


This battle-tested playbook traces Wix's journey from a fragile monolith to a reliability-first microservices architecture - including the "Split by SLO" pattern that still guides how we build today. Download it here:


[From Chaos to Control: How We Rebuilt Reliability in a World of Microservices (Wix Engineering’s Battle-Tested Playbook)](https://www.wix.engineering/post/microservices-reliability-playbook)


###


Data Discovery, Search & Knowledge at Scale


When thousands of teams and data sources collide - discovery becomes the real challenge.


####


Solving Data Discovery at Scale with RAG & Multi-Agent Systems / Yael Rafalovich


How we built "Anna", our intelligent assistant that helps teams find the right data fast - even in a decentralized, domain-driven environment. Read here:


-


Part 1:


[Solving Data Discovery at Scale: How Wix Uses RAG and Multi-Agent Systems to Find the Right Data Fast](https://www.wix.engineering/post/solving-data-discovery-at-scale-how-wix-uses-rag-and-multi-agent-systems-to-find-the-right-data-fas)


-


Part 2:


[The 95% Breakthrough: Ditching RAG for MAG in Wix's AI-Powered Data Discovery](https://www.wix.engineering/post/the-95-breakthrough-ditching-rag-for-mag-in-wix-s-ai-powered-data-discovery)


####


The Art of Secure Search: Mastering PII in Vespa / Anton Kolhun and Gregory Bondar


How we enabled full-text search over encrypted PII data without compromising privacy - and what it took to make it production-safe:


[The Art of Secure Search - How Wix Mastered PII Data in Vespa Search Engine](https://www.wix.engineering/post/the-art-of-secure-search-how-wix-mastered-pii-data-in-vespa-search-engine) .


###


Platform Engineering, Dev Experience & Developer Productivity


Speed doesn’t scale without platforms designed as products.


####


Code at AI Speed: Fixing the Real Bottleneck / Zachi Nachshon


As AI speeds up code generation, the real bottleneck is building and managing code at scale. At Wix, 30M+ lines across 60 repos made that clear. "From 60 Repos to One: How Wix Tackled Monorepo Migration" (


[Part 1](https://www.wix.engineering/post/from-60-repos-to-one-how-wix-tackled-monorepo-migration-part-1) ,


[Part 2](https://www.wix.engineering/post/from-60-repos-to-one-how-wix-tackled-monorepo-migration-part-2) ,


[Part 3](https://www.wix.engineering/post/from-60-repos-to-one-how-wix-tackled-monorepo-migration-part3) ) shows how moving to a single 22GB monorepo unlocked faster builds, clearer ownership, and AI-ready workflows.


You can also watch Zachi's talk as part of Wix Engineering conference:


####


If You Think Platform Engineering Is Just DevOps, You’re Already Behind / Ben Chen


Why platform engineering is a business capability - not a tooling upgrade - and how Wix treats its internal platform as a product.


[Read](https://www.wix.engineering/post/if-you-think-platform-engineering-is-just-devops-you-re-already-behind)


####


Developer Platform: Build & Ownership / Tomer Pilossof and Ethan Pogrebizsky


How Wix built a self-service developer platform while embracing the principle: "You build it, you own it". Watch this deep dive talk:


####


We Added Chaos to Our CI/CD Pipelines - It Made Everything More Stable / Itai Schuftan


How Wix brings AI into CI/CD (logs, triage, migrations) to reduce noise and make pipelines feel more stable, without letting AI touch deterministic deploy decisions.


[Read here](https://medium.com/wix-engineering/we-added-chaos-to-our-ci-cd-pipelines-it-made-everything-more-stable-f03207b3c993)


####


Distributing Context Across Repositories / Ran Yitzhaki


How sharing structured rules and context across repositories makes AI coding assistants dramatically more effective - and how teams can scale that context using Cursor rules and shared tooling. Watch:


####


50× Faster Integration Testing / Natan Silnitsky


How in-memory integration testing and hexagonal architecture transformed reliability, speed, and developer happiness?


[Read here](https://www.wix.engineering/post/50x-faster-and-100x-happier-how-wix-reinvented-integration-testing)


####


From 2 Weeks to 2 Seconds: Elevating Mobile Releases by Streamlining Bundling / Yoav Tsook


How breaking a massive mobile app into pre-built package bundles cut build times by over 80%, unlocked local development, and finally made safe code push possible:


####


Creating a Smart GitHub Code Search MCP from Scratch / Guy Bary


How building a custom MCP server enabled deep, AI-powered code search across repositories, docs, PRs, and commits - turning raw codebases into rich, queryable context for AI tools. Where it fits:


####


On the Way to Perfection: How to Make API Changes Fast and Safely / Elena Kravchenko


How to evolve internal APIs safely without breaking clients - and why "just deprecate it" is rarely enough in real systems:


###


Data Platforms, Spark & Cost Efficiency at Massive Scale


We believe that scale without cost control isn't real scale...


####


How Wix Cut 50% of Its Data Platform Costs / Valery Frolov


A two-part deep dive into cost visibility, attribution, and optimization across a massive multi-cloud data platform.


[Part 1](https://www.wix.engineering/post/how-wix-cut-50-of-our-data-platform-costs-without-sacrificing-performance-part-1) ,


[Part 2](https://www.wix.engineering/post/how-wix-cut-50-of-its-data-platform-costs-without-sacrificing-performance-part-2)


####


PlatySpark: Building Spark-as-a-Service / Almog Gelber


Why off-the-shelf tools weren’t enough - and what it really takes to run 5,000+ Spark jobs a day reliably. (


[Part 1](https://www.wix.engineering/post/how-wix-built-the-ultimate-spark-as-a-service-platform-part1) ,


[Part 2](https://www.wix.engineering/post/introducing-platyspark-how-wix-built-the-ultimate-spark-as-a-service-platform-part-2) ):


####


Migrating 5,000+ Spark Jobs to EMR on EKS / Almog Gelber


A near-transparent migration that cut costs by 50% and reduced spin-up times dramatically. Read this article


[How Wix Slashed Spark Costs by 50% and Migrated 5,000+ Daily Workflows from EMR to EMR on EKS](https://www.wix.engineering/post/how-wix-slashed-spark-costs-by-60-and-migrated-5-000-daily-workflows-from-emr-to-emr-on-eks) to know how.


###


Infrastructure, Performance & Reliability


####


Lessons From Overcoming TimeCapsule Performance Challenges / Dmytro Kostiuchenko


A real-world performance journey driven by incidents and alerts - covering autoscaling, Kafka tuning, and practical techniques for operating high-throughput systems under pressure:


####


Engineering Resilience at Scale / Ben Chen


2025 reminded us how fragile the internet still is. The global Cloudflare outage showed what happens when a single critical player fails. At Wix, we've been building for exactly these moments - with Production State as a single source of truth for system health, and Traffic Light enabling zero-downtime traffic shifts across data centers.


The lesson is clear: Resilience isn't a feature. It's a prerequisite. And if you're not designing for failure, you're not designing at all - read more here:


[Engineering Resilience: Behind the Scenes of Wix's Global Platform](https://www.wix.engineering/post/engineering-resilience-at-scale-behind-the-scenes-of-wix-s-global-platform)


####


Kafka at Scale: Cutting Costs by 30% / Noam Berman


How a push-based consumer proxy reduced redundant reads and eased Kafka cluster load:


[From Bottleneck to Breakthrough - How Wix Cut Kafka Costs by 30% with a Push-Based Consumer Proxy](https://www.wix.engineering/post/from-bottleneck-to-breakthrough-how-wix-cut-kafka-costs-by-30-with-a-push-based-consumer-proxy)


####


Wix Media Platform: The Engine Behind Your Media / Tomer Salton and Eli Ben-David


How Wix serves images and video at massive scale -


**250M+ sites, 6B+ requests/day** - and why fast media delivery is deep platform engineering. Watch:


####


40% Faster Interaction with React Selective Hydration / Gil Eckstein


How React 18’s Suspense enabled faster interactivity without rewrites or new frameworks - 40% Faster Interaction.


[Read this post to learn](https://www.wix.engineering/post/40-faster-interaction-how-wix-solved-react-s-hydration-problem-with-selective-hydration-and-suspen)


####


The Future of React: What’s New, What’s Next, and What Matters for Us / Hadar Grossman


What React 18 and 19 really change in production - from Suspense and concurrency to the new React compiler, and their impact on performance at scale. Watch:


####


Revolutionizing Kafka: Messaging at Scale / Or Ben Ari


How Wix replaced client-side Kafka consumers with a centralized gRPC-based proxy - simplifying development, improving scalability, and cutting messaging costs at scale. Watch:


###


Engineering Culture, Guilds & Communities


####


Building the Guild Energy Wheel: Engagement in Motion - Aviva Peisach


A behind-the-scenes look at how engineering guilds scale engagement, collaboration, and culture - and how interconnected "energy wheels" power long-term organizational impact:


####


Wix Engineering Conference: AI, Platform Engineering & Scaling


A full-day internal conference bringing together real-world lessons from across Wix - from AI systems and platform engineering to performance, reliability, culture, and leadership at scale. Here's our


[best engineering highlights and insights](https://www.wix.engineering/post/recap-from-wix-engineering-conference-2024-ai-platform-engineering-and-scaling) . You can also watch this cool clip about our unique event:


###


Engineering Craft & Unexpected Lessons


####


Keep Your Users Close and Your Bugs Closer / Evelyn Moadi


How deep exposure to user feedback and support workflows leads to better debugging, stronger system understanding, and developers who deliver impact beyond writing code:


####


Pinpointing Pain Points in Your Code: Effective Value-Driven Refactoring / Asaf Korem


How identifying code "hot-spots" - areas with frequent changes - helps focus refactoring efforts where they deliver the highest value, using simple rules and lightweight tools:


####


Fixing 2,000 Code Samples with AI Automation / Adam Friedmann


How documentation teams used AI to eliminate weeks of manual work:


[From Manual Nightmare to AI Automation - Fixing 2,000 Code Samples](https://www.wix.engineering/post/docs-automating-bulk-api-reference-rewrites-with-ai)


####


Team Lead vs. Tech Debt / Itay Friedman


How team leads can turn technical debt from a burden into leverage - with practical frameworks, prioritization tactics, and real-world trade-offs from Wix Engineering. Watch:


####


Type Testing: Do We Really Need It? / Matan Cohen


A sharp, practical session that showed why type testing isn’t a nice-to-have, but a critical tool in complex codebases. Matan demonstrated how type tests help prevent subtle type-related bugs, enable confident creation of advanced type definitions, and clarify when type testing is worth the investment - and when it isn’t.


By the end, you walk away with a clear understanding of when, why, and how to write type tests, and how to ensure your TypeScript types behave exactly as intended. Watch:


####


TypeSystem Shenanigans / Ronen Amiel


TypeScript helps catch bugs at compile time - but Ronen took it several steps further. In an eye-opening live-coding session, he pushed the TypeScript type system to its limits, implementing logic and behavior purely at the type level. The result is a deeper understanding of both the power and the boundaries of the type system - and how to use it to write safer, smarter, and sometimes surprisingly expressive code:


####


Taming Text in MySQL: When an Emoji Breaks Production / Simonas Viliunas


A deep dive into Unicode, collations, and how tiny details can cause massive failures:


[Taming Text in MySQL - How a Simple Emoji Uncovered a Complex Collation Problem](https://www.wix.engineering/post/taming-text-in-mysql-how-a-simple-emoji-uncovered-a-complex-collation-problem)


####


Where Do Dev Terms Come From? / Yonatan Mevorach


A fast, entertaining deep dive into the surprising origins of everyday software engineering terms - and how concepts from other engineering disciplines shaped the language we use daily.


####


SQLing Your Cloud with Steampipe /


**Omer Dolev**


Query cloud infrastructure like a database - and stop drowning in scripts and dashboards:


[Have You Ever Tried SQLing Your Cloud? The Power of Steampipe](https://www.wix.engineering/post/have-you-ever-tried-sqling-your-cloud-the-power-of-steampipe)


###


Oh, One More Thing


####


Introducing Wix Engineering GPT


This year we also shared our new AI assistant for exploring Wix Engineering content, culture, roles, and open-source work.


[Talk to us here](https://chatgpt.com/g/g-687ce507c2f48191b2bb7f8d38a7aaeb-wix-engineering)


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) or


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA)
