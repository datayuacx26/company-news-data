---
schema_version: "1.0.0"
document_id: "9fdddab25276a4209f8ae18afbc5d05368f88f74a1ad9be182cce80da24f00b2"
company_key: "yc-shepherd"
company: "Shepherd"
source_id: "yc-shepherd-news-import-bb872756570a"
canonical_url: "https://shepherdinsurance.com/blog/back-to-the-monolith"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T19:29:23.349001+00:00"
fetched_at: "2026-07-31T19:29:24.427215+00:00"
content_hash: "sha256:82a15121ae4a2c6cd101d4c2f8edd1a59a2feaa6d42b39185e61b71a1e011a90"
---

# Back to the Monolith: AI Cost, Context, and our Changing Priorities

Most of Shepherd's architecture existed to compensate for our coordination limits, but AI-driven development removed that need to compensate. We merged our polyrepo into a monorepo, collapsed it into a single Next.js monolith, and cut our development cycle from nearly an hour to under 10 minutes.


For the last 5 years, Shepherd’s insurance platform operated on what had been industry best practice. We split our frontend and backend services into separate repositories so our teams could operate independently. Barring the occasional broken deploy, this process served us incredibly well. Shepherd is uniquely positioned: as a predominantly internal platform, we can move faster than a team shipping to millions of users. It does not let us be careless, our users are the underwriters pricing live submissions, and they notice everything. But we can take risks that allow us to invest deeply in developer efficiency.


### Why Monolith? What Changed?


Since I joined Shepherd, our biggest problem has been keeping the contract between our frontend and backend services in sync. We rarely shipped a breaking change, but when we did, it could cost us 10 or 15 minutes of unexpected downtime that hurt both our users and our engineers’ pride. We had floated the idea of a monorepo and shared types, but our cost estimate was almost entirely in engineer hours, blocked most by coordination amongst different teams. A single engineer never had enough time to put in the work and make it happen, and we couldn’t justify handing it to more than one.


And then the cost disappeared. As models improved, so did the amount of code Shepherd engineers were producing. The more code we produced, the more architectural context an agent had to carry, and the more important it became to preserve that context and minimize rot. An agent working across two repos, a GraphQL schema, and multiple service boundaries burns tokens and attention reconstructing how those services work together. With AI-assisted development, an incredibly demanding harness just takes iteration, and iteration is substantially cheaper than an engineer trying and failing to define every parameter up front. Through repeated testing and improvements to the harness, we found that colocating our code reduced context usage by up to 40% on the same task. That made reorganizing our code economically obvious, but we could take it even further with a monolith.


### Monorail: The Migration


Before the change, Shepherd’s platform consisted of two separate applications. Goldengate was our backend Node.js server on AWS that exposed GraphQL. Brooklyn was our frontend Next.js SPA on Vercel that communicated with it. Two repos, two deployments, two sets of infrastructure, and a schema contract between them that every feature had to cross.


Monorail was a single platform, a brand new third (and final) service bridging the gap between the two. We made an entirely new Next.js application on Vercel, backed by Neon Postgres, and started from scratch. We ran the whole thing in four beats, and the sequencing mattered more than any single decision. We would derisk with a smoke test, run in shadow mode, cut over in one move, then distribute the tail-end migration with AI-assisted tooling.


The process here was just as important to us as the end result. It was essential that an engineer shouldn’t need to know about the migration until Monorail was ready for them. We started off small, without a big rewrite, and ran a single smoke test. We imported a few pieces of the legacy code into the new application and made sure deployments work and all builds were green.


The strategy we settled on was to mount, not rewrite. Our Goldengate backend service got mounted inside Monorail. The entire GraphQL schema engine was served, completely unchanged, through an internal API route. Every API endpoint, every webhook, and every external service got its own Next.js route. Brooklyn’s frontend routes were programmatically imported by a script that allowed the rest of our team to keep developing, unaware of the work going on. The PR that brought Monorail to life consisted of the bare-bones skeleton of a Next.js app, 200 page files, and the script that traversed Brooklyn and generated them. Nearly every file was three lines long:


```text
// monorail/src/app/u/carriers/page.tsx
'use client'  ;
export   {   default   }   from     '@brooklyn/app/u/carriers/page'  ;
export   *   from     '@brooklyn/app/u/carriers/page'  ;
```


