---
schema_version: "1.0.0"
document_id: "8a9daf73157e3404ec60bbef5dfaccd52a55f13dbc0c9dd981ddc3dd8d2b5313"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-rss-35f82ba9b7dd"
canonical_url: "https://embrace.io/blog/core-mobile-vitals/"
published_at: "2026-05-27T17:46:45+00:00"
first_seen_at: "2026-07-20T23:20:15.781670+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:8539fb700d7050937b65af558852d6ad31e5f3e7f8f5aca0168e9ff59eaea1be"
---

# We’re building Core Mobile Vitals. Here’s why that’s a big deal.

Core Web Vitals transformed how entire organizations think and talk about web performance. Mobile deserves an equivalent framework — one that's rigorous, user-focused, and tied to real business outcomes. We're building it. Here's how.


Ever since[SpeedCurve joined Embrace](https://embrace.io/blog/speedcurve-joins-embrace/) , I’ve had different versions of the same conversation with our wonderful customers…


It starts with an engineering leader who’s been using SpeedCurve to monitor web performance for years, but who isn’t just thinking about web performance anymore. They want to expand the umbrella to include their mobile teams. They want to lead their company toward fast, seamless, beautiful user experiences across web and mobile. They’ve seen how galvanizing[Core Web Vitals](https://www.speedcurve.com/web-performance-guide/get-started-with-core-web-vitals/) have been, and so they ask the natural question:


*“Is there a mobile equivalent of Core Web Vitals?”*


When I say no, their face falls. But then when I say “we’re working on it”, their face lights up.


This has happened in literally every conversation I’ve had over the past six months. Not most of them. Every single one. People aren’t just curious about Core Mobile Vitals. They’re hungry for them.


So I’m here to share — for the very first time in public — what we’re building, why it matters, and why we’re the right people to build it.


## First: A brief history of Core Web Vitals


I know, I know… you’re here for the mobile stuff. Bear with me. The back story matters.


For most of web performance’s long and storied history, the answer to “which metric should I focus on?” was <drumroll> *it depends* .


There were (and still are) literally hundreds of potential front-end metrics to track. Developers and engineers (and persnickety tool creators like us) had strong opinions about many of them. But if you were a product manager, a VP, or anyone else who didn’t spend their days in the browser console, you were basically out of luck. Performance metrics were a foreign language, and nobody had written the phrasebook yet.


Google changed everything in 2020 when they launched[Core Web Vitals](https://www.speedcurve.com/web-performance-guide/get-started-with-core-web-vitals/) , three metrics chosen specifically because they map to things users can actually feel:


- **Is the page loading?** Largest Contentful Paint tells you how quickly users see important content.
- **Is it responding to my clicks and taps?** Interaction to Next Paint lets you know how long users have to wait.
- **Is it jumping around while I’m trying to click something?** Cumulative Layout Shift helps you understand how janky or unstable a page feels.


But what made Core Web Vitals transformative wasn’t just the metrics. It was everything that came with them:


- grounded in UX research,
- backed by clear thresholds,
- tied to search ranking (which, let’s be honest, is what got a lot of people’s attention), and
- committed to evolving over time.


That last one deserves a quick sidebar…


In 2023,[Google replaced First Input Delay with Interaction to Next Paint](https://www.speedcurve.com/blog/interaction-to-next-paint-core-web-vitals/) as the Core Web Vital for interactivity. This happened partly because of correlation research we did at SpeedCurve.


(I’m using the royal we here.[Cliff Crocker](https://www.linkedin.com/in/cliffcrocker/) did all the heavy lifting. Thanks, Cliff!)


[We were looking at FID in customer data and it just wasn’t correlating with conversion rate](https://www.speedcurve.com/blog/first-input-delay-google-core-web-vitals/) the way a good user experience metric should. We flagged that to our friends at Google. They went away, investigated, and came back with INP. When we/Cliff did our own analysis, we found that[INP does indeed correlate to metrics like bounce rate and conversion rate](https://www.speedcurve.com/blog/INP-user-experience-correlation/) .


That’s how good metrics get made: real data, transparent processes, and the willingness to say we can always do better.


## The impact of Core Web Vitals was huge and immediate


Suddenly, we were having conversations with people we’d never been able to reach before: product managers, SEO teams, marketing folks, business stakeholders. People who needed a way to talk about performance without getting lost in technical weeds. The umbrella got a lot bigger. (Yes, I use the umbrella metaphor a lot. My colleagues have noticed. I regret nothing.)


## Why does mobile feel like it’s stuck in 2015?


Mobile apps are where many of your users are. In industry after industry, native mobile drives a huge share of engagement and revenue. If you’re in ecommerce, you probably already know that the majority of your customers would rather shop via your app than on your mobile website.


But if you ask a mobile team “is your app healthy?”, you’ll probably get a 20-minute technical briefing that involves crash rates, ANRs, Apdex scores, CPU utilization, and a handful of other metrics that make total sense to engineers and mean absolutely nothing to everyone else in the building.


To be clear, those metrics are useful! But they don’t do what Core Web Vitals do.


*They don’t give a product manager a meaningful way to talk with their mobile engineers.*


*They don’t help an engineering leader tell the story of app performance to a business audience.*


*They don’t help anyone figure out which problems to fix first based on actual impact to users and revenue.*


The mobile teams we talk to feel this acutely. They’ve watched their web colleagues use Core Web Vitals to make the case for performance investments, align cross-functional teams, and build a shared language around user experience. They’ve wanted the same thing for mobile for years.


Some mobile observability tools have slapped a “mobile vitals” label on a dashboard of summary metrics and called it done. I absolutely understand the impulse — people are clearly asking for this — but the label is easy. The rigor is hard. And the rigor is what makes Core Web Vitals meaningful rather than decorative.


## What makes a vital a vital?


A vital is more than a summary metric. Our core (no pun intended?) belief about what makes a vital a *vital* :


- It tracks something the user actually experiences.
- It can be measured reliably and consistently, and is supported out of the box across platforms.
- It has clear, evidence-backed thresholds for good and poor.
- *It correlates with real business outcomes, like conversion, retention, and engagement.*


That last point is crucial. If a metric doesn’t connect to something that matters to the business, why should anyone prioritize fixing it?


## Our approach to Core Mobile Vitals


We’re approaching Core Mobile Vitals the same way Core Web Vitals have been developed: with research, community input, real-world data, and humility about what we don’t know yet.


We’re working with industry-leading companies to test our hypotheses, validate our proposed metrics against real user data, and establish thresholds grounded in evidence rather than vibes.


We’re hoping that other vendors in our community join our quest!


We’re starting with Android and then expanding to iOS. We’ll be sharing our journey publicly — including a talk at[performance.now()](https://perfnow.nl/) in November. (If you’re going to debut a new performance framework, that’s the place to do it!)


## The four pillars of Core Mobile Vitals


Now let’s get to the juicy stuff.


Before we can nail down specific metrics, we need to define what we’re actually trying to measure and, most important, why we’re trying to measure it. To start, we’ve identified four user-centered goals that we believe Core Mobile Vitals should address.


**Important:** These pillars aren’t the metrics themselves; they’re the *experiences* we’re trying to capture and improve. The metrics will follow from the goals, not the other way around.


It’s also important to say up front that we know these will be difficult to define on mobile, which has constraints we’re blissfully free from with the web. But we’re working hard on it.


### Screen Load: How quickly does meaningful content appear?


Screen Load is about the moment a user navigates to a new screen. This is the mobile equivalent of LCP.


Users make instant judgments about whether your app is fast, stuck, or broken, and they make those judgments based on what they can see. A screen that loads quickly signals a healthy, responsive app. A screen that stalls signals the opposite, even if everything is technically fine under the hood.


### Smoothness: How does your app feel?


Smoothness is the mobile cousin of CLS. Both are about whether the experience feels stable and controlled, or chaotic and out of your hands.


A smooth experience is almost invisible. Users don’t notice it, they just enjoy it. A janky experience is impossible to ignore. Dropped frames, stuttery animations, and hesitant transitions make an app feel cheap and unreliable, regardless of how much engineering went into it.


### Responsiveness: What happens after a user taps, scrolls, or swipes?


This is the mobile equivalent of INP. When a user taps a button, pulls to refresh, or otherwise interacts with your page, how quickly does your app acknowledge that action and deliver a result? The gap between action and response is where users either feel in control of their app or feel like they’re shouting into the void.


### Stability: Does your app feel trustworthy?


This is arguably the issue that gets under users’ skin the most, because instability is the most disruptive experience of all. Crashes and unexpected restarts don’t just frustrate users — they violate trust in a way that’s hard to repair. An app can have decent load times and reasonable responsiveness, but if it crashes regularly, none of that matters. Stability is the foundation everything else is built on.


## Something you might have noticed...


You’ll see that we’re steering away from naming the metrics that align with each of these pillars. That’s intentional. By focusing on goals first, we’re creating a framework for exploring the most important question: *What does a good mobile experience actually look and feel like for your users?*


We’re working with real users and real data to not just answer that question, but to define metrics and thresholds that mean something, rather than metrics that are simply easy to capture and thresholds that just sound reasonable.


## Why us? Why now?


I’ve been in performance for over 16 years. I watched the web performance industry spend a long time wandering in the wilderness of too-many-metrics-nobody-agrees-on. I’d really rather not watch mobile spend another decade in that same wilderness. (There are no good snacks in the wilderness.)


The team we’ve assembled for this initiative is uniquely suited to the work.


Embrace is a mobile observability leader built on OpenTelemetry, with deep roots in openness, standards, and user-focused observability — not bolted-on marketing language, but a genuine commitment baked into how Embrace was conceived and built.


The SpeedCurve side of the family brings a long history of pioneering new performance metrics, collaborating directly with browser vendors, and validating the relationship between metrics and real-world user behavior.


Put those two things together and you have a beautiful combination: the mobile observability depth to instrument and measure native app performance at scale, and the performance research chops to know what those measurements should actually mean for users and for the business. Our shared mission — helping companies deliver seamless, beautiful user experiences across web and mobile — is exactly what makes Core Mobile Vitals the right project for our team.


## Core Mobile Vitals are for EVERYONE


I want to make this clear: these are NOT proprietary metrics. Our goal is to create a standardized set of metrics that our entire community can explore, test, and iterate together. Because yes, there almost certainly will be iterations.


We’re going to great pains to land on a set of metrics that are available out of the box via any mobile observability platform, not just ours. (Though it goes without saying, we think there are a lot of good reasons to choose Embrace if you care about user-focused observability!)


## This is a long game. Here's what comes next...


We’re not the first to use the phrase “mobile vitals.” But we intend to do it right, mirroring the rigor that’s gone into developing Core Web Vitals.


The web version took years and is still evolving. (Safari just announced support for LCP and INP a few months ago. It’s a journey!) Mobile will be the same. But it’s a journey worth taking, and based on all those faces that have been lighting up over the past six months, the industry has been waiting for someone to start it.


Here’s what’s happening next:


- We’re including our beta metrics (TBA) in our mobile SDK and starting to gather data.
- We’re working with a handful of enterprise customers who are interested in being guinea pigs for this initiative. If that sounds like you, let us know!
- We’ll release early findings — including the names of our metrics — as soon as we can.


It’s happening! It’s exciting! Stay tuned.


Deliver incredible mobile experiences with Embrace.


Get started today with 1 million free user sessions.


[Get started free](https://dash.embrace.io/signup/)


Author


[Tammy Everts](https://embrace.io/author/tammy-evertsembrace-io/) Tammy is a UX researcher, writer, and speaker who has spent more than two decades studying how people use the web. Her focus is the intersection of performance, user experience, and business outcomes. She's helped some of the biggest companies in the world – from Amazon to Zillow – deliver fast, joyous experiences to their users. Tammy is the author of 'Time Is Money: The Business Value of Web Performance' (O’Reilly) and co-chair of the annual performance.now() conference.


Related Content


business


29 April 2026


• 5 min read


### [Make the business case for web performance with just three visuals](https://embrace.io/blog/web-performance-business/)


If you want to get the attention of non-technical stakeholders, these are the three most powerful charts to immediately grab folks' interest and get them to care about site speed and user experience.
