---
schema_version: "1.0.0"
document_id: "2e85b64b6cba82326f00ecbd4f0d58d13abbc78e841a717f22447cae344e501d"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/technology/is-customer-lifetime-value-a-powerful-signal-or-just-false-protection-in-insurance-pricing"
published_at: "2026-02-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:19:12.535128+00:00"
content_hash: "sha256:02c80d3fde83f6f5b07d9973e69701f8d9fbf30e4335ee1d1d5efb657893c80b"
---

# Is Customer Lifetime Value a Powerful Signal or Just False Precision in Insurance Pricing?

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Technology](https://www.guidewire.com/resources/blog/technology)


- Is Customer Lifetime Value a Powerful Signal or Just False Precision in Insurance Pricing?


## My Case Against LTV as a Financial Metric


In modern insurance pricing software, customer lifetime value (LTV), also referred to as CLV or CLTV, is a business metric used primarily in marketing. It’s calculated as the present value of predicted future profits over a customer's “lifetime.” However, it should not be treated as a true financial present value of predicted future profits.


In this article, I argue that, due to the uncertainties inherent to the LTV model, it’s unreliable as a financial metric. I begin with a bit of history of LTV and compare it to a model that shares a similar structure but is considered fully reliable. From there, I look at the assumptions behind LTV and where they start to break down.


Most of the examples refer to the UK market, which provides a clear picture, as it’s a single country with relatively widespread LTV in pricing. However, most conclusions remain relevant for the rest of the world, including the Nordics, the US, and South Africa.


For those of you interested in actual application in pricing and other models, such as marketing, I recommend skipping directly to the Modern Use section.


## From Discounted Cash Flow to Annuities to LTV


The concept of discounted cash flows has existed in finance for centuries. In fact, evidence suggests that Leonardo of Pisa, the mathematician known for developing the Fibonacci sequence, was one of the first to develop a mathematical approach to present value analysis in his 1202 work Liber Abraci, using it to compare different investment alternatives. Four centuries later, it gained formalization in investment practice, where, for example, the Dutch East India Company and the British East India Company used discounting principles to value their long-term investments.


By the end of the seventeenth century, Edmond Halley (of comet fame) took the idea a step further. He saw in Breslau (or Wrocław, as it’s called today in Poland) that discounted cash flows love mortality statistics, and soon actuarial science was born.


The firstborn child of that marriage was the fair price of a life annuity. It became a cornerstone of modern actuarial science, formally combining financial discounting principles with probability theory in the context of human mortality risk.


It’s interesting to note that, although companies had long applied discounting principles to value investments, modern financial science as a formal discipline dates only to the mid-twentieth century. Before that, financial analysis was largely descriptive, based on rules of thumb and fundamentals, even though Irving Fisher had already introduced and popularized the concept of the present value of cash flows in the 1930s.


Halley basically put a price tag on an annuity by determining the lump-sum payment required today to provide a stream of annual payments for the duration of a person's life. His work was revolutionary because, for the first time, a financial product's value was based on a solid mathematical foundation of probability and present value, moving it from speculation toward a scientific discipline. Let’s remember the formula, but without classical actuarial notation:


## Where LTV Comes From and Why That Matters


During the mid-twentieth century, when finance was being formalized as a science, some economists recognized the economic value of repeat customers and the long-term cash flows they bring in. The concept of a long-term relationship with a client was formed.


Later, in the 1980s, the first formulas for Customer Lifetime Value were introduced in marketing literature. The idea gained traction as a way to quantify the value of a single customer beyond a single transaction. A decade later, as customer relationship management systems began to appear, the data collection for LTV calculation got easier and the whole concept gained momentum.


The first derivations of the LTV formula were based on probabilistic reasoning, assuming infinite time-horizon and fixed parameters. Indeed, if we model customer behavior via[Bernoulli trials](https://www.sciencedirect.com/topics/mathematics/bernoulli-trial) (with fixed retention rate r and churn rate 1-r), we find out that the number of periods the customer is retained follows a geometric probability distribution. Knowing the expected value of that distribution, the first LTV formula was:


Or equivalently:


This approach doesn’t really model cash flow per period. Instead, it estimates total cash flows within t periods as mean profit multiplied by the expected length of the relationship, adjusted by probability of the retention up to k-th period.


It didn’t take long, however, to connect the dots and derive the formula modelling a single cash flow per period, adjusted by retention rate r and discount rate d. The new formula incorporates the time value of money, bringing LTV closer to the rigor of financial mathematics. The new formula becomes:


Now, if we take a step back and allow ourselves a bit more of generality, we see that both this new LTV formula and the well-known annuity formula share the same structure:


"In this formula, d is the discount rate per period, and r is the generalized retention rate per period (generalized in the sense that it can also incorporate a conversion model for customer acquisition, and I’ll continue with this in mind).


With a bit of flexibility, we can think of the survival rate in the annuity formula as “retention in life.” This formula in its full generality can accommodate variable cash flow values, a unique discount factor per period, and more complex retention structures, making it a perfect model for actuaries to build and use.


From what I”ve been able to gather, LTV gained traction in the early 2000s as businesses developed big data and machine learning capabilities. CAS published a paper in 2004 discussing LTV in P&C pricing and underwriting, and a later paper in 2014 suggested using it as a core component in price optimization.


## Why LTV and Annuity Valuation Are Not the Same


What similarities does LTV share with an annuity?


Very few beyond the context, the structure of the formula, and the use of discount rates. At least no material similarities.


To understand why, we need to take the formulas apart and look at their components. Let’s start with the backbone of the whole thing: cash flows.


### The Difference in Cash Flows


An annuity is a liability under which the annuitant receives a fixed amount of money at regular intervals. The crucial point here is that the payment in each period is known in advance and remains unchanged throughout the payout period. In other words, there’s no uncertainty about the amount paid.


On the other hand, LTV assumes each cash flow in the equation is the profit the company gets from the customer, provided they won’t churn. Obviously, even in year zero, this value is subject to all sorts of risk factors that actuaries meticulously include in their frequency, severity, demand, and discount models. As such, the cash flow of profit from a customer can’t be considered on a per customer basis. It must be analyzed in cohorts, as an aggregated amount, much like an annuity reserve.


Pricing actuaries dedicate most of their focus to estimating premiums in year zero in such a way that, after accounting for costs and other factors, there is profit. But do we predict premiums and costs for years one, two, three, and beyond? Not really.


What we can do is assume the models for year zero will still be relevant for years one, two, and three under the mild assumption that the age of anyone and anything included as a risk factor increases by one year at a time. Does that make much sense? Not really, especially given that more and more insurance companies want to change their price models more than once per year.


Market conditions shift. Competitors change their strategies. Trade policies evolve. All of this adds to market dynamics, making frequent updates a necessity rather than a whim.


Most importantly, many companies now have the resources, pricing tools, and pipelines to support those updates. So no, the projected cash flows in the LTV model aren’t backed by solid analysis. They rely on unrealistic assumptions.


### Mortality Tables vs. Customer Retention Models


Survival probabilities (rates of “retention in life”) are derived from the mortality tables. Many countries calculate them based on population-wide mortality data. Some international organizations, like[WHO](https://platform.who.int/mortality) , also collect and compile this information.


We’re talking about large numbers, countrywide statistics with known methodology. Mortality tables have proven reliable in life insurance. Every insurance company uses them, provided it has either life insurance obligations or annuity reserves in P&C.


And importantly, when we consider annuity reserves, they can’t really be evaluated on a per-person basis. It’s only in wider analysis that they consistently prove strong enough to sit on the balance sheet.


Have you heard about the institution that collects country-wide data on customer loyalty and consistently publishes churn tables? I haven’t either.


What’s left, then, is to analyze our own customer base. Here, the situation isn’t as dire as with cash flows. There are models capable of accurately predicting future lapse events. Every actuary should know survival analysis, but not all actuaries develop their modelling skills in this area. And sometimes the data can be more of an obstacle than a help, especially if lapse prediction is not a core focus of the company.


Still, just like with cash flows, even accurate models have limits. They may predict the future reasonably well, but their stability only holds as long as the company’s situation, such as market share, portfolio composition, claims costs, remains relatively unchanged over the next few years.


### Validated Interest Rates vs. Assumed Discount Rates


Discounted cash flows produce a financial metric that allows us to compare investment alternatives by factoring in the time value of money. This is the principle that money received today can earn interest or returns over time, making it more valuable than the same amount received in the future.


The accuracy of the interest rate curve can be verified by comparing the actual interest earned with the interest expected based on the curve. It’s a common practice to rely on externally published values, such as[EIOPA RFR curves](https://www.eiopa.europa.eu/tools-and-data/risk-free-interest-rate-term-structures_en) , although in some contexts, a fixed rate determined by the company may also be allowed.


How does it relate to our two examples?


Annuity reserves are recorded as liabilities on the balance sheet. By basic accounting principles, there must be at least one corresponding asset to cover these liabilities. In such a case, the payouts from the annuity liabilities can be compared to the interest earned on the underlying assets, which helps validate the assumptions about interest rates.


LTV, on the other hand, doesn’t appear on the balance sheet. Assets cannot meaningfully be allocated to it, and there’s no investment return that can be compared to the projected cash flows. Besides, LTV in its typical formulation is based on the technical margin and excludes taxes, interest, amortization, and similar elements. While some versions may include cost of capital or investment income, these components aren’t typically broken out in a way that allows meaningful comparison with actual asset performance.


What’s left are indirect methods of validation, like sensitivity testing, scenario analysis, and backtesting. However, these tests answer a different question. They don’t tell us whether the chosen interest rate was correct. They only help us recognize and measure the risks associated with interest rate assumptions.


## So What Is LTV, Really?


Even though annuity reserve and LTV share the structure of present value of future cash flows, the former is reliable enough to sit on the balance sheet, while the latter is loaded with uncertainty and unrealistic assumptions.


If LTV isn’t a hard financial metric, then what is it? It’s a business metric that signals potential profitability of a given profile of customers. Its outputs shouldn’t be considered as exact values. If the values are irrelevant, then it’s their distribution that matters.


Think of it this way:


The LTV model can return almost any number or range of values. On a histogram of model outputs, the horizontal axis may span a wide range. What matters is not the specific value, but whether the model meaningfully separates low-profit customers from medium- and high-profit ones. That’s where the real insight lies.


And that insight should be used where it has the greatest impact: in marketing and broader business decisions. A well-calibrated model can support practical questions such as: Should we invest to retain customers from segment A? Should we stop offering insurance to segment B? If we grant an additional discount to segment C, will their LTV improve?


## How LTV Is Used in Pricing


### Optimizing Premiums Around LTV


As I already mentioned, the idea of using the LTV model as a core component in price optimization has existed in insurance for at least a decade. Optimization schemes typically take written margin as the objective and aggregate-level KPIs as constraints. Price optimization can be performed at the policy level or at the portfolio level, balancing improvements in the objective function per policy with overall KPI targets for the portfolio.


LTV can, therefore, be used as the objective function, as a KPI within a global constraint or both at the same time. While the most common use case is to treat LTV as the objective, nothing really stands in the way of using it as an aggregative constraint. Regardless of how LTV is used in optimization, one thing is certain: if you build the model, then you should definitely make it one of your most important KPIs.


Another important detail is how premiums are structured over time. In theory, the LTV formula can extend to an infinite time horizon. But in real world applications, it’s usually reduced to a two- to five-year year period. This reduces uncertainty by omitting the less credible projections for further years, although years four and five may already be too far into the future.


Modern pricing insurance software allows optimization across multiple premium dimensions, for example through joint optimization of liability and comprehensive coverage, where prices are adjusted semi-independently. In LTV-based setups, however, the common approach is to assume premiums increase steadily by a fixed percentage over time. This simplifies the problem by reducing the dimensionality of the model.


So the typical three-year setup would look like this:


Here, AC stands for acquisition cost. I include it explicitly to follow the typical formula representation, although in general, it’s a part of cash flows for y=0. The functions m and l represent written margin and lapse models, respectively, and m⋅l denotes their pointwise multiplication.


Note the assumption that premium p increases steadily at a fixed yearly rate of 1+𝛼, which reduces the dimensionality of the optimization problem to one dimension. The discount factor d plays a marginal role, but it shouldn’t be omitted for consistency.


An aggregative constraint could be placed on the mean demand, or conversion/retention) value, for example:


To make sure you evaluate your risk, demand, and lapse models at aged exposure, either rely on insurance pricing software that supports it natively or work with explicit formulas. Explicit formulas can always be adjusted to reflect time passed.


### Modeling Cross-Sell and Multi-Product LTV


As a tool for driving business decisions, LTV can be used in more holistic ways. The next example stays within the field of marketing and extends the formula to multiple products:


where the additional summation is taken over P - set of all products.


When retention rates are modeled jointly, LTV can estimate profitability potential in cross-sell scenarios. As we consider all products a customer may buy, we must account for product interdependencies.


Take a car insurance package as an example: liability and comprehensive (CASCO). Some insurers may refuse to sell standalone comprehensive coverage. They sell it only bundled with liability, which is the core of their offering. In such a scenario, a lapse in liability typically implies a lapse in casco. In fact, this is a very common pattern. When a customer rejects a renewal in one line, they are very likely to lapse in all products.


The key challenge in modelling cross-sell with LTV is data. Not all companies have the capabilities to collect high-quality data that gives a holistic view of the customer. In fact, in a typical situation, data for different product lines (e.g., home, car, travel) are stored in different systems. Even when a shared system exists, substantial differences in meaning may appear between products. Siloed data is difficult for analysts to clean and join consistently, which can severely impact the model's quality.


Once those issues are solved, the payoff from a well-calibrated and validated LTV cross-sell model can be enormous. Assuming the customer acquisition cost (CAC) has been paid, the model can guide more targeted offerings. These may be optimized to drive product adoption, increase retention, or both. Indeed, customers who hold multiple products are statistically more likely to stay longer with the company. Using LTV to drive cross-sell decisions can lock in a customer and reduce their likelihood of churn. This creates a virtuous cycle where cross-selling increases LTV, and higher LTV leads to better customer retention.


Now, some of you may ask about multidimensional optimization in cross-sell. It seems like a very logical next step. In theory, optimizing across multiple products simultaneously is attractive. However, in practice, the curse of dimensionality is real. As the number of independent premium variables grows, the number of parameters increases exponentially. While a single product or a bundle of two can usually be optimized within a reasonable timeframe, three or more products can quickly become computationally expensive.


As a result,companies often simplify the task. Optimization may be performed sequentially, one product after another, or by segment rather than across the entire portfolio at once. These approaches make the model more manageable, but they come at the cost of some precision.


### How Regulators View LTV in Pricing


As with many modern tools and approaches, regulators may view some practices as unfair or lacking transparency. In the case of LTV, their main issue seems to be in fairness. The price of an insurance policy should reflect insurance risk and not insurance shopping habits. This approach is shared by regulators in many countries, such as the UK, the US, or Poland.


However, this line of reasoning doesn’t always extend to other industries. Retailers, for example, may increase base prices to benefit from customers with lower elasticity and then offer substantial discounts that effectively bring the price back down for everyone else. Yet in those industries, no one really expects prices to reflect a specific type of customer.


Consider the UK. Through guidance, policy documents, and the 2022 price walking ban, the FCA limited aggressive price differentiation between new business and renewals. In doing so, it created conditions in which LTV becomes a logical and compliant optimization approach.


At the same time, the FCA neither explicitly allows nor explicitly forbids the use of LTV in pricing. What we do know is that many large or sufficiently advanced insurers already use it as part of their optimization process.


## Turning LTV Insights into Pricing Decisions


Thinking in terms of LTV is the first step. The next step is being able to act on it. Turning LTV from a theoretical construct into a practical business lever requires robust risk modeling and pricing optimization capabilities.


Modern insurance pricing software should do more than calculate premiums. It should allow pricing teams to test scenarios, adjust assumptions, and understand how changes ripple across the portfolio before they ever reach the market.


[Guidewire PricingCenter](https://www.guidewire.com/products/core-products/insurancesuite/pricingcenter-insurance-pricing-software) is built for that level of work. It gives pricing teams the structure and flexibility to build, refine, and deploy advanced pricing and optimization models with confidence.


If you’re looking to incorporate LTV into your pricing strategy, the right tools matter. Get in touch to learn how PricingCenter can support your approach.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