That’s it. Our migration has begun, and it brought us complete developer parity with a direct reference to the original source code. No code freeze needed just yet, no schema drift, no new process for our engineers.


Monorail went live in shadow mode on its own subdomain alongside production, which let us migrate essential services one at a time. The pieces that couldn’t simply be mounted — authentication, navigation, our Policy and Quote flows, document generation, webhooks, external services, and background jobs — were rebuilt under a microscope. We found and squashed bugs at every layer, from CSS quirks to payload size limits to document generation failures. Our migration spent weeks living as a preview environment while we built parity with production and gained confidence.


Only then did we cut over. We gave the company 10 days’ notice, planned, prepped, planned again, and made sure the to-do list was extensive. At the heart of it, the list was quite simple:


1. Update Vercel’s DNS to point at the new project
2. Migrate our RDS database into a fresh Neon database
3. Update environment variables


As far as skeletons in the closet go, we had significantly fewer than expected: a Lambda whose database URL took longer to update than we’d care to admit, a Gmail extension that broke after its settings seemingly disappeared into the Google Console ether, and a handful of Vercel environment variables that needed tweaking to point the client back at the server correctly. We gave a heads up to the office at 2:30 PM, started at 3, and by 6:30 we were out the door to go crack open champagne at our favorite French bistro around the corner.


Our users came in on Monday and were greeted with the exact same application they had been using on Friday. Once we put out the fires they found (and boy do our users know how to find everything we missed), what was left was the shim. Don’t forget, we still had a GraphQL server running inside our Next.js application, mounted exactly where we had put it, and it would have kept running there as long as we let it.


Being able to stop there was the biggest benefit of a migration shaped like this, because nothing else **needed** to happen. Even if we had stopped that Monday, we had already cut our deploy times and practically obliterated the chance that an unintentional breaking change could take down the platform, and no roadmap conversation could claw those wins back. But leaving the shim in place meant leaving the real prize on the table, and I wasn’t done. Shepherd was going to get a context-optimized monolith, and that meant addressing the long tail: ~600 GraphQL resolvers across 1,200 call sites.


My part in this post-migration world was to provide the rest of the team with the tooling needed to migrate those call sites seamlessly. We ran countless iterations on our smallest routes and pages, moving them over using best-practice Next.js development, and each one was scrutinized by me, Mo (our CTO), and my new best friend, 5.6 Sol.


The difficulty here was controlling scope and making sure our base case had only what was needed to migrate a page off GraphQL, turning that three-line stub above into something as simple as this:


```text
// monorail/src/app/u/carriers/page.tsx — after migration
import   { getCarriers }   from     'monorail/server/entities/organizations/services/carriers'  ;
import   CarriersView   from     './CarriersView'  ;


const   CarriersPage =   async   () => {
return     <  CarriersView     carriers  =  {await     getCarriers  ()} />  ;
};


export     default   CarriersPage;
```


No client-side GraphQL round trip and no schema layer. The page fetches its own data on the server right next to where it renders. Behind the scenes are countless wrappers handling caching, server actions, and client-side behavior, but our components were (for the most part) organized to accept data no matter where it came from, so changing from GraphQL to a function call was trivial.


After weeks of testing, including moving the first few examples by hand, our migration harness brought us to the same conclusion we’ve been coming to about AI-assisted development for years: the best harnesses lay out incremental steps, draw strict boundaries, and give examples of how to handle different situations. Our harness can make smaller, more incremental PRs than an engineer would ever have time for, and review gets easier because each step is clear: remove the GraphQL call in favor of a wrapper directly to the backend code, then move the backend logic out of the Goldengate repository and into the Monorail monolith. Easier said than done, but encouraging the agent to break this up into two, three, or even four PRs allows for easier incremental review and stronger testing.


The end result is a skill that allows an engineer to point Claude at a route and receive one or many PRs migrating the code off GraphQL and into a React Server Component. A migration this broad used to turn a single engineer into a martyr (this would have been me), but AI-assisted tooling lets us pass the skill to the engineering teams that know their domain better than anyone. They can run the skill, generate the code, and open the preview links to verify the behavior is unchanged.


### Preview Links


