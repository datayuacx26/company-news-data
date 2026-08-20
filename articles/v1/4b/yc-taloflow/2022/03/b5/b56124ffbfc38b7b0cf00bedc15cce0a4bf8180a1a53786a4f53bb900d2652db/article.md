---
schema_version: "1.0.0"
document_id: "b56124ffbfc38b7b0cf00bedc15cce0a4bf8180a1a53786a4f53bb900d2652db"
company_key: "yc-taloflow"
company: "Taloflow"
source_id: "yc-taloflow-rss-cfae8c512c9e"
canonical_url: "https://www.taloflow.ai/blog/buying-things-that-work"
published_at: "2022-03-17T22:02:00+00:00"
first_seen_at: "2026-07-26T01:23:39.915767+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:3e4dca72401e0a8eb6c6f62ade4006fa2830c496a217b6b31fa22cd6a8a8186c"
---

# Why just one person can't buy things that work well

It's too difficult for product teams to find the right vendors. Vendors obscure details, promise everything or downright lie, have special pricing for those who know how to ask, and there are just too many of them!


This problem is getting worse because of the "Cambrian explosion" in cloud tooling, a blossoming in the number of solutions and niche specializations.


It's a problem that resonates in the ranks of virtually every product-first company I meet with ([this HN thread](https://news.ycombinator.com/item?id=30679935) also illustrates how common it is). How many more months of precious engineering time will we waste building massive spreadsheet matrices, debating use case fit internally to figure out what matters, and pouring through developer documentation?


**This post:**


1. Explores our thesis on how the vendor selection process should change
2. How companies like ours can implement these changes
3. Known problems with the model


## The “Boss” Spreadsheet


I wanted to find a suitable email marketing API for our company's use case a while ago. A fellow entrepreneur sent me a matrix of vendor functionality he found through a startup studio (Venture Harbour).


1. It was already big. It had 99 vendors evaluated across 45 dimensions or roughly 4,455 data points to consider.
2. A few of the data points had gone stale.
3. There were several things about our use case it didn't cover.


I had to spend roughly 30 more hours before I was ready to make a decision:


- in demo calls;
- adding and prioritizing dimensions;
- pouring through documentation; and,
- actually testing a half dozen products.


Low and behold, I still made the wrong decision. Two months later, our team had to switch to another vendor despite the pain of integrating everything all over again.


