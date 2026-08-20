---
schema_version: "1.0.0"
document_id: "a7a2454bc0e6aaf0c2e3025c6fc242841849122cd003b51a02966833999148c1"
company_key: "yc-lopus-ai"
company: "Lopus AI"
source_id: "yc-lopus-ai-news-import-017c159a9d87"
canonical_url: "https://lopus.ai/blogs/north-star-metric"
published_at: "2026-05-16T00:00:00+00:00"
first_seen_at: "2026-07-24T09:58:07.894057+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:7a295e2010d74d987ec95e036c6957a035f80fa1de1063475c3aa559c6f9b0f4"
---

# Your north star metric is probably wrong

## Your north star metric is probably wrong


Every startup picks a north star metric eventually. It's the one number the whole company is supposed to rally around — the thing that goes up when you're winning and down when you're not.


The problem is that most teams pick it once and never revisit it. The product changes. The market shifts. The customer base evolves. And the metric that made sense six months ago starts quietly incentivizing the wrong behavior, masking the wrong problems, or measuring something that no longer reflects what your product actually does for people.


A north star metric isn't a tattoo. It's a hypothesis about what matters most right now. And like any hypothesis, it needs to be tested, questioned, and sometimes thrown out.


## What makes a north star metric actually work


A useful north star does three things at once: it reflects the value your customers get from your product, it predicts long-term revenue, and it gives every team a clear line from their daily work to the company's trajectory.


Amplitude tracks "Weekly Learning Users" — people who consume and share three or more charts per week. That's specific. It ties to the behavior that predicts retention, it captures genuine product value (not just logins), and an engineer can draw a direct line between what they're building and whether that number moves.


Compare that to "monthly active users." MAU tells you how many people showed up. It says nothing about whether they got value or whether they'll come back. It's a census, not a health check.


The right north star is specific enough to guide decisions and broad enough to represent the whole product's health. Fail either test, and you're optimizing for a mirage.


## Three signals your metric has gone stale


### It rewards behavior you didn't intend


This is the most dangerous failure mode because it looks like success on a dashboard.


A social product tracks clicks. Teams respond rationally: they make thumbnails more provocative, headlines more sensational. Clicks go up. The weekly review looks great. But users click, find something different from what they expected, and bounce. Engagement drops. Retention drops. The metric went up while the product got worse.


At a smaller scale, I've seen B2B products track "reports generated" as a core metric. More reports sounds like more value — until you realize a customer might generate ten reports because the first nine were broken. The metric conflates frustration with engagement. If your teams can game the metric without improving the customer's experience, the metric is broken.


### It no longer matches your product's value surface


Products evolve faster than metrics do.


Facebook's notification team started by optimizing for comments — the theory being that comments represent meaningful engagement. For a while, that was true. But as the platform grew, reactions became a genuine signal of value. Sharing became a distribution mechanism. "Comments" stopped capturing the full picture.


The team evolved to "meaningful interactions" — a broader frame that encompassed the multiple ways users engaged with content. They revised their north star roughly every year. Not because the old one was bad when it was chosen, but because the product had outgrown it.


For a B2B SaaS company, this often plays out like: you started as a reporting tool, so you tracked "dashboards created." But now your product also does alerts, ad hoc queries, and embedded analytics. "Dashboards created" captures one of four value surfaces. Three-quarters of your product is invisible to your primary metric.


### It hides what's driving the number


This is the decomposition problem from[growth accounting](https://framer.com/blog/growth-accounting-framework) . A single topline metric can look healthy while the components underneath are alarming.


10% net user growth sounds good. But it's 30% new users minus 20% churn. That's a retention crisis masked by acquisition.


Or a revenue metric growing 15% quarterly — but 80% of that growth comes from one enterprise contract, and the SMB base is slowly eroding. The north star says "up." The underlying business is fragile.


If your metric can look green while your business has a structural problem, it's not a diagnostic tool. It's a comfort blanket.


## How to revisit it without making it a big production


You don't need a six-week offsite. You need an honest hour with three questions:


**What behavior does our metric actually reward?** Not what it's supposed to reward — what it rewards in practice. Ask your team leads what they optimize for week to week. If the answer doesn't match the intent, the metric has drifted.


**Does the metric cover our full value surface?** List every way a customer gets value from your product today — not six months ago, today. If the metric only covers some of those surfaces, it's too narrow and you're steering with a partial map.


**Can we decompose it and see healthy components?** Break the metric into its parts. If one component is carrying everything, or if churn is hidden inside a net number, the metric is concealing more than it's revealing.


At Lopus, this is one of the first things founders use discovery chat for after setup — decompose the current north star and see, often for the first time, what's underneath it. The exercise takes an hour. The insight lasts until the product changes enough to need a new answer.


## A useful heuristic for timing


Every time you make a major product decision — launch a new feature, enter a new market, change pricing — ask whether the north star still captures the full value of what you're building.


For most Seed to Series B companies, that means a serious evaluation every six months, with lighter check-ins quarterly. Early-stage companies where the product changes every sprint should probably revisit more frequently than that.


The clearest sign you're overdue: teams are hitting their metric targets, but the business doesn't feel like it's moving. That disconnect — metric up, conviction down — is almost always a stale north star.


## The real risk isn't picking wrong — it's never changing


Markets shift. Products grow. Customer segments evolve. The metric that captured your product's value at $10K MRR might be measuring a side effect by the time you hit $200K.


Build the habit of questioning it. Decompose it regularly. Watch for the three signals — gaming, misalignment, hidden composition. And when the metric stops telling you something true about your business, change it before it starts telling you something false.
