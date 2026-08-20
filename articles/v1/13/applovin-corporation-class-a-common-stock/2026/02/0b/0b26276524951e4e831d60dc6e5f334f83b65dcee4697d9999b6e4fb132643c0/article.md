---
schema_version: "1.0.0"
document_id: "0b26276524951e4e831d60dc6e5f334f83b65dcee4697d9999b6e4fb132643c0"
company_key: "applovin-corporation-class-a-common-stock"
company: "Applovin Corporation"
source_id: "applovin-corporation-class-a-common-stock-news-import-d34419822e76"
canonical_url: "https://applovin.com/en/blog/why-started-this-blog"
published_at: "2026-02-05T00:00:00+00:00"
first_seen_at: "2026-07-24T16:57:30.934674+00:00"
fetched_at: "2026-07-28T22:21:38.029889+00:00"
content_hash: "sha256:b991b5a7312ff2d6b3912bb10a341f4fa859db18ffd4892d3241200215700210"
---

# Why we’re starting this blog

Building leaves a lot of thinking invisible. As the scope and impact of our work has grown, we’ve accumulated a large set of lessons that are worth sharing. These lessons span engineering, product, and organizational decisions.


This blog is a place to make some of that thinking public. Over time, we’ll write about how we build, how we make decisions, and how we try to balance scale, quality, and velocity. We hope this helps others understand how we work and reflects an engineering culture we are proud of.


## **Builder first, always**


I’m currently the Chief Product and Engineering Officer at AppLovin. In day-to-day work, my role is MTS (Member of Technical Staff), the same role many people on the team hold.


On most days, my work looks similar to others on the team. I spend time in code, in designs, and in technical discussions. The same is true for leaders across AppLovin Engineering and Product.


We strongly believe that leaders who set direction need to stay close to hands-on work. A deep understanding of details, how systems actually behave, where tradeoffs show up, and where complexity hides, is the foundation for making good long-term decisions.


At AppLovin, being a builder is not a phase you graduate from. It is a work and life state we intentionally want to stay in.


This mindset also shapes how we communicate internally. We do not rely heavily on polished leadership reports or presentation-driven updates. Instead, we try to communicate through the work itself. Code, dashboards, designs, and systems make state and progress visible. When communication is grounded in artifacts that run in production, it stays honest and reduces translation loss.


## **What we try to optimize for**


Our philosophy is simple to say and hard to execute.


We aim to achieve three things at the same time:


1. Build great products that make the world incrementally better
2. Build great engineering that builders are genuinely proud of
3. Help team members grow meaningfully in the process


We do not believe these goals are in conflict. If one is consistently sacrificed for another, something is wrong. It is usually an issue of incentives, structure, or taste.


## **A deliberately lean team**


We are responsible for products operating at massive scale. Our team, by comparison, is much smaller than what you might expect at similar companies.


This is not accidental.


We operate as a multi-functional team with relatively homogeneous skill sets. We avoid overly fine-grained roles and rigid ownership boundaries. Instead, we give people the freedom to move across problem spaces, technologies, and projects as the work evolves.


This allows us to move faster with fewer people, reduce organizational drag, and keep context where decisions are actually made. The tradeoff is clear. This model demands strong fundamentals and high trust, and we explicitly choose that trade.


## **Ownership without territoriality**


We do not fix scopes tightly, and almost anyone can work on almost anything.


As a result, teams do not have to protect systems they built in the past or feel permanently burdened by them. You build something, improve it until it meets the bar, and then move on. Ownership and maintenance responsibility are shared across the broader AppLovin engineering team.


This removes a large amount of hidden friction that tends to accumulate in long-lived systems and helps avoid information silos.


## **Intuition over documentation**


This may run counter to conventional wisdom, but we do not encourage heavy documentation.


Instead, we shift the burden toward intuitive engineering.


If a system is designed with fewer anti-patterns and follows a logical and elegant structure, it is naturally easier to understand and evolve. Documentation is often used as a free pass for poor design, and many companies end up with documentation that is stale, incomplete, or ignored.


Rather than trying to fix bad documentation, we try to eliminate the need for it.


This approach reduces information silos, makes shared ownership practical, and forces higher engineering discipline upfront.


## **Engineering taste: build deeply, borrow wisely**


We care deeply about engineering quality and taste.


For critical problems, we sometimes build deeply optimized internal systems when existing solutions do not meet our needs. One example is AxonCache, which we recently open-sourced.


At the same time, we are disciplined about not reinventing wheels. We leverage mature cloud infrastructure and facilities where they make sense. This is a key reason we can operate with a lean team.


## **How we use AI**


We actively leverage AI across both engineering and product, not as a novelty, but as a practical force multiplier.


On the product side, we use AI to support safety, product intelligence, and automation.


On the engineering side, we use AI for debugging, roadmap exploration, repetitive work, and virtual engineers that participate in design and problem-solving.


We are excited about AI, but we try to keep common sense intact. We use it proactively where it helps and resist hype where it distracts.


## **Why this blog exists**


The purpose of this blog is simple. We want to share learnings and practices that might be useful to others building complex systems with small, high-trust teams.


Some posts will be deeply technical.


Some will focus on product decisions.


Others will reflect on organizational tradeoffs.


Stories will be told from a builder’s perspective, because that is how we work.
