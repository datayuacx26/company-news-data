---
schema_version: "1.0.0"
document_id: "d87a29016aa8b3772a588816bea3438b4dcb0820a71145602e735818c3758312"
company_key: "yc-flightfox"
company: "Flightfox"
source_id: "yc-flightfox-news-import-4959b6073008"
canonical_url: "https://www.flightfox.com/blog/b2c-vs-b2b-online-travel"
published_at: "2024-02-12T00:00:00+00:00"
first_seen_at: "2026-07-21T20:27:42.510466+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:d463485d7738b9a974b85d79977e1f975db2c17e11f8b223eba45edf5166f03a"
---

# Our Experience With B2C vs. B2B in Online Travel

To mark the public launch of[Flightfox for Business](https://flightfox.com/teams) , we’ve compressed a lot of our previous experiments into one single post; the mistakes, the lessons, and the successes.


Continue reading for our never-ending journey to product/market fit that started as an idea that “no one would pay for” to $3 million in revenue.


## Fundamentals


We need to start with how we think about building a great business in online travel. In terms of strategy, we focus on the following fundamentals (in order of priority):


1. Customer Value
2. Customer Experience
3. Revenue & Profit Growth


To measure the first two we use the Net Promoter Score (NPS). The NPS asks customers how likely they’d recommend us to friends. While not perfect, it’s a pretty good proxy for the value and experience we deliver to customers.


To read a detailed account of how we improved our NPS from less than 0 to more than 60, check out[How We Measure Customer Happiness](https://flightfox.com/business/how-we-measure-customer-happiness) . Today our NPS approaches 80.


To measure growth, we could simply look at our financial statements, but we’re more concerned about our *potential* for growth in the future. For this we analyze:


- Lifetime value of a customer (LTV)
- Cost to acquire a customer (CAC)
- Compound rate of referrals (viral co-efficient)


These may sound like startup doublespeak, but they form a few simple truths for success in online travel. We wish we took more notice of these truths sooner, but sometimes you have to make the mistakes yourself to see the opportunities.


## B2C


Flightfox began as a B2C-only service; we were a crowdsourced marketplace of travel hackers who helped consumers “travel better for less”. We charged $19 per contest for which our travel experts competed against each other to build the best and cheapest itineraries.


There were a handful of issues with this model:


- **Customer Value:** $19 wasn’t enough for our experts to spend the required time to maximize value; in some cases, it was barely enough to scratch the surface.
- **Customer Experience:** crowdsourcing and competition meant no single expert was responsible for the customer’s satisfaction; if they didn’t fancy their chances of winning, they’d abandon the customer.
- **Revenue & Profit Growth:** our 25% cut meant our LTV was $5–10 (assuming people used us once or twice for complex trips that don’t happen very often), which was a fraction of our CAC, realistically at $50–200.


A few important points about the above:


Firstly, value is a function of price *and* utility. Charging less doesn’t always increase customer value; it can reduce it to zero if utility goes to zero. In retrospect, we needed to charge more to give our experts enough time to create enough utility.


Secondly, very few startups in B2C travel get their CAC below LTV to create a sustainable growth machine. Optimism was our worst enemy. Given our LTV, our CAC needed to be lower than *anything in the history of B2C travel. Ever.* In retrospect, we persisted with this impossible goal for much too long.


Lastly, there are many ways to unknowingly manipulate the CAC calculation. For example, using very short-term experiments, counting organically acquired customers, or assuming the time it takes to acquire customers is free. While $50–200 may sound high, it’s our total long-term cost to *directly* acquire a customer.


In the first year, we delivered considerable value to most of our customers, but "most" wasn’t enough. Growth hit the brakes and we found ourselves crawling in the trough of sorrow.


***


To break even on paid growth (the *holy grail* for an e-commerce startup), we realized we needed to bring our LTV *up* to meet our CAC instead of the other way around. For this, we’d need each customer to pay us $200–800 based on our 25% cut and a CAC of $50–200.


We could achieve this by assuming customers would return many times, but... it was time to get real. Even if we looked many years ahead, we had to consider the *time value of money* , which is tough on startups since the cost of capital is high. We needed customers to use us more often right now, not later.


After many experiments to increase our repeat business, we concluded the following:


- The *average* customer will *not* pay for our service *many* times per year. There are some exceptions in the travel industry (e.g. Uber), but if we’re talking vacation travel, forget it.
- Unless we deliver exceptional value every time, our long time-to-repeat cycle means customers will forget us. We may have to acquire them again, which excludes those subsequent sales from our LTV.
- If we can’t rely on repeat business to increase our LTV, we’re left with increasing our average transaction value.


Even at this stage, some of our customers used us 10+ times per year. But we needed to consider the repeat rate for an *average* target customer, not our best target customer.


Let’s talk about our “target” customer.


***


When we started Flightfox, we thought we knew our demographic and target customer (international trips, multi-city trips, group trips and premium-class trips). That went out the window once we realized we don’t get to choose who uses us; whomever needs us, uses us. So we resigned ourselves to targeting the mainstream B2C market.


This had some noteworthy growth-related benefits:


- The size of our the addressable market was maximized
- Word-of-mouth was maximized (everyone could refer everyone)
- Major growth channels (such as mainstream media) were viable


However, mainstream customers would neither use us enough times per year nor pay enough per trip to make paid customer acquisition break-even.


Our LTV was stuck at $5–10. We were at an impasse.


Suddenly, like the most predictable startup in history, we had the great idea of focusing on niche segments.


“If we have a handful of customers who use us 10+ times per year or pay $300 per trip, how about we only focus on them?!”


We then targeted niche segments, such as luxury travel, adventure travel and sports teams, but we found the following:


1. CAC increases in niche segments due to increased competition for high-LTV customers. The challenge of CAC < LTV just shifts higher to similarly disparate numbers.
2. Going niche reduces the size of your addressable market. This is okay if the niche is still large and/or transaction values are high, but they weren’t high enough in our case.


We didn’t know these facts with absolute certainty for every possible niche, but we had to pick our battles wisely. None of the niches we tested seemed promising enough to warrant the compromise and risk.


This was the first time (of many) that we considered closing down.


***


Like many startups in this position, we resorted to the idea of making our product “viral”. Not viral like cat videos, just viral enough to rely on organic growth.


That may sound naive, especially for a human-powered travel marketplace, but all we needed was for each customer to bring one more, and for that growth to keep compounding.


We read everything on viral growth. A lot of the literature talked about invitation rates, acceptance rates and K-factors, but it all sounded too academic and not remotely practical for our application. We suspected it was more for social networks or mobile apps where you can track these things with some modicum of accuracy.


So we went back to basics.


“Let’s give our customers the absolute best value and best experience possible and bet the house on them telling their family, colleagues and friends, enough so we grow.”


We would also make it easier for our customers to share their experience. We built tight social integrations, introduced expert reviews, and even offered tipping (it’s the ultimate social proof when you see a friend tip).


This was a turning point: goodbye CAC/LTV, hello NPS.


***


Firstly, we increased our pricing.


We went from $19 to $29 and the strangest thing happened. Our NPS shot up. Our refund rate dropped too. We couldn’t believe these events were related, but when we kept increasing our price, again and again our metrics moved. We’d struggled for so long to increase our NPS and here it just happened in minutes.


Interestingly, it turns out pricing is one of the best tools for segmenting. By increasing our price, customers segmented themselves. If they saw good value at $19 but not at $49, maybe they (or their trip) weren’t suited to a human-powered model. These price increases drove our NPS from below 0 to over 20.


Secondly, we removed the competitive crowdsourcing aspect.


Customers loved the concept of experts competing against each other to arrive at the best possible result. But it didn’t work like that in practice. Instead, with only a 20% chance of winning (an average of five experts per trip), the experts only applied a fraction of their potential effort. It wasn’t their fault, they had to optimize their time versus earnings. They couldn’t spend three hours per trip if they were only paid for one out of five.


Crowdsourcing also led to a lot of overlap. The experts didn’t share ideas since it was a competition, so they would perform a lot of the same work. This also meant no single expert was responsible for the customer. They could abandon a trip if they thought they wouldn’t win or it wasn’t worth their time. This created a terrible customer experience. Removing crowdsourcing increased our NPS from the 20s to over 50.


Matching a customer to a single "Concierge" provided a lot more stability and consistency in the experience. It also gave a personal connection where customers felt they could ask anything travel-related. As a business, we were morphing into a traditional travel agent model, but we could clearly see our customers getting more value from an unbiased expert passionate about travel and saving money.


Lastly, we implemented guarantees.


We previously thought of Flightfox as a traditional service provider. The customer would pay, we’d do our best work, but we could only work within the customer’s parameters. If their expectations were unreasonable, we thought they should still have to pay for our time and effort. Bad idea.


Flightfox was still a new idea, one that most people told us “no one would pay for”, so we couldn’t operate like that, we needed to guarantee great results. Once realizing this, we implemented guarantees. Basically, we promised to beat customers’ best quotes by at least our fee. If they didn’t want to find a quote, we guaranteed the lowest possible price.


Our NPS hit record highs between 60 and 85, and they've been consistently within this range ever since.


***


Our customers were happier than ever (more than 90% rated us 9/10 and higher) and most told their friends, but we failed to see month-on-month organic growth.


We found the problem: we were now a casino.


With our new guarantees, we were gambling on beating customers’ quotes, hence gambling on creating customer value. This was great for the customer, because they only paid if they received great value, but it was bad for our organic growth.


It didn’t matter that our service was free when we couldn’t beat a quote, it mattered that the customer remembered we couldn’t help in that instance. It was a negative experience to tell a customer we couldn’t help them, and if it was their first time with us, they were unlikely to try us again.


We couldn’t get rid of the guarantees because they underpinned our promise of customer satisfaction. All we could do was focus on segmenting to beat more quotes and improve our customers’ overall word-of-mouth.


We again tried segmenting, but it was futile. So, building a big business in B2C travel was over.


People told us this was good news. Lifestyle businesses were better. We could now spend less time working and more time traveling. One problem: we didn’t want that. We’d traveled the world many times, we now wanted to build a great business more than anything.


## B2B


We had already run small experiments with business customers, but couldn’t find a competitive advantage worth exploring. From what we’d researched, these customers wanted group bookings and expense reporting, whereas we offered savings on complex itineraries with travel hacking.


We continued looking, but a pivot from B2C to B2B continued to make no sense.


Then it dawned on us.


Surveying customers with questionnaires or half-baked apps is the ultimate self-con. When we'd conducted this type of research before, we knew it wasn't remotely accurate, so why pretend now? If we had to be *in* a market to really *know* a market, what are we waiting for?


Now, with the theory that *no evidence is good evidence* , it was time to get to work.


(We're being somewhat facetious here, but it's really how it played out. We've previously been kicked out of trade shows for surveying customers, which we wore as a badge of honor, but that survey data was useless. Even more thought-out experiments since then involving real payments were naive. That's not a good reason to ignore all customer development, but we were desperate and maybe we were being overly analytical, so it was time to take a leap of faith, even if we had no faith.)


***


We built a basic B2B product as quickly as possible:


- Trip bookings
- Centralized billing
- Team member administration
- Monthly expense and savings reports
- Etc.


And 200+ businesses signed up in the first two weeks.


Even better, we received reams of positive feedback from the most active new customers. They posted new trips each day, we booked their trips each day, and they posted more trips the next day.


But why? Where's the value?


Let’s revisit what each type of customer wants:


- B2C: savings.
- B2B: bookings, convenience, comfort, savings, loyalty benefits, upgrades, reporting, centralized management, and more.


With B2C, we only have one chance to make the customer happy: by providing savings. They may also want convenience and comfort, but with the experience of 60,000+ paying customers, we’ve learned it’s all still in the context of savings.


With B2B, we have many chances to make the customer happy. At a minimum, we must do the work of managing the team’s travel so they can focus on their real work, but that’s only the beginning. We can create much more value.


And this is key: our travel experts know more about airlines, hotels, aircraft, routing, pricing, loyalty programs, and upgrades than anyone on the planet. Not because they’re better trained, but because they’re more obsessed. This isn’t their job, it’s what they do each day, every day, weekends too.


In B2C, most of this skill and experience isn’t useful. But with B2B, it all has value.


Let’s revisit our fundamentals for B2B:


- **Customer Value:** firstly savings. We save customers more than any competitor because our pedigree is exceeding tough expectations in B2C. Then there's the convenience and comfort, but also loyalty benefits. We help businesses optimize their spending to create $1000s in free travel and benefits so they really can “travel better for less”.
- **Customer Experience:** our global network of experts have travelled to 140+ countries and provide 24/7 service. With so much first-hand travel experience they speak the same language. (Want to fly SQ from SIN to the US in F on the A380? No problem. We’ve flown that many times and suggest going via NRT, not FRA, so you get 2x full meal service. Make sure to “book the cook” and request the Lobster Thermidor.)
- **Revenue & Profit Growth:** our LTV for B2B is ~$5,000, which is 250x our LTV for B2C. It’s too early to calculate our CAC, but this higher LTV opens many more potential growth channels such as direct sales and advertising. Having a more consistent value proposition (i.e. no longer being a casino) is so important for growth too.


When we originally tested our model with business customers, we were looking at our competitors, not our own value. We couldn’t see ourselves winning at traditional travel management, but it turns out many potential customers don't want *traditional* travel management. It took a big leap of faith to reframe our thinking, which may not work in every case, but was certainly helpful for us.


***


I hope you enjoyed reading about our ups and downs in building an online travel startup. The ironic thing is we’ve come full circle and are now focused back on CAC and trying to build a sustainable growth machine. We’ll write about those experiments and results soon.


[Flightfox for Business](https://www.flightfox.com/teams)