The headline of Monorail might be the monolith, but the piece that changed a Shepherd engineer’s daily life is the preview environment, and none of the AI-driven migration above would have been possible without it. Previously, our frontend service, Brooklyn, was the only part served by Vercel. Preview environments pointed directly at our sandbox and could not be paired with the corresponding backend work unless it had already been deployed. Now, every pull request, whether human or machine-generated, gets a full running copy of the product deployed in a Vercel preview environment, backed by its own Neon database branch, hidden behind real authentication.


Getting there took a deliberate infrastructure roadmap. We tackled auth first, then previews, then data. We consolidated our authentication onto Better Auth, replacing the fickle patchwork we had before and giving both users and agents access to every environment. Previews need data, so we leaned on Neon’s branching, giving every preview an isolated copy-on-write branch of the production database that spins up in seconds and cleans itself up automatically once it goes stale. That same branching killed our local DB-dump ritual. A 10-minute dump script feeding into a local database gave way to a script that hands an engineer a personal branch off production. At one point, we counted 239 live preview branches. That number would have incapacitated our old setup, and now it’s a typical Tuesday.


At Shepherd, this matters more than any performance statistic. An engineer can have multiple agents working in parallel, materializing every unit of work as a clickable preview link. Line-by-line diff reviews turn into walkthroughs of running software in a continuous feedback loop. We click the link, try the flow, review the code, suggest changes, and repeat. An engineer becomes an engineering orchestrator, and the preview link is the interface converting code that claims to work into software we can judge in 30 seconds. It also lets us share new features directly with users, which catches edge cases and bugs long before anything goes live.


None of this works without our monolith. One app means one preview captures the entire product. When our frontend, API layer, business logic, and data model lived in separate repositories with separate deploy pipelines, no single link could show you the truth.


### The Numbers


Our results were anything but subtle.


Our development cycle, the actual process from making a change to seeing it live, dropped from 45–60 minutes to under 10. That’s an entirely different lifestyle. Helpdesk tickets get fixed in minutes, not hours. An engineer who gets six shots a day at proving out an idea out in production works fundamentally differently from the engineer who gets 40.


Deployments themselves dropped from 15 minutes to less than 5. Once shipping got really cheap, people shipped. Engineers are deploying 30+ times a day.


Our platform performance followed shortly after. The load time for our worst-offending page, a claims view, fell from 56 seconds to 5, and quote pages went from 9–17 seconds to 3–5. Nobody planned performance sprints to get these numbers down; they came naturally, page by page, as routes came off GraphQL. Delete the hop, move a render off the client, and React Server Components make the page fast on their own.


Breaking changes stopped. An engineer no longer needs to write a backend PR to update the contract, deploy unused code, and then swap the frontend over. GraphQL changes can’t strand the frontend on an old contract anymore. A whole category of coordination bugs, the kind you can’t catch in local development, disappeared overnight.


### The Uncomfortable Lesson


Shepherd’s uncomfortable lesson this year is that a lot of our architectural complexity existed to compensate for our coordination limitations, and AI-driven development has changed those limits.


A monorepo, never mind a monolith, had been discussed long before I joined Shepherd. We split the systems apart because no single engineer could hold them all in their head. Agents can navigate more of that complexity, but they still pay for every repository, schema, and service boundary they have to reconstruct. We added contract layers so teams could trust each other and their changes; a monorepo with our fast test suite enforces that trust mechanically.


Monoliths are not the solution to every problem, and this certainly isn’t a suggestion to un-distribute every repository. Shepherd’s engineering team has already begun handing our pricing engine over to the Actuarial team in the form of a Python microservice. Given our 20-person team and largely internal-only platform, we are able to take risks and weigh decisions in a way that a team of a thousand couldn’t. We’re a focused team insuring commercial construction and energy, small enough that one codebase holds the entire product, with senior engineers overseeing fleets of agents. For an org shaped like ours, the questions we’ve recently asked ourselves are less and less about what we can split out and more and more about what we can consolidate.


Answering those questions honestly lets us take a risk that would have been nigh impossible a year ago. The payout has been compounding speed and the room to pay down debts the distributed-system tax never left us. These decisions came down to what it costs to coordinate across system boundaries, and AI repriced those costs. It’s worth checking the budget.
