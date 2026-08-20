---
schema_version: "1.0.0"
document_id: "cc210f640441dabc45268c5346516c34a2e983608a1f963ae68f1219e1ea6de4"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/growth/optimize-funnel-metrics-mimo"
published_at: "2026-05-19T15:07:31+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:c9ee510b452bfd9767153c84d614196f3777fd61f7179903c156930d47ab0e98"
---

# This is not a playbook: how Mimo grew customer LTV by 65%

Many in the app business are looking for *the* playbook. You know the one — a canonical set of moves that will grow LTV, lower CAC, and compound into a healthy, scaling business.


After a decade of growing mobile apps and working at every step of the funnel as an IC, I still don’t have a playbook. What I do have by now is a good picture and a wealth of lessons on how businesses can turn around within 6–12 months, once the funnel is understood and the right problems are tackled.


This article isn’t a magic playbook, but I will share examples of what to watch out for when it comes to a fully-functioning funnel vs. a set of hectic moves that barely move the needle, and how to engineer growth in your favor.


## **Viewing the funnel as more than the sum of its parts**


I’ve been lucky to have worked with a few apps at their inflection point, when the LTV/CAC math stops working, and the path forward isn’t obvious. Some we turned around in six months. Others grew MRR 10 to 25x in a year.


Regardless of where I’ve worked, I’ve seen a throughline: there is a **dense web of interdependencies between every part of your funnel** . It may sound obvious, but, in practice, it’s so easy to end up looking at the KPIs in silos instead of seeing the funnel as a *whole* .


