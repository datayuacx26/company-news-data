---
schema_version: "1.0.0"
document_id: "4c7a3ca53e3f8f3461f18dcfe1062bd26598defd871cfac6d97fb3df338e26f1"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/one-word-changed-everything-the-origin-story-of-supabase"
published_at: "2026-03-23T14:32:31+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:fea19ffc79192bf9952d97ce087d96a7ff2c68c74c5be3723b7e90cf3c54abf7"
---

# One Word Changed Everything: The Origin Story of Supabase

## Eight Databases


### Eight Databases


It's April 2020. The world is in lockdown. Paul Copplestone sits somewhere in Singapore, in front of a screen that is telling him something he does not want to see.


Eight databases.


That's all. After four months of building — after quitting his last startup, after convincing his closest friend to bet his career on this, after raising $100,000 from angels who believed the pitch — Supabase has eight hosted databases in production.


Not eight hundred. Not eighty. Eight.


He stares at the number long enough that it stops looking like a number. He knows the product works. He knows the problem is real. He built it to solve a problem he lived through himself, a problem that left him so frustrated he open-sourced a workaround just to see if anyone else cared. Someone on Hacker News gave it 62 upvotes and 21 comments. That wasn't nothing. That was something. But the actual product — the thing with the dashboard and the hosting and the instant Postgres database — is being used by eight people.


He opens the website.


He looks at the tagline: *"Real-time Postgres."*


Technically, it's perfect. Technically, it describes exactly what Supabase does. But Copplestone has been coding long enough to know that technically perfect is the enemy of immediately understood. Real-time Postgres tells you what the product is. It tells you nothing about what the product is *for* .


He thinks about the actual problem. Not the abstract, architect-level problem. The felt problem. The one that woke him up at 3am in a past life, staring at Firebase error logs.


He types five words into the headline field.


*The open-source Firebase alternative.*


He hits save. Pushes it live. Goes to bed.


Three days later: eight hundred databases.


That is not a typo. Eight to eight hundred in seventy-two hours. A 100x increase from five words and a reframe. Every developer who had ever screamed at Firebase's rate limits, its opaque pricing, its proprietary NoSQL that handcuffed your data model — every one of them saw the tagline and thought: *finally* .


Paul Copplestone had not launched a new product. He had named a frustration that already existed. And a community that had been waiting for someone to name it came running.


---


## A Farm in New Zealand and a Penthouse in Singapore


### A Farm in New Zealand, a Penthouse in Singapore, and the Thing They Were Both Looking For


**Paul Copplestone was not supposed to be a founder.**


He grew up on a farm near Kaikoura, on New Zealand's South Island — the kind of place where the nearest town is small and the sky is enormous. His father ran the land. There was no startup culture. No venture capital mythology. No path that went from rural New Zealand to building a database company worth $5 billion. He just started coding, the way curious people on farms sometimes do, because the software was there and the question was: *what can you make it do?*


He left school at 18 and started contracting while studying at university. He was good at it. Good enough to get hired after graduation by a hedge fund in Australia, then Accenture — the kind of career path that looked like success from the outside and felt, from the inside, like walking in the wrong direction. Corporate rhythms. Project management. Process over product.


He didn't stay long.


A phone call changed the trajectory. He was waiting on an Australian government contract clearance that was taking months — the bureaucratic purgatory that eats years of capable people's lives — when a friend mentioned a startup forming in Malaysia. Copplestone flew to Kuala Lumpur and co-founded ServisHero in 2015, a marketplace connecting homeowners with service providers across Southeast Asia. It was his first real startup. He learned what it meant to build something from scratch, to watch users arrive and leave and arrive again, to be the CTO of something that had nothing when you started and had to become everything.


ServisHero worked. It became one of Southeast Asia's larger platforms in its category. But Copplestone's eyes had already moved ahead.


He came to Singapore for the Entrepreneur First program — one of those accelerators that operates less like a school and more like a pressure chamber, assembling roughly a hundred founders-in-waiting and watching what crystallizes. He didn't stay in the program. But he stayed in Singapore. And while he was there, he met Ant Wilson.


**Ant Wilson was from Liverpool.**


Not startup Liverpool — not the version of the city that exists in press releases. The Liverpool where nobody told you that starting a company was a real thing you could do. He earned an MSc in software engineering from Imperial College London, which is where you go when you're good at math and software and need the world to know it. Then he spent a decade attempting things. Three venture-backed startups, with "varying levels of success" — which is the polite way of saying: he learned a great deal at considerable cost.


