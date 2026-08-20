---
schema_version: "1.0.0"
document_id: "38a044fe3bc8568926331d16ff27a487daac48959c233e984dcc9302e5e84a6b"
company_key: "rackspace-technology-inc-common-stock"
company: "Rackspace Technology Inc."
source_id: "rackspace-technology-inc-common-stock-news-import-038771c82b17"
canonical_url: "https://spot.rackspace.com/blog/history-of-spot-instances"
published_at: "2026-01-16T00:00:00+00:00"
first_seen_at: "2026-07-22T10:47:01.830546+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:57e72b99194faf8263803b40d4fc92f9764e99b3f3d4b21d74b29395ae328541"
---

# History of Spot Instances

## TL;DR


‍


Spot Instances enable cloud providers to monetize idle data center capacity at steep discounts (50-90% off). Amazon Web Services (AWS) pioneered auction-based Spot markets in 2009, but abandoned them in 2017 for provider-managed spot pricing through price smoothing, where prices change gradually based on longer-term supply and demand trends. Google Cloud Platform (GCP) and Azure never used auctions at all, relying instead on provider-managed pricing models. In 2024, Rackspace revived the auction model with full transparency.


**Key Takeaways:**


- **1960s-1990s:** Researchers proposed auctions as the solution to allocating scarce compute resources
- **2009-2017 (AWS Auction Era):** AWS ran Spot as an auction where users bid for spare capacity. Despite appearing market-driven, researchers found prices were algorithmically controlled with hidden reserve prices.
- **2017 AWS Shift:** AWS abandoned auctions for provider-managed Spot pricing with price smoothing. Ironically, this made spot instances *more expensive* on average. Users lost the ability to capture rock-bottom prices during low-demand periods.
- **2023 Reality:** Research shows AWS Spot prices increased significantly in major regions. AWS deliberately increased prices to push users toward less-popular instance types and reduce congestion on popular ones.
- **2024 Rackspace Spot, The Transparent Alternative:**[Rackspace Spot](https://spot.rackspace.com/) revived auction-based pricing with full transparency. Your bid actually matters, prices reflect real supply and demand, and you can see exactly how the market works.
- **GCP and Azure's Approach:** Both GCP and Azure use variable pricing that adjusts based on supply and demand. Neither provider ever used auctions.


**Bottom line:** Understanding how Spot pricing works, and which provider you choose, directly impacts your cloud costs. Provider-managed opaque pricing from AWS, GCP, and Azure may cost you more than transparent auctions where you directly participate in price formation.


‍


## Introduction


‍


***“We should probably be using spot instances more.”***


This realization comes up on many engineering teams, and it’s usually followed by hesitation. Spot Instances promise significant cost savings, yet many teams still hesitate to run them in production. Not because teams are unaware of them, but because reasoning about them in practice is difficult.


At their core, spot instances are unused cloud capacity sold at steep discounts, often 50–90% below on-demand prices. The tradeoff is that this capacity can be reclaimed with little notice when demand rises.


Even so, there are clear examples of Spot being used successfully at scale. When Chegg adopted spot instances for their microservices infrastructure in 2016,[they achieved a 70% reduction in EC2 costs.](https://www.flexera.com/resources/case-studies/chegg) OpsMx, a DevSecOps platform provider, initially tried AWS Spot but found it "only marginally cheaper than savings plans" and hard to justify. By migrating to a transparent auction model like Rackspace Spot,[they achieved an 83% reduction in infrastructure costs.](https://spot.rackspace.com/case-studies/using-spot-instances-to-maximize-cloud-cost-efficiency-and-performance-opsmxs-experience)


***Transparent auction model?***


This naturally raises other questions:


- What other pricing models exist?
- How do different cloud providers determine their prices?
- How do these different models shape the spot market?
- And, most importantly, how do these mechanics affect instance interruptions and cost savings?


This article answers these questions and traces the evolution of spot instance markets. You’ll explore the foundational research that proposed market-based allocation to handlescarce compute resources, then see how cloud providers implemented these ideas.


AWS plays a central role in this story because EC2 Spot was the first large-scale public implementation of computational markets, shaping how both researchers and practitioners understood pricing, interruptions, and cost optimization.


You'll see how[AWS initially ran its Spot service as an auction](https://aws.amazon.com/about-aws/whats-new/2009/12/14/announcing-amazon-ec2-spot-instances/) where users bid for capacity, why it[abandoned that model for price-smoothed pricing in 2017,](https://aws.amazon.com/about-aws/whats-new/2017/11/amazon-ec2-spot-introduces-new-pricing-model-and-the-ability-to-launch-new-spot-instances-via-runinstances-api/) making AWS Spot resemble the transient offerings from Google Cloud and Azure, which had always used provider-managed pricing models.


You'll also discover how AWS maintained an opaque approach during it’s auction-era, using undisclosed mechanisms to balance revenue, utilization, and capacity risk.


Then learn how users and researchers responded by reverse-engineering algorithms and developing increasingly sophisticated strategies to extract savings.


Finally, we examine the current spot landscape and how[providers like Rackspace Spot are pioneering transparent spot auction markets](https://spot.rackspace.com/blogs/reclaiming-the-cloud-with-rackspace-spot) .


Understanding this history will help you clarify why spot behaves the way it does, and how much of the cost advantage is driven by market-design.


‍


## What are spot instances?


‍


Spot instances are often introduced with a **single, recurring headline** : *save up to ~90% compared to On-Demand pricing.*


The exact number shifts depending on who’s doing the talking.[AWS](https://aws.amazon.com/ec2/spot/?cards.sort-by=item.additionalFields.startDateTime&cards.sort-order=asc&trk=6e296f5a-ce1c-4d4c-901f-c925fdb9c870&sc_channel=ps&trk=6e296f5a-ce1c-4d4c-901f-c925fdb9c870&sc_channel=ps&ef_id=CjwKCAiAu67KBhAkEiwAY0jAlQxVCxT65mC9iVEH00ioWR6pA9cxfUfudsPEqmowcve9lOy5sTda-RoCGJAQAvD_BwE:G:s&s_kwcid=AL!4422!3!669080180281!e!!g!!spot%20instance&gad_campaignid=20443322872&gbraid=0AAAAADjHtp8tBOjKwGQ0u7jNOqJrgGwSw&gclid=CjwKCAiAu67KBhAkEiwAY0jAlQxVCxT65mC9iVEH00ioWR6pA9cxfUfudsPEqmowcve9lOy5sTda-RoCGJAQAvD_BwE) points to savings “up to 90%.”[GCP](https://cloud.google.com/solutions/spot-vms) claims up to 91%.[Rackspace](https://spot.rackspace.com/) goes even further, advertising discounts as high as 92%.


*But why are they so cheap?*


Spot instances represent unused compute capacity that cloud providers make available at steep discounts. You benefit from cheaper compute costs, while providers monetize capacity that would otherwise sit idle generating no revenue.


However, this capacity is transient. When higher-priority demand arises, providers can reclaim the resources with as little as 30 seconds to two minutes of notice. This interruption usually results in the instance being terminated. This makes Spot Instances a good fit for fault-tolerant workloads such as batch processing, AI model training, and CI/CD pipelines. To compensate for interruption risk, spot capacity is priced at extreme discounts, typically 50–90% lower than on-demand rates.


While the concept of discounted, interruptible compute is consistent across cloud providers, the pricing mechanisms used to determine those discounts are not.


‍


## How spot pricing mechanisms differ across cloud providers


‍


To understand these differences, we need to first examine the foundational concepts that shaped spot pricing when it was first introduced.


As cloud platforms scaled, cloud economics research identified two distinct approaches to managing compute. The first is ***utility computing:*** the notion that compute should be allocated on demand, the same way we think about electricity or water, at a predictable price. On-demand instances solve this with fixed, pay-as-you-go pricing.


The second is ***market-based allocation*** : the idea that competitive markets can set compute prices that fluctuate in real-time to match supply with demand.


AWS implemented this model in 2009, selling surplus capacity through market-driven auctions where user bids moved prices up or down.


However, in 2017, AWS abandoned its auction model for a different approach. Today, AWS Spot pricing responds to supply and demand through price smoothing based on longer-term trends, rather than through user bids.


Azure and GCP Spot VMs use similar provider-managed variable pricing models, where prices adjust based on supply and demand.


Rackspace Spot returns to market-based auction pricing, where prices emerge directly from user bids and market conditions are fully visible.


These differences determine how predictable spot behavior is, how efficiently capacity is allocated, and ultimately how much you pay.


Provider Pricing model How prices are determined Price behavior


**AWS Spot (2009–2017)** **Market-based auction** User bids compete for capacity; market-clearing price emerges Variable; driven by user bid competition and capacity availability


**AWS Spot (2017–Present)** Provider-managed variable pricing Internal capacity signals Variable; adjusts daily based on supply/demand


**Azure Spot VMs** Provider-managed variable pricing Internal capacity signals Variable; adjusts daily based on supply/demand


**GCP Spot VMs** Provider-managed variable pricing Internal capacity signals Variable; adjusts daily based on supply/demand


**Rackspace Spot** **Market-based auction** User bids compete for capacity; market-clearing price emerges Variable; driven by user bid competition and capacity availability


We've seen how cloud providers implemented different pricing mechanisms, with AWS originally pioneering market-based allocation through auctions and Rackspace maintaining that approach today.


This tension between provider-managed and market-based pricing raises a deeper question: Why did researchers believe markets were the right solution for cloud pricing in the first place?


The answer lies in a fundamental challenge that emerged as distributed computing systems began to scale.


‍


## Why resource allocation in distributed systems needed markets


‍


As distributed systems scaled through the 1960s and 70s, a persistent problem emerged: demand for compute fluctuated sharply.


Building enough infrastructure to handle peak demand meant massive idle capacity during normal periods. Provisioning for average demand meant shortages during peaks. The machines were there and operational, but no mechanism existed to efficiently match fluctuating demand with available supply.


Traditional solutions like first-come-first-served or centralized schedulers worked when systems were small and owned by a single organization. But as computing became shared across many independent users, often across different organizations, centralized control couldn't scale. There was no fair way to decide whose workloads should run when demand exceeded supply, and no mechanism to connect idle capacity with users who needed it.


Researchers began proposing market-based allocation as a solution.


Sutherland's 1968 paper["A Futures Market in Computer Time"](https://dl.acm.org/doi/epdf/10.1145/363347.363396) explored treating computer time as a tradeable commodity where users bid in a continuous auction to reserve future capacity.


Waldspurger's 1992 work on["Spawn"](https://www.waldspurger.org/carl/papers/spawn-ieee-tse-feb92.pdf) proposed a distributed computational economy where users would bid in auctions for CPU, storage, and memory.


The core insight was to let users express how much they value resources through what they're willing to pay and let auctions determine who gets access when capacity is scarce.


Cloud providers later adapted this research to address idle capacity.


When AWS launched EC2 in 2006, on-demand instances provided guaranteed capacity at fixed prices, but there was still idle capacity to sell.


Providers could not lower on-demand prices to monetize this idle capacity without cannibalizing their primary revenue stream.


Instead, in 2009 AWS launched Spot Instances, applying auction-based allocation specifically to surplus capacity. Users would bid for idle resources, and instances could be interrupted either when available capacity decreased or when higher bids displaced lower ones.


The auction mechanism from research provided the coordination layer where users willing to pay more got priority, and prices reflected real-time supply and demand for surplus capacity.


But how did this auction mechanism actually work in practice, and why did AWS eventually abandon it?


‍


## How AWS's original auction implementation (2009–2017) shaped spot's evolution


‍


AWS Spot launched with three core features:


1. **Auction-based allocation:** Users submitted bids indicating the maximum price they'd pay per hour. Instances ran as long as the market-clearing price remained below their bid.
2. **Interruptibility:** Instances could be reclaimed with short notice. Revocations could occur for two reasons: when the underlying supply of spot capacity decreased, or when higher competing bids displaced lower ones.
3. **Variable pricing:** Prices fluctuated based on real-time supply and demand, enabling the market to clear efficiently.


These characteristics defined spot's value proposition.


The auction mechanism solved a fundamental problem: cloud providers didn’t know how much users valued interruptible capacity, and users didn’t know how much spare capacity existed at any given moment. Rather than the provider setting prices arbitrarily, auctions allowed the market to discover an equilibrium price, where supply and demand balanced.


However, auctions are not neutral by default. The specific way AWS implemented the auction mechanism mattered, because it shaped not just pricing, but predictability, fairness, and user behavior. The rules of the market and the transparency of pricing would ultimately determine whether Spot delivered the benefits that prior research promised.


Now let’s look more closely at how this auction market actually worked.


‍


### The auction mechanics of spot pricing


‍


Here's how it worked:


1. **Users submitted bids** - Each bid indicated the maximum price they were willing to pay per hour for a specific instance type in a specific availability zone. These weren't one-time bids, they were standing offers that remained active as long as the user wanted capacity.


*Auction-era Spot Console UI: users expressed demand (capacity + instance requirements) and set a bid directly. Source:*[Amazon Web Services Spot Fleet Update – Console Support, Fleet Scaling, CloudFor…](https://aws.amazon.com/blogs/aws/spot-fleet-update-console-support-fleet-scaling-cloudformation/) *(2015)*


1. **AWS determined the spot price** - AWS looked at all active bids and available capacity. The price was set at the level needed to allocate all available instances.
2. **Winners paid a uniform price** - Everyone who bid at or above the spot price received instances. They all paid the same uniform spot price, not their individual bid amount, but the market-clearing price.


Here's a concrete example:


Suppose AWS has 5 spare instances available and 7 users bidding for them:


- AWS ranks the 7 bids from highest to lowest.
- AWS allocates instances to the top 5 bidders.
- The spot price is set at the 5th highest bid.
- If the 5th highest bid is $0.30/hour, this becomes the market-clearing price.
- All 5 winning users pay $0.30/hour (regardless of whether they bid $0.50 or $0.31).
- The 2 users who bid below $0.30 receive no instances.


This price adjusted continuously as conditions changed. When new users submitted higher bids or spare capacity decreased, prices rose. When demand fell or capacity increased, prices dropped.


‍


**Scenario 1: Demand Exceeds Supply**


Suppose AWS has 5 available instances and receives these bids:


User Bid Result Price Paid


A $0.50 ✓ Wins $0.30/hr


B $0.45 ✓ Wins $0.30/hr


C $0.40 ✓ Wins $0.30/hr


D $0.35 ✓ Wins $0.30/hr


E $0.30 ✓ Wins $0.30/hr


F $0.25 ✗ Outbid —


G $0.20 ✗ Outbid —


‍


Since only 5 instances exist, AWS allocates them to the top 5 bidders (A through E). The lowest accepted bid is $0.30, so that becomes the clearing price. All five winning users pay $0.30/hour, regardless of their individual bids. Users F and G, who bid below the clearing price, receive nothing.


‍


**Scenario 2: Supply Exceeds Demand**


Now suppose AWS has 10 available instances but receives only 5 bids:


User Bid Result Price Paid


A $0.50 ✓ Wins $0.05/hr


B $0.30 ✓ Wins $0.05/hr


C $0.20 ✓ Wins $0.05/hr


D $0.10 ✓ Wins $0.05/hr


E $0.05 ✓ Wins $0.05/hr


‍


Supply exceeds demand, so there's no competition. Everyone gets an instance. The auction clears at the lowest accepted bid, in this case $0.05/hour. All five users pay approximately $0.05/hour.


This second scenario reveals something crucial about how markets should work. When capacity is abundant, low bids should pull prices down.


Users bidding strategically low during periods of excess supply would naturally reduce the clearing price. This is exactly what markets are supposed to do. Prices rise to ration scarce resources when supply is tight. Prices fall to attract demand when supply is abundant.


‍


### Hourly spot billing based on changes in the market-clearing price


‍


During AWS's auction-based pricing era, Spot instances were charged by the hour. Once an instance was running, the user was billed at the Spot price in effect at the beginning of each hour, and that price applied for the entire hour of execution. Simply put, the price could change hour to hour based on the auction. Your hourly price did not stay at the Spot price you started with.


For example, a user might launch a Spot instance when the price is $0.10 per hour. For the first hour, they are charged $0.10. If, at the start of the next hour, the Spot price rises to $0.45 per hour and remains below the user’s maximum bid, the instance continues running and the user is charged $0.45 for that hour. The price paid changes hour to hour based on the market.


‍


From the user’s perspective, this played out like this:


- You might start at a low spot price
- The price may increase over time
- As long as it stays below your bid, your instance continues running
- In the worst case, you could end up paying nearly your bid price per hour


‍


### Understanding Spot Instance interruptions in AWS’s auction model


‍


The auction mechanism was tightly coupled to spot instances' revocation policy. Instances would run as long as their bid exceeded the current spot market clearing price. This created a direct relationship between price movements and interruptions.


Returning to our example from Scenario 1, let’s say:


- AWS has 5 spare instances with the clearing price at $0.30/hour.
- Now suppose spare capacity suddenly drops from 5 instances to 3 instances because on-demand customers need resources.
- AWS must revoke 2 spot instances.
- The spot price would rise to match the 3rd highest bid, jumping from $0.30 to $0.40/hour.
- The 2 users with the lowest bids (Users D at $0.35 and E at $0.30) would see the price rise above their bid and have their instances terminated with two minutes' notice.


Users could see why their instances were terminated because the spot price had risen above their bid. This transparency made the system's behavior predictable and verifiable.


‍


### Market visibility and information


‍


AWS operated this system at massive scale, running separate spot markets for each instance type in each availability zone of each geographic region. Thousands of distinct markets, each with its own price dynamics. Spot prices were published in real-time, and AWS made the previous three months of historical price data available for download.


‍


### AWS Spot Market Data Visible to Users


‍


During the auction era, AWS made the following information directly visible:


- **Current Spot prices** for each instance type, published per Availability Zone
- **Real-time price updates** , reflecting changes as market conditions shifted
- **Historical Spot price data** , with up to three months of price history available for download
- **Per-AZ price differentiation** , allowing users to see that the same instance type could have different prices in different zones
- **Time-series pricing behavior** , enabling users to observe price changes over minutes, hours, and days


### ‍


***Auction-era Spot pricing history chart:*** *spot price over time for a single instance type, shown separately for each Availability Zone. Source:*[https://www.databricks.com/blog/2016/10/25/running-apache-spark-clusters-with-spot-instances-in-databricks.html](https://www.databricks.com/blog/2016/10/25/running-apache-spark-clusters-with-spot-instances-in-databricks.html) *(2016)*


‍


The real-time and historical spot price data revealed patterns that users could analyze such as:


- **Revocation frequency** : By tracking how often prices spiked above different bid levels, users could estimate how many interruptions to expect
- **Availability** : Users could calculate what percentage of time the spot price stayed below their bid
- **Instance type characteristics** : Some instance types had stable, predictable prices while others were highly volatile
- **Demand patterns** : Historical data showed when demand typically peaked or fell in different regions


This information became the foundation for a decade of research into optimizing applications for spot instances. Sophisticated users could estimate costs accounting for interruption overhead, choose instance types that matched their fault-tolerance capabilities, and time their workloads to coincide with low-price periods.


The auction mechanism gave users both control through their bid prices and visibility into the market dynamics that determined when their instances would run and when they'd be interrupted.


On paper, AWS Spot looked like the ideal auction market, transparent, real time, and driven purely by supply, demand, and user bids. But… researchers who studied AWS Spot closely discovered that AWS wasn't operating a truly open auction market and they were using undisclosed mechanisms to constrain prices and control market behavior.


‍


## The hidden constraints behind AWS's original auction-era spot pricing


‍


According to researchers, even during the auction era, AWS Spot was never a fully open or transparent market.


‍


### The undisclosed algorithm and price manipulation


‍


While EC2's documentation stated that spot prices["fluctuated periodically depending on the supply and demand of spot instance capacity,"](https://aws.amazon.com/about-aws/whats-new/2009/12/14/announcing-amazon-ec2-spot-instances/) and to some implied a uniform price auction, AWS never explicitly disclosed the actual pricing algorithm. Users could see historical spot prices and observe their instances being revoked when prices rose above their bids, but they could not see:


- How bids were actually aggregated and processed
- Whether AWS applied hidden price floors or ceilings
- Whether bids below certain thresholds were simply ignored
- How capacity management decisions interacted with the bidding system


AWS had no obligation to disclose how its “market-driven” auction worked, but the lack of transparency created practical problems. Without understanding how prices were actually set, users couldn't build ***reliable*** bidding strategies or predict when instances would be terminated.


This opacity forced researchers into reverse engineering mode. Rather than working from documented rules, they had to infer mechanisms from observed price patterns, building increasingly complex models to explain behavior they couldn't directly observe.


‍


### How reverse engineering uncovered hidden reserve prices


‍


A 2013 study,["Deconstructing Amazon EC2 Spot Instance Pricing,"](https://dants.github.io/papers/Spotprice11CloudCom.pdf) reverse-engineered how spot prices were actually generated. By analyzing price histories across multiple regions and instance types, the researchers found that prices were "usually not market-driven as sometimes previously assumed.” Rather than reflecting real user bids, they found that prices were typically "generated at random from within a tight price interval via a dynamic hidden reserve price,” designed to create an "impression of false activity" regardless of actual demand.


The researchers identified the mechanism as an autoregressive AR(1) process that generated prices algorithmically. If true, this wasn't a market clearing through competitive bidding. It was an algorithm creating the appearance of market activity.


The research revealed specific constraints AWS imposed on pricing:


- Spot prices operated within a defined band with both floor and ceiling prices.
- The floor price prevented prices from dropping too low, even during periods of low demand.
- The ceiling price, often set absurdly high, prevented instances from running when AWS wanted to restrict capacity.


Critically, AWS appeared to ignore bids below the floor price. When demand was low, instead of prices dropping toward zero as a pure auction would suggest, they stopped at this hidden minimum. The floor wasn't fixed but moved gradually over time, tracking patterns determined by the same autoregressive model. This meant that even if users bid very low and spare capacity was abundant, prices wouldn't fall below AWS's predetermined threshold.


Further analysis revealed that this algorithmic control hadn't always existed. The research revealed that spot pricing had evolved through two distinct periods:


1. **A Genuine market phase** : December 2009, when spot first launched with what appeared to be true auction-based pricing
2. **Algorithmic control** : January 2010 onward, when the AR(1) model took over and prices became artificially generated


This timeline suggested AWS had experimented with a real auction mechanism briefly, then replaced it with algorithmic price generation within weeks of launch.


Another study,[" Analyzing AWS Spot Instance Pricing, "](https://www.cs.ucsb.edu/sites/default/files/documents/master_1.pdf) found similar evidence while analyzing the auction-era pricing data from 2016. Researchers documented a concrete example where a c4.8xlarge instance in us-west-1b where pricing fluctuated under $1.00 suddenly spiked to $22.08 and remained fixed at that price for six hours. The on-demand price for the same instance was $1.19. No rational user would bid $22.08 for an instance they could get on-demand for $1.19. The researchers concluded, like the 2013 study, that this was a hidden reserve price AWS used to prevent instances from running.


These were inferences drawn from observable price patterns, not confirmed disclosures from AWS. The actual algorithms remained opaque. Yet the evidence was consistent across multiple independent studies analyzing different time periods: AWS was constraining spot prices through hidden floor prices, ceiling prices, and algorithmic bands that ignored real user bids.


*The question remained: why? Was it to maximize profit? To ensure capacity utilization? To obfuscate supply and demand signals? AWS never disclosed its reasoning, leaving researchers and users to speculate.*


‍


### AWS as a provider, not neutral auctioneer


‍


There are compelling economic reasons why AWS would impose such constraints, even in an ostensibly market-driven system. They never promised to be a neutral auctioneer simply matching buyers to supply. It was a cloud provider balancing multiple objectives:


- Maximizing revenue from spare capacity
- Ensuring high capacity utilization
- Protecting the pricing integrity of on-demand instances
- Managing capacity for future demand
- Maintaining predictable operations


But this approach created a fundamental mismatch. AWS marketed spot instances as auction-based pricing where user bids determined outcomes. Users developed bidding strategies assuming their bids mattered. In reality, the research suggested they were competing against an opaque algorithm that appeared to ignore their bids. Spot pricing during this era created a gap between what users expected and what actually determined their costs.


‍


### How this opaque auction model shaped user behavior


‍


The auction mechanism introduced direct competition for capacity. AWS’s opaque implementation shaped how users responded to that competition. Without clear signals about how prices were set, users competed with limited information and often:


- Bidding defensively high to avoid revocations, often near the on-demand price, but sometimes absurdly higher. In extreme cases, users bid over $1,000 per hour for instances with on-demand prices of $0.10, assuming prices would never actually reach those levels. When multiple users employed this strategy simultaneously, spot prices occasionally spiked to 10,000× the on-demand rate.
- Building complex price prediction models to anticipate movements
- Treating spot instances as fundamentally unpredictable and risky


‍


### The volatility and complexity of AWS's auction-based spot market


‍


For most users, complexity deterred adoption. The spot market was highly complex, with thousands of server types each having their own dynamic price.


Most users lacked the sophistication to navigate this complexity and effectively use the information to optimize their applications.


While the auction model enabled powerful optimization techniques for sophisticated users, most of the advantages were primarily documented in research papers rather than deployed in production systems.


The volatility and complexity of the auction model stemmed from users' reactions to the competitive bidding mechanism. Users, lacking effective bidding strategies and unable to understand AWS's supposed algorithms, responded with defensive approaches that amplified price swings. This combination produced excessive revocations that made spot instances difficult to rely on.


AWS's hidden constraints may have served legitimate business purposes: protecting revenue and managing capacity. The auction mechanism, however, created market dynamics AWS ultimately decided were unsustainable.


‍


## November 2017: AWS abandons auction-based pricing


‍


Due to undesirable spot price volatility, in November 2017, Amazon announced in a[blog post](https://aws.amazon.com/blogs/compute/new-amazon-ec2-spot-pricing/) that it was ending the auction-based spot market that had existed since 2009. The changes were presented as improvements to user experience, with spot prices no longer set by real-time bidding but instead "determined based on multiple factors such as long-term trends in demand and supply.


Under the new model, several things changed:


‍


‍ **1. From real-time auctions to trend-based pricing:** **Replacing market signals with price smoothing**


The most fundamental change was how prices were determined. Prior to November 2017, Spot prices closely matched instantaneous supply and demand. They rose and fell as bids arrived and capacity fluctuated, sometimes multiple times per hour.


Under the new model, Amazon announced that Spot prices would “adjust more gradually, based on longer-term trends in supply and demand.” Instead of reflecting real-time market conditions, prices began to move slowly, tracking patterns over days or weeks rather than minutes or hours.


The effect was dramatic. An analysis from the research paper[The Price is (Not) Right](https://homes.luddy.indiana.edu/prateeks/papers/icccn19-price.pdf) examined Spot pricing behavior before and after the change and found a clear shift. Prior to November 2017, prices spiked frequently and at times exceeded the on-demand rate. After the transition, prices became largely flat, remaining stable for weeks at a time with minimal variation.


**2. The end of competitive bidding**


AWS eliminated the requirement to place bids. The "bid price" was replaced with an optional "maximum price" that users could set if they wanted to cap their costs. If users didn't specify a maximum price, it defaulted to the on-demand price.


AWS may then change the market price over time, and users pay the current market price. If the market price exceeds a user’s maximum price, the instance will be terminated. AWS may also terminate instances at any time, irrespective of the market price and user bids.


This was framed as simplification because users no longer needed to navigate complex, fluctuating price streams or develop bidding strategies. They could simply request spot capacity and trust it would be cheaper than on-demand, without needing to actively manage bids.


**3. Decoupling price from revocations**


Perhaps the most technically significant change was breaking the link between spot prices and instance interruptions.


Under the original auction model, instances were revoked only when the spot price exceeded the user's bid. Under the new model, AWS decoupled revocations from pricing. Instances can now be terminated when capacity is needed, even if the spot price remains below the user's maximum.


[Users reported exactly this behavior](https://repost.aws/knowledge-center/ec2-spot-instance-termination-reasons) . Instances were terminated during periods when the spot price was stable and significantly below their maximum price. The spot price no longer explained when or why revocations occurred. It became a largely independent signal that moved on its own schedule.


‍


### What users lost when AWS abandoned the auction model


‍


The AWS auction model, for all its flaws, had one powerful characteristic: competition still drove prices down during periods of low demand.


When spot capacity was abundant, users could submit low bids and often win capacity at prices far below on-demand rates, sometimes achieving 90-95% discounts. The auction mechanism, even if not purely market-driven, still responded to actual supply conditions. Excess capacity meant cheap compute.


‍


## Why did EC2 spot instances become more expensive after 2017?


‍


Researchers who analyzed spot pricing before and after 2017 found out that users generally saved more money using the older auction-based model due to lower average prices. This means that despite its volatility and complexity, the auction era delivered cheaper compute.


The new simplified, stable pricing that replaced it came at a cost.


On the surface, spot prices should have decreased after 2017. According to the research paper["Analyzing AWS Spot Instance Pricing,"](https://www.cs.ucsb.edu/sites/default/files/documents/master_1.pdf) spot instance execution duration was "no longer partially determined by the client's 'bid price.'" This represented "a drop in reliability," and the researchers expected prices to fall accordingly.


Under the auction model, bidding higher reduced your interruption risk. You could essentially pay for greater reliability through your bid. Once AWS removed this mechanism, users lost control over their revocation probability. Since unreliability is the primary disadvantage of spot instances compared to on-demand instances, basic economics suggests prices should have fallen to compensate for this lost control.


But prices didn't fall. In many cases, they rose.


Researchers identified that "the introduction of new features such as price smoothing and termination notices could explain increased prices to 'cover' sharp changes in demand or supply." Essentially, AWS was absorbing market volatility internally rather than exposing users to rapid price swings. This acted as a form of insurance where users got price stability but paid for it through higher average costs.


‍


## What does AWS spot pricing look like today?


‍


AWS spot prices have risen significantly since 2017 and many users now question whether spot instances still deliver meaningful cost savings.


In May 2023, researcher Eric Pauley published an analysis titled["Farewell to the Era of Cheap EC2 Spot Instances"](https://pauley.me/post/2023/spot-price-trends/) that documented a troubling trend. Spot price ratios, which measure spot price relative to on-demand price, had spiked as much as 55% in us-east-1, AWS's largest region, since the start of 2023. Four of AWS's biggest regions saw prices "skyrocket."


A[response from Cristian Măgherușan-Stanciu](https://leanercloud.beehiiv.com/p/thoughts-current-state-ec2-spot-pricing) , a former AWS employee who worked on Spot, confirmed the trend and revealed the strategy behind it. The price increases weren't accidental or purely market-driven, but deliberate.


AWS, he explained, is incentivized to maximize Spot utilization because that's what generates revenue from otherwise idle capacity. As economic pressures drove more companies to adopt Spot instances for cost optimization, aggregate utilization increased.


AWS's response? Raise prices on heavily-utilized instance types to push users toward underutilized ones. As Măgherușan-Stanciu put it: "Their way to spread out the load across their many instance types is by increasing the hourly price, in addition to the inherent increase in interruptions."


The goal is to encourage diversification. Instead of everyone competing for popular instance types, AWS wants users spread across 600+ available instance types, including older, less desirable types that still has spare capacity.


To get decent Spot savings now, you must:


- Diversify across dozens or hundreds of instance types
- Use allocation strategies like "price-capacity-optimized"
- Constantly monitor which obscure instance types still offer discounts
- Accept older hardware or unusual configurations
- Set hard limits on acceptable savings percentages
- Layer Reserved Instances and Savings Plans on top for types that became too expensive


‍


This raises a fundamental question: **could an auction-based spot market work if it were truly transparent and open?** AWS's implementation was opaque and algorithmically controlled. But what if a cloud provider built a genuinely competitive auction system where prices reflected real supply and demand and disclosed how the mechanism worked?


‍


## Rackspace's approach: What true open-market auctions actually look like


‍


After AWS moved on from its auction-based spot pricing in 2017, it left people thinking that real-time computational markets were too complex, too volatile, and ultimately unsustainable.


But[Rackspace drew a different conclusion](https://spot.rackspace.com/blogs/reclaiming-the-cloud-with-rackspace-spot) and pushed forward with its aim to democratize infrastructure again. As Rackspace stated, "We're trying to offer something those providers can't or won't: an honest cloud, built by engineers who understand what it means to run real systems, at real scale, with real constraints.”


The problem wasn't auctions themselves, but the dynamic AWS's implementation created. As we've seen, even during AWS's "auction era," prices were generated artificially. Hidden reserve prices, opaque algorithms, and undisclosed constraints made the auction theatrical, not genuine.


[Rackspace Spot](https://spot.rackspace.com/?utm_source=chatgpt.com) , launched in 2024, is built on a simple hypothesis: **what if you actually ran an open auction?**


‍


### What makes Rackspace's auction different from AWS's ?


‍


*The obvious question: if AWS's auction was too complex for users, why would Rackspace's work?*


While AWS’s auction mechanism introduced complexity and opacity through hidden reserve prices and algorithmic price controls, Rackspace takes a much simpler and more[transparent](https://spot.rackspace.com/docs/en/open-market-auction) approach. With[Rackspace Spot](https://spot.rackspace.com/?utm_source=chatgpt.com) , the rule is clear, which is that[your instance is only pre-empted if the market price exceeds your bid](https://spot.rackspace.com/docs/en/pre-emption) . There are no hidden algorithms, no artificial price bands, and no opaque capacity manipulation.


‍


### Launching spot instances on Rackspace


‍


Here’s what you’ll see in the Rackspace Spot UI when you launch instances. **‍**


1. **Select bid strategy**


Rackspace simplifies bidding by helping you decide if you want lower cost by bidding just above the market price, or lower risk by bidding a bit higher. You can also set a custom bid, and the slider makes it easy to see your options starting from the current market price.


For more guidance on how to think about bidding, see our Spot bidding best practices:[https://spot.rackspace.com/docs/en/bidding-best-practices.](https://spot.rackspace.com/docs/en/bidding-best-practices)


2. **Current market price**


The current market price is shown upfront, so you immediately know the going rate and what you’re bidding against.


‍


These two features remove much of the guesswork that historically pushed users to overbid in opaque auction Spot markets.


By showing the current market price upfront, you immediately know the going rate. The guided bid strategy then helps you choose sensible bids relative to that price, instead of bidding defensively.


The result is more predictable bidding, less unnecessary overbidding, and a spot market that’s easier to participate in and less volatile overall.


‍


### Cheaper instances at true market rates


‍


Because we drive pricing through an open and transparent auction, bids can start as low as **$0.001 per hour** . From there, prices only increase when real demand rises, with no fixed discounts or hidden controls pushing them up.


Try it yourself:[Get started with Spot](https://spot.rackspace.com/) and place your first bid in minutes.


‍


## Conclusion


‍


Throughout this article, you’ve explored how markets were first proposed as a way to handle idle capacity in large data centers, how AWS implemented this idea through auction-based Spot markets, and how researchers responded by developing increasingly complex bidding strategies to extract value. Over time, it became clear they weren’t interacting with a truly open market, but with a system constrained by hidden reserve prices and algorithmic controls. We then saw how AWS moved away from auctions entirely, transitioning to a provider-managed variable pricing model similar to what providers like GCP and Azure had always used. Since then these provider spot prices have steadily increased, raising questions about how much of the original value proposition remains.


Rackspace gives you a different approach, reviving a true market-based auction where prices are set by actual clearing prices and capacity is allocated based on the value users place on it, delivering the original promise of the computational market: cheaper compute driven by transparent signals, not opaque algorithms.


‍


## Summary: How spot compute differs across providers


‍


The table below provides a side-by-side comparison of Spot offerings across major cloud providers, highlighting differences in pricing, interruptions, and operational support.


‍


Feature AWS Spot Instances GCP Spot VMs Azure Spot VMs Rackspace Spot


**Pricing mechanism** Provider-managed variable pricing based on supply and demand Provider-managed variable pricing; adjusts daily based on supply and demand (up to 91% discount) Provider-managed variable pricing based on supply and demand Market-based auction; user bids compete for capacity with transparent market prices


**Maximum runtime** No hard limit No hard limit No hard limit No hard limit


**Interruption warning** 2-minute notification 30-second notification 30-second notification 2-minute notification


**Orchestration support** Auto Scaling Groups, EC2 Fleet, ECS, EKS Managed Instance Groups, Google Kubernetes Engine Virtual Machine Scale Sets, Azure Kubernetes Service Fully managed Kubernetes clusters


**Service guarantees** No SLA provided No SLA provided No SLA provided No SLA provided


**Billing increment** Per-second (60-second minimum) Per-second Per-second Per-second (60-second minimum)


**What triggers revocation** AWS needs the capacity GCP needs the capacity Azure needs the capacity The market price goes above the user's bid


‍


## Glossary


Term Definition


**Spot instance** A virtual server using surplus cloud capacity at 50-90% discounts. Can be interrupted with short notice when the provider needs capacity for higher-priority workloads.


**Spot market** The marketplace where providers sell surplus capacity at discounted prices. Each instance type per availability zone typically has its own spot market.


**Spot pricing** The mechanism determining hourly rates for Spot instances. Prices may be set through auctions (user bids), provider-managed dynamic pricing (prices adjusted internally based on capacity and demand), or static discounts (fixed percentage reductions).


**Market-based allocation** A resource allocation approach where prices are set by supply and demand rather than fixed rates. As demand for capacity increases, prices rise; as demand falls, prices drop. This model is used to efficiently distribute scarce compute resources and incentivize flexible usage.


**Auction-based pricing** A pricing mechanism where users bid for capacity. When the auction runs, bids are ordered from lowest to highest, and capacity is allocated to the highest bidders until the available supply is filled. All winning bidders pay the same market-clearing price. AWS used this model from 2009–2017; Rackspace uses it today.


**Bid** (Auction-based Spot pricing)


The highest price a user is willing to offer for capacity in an auction-based Spot system. A bid participates directly in the market: it is compared against other users' bids to determine who receives capacity and what the market-clearing price will be.


**Maximum price** The highest price you are willing to pay per hour for a Spot instance. This is not a bid. The cloud provider sets Spot prices independently, and your maximum price does not influence the market price. It acts as a threshold: if the current Spot price exceeds your maximum, your instance will not start or will be stopped/evicted. AWS, Azure, and GCP all support maximum price settings.


**Price smoothing** Gradual price changes based on long-term trends rather than real-time supply/demand fluctuations.


**Interruption** Termination of a spot instance by the provider with 30 seconds to 2 minutes warning. Occurs when capacity is needed or (in auctions) when price exceeds user's bid.


‍
