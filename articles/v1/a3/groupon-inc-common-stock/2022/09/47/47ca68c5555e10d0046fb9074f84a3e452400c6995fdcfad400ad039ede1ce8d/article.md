---
schema_version: "1.0.0"
document_id: "47ca68c5555e10d0046fb9074f84a3e452400c6995fdcfad400ad039ede1ce8d"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/migrating-to-optimizely-at-groupon-27fa56f86d12"
published_at: "2022-09-12T21:17:34+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:94c1ff07b47a7d814e37567d123274d1ed6a18dfe7134c4108501613c5167d21"
---

# Migrating to Optimizely at Groupon

# Migrating to Optimizely at Groupon


## Sharing experiences and learnings 🧪


[Andres Otarola](https://medium.com/@aotarola?source=post_page---byline--27fa56f86d12---------------------------------------)


6 min read


·


Sep 12, 2022


--


Press enter or click to view image in full size


Photo by[Alex Kondratiev](https://unsplash.com/@alexkondratiev?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


Experimentation is a key indicator of whether a feature will be appealing to users, maybe fall flat, or worse: cause a negative impact on them. Here at Groupon, we have been data driven for years, so it is in our nature to[A/B Test](https://en.wikipedia.org/wiki/A/B_testing) everything that we present to our customer base.


The objective of this article is to give you some *insights* into how we migrated our **existing** experimentation pipeline onto[Optimizely’s](https://www.optimizely.com/) **** solution by swapping certain components from our pipeline. Let’s dive in.


As mentioned, we already had an existing experimentation pipeline, which can be generally split into two main components:


- Data transport
- Experiment engine


Generalised diagram of experiment events


## Data transport


This is the layer that is responsible for transporting the events of an experiment. “What is an impression event?” you may inquire. It’s an interaction of a unique user with the treatment of an experiment. For Example, an experiment that presents either a red button or a green button to a user will be counted as an *impression event* with the chosen variation (either a red button or a green button).


Such events are handed over to specialized channels which end up being cleaned up by a central process in charge of canonicalizing the events so upstream services can easily parse the data.


It is important to emphasize that the Data transport layer is ignorant of the details of the experiment engine.


## Experiment engine


This is the actual component that decides how an experiment is performing based on the *events* it is receiving, as well as providing the information of the currently available experiment to query from. (In our case this implementation varies per client; more on this later on.)


## Enter Optimizely


When we decided to switch from our in-house experimentation solution to a cloud-based one, in this case, Optimizely, we immediately faced various challenges:


- Keep backward compatibility (Even more so for Mobile!)
- Support both Web and Mobile


*Note: Actually, there were many other challenges as well, but for the purposes of this post, I’ll just mention those two.*


## Migrating over for Mobile


Believe it or not, this was not difficult. The current experiment flow looks like this:


1. Once the app is launched, it fetches a pre-bucketed list of experiments (i.e. experiments get evaluated for a specific device, but no *impression events* are triggered) from a public-facing API, and this data is held in memory and used in the app’s session (yes, there’s a cache refresh policy in place on the mobile client as well).
2. Whenever a User is presented with the pre-bucketed treatment (e.g. a red button, which is under experimentation in our previous example) then the application will send an event with this information plus the user identification through the data transport layer.


In this case, we just had to update the response the app receives when it fetches a pre-bucketed list of experiments, and that was all. No code changes were required for either of the Mobile clients (iOS and Android) 🎉


## Migrating over for Web


Web applications presented a different level of challenge. The main reason was that we cannot just fetch experimentation data per request (it’d kill the service with a high volume of traffic), plus we had the added challenge of keeping our performance numbers under a certain budget per service, so the experiment evaluation has to be fast.


We had several options on how to do this, but the deciding factor comes down to two indicators:


1. Performance
2. Developer Experience


We wanted to come up with a sweet spot between those two, so our final solution was to wrap Optimizely’s SDK inside of an npm library that hides away such detail, which had added the benefit of:


- Instantly evaluating an experiment, thus no performance penalty
- Prevent developers from using unsupported (by us) Optimizely features


The only downside is that services needed to upgrade to use this new library. Fortunately, the old Experimentation implementation we had been using already followed this approach in Web, so we just had to add support to the same npm library.


## Does Optimizely support everything that we had before? not quite.


Sadly, this migration was not 1:1 feature parity-wise, and there were some really good bits from our previous system that we wanted to have as well in Optimizely, which we ended up implementing in it:


- *Experiment tagging:* services can tag their experiments in the Optimizely platform, then when fetching all available experiments, they can request the ones that have only the required *tags* , thus reducing the data file size considerably, along with other specialized requirements from the business.
- *Experiment rollouts as features* : This is basically to roll out experiments but keep them as features in Optimizely, so Experiment owners can choose to control the experiment behavior from the Optimizely dashboard once it has been declared to be rolled out. This liberates teams from doing code changes.
- *Bucketing Strategy* : By default, we use the user’s device as the identifier for experimentation purposes, but not all experiments have to be implemented this way. For such cases, experiment owners can alter the way an experiment is evaluated.


You might be wondering how we implemented these missing features without actually modifying Optimizely’s behavior since we have zero control over a third-party system. Well, it turns out that experiments in Optimizely can hold an arbitrary list of attributes with varying types. Among those types are ones that support JSON data, so, as you probably guessed, we leveraged that feature in order to add our metadata 😉.


## Adding arbitrary experiment metadata sounds error prone


It does, and it is! To keep experiment owners from adding typos in the metadata we have come up with a simple solution: have a simple UI that reads experiment information from Optimizely API, and shows whether an experiment was created properly and is ready to be launched following Groupon’s specific set of rules. This way Experiment owners check that UI before starting an experiment.


This can, of course, be extended to a more proper solution such as:


1. A browser plugin that integrates with Optimizely and validates an experiment within the Optimizely dashboard
2. Our own experimentation UI that allows experiment creation with our set of rules handled automatically by the application


Both sound reasonable, with different levels of effort, and totally worth exploring.


## Managing the decommission of the old Experimentation engine


Migrations always bring challenges that are mostly *fun-ish* , and they are always accompanied by certain hard deadlines — in our case, the decommissioning of the old Experimentation engine.


In a perfect world, you’d have all your services migrated over to the new solution before any decommission can happen, right? But the real world is more fun! So in our case what we did was to generate an experiment list snapshot and store those in S3 buckets and serve them using the old hostname that services used to access the old service, thus keeping them from being impacted and allowing the decommissioning of the old Experimentation engine. This one was a smooth-as-butter transition.


## Final Thoughts


1. When implementing a data-driven system, even if it is an in-house one, it is always a good idea to abstract the details of it away from other systems, so you have the liberty (or at the very minimum; reduce the pain) of swapping out components in your pipeline with minimal risk.
2. When moving to a new solution from a previously known one, you usually gain a lot of shiny features, but you probably will miss others not present in the new solution; there’s always a way to bring them in one shape or another into the new system. 😉


## Disclaimer


This article might sound as if the overall migration was planned and executed flawlessly; that was not quite the case! Mistakes were made along the way, tears were shed, but most importantly, lessons were learned! 😸.
