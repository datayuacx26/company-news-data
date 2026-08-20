---
schema_version: "1.0.0"
document_id: "f23fcd3757b5d3ad88ee6d3be0bc5c21ec667aea8b276013517230a36455e7dd"
company_key: "braze-inc-class-a-common-stock"
company: "Braze Inc."
source_id: "braze-inc-class-a-common-stock-news-import-06f37ae7f1b6"
canonical_url: "https://www.braze.com/resources/articles/mobile-app-retention"
published_at: "2026-07-24T13:58:00+00:00"
first_seen_at: "2026-07-24T18:25:56.042961+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:9d38e8f728a395a15a3bb630bad22702fa290be95f9ef1d5cafdf2a6c99e5e13"
---

# App retention: How to measure, benchmark, and improve it

App retention trips up most mobile apps, which lose the majority of their users before those users form a habit. The ones who stay are disproportionately valuable, which makes app retention a higher priority than acquisition alone.[More than 90% of users give up on an app](https://www.businessofapps.com/data/app-retention-rates/) within 30 days of installing it.


App retention is the percentage of users who keep engaging after first use, tracked at Day 1, Day 7, Day 30, and Day 90. Plotted over time, it forms a retention curve with a steep early drop-off that flattens as the users who found real value settle in.


Here's what app retention is, how to measure and benchmark it, why users leave, and the strategies that move the numbers.


### **TL;DR**


- App retention is the percentage of users who keep engaging with an app after their first use, tracked at Day 1, 7, 30, and 90. It drops off an early cliff, then flattens once users form a habit.
- To calculate app retention rate, divide active users at the end of a period by those at the start, then multiply by 100. Agree on what "active" means, and pick classic or rolling retention before you compare.
- Day 30 benchmarks run from about 1% to 15% depending on category and platform, so read your number against your category and your own cohort trend rather than a single universal figure.
- Users churn for a handful of reasons, from rough onboarding to notification fatigue, and most show up in the data before the uninstall, giving you time to step in.
- The strategies that move retention run across the lifecycle, onboarding to first value, cross-channel messaging with frequency capping, behavioral personalization, persistent in-app content, and re-engagement, with AI decisioning tuning them per user at scale.


## What is app retention?


App retention is the percentage of users who keep engaging with an app after their first use, tracked at Day 1, Day 7, Day 30, and Day 90. A high rate means people keep coming back after installing. A low one means they installed the app but either didn't continue to use it or stopped using it over time.


[Retention marketing](https://www.braze.com/resources/articles/complete-guide-to-retention-marketing) is important because a user who stays, returns value long after you paid to acquire them. As acquisition costs rise, that spend only turns into lifetime value if the user sticks around, so every extra week of engagement lifts their customer lifetime value.


Plotted over time, retention drops off an early cliff in the first few days, then flattens into a long tail. Most users leave in that first steep stretch. The ones who push past the first week or two have usually built the app into a habit and hold far steadier, and it's this span between the first few days and the first month that makes up the habit-formation window.


Retention is also one of the clearest signals of product value and business health. A steady or rising curve says the product delivers and the business has a base to grow on. A sinking curve points to a problem upstream, in onboarding, core value, or the fit between the app and the people installing it.


## How to calculate mobile app retention rate


To calculate app retention rate, divide the number of users still active at the end of a period by the number who started it, then multiply by 100. The formula stays the same across every timeframe you measure.


### The app retention rate formula


The formula runs one time window at a time. For Day 30, it looks like this.


App retention rate = (users from the install cohort still active on Day 30 ÷ total users in that cohort) × 100


Say 1,000 people install your app on the same day. Thirty days later, 150 are still active. That's a Day 30 retention rate of 15%.


But first, you need to decide what "active" means for your team. Does it mean opening the app, or completing a core action like a logged workout, a sent message, or a completed lesson? Completing a core action says far more about whether people are getting value. If you simply count opens, you'll get inflated numbers that hide whether the product is working.


Each window of time shows you a different snapshot of the customer in their journey. Teams track Day 1, Day 7, and Day 30 retention most often, with Day 3 and Day 90 filling in the picture.


Timeframe


What it helps show


Day 1


The first impression. Whether people came back the day after installing. A weak number usually points at onboarding.


Day 3


The steepest part of the curve, when early drop-off is often heaviest.


Day 7


Early habit formation. Returning after a week means the app is entering a routine.


Day 30


Sustained value, where the curve flattens and the users who remain tend to stay.


Day 90


Durable retention, and a base to project customer lifetime value against.


### App retention rate vs. app churn rate


Keeping an eye on churn is also important for retention. App churn rate is the percentage of users who stop using the app over a given period, and it's the mirror image of retention. Churn = 100 − retention, so a Day 30 retention rate of 15% means a Day 30 churn rate of 85%.


Churn is worth watching because it tells you how fast users are leaving, which is the pressure your retention work has to push against. For mobile apps, if you measure by uninstalls alone, you miss dormant users who keep the app installed but stop opening it. They churn too, so tracking churn through inactivity gives you a truer read.


## Mobile app retention benchmarks by category and platform


A good Day 30 retention rate depends on your category and platform. Across categories, Day 30 retention runs from about 1% to 15%, with News the highest and Photo & Video the lowest.


Here's the current picture at Day 30, split across iOS and Android.


Category


iOS


Android


News


15.3%


9.9%


Finance


6.6%


3.1%


Transportation


4.8%


4.4%


Shopping


4.6%


4.0%


Dating


4.0%


2.7%


Health & Fitness


3.9%


3.4%


Food & Drink


3.6%


2.9%


Entertainment


3.6%


2.8%


Gaming


3.6%


1.7%


Travel


3.1%


2.8%


Social Media


3.0%


1.6%


Education


2.7%


2.2%


Source:[AppsFlyer data via Business of Apps, 2026](https://www.businessofapps.com/data/app-retention-rates/)


News sits far above everything else, partly because it's a small category people seek out on purpose rather than stumble into. iOS holds users a little better than Android in most categories.


### Why monetization and usage frequency change interpretation


The same Day 30 number can read as strong or weak depending on how the app makes money and how often people open it.


A subscription app can run on a smaller, committed base, so a modest retention rate still supports the business. An ad-supported app depends on frequent sessions, so the same rate would worry you.


A social or fitness app expects people back most days, so its retention should sit high. A tax app or a travel-booking app gets opened a few times a year, so a low Day 30 number there is normal, not a problem. Match the benchmark to how often your app is meant to be used.


### Read against category data and your own trend, not a universal number


There's no universal target, so compare against two things, your category's benchmark and your own trend over time.


Keep your free and paid tiers apart. They behave differently, and mashing them into one number gives you an average that fits neither.


Your own trend usually tells you more than any benchmark. Is this month's cohort holding better than last month's? That's what shows whether your retention work is landing. And when a cohort starts to slip, you'll usually see it in the data well before anyone uninstalls.


## Why users leave (and what the data tells you)


Users churn for a handful of reasons, and each one leaves a different trace in your data. Learn to read those traces and you can tell not just that people are leaving, but why.


### What are the most common reasons users churn?


Users churn most often because of a rough onboarding, no perceived value, performance problems, irrelevant notifications, or notification fatigue.


Why users leave


What you'll see first


Poor onboarding


Users drop out mid-setup, and Day 1 retention falls


No perceived value


They open once or twice but never reach the core action


Performance issues


Shorter sessions, crash and error spikes, abandoned screens


Irrelevant notifications


Push open rates fall and opt-outs climb


Notification fatigue


Users mute or turn off notifications as your send volume rises


### When do users decide to leave?


Users decide in three windows: the first session, the first 72 hours, and the first 30 days.


The first session is where someone either reaches first value or doesn't. Miss it and a lot of users never come back.


The first 72 hours are the steepest part of the curve. If a user hasn't returned within three days, the odds they ever do drop fast.


The first 30 days are where the habit sticks or it doesn't. Users still active at Day 30 tend to stay, which is why most retention work gets front-loaded into the first month.


### How do you diagnose churn in your data?


To diagnose churn, lean on three methods: funnel analysis, cohort curves, and session-frequency decay.


- **Funnel analysis** maps the steps from install to first value and shows where people fall out. A big drop at one step points straight at what's broken.
- **Cohort curves** plot retention for each install group so you can compare them. If one week's cohort fell faster than the rest, something changed then, whether a release, a campaign, or a new acquisition source.
- **Session-frequency decay** tracks the gap between visits. Widening gaps are one of the earliest signs a user is drifting. Pair it with uninstall tracking to see when the app actually gets removed, not just when someone goes quiet.


## How to improve app retention


To improve app retention, work through the user lifecycle in order. These five steps move a user from first open to long-term habit. The wider, relationship-level work sits in[broader customer retention strategies](https://www.braze.com/resources/articles/customer-retention-strategies) .


#### 1. Best for onboarding and time-to-value


Get users to something useful in their first session, guiding them straight to the core action where the app proves its worth. The shorter the time-to-value, the more Day 1 retention you keep.


This is also the moment to earn the right to send notifications. A phone only lets you ask once, so show friendly[mobile app messaging](https://www.braze.com/product/mobile-app-messaging) that explains what users will get before you ask them to allow notifications.


#### 2. Best for cross-channel engagement with frequency capping


Users live across channels, so your retention program should too.[Cross-channel messaging](https://www.braze.com/product/cross-channel-messaging) across push, email, in-app, and SMS keeps you present as someone moves through their lifecycle.


Give each channel a clear job, then cap how often you contact a person across all of them combined. With frequency capping in place, the channels reinforce each other instead of competing for the same attention.


#### 3. Best for behavioral personalization and segmentation


Generic broadcasts underperform messages tied to what a user actually did. Behavioral segmentation groups people by their actions, so you can send each group something that fits where they are.


#### 4. Best for persistent in-app content (Content Cards)


Not every message needs to interrupt. Persistent in-app content, like Content Cards, lives inside the app and waits for users to find it when they open up. Because it sits in the app itself, it also reaches users who never turned on notifications.


#### 5. Best for re-engagement and win-back for lapsing users


When an active user goes quiet, a timed[re-engagement and win-back](https://www.braze.com/resources/articles/churn-prevention) campaign can bring them back before they uninstall. Move quickly, because the window to recover a lapsing user runs in days, not weeks.


## Using AI decisioning to improve retention at scale


AI decisioning improves retention by giving each user the right message at the right time, worked out automatically from how they behave. It personalizes retention across millions of users without hand-building a campaign for every single person.


Rule-based campaigns treat every user the same. You set the rule once, "send this push at 6pm," and everyone gets it at once whether it suits them or not. Reinforcement-learning-based decisioning works per person instead. It reads each user's engagement history to handle per-user send-time, channel, and content optimization, for individual-level-decisioning, choosing what's most likely to keep them active. It continuously optimizes too, learning from every interaction to get more relevant over time.


Generative AI and action optimization are two important parts of making those 1:1 decisions.


- **Generative AI** writes the content, producing subject lines, copy, and message variants.
- **Action optimization** chooses the action, deciding which message, channel, and moment a given user gets.


[BrazeAI Decisioning Studio™](https://www.braze.com/product/brazeai-decisioning-studio) brings both together, making 1:1 decisions that optimize any business KPI, retention included, for each user across your channels. For a closer look at how this works over the full lifecycle, see[how AI supports customer retention](https://www.braze.com/resources/articles/ai-customer-retention) .


## Key app retention metrics to track


Looking at any metric on its own can mislead you. It's only when you look at several together that you get the whole picture of how healthy retention is, and what it's worth to the business.


These three tell you how engaged people are day to day.


- **Cohort retention rate** uses cohort analysis to group users by install date, so you can watch how each group holds up instead of blurring everyone into one number.
- **Session frequency and length** show how often people come back, and how long they stay once they do.
- **Stickiness ratio (DAU/MAU)** divides daily active users by monthly active users, giving you a read on how habitual the app has become.


These tell you what that engagement is worth.


- **Uninstall rate and timing** show how many users leave for good and, just as usefully, when they go.
- **Customer lifetime value** shows what a user is worth across their time with the app. Line it up against uninstall timing and you can see how much value walks out the door when people leave early, and where protecting it pays off most.
- **Attribution** connects your retention work to revenue. Tie campaigns to what users do next and you can see which efforts actually moved retention, and what that was worth.


See how Braze helps brands build retention machines across each mobile channel.


[Connect with sales](https://www.braze.com/get-started)
