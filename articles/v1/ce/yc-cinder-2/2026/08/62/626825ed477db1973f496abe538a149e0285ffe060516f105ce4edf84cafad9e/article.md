---
schema_version: "1.0.0"
document_id: "626825ed477db1973f496abe538a149e0285ffe060516f105ce4edf84cafad9e"
company_key: "yc-cinder-2"
company: "Cinder"
source_id: "yc-cinder-2-news-import-d9673863c17b"
canonical_url: "https://cinder.ai/resources/blog/glen-wise-tbpn-stopping-ai-powered-abuse"
published_at: null
first_seen_at: "2026-08-09T20:41:29.044096+00:00"
fetched_at: "2026-08-09T20:41:30.024984+00:00"
content_hash: "sha256:7d53f68966397fbfc2e73b24c5eb49721818e416080b188a7c308194c5c69a24"
---

# Glen Wise on TBPN: Stopping AI-Powered Abuse at Scale

Cinder's CEO and cofounder Glen Wise appeared on[TBPN](https://www.youtube.com/live/-XroYgyoNuU?si=1sUCY8wSaslo1E2f&t=6591) , the OpenAI-owned daily business and technology talk show hosted by John Coogan and Jordi Hays, to discuss the company's Series B fundraise. Wise spoke with the hosts about trust and safety at scale, common forms of generative AI abuse and the tactics used to prevent them, and the considerations companies face when deciding whether to build their own moderation tools or partner with a third-party harm prevention platform.


*The following Q&A has been edited for length and clarity.*


## What does it mean to stop AI-powered abuse?


TBPN: There’s cyberbullying and mean comments on YouTube videos or live streams, and then there’s spam and hacking and cyber attacks and all sorts of crazy stuff.


Glen Wise: Honestly, it means the entire gambit of threats. And that's what I think gets missed in this conversation: the fact that there are small incidents such as bullying, and there's large incidents such as state-sponsored espionage. But companies need to respond to all of these. And we've never before seen the kind of scale of threats that we have today. And this was a problem before generative AI, but obviously gen AI has made threats exponentially worse.


## How Cinder works to prevent AI abuse


TBPN: How does Cinder work to prevent AI abuse?


Glen Wise: How the platform actually works is that our customers set which policies they care about. Some of the really big ones we see a lot now are AI-generated NCII and AI-generated deepfake porn. That's a huge issue that a bunch of people see. Obviously anything child safety related, any egregious hate speech and things like that, customers are able to set these policies on our platform, and then we use AI to detect and mitigate it at whatever scale the platform is operating at.


TBPN: How does Cinder handle the scale of bigger customer’s AI abuse needs and tackle the firehose of high-traffic platforms?


Glen Wise: The whole founding team came from Meta. And prior to that we were at the US government, so we've seen what harm at scale looks like. Obviously volume is an infrastructure challenge that we deal with: being able to process data as quickly as possible. How can we make a decision as fast as possible as to whether or not something violates your policies? There's a bunch of techniques there for being able to handle that scale. We have some customers that have a really large Gen Z audience that all log in at the same time, for example. So we’re always tackling different kinds of distributed computing challenges. That’s part of the fun of building this.


## Balancing cost, latency, and accuracy


TBPN: As you scale Cinder, how have you thought about the infrastructure tradeoffs?


Glen Wise: The thing that is most important for Cinder customers is evals and ground truth data. Our customers set within our platform what true looks like for them. What does an actual violation look like? Because, as you can imagine, these violations are incredibly nuanced. It really depends on the platform where they're based, or how old their users are. A classic example is a gaming company that has two different games: one is a first-person shooter for adults, the other is a game for children. Obviously they're going to define a threat of violence very differently even within their own platform.


Our users need to be able to set ground truth and set these evals. From there, you run evals on these models. It depends on whether you are prioritizing costs, latency, or accuracy. Those are basically the three tradeoffs that we see. You can get really great results now, especially around fine-tuning some of these open source models. But what's funny is that obviously these models themselves are trained to not be able to produce this content. And so you do start hitting limitations with these foundation models. You can do techniques like model obliteration, where you can actually remove guardrails and host them yourself, or you can do traditional classification, depending on what the policy area is.


## Why companies partner with harm-prevention experts


TBPN: Is there something about the problem of AI harm prevention that makes companies want to outsource this function?


Glen Wise: I think there's a few. Primarily, it's taking the human expertise and really understanding the policy and understanding how to mitigate that policy. Every customer of ours can't be an expert in every single AI abuse issue that they might face. So that right there means that they need to bring people on. I was on the threat intelligence team at Facebook, and they have an amazing intel capability. But they're Facebook, so they can spend on building out that threat intel capability internally. Not everyone can or should have to do that. So that's a big piece.


Another one that we've been seeing more and more often is the third-party credibility of going with a company that's also truly a set of experts. So you're not grading your own homework when you're trying to defend your platform. Using Cinder means you have someone else that can bring that expertise in and do that for you.
