---
schema_version: "1.0.0"
document_id: "bda87e6a914b3e5ef200f414be332a0f280b4154a9e4cb9800536f23e09ed328"
company_key: "braze-inc-class-a-common-stock"
company: "Braze Inc."
source_id: "braze-inc-class-a-common-stock-news-import-06f37ae7f1b6"
canonical_url: "https://www.braze.com/resources/articles/how-product-teams-build-braze"
published_at: "2026-08-04T19:01:00+00:00"
first_seen_at: "2026-08-05T21:21:37.297+00:00"
fetched_at: "2026-08-05T21:21:39.057372+00:00"
content_hash: "sha256:5f43a5834bded53ff5f102a4c241cb6925253a07abdf7c3ec86326798f811b72"
---

# How product teams at Braze build at scale

Many product roles ask you to execute someone else's vision. At Braze, we ask you to help build it — and leave a mark on the product and the culture in the process.


George Khachatryan leads AI Decisioning Product at Braze. His team builds BrazeAI Decisioning Studio™, which helps brands use data to make smarter, faster 1:1 decisions about how and when to reach customers, including channel, message, creative, offer, timing, and frequency, all optimized continuously and automatically. It's some of the most technically ambitious work happening at Braze.


He got here by building the thing himself: he co-founded and served as the CEO of OfferFit, an AI decisioning engine that Braze acquired in 2025. Before that, he was an Associate Partner at McKinsey, working on large-company turnarounds, and before that, he co-founded Reasoning Mind, an education software company that's reached over a million students. He has a Ph.D. in Mathematics from Cornell and a master's degree from the University of Cambridge.


We asked him what it's actually like to build AI products at Braze: how teams work together, how decisions get made, and what the day-to-day looks like for the people doing it. His answers below.


**How would you describe what your team does?**


We're the BrazeAI™ Decisioning Studio team, and we build Braze's AI Decisioning solutions. These are products that look at data about each individual user and then make personalized decisions about how to market to that person, learning and improving through autonomous experimentation. We use a type of AI called reinforcement learning (RL) to make it work.


Here's a way to picture it. Say a company is running a journey to cross-sell products to its existing customers. Before using our product, they'd typically run a bunch of A/B tests to figure out what emails, push notifications, or texts to send, which is slow and doesn't personalize well, since different people need different things. With our product, they set up an AI agent that makes an individual decision for each person based on that person's actual behavior, and it gets better and better over time at maximizing whatever the company cares about, whether that's conversions, revenue, or lifetime value.


**BrazeAI Decisioning Studio is built around the idea of making real-time, personalized decisions at scale for every customer. What does it actually take to build something like that, and what makes it a hard but compelling problem for someone to work on?**


Reinforcement learning, the type of machine learning we use, is incredibly powerful but also very technically complex. Rather than training one model and running inference from it for everyone, we train a separate model for each and every company. That matters because what works at one company won't necessarily work at another. Each company is in a different industry, with a different customer base and different products, so you have to learn on their actual data to get good results.


The real complexity lives in doing this robustly at scale. Model training for reinforcement learning is finicky, so the challenge we're solving is how to truly productize it, how to bring the full power of RL to companies in a way that performs reliably every single time. Each company has different KPIs and different data, so you need a lot of flexibility, and the hard part is providing that flexibility while still guaranteeing robust performance. It's a great problem because it's both intellectually really cool and practically relevant. Many of the brands we work with reach hundreds of millions or even billions of people, so the work is intellectually stimulating and it directly shapes how people interact with the products they use.


**How does your team stay aligned with other departments when you're working on AI features that are still evolving as you build them?**


AI Decisioning is a very specific kind of product. It's used by lifecycle marketers, but it works differently from the core components of a customer engagement platform, so it takes a lot of cross-functional coordination. On the Product team, we write up how the product works so the Sales team can be trained on it, and we partner with Marketing to run masterclasses on AI decisioning at Braze's global events to help customers understand the technology. We also work closely with our Delivery team, a group of forward-deployed data scientists who deliver the more advanced, flexible version of the product and customize it to each customer's needs.


That last collaboration is the tightest, and it's a good example of coordination paying off. The forward-deployed data scientists come up with ideas and initially custom-implement them for specific customers. Once we see a pattern getting built over and over, we productize it. Constrained optimization is a great case. A lot of customers were saying things like "I want to maximize revenue, but I don't want to send more than X messages a week or make more than Y worth of offers." Our data scientists were solving that by hand with custom code for each customer, and once it became a clear pattern, the product team worked with them to productize it. Now every customer can set it up easily without needing us to build something custom.