Several senior architects have shown me the pain-staking exercise they go through when compiling complex spreadsheets (like the one above) for their next buying decision. Sometimes they get it right, but as Dan Luu notes,[this often goes wrong](https://danluu.com/nothing-works/) .


## No more conjecture, please.


What I needed was an expert analysis tailored to my use case. But unfortunately, I didn't have the time to run the analysis, nor am I an expert in email marketing APIs. Besides, why should I be forced to invest the time into becoming an expert for what should be a one-time decision?


What if what we learned from the failed spreadsheet experiment was fixed? Can we find a way to:


- Keep information accurate and updated
- Prioritize (i.e.: weight) the dimensions
- Make a clear recommendation for a user based on their use case
- Keep the process from start to report/insights to under 10 minutes


Surely, there's a market for this, right?


Below, we cover the four main steps we take to do it and the potential problems with our model.


### Step 1: Commoditize the expert


In the abstract, the expert's role is to build the matrix of vendor functionality and possible points of differentiation, group features into dimensions, and weight these dimensions based on their understanding of the use case. It's what industry analysts and consultants at companies like Gartner, Deloitte, or specialty firms, do all day. There is a risk to engaging them: consultants and analysts possibly are expensive, unskilled (and you might not be able to tell), have secretive methodologies, and even conflicts of interest.


However, providing an **expert system** that encodes a matrix of vendor functionality, asks relevant questions, and provides recommendations based on unique needs, can help most people make better decisions. That is, as long as we execute the following:


1. Build a diverse group of experts that devise the matrices and maintain these systems as the market changes.
2. Actively mitigate expert bias, or make the components that go into the analysis transparent and understandable for the user. We have to show that use-case fit is quantifiable and not a gut feeling.
3. Understand the use case (not just the requirements). For example, if I say that Email Deliverability is more critical than Pricing, it's important to quantify, "by how much?".


#### There is always some nuance to capture


Some things are an explicit "Yes" or "No" when building matrices. For example, our Observability experts can rate Instana as best-in-class for "Visible Metrics in <1 Second" (it's the only one that has it) and New Relic as unavailable for "Language Independent Agent with Dynamic Instrumentation" without giving it too much thought. However, vendor support for "Continuous Profiling (Capture Code Profile for Every Trace/Transaction)" is more nuanced, and that's where the expert and a gradient for rating vendors against any given feature is vital. We can take multiple expert opinions and normalize the ratings in such cases.


### Step 2: Keep up with changes


Features are constantly changing, and service levels improve and worsen too. Without a robust system to monitor these changes, any system will quickly go out of date. Given the feature velocity in IaaS and PaaS, I believe the cycle for refreshes should be within two weeks. Tools like[VisualPing](https://visualping.io/business/) can check pricing pages and documentation in short intervals for any changes. Category-focussed analysts can then triage and extract the relevant information based on their general knowledge of the category and update matrices accordingly.


### Step 3: Monetize without compromise


There are multiple options for monetization, and frankly, we're exploring several.


Here are some quick ones:


#### **Locked-up Referral Rewards**


**‍** What if the AWS affiliates that clog up Google's search results only got paid if you used that toaster oven for the next five years? That's the kind of model we think it takes to incentivize accurate recommendations. Specifically, an affiliate-style system that only pays out if a company sticks for a certain amount of time and is paid out over the course of a few years. However, you have to include a comprehensive group of vendors in the analysis, no matter the referral relationship. If AWS won't pay for a referral, what good would an object storage recommendation be if it didn't include Amazon S3 in the analysis?


#### Upsell Enterprise Collaboration Features


The idea is to upsell mid-market and enterprise customers on a collaborative decision-making workflow. The compendium of reports can become an enterprise record system for the "Why?" behind buying decisions. For example, have you ever wondered why the Director of Engineering who left your company several quarters ago made a particular vendor decision? Now, you can look it up and benefit from the same analysis or start anew.


### Step 4: Streamline prioritization and trade-offs


How do we avoid asking users about the importance of each feature? There are hundreds of table-stake features, and even after distilling to the actual points of differentiation, you're usually still left with 100-200 features to evaluate against a typical use case. That would make for one of the most tedious intake forms ever.


The goal is to get users the relevant information to build conviction in five minutes:


1. Questions during intake have to gather requirements implicitly at first because users usually don't know their requirements. For example, rather than asking the user whether they need unlimited egress for a storage service, we ask them about the expected ratio of uploads to downloads.
2. Branching questions come in handy, mainly if you focus on categorizing the user into a use case at the onset of the intake, either explicitly or in the background.
3. Use case categorization also helps set a series of default requirements. For example, knowing that a customer is all-in on Kubernetes helps prioritize a lot of features in the Cloud Cost Management category.
4. A single question can do a lot of "work". For example, asking a user about the range of file sizes for object storage can impact both pricing-related and performance-related feature requirements.
5. Have a point-based system to prioritize dimensions (i.e., categories of features) based on the answers given and allow the user to fine-tune the point allocation.


A similar thing can be done for other products, like DSLRs. For example, suppose you're a wildlife photographer. In that case, you're going to prioritize systems with high-quality long-distance zooms and are likely to prefer a system with some level of weather-sealing.


*This process is not specific to cloud tooling. We chose cloud services because we previously were a cloud cost optimization platform before our pivot.*


## Better insights make better decisions


Thanks to the model described in Steps 1 through 4, your use case-specific needs are weighed individually, and the final result is a ranked set of vendors and their properties (including gotchas) for your use case. You can then make an informed purchasing decision.


Dimension Old Way New Way


Cost of Expertise Expensive because of the time investment required for an expert to address each individual use case. Cheap because the expertise is commoditized through automation.


Time Investment 20+ hours of research spread across several weeks. 5-10 minutes to get to best fit vendor(s) and have the important trade-offs immediately identified.


Accuracy of Opinion More variance in the accuracy with single source of expertise. More normalized level of accuracy based on multiple sources of expertise.


Trust Secretive methodologies for ranking and rating vendors. Transparent analysis presented in easily-digestible format.


## Problems in implementing this model


Most similar-looking products are dramatically worse. It's a common marketing strategy to throw a fake quiz before a quote. Take car insurance, for example. It's pervasive to see lengthy quizzes that end with a form that asks for your name and number, which is then sold to vendors that will spam call you for weeks.


There's negative value created, and the public perception of "answer these questions for a recommendation" tends to be very poor.


It's challenging to communicate to potential users that you will:


1. Provide value for their time
2. Not sell their information
3. Provide unbiased results


## Are peer insights the alternative?


Can't you talk to peers in your industry and do precisely what the smartest ones do? That seems to be the approach of platforms like G2 and many Slack communities organized around specific functions, like marketing, data science, or DevOps. That sometimes works, mainly for SaaS products. You can possibly figure out whether to use Slack or Mattermost based on a handful of valued peer opinions. However, when it comes to software used to make software (i.e., developer and cloud tooling, or IaaS and PaaS), it's almost always cloudier (pun intended), and the new model we described is more necessary.


> I do this some now, but it’s hard because most vendors are terrible on at least one axis, and most people haven’t used multiple vendors for the same thing so can’t make a great comparison.
>
>
> — Ben (hiring 72/∞ teammates) (@benskuhn)[March 29, 2021](https://twitter.com/benskuhn/status/1376560191672492036?ref_src=twsrc%5Etfw)


Why is it cloudier than SaaS? Primarily, it's because the quality of the product experience for IaaS and PaaS varies much more between use cases. Underlying this are more complex offerings with hundreds of points of differentiation and more than a handful of dimensions that should be considered when making a decision. As an example, here's a shortlist of the dimensions we studied for Observability tooling:


- Application Architecture Supported
- Deployment Options
- Instrumentation (Data Collection)
- Data Types Collected
- Metrics Language Support
- Logging Language Support
- Tracing Language Support
- Code Profiling Language Support
- JVM Support
- Integrations
- Framework Support
- Kubernetes Support
- Alert and Notification Support
- Open Source Project Support
- Dependency and Relationship Analysis
- Change Analysis
- Root Cause Analysis
- Automated Operations
- Security and Compliance
- Pricing Suitability


G2 will not complete the last "selection" mile in the buyer's journey when choosing between Dynatrace, Splunk Observability, and the like. However, we think our new model can.
