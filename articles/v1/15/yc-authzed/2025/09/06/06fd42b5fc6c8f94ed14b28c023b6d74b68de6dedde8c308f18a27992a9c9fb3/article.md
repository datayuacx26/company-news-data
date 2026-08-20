---
schema_version: "1.0.0"
document_id: "06fd42b5fc6c8f94ed14b28c023b6d74b68de6dedde8c308f18a27992a9c9fb3"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authzed-is-5-event-recap-authorization-infrastructure-insights"
published_at: "2025-09-02T18:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:02ea816fc6518961be1d752db34eafee114e2a5942716291c5e890a39af77699"
---

# AuthZed is 5: What We Learned from Our First Authorization Infrastructure Event

Last month we celebrated AuthZed's fifth birthday with our first-ever "Authorization Infrastructure Event" - a deep dive into the technical challenges and innovations shaping the future of access control.


The livestream brought together industry experts from companies like Canva and Turo to share real-world experiences with authorization at scale, featured major product announcements including the launch of AuthZed Cloud, and included fascinating discussions with database researchers about the evolution of data infrastructure. From solving the dual-write consistency problem to powering OpenAI's document processing, we covered the full spectrum of modern authorization challenges.


**[Watch the full event recording](https://youtu.be/uz_gxz3whS0)** (2.5 hours)


## The Big News


Before we dive into the technical talks, let's start with the big announcements:


### AuthZed Cloud is Live


We finally launched[AuthZed Cloud](https://authzed.com/cloud) - a self-service platform that allows you to provision, manage, and scale your authorization infrastructure on demand. Sign up with a credit card, get your permission system running in minutes, and scale as needed - authorization that runs like cloud infrastructure. Through our[AuthZed Cloud Starter Program](https://authzed.com/cloud/starter-program) , we're also providing credits to help teams try out the platform.


**[Watch Jake's keynote](https://youtu.be/uz_gxz3whS0?t=197)**


### AuthZed Powers OpenAI's Data Connectors


[OpenAI](https://authzed.com/customers/openai) securely connects enterprise knowledge with ChatGPT by using AuthZed to handle permissions for their corporate data connectors - when ChatGPT connects to your company's Google Drive or SharePoint. They've built connectors to process and search over **37 billion documents** for more than 5 million business users while respecting existing data permissions using AuthZed's authorization infrastructure.


This demonstrates how authorization infrastructure has become critical for AI systems that need to understand and respect complex organizational data permissions at massive scale.


## Technical Deep Dives and Customer Stories


### Real Talk: The Dual-Write Problem


**Artie Shevchenko from Canva** delivered an excellent explanation of the dual-write problem that many authorization teams face. Anyone who has tried to keep data consistent between two different databases (such as your main database + SpiceDB) will recognize this challenge. **[Watch Artie's full talk](https://youtu.be/uz_gxz3whS0?t=1715)**


Artie was direct about the reality: the dual-write problem is hard. Here's what teams need to understand:


**Things Will Go Wrong**


- Network calls fail between your database writes
- Race conditions happen when multiple requests hit at once
- Backfill processes create their own special category of chaos


**Four Ways to Deal With It**


1. **Sync jobs** - Run periodic cleanup to fix inconsistencies. Expensive but reliable.
2. **Micro-syncs** - Target specific relationships when they change. Faster than full syncs.
3. **Version fields** - Add versioning to prevent overwriting newer data. Complex but prevents races.
4. **FIFO queues** - Process everything in order. Simple but doesn't scale well.


Canva uses sync jobs as their safety net. Artie's team found that most inconsistencies actually came from bugs in their replication logic, not from the network problems everyone worries about. The sync jobs caught everything and gave them visibility into what was actually happening.


**The Real Lesson** : Don't try to be clever. Pick an approach, implement it well, and have monitoring so you know when things break.


### How Turo Built Authorization That Actually Works


**Andre Sanches from Turo** told the story of how they moved from "just share your password with your employees" to accurate fine-grained access controls. **[Watch Andre's talk](https://youtu.be/uz_gxz3whS0?t=4247)**


**The Problem Was Real** Turo hosts were sharing account credentials with their team members. Fleet owners needed help managing vehicles, but Turo's permission system only understood "you own it or you don't." This created significant security challenges.


**The Solution Was Surprisingly Straightforward** Andre's team built a relationship-based permission system using SpiceDB that supports:


- Teams with admin and member roles
- Fine-grained permissions (who can message guests vs. who can see finances)
- Vehicle-level access controls
- Support for pending team invitations


The best part? When they needed to add support for inactive team members late in development, it was literally a one-line schema change. This exemplifies the utility of SpiceDB schemas and authorization as infrastructure.


**Two Years Later** Turo has had exactly one incident with their AuthZed Dedicated deployment in over two years - and that lasted 38 minutes. Andre made it clear: letting AuthZed handle the infrastructure complexity was absolutely worth it. His team focuses on building features, not babysitting databases.


### Database Philosophy and Spicy Takes


**Professor Andy Pavlo from Carnegie Mellon** joined our co-founder **Jimmy Zelinskie** for a chat about databases, AI, and why new data models keep trying to kill SQL. **[Watch the fireside chat](https://youtu.be/uz_gxz3whS0?t=6707)**


**The SQL Cycle** Andy's been watching this pattern for decades:


1. Someone announces SQL is dead and their new data model is the future
2. Everyone gets excited about the revolutionary approach
3. Turns out the new thing solves some problems but creates others
4. SQL absorbs the useful parts and keeps trucking


Vector databases? Being absorbed into PostgreSQL. Graph databases? SQL 2024 added property graph queries. NoSQL? Most of those companies quietly added SQL interfaces.


**The Spiciest Take** Jimmy dropped this one: "The PostgreSQL wire protocol needs to die."


His argument: Everyone keeps reimplementing PostgreSQL compatibility thinking they'll get all the client library benefits for free. But what actually happens is you inherit all the complexity of working around a pretty terrible wire protocol, and you never know how far down the rabbit hole you'll need to go.


Andy agreed it's terrible, but pointed out there's not enough incentive for anyone to build something better. Classic tech industry problem.


**AI and Databases** They both agreed that current AI hardware isn't radically different from traditional computer architecture - it's just specialized accelerators. The real revolution will come from new hardware designs that change how we think about data processing entirely.


## Sneak Peeks from the AuthZed Lab


### PostgreSQL Foreign Data Wrapper


**Joey Schorr (our CTO)** showed off something that made me genuinely excited: a way to make SpiceDB look like regular PostgreSQL tables. **[Watch Joey's demo](https://youtu.be/uz_gxz3whS0?t=3461)**


You can literally write SQL like this:


sql


1


2


3


4


```text
SELECT    *    FROM   documents
JOIN   permissions  ON   documents.id  =   permissions.resource_id
WHERE   permissions.subject_id  =    'user:jerry'    AND   permissions.permission  =    'view'
ORDER    BY   documents.title  DESC  ;


```


sql


1


2


3


4


```text
SELECT    *    FROM   documents
JOIN   permissions  ON   documents.id  =   permissions.resource_id
ORDER    BY   documents.title  DESC  ;


```


The foreign data wrapper handles the SpiceDB API calls behind the scenes, and PostgreSQL's query planner figures out the optimal way to fetch the data. Authorization-aware queries become just... queries.


### AuthZed Materialize Gets Real


**Victor Roldán Betancort** demonstrated AuthZed Materialize, which precomputes complex permission decisions so SpiceDB doesn't have to traverse complex relationship graphs in real-time. **[Watch Victor's demo](https://youtu.be/uz_gxz3whS0?t=5940)**


The demo showed streaming permission updates into DuckDB, then running SQL queries against the materialized permission sets. This creates a real-time index of who can access what, without the performance penalty of traversing permission hierarchies on every query.


### Authorization and MCP Servers


**Sam Kim** talked about authorization for Model Context Protocol servers and released a reference implementation for a MCP server with fine-grained authorization support build in. **[Watch Sam's MCP talk](https://youtu.be/uz_gxz3whS0?t=9640)**


The key insight: if you don't build official MCP servers for your APIs, someone else will. And you probably won't like how they handle authorization. Better to get ahead of it with proper access controls baked in.


## What We're Thinking About


**Irit Goihman (our VP of Engineering)** shared some thoughts on how we approach building software. **[Watch Irit's insights](https://youtu.be/uz_gxz3whS0?t=9139)**


- **Bottom-up innovation** : Engineers who talk to customers and operate what they build make better decisions
- **Responsible AI adoption** : We use AI tools extensively, but with humans in the loop and measurable outcomes
- **Test coverage through AI** : AI-generated test cases with human review have significantly improved our coverage


Remote-first engineering teams need different approaches to knowledge sharing and innovation.


## Community Love


We recognized the contributors who make SpiceDB a thriving open source project. The community response has been exceptional:


**Core SpiceDB Contributors** :


- **Kartikay Saxena** - Student contributor who's been consistently improving the codebase
- **Braden Groom** from Reddit - Bringing real-world production experience back to the project
- **Jesse White** from RELiON - Infrastructure and reliability improvements
- **Sean Bryant** from GitHub - Core functionality enhancements
- **Nicolas Barbey** from leboncoin - International perspective and contributions
- **Chris Kellendonk** from PagerDuty - Monitoring and observability improvements
- **Lex Cao** - Performance and optimization work
- **Meyazhagan** - Documentation and developer experience improvements


**Client Library Heroes** (making SpiceDB accessible everywhere):


- **Danh Tran Thanh** - AuthZed-codegen for type-safe Go code generation
- **Michael Tanczos** - SpiceDB.net bringing authorization to the .NET ecosystem
- **Shubham Gupta** - AuthZed_ex for Elixir developers
- **Ioannis Canellos** - Quarkus-AuthZed-client for Java/Quarkus apps
- **Lauren (Lurian)** - SpiceDB-rust client for the Rust community
- **Thomas Richner** - SpiceGen for Java client generation
- **David Alsbury & Michael O'Connell** - Chipotle-rest PHP client (amazing name)
- **Link Orb team** - Both spicedb-php HTTP client and spicedb-bundle Symfony integration


**Community Tooling Builders** (the ecosystem enablers):


- **Mohd Ejaz Siddiqui** - SpiceDB UI for visual management
- **Chris Roemmich & Eytan Hanig** - SpiceDB operator Helm charts for Kubernetes
- **Nicole Hubbard/Infratographer** - Permissions API service layer
- **Guilherme Cassolato** from Red Hat - Authorino-SpiceDB integration
- **Thomas Darimont** - OPA-SpiceDB experiments bridging policy engines
- **Dominik Guhr** from INNOQ - Keycloak-SpiceDB event listener
- **Chip** - VS Code syntax highlighting for .zed files
- **Mike Leone** - Tree-sitter grammar for AuthZed schema


Every single one of these folks saw a gap and decided to fill it. That's what makes open source communities amazing.


## Looking Back, Looking Forward


Five years ago, application authorization was often something that was DIY and hard to scale. Today, companies are processing billions of permission checks through purpose-built infrastructure.


The next five years? AI agents are going to need authorization systems that don't exist yet. Real-time permission materialization will become table stakes. Integration with existing databases will get so seamless you won't think about it.


## Key Takeaways


If you take anything away from our fifth birthday celebration, let it be this:


1. **Managed authorization infrastructure** lets you focus on building features instead of managing database operations
2. **Relationship-based access control** can express complex permissions elegantly instead of trying to force everything into roles
3. **Community-driven development** makes everyone's authorization better
4. **AI and authorization** are going to become inseparable as AI agents are given access to more business data


Authorization infrastructure has gone from "development requirement" to "strategic advantage." The companies that figure this out first will have a significant edge in keeping pace with quickening development cycles and heightene security needs.


Thanks to everyone who joined AuthZed for the celebration, and here's to the next five years of fixing access control for everyone.


---


Want to try AuthZed Cloud?[Sign up here](https://authzed.com/cloud) and get started in minutes.


Join our community on[Discord](https://discord.gg/authzed) and star[SpiceDB on GitHub](https://github.com/authzed/spicedb) .


On this page


- The Big News
- AuthZed Cloud is Live
- AuthZed Powers OpenAI's Data Connectors
- Technical Deep Dives and Customer Stories
- Real Talk: The Dual-Write Problem
- How Turo Built Authorization That Actually Works
- Database Philosophy and Spicy Takes
- Sneak Peeks from the AuthZed Lab
- PostgreSQL Foreign Data Wrapper
- AuthZed Materialize Gets Real
- Authorization and MCP Servers
- What We're Thinking About
- Community Love
- Looking Back, Looking Forward
- Key Takeaways


## Related


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)


[Engineering The Culture Panel: Inside AuthZed's Engineering Interview Process Part 3 of our interview series: Learn how AuthZed evaluates collaboration, ownership, and cultural fit in engineering candidates. Discover what we're looking for beyond technical skills and how to prepare for behavioral questions that matter. May 19, 2026 · 4 min](https://authzed.com/blog/the-culture-panel-authzed-engineering-interview-process)[Engineering The Culture Panel: Inside AuthZed's Engineering Interview Process Part 3 of our interview series: Learn how AuthZed evaluates collaboration, ownership, and cultural fit in engineering candidates. Discover what we're looking for beyond technical skills and how to prepare for behavioral questions that matter. Irit Goihman · May 19, 2026 · 4 min](https://authzed.com/blog/the-culture-panel-authzed-engineering-interview-process)


[Engineering The Technical Skills Panel: Inside AuthZed's Engineering Interview Process Part 2 of our interview series: Discover how AuthZed evaluates your ability to design, debug, and reason about real systems. Learn what to expect from our technical skills panel and how to showcase your architectural thinking. Apr 3, 2026 · 4 min](https://authzed.com/blog/the-technical-skills-panel-authzed-engineering-interview-process)[Engineering The Technical Skills Panel: Inside AuthZed's Engineering Interview Process Part 2 of our interview series: Discover how AuthZed evaluates your ability to design, debug, and reason about real systems. Learn what to expect from our technical skills panel and how to showcase your architectural thinking. Irit Goihman · Apr 3, 2026 · 4 min](https://authzed.com/blog/the-technical-skills-panel-authzed-engineering-interview-process)
