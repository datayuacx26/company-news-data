---
schema_version: "1.0.0"
document_id: "234c8d1b592ab13045e3408ec9061c933b1679c2b92084364eb4afee1c2d0276"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/mykonos-hackathon"
published_at: "2024-05-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:5f2b1f1b613fb94bfa257461b987d127498790473bdfce63dc9a3c0f10cc7593"
---

# What we built at our windswept Mykonos hackathon

# What we built at our windswept Mykonos hackathon


- [James Temperton](https://posthog.com/community/profiles/29278)


May 31, 2024


- [Offsites](https://posthog.com/blog/offsites)


#### Contents


-
-
-
-
-
-
-
-
-
-
-
-
-
-


As a fully-remote company with 47 misfits spread across ten countries, our offsites are a vital part of our culture. They’re a great way to get to know colleagues better, and the connections formed during offsites bring extra energy and creativity to our work throughout the rest of the year.


This year, we headed to Mykonos – not to rave, but to code. And to enjoy a cocktail or two in the sun, of course.


Our all-company offsites are a mix of socializing, group activities, strategic sessions, Post-its, workshops, more Post-its, and, the star of the show, the annual PostHog[hackathon](https://posthog.com/newsletter/hackathons)


.


Everyone in the company has to pitch a couple of ideas, then we all vote on our favorites, assemble teams, and have a little over a day to go from pitch to demo — and[the tricks that make those demos memorable](https://posthog.com/newsletter/how-to-demo#storytelling-tactics)


are worth knowing before you get up there.


In some cases, our hackathon projects are ready to ship right away. Some have even become core parts of our product –[session replay](https://posthog.com/session-replay)


started as a hackathon project, as did our[data warehouse beta](https://posthog.com/docs/data-warehouse)


.


Here’s what we built in Mykonos.


##


Our own programming language


Built by:


- [Marius Andra Marius Andra](https://posthog.com/community/profiles/30202)


- [Anirudh Pillai Anirudh Pillai](https://posthog.com/community/profiles/30974)


What’s cooler than having your[own query language](https://posthog.com/docs/sql)


? Having your own general purpose programming language, of course! That’s literally what we built: PostHog’s first ever programming language,[Hog](https://posthog.com/docs/hog)


. (Earlier versions of this name included Hög and Höge, but turns out we're anti-umlaut.)


Things moved fast the week after Mykonos: we split up the existing product analytics team and built a new team to productize Hog. We plan to use Hog to build our CDP and messaging products, and might even pivot the entire company around it.


Why? Because, in the past, we’ve sometimes struggled to build UI fast enough for users to take advantage of new products that are ready to ship. Hog will mean anyone can drop down into hacker mode and get things done. We move fast. You move fast. Everyone is happy.


It was just another PostHog hackathon, no big deal. Stay tuned for the aftermath.


##


RealTimeHog 3000


Built by:


- [James Greenhill James Greenhill](https://posthog.com/community/profiles/30174)


- [Zach Waterfield Zach Waterfield](https://posthog.com/community/profiles/30086)


- [Michael Matloka Michael Matloka](https://posthog.com/community/profiles/28847)


- [Brett Hoerner Brett Hoerner](https://posthog.com/community/profiles/28390)


Seeing people using your product live boosts dopamine levels. Probably. PostHog does that, but right now we keep you waiting a bit.


Before a PostHog event is available for querying, it goes through our ingestion pipeline, where users are identified and the events themselves enriched. The process takes seconds, rarely a minute – a delay imperceptible in analytical queries, but a dopamine decrease in the live view.


The solution? Introducing RealTimeHog 3000, a[livestream](https://github.com/PostHog/livestream)


service powering our first truly real-time view of user activity. The process is simple:


1. Consume the raw events from the same Kafka topic as our ingestion service.
2. Stream them to you ASAP using server-sent events.


The lack of person data is a fair trade-off, because it doesn’t get more live than this. User activity appears within milliseconds of happening on the other side of the world.


Speaking of data being processed live, perhaps at some point you’ve wondered if PostHog actually scales, or just talks the talk.


To dispel any doubts, we developed one extra feature: an anonymized stream of all events being captured globally, with only the geolocation included.


Millions of events per minute, and they look great on a 3D globe on PostHog’s website, where each event is an arc from the user’s location to that of the relevant data center. Global scale, visualized for your pleasure.


##


MykoLogs


Built by:


- [Paul D'Ambra Paul D'Ambra](https://posthog.com/community/profiles/30173)


- [Tom Owers Tom Owers](https://posthog.com/community/profiles/29577)


- [Ted Kaemming Ted Kaemming](https://posthog.com/community/profiles/29568)


MykoLogs is a logging product that integrates with the existing PostHog SDKs, bringing backend logs straight into a shiny new product on PostHog.


The best part? You can link backend logs to session recordings through the user’s session ID – letting you debug what was happening on the backend during your user's API requests. MykoLogs plays nicely with all other PostHog products, meaning logs and session replay are now BFFs. Debugging has never been this breezy!


It's internal-only for now, but could one day be made public.


##


The presidential briefing


Built by:


- [Charles Cook Charles Cook](https://posthog.com/community/profiles/28625)


- [Eric Duong Eric Duong](https://posthog.com/community/profiles/30209)


- [James Hawkins James Hawkins](https://posthog.com/community/profiles/27732)


- [James Temperton James Temperton](https://posthog.com/community/profiles/29278)


As PostHog grows as a company, keeping track of everything that’s happening will get harder. Yes, we[write everything down](https://posthog.com/newsletter/remote-working)


but that creates a lot of reading, and a lot of noise. The solution? An AI-generated briefing, tailored to each individual team member and their interests.


The presidential briefing was built by scraping PRs and issues from GitHub, along with Slack messages, and then training an LLM to understand what’s interesting and important. The bot then produces a pithy briefing that removes the noise and gives people just the information they need.


While just a proof-of-concept for now, if we were to ship it we’d want to add more data sources and build it using Llama to avoid the need to send any data to external services.


##


10x terms


Built by:


- [Cory Watilo Cory Watilo](https://posthog.com/community/profiles/30200)


- [Andy Vandervell Andy Vandervell](https://posthog.com/community/profiles/30208)


- [Fraser Hopper Fraser Hopper](https://posthog.com/community/profiles/30207)


Are you fed up with lawyers making everything so hard to understand? Are you fed up with those nerds in Brussels making us sign DPAs for everything? Not anymore! On PostHog.com, we’ve made all the legal stuff fun – and kept the lawyers happy.


First up, we summarized our[terms](https://posthog.com/terms)


and[privacy policy](https://posthog.com/privacy)


in plain English. You can still read the long, legal-y version, but it’s now way easier to understand what it actually means. And we didn’t stop there.


Then we took on our data processing agreement, or DPA, to create a generator that makes this hugely exciting task even more fun. You can quickly populate your own form, select the data region, and, if you want, add some pizazz with fairy tale or Taylor Swift mode. DPA?[Try DPYAY!](https://posthog.com/dpa)


##


ZenHog


Built by:


- [Dylan Martin Dylan Martin](https://posthog.com/community/profiles/30455)


- [Tiina Turban Tiina Turban](https://posthog.com/community/profiles/28442)


- [Marcus Hof Marcus Hof](https://posthog.com/community/profiles/30211)


- [Neil Kakkar Neil Kakkar](https://posthog.com/community/profiles/28695)


Our support flow currently uses Zendesk and that goes through email. This causes three problems:


1. Emails sometimes bounce
2. There are long delays in checking emails
3. It’s really clunky to add other team members via CC.


It’s not the most optimal flow. So, imagine if instead users could view and respond to their open support tickets without ever having to leave PostHog? Yhat's exactly what we built.


But that's not all, if you're using Zendesk then you could, in the future, add this view to your customer-facing website with just a few clicks. Like the sound of this project?[Got to our public roadmap](https://posthog.com/roadmap)


, search for 'Customer support product', and vote for it.


Subscribe to our newsletter


#### build mode


Read by 75,000+ founders and builders


We'll share your email with Substack


##


The referral scheme


Built by:


- [Ben White Ben White](https://posthog.com/community/profiles/30205)


- [Raquel Smith Raquel Smith](https://posthog.com/community/profiles/28693)


- [Joe Martin Joe Martin](https://posthog.com/community/profiles/29070)


Everyone loves a pyramid scheme, right?!


Wait, no, we mean a *referral program.* Ben, Raquel, and Joe worked together to build a referral product right into PostHog. This means we can offer sweet merch, platform credits, good vibes, and other things to loyal PostHog users who lure in their friends and family.


What’s more, as the referral program product is built right into PostHog, you can build your own for your customers. The system is hooked up to Zapier to automate the process for redeeming codes making the whole thing a doddle.


It’s not shipped yet, but we’re close.


##


Managed reverse proxy


Built by:


- [Frank Hamand Frank Hamand](https://posthog.com/community/profiles/29192)


- [David Newell David Newell](https://posthog.com/community/profiles/30203)


- [Steven Shults Steven Shults](https://posthog.com/community/profiles/28949)


Everyone loves ad-blockers. But, for a lot of our customers, they stop data from reaching PostHog.


You can already deploy a reverse proxy to PostHog Cloud to get around this, but it’s a somewhat convoluted process that requires you to jump through 16 hoops and login to AWS.[Our docs on this are great](https://posthog.com/docs/advanced/proxy)


, but Frank decided to build a better solution.


During the hackathon, he built the reverse proxy functionality right into PostHog. The option is tucked away in the PostHog settings.


Simply add in any domain you control and the system will spit out a CNAME that you then need to set in your DNS provider. Wait a few seconds for the update to happen and voila, the reverse proxy is live.


##


A/B TestHog


Built by:


- [Ian Vanagas Ian Vanagas](https://posthog.com/community/profiles/29296)


- [Juraj Majerik Juraj Majerik](https://posthog.com/community/profiles/29792)


- [Lior Neu-ner Lior Neu-ner](https://posthog.com/community/profiles/28754)


Want to know how to improve your website but don’t know where to start? You need A/B TestHog. Enter a website URL, click ‘Analyze’ and an ingenious generative AI system will give you a bunch of recommendations for what A/B tests you might run to take your website to the next level.


These are all expertly authored by an AI, and include the goal metrics, secondary metrics, and guardrail metrics and detailed instructions of what to change for your test.


##


HERMES


Built by:


- [Annika Schmid Annika Schmid](https://posthog.com/community/profiles/28619)


- [Simon Fisher Simon Fisher](https://posthog.com/community/profiles/28895)


- [Mine Kansu Mine Kansu](https://posthog.com/community/profiles/29862)


At PostHog, we love speaking to our users. Maybe a bit too much. Right now, our master customer interviews doc is 382 pages long and contains almost 200 user interviews. It’s a great resource, but it’s getting a bit unwieldy.


But we've now entered a bold new era of feedback management at PostHog thanks to HERMES, or... Holistic Evaluation Repository for Managing Enhancements and Suggestions.


This is effectively a database of user interviews, showing who was interviewed, who they work for, what they do, how much they pay for PostHog, the products they talked to us about, and an AI-generated summary of our user interview notes. The database is searchable and you can easily add new interviews in a couple of clicks.


The database is linked right into PostHog, making it easy to see associated user and organization profiles. It’s also hooked into Vitaly, our customer success tool, to automatically pull in more customer and business information.


HERMES also uses ChatGPT to generate a summary of the features requested during the interview based on the human-authored interview notes. This makes it easy to share actionable feedback from users directly with the product team responsible for that feature.


As part of the project, we also revamped our system for categorizing and tracking feature requests from customers, making it easier for us to see what people want and prioritize the most important product work.


##


Data crunching


Built by:


- [Sandy Spicer Sandy Spicer](https://posthog.com/community/profiles/30369)


- [Tim Glaser Tim Glaser](https://posthog.com/community/profiles/27730)


PostHog crunches a lot of data, especially on very complext queries. To help users better understand the hard work we’re doing when they make a query, we built a loading bar that includes live data on how much data we're crunching (database rows and data volume) and CPU usage we're deploying to generate an answer for you.


If you feel the need for a quick distraction while you wait, you could also check out[Hedgehog mode 2.0](https://posthog.com/changelog?id=2014)


, which also shipped recently.


##


CLI


Built by:


- [Manoel Aranda Neto Manoel Aranda Neto](https://posthog.com/community/profiles/30206)


We build products for engineers, so there’s nothing better than bringing PostHog closer to their natural environment: the terminal.


The PostHog CLI is a command line that allows users to do a few things that are normally tucked away in the PostHog app: creating, reading, updating, deleting, and enabling or disabling feature flags, for example. The PostHog CLI authentication flow is also seamless as it spins up a new browser and allows you to log in with your SSO instead of copying and pasting tokens manually.


In the future, the CLI could be expanded with more features such as creating surveys, and events, installing SDKs automatically, uploading debug symbols, using the CLI as a package, or even as a GitHub action. And, even better, you could do all of that with natural language, no need to memorize all commands by heart.


It's an internal tool for now, but could be made public if people ask for it enough...


##


4 years at PostHog


Built by:


- [Coua Phang Coua Phang](https://posthog.com/community/profiles/28626)


We’re still a young company, but some of our wonderful team members have now been with us for four years or more. So we want to celebrate them. Coua and Kendal came up with a great anniversary gift scheme, meaning our longest-serving colleagues get something special to celebrate.


This year, Marius, Eric, James G, Lottie, Charles, and Michael all celebrate four years at PostHog and will get to pick between a fancy luggage set (handy for traveling to all-team offsites and other PostHog meet-ups), or a James Hawkins-approved coffee machine.


##


Forbidden secret project


Built by:


- [Lior Neu-ner Lior Neu-ner](https://posthog.com/community/profiles/28754)


- [Annika Schmid Annika Schmid](https://posthog.com/community/profiles/28619)


- [Mine Kansu Mine Kansu](https://posthog.com/community/profiles/29862)


We have a Slack channel called #do-more-weird for odd, fun ideas, and this hackathon project belongs there.


Head to the[careers page](https://posthog.com/careers)


and click on James Hawkins' face for an inspirational surprise...


Subscribe to our newsletter


#### build mode


Read by 75,000+ founders and builders


We'll share your email with Substack


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
