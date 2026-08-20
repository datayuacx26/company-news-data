---
schema_version: "1.0.0"
document_id: "a631413e2b5c1f30cd47260d5f034f22293c61a37673dc1c0a2d764b088eac16"
company_key: "sonos-inc-common-stock"
company: "Sonos Inc."
source_id: "sonos-inc-common-stock-rss-b40e25ea1a3a"
canonical_url: "https://tech-blog.sonos.com/posts/better-data-better-products-building-confidence-into-the-data-behind-every-sonos-experience/"
published_at: "2026-03-28T02:50:00+00:00"
first_seen_at: "2026-07-20T23:18:24.938468+00:00"
fetched_at: "2026-07-28T22:00:16.885857+00:00"
content_hash: "sha256:7d100bd8a8e7eb4a06e9eb22c83decd83f4ac14ed7c27264d9bf838eabfdfa59"
---

# Better Data, Better Products: Building Confidence into the Data Behind Every Sonos Experience

At Sonos, everything we build is centered around the experience in the home. Our understanding of how that experience unfolds is shaped by the data our products generate, which is used to inform decisions such as how we create new products, design new app features, and improve system performance. Because this data supports nearly every part of the company, much of Sonos’ day-to-day operations depend on it behaving predictably behind the scenes. Yet the Sonos ecosystem is a highly complex and vast software product. It spans the apps users rely on to control their systems, the firmware running on speakers, and the fleet of cloud services that enable features like voice control and streaming music from your favorite providers. For more details on our data usage and privacy, you can visit[sonos.com/legal/privacy](https://www.sonos.com/en-us/legal/privacy) .


But for data to truly power all of these operations, it has to be consistent, coherent, and trustworthy; it’s impossible to use information at scale if data from different parts of our ecosystem don’t align, can’t be validated, or mean something different depending on its origin. Expecting dozens of software teams to manually align on thousands of record types, and implement them in exactly the same way across mobile, web, and speaker software proved unrealistic, as our early systems suffered from mismatched, unprocessable, and missing data. In an infrastructure of this scale, data inconsistencies were not only difficult to trace, but often required weeks of coordination across software teams to resolve, and would render affected data unusable. This made it difficult to trust the insights shaping our products and listening experiences, even when data appeared complete. We needed a way to guarantee that every Sonos product spoke the exact same data language, always.


## A Fragmented Data Story


### *Why Confidence in Data Matters*


Before diving into how we addressed these problems, it’s worth understanding what unreliable data actually costs in practice. Every team relies on the data produced by our products in many ways, but a few representative questions help illustrate how it directly impacts our product development, quality, and support: Are customers actually using a new feature after we enable it, and should we continue enhancing it? Are products performing reliably across different home and network environments, and what’s common in households that are or are not stable? Which parts of product setup are taking the longest, and how can we make those processes smoother? Are new acoustic tuning changes making a real difference in sound quality?


High-quality data is what allows us to ask and answer questions like these with confidence. When data is trustworthy, we can identify patterns that inspire new features, detect issues earlier, and confirm that changes we make are truly improving the Sonos experience for our users. Poor-quality data has the opposite effect. When records are inconsistent, missing values, or interpreted differently across platforms, it becomes difficult to understand what is actually happening. Insights slow down or point us in the wrong direction, and we may not be able to answer the critical questions teams rely on to guide development.


### *Outgrowing Our First Approach*


Before we could design a system that guaranteed reliable data, we had to understand why our earlier data collection approaches (referred to as *telemetry* ) were falling short.


Historically, each data-producing team assembled records by choosing their own field names, values, and types before simply sending them to our data-processing pipeline. While this initially helped teams develop quickly, inconsistencies compounded as our ecosystem grew. A speaker’s equalizer value might be sent as` bass_level` on iOS but` bass` on Android, or as a string on one platform and an integer on another. Trueplay data may appear as` trueplay_tuning` or` trueplay_calibration` depending on the codebase. An ambiguous field like` time` gave no indication whether it represented a timestamp, a duration, or an end moment, nor what unit it was in. Because of this, backend pipelines and data users were left to infer the meaning of records and consolidate them after the fact, an approach that was costly, error-prone, and impossible to scale. This also meant records had no reliable way to be validated, and that a field could be accidentally renamed, changed, or dropped entirely and shipped to production unnoticed.


In the absence of shared guidelines, data producers had little choice but to rely on their own context and intuition when deciding what records should contain. But with developers lacking visibility into data’s downstream use cases, and analysts having limited input into how it was generated, this disconnect led to discrepancies between the data produced and how it was intended to be used. This meant that even when records appeared well-formed from the producer’s perspective, they may have not contained the nuanced details or values that analysts needed to answer their questions.


The hardest data problems weren’t outright failures, but systemic shortfalls that made misalignments inevitable, both between platforms, and what teams felt data was meant to represent:


Without shared guidelines, each platform produces the same record differently, resulting in confusion and ambiguity for teams relying on data


We tried addressing these issues incrementally, but each fix exposed new edge cases elsewhere in the system, making it clear our problem was structural. We needed an entirely new approach to telemetry that eliminated guesswork, pre-defined record structures, and guaranteed consistency across every Sonos product and platform.


### *Rethinking Data as a System*


Once we understood why early telemetry systems were failing, the next step was defining what a replacement needed to do.


The first requirement was a single source of truth: one place where every record type is defined *once* with clear field names, data types, constraints, and documentation. This space would also be where data producers, consumers, and experts can review and approve changes to records together. But good definitions would only be a start, as that would still rely on every team implementing them correctly by hand. To ensure true consistency in records produced across iOS, Android, web, and firmware, record definitions would need to translate directly into code and integrate seamlessly. Along with this, we recognized that knowing record expectations in advance made validation feasible for the first time. Our initial instinct was to validate telemetry in the backend to keep validation centralized and operationally simple, but we quickly realized this would still allow malformed data to travel too far before failing, leaving little opportunity to correct it. Instead, we decided that another requirement was to shift reliability ‘left’, validating records on producing devices at the time records were created to prevent incorrect data from ever entering the system. In practice, this would mean issues are caught closer to where they originate, reducing the chance that flawed data leads to flawed decisions that affect customers at scale.


Together, these requirements shaped the foundation of a new telemetry architecture that defines records once, ensures consistent code everywhere, and validates data as it’s created. These became the basis for the **Sonos Telemetry Interface Definition Language** .


## **One Data Language for All**


The Sonos Telemetry Interface Definition Language (IDL) is the system we engineered to address these challenges. At a high level, the IDL acts as a shared contract between every Sonos product and platform, defining how data should be produced and enforcing those rules everywhere. Instead of relying on shared conventions and manual alignment, the IDL integrates directly into Sonos software, and is responsible for keeping records and their definitions aligned at scale, removing the ambiguity that previously made data difficult to trust.


### *Shared Definitions, Shared Understanding*


A reliable data system starts with clear definitions shared across the company, so our first requirement was creating a single source of truth where every type of record could be defined, discussed, and approved before implementation. To make definitions accessible to both engineers and non-engineers, we needed a format that was both human-readable while machine-enforceable.[JSON Schema](https://json-schema.org/learn) provided that balance, allowing us to describe fields, constraints, and documentation in a structured way. Moreover, it stood out compared to other options with its support for shared schemas, where common fields could be defined once in a base schema and referenced across many record types.


In the IDL, each record type is defined in its own individual *schema file* , which describes that record’s structure and meaning so any team can understand its purpose and rely on it having that shape. For example, both technical and non-technical readers can review the following schema and immediately understand what the record represents, contains, and how it’s structured:


With the format decided, GitHub became the natural home for these schema files. Its pull-request workflow provides a shared place for developers, analysts, and product partners to review and shape data together. This ensures that new records, or changes to existing ones, are always reviewed for alignment between producing and consuming teams before becoming part of the source of truth. Each schema file is versioned and tracked in this repository like any other source code, creating a transparent history of record shapes that all teams can depend on.


### *From Contracts to Code*


Once we established a standard location for record definitions, it became clear that they alone still could not guarantee correct usage. Our next challenge was then ensuring these definitions were implemented consistently across each platform.


When telemetry code was hand-written, even small mistakes such as misspelled fields, missing required values, or subtle structural differences led to downstream issues. To eliminate this entire class of errors, the IDL generates telemetry code that producers can import directly into software to ensure their records are created correctly. While we initially considered generating only record validation code, we realized that approach would still require teams to manually implement record construction logic, which at Sonos’ scale is one of the most time-consuming pain points for developers. Instead, the IDL generates complete, production-ready libraries, known as *serializers* , that provide everything necessary for our apps, firmware, and services to construct and validate records together. This system, aptly monikered “codegen”, leverages the predictable structure JSON Schema provides to transform record definitions into consistent telemetry code for each platform, whether it’s Swift for iOS, Kotlin for Android, JavaScript for web, or C++ for speakers. By generating all code ahead of time from one source, we remove the possibility for platforms to implement telemetry differently, enforcing correctness automatically at integration while eliminating tedious development work for producers.


The IDL’s codegen process functions by parsing each schema file into a normalized, language-aware representation, where a rules engine maps JSON Schema types and attributes into their appropriate counterparts for each language, applying necessary naming conventions and rules along the way. For example, the JSON Schema type` number` becomes` Double` in Swift and Kotlin,` string` becomes` std::string` in C++, and` step_name` is renamed` stepName` in languages where camelCase is standard. The result is a uniform data structure, known as a *context* , that describes a record in terms native to each platform, with a unique context generated for each target language. From there, the IDL contains a set of templates in the form of[Mustache](https://mustache.github.io/mustache.5.html) files that act as the blueprints for how telemetry code should be generated for each language. These describe how to iterate over a record’s context and output the code necessary to construct, fill, and validate that specific record. By encoding rules in templates, we guarantee the same conventions are applied to every record across all languages.


When the IDL renders a context with its corresponding template, the result is complete serializer code that can be imported directly into Sonos software, giving developers everything they need to create the record to the exact spec of its schema. Using the SetupStep schema shown earlier, the codegen process would automatically generate the following Swift code:


### *Products That Evolve Together*


Once the telemetry code files are generated, the final step is ensuring every Sonos product receives them, and any future updates to them, at the same time. To keep every device leveraging these files aligned, the IDL includes an automated distribution system that synchronizes schemas and serializers across our ecosystem.


When a schema file is added or changed, an automated workflow runs the codegen process, publishes the resulting serializers to a cloud registry, and sends a notification to a shared endpoint that every dependent software team listens to. Upon receiving the notification, each team pulls the latest telemetry code from the registry into their software builds, and because all teams are notified simultaneously, updates propagate uniformly across every platform. This ensures record definitions apply and update systematically, without teams needing to coordinate releases, track versions by hand, or worry about drifting out of sync. By coupling codegen with deployment automation, the IDL keeps telemetry continuously aligned across the millions of devices running Sonos without any manual coordination.


### *Record Lifecycle: Definition to Creation*


With definitions centralized, code generated automatically, and updates distributed uniformly, the final step is seeing how everything works together end to end:


The IDL takes approved schemas and automatically produces and distributes serializers to ensure the records are created consistently across iOS, Android, and web


## **Data That Just Works**


The introduction of the Telemetry IDL has fundamentally changed how teams across Sonos produce and use product data.


From a data engineering perspective, strongly-defined definitions have largely eliminated processing and schema mismatch errors, and our IDL-backed pipelines now process billions of records with nearly nine nines (99.9999997%) of reliability. This has significantly reduced the operational cost of consolidating and recovering data, and data engineers now spend far less time fixing telemetry-related issues or compensating for malformed records. This allows them to focus on building and improving core data systems that drive new impact instead.


Product development teams saw a similar shift. With developers able to rely on the automatically generated code from the IDL and trust that it evolves autonomously alongside product code, the effort required to create new telemetry has been greatly reduced. This allows product engineers to focus on building new products and features our data can support rather than debugging subtle data discrepancies. With entire classes of cross-platform dependencies and misalignments handled, software releases are more predictable and provide teams clearer visibility into the direct impact of their changes and updates.


Analytics teams benefited most from the IDL’s emphasis on shared definitions and built-in documentation. With analysts able to participate in the shaping of records, data now captures the information needed to actually answer the questions it was created for. With well-written schemas describing an event’s purpose and intended use by design, data users no longer need to interpret meaning after the fact, allowing analyses to move faster and provide previously unattainable insights with more certainty. Questions shifted from “can we trust this data?” to “what does this data tell us?”


Together, the Sonos Telemetry IDL has made product data dependable across the ecosystem, giving teams a clear and unified view of how products and experiences perform at scale. With reliability built into our data by design, every product now evolves from the same shared foundation. That consistency allows teams to confidently improve the Sonos experience across devices and updates in a way that’s cohesive and seamless, resulting in elevated experiences that our listeners feel, hear, and enjoy in their homes around the world.


Share
