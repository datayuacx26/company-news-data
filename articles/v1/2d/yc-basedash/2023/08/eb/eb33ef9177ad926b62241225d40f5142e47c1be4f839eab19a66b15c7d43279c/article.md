---
schema_version: "1.0.0"
document_id: "eb33ef9177ad926b62241225d40f5142e47c1be4f839eab19a66b15c7d43279c"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-supabase-became-this-generation-s-database/"
published_at: "2023-08-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:3baabcb75c5900cfef115640c5c63a4e3483ce2e9430256cf24d375adc25557c"
---

# How Supabase became this generation’s database

Each generation of software has corresponded with a different database provider. It started with Oracle and Ingres who battled for supremacy in the 1980s. Next came MongoDB in the 2000s, then Firebase and Parse in the 2010s.


Each company started by targeting developers but would eventually adapt their products for larger companies. This alienated younger developers who had different priorities and tastes, which in turn left room for a new product to enter the picture.


This raises an interesting question: what is this generation’s database?


To me, the answer is Supabase. Developers increasingly turn to it before anything else when spinning up a new project. Beyond just a database, Supabase also gives developers authentication, storage, edge functions, and instant APIs. All of this is perfectly encapsulated in Supabase’s tagline - “Build in a weekend. Scale to millions.”


This is huge for the state of programming. I was in YC with Supabase and was always curious about how they became so successful, so I spent a weekend learning everything I could about the company. Here’s what I found.


### **Founding Story**


The story of Supabase starts with Paul Copplestone.


At the time, Paul was the CTO of a startup called Nimbus for Work, an office management platform and service provider. To streamline operations, he built a chat application using Firebase, but he quickly ran into a major problem – users weren’t seeing the responses until they refreshed their webpage.


It turned out that Firebase had a limit of one query per second for each document. It wasn’t a hard-blocker, but Paul had to re-engineer everything around this new limitation. So, he went to investigate if he could rebuild the same product with Postgres.