What he learned was this: building backend infrastructure is expensive, painful, and almost always undervalued until the moment it fails. Every one of his startups had to rebuild the same things from scratch — authentication, database wiring, APIs, storage. The same boilerplate, repeated endlessly, while the actual product sat waiting.


He wasn't done trying. He just needed the right idea.


**They lived together in a penthouse in Orchard.**


Seven of them split the rent. Entrepreneur First participants sharing an apartment in one of Singapore's most expensive neighborhoods, which is what you do when the program covers housing and you'd rather have co-conspirators than square footage. Copplestone was there. Wilson was there. Every weekend, they'd gather in the living room and throw ideas at each other — the most random, the most ambitious, the most absurd, seeing what survived contact with actual humans.


Neither of them started a company together during the program. Copplestone left to build Nimbus for Work with a former ServisHero colleague. Wilson kept building. They stayed in each other's orbit the way people do when they've already passed the test of sharing a kitchen and still want to have the conversation.


**Nimbus for Work is where Supabase was born.**


Nimbus was building a workplace management platform. Chat, scheduling, the kind of internal tooling that mid-sized companies in Southeast Asia needed. For the chat functionality — the WhatsApp-like experience that users expected — Copplestone's team reached for the obvious tool: Google Firebase.


Firebase was fast to implement. The real-time sync was elegant. The authentication was pre-built. For a startup moving quickly, it made every sense.


And then it stopped making sense.


The specific moment that broke it: Firebase's Firestore imposed a rate limit of one write per document per second. For a chat application — where a single conversation could generate multiple messages per second — this was not a constraint you could engineer around. It was a wall. You could not go through it. You could not go around it. You could only find a different road.


Copplestone found his own workaround. He attached serverless functions to Firestore and replicated the data into a Postgres database on the side — a two-database architecture that was messy and expensive but functional. He watched the Postgres side handle what Firebase couldn't. He noticed how much more the Postgres side could do.


Then he asked the obvious question: why was Firebase the front door, exactly?


He started building a real-time engine for Postgres directly — using Postgres's own` LISTEN/NOTIFY` features, with Elixir handling the WebSocket connections to push changes to clients. It was the Firebase real-time feature, rebuilt on open-source infrastructure. No proprietary format. No rate limit. No single company owning the architecture.


He open-sourced it, posted it to Hacker News, and watched 62 people upvote it. Small number. But specific people. Developers who understood exactly what they were seeing.


That was the proof of concept. Not for the product — for the *problem* .


**He walked to a café. He sat down with Ant Wilson. He explained the idea.**


The pitch was simple: Firebase is the most popular backend-as-a-service for startups. Developers love the DX. They hate the lock-in, the pricing surprises, the NoSQL constraints, the rate limits. What if you built the same DX — instant database, authentication, real-time, storage — but on Postgres, fully open source, with your data always portable?


Wilson listened. He didn't take long to decide.


"One of those things that so obviously should exist in the world."


He joined as co-founder and CTO in January 2020.


---


## Building a Backend During the Pandemic


### Building the Backend of the World During the Pandemic


**January 2020. Two founders. A $100,000 pre-seed. A blank repository.**


