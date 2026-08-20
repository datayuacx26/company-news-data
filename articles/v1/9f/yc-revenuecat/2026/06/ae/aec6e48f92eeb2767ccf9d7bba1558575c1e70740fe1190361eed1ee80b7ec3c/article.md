---
schema_version: "1.0.0"
document_id: "aec6e48f92eeb2767ccf9d7bba1558575c1e70740fe1190361eed1ee80b7ec3c"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/why-react-native-apps-make-more-money"
published_at: "2026-06-03T15:02:21+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:145169009c39236fba95cf49cfee8a9d7b95471f31eaab78ce17195e8e3afc08"
---

# Why are React Native apps making more money?

Earlier this year, we released the newest version of our[annual State of Subscription apps report](https://www.revenuecat.com/state-of-subscription-apps/) (SOSA), which contains a huge amount of interesting insights for app builders. One of these insights was how well apps with different development stacks (also referred to as frameworks in the report) are performing when it comes to monetization.


This one insight turned out to be rather controversial, despite the previous year’s data showing similar results: React native apps are monetizing better than Flutter and native apps. Specifically, they are excelling in:


- Higher conversion from download to paid
- Higher revenue per install
- Higher lifetime value


When I[tweeted about it](https://x.com/plahteenlahti/status/2029961382121050527) , some developers were quick to question the results, the dataset, correlation vs causation, and essentially everything that goes against the “intuition” that technology is the key to building great apps that customers want to pay for. This article is about looking at the data and figuring out the why.


## How is this data collected?


The data for the State of Subscription Apps report is collected from 115,000 apps, making a combined $16 billion in revenue. RevenueCat tracks more than the mentioned amount of apps, but only apps that have active subscription revenue, meet a minimum threshold of installs and revenue (to ensure statistically meaningful findings) are included in the report. Apps come from indie teams to mid-size organizations and large publishers.


The time frame for the data is 2025. This means that the data set includes apps built with agents and vibe coding tools. All data is anonymized and aggregated, ensuring that no single app’s performance metrics are individually identifiable. The findings are presented as aggregated performance benchmarks across segments, categories, and platforms.


### Understanding the visualizations


Before diving into what the numbers tell us about React Native, Flutter, and native apps, it’s important to understand the structure of the SOSA dataset and how its visualizations work, since I’ll be showing them in this article.


The report uses box-and-whisker (candlestick) charts to visualize the data:


- **Lower ‘whisker’: marks P10 (10th percentile),** the bottom 10% of app performance.
- Bottom of the ‘box’: marks P25 (25th percentile) and represents Q1 (bottom quartile) — apps below this make up the lowest 25% of performers.
- **Marker inside the ‘box’:** marks P50 (50th percentile) and represents Q2 (median) — this is the midpoint of app performance.
- **Top of the ‘box’:** marks P75 (75th percentile) and represents Q3 (upper quartile) — apps above this fall into the top 25% of performers.
- **Upper ‘whisker’:** marks P90 (90th percentile) — apps here or above are the highest performers.


This structure gives a more realistic picture of how apps perform in the real world. The “box” shows where most apps cluster, while the whiskers show the spread between weaker and exceptional performers. When we talk about React Native outperforming other stacks, we’re referring to these distribution points — not isolated outliers.


## React Native leads in download to paid conversion


The first chart to look at is the Download-to-paid conversion rate (D35): the share of installs that result in at least one paid subscription within 35 days of the install date. This metric captures how effectively an app converts new users into paying customers in the first ~5 weeks.


The first thing you should notice from this chart is that React Native apps have the highest median conversion rate at 2.5%. That might not sound like much, but it is still 25% higher than native and Flutter. However, the absolute spread between stacks is modest compared to within-stack variation.


After that, pay attention to the top whisker of React Native. Being higher than others means that top React Native apps are outperforming other stacks; at the same time, the results are very widespread. Some React Native apps do great, others don’t.


### First key learning: execution matters far more than stack choice


If you look at the spread inside each stack, you start to get a picture of the real insight in this data: the variance within a stack absolutely dwarfs the variance between stacks.


Put another way, the top performers convert to paying customers 10+ times better than the bottom performers in their own stack. Execution matters more than the stack itself.


What’s actually pulling the top React Native apps ahead? Probably not the stack itself. More likely, it’s the teams building them. These teams have to focus on two things to win: building a great app and monetizing it effectively. Stack choice can enable you to do that, but it is not a magic spell that doubles your conversion.


## React Native apps generate more revenue per install


The second chart to look at is the revenue per install (RPI) chart. This chart tells us how much revenue an app makes 14 days after a user installs it. From the chart, we can see that React native leads with a median of $0.34, which is 55% higher than native and 79% higher than Flutter. The top quartile of React Native apps is also making more money than the top quartile of Native or Flutter apps.


Despite React Native’s numbers looking good, there is a huge difference between apps using the same stack. Some apps are only making $0.10 per install, compared to some making $2.58. Similar patterns exist across every stack. The difference between top-performing and bottom-performing apps is far larger than the difference between the stacks themselves.


### Second key learning: feedback loops


Stack choice appears to have a relationship with monetization outcomes. One possible explanation is that successful subscription businesses tend to optimize relentlessly based on customer feedback.


As discussed in the previous section, building great products is largely about shortening the time between learning something from customers and acting on it. The faster a team can understand what customers want, which features they value, and what they are willing to pay for, the faster it can improve the product. Over time, this creates a powerful feedback loop that compounds into a better user experience and stronger monetization.


React Native can be a strong fit for this style of development. Services such as EAS and Codemagic allow teams to ship many changes instantly without app reviews, while tools like RevenueCat enable teams to experiment with pricing, offerings, and paywalls without requiring a new app release. Together, these tools can reduce the time between identifying an opportunity and delivering an improvement.


That said, the chart still suggests that execution matters more than stack choice. The difference between the highest- and lowest-performing apps within each stack is much larger than the difference between the stacks themselves. While React Native apps generate more revenue per install on average, the data does not suggest that React Native is a prerequisite for strong monetization.


## React Native pulls further ahead by day 60


Download to paid percentage, and revenue after 14 days tells about the early success React Native apps have with customers. Looking at the revenue per install after 60 days starts to tell a stronger story of React Native’s monetization performance. React Native not only seems to hold the gap, but actually widens it to other development stacks.


With median revenue per install at $0.51, compared to the $0.31 for native apps and $0.29 for Flutter apps, React Native apps earn almost 65% more revenue per install than the former, and 76% more than the latter. For reference, the numbers were x and y for 14-day revenue per install.


However, as with the day 14 data, the biggest differences are not between stacks but between apps. Within React Native alone, revenue per install ranges from roughly $0.16 at the lower end to $3.60 at the upper end.


### Third key learning: monetization is a product problem


If monetization gains persist for two months, the explanation probably isn’t a better payment process. Realistically, it’s about the product being great. Monetizing well beyond the first month means that apps built with React Native are clearly valuable products that users want to pay for. To get to that, you need to be a product-oriented person or a team.


Something in React Native makes product-oriented teams choose it. Reasons are many, but they most likely come down to the benefits that it provides, most of which I’ve already mentioned. When I originally posted about how well React Native is monetizing, all the people who challenged the results were engineers. From an engineering perspective, React Native is easy to challenge; there’s a bunch of weird stuff if you compare it to something like Swift.


However, most customers are not engineers, and even if they are, they don’t approach the products they use from an engineering perspective. For them, it’s more important if the product solves the problem, not how.


What does this mean in practice for a developer who is building an app? First, focus on evaluating the benefits of the development stack from a product perspective: what gives you the best results with your current capabilities. Second, validate with a stack that gives you results the fastest. Nothing’s stopping you from going from React Native to Swift UI after you’ve built the first version of the app, and you feel that your technology of choice is the limit. As long as you don’t choose a technology based on what other engineers said, you’re probably good.


## Year-1 Retention by plan type shows only modest differences across stacks


Here’s the real story of this article: retention at one year barely differs between Native, Flutter, React Native, or any other stack. That means React Native’s massive lead in conversion and revenue cannot be explained by better long-term retention. Users don’t stay subscribed longer in React Native apps. They stay about the same.


And that is the interesting part, because it tells us exactly where the advantage must be coming from:


- React Native apps convert more users earlier
- React Native apps extract more revenue per install
- But when it comes to keeping customers for a year, all stack perform similarly


In other words, React Native wins by front-loading value, not by keeping users longer. If an app reliably solves a recurring problem, users stay subscribed. Data tells us that technology choices matter less in the long term, at least when it comes to keeping your customers.


### Fourth key learning: play long-term games


The retention data tells an important story: stack choice has very little impact on whether customers stay subscribed over the long term. Annual retention rates are remarkably similar across Native, Flutter, and React Native apps.


Your app business should not be about stacks and hacks, at least if you’re in it for the long term. It’s possible to build a decent business by monetizing a loved product with a small but significant audience. As long as you keep those customers.


Frameworks can help teams move faster, experiment more often, and improve their products more quickly. But they cannot replace the fundamentals: understanding customer problems, building features people genuinely need, and delivering enough value to justify an ongoing subscription. Do that, and you keep your customers for longer. The longer you keep your customers, the more money they make. More money long term will eventually win over apps that convert better, or charge more (unless they also retain users better).


Retention data here is a reminder that customer loyalty is earned through product value, not technology choices. If customers continue finding value in your product months after they subscribe, they’ll keep paying regardless of whether the app was built with React Native, Flutter, or native technologies.


## React Native delivers the highest RLTV per paying user — by a wide margin


While the previous section brought some hope for people thinking that React Native is just an anomaly in early-stage monetization, our last chart about realized lifetime value (RLTV) is going to shatter that hope (at least a bit).


RLTV measures how much revenue a paying customer generates during their first year. In other words, once someone becomes a subscriber, how much are they actually worth to the business?


And the results are hard to ignore. The median React Native app generates $31.78 in first-year lifetime value per payer. Native and Flutter apps, meanwhile, are clustered around $21. That’s a difference of roughly 50%.


The gap isn’t limited to the median either. The top React Native apps generate more value from paying customers than the top Native and Flutter apps, reaching $95.32 in RLTV compared to $74.68 for Flutter and $61.81 for Native.


At this point, a pattern starts to emerge. React Native apps convert more users into customers. They generate more revenue after 14 days. They generate more revenue after 60 days. And now we can see that their paying customers are worth substantially more over the course of a year. As we saw in the last section, there’s no meaningful difference in retention between the stacks. Yet React Native apps still generate significantly more value per payer.


That suggests the explanation is not retention alone. React Native apps appear to be doing a better job of monetizing the customers they acquire, whether through pricing, packaging, product positioning, customer quality, or a combination of all four.


### Fifth key learning: valuable customers beat more customers


One of the most encouraging things about this data is that it doesn’t suggest you need millions of users to build a successful app business. The apps that perform best are not necessarily the ones attracting the most customers. They’re often the ones creating the most value for the customers they already have.


Looking at the retention data, React Native apps don’t appear to keep subscribers significantly longer than Flutter or native apps. Yet those same subscribers end up being worth substantially more over the course of a year.


That should be good news for builders. It means success isn’t just about growth at all costs. You don’t need to win every download, dominate every category, or have the largest audience. Sometimes the biggest opportunities come from deeply understanding a smaller group of customers and building something they’re genuinely happy to pay for.


The best subscription businesses are often built this way. They solve a real problem, deliver enough value that customers stick around, and continuously improve over time. When you do that, each customer becomes more valuable. Not because you’re extracting more from them, but because you’re giving them more reasons to stay.


## Predictions for next year’s State of Subscriptions app report


We now have two years of React Native leading the framework monetization section (report from 2025 showed similar results). The question is, of course, whether we still see similar results next year.


Last year we started a little bit of the agentic coding workflow, which is now the default for most developers. It will be interesting to see how that might change the results. Previously, React Native was argued to be the easy stack to land on if you were a beginner, or came from, for example, a web development background. Now that advantage is gone. You can easily build apps with any language, without having to understand it yourself.


What most likely happens is that we will get more apps than ever before, built in a variety of technologies. If developers focus on the principles of building great apps, the ones that this article has hammered on as well, we most likely see stabilization in the data. Perhaps a slightly hopeful prediction is that differences between frameworks will get smaller. Hopefully not at the cost of conversion, retention, and LTV. Realistically, we will see all of those go down, as the quality of new apps is generally low still.


But that could, of course, change as we are only halfway through the year.


[We recently rolled out support for tracking ad revenue and impressions](https://www.revenuecat.com/docs/getting-started/ad-monetization) alongside your subscription data. We will hopefully have this in next year’s report, as it might tell a completely different story when it comes to monetization. Who knows, maybe Flutter apps take the lead because they are better at utilizing subscriptions and ads together?


## Conclusion


In this article, we’ve looked at all the different ways React Native apps are monetizing better than native and Flutter apps, and the reasons behind that. At each section, I’ve done my best to summarize how a developer should interpret this data and make use of it. Even if they’re not building with React Native.


The lesson for developers is simple: focus on building a tight feedback loop with your customers. Whether you’re using React Native, Flutter, or native technologies, the teams that learn fastest, iterate fastest, and continuously improve their product are the ones most likely to win. The strongest monetization outcomes don’t appear to come from a secret framework feature or growth hack. They come from building products that people truly value.
