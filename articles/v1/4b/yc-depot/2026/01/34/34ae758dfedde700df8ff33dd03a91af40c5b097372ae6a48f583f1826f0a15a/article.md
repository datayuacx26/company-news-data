---
schema_version: "1.0.0"
document_id: "34ae758dfedde700df8ff33dd03a91af40c5b097372ae6a48f583f1826f0a15a"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-2025-recap"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:6a4d09e4b70d339b879ad53c92b062d728cb9720f8acf8f6cf70b02895f5cce3"
---

# What we learned accelerating 100 million builds in 2025

It's the season where tools and services start dropping their annual recaps and rolling out the Spotify Wrapped concept to their users. Depot is no exception. We dropped our first ever[Depot Wrapped](https://depot.dev/blog/depot-wrapped-2025) and now I'm excited to share our 2025 recap.


I write recaps for Depot every year, but this is the first year I'm sharing it publicly. I've spent a large chunk of my time over the past year talking to folks who are using Depot every day and learning about their experiences, gathering feedback, and talking about ideas for the future.


In those conversations, one thing has come up repeatedly. People love our transparency and our willingness to share what's happening behind the scenes.


So, my goal with this post is to take you behind the scenes of Depot and share some numbers, learnings, and what we see for the future having scaled to over 100 million builds in 2025.


## The numbers


So far this year we've helped accelerate over **100 million builds** and helped save engineering teams over **8,489,520** hours of build time, roughly the equivalent of 968 years of build time in 2025 alone.


The wild part is that we've managed to 8x our build volume and actually make the average build time faster from **3 minutes and 11 seconds** last year, to a flat **3 minutes** this year. All this while processing north of **2,000 builds per minute** at our busiest times across Depot.


We're proud of the work we've done this year and the progress we've made. But, it's only really possible because of the folks that trust us to make their lives easier by making their builds exponentially faster.


Whether it's a healthcare tech company sending us Datadog graphs showing 30% faster builds from an optimization we pushed without any work on their end. Or the HR company we get to sit across from over dinner and talk about how Depot continues to "just work" and "make our lives easier". Or the customer relying on Depot Registry to reliably push images to their customers across the globe who pushes us to keep getting better at what we do.


We're privileged to help accelerate the work of so many teams that are on the bleeding edge of helping their own customers.


## The learnings


Scaling to that number has also unlocked a lot of learnings and insights. Some kind of obvious in retrospect, but others that were more subtle and were a gentle reminder that we're not alone in this journey.


### External systems are challenging


An interesting thing arises with scale: you start to see and learn about new bottlenecks in your system.


It starts with the bottlenecks that are within your control. These are the bottlenecks that are easy to identify and fix, given enough time and resources. Think of a memory leak in your application code, or a bug in your frontend that is causing users to not be able to save a form. Most of the time, these live inside of code you control or write and thus are easy to identify and fix.


But there are also bottlenecks outside of your control. For us at Depot, these bottlenecks outside of our control are the biggest challenges. Part of that is because we've explicitly decided to solve some of them (GitHub Actions runners). But others we're learning are less reliable or slower than we'd like (Docker Hub, GitHub control plane, etc).


For example, we had to build our own internal control plane for interacting with GitHub to listen for events and trigger jobs whenever GitHub fails to deliver a webhook event to our system. Why? Because GitHub doesn't guarantee webhook delivery in any way.


This is software engineering. It's not just about the code you write, but also about the systems you integrate with and the external dependencies you rely on.


This story isn't over, and probably never will be. But it's a journey that we're excited to continue to take.


### Data tells the story


We've always been a data-driven company. We've always been obsessed with measuring and tracking everything we do. This is baked into our DNA and our culture. From the very beginning, we've been tracking every single usage metric we could think of and measuring it week after week, month after month, year after year. Shout out to PostHog for giving us a service that makes that incredibly easy.


This year, we've taken that to another level by trying to go even lower level and track the performance of the individual components that make up the Depot platform. We've also strived to make this data available to everyone using Depot via things like GitHub Actions analytics, our new Usage Dashboard, and more.


Data is incredibly powerful. It can help you identify problems, it can help you find opportunities, and it can help you make decisions. There have been times throughout the year when we've had to remind ourselves that we already have all of this data available to make better decisions. As you scale a tool like Depot, data becomes the lifeblood of your product.


### We're not just a tool, we're an extension of your team


It's an incredibly humbling and privileged experience to walk into a room full of engineers from a 2,000 person company and hear them say things like "Depot is a critical part of our infrastructure" or "Depot has made our lives so much easier" or my personal favorite **"The entire Depot team is like an extension of our engineering team"** .


I often remind our team that Depot is built by people for other people sitting at the table with us, not across from us. We're not just a vendor, tool, or service trying to extract a transaction from you. We're a team of people who are deeply committed to making your life easier, your builds faster, and your engineering team more productive.


This is why we've always been so transparent about what we're doing and why we're doing it. We're not trying to hide anything from you. We're not trying to sell you something you don't need. We're not trying to make you feel like you're being sold something you don't understand or even need.


### AI and LLMs are changing the delivery pipeline


In case you haven't noticed, AI and LLMs are all the rage this year. And for good reason. It does truly feel like we've jumped some kind of chasm in software engineering with the help of coding agents like Claude Code, Cursor, and others.


What we're seeing is more and more organizations are adopting these agents to help them accelerate their work. In a lot of ways, coding is no longer the biggest bottleneck to jumpstarting a new idea or project. It's everything that comes after that is slowing them down. Builds being one bottleneck. But we hear from many folks about several others that span the entire delivery pipeline.


What does this mean? It's too early to predict exactly where this leads, but one thing is clear: the bottleneck has shifted from writing code to integrating it. It's safe to say that build performance, testing, documentation, and reliable & performant systems are even more important than they have ever been before.


## What's next for Depot?


The second most common question I get asked. We're working on a lot of new things for the year ahead. Some of them are publicly known or we have talked about in the past:


- Even faster container image builds running our own build engine
- Faster GitHub Actions runners with an enhanced caching system
- Continuous reliability improvements to make Depot more resilient to upstream outages and more in control of our own destiny
- Performance, performance, performance; because we never stop looking for ways to make builds faster
- Continuing to bring more observability and visibility into what's happening in your builds and your organization like we have done with our[GitHub Actions analytics](https://depot.dev/blog/now-available-github-actions-analytics) and[job logs search](https://depot.dev/blog/now-available-gha-log-search)
- Enhancements to our Depot Registry to improve the overall developer experience for using it outside of Depot builds


We also have a lot of things still in the works that we're not quite ready to share yet. Like what does it look like when you rethink the collaboration patterns we have today that are introducing bottlenecks as code velocity increases?


We're excited to share more about them in the coming months as we start to roll out what we think delivery pipelines should look like given this new world we are all working in. If you have been thinking about how existing systems and services from CI to source control to testing to production deployments should work better together, we'd love to hear from you, so[join our Discord Community](https://discord.gg/MMPqYSgDCg) and let's chat.


Also, Depot is actively hiring. If you're interested in joining the team, please check out our[careers page](https://depot.dev/careers) and apply. We're always looking for talented people to join the team and help us build the future of software delivery.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