The technical vision was unusually clear from the start:
- PostgreSQL as the core — not a custom database, not a fork, *actual Postgres* — because Postgres was the most trusted open-source relational database on earth and building on it meant inheriting 30 years of battle-testing
- Real-time subscriptions via[logical replication decoding](https://www.stacksync.com/blog/supabase-change-data-capture) . Postgres already had the mechanism; you just needed to decode the stream and push it to clients over WebSockets
- PostgREST for auto-generated APIs — an open-source project that turned your Postgres schema into a REST API automatically; they didn't build it, they *employed its maintainer*
- Authentication via Row Level Security — using Postgres's own security model rather than building a separate auth layer
- The whole thing open source, from the first commit


The engineering philosophy was anti-magic. During their YC application process, a Sequoia investor who had watched Parse — the Facebook-acquired backend platform — collapse under the weight of its own abstractions, told Copplestone: the danger with developer tools is when users can't understand what's happening inside. Supabase internalized this as a founding principle. No abstractions that you couldn't see through. No magic that became a prison.


**Then the world stopped.**


COVID-19 locked down Singapore in February and March. YC pivoted its entire Summer 2020 cohort to remote. The global developer workforce went home and sat down in front of their screens. For a company building remote-first developer infrastructure, the timing was, in the most darkly ironic sense, perfect.


Supabase was already distributed by design. Copplestone was in Singapore. Wilson was remote. Their team of three — including Steve Chavez, one of the primary maintainers of PostgREST, who they'd hired specifically because you should employ the people who maintain the tools you depend on — was scattered. The pandemic didn't change their working model. It validated it.


**By April, eight databases.**


They launched. Developers came, looked, and mostly left. The product worked. The marketing didn't. "Real-time Postgres" was a description for people who already understood the problem. It didn't carry the weight of the frustration.


The tagline change to "the open-source Firebase alternative" did what four months of building could not do in three days. Supabase hit the front page of Hacker News on May 27, 2020. The post earned 1,120 upvotes. One developer wrote what hundreds were thinking: *"I am ecstatic that someone is finally taking on Firebase...Firebase has no true competitor, let alone an open-source one."*


The team woke up to a Slack full of alerts. The free tier was being consumed in real-time. Their entire stack — still running as a Docker Compose application on a single server — held. It did not crash. It processed every request.


That was important. Supabase had just proved, under live fire, that it could handle the kind of traffic spike that would embarrass most infrastructure startups.


**Y Combinator, Summer 2020.**


The cohort was conducted entirely over Zoom. Demo Day was a video call. The famous dinners and hallway conversations and founder-to-founder collisions that defined the YC experience had been replaced by scheduled Zoom breakouts.


But the core of YC was still intact: Michael Seibel's advice cut through abstraction the way good advice always does. When Copplestone mentioned they were thinking about building authentication, Seibel's response was characteristically direct. Stop thinking. Ship it. Developers are asking for it. That's the signal. Don't have meetings about it. Build it.


They built it.


By the time they exited alpha in December 2020, Supabase hosted 3,100 databases. GitHub stars had climbed from under 500 in April to nearly 5,500 by year's end. Coatue led a $6 million seed round — with angels who notably had developer advocacy roles, not just finance roles. This was intentional. Supabase wanted their cap table to include people who would actually use the product, talk about the product, and understand what "developer trust" meant.


**The Rory Wilding Method.**


After a particularly sharp sign-up spike, Supabase's Head of Growth Rory Wilding did something that no growth playbook prescribes but every founder who has ever cared about their users eventually does: he spent an entire weekend manually going through more than 3,000 developer GitHub profiles who had signed up, and reached out to as many as possible to have a real conversation.


Not a survey. Not a Typeform. A conversation.


Who are you? What are you building? What's working? What isn't? What do you wish existed?


The answers went into Notion. Every piece of feedback — from GitHub issues, Twitter mentions, Discord messages, support emails — was tagged and tracked by feature area. The product roadmap was not a founder's vision imposed downward. It was a community's frustrations mapped upward.


This is how they learned that authentication was non-negotiable. That storage was more requested than anyone had expected. That the thing developers actually cared about, underneath all the features, was trust: trust that their data was portable, that the product would keep working, that Supabase wouldn't do to them what Firebase had done.


---


## When Developers Made It Theirs


### When Developers Made It Theirs


**March 2021. The First Launch Week.**


After YC, Copplestone and Wilson faced the question every accelerator graduate faces: how do you keep the momentum without the external structure? YC imposed a rhythm. Demo Day imposed a deadline. Without them, sprints become sprawl.


Their answer was to invent their own accelerator internally.


"We're just going to pretend we're starting the batch again."


They established three-month product cycles, each ending with a week of daily feature launches. Not a single announcement. Not a press release. One new thing, every day, for five days. Each announcement paired with a blog post written by the engineer who built the feature — because only the person who built it understood the problem well enough to explain it to the people who had the problem.


The first Launch Week shipped seven features in five days:
- Pricing (Supabase had been free; they introduced paid tiers positioned explicitly against Firebase's unpredictable billing)
- Storage (S3-backed file storage with Postgres as middleware — the fourth pillar of the Firebase stack they were dismantling)
- CLI for local development
- Open-sourced UI components
- Serverless Functions (a Firebase Cloud Functions alternative)


The community response was immediate. Not just usage — *participation* . Developers started submitting features, writing client libraries in languages the team hadn't even planned for (TypeScript support came from a community contributor), and posting their own Supabase tutorials without anyone asking them to.


This is when it became clear that something different was happening. Most developer tools have users. Supabase was developing advocates.


**The SupaSquad.**


The SupaSquad was not a marketing program. It emerged from something simpler: some people in the Discord and on GitHub were doing the work of support and evangelism not because they were paid but because they found it meaningful. A retired IBM engineer in his sixties became the primary source of support in Discord — answering questions faster than the Supabase team, with more patience, at all hours.


Supabase's response was not to hire him. It was to formalize the loose relationship, acknowledge the contribution publicly, and build a structure that let people like him keep doing what they were already doing. The SupaSquad gave identity and infrastructure to the community's natural leaders without extracting them from the community.


Paul Copplestone personally monitored all mentions of Supabase online. When a developer tweeted about a missing feature, the frontend team had a fix live within twelve hours. Not every time. But often enough that the community believed Supabase was actually listening — because Supabase was actually listening.


**The GitHub explosion.**


In April 2020, Supabase had fewer than 500 GitHub stars. By June 2020, 1,500. By December 2020, nearly 5,500. By the time they raised their Series A in September 2021, they were one of the fastest-growing open-source projects on GitHub — fewer than 350 projects in GitHub's entire history had reached 30,000 stars at comparable speed.


The GitHub star is a nearly meaningless metric on its own. But as a signal of developer trust and attention, it is hard to fake. You cannot buy it, not really. You cannot campaign for it in the way you can campaign for press. A developer stars a repository because they want to remember it, or because they want their followers to see them associating with it, or because the project represents something they care about.


Supabase's stars were the sound of a community saying: *this one is ours.*


**The Series A, and what it proved.**


September 2021. $30 million. Coatue led again. They came to the round with 50,000 total databases created — sixteen times the number they'd had nine months earlier. The investor note would have said: strong growth. The actual proof was more specific.


Supabase's users were not churning. Databases were not being abandoned. The growth was not the meaningless kind — sign-ups that arrive on the Hacker News spike and leave when the novelty fades. It was compounding in the way that happens when a product becomes the default choice for a generation of developers starting new projects.


The database is, as Copplestone understood from the beginning, the most sticky layer of any application. Developers don't migrate databases. They build on them and stay. Every developer who chose Supabase for a new project was, with extremely high probability, a Supabase customer for that project's lifetime.


Forty percent of Y Combinator's most recent batch was building on Supabase.


That was not a coincidence. That was a generation.


---


## The Default Backend


### The Default Backend


**The company that Copplestone had imagined fifteen years before he built it.**


Among the details that emerged from his interviews: Copplestone had sketched a concept remarkably close to Supabase roughly fifteen years before founding it. The idea wasn't fully formed. The timing was wrong. He filed it in the mental archive of things that weren't ready yet and moved on.


When the conditions finally aligned — the maturity of PostgreSQL's replication features, the emergence of PostgREST, his own firsthand experience of Firebase's failures, the meeting with Ant Wilson — the idea came back, and this time, everything was ready.


This is not an unusual story in technology. Most category-defining companies are built by people who saw the shape of the thing long before the moment existed to build it. The founders of Google thought about search engines before search engines were good. The founders of Stripe thought about internet payments before the infrastructure existed to make them simple. Copplestone had thought about this problem in an earlier era of the web, walked away because the tools weren't there, and returned when they were.


**Why PostgreSQL and not something new.**


The question was asked of Copplestone many times, because it seems counterintuitive. Startups are supposed to build new things. Supabase's centerpiece was a 30-year-old open-source database. Why not build a new database? Why not create something proprietary, something that would compound into a technical moat?


His answer was consistent and philosophically clear: "If our platform is not the best, then you should be able to take your data and take it to any other Postgres provider." That portability is the same idea behind a real-time[two-way sync](https://www.stacksync.com/two-way-sync) , where your data stays yours and still moves between systems as it changes.


This is not a competitive concession. It is a competitive stance. Copplestone understood that developer trust, once broken, is nearly impossible to rebuild. Proprietary formats are promises with fine print. Open source is a promise you can actually read. It is also why teams now run a[real-time, bi-directional Supabase Postgres integration](https://www.stacksync.com/blog/supabase-postgresql-integration-real-time-bi-directional-sync) with the rest of their stack instead of exporting files.


More practically: PostgreSQL had 30 years of hardened code, a global community of contributors, and an extensibility model that let Supabase build on top of it without forking it. They contributed improvements back upstream. They hired the maintainers of the tools they depended on. When they needed GraphQL, instead of adding a separate server, they built it as a PostgreSQL extension — the resolvers living inside the database itself, eliminating N+1 query problems at the architectural level.


The choice of Postgres was not conservatism. It was the decision to build on the most solid foundation available and compound on top of it rather than spend energy maintaining a foundation of their own.


**The numbers that followed.**


-


*December 2020:* $6M seed. 3,100 databases. 5,500 GitHub stars.


-


*September 2021:* $30M Series A. 50,000 databases. 30,000+ GitHub stars.


-


*May 2022:* $80M Series B, led by Felicis. 100,000 databases. 5 million developers.


-


*September 2024:* $80M Series C. 65,000+ GitHub stars. 7 million+ registered developers.


-


*April 2025:* $200M Series D. $2 billion valuation.


-


*October 2025:* $5 billion valuation. 98,000+ GitHub stars. 2,500 new databases launched every single day.


-


*2025 ARR:* $70 million, growing at 250% year-over-year.


Wells Fargo. Coinbase. Uber. Microsoft. Netflix. Google. Shopify.


These are the companies now running on a database platform that had eight users in April 2020.


**The "egoless" culture.**


One of the things journalists found when they came to write about Supabase was that it was hard to find the drama. No co-founder fights. No pivot wars. No moment where the company nearly died and was saved by a heroic speech. There was the eight-database moment, which was a crisis of positioning rather than survival. There was the infrastructure scramble when Hacker News sent more traffic than DigitalOcean's limits could absorb. But the arc of Supabase is unusually clean.


Copplestone, when asked about this, pointed to the hiring philosophy: they recruited people who already had the characteristics they valued — low ego, intrinsic motivation, focus, quality standards — rather than training for them. A globally distributed team across 30+ countries, assembled through what Paul called a "Moneyball" approach: instead of competing for Bay Area talent at Bay Area prices, go find the exceptional engineers who happen to live in Peru, or Spain, or New Zealand, or Liverpool, who are extraordinary and available and not being bid on by fifteen other companies.


The team could not be poached. They were too distributed, too global, too attached to a culture they'd built together across time zones.


**What Supabase actually is now.**


It is no longer just the Firebase alternative. The tagline that launched it in 2020 has been retired in favor of something more ambitious: "Build in a weekend. Scale to millions." The product has outgrown the comparison it was born in.


Supabase is, increasingly, the infrastructure layer for AI applications. The emergence of vector databases for LLM memory and retrieval fit Supabase's architecture naturally, with PostgreSQL and the` pgvector` extension becoming one of the most popular substrates for AI builders. When OpenAI needed to recommend a database for ChatGPT plugin developers, Supabase was in the conversation. Beyond AI workloads, most product teams also need that database to talk to their other tools, which is why many[connect Supabase to tools like HubSpot](https://www.stacksync.com/blog/how-to-sync-hubspot-and-supabase-in-minutes-with-stacksync) .


Copplestone articulated a three-phase vision:
1. Scalability — every project provisioned as a full PostgreSQL instance, not a schema, not a sandbox
2. Ease of use — authentication, storage, APIs, all the Firebase things, but on Postgres
3. Cloud-native PostgreSQL — database branching, distributed compute, ephemeral instances, the infrastructure primitives that the next generation of applications will require


They are in phase three now, building the things that don't have names yet because the applications that will use them haven't been written yet.


---


## The Tagline He Changed in the Dark


There is a version of this story that focuses on the funding, the valuation, the GitHub stars, the developer community that grew into one of the largest in the history of open source.


But the version that matters is smaller.


A developer in Singapore, in April 2020, staring at eight databases. The world in lockdown. A co-founder somewhere. An idea that worked technically but wasn't being seen. And one moment of clarity — not about the product, about the language — that turned eight into eight hundred.


Everything after that was real work. Real engineering. Real community. Real relationships built one Discord message and one GitHub issue and one Launch Week at a time. The Hacker News upvotes were real. The 98,000 GitHub stars are real. The $5 billion valuation is real.


But the seed was five words.


*The open-source Firebase alternative.*


Paul Copplestone didn't invent a new technology. He named a frustration that every developer who had ever touched Firebase already carried. He named it clearly, in five words, and he opened a door.


The developers who walked through it didn't need to be told twice.


---
