---
schema_version: "1.0.0"
document_id: "eb752496bede8534c46c3d9305acb8c3872bc5fdc0596de80ca8fb37bf5c571d"
company_key: "yc-datafold"
company: "Datafold"
source_id: "yc-datafold-news-import-e233b68116ce"
canonical_url: "https://www.datafold.com/blog/the-datafold-migration-agent-2-0/"
published_at: "2025-09-29T00:00:00+00:00"
first_seen_at: "2026-07-24T03:54:32.907441+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:61dedf221902cae60256a72413cf232818f1970ba12a1633b30dbc76f0ea7bcd"
---

# Introducing the Datafold Migration Agent 2.0

Since we launched the Datafold Migration Agent last year, we’ve helped teams modernize their data stacks at scale. In that time, DMA has done what once looked impossible: translating[300,000-character stored procedures into dbt](https://www.datafold.com/blog/stored-procedure-migration-slicer) ,[cutting migration timelines by 6x](https://www.datafold.com/case-study/chg-healthcare-mysql-to-snowflake-migration) , and[delivering cost savings of 75%](https://www.datafold.com/case-study/ingersoll-rand-synapse-to-snowflake-migration) compared to traditional approaches. With each migration, the Agent got smarter.


That’s the difference between DMA and traditional consulting-heavy approaches. Where others scale by adding people, we scale by improving software. Every migration gives us insight that helps improve the Agent itself, making it better at handling greater scale, deeper complexity, and the hidden bottlenecks that stall migrations. (Quick note: we’re not talking about training on your data; we’ll never do that).


Today, we’re announcing the culmination of those learnings: **Datafold Migration Agent 2.0** . It’s a complete rebuild of the Migration Agent from the ground up. Built on a graph-based model that maps entire environments, it understands dependencies and intelligently refactors code into a modern paradigm.


## From translator to navigator


For years, our industry has assumed that automation in migrations meant faster translation. But translation was never the only bottleneck, it was everything around it too: discovery, dependency analysis, refactoring, data access, and, most critically, validation.


With its 2.0 release, we not only improved these core capabilities for DMA, we made them work better, together. Now, all DMA functionality is orchestrated in a single, intelligent system that delivers trustworthy migration outcomes at scale.


Here’s what’s new:


### **Surfacing hidden dependencies **


Legacy stacks often bundle databases, ETL tools, orchestrators, and BI layers together, with dependencies that customers themselves can’t fully map or list out. DMA 2.0 models these systems as a graph, surfacing hidden dependencies and letting the Agent traverse them intelligently.


### **Translation at (seriously massive) scale**


Converting SQL or stored procedures can be a straightforward task for smaller projects. But at scale it gets tricky fast. AI alone isn’t a salve–generic LLMs quickly hit context-window limits on massive procedures, losing business logic or breaking dependencies. DMA 2.0 is purpose-built for this complexity:[it slices massive procedures into modular pieces](https://www.datafold.com/blog/stored-procedure-migration-slicer) , consolidates scattered fragments, and ensures that what emerges is accurate and maintainable.


### **Refactoring handled automatically**


Lift-and-shift gets you into the new environment quickly, but some legacy artifacts can’t just be carried over. Stored procedures are a good example: when translating them into modern code like dbt, they often need to be restructured into modular dbt models. DMA 2.0 understands how these pieces fit together and automatically restructures logic where it belongs, turning brittle legacy code into clean, modern pipelines.


### **Best in-class testing data**


When your team is ready to validate a migration, waiting for access to production data can delay the entire process. And when that data is sensitive, privacy concerns can stall testing altogether. In these situations, DMA 2.0 solves both problems by generating best-in-class synthetic datasets, enabling large-scale validation without compromising security.


### **Validation built in**


Validation is the hardest part of any migration. Proving that the translated code produces the exact same results is what makes or breaks a migration. DMA 2.0 automatically creates a test harness to prove the correctness of every translation. It freezes source data (real or synthetic), copies it to the target, runs translated code against both sides, and compares outputs using data diffs. The result is complete parity you can trust at scale.


By moving from a translator to a navigator, DMA 2.0 understands code in its old and new contexts, reshapes it for the future, and manages the migration process end to end.


## The modernization accelerant


We’re building the intelligence and automation to handle migrations of any scale and across any platform. With a graph-based core, full lifecycle automation, and synthetic data generation, DMA 2.0 has turned what used to be a painful, years-long process into a predictable, accelerated path to modernization.


Ready to see what it looks like in action?[Book a call with our team of data engineering experts here.](https://www.datafold.com/demo/)