**How does your team maintain a high bar for quality on AI product when "good" is constantly evolving?**


Quality in AI Decisioning is unusually measurable, probably more so than in a lot of other products, because the whole reason to use it is to get performance uplift. Every marketing campaign or journey has a target it's trying to maximize, whether that's conversions, revenue, or margin. That means you can randomly hold out a control group that gets the business-as-usual treatment and directly measure how much impact the AI is driving.


Keeping that rigor is a lot of work, and plenty of companies probably wouldn't bother, but it's always been core to how we operate. We always keep a control group and always preserve that measurability, which gives customers real transparency into the value they're getting. It does put an onus on us to actually perform, and we like it that way. It's the most intellectually honest way to market the product, and in the long run it's what makes us successful.


**What's something the team built for BrazeAI Decisioning Studio that started as an internal idea and ended up mattering to customers and being a “win” in a way nobody fully anticipated?**


One of our engineers had an idea around observability. We have all these complex reinforcement learning models running in production, and we didn't always have great visibility into how they were behaving. Troubleshooting models was very manual. His insight was that good observability takes more than one report. You need to be able to spin up a large number of them easily, so he proposed a lightweight tool that made that trivial.


A prototype went up fast and just took fire across the team. People started building reports right away, and we had literally dozens within weeks. It dramatically cut down underperforming use cases, because before, an implementation might quietly underperform for months, with no way to diagnose it apart from custom analyses. Now, we can suddenly diagnose and fix problems quickly. It's a mainstay of how we work, called the AI Analytics Hub, and it's matured into a core tool our forward-deployed data scientists rely on to keep every customer implementation performing well. The whole thing started as one engineer's idea that they just ran with.


**You came onboard a year ago when OfferFit joined Braze. Looking back on the past year, what accomplishment are you most proud of? What stands out to you when you think about what the team has worked on and the impact it has had on customers and the customer engagement industry as a whole?**


The thing I'm most proud of is how fast we've integrated the products. Within just a few weeks of joining Braze, we'd already shipped reporting for AI Decisioning Studio natively inside Braze, and since then we've been shipping all kinds of functionality so customers can log into Braze and interact directly with AI Decisioning Studio.


There's been a huge amount of work on both the front end and the backend to make that happen. The result is a much smoother experience, especially for customers who use Braze as their engagement platform, and it's a smoother experience across the board than anything that was possible before.


**For someone who wants to build a career in the AI space, what does Braze offer that's hard to find elsewhere?**


What's hard to find elsewhere is the combination of intellectually interesting work and truly scaled impact. Many of the world's best-known brands use Braze to power their marketing and customer communications, so we sit right at the heart of the market. When you interact with a brand you're loyal to and they communicate with you, that's often what we power. We're powering people's relationships with the brands that matter most to them, which is a real privilege, because it touches both people's day-to-day lives and the success of major consumer companies.


For someone building a career in AI, getting to do really cool technical work, like ours with reinforcement learning, while having immediate practical impact at that scale, is pretty rare.


**What's it actually like to work across the AI Decisioning Studio organization?**


Even though Braze is a public company, it moves fast, and that was actually one of the main reasons OfferFit chose to join Braze. Among all the companies in the space, we knew Braze had the energy, the vitality, and the product velocity we wanted. There's a real sense of urgency in the culture, both on the AI Decisioning Studio team and across the overall Product organization.


We're a large, established company with many of the world's biggest brands as customers, but we still have the hustle and energy. The expectation is that each of us comes to work every day to demonstrate impact and make real progress.


**If someone was deciding between Braze and another company, what would you want them to know?**


What Braze combines that's pretty rare is a team that holds an extremely high bar and a team that's kind and genuine. One of our values is to "Be a Human," and it really shows up in how people treat each other. We care about each other beyond just being colleagues, and there's a real effort to be there for one another and to treat people the way you'd want to be treated. So it's a place where people are kind to each other and also very much in it to win it. That combination is what a lot of top performers are looking for, an environment with really high expectations that still brings out the best in you because the people around you actually care.


#### **Final thoughts**


Being close to the problem, close to the customer, and trusted to make real calls isn't unique to George's team. It shows up across departments at Braze as we scale globally.


#### *Ready to ignite your career at Braze?*


*We're hiring across our product teams globally, including the BrazeAI™ Decisioning Studio team. If hearing George talk about tangible impact at Braze resonates with you and you're the kind of person who wants to genuinely grow and stay curious in your work, explore our open roles.*


Check out our open roles


[Apply today!](https://www.braze.com/company/careers?gh_src=7e6eb4741us)
