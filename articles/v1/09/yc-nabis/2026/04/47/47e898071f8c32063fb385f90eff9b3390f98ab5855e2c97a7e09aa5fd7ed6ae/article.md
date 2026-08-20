---
schema_version: "1.0.0"
document_id: "47e898071f8c32063fb385f90eff9b3390f98ab5855e2c97a7e09aa5fd7ed6ae"
company_key: "yc-nabis"
company: "Nabis"
source_id: "yc-nabis-news-import-2c6e9e723fce"
canonical_url: "https://nabis.com/blog/from-fragmented-to-real-time-rebuilding-a-marketplace-that-moves-at-the-speed-of-demand"
published_at: "2026-04-10T00:00:00+00:00"
first_seen_at: "2026-07-22T05:11:17.136501+00:00"
fetched_at: "2026-07-28T22:15:54.285032+00:00"
content_hash: "sha256:c71444b1e0cbdd061e73c43966a8e81a25724909fcaec4bccacbbe3246d0901f"
---

# From Fragmented to Real-Time: Rebuilding a Marketplace That Moves at the Speed of Demand

Marketplace V2 grew out of a familiar tension: the quiet buildup of complexity beneath a marketplace that continued to serve customers well, and the louder pull to move faster, test ideas more freely, and create better paths to choice, access, and innovation.


Our goal was to build a foundation that supports discovery and iteration while respecting the realities of a multi-state cannabis ecosystem, where each market brings its own legal, operational, and compliance constraints.


This post is a high-level look at how we approached that challenge, the architectural choices we made along the way, and how Marketplace V2 sets us up to build faster, smarter, and more reliably in the future.


## From monoliths to models that make sense


In our legacy systems, many of our core data models grew organically alongside the business.


An *Order* , for example, came to represent far more than just a transaction. It contained delivery details, driver assignments, reservations, pricing adjustments, warehousing context, compliance metadata, and more, all bundled into a single, tightly coupled structure.


While this approach worked, it made the system harder to reason about, harder to evolve, and harder to reuse across new experiences.


In Marketplace V2, newer systems introduce more clearly defined, purpose-built tables with stronger boundaries and ownership, while legacy data models continue to play an important role where they still make sense.


The result is a marketplace that can draw from both worlds, combining improved structure where it exists with careful integration where it doesn’t, all in service of a data model that’s more reliable, easier to understand, and better aligned with how the marketplace actually behaves.


This shift wasn’t just about cleanliness. It was a prerequisite for speed, accuracy, and long-term flexibility.


## A marketplace built on many realities


One of the core challenges in rebuilding the marketplace is the lack of a single source of truth.


We support multiple states, each with its own systems, schemas, and compliance requirements. Retailer rules, brand preferences, inventory state, order data, and product information all live in different databases, often shaped by different historical and regulatory needs.


At the same time, the marketplace experience demands speed. Search must be fast. Reordering must be accurate. Recommendations must reflect reality. That means pulling together disparate data models into a single, retailer-focused view without forcing every engineer to hold the entire Nabis system in their head.


Our solution was to treat the Marketplace as its own independent system, with its own rules, data shapes, and guarantees, designed around the retailer experience.


## Real-time data replication as a foundation


To make this possible, we leaned heavily on real-time data replication using Kafka.


Rather than synchronizing systems through periodic batch jobs or tightly coupled APIs, we listen directly to changes happening in our source databases.


Whenever a row is inserted, updated, or deleted in any of these systems, that change is detected immediately. We then transform the data into the shape required by the Marketplace and stream it into our own marketplace database. This includes data from multiple sources flowing into a single, query-optimized model.


Because this process is event-driven and continuous, our systems stay in sync in near real time, often within milliseconds. It also allows us to evolve marketplace-specific data models independently, without forcing upstream systems to change or slow down.


This source-and-sink model is a key enabler of Marketplace V2, providing a reliable way to unify disparate systems in real time while keeping the marketplace fast, focused, and adaptable.


## Better data, better experiences


This architectural shift paid off quickly in customer-facing ways.


Marketplace V2 unlocks more accurate reordering flows, better product suggestions, and dramatic improvements in search speed and relevance. Perhaps most importantly, it enables the multi-cart experience, allowing retailers to shop for multiple licensed locations simultaneously, all within a single, coherent workflow.


These improvements aren’t just performance wins. They’re about reducing friction, increasing confidence, and giving retailers better tools to run their businesses.


## Built with the mission in mind


Marketplace V2 represents a significant investment in the platform's future.


Every decision, from data modeling to replication strategy, was made with our mission in mind: empowering the world to discover cannabis by providing choice, access, and innovation.


We built this system not just to solve today’s problems, but to create room for what comes next. We’re excited to share how it works, why we made the choices we did, and how it positions us to better support the cannabis industry as it continues to evolve.
