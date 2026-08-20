---
schema_version: "1.0.0"
document_id: "c556b9d87c0490059077dd68be69091dc0e508b4670c282df0ac047b36ceb35b"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/navigating-complex-migrations-for-success"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-22T23:44:16.700638+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:7385b793b1ab70bafbb5ed898cdac75ec92890dedea779634b500909bcd7b091"
---

# Migrating Analytics Platforms Without The Chaos

Most teams put off migrating their analytics for one reason: it feels risky. A new platform to learn, historical data to move, dashboards to rebuild, and a thousand microdecisions that carry the risk of breaking something.


Our team at[Human37](https://www.human37.com/) has run hundreds of migrations. Everything from a few million events to billions of rows. We can tell you: the fear is almost always bigger than the migration itself. The teams that struggle aren’t the ones with the most data. They’re the ones who skip the planning.


We recently shared our migration playbook in a live session,[Navigating Complex Migrations for Success](https://amplitude.com/events/navigating-complex-migrations-for-success) . Human37 cofounder[Vincent Crochet](https://www.linkedin.com/in/crochetvincent?originalSubdomain=be) and I walked through the technical path, and Amplitude Technical Success Manager[Catherine Adeniran](https://www.linkedin.com/in/catherineadeniran/) shared how the two organizations work together during migration. Here are seven key takeaways to help you succeed in your transition.


Watch the full session[on demand](https://amplitude.com/events/navigating-complex-migrations-for-success) .


## 1. Lock in the "why" before you move a single event


This is the most overlooked, yet important, step. Lots of organizations migrate for the sake of migrating. They know they want the latest tool, but they don’t know why. Without a clear problem to solve, they can’t know if they solved it down the road.


Before anything else, answer two questions.


1. **What are you trying to do with your data?**
2. **What’s stopping you from doing it today?**


If the honest answer is that your current platform limits your team, or you believe another product is heading in a better direction, migration makes sense. Write those reasons down. It becomes the benchmark you later measure success against.


## 2. Treat your migration as a spring cleaning


Migration is more than just moving all your tracking and data from platform A to B. It’s a rare opportunity to clean up shop. Take it.


We run three inventories before starting any migration:


- **Complete a full asset inventory.** Record every app, website, subdomain, and data source you track. Teams are routinely surprised by how many forgotten or orphaned assets turn up. That one marketing domain everyone forgot, the cross-domain setup from two years ago. Decide what’s worth bringing along and leave behind what’s not.
- **Index every event.** Check your naming conventions, identify discrepancies across products, and spot events that should be merged into a single event. Decide what to bring or rework.
- **Align on your metrics.** When teams argue about “the right number,” it’s usually because nobody agreed on a common definition. Settle what “conversion rate” actually means now, so everyone can start clean. Amplitude’s verified metrics and shared metric library are built for exactly this. Define your metrics before you migrate, not after.


## 3. Bring as much data as you need, not as much as you can


Ask a team how much historical data they want to migrate, and the answer is almost always “all of it.”


But data comes with a cost, and old data incurs technical debt from day one. Instead, we ask customers: what couldn't you do without this historical data? Bring what your business needs for continuity, and be honest about the rest. Whatever you decide to migrate, you want 100% of it. No gaps, no silent data loss.


Catherine offered a practical way to prioritize in the webinar. Look at which dashboards and reports get the most engagement, migrate the data behind those first, and phase in the rest. Start with what drives the day-to-day business.


## 4. Track in parallel before switching anything off


Don’t turn off your old platform on the 31st and flip on the new one on the 1st.


Instead, run both in parallel for a set window of 30, 60, or 90 days. This does two things. It builds trust in the new numbers, and it gives you something to fall back on.


When your “5” in the old platform shows up as “5.1” in Amplitude, don’t panic. Small variations come from differences in how each platform measures. Your 5.1 isn’t an error; it’s your new baseline.


## 5. Build access and documentation from the beginning


Governance and documentation aren’t anyone’s favorite subjects, but they’re nonnegotiable. We recommend two habits that pay off every single time.


First, set up role-based access from the start. On migrations with 200+ users, establish clear roles, such as admin, manager, and member, with defined criteria for each. Doing so makes granting access easy and keeps things consistent at scale.


Second, create your onboarding documentation *during* the migration, not after. As you configure time zones, session settings, currencies, channel groupings, and metric definitions, document each choice. You’re the first one in your new platform; an army of analysts comes next. During our largest migrations, the documentation created early on served as the onboarding path, helping everyone get comfortable quickly.


## 6. Realize that the work doesn’t stop at go-live


Going live is a milestone, not the finish line. When migrating customers, we build a support program for the weeks that follow:


- **Establish a regular forum.** One customer branded their recurring forums “Synergy Meetings.” It’s a space for stakeholders to meet every two weeks to share findings, new features, and quirks. It builds a community around the platform and the onboarding journey.
- **Host office hours and a community channel.** The team at Human37 makes itself available to customers for drop-in help a couple of times a week. We also monitor a Slack or Teams channel for quick questions. Ensuring people can find you and easily get help builds confidence in the new platform.
- **Create a known-bug log.** Bugs are inevitable in any migration. Keep a public log, in Confluence, Notion, wherever, with each known issue and its ETA. When an analyst hits something odd, they can check the page instead of burning half a day on a problem you’re already aware of.


Governance is a forever thing. Treat it as an ongoing process. Assign a clear owner to the platform and its processes. Build in a regular check for unexpected events or properties. Platforms without owners decline fast.


## 7. Run two tracks in parallel


We typically split migrations into two simultaneous tracks.


### Live tracking


Build your tracking plan first. List your events, the properties on each, and the data type for every value. Get sign-off from both the business and engineering. Configure Amplitude to match, go live, and onboard your dev team.


We recommend tracking live data before you backfill history. That way, any developer deviations from the plan surface early, and you avoid a gap between the two.


### Historical data


Export your history. Since Amplitude natively supports cloud providers, we route this through BigQuery to keep the whole process smooth. Map every source field to your new tracking plan before transforming anything.


Mapping is where you win or lose data quality: handle every edge case, define your deduplication rules, and sort out identity resolution up front. Then, validate in a dev environment before importing the full dataset into production. Don’t push billions of events straight to prod and hope it works, because it won’t.


For a detailed walkthrough of these steps, including a look at our migration tool, Sherpa, check out the session recording.


## The bottom line


When it comes to migrating analytics platforms, too many teams become paralyzed by fear and what-ifs. They wait to migrate until the pain of staying put outweighs the imagined pain of moving. But that imagined pain is almost always far worse than the real thing.


Establish your “why” and clean as you go. Bring only the data you need and run in parallel. Treat migration as a journey and establish ownership for the long haul. Doing so will transform your complex migration from a leap of faith into a controlled, measurable project.


That’s what we do every day at Human37. If you’re considering migrating to Amplitude and want to talk it through,[reach out](https://www.human37.com/) . We’re happy to help you scope it.