A change to your paywall affects your plan distribution, which in turn affects[lifetime value (LTV)](https://www.revenuecat.com/glossary/#lifetime-value-ltv) and ultimately determines how much you can spend on acquisition. A change to your acquisition strategy affects who enters your funnel, which in turn affects activation rates, retention, word of mouth, and the revenue that cohort generates.


Pull one thread, and the whole fabric moves — sometimes in the direction you wanted. So, the most valuable thing one can do is learn to manipulate how the fabric moves.


When I took on the growth challenge at Mimo (back then, it was an app that taught you how to code, but it has since evolved significantly), we were facing a situation that every growth-stage app eventually hits: **LTV had plateaued while customer acquisition costs kept climbing** .


Standing still wasn’t an option for a bootstrapped company. Scaling down (in the worst-case scenario) meant giving market share to competitors, losing the event volume needed to optimize campaigns, and losing data, organic downloads, and momentum. Paid acquisition was one of our core growth loops alongside organic traffic, and we had to fix the unit economics or stop spending.


I was fortunate to lead both product growth and marketing simultaneously, allowing me and my teams to optimize the funnel end-to-end. What I learned from that vantage point is that the critical skill lies in **understanding what you’re actually testing, and what the downstream consequences might look like.**


### **One prerequisite for success**


At that point at Mimo, we didn’t sit down and debate if we should ‘increase LTV’ or ‘decrease CAC’. We *already knew* from user research and data where people got stuck, what made them question the upgrade, and what made them leave. We knew that:


- The trial anxiety was causing friction because we read it in store reviews every day
- Users didn’t perceive the app as a serious learning tool because we weren’t always consistent in communicating it this way
- The language barrier was costing us because the users were vocal about it


Example of one of the many research projects we were regularly conducting


**But we also knew our target users, what they needed, and what set us apart from the competition.** If you don’t have a foundation from user research, don’t know the qualitative signals, or don’t have an honest analysis of where value breaks down, start there.


## **Interdependency #1: your paywall changes a lot downstream**


The[paywall](https://www.revenuecat.com/blog/growth/paywalls-study-guide/) is the most obvious lever for conversion. But it’s also a distribution mechanism: for plans, price points, user intent, ultimately LTV.


### **The trial screen that changed our trajectory**


The paywall change that had the biggest impact at Mimo was shifting to the ‘honest trial paywall’ or ‘[Blinkist paywall](https://www.revenuecat.com/blog/engineering/how-to-build-a-blinkist-style-paywall-using-revenuecat-webhooks-and-zapier/) ’: a clear, transparent explanation of how the free trial works. When Blinkist shared[the results of this experiment](https://uxplanet.org/how-solving-our-biggest-customer-complaint-at-blinkist-led-to-a-23-increase-in-conversion-b60ad514134b) , I wanted to test it immediately because we were seeing the same anxieties about trials in our reviews every day.


Our results were great: **trial opt-in rates more than doubled** , and **[conversion from trial-to-purchase](https://www.revenuecat.com/blog/growth/app-trial-conversion-rate-insights/) improved by 50%** . The push notifications and emails I’d set up to remind users of the trial expiration didn’t have a strong negative effect on trial cancellations, which was a relief.


But the downstream effect on LTV was the more interesting story: most of Mimo’s trials and purchases were happening during onboarding. Users who started a trial and didn’t cancel were our primary paying customers. The secondary segment was coming through a discount campaign later. So by dramatically increasing the number of users opting in and converting at full price, we shifted the plan mix: **more users paid full price rather than a discounted rate** . The improvement in[trial conversion rate](https://www.revenuecat.com/glossary/#trial-conversion-rate) cascaded directly into an LTV lift.


Before


After (1)


After (2)


**The general principle:**[paywall changes](https://www.revenuecat.com/blog/growth/paywall-redesigns-case-studies/) don’t just move trial and/or purchase rates. They shift who pays, how much, and at which stage of their journey.


### **Price changes don’t just change revenue**


When we raised Mimo’s yearly price by 20%, the obvious effects were higher ARPU and higher subscriber LTV, as the conversion rate hadn’t changed. But pricing changes also shift how users distribute across plans. A more expensive[annual plan](https://www.revenuecat.com/blog/growth/annual-subscriptions-apps-pros-cons/) makes the monthly plan look like a lower-commitment alternative (which most of your users need at the beginning), and affects renewal behavior down the line.


These distribution effects are often invisible in short-term (30-day) analysis, so you need to model them across the full cohort lifecycle.


I’ve also seen the reverse: prices that are way too high for where the app sits in users’ minds. Most users won’t have had a deep (or any) experience with the product by the time they hit the paywall. For better or worse, your app has already fallen into a mental category with a certain willingness-to-pay before they even open it. If the price doesn’t match that expectation, you can:


1. Lower your price
2. Make sure you nail communicating the value through your onboarding, before the paywall
3. Do both of the above


Lowering the price can sometimes be the price you need to pay to get those first paying customers in. From there, you can analyze these segments, how they use the product, and whether they derive any value from it. This paves the way to the[product-market and product-model fits](https://www.revenuecat.com/blog/growth/product-market-fit-subscription-apps/) .


It’s not every app’s case, and most of the time, increasing the price is what apps end up doing, but if your users are telling you that’s why they’re not upgrading, I recommend treating it as an investment in the insights you need.


**The general principle:** A price change can reshape your subscriber mix, not just your ARPU.[The right price](https://www.businessofapps.com/video/the-value-of-pricing-research/) isn’t the highest price the market will bear, it’s the one that attracts users with the intent and means to stay.


### **Offering a trial on only one plan can change how users perceive other plans**


Similarly, offering a free trial only on the yearly plan makes the yearly plan more attractive, which is usually the goal, as we want a higher share of yearly subscribers.


But depending on your target markets and demographics,[users who aren’t ready to commit to a year](https://www.revenuecat.com/blog/engineering/monthly-subscription-12-month-commitment/) , who still feel anxious about trials (no matter how much reassurance you add), and who are unsure about the product value, might be treating the monthly plan as their trial. They subscribe for a month to test the product, cancel immediately to avoid being charged on the stores’ schedule rather than their own, and leave silently.


The monthly conversion numbers look fine; you see your total active subscribers number growing, but the churn rates a month later tell the real story.


If this sounds (or looks) like your reality, start asking your short-plan customers why they are canceling. It might be they see no other way to try out your paid plan, but believe they’ll re-subscribe later if they like it.


**The general principle:** some users might treat shorter plans as a trial run. Learn if this is happening, and make sure to separate ‘real churn’ from plan upgrades.


### **Trial duration is not universal**


Data from RevenueCat’s[State of Subscription Apps](https://www.revenuecat.com/state-of-subscription-apps/) report shows shorter trials have huge day 0–1 cancellation spikes (over 55% for three-day trials vs. 31% for 30-day trials), but there is no universal trial length that *will* boost conversion.[The right trial length](https://www.revenuecat.com/blog/growth/7-day-trial-subscription-app/) depends on your product’s activation curve, which is invariably linked to your app category.


Trial durations, by category — State of Subscription Apps report 2026


Put simply, three-day trials can work well for utilities and for weekly plans, where time-to-value is short, and users quickly know whether they want to continue. Seven or 14 days often work better for education and health apps, where habit formation takes more time.


At Mimo, we were offering 30-day trials for a long time. Trial opt-in rates were high, but by day 30, so were the[cancellation rates](https://www.revenuecat.com/blog/growth/app-cancellation-flow-best-practices/) : users assumed they’d exhausted the content, or they hadn’t built a habit and drifted away. Shortening the trial to 14 days and communicating the depth of content more clearly helped address this.


**The general principle:** trial length should match your product’s activation curve, not your optimism about how quickly users will fall in love with it. A longer trial doesn’t automatically mean better conversion. If users aren’t getting activated and retained, you’re just giving them more time to cancel.


### **Layout and design should help address purchase barriers**


For years,[trial timeline screens](https://www.youtube.com/watch?v=aJp7m4TYK7E) showing users exactly how and when they’d be charged were among the most successful paywall formats. But it doesn’t have to be the only one.


Do your users understand the trial mechanic? Is trial anxiety a barrier for them? Do they understand the difference between your free and paid plans? Are they even aware of the paid plan? Maybe your free plan is too good, and you’re not pushy with upgrades?


The concerns users have before upgrading should help you prioritize experiments between premium feature lists, benefit-led copy, explanations of how the trial works, and promising the reminder or social proof and cancellation policy FAQ. **Start from the upgrade barriers and find a way to solve them with design and copy.**


Last year, I came up with this ‘paywall anatomy’ to visualize the multiple areas one can leverage. In my experience, most of the leverage lies in pricing and packaging.


## **Interdependency #2: acquisition quality flows through the entire funnel**


Who you acquire determines how they activate, retain, convert, and refer. Changes to your acquisition strategy don’t just affect CAC but many other[downstream metrics](https://www.revenuecat.com/blog/growth/metrics-for-scaling-paid-ads/) , often with a delay that makes the connection easy to miss.


### **Creative concepts aren’t just for solving ‘the click problem’**


At Mimo, our earlier ads were animated and visually playful. Hook rates and CTRs were good. But cartoon-style visuals were likely signaling ‘game’ and ‘easy’ rather than ’serious learning tool’, which subtly suppressed the intent of users who did download.


When we shifted toward more professional imagery and videos in ads and store listings — cleaner, expert-looking, with more prominent references to coding — we didn’t just see[top-funnel improvements](https://www.revenuecat.com/blog/growth/ugc-ads-apps/) . The profile of users entering the funnel changed. Also, over time, this helped us shift the narrative that learning to code on mobile was not only possible but also worth paying for, before we rebranded a few years later.


This was evolution, not revolution, though


**The general principle:** the creative that brings someone in sets their expectations for everything that follows.


### **Seasonal campaigns and discounts can bring high-volume, but low-intent cohorts**


Seasonal moments are real: Q5 with New Year resolutions, Black Friday sales, or Back-to-School. Conversion rates are naturally better, and revenue spikes. It feels great. But the problem can manifest as subscriber churn a few months later.


Users acquired during such seasons often have strong momentary motivation but lower sustained intent. I worked with a few apps where paying subscribers who started in late December would barely open the app in January. The revenue was real, but the retention wasn’t.


Similarly, frequent discounting generates revenue in the moment while degrading your customers’ LTV.


Here’s the dynamic I often see when teams try to ‘manipulate’ growth by **attracting users and customers via cheaper ad networks, false claims in ads, and non-stop discounts, until users develop ‘discount blindness’:**


**The general principle:** observe your subscriber cohorts over their lifetimes, not just at the outset. Check trends from previous years to see how the seasonal cohorts developed, as not all revenue is created equal.


## **Interdependency #3: what happens inside the app doesn’t stay inside the app**


User retention doesn’t just affect users’ and customers’ lifetimes. It can also flow back up the funnel in ways that are easy to miss.


### **The language barrier was costing us across every metric**


Years ago, we decided to[localize the Mimo app into six languages](https://www.revenuecat.com/blog/growth/price-localization-for-apps/) after testing it in two languages. **The intent was to remove the language barrier for users who were already downloading the app** **and had already cleared many psychological hurdles:** thinking they weren’t smart enough, or young enough, or were bad at math, or that coding just wasn’t for them.


The positive results rippled up through the entire funnel. A better in-app experience led to higher ratings and reviews, which increased our download conversion rate and helped lower blended CAC. Better retention in core markets (D7 retention increased by +40% on average), and to a 25%-100% increase in purchase rate.


The lower the market’s adoption of English, the higher the[activation metrics](https://www.revenuecat.com/blog/growth/activation-metrics/) moved. An activation and retention investment became a lever across the full funnel.


**The general principle:** activation and retention improvements don’t stay in their layers. They impact CAC, organic growth, and word-of-mouth.


### **Where you invest in retention depends on where your revenue actually comes from**


We invested significant effort into[managing churn and improving subscriber retention](https://www.revenuecat.com/blog/growth/subscription-app-churn-reasons-how-to-fix/) by implementing payment-failure reminders, trial-expiration reminders, extensive value messaging via automated lifecycle campaigns, and in-app cancellation flows to reactivate users and customers. None of it significantly moved the needle on LTV.


In hindsight, this was not truly necessary. Most of our revenue at that stage came from new subscriptions, and the churn cohorts simply weren’t large enough to drive a meaningful uplift, even if we convinced them to stay.


Resubscribe offer evolution


Payment failure reminders


These days, some of these solutions are much easier to implement. I’d recommend covering your bases with:


- Adding subscription benefits on Google Play
- Utilizing[Apple’s Retention Messaging API](https://www.revenuecat.com/blog/engineering/apple-retention-messaging-api/)
- Improving[handling of refund requests on the App Store](https://www.revenuecat.com/docs/platform-resources/apple-platform-resources/handling-refund-requests)
- Introducing automated (or personal) emails asking for trial and purchase cancellation reasons


**The general principle:** subscriber retention optimization is the right investment when recurring revenue is a significant portion of your total revenue, or when you’ve hit your growth ceiling (when the number of churning subscribers starts exceeding the newly acquired ones). Before that, you might be solving a problem that doesn’t exist.


## **Interdependency #4: organic loops for lowering blended CAC**


Viral and organic levers feel like free growth. But, most of the time, they have natural ceilings set primarily by your product category and your users’ actual behavior. And it’s worth testing at a small scale before a big investment.


### **3x our k-factor wasn’t the win it sounds like**


We rebuilt our referral program: double-sided rewards, 14 days of free premium for both parties, simplified sharing options, and multiple bug fixes to ensure rewards are delivered. These efforts tripled our k-factor.


Tripling sounds significant. But, in this case, a triple from 0 was still pretty much 0. The work was directionally correct and cost-effective. We didn’t let it distract us from the primary focus after we determined it wouldn’t be a significant growth driver.


Before


After


**The general principle:** the ceiling is usually determined by your app’s category, your target segment, and how much they want to and can share the word about your app. Less often by the size of the reward.


### **Content sharing requires the product to be built for it**


At Mimo, we were among the first apps to implement the share-to-stories feature, working directly with Facebook when the feature was still in beta. We embedded sharing buttons at *aha!* moments throughout the user journey, such as starting and continuing a streak, moving to a higher league, and other milestones. Many users were motivated by it and shared their progress on social media daily.


But I’ve seen this feature fail when working with other apps or trying to add it to other parts of the journey. It works[when the product generates genuinely shareable moments](https://www.revenuecat.com/blog/growth/ryan-jones-flighty-launched-podcast-2025/) : when progress is visible, meaningful, and something the user actually wants to show off (for example, this was not the case with Playgrounds, where users could practice and build their projects but were far from being proud of them).


The very first iteration of the Share-to-Stories feature


**The general principle:** you can’t retrofit shareability. Sharing buttons on top of a mediocre experience just get ignored. The foundation needs to be users feeling proud of their progress.


## **What actually grew our LTV, and three questions before every experiment**


Ultimately, over about three quarters, we managed to reverse the trend and grow cLTV by 65%, decrease paid CAC by 20%, and increase monthly proceeds by 35%. In less than 12 months, we were back on track, with a healthy blended[LTV/CAC ratio](https://www.revenuecat.com/glossary/#ltvcac-ratio) , which we maintained for the years ahead.


For the record, here’s what ultimately moved the needle at Mimo:


- **The honest trial paywall** : doubled trial opt-in, lifted purchase rate by 53%, and improved LTV by increasing the percentage of full-price subscribers
- **Change in creative direction** : changed the perception of users entering the funnel via ads and search visibility on the stores
- **Localization** : produced retention and conversion improvements and positive effects at the top of the funnel
- **A 20% yearly plan price increase** : raised ARPU and cLTV, while we monitored the distribution shift and conversion rates carefully
- **Improvements in organic acquisition** : extensive ASO work, as well as less meaningful initiatives such as increasing the referral program’s efficiency and implementing a content-sharing loop
- Many other improvements that compounded


None of the above are universal prescriptions. But there is a discipline: **understand what you’re testing, track what it changes beyond the primary success metric, and follow the KPIs all the way down (or up) the funnel.**


Before making a change, ask:


1. What is the expected **first-order effect and primary KPIs** ?
2. What’s the **downstream consequence and secondary KPIs** : plans distribution, user intent level, cohort quality, etc.?
3. What **might get worse as a result** of this getting better, aka the tradeoffs?


The dangerous scenario is when you optimize aggressively in one area, see a metric improve, declare a win, and miss the lagging negative effects elsewhere. Seasonal campaigns look like wins in December and early January. Heavy discounting makes you feel like a monetization genius until the cohort’s LTV matures and users start complaining.


There’s no playbook.


The apps that scale well aren’t the ones that find the best individual tactics. They’re the ones that build the clearest picture of how their funnel and value delivery actually work, then optimize with this picture in mind.