Firebase had one killer feature that Postgres didn’t: real-time systems.[In an interview](https://www.primary.vc/firstedition/posts/supabase-s-paul-copplestone-on-working-to-create-the-world-s-most-productive-developer-platform) , Paul said, “With Firebase, you can insert data and you can have many devices listening to the database, and they’ll receive those messages. Postgres doesn’t have that built in. So I ended up building a real-time engine, open-sourcing it, and I put it on Hacker News.”


It turned out that others had the same problem. The Show HN for Paul’s real-time Postgres had 62 upvotes and 21 comments.


More importantly, even after the initial Show HN, Paul’s open-source repo continued to gain traction.


Buoyed by the continued success, he pitched the idea to his friend Ant Wilson a couple of months later. They had previously lived together in Singapore and got along super well: as Paul put it, “we’re very aligned in the way we think.”


The feeling was mutual. Ant’s anecdote about Paul was that they would have a long discussion about an idea. After the discussion, most people would take a break and maybe go for food. Not Paul, though: he’d open his laptop and start coding.


So together, they started Supabase in January 2020. The initial vision was to create a product out of Paul’s MVP, and they pitched Supabase as “real-time Postgres.”


### **Finding Product-Market Fit (PMF)**


One of the constants in Supabase’s history is that they move super quickly. Within a few months of writing their first line of code, Paul and Ant had raised around $100,000 from angels and joined YC.


However, Paul’s early HN success and investor validation didn’t translate to real usage. At the end of April, Supabase still only had 8 hosted databases.


But they were much closer to product-market fit than they realized at the time. Paul had the idea to change the tagline on the website from “Real-time Postgres” to “The Open-Source Firebase Alternative.” Both Paul and Ant were huge fans of Firebase. In particular, they admired how Firebase empowered a generation of developers to spin up a new project in hours rather than weeks.


The Supabase team wanted to emulate that experience. So as Paul browsed through HN, an idea came to him to try out this new positioning. “I know that people are frustrated with Firebase, and most of the tooling that we’re building is to replace Firebase.”


That turned out to be the push that Supabase needed to find PMF.


At the end of May, a user posted a link to Supabase on HN with the title: “[Supabase (YC S20) – An open source Firebase alternative](https://news.ycombinator.com/item?id=23319901) .” This time, the product really struck a chord with developers. When the post went to the top, dang (HN’s mod) just said, “Well, this can be your Launch HN (specific to YC companies) now.” Over 1,120 people upvoted the post, and it became the second most popular[Launch HN](https://news.ycombinator.com/yli.html) in HackerNews history.


The top comment on the post included, “I am ecstatic that someone is finally taking on Firebase. As a Firebase user, I find it invaluable… Following this intently, because Firebase has no true competitor, let alone an open source one. Nice work so far.”


Seeing the response, the Supabase team decided to use the opportunity to launch to the world.[Their Product Hunt launch](https://www.producthunt.com/products/supabase#supabase-alpha) became the 3rd most upvoted product of the day with 818 upvotes.


Usage-wise, Supabase went from 8 hosted databases to 800 in 3 days, and stats-wise, their GitHub repo stars jumped from less than 500 in April to over 1,500 by June.


The counter-positioning against Firebase turned out to be a smart move. It gave Supabase the early traction necessary to get off the ground. More importantly, the success gave the team confidence to broaden their vision and build additional tooling on top of Postgres.


Crucially, instead of adding every feature that the community requested, Supabase maintained their focus on database-level problems. Instead of building features like website hosting, the team went to work on new features like Instant RESTful APIs and auto-documentation.


### **Scaling Up**


The wild attention from the new positioning also exposed some growing pains. Ant’s background was in systems, so the Supabase infra held up well. The problem was that the rapid growth hit service limits on DigitalOcean and Cloudflare. But with the fast pace of shipping, these limits were quickly addressed.


In the early days, Supabase shipped new updates every day and at the end of each month, they made a demo video to showcase the new features. Below are a couple of the early features / products shipped when the team was just Paul, Ant, and Steve Chavez, a maintainer of PostgREST:


- [April 2020](https://www.youtube.com/watch?v=ck5MM_PD4Co&embeds_referring_euri=https%3A%2F%2Fsupabase.com%2F&feature=emb_logo) (first update, so this represents progress until the end of April)


- Spin up Postgres in less than 2 minutes
- Auto-generated API (fetch, insert, delete using Supabase client)
- Query databases from Supabase’s dashboard
- Analyze queries (planning and execution time)


- [May 2020](https://www.youtube.com/watch?v=e4qXmcEFaUs)


- Onboarding guides with quick scripts + a sample starter project
- New UI
- Auto-generated documentation for APIs
- Table view for the database (with edit, sorting, and filters)


- [June 2020](https://www.youtube.com/watch?v=KH-tyOSW_04)


- Ability to select region for the database
- Migration from DigitalOcean to AWS


- The traffic from Supabase’s launch led to DigitalOcean production errors, credit limits, and general cloud limits. Supabase increased their Cloudflare subdomain limits and migrated to AWS t3a


- Relational database UX (click into new database within Supabase dashboard)
- Json editor + viewer
- Database backups
- Early version of Auth


- [July 2020](https://www.youtube.com/watch?v=_iJtMFTFfNA)


- Auth (users and policies – row-level security)


- This was the biggest feature of Supabase since launch. It’s hard to overstate the impact of it. From Paul’s own words, “it’s definitely our most requested feature.” In this release, Supabase provided user management, PostgresSQL’s[Row Level Security](https://www.postgresql.org/docs/current/ddl-rowsecurity.html) , policies with SQL rules, and built on open-source software.


- Updates to the table-view UI
- New Postgres extensions


- [August 2020](https://www.youtube.com/watch?v=O_ZwZa673bY) (6 months into building Supabase)


- Create table with data from dashboard
- Email templates + custom SMTP server for Auth
- Team collaboration within Supabase
- TypeScript support (contributed by community)


The breadth and depth which the Supabase team accomplished in their first 6 months is stunning. In between, Paul and Ant also went through YC S20 (June to August 2020) and focused primarily on a single feature during the batch – Auth. By the end of August, they had built a powerful auth solution to pair with their real-time Postgres database.


After YC, Supabase also saw the power of having something to rally around. They spent the next three months sprinting toward exiting Alpha and announced their Beta in December 2020.


The key metrics for exiting alpha were:


- Performance – Supabase was now 3x faster than Firestore for read and write
- Security – full Pen Test + worked with open-source partners to improve security
- Reliability – public facing uptime page for services and infra, the next incident ([front end hosting provider being down for 13 minutes](https://status.supabase.com/history?page=10) ) would take a full four months to appear


By the end of Supabase’s alpha, they had 3.1k hosted databases and nearly 5,500 GitHub Stars. Super far from the early days of only 8 databases.


Along with starting beta, Supabase also announced their $6 million seed round led by Coatue. What’s notable about that fundraise is that many of the[angels included in the round](https://supabase.com/blog/angels-of-supabase) held job titles focused on developer advocacy.


Instead of slowing down after such a long sprint, the Supabase team asked, “what’s the most ambitious thing we can hope to launch 3 months from now?”


That question led to Supabase’s philosophy of building for 3 months and then launching multiple features / products every day for a week in an event called Launch Week.


In Supabase’s[first Launch Week in March 2021,](https://supabase.com/blog/launch-week) they released:


- Pricing (up until now, Supabase was entirely free)


- Pricing was counter positioned against Firebase - unlimited API calls, no additional cost for auth etc.


- Storage (S3 bucket with API Server and Postgres as middleware)
- CLI (run Supabase locally)
- UI (open sourced their UI components)
- Workflows (with the goal of replacing Firebase Functions)


Their[second Launch Week](https://supabase.com/blog/supabase-launch-week-sql) was equally impressive with:


- Storage (V2, streaming media, public buckets, directory uploads etc.)
- Passwordless Logins
- Hooks (another part of replacing Firebase Functions)


A couple of launch weeks later, in September 2021, Supabase raised a $30 million Series A, again led by Coatue. Beyond the dollar figure, Supabase’s Series A had two main differences compared to their seed round. First, the angels in this round were more slanted to previous founders and professional investors such as Elad Gill and Tom Preston-Werner. Second, Supabase’s growth had rocketed up. They now had 50k databases created, up from 3k databases just 9 months ago (16x growth).


Databases created on Supabase over time


### **Building a company**


Supabase clearly ships new features quickly, which is surely one reason why they are already so popular with developers. But there’s another ingredient behind their success: an uncanny ability to figure out what to build, who to hire and how to generate attention.


**What to build**


Supabase is never in need of customer feedback — it just pours in from their community. Developers are opinionated and often unafraid to share their feelings about their favorite products.


Rather than trying to *get* user feedback, Supabase’s challenge is to figure out how to organize the avalanche of GitHub issues, Twitter and Discord messages, and feedback emails it receives. Despite how fast Supabase ships, they’re still a small team of 65 people. It would be impossible to follow up on all these distinct data points.


The solution that Supabase came up with is to funnel all of the feedback into Notion. Then, team members browse through the feedback and tag it with the corresponding feature / team.


So, when a team is trying to prioritize between two features or find the next product to build, they can look at the corresponding customer feedback and gauge the volume / intensity of the requests. Instead of being biased by recent feedback, Supabase can look at a historical index of what their users want.


One of the best examples of this was the fact that users consistently asked for Functions. It was never something that the Supabase team had intended to build at the start, but the community requests kept piling up. And so, they started incrementally building the product at the first Launch Week in March 2021. It would take another year before Supabase’s Edge Function product was fully launched.


Another example was adding GraphQL. Once again, this wasn’t something on Supabase’s roadmap and the team didn’t want to add another “heavy” server to the stack, but a few members of the community were insistent in their feedback. In the end, Supabase found a way to offer GraphQL as a Postgres extension, which turned out to be an extremely elegant solution. The[HackerNews community agreed](https://news.ycombinator.com/item?id=29430720) .


**Who to hire**


Paul and Ant live in Singapore. The company was founded during COVID. As a result, Supabase has always been remote-first.


Since both Paul and Ant were technical founders building a technical product for a technical audience, the[first ~50 people hired at Supabase were all technical](https://podcast.scalingdevtools.com/episodes/product-market-fit-is-one-pivot-away-with-ant-wilson-founder-of-supabase) . Many of these early hires were already well-known in the open source community before they joined the company. This meant that the first marketers and other non-technical employees were actually developers. Users loved this.


Beyond who to hire, Supabase faces the challenge of building multiple products as a small startup. To maintain their shipping cadence, Supabase is split into teams by product:


- Postgres / PostgREST
- Platform
- Auth
- Storage
- Realtime
- Functions
- CLI / API


Each of these teams is responsible for their own product. To ensure that teams aren’t siloed, Supabase also has multiple cross-functional teams that act more as consultancies than services.


- QA
- Docs
- Design
- Front End


The combination of deep vertical teams along with cross-cutting horizontal teams helps keep communication between teams super open. Everyone knows how their work contributes to overall company progress.


**How to generate attention**


Since Supabase’s first tagline change, traction has felt like a straight line for them. That makes it easy to overlook the smaller details behind Supabase’s marketing strategy, but you shouldn’t because they’re worth studying.


First, they never stopped experimenting with messaging. Today, Supabase’s home page still uses original copy of “Open-source Firebase” but the header is “Build in a week. Scale to millions.” In some ways, this represents the evolution of Supabase’s product. What started as real-time Postgres has become a full toolkit of products for developers to start their new project.


Second, they tried different marketing channels, including Instagram, TikTok, and Reddit. None of them performed as well as Twitter. In particular, nothing worked better than Twitter memes. That was perfect for Ant.


In a blog post, Paul wrote, “Anybody who knows Ant, knows that he[loves memes](https://supabase.com/blog/launch-week) .” This love extended to Supabase’s name. Originally meant as a placeholder, “\[Supabase\] wasn’t a strong contender, but it served a very important purpose - it was extremely ‘meme-able’ because it sounds similar to Nikki Minaj’s[Super Bass](https://youtu.be/4JipHEz53sU) .”


Third, a lot of Supabase’s marketing is aligned with their company strategy.


One insight for databases is that it’s a very sticky product. Many developers complain about their database, but rarely migrate to another provider. As a result, most of Supabase’s users come from spinning up a new project rather than migrations from Firebase.


To catch these new projects, Supabase’s marketing is entirely bottoms-up. They target developers rather than CTOs. Memes probably don’t do much for a Fortune 500 CTO or CIO (high ticket sales is one of the core competencies of legacy database companies like Oracle), but is the exact right language for the younger generation of developers.


All of this cumulates in an impression-driven marketing strategy. Rather than focusing on channels like SEO or sales, Supabase has been able to leverage their marketing on memes and events like their Launch Weeks to get branded traffic (people searching for Supabase specifically).


### What’s next?


One of the first signs that Supabase had a broader vision than simply replacing Firebase was their acquisition of Logflare in December 2021.


From their announcement, “A log platform might seem like a strange acquisition for Supabase - after all, there are no ‘Firebase Logs’. Logging is one of the most unappreciated parts of a tech stack… Database debuggers have a lot to learn from programmer tooling. We want to provide the best database developer experience in the world, and so we’re making debugging a first-class citizen.”


Investors and users agreed. In May 2022, Supabase raised an $80 million Series B led by Felicis ventures. They had doubled from the 50k databases 8 months ago to 100k databases created and in another three months (August 2022), that would become 150k databases created.


Paul wrote about Supabase’s vision during their Series A. The plan had three phrases:


- Start with scalability – every project is a full Postgres database
- Make it easy to use – Postgres tooling like APIs, auth, and file storage
- Cloud-native Postgres


- Branching
- Scalable storage
- Distributed database
- Ephemeral compute
- Backups/ snapshots


At the time, Supabase was still in phase two, making Postgres easy to use. Since then, the team added GraphQL support, enterprise features for Supabase Pro and Enterprise, Edge Functions for developers to execute Typescript code close to their users, and Supabase Realtime for developers to build collaborative multiplayer experiences. I told you, they ship super fast.


With Supabase’s Series B, they were now ready to chew through the features in phase 3 of the plan.


Fast-forward to today, Supabase is a lot closer to realizing their vision of a cloud-native Posgres. A developer using Supabase gets access to databases, authentication, storage, edge function, real-time client states, and even vector embeddings.


With the rise of AI, Supabase has been at the forefront of providing developers tools to get started. Their May 2023 feature roundup included Supabase Vector (open-source vector toolkit for Postgres) and ChatGPT plugins that support Postgres and Supabase.


Paul shared an updated vision in an interview: “We want to be the world’s most productive developer platform. We want to offer all the things that a developer needs to get started with a project. We want to make sure that when you start with us, you never see any limitations. The idea is that whatever you think is the best tech, that’s what we have provided. So we’ve given you the best tools, no matter what stage of your journey. The ultimate goal is to make sure that the developer can, essentially, build in a weekend and then scale to billions.”


Not only is Supabase well on its way to being this generation’s database company, but they seem to have their sights set on something bigger: a suite of tools for every developer to do their best work.


It’s crazy. But I think they’ll do it.
