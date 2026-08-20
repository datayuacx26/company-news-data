---
schema_version: "1.0.0"
document_id: "e5e9523806b3d03b7ac1b6d9657d7151b433df2be961d8d416dd955cd57d8687"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/what-is-a-b-testing"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-31T19:45:33.762968+00:00"
fetched_at: "2026-07-31T19:45:34.191941+00:00"
content_hash: "sha256:705b1eaa17bf06ce5a035ccb481d0b802d216c64a13d530d4ba9eca94e55ea6e"
---

# What Is A/B Testing? A Complete Guide | GrowthBook | Growthbook Blog

A/B testing is simple in concept. Split your users, show them different experiences, and measure what happens.


In practice, A/B testing for product teams is rarely that clean. Real products have real constraints in tracking, assignment, and metric definition, quickly making a straightforward test complicated.


While low-velocity teams can absorb slow, isolated mistakes, high-volume experimentation at scale requires mastering the fundamentals, as flaws compound leading to bad, high-confidence product decisions. Fortunately, these failure modes are well-understood and avoidable.


## What is A/B testing


[A/B testing](https://www.growthbook.io/products/experimentation) , sometimes called split testing, is a randomized experiment in which multiple versions of something are shown to different groups simultaneously. Each group is measured against a defined metric to determine which performs better.


By randomly assigning units to each version, you control for external factors like seasonality, changes in traffic mix, and broader market conditions, so any difference in outcomes can be attributed to your change and nothing else.


## What does A/B testing look like in practice


In product development, an A/B test runs alongside your normal release process. Rather than shipping a change to everyone at once, you expose a subset of your users to the new experience while the rest continue seeing the existing one. Both groups run simultaneously, and you measure the difference.


1. Define a hypothesis including the metric you're testing against.
2. Randomly split your audience into groups, each exposed to a different version.
3. Analyze the difference between groups using a statistical framework.
4. Ship the winning variant, or go back to the drawing board with what you learned.


Without that structure, you're left comparing against historical data. Consider a team that ships a new feature and watches new signups drop 8% over the following two weeks. They blame the release and roll it back, but sales stay flat. It turns out it was a seasonal dip that would have happened regardless of what was shipped, and now the team has spent a week in firefighting mode reverting a change that had nothing to do with the decline.


Or consider a team deciding between two redesigns of the same checkout flow. Rather than debating which one to ship, they test both against the current experience simultaneously. One variant performs similarly to the control. The other increases completed purchases by 12%. Without the test, that call comes down to whoever argues most convincingly in the design review.


## Why does A/B testing matter


For product teams, the value of A/B testing isn't just finding winning variants. It's making consequential decisions about how your product works based on what users actually do, rather than what your team thinks they'll do.


It's also one of the few tools that gives teams the ability to push back on the HiPPO (the highest paid person's opinion) with something more than a gut feeling of their own. When the data says otherwise, it says so for everyone in the room.


### The critical difference: A/B testing vs gut/intuition


Without A/B testing, product decisions tend to default to a familiar set of inputs:


- **HiPPO (Highest Paid Person's Opinion)** . The person with the most seniority in the room has the most influence over what ships. Experience and instinct have value, but they're not a substitute for knowing what your users actually do
- ‍ **Best practices that may not apply to your audience** . What worked for another product, in another market, with a different user base is a starting point at best. Your users are not their users. **‍**
- **Assumptions about user behavior.** Intuition about how users will respond to a change is useful for generating hypotheses, but assumptions are often wrong. **‍**
- **Competitor copying without context** . You can see what your competitors ship, but you can't see whether it worked or what they had to give up to get there.


With A/B testing, product decisions are grounded in more reliable inputs:


- **Actual user behavior from your specific audience** . Benchmarks and case studies tell you what worked somewhere else. This tells you what works for *your* users, in *your* product.
- **Statistically validated results** . Results you can trust, reproduce, and build on rather than ones you have to take on faith.
- **Measurable business impact** . You can tie the outcome of an experiment directly to the metrics the business cares about, whether that's retention, revenue, or engagement.
- **Continuous learning** . Every experiment, whether it wins or loses, tells you something about how your users behave.


## What are the benefits of A/B testing in 2026


For modern product teams, the benefits of A/B testing go well beyond finding a winning variant. In 2026, with AI accelerating the pace of product development and raising the bar for what teams can ship, the cost of making bad product decisions has never been higher. Done consistently and rigorously, experimentation touches how teams make decisions, allocate resources, and understand their users.


### 1. Get more value from your existing traffic


Customer acquisition costs have climbed[as high as 60% since 2023](https://www.phoenixstrategy.group/blog/cac-benchmarks-by-channel-2025#:~:text=Customer%20Acquisition%20Costs%20,what%20you%20need%20to%20know) .


- Paid channels are getting more expensive as competition for inventory increases and AI-driven bidding pushes auction prices up.
- Organic search is delivering fewer clicks as LLMs answer queries before users leave the results page.
- Social platforms are increasingly designed to keep users on-platform rather than send them to yours.


Getting more value out of the traffic you already have is increasingly a business necessity, and A/B testing is how you do it systematically.


### 2. Reduce the risk of rolling out major changes


Every product change carries risk. A change can perform worse than expected for a variety of reasons: a bug that only surfaces under certain conditions, user behavior that didn't match your assumptions, or a change that worked well for one segment while degrading the experience for another. Without feature experimentation, you find out about these issues after the fact, when it has already reached your entire user base.


By[feature flagging](https://www.growthbook.io/products/feature-flags) and exposing a change to a subset of users first, you limit the damage if something goes wrong. A variant that damages an important metric affects 10% of your traffic, not 100%. If it performs well, you can roll it out knowing what to expect. If it doesn't, you can roll it back before most of your users ever see it.


### 3. Speed up product decision making


Product decisions are slow when they rely on opinion. Design reviews stretch into hours as stakeholders debate, and the person with the most seniority often wins, not because they're right, but because they're the loudest voice in the room.


Product experimentation changes how those conversations go. When you have data on how users actually behaved, the debate shifts from “I think" to "here's what we know." As one PM put it: "A/B testing turned our three-hour design debates into 30-minute data reviews."


That speed compounds over time. Teams that can make and validate product decisions faster than their competitors ship more, learn more, and course-correct before small mistakes become expensive ones.


### 4. Develop a deeper understanding of your users


Every experiment tells you something about your users, whether it wins or loses. A variant that underperforms is still evidence. It tells you what your users don't respond to, which is often just as useful as knowing what they do.


Over time, that body of evidence becomes more valuable than any single test result. Teams that maintain a searchable archive of past experiments (GrowthBook does this automatically) stop asking "Didn't we already test this?" and start forming better hypotheses from the outset. This process builds a richer understanding of their users and how they actually behave, leading to better prioritization as the most impactful initiatives become clearer.


### 5. Uncover surprising insights


Not every valuable idea looks valuable before it's tested. A Microsoft engineer once ran a quick A/B test on a low-priority change to how Bing displayed ad headlines (an idea that had sat untouched for over six months). The test showed a 12% increase in revenue, which translated to more than $100 million annually in the US alone. It turned out to be the[best revenue-generating idea in Bing's history](https://hbr.org/2017/09/the-surprising-power-of-online-experiments) and it almost never got tested at all.


These insights only surface when you have an A/B testing framework that makes it easy to ship any product change as a controlled experiment.


### 6. Build a competitive advantage


The teams that consistently outperform their competitors aren't necessarily the ones with the best ideas. They're the ones who can validate ideas faster and learn from failures.


Netflix is a[well-documented example](https://netflixtechblog.com/a-b-testing-and-beyond-improving-the-netflix-streaming-experience-with-experimentation-and-data-5b0ae9295bdf) . The company runs experiments across virtually every aspect of its product, optimizing everything from thumbnails to recommendation algorithms to ensure that data (rather than opinion) drives decisions. That commitment to experimentation at scale is part of what allows a company of that size to keep iterating as fast as it does.


The more consistently you test, the better your decisions get, and the harder it is for competitors to close that advantage.


## Who should use A/B testing (and who shouldn’t)


Most teams can benefit from A/B testing in some form. But the teams that get the most out of it tend to share a few things in common: enough volume to reach statistically meaningful results, the technical infrastructure to instrument changes correctly, and decisions that are frequent enough to make a testing practice worthwhile.


### Product teams


[Product teams](https://www.growthbook.io/roles/product-managment) **** should run experiments to make confident decisions about what to build and how to build it. Does this feature change improve engagement? Does this new experience keep users on the platform longer? Experimentation answers those questions before a change is fully committed. Smaller tests can also validate hypotheses early, before significant product development investment is made, informing broader product strategy along the way. It also gives product teams a clearer read on the actual impact of their work, which is often harder to measure than it looks.


### Engineering teams


[Engineering and dev teams](https://www.growthbook.io/roles/engineering) **** should run experiments to ship changes with confidence and get direct visibility into the impact of their work on product outcomes, not just system performance. Does this algorithm change actually improve the metric it was designed to improve? Does this infrastructure change affect user behavior in ways that weren't anticipated? Rather than shipping to everyone at once, changes can be rolled out to a subset of users first, catching unexpected behavior before it reaches your entire user base.


### Growth and marketing teams


Growth and marketing teams should run experiments to validate what actually resonates with their audience before committing to a direction. Does this landing page copy increase signups? Does this email subject line improve open rates? The feedback loops are short, and the metrics are clear, making experimentation a natural fit for fast iteration.


### Design teams


Design teams should run experiments to resolve design debates with data (rather than opinion alone) and validate changes before they're fully built. Does this layout change make the key action more obvious? Does this navigation pattern reduce friction or just introduce unfamiliarity? A/B testing gives design teams a way to move forward on contested decisions without waiting for consensus.


## When NOT to use A/B testing


A/B testing isn't the right tool in every situation. There are a few conditions where it will either produce unreliable results or simply isn't worth the investment.


- **Product is too early stage** . If you're still searching for product-market fit, optimizing individual features is a distraction. The priority at that stage is to learn whether the core product solves a real problem, which requires qualitative research and iteration, not controlled experiments.
- **Not enough units** . A/B testing requires enough users moving through the experience you're testing to produce statistically meaningful results within a reasonable timeframe. If your sample size is too small, you'll either run tests for months or make decisions based on underpowered results that don't hold up.
- **Decisions with an obvious right answer.** Some changes don't need a test. Accessibility improvements, critical bug fixes, and security patches should be shipped because they're the right thing to do for your users, not because an experiment validated them. Testing these changes introduces unnecessary delay and in some cases raises ethical questions about deliberately exposing a subset of users to an inferior or broken experience. However, it can still be valuable to run non-inferiority tests to ensure changes don’t introduce new issues that affect the customer experience.
- **No internal alignment** . A/B testing only produces value if the results get acted on. If your team can't agree on what success looks like before the experiment starts, or if stakeholders routinely override data-driven conclusions with opinion, the infrastructure of experimentation exists without the culture to support it. The tool is only as useful as the organization's willingness to trust and act on what it finds. Getting alignment on the OEC, Overall Evaluation Criteria, is usually a critical first step. If your teams can’t agree on a north star metric, then it's very difficult to grow the business effectively.
- **Significant brand changes** . A/B testing works well for changes with measurable behavioral outcomes, but brand identity isn't that kind of decision. Testing radically different brand expressions simultaneously means that different users see different versions of who you are, creating inconsistency that's difficult to undo. For changes to core brand messaging, tone, or visual identity, market research and qualitative methods are better inputs than a randomized experiment.
- **Regulated industries with constraints on user treatment** . In some industries, randomly assigning users to different experiences raises legal or ethical issues.[Healthcare](https://www.growthbook.io/industry/healthtech) ,[financial services](https://www.growthbook.io/industry/fintech) and[edtech](https://www.growthbook.io/industry/edtech) are all industries where A/B testing requires additional thought. For example, you don’t want half the students in a class to have one learning experience and the other half having another. This could be very hard on the teacher and students. Data privacy is also extremely important in these industries. This doesn’t mean these industries can’t run A/B testing. It just means they need to be more thoughtful about their experiment design. (GrowthBook's self-hosting and privacy-first architecture is specifically designed for teams operating in regulated environments), but they do mean the standard experimentation framework needs to be adapted before it can be applied safely.


## What can you A/B test?


Most teams start experimenting with the most visible parts of their product and stop there. The reality is that if a change can be measured and randomly assigned, it can be tested. That applies as much to a ranking algorithm or a model prompt as it does to a button label or a checkout flow, and the most sophisticated experimentation programs treat almost every product change as a candidate for a controlled experiment.


### User-facing product experiences


Changes to what users see and interact with directly are often the easiest to instrument, the most straightforward to design a clean experiment around, and the most immediately connected to the metrics product teams care about.


#### Copy and messaging


The words you use to describe your product, explain a feature, or prompt an action affect how users respond in ways that are hard to predict without testing. This includes headlines, body copy, error messages, empty states, and tooltips. Copy that works well in one context often fails in another, which makes experimentation more reliable than intuition.


#### Visual design elements


Colors, typography, imagery, iconography, and visual hierarchy all affect how users perceive and engage with a product. These elements are worth testing on high-traffic acquisition surfaces where visual choices directly affect first impressions and conversion.


#### Social proof and rust signals


The placement, format, and type of social proof affects how users evaluate whether to take action. Testimonials, review counts, trust badges, and case study callouts are all worth testing at high-stakes moments in the user journey, like pricing pages or checkout flows, where trust is a meaningful factor in the decision.


#### Calls to action


Button text, placement, size, and visual weight all affect whether users take the action you want. The difference between "Start free trial" and "Get started" may seem trivial, but it can produce measurable differences in click-through and conversion rates.


#### Forms and data collection


The number of fields, their order, their labels, and how validation errors are presented all affect completion rates. For teams with signup flows, checkout processes, or any other form-gated experience, this is a productive area for experimentation.


#### Layout and navigation


How you organize and present information affects how users move through a product and what they do next. Single versus multi-column layouts, card versus list views, menu structure, and the placement of key actions relative to supporting content are structural decisions that are harder to get right through intuition alone.


#### Onboarding flows


What happens in a user's first few sessions shapes everything that comes after. Changes to the number of steps, the order of actions, or the point at which users are asked to commit to something can have measurable downstream effects on activation and retention metrics.


#### Pricing and packaging display


How you present pricing affects conversion without changing the underlying price. Tier ordering, anchoring, and the framing of free versus paid features are all worth testing for any team with a monetization surface, though the effects can take time to manifest.


### Backend and infrastructure


The most impactful experiments a product team can run are often invisible to users. A change to a ranking algorithm or a model prompt can affect user behavior just as much as a redesigned interface, and without a controlled experiment, the effect is nearly impossible to isolate.


#### Infrastructure and performance


Performance improvements are generally good for users, but testing them as controlled experiments lets you quantify exactly how much they matter for the metrics you care about. Knowing which specific infrastructure investments moved conversion by 3% and which didn't gives teams a more reliable basis for deciding where to invest next.


#### Default settings and configurations


Most users never change defaults, which means the state you ship with has an outsized effect on how a feature gets used. Testing different default configurations is low-cost to implement and can meaningfully affect adoption and engagement.


#### Notification timing and content


Both the notification you send and what it says affect whether users engage with it. Testing send timing, message length, and the specific action you're prompting can improve open rates and click-through without increasing notification volume.


#### Product features and functionality


Beyond how a feature looks, you can test how it behaves. The results often reveal that users interact with the functionality in ways that don't align with the original design assumptions, which is useful information regardless of which variant wins.


#### Search and discovery


Search ranking, autocomplete behavior, and filtering defaults all affect whether users find what they're looking for. Search is often a high-intent surface where small improvements in relevance or presentation directly affect conversion or engagement.


#### Algorithms and ranking


Ranking and recommendation algorithms affect every user simultaneously, which makes them worth testing carefully. Small changes to the underlying logic can produce meaningful differences in engagement and retention that aren't visible until you measure them.


#### AI and ML models


[AI and ML models](https://www.growthbook.io/industry/ai-software) are particularly[hard to evaluate without controlled experiments](https://www.growthbook.io/blog/ai-evals-vs-a-b-testing-why-you-need-both-to-ship-genai) . A model that[scores better on benchmarks](https://www.growthbook.io/blog/why-a-b-test-your-ai) doesn't always perform better in production, which makes[A/B testing AI](https://blog.growthbook.io/how-to-a-b-test-ai-a-practical-guide/) the only way to know for sure. Performance, quality and speed are all important to test. Slight changes in system prompts also require in-depth testing.


### Growth and acquisition surfaces


Growth and acquisition surfaces are where most teams first encounter A/B testing, and for good reason. The metrics are clear, the feedback loops are short, and the tests are relatively cheap to run compared to changes deeper in the product.


#### Email campaigns


Subject lines, send timing, message length, preview text, and calls to action all affect whether users open, click, and convert. Email is one of the more forgiving surfaces for experimentation because tests are cheap to run and results come in quickly, making it a good starting point before moving into more complex product surfaces.


#### Paid ads


Ad creative, copy, targeting parameters, and landing page destinations all affect cost per acquisition and return on ad spend. Testing these systematically rather than relying on platform optimization alone gives teams more control over what's actually driving performance and makes it easier to apply what you learn across campaigns.


#### Landing pages


Landing pages connect acquisition and product, which makes them worth testing carefully. Headline copy, hero imagery, social proof placement, form length, and page structure all affect conversion, and improvements here affect the efficiency of every upstream acquisition channel.


#### Mobile app stores (ASO)


App store listings are a testable surface that many teams overlook. Screenshots, preview videos, descriptions, and icon design all affect install rates, and both the App Store and Google Play offer native tools for running controlled tests on these elements.


### Internal tools and systems


Most teams think of A/B testing as something you do on user-facing surfaces. Internal tooling is worth the same rigor. The workflows your team uses, the interfaces they navigate, and the systems that handle billing and support all affect business outcomes in measurable, improvable ways.


#### Billing systems


When and how you charge users affects conversion, retention, and revenue in ways that aren't always intuitive. Credit charging timing, trial length, grace periods, and dunning flows are all worth testing, and the effects can be substantial even when the changes seem minor.


#### Customer success


The interfaces and workflows your support team uses directly affect both resolution times and the experience customers receive on the other end. Testing different queue structures, response templates, or escalation flows can surface improvements that are invisible from the outside but meaningful to the people doing the work and the customers they're helping.


#### Dashboard and reporting Iinterfaces


How data is presented to internal users affects the decisions they make. Testing different visualizations, metric groupings, or alert thresholds can improve how quickly teams identify issues and act on them.


#### Internal search and navigation


How employees find information and move through internal tools affects productivity in ways that are easy to underestimate. Testing search ranking, navigation structure, and information hierarchy in internal tools follows the same principles as product experimentation, just with a different user base.


#### Workflow and prrocess design


Internal processes are testable too. Whether it's the order of steps in an approval flow, the default assignee for a task, or the trigger conditions for an automated action, small changes to how work moves through a system can have measurable effects on speed and accuracy.


## Different types of A/B tests


Not all experiments are structured the same way. The standard A/B test is the right tool for most situations, but there are[different types of A/B tests](https://www.growthbook.io/blog/flavors-of-experimentation-in-growthbook) for different situations.


### A/A test


An[A/A test](https://docs.growthbook.io/kb/experiments/aa-tests) runs two identical variants against each other. The purpose isn't to find a winner but to confirm your experimentation infrastructure is working correctly. You should test a number of metrics to confirm data is flowing correctly, that you're seeing an equal number of users assigned to each test. You should expect 1 out of 20 tests to show a statistically significant result with a 95%[confidence interval](https://www.growthbook.io/insights/how-interpret-confidence-interval-step-by-step) .


### A/B/n test


An A/B/n test extends the standard A/B test to include multiple variants tested simultaneously against a single control. You evaluate several hypotheses in one experiment rather than running them sequentially. Each additional variant requires more units to reach significance, so population requirements scale with the number of variants. If you have enough traffic, multiple variant tests are a great way to accelerate learning.


### Multivariate test


A multivariate test changes multiple elements simultaneously and tests combinations of them. If you're testing two headlines and two button colors, a multivariate test runs all four combinations to understand not just which elements perform better individually, but how they interact. The tradeoff is that you need considerably more traffic than a standard A/B test, because the population is split across every combination.


### Holdouts


A[holdout test](https://www.growthbook.io/blog/holdouts-in-growthbook) withholds a feature from a group of users after it has been fully rolled out to everyone else. The holdout group continues to see the old experience, which lets you measure the long-term effect on retention and engagement that takes time to manifest. A new onboarding flow might look neutral in a two-week test but show meaningful differences in retention at 90 days. Holdouts are also useful for measuring the cumulative effect of many experiments running simultaneously. By comparing the holdout group to the fully treated population over 3–6 months, you can measure the combined effect of all your experiments. Learn more about[what a holdout actually measures](https://www.growthbook.io/blog/what-does-a-holdout-measure) .


## Statistical approaches to A/B testing


Most modern experimentation platforms, like GrowthBook, give you a choice between Bayesian and frequentist statistics. Both are good options but understanding the differences can help you decide which approach is best for you.


### Bayesian statistics


[Bayesian statistics](https://docs.growthbook.io/statistics/overview#bayesian-statistics) handles hypothesis testing by expressing results as probabilities. Instead of a binary significant/not-significant decision, you get a probability distribution: what's the chance variant B is better than variant A, and by how much? This makes results easier to interpret and communicate to non-technical stakeholders. Bayesian methods can also incorporate prior beliefs about the metric being tested, helping avoid over-interpreting results from small samples.


#### Benefits of Bayesian statistics


- Results are expressed as probabilities that are intuitive to act on, like, "There's a 92% chance variant B is best.”
- The probability distribution shows the full range of likely outcomes, not just a point estimate.
- Probabilities are well-suited for communicating results to non-technical stakeholders.
- Using informed priors can help reduce uncertainty in smaller samples.


#### Drawbacks of Bayesian statistics


- Poorly calibrated priors can skew results, particularly with small sample sizes.
- Not immune to peeking; stopping rules should be defined upfront and followed.


### Frequentist statistics


[Frequentist statistics](https://docs.growthbook.io/statistics/overview#frequentist-statistics) is the more traditional approach to hypothesis testing. It calculates the probability of observing your results if there were no real difference between variants. That probability is the p-value, which is compared against a predetermined significance threshold, typically 0.05.


#### Benefits of frequentist statistics


- Widely understood, with transparent math and familiar outputs.
- Results are easy to audit or present in contexts where frequentist methods are the established standard.
- Sequential testing can be used for continuous monitoring without inflating[false positive rates](https://www.growthbook.io/blog/avoid-false-positives-high-velocity-experimentation) .
- A good fit when your team is more comfortable with p-values and confidence intervals.


#### Drawbacks of frequentist statistics


- The binary nature of significance decisions can lead to misinterpretation. Teams also frequently misread “not significant" as "no effect" rather than "insufficient evidence to detect an effect."
- Without sequential testing enabled, results are only valid if you don't peek before reaching a pre-determined sample size.


### Concepts shared by both bayesian and Frequentist statistics


Despite their differences, Bayesian and frequentist statistics share many common concepts:


- **CUPED Compatible:**[CUPED](https://www.growthbook.io/blog/cuped-for-faster-experimentation-in-growthbook) uses pre-experiment data to reduce noise in metric estimates, allowing you to detect an effect faster with the same sample size.
- **Random Assignments** : Random assignment is what makes an experiment causal. Violations (users assigned to multiple variants, or assignment correlated with the metric) can invalidate results regardless of which framework you use.
- **Statistical Significance and Confidence Level:** Both approaches use a threshold to determine when a result is reliable enough to act on. In frequentist statistics this is the significance level, while in Bayesian statistics it's expressed as a probability threshold. In both cases, set the threshold before the experiment starts.
- **Statistical Power and Sample Size:** Power is the probability of detecting a real effect when one exists. Most teams aim for 80% as a minimum. Before starting an experiment, both approaches require a[power analysis](https://docs.growthbook.io/statistics/power) to determine the sample size you need to detect the effect you're looking for. Without one, you risk either stopping too early and acting on noise, or running longer than necessary. While not as prevalent in Bayesian statistics, if you have a stopping criteria, then computing your power to detect that stopping criteria is still valuable.
- [Peeking](https://docs.growthbook.io/using/experimentation-problems#peeking) **and False Positive Risk** : Both approaches are susceptible to inflated false positive rates if you stop early based on favorable results. (GrowthBook's frequentist stats engine enables[sequential testing](https://docs.growthbook.io/statistics/sequential) to safely allow early stopping.)


### Which statistical approach should you use?


**Use Bayesian** when you want probability-based results or have well-established priors that can reduce uncertainty in smaller samples.


**Use Frequentist** when results need to meet an established statistical standard, or when you want to enable sequential testing.


## Step-by-step A/B testing process


How you plan and run a test determines whether the results can actually be trusted. Here’s the step-by-step process from developing a hypothesis all the way through to implementing a winning variant.


### Step 1: research and identify opportunities


Good experiments start with a clear understanding of where the opportunity is. For product development teams, that usually means looking at where users drop off, where engagement is lower than expected, or where there's a meaningful gap between how a feature was designed to be used and how it's actually used.


Start with quantitative data like funnel drop-off rates, feature adoption rates to identify potential opportunities, then use qualitative data like user interviews, support tickets, and session recordings to better understand the situation.


#### How to prioritize experiments


Not every problem is worth testing. The best starting point is your team's current roadmap and goals. If you're focused on improving activation this quarter, test things that affect activation. Experiments that don't connect to what your team is actively solving are a distraction, however interesting the hypothesis.


Before committing, use an[objective scoring system](https://www.growthbook.io/blog/objective-a-b-test-prioritization) or prioritization framework like ICE to evaluate each opportunity:


- **Impact** : How much could this improve the metrics your team cares about?
- **Confidence** : How sure are you it will work, based on the data and research you have?
- **Ease** : How much engineering effort does it require to implement and instrument?


### Step 2: form a strong hypothesis


A good hypothesis forces you to be specific about what you're changing, why you expect it to work, and how you'll know if it did.


A weak hypothesis sounds like: "Let's try a shorter onboarding flow."


A strong one sounds like: "Reducing the onboarding flow from five steps to three will increase 7-day activation because users are dropping off at step three."


Here are a few more examples for weak and strong hypotheses.


**Weak Hypothesis** **Strong Hypothesis**


The recommendation widget will increase sales. Adding a personalized recommendation widget below the product description will increase average order value because users who see relevant product suggestions will add more items to their cart before checkout.


We will surface errors more clearly. Replacing generic error messages with specific guidance on how to fix the issue will increase form completion rates, because users currently abandon forms at the error state without understanding what went wrong.


We think the pricing page is confusing. Replacing the feature comparison table with a use-case based pricing guide will increase trial conversion, because users in exit surveys say they can't determine which plan is right for them.


Dark mode would probably improve engagement. Adding a dark mode option to the dashboard will increase daily active usage among power users because power users spend more than 4 hours per day in the product and have requested this feature in support tickets.


Use this structure as a starting point for writing your own hypotheses:


**\[Specific change\] will cause \[measurable effect\] because \[reasoning based on research\].**


### Step 3: design your experiment


Most of the work in running a good experiment happens *before* you launch. The decisions you make at the[experimental design](https://www.growthbook.io/blog/7-steps-to-better-experiment-design) stage will determine how useful your experiment is.


#### Define your measurement criteria


Before you build anything, be clear on what you're measuring and why. Your primary metric should flow directly from your hypothesis. It's the specific effect you expect to see. If your hypothesis is that reducing onboarding steps will improve 7-day activation, then 7-day activation is your primary metric.


- **Primary Metric** : The single metric that determines whether the variant wins or loses, defined before the test starts and tied directly to your hypothesis.
- **Secondary Metrics** : Metrics that you’re not specifically trying to improve but may help you further understand your experiment's impact including related metrics and lagging indicators.
- [Guardrail Metrics](https://docs.growthbook.io/app/experiment-results#guardrails) : Metrics that you’re specifically *not* trying to hurt.


Here’s what each metric might be for our onboarding experiment example.


**Hypothesis** Reducing the onboarding flow from five steps to three will increase 7-day activation rate by 15%, because users are dropping off at step three.


Primary Metric 7-day activation


Secondary Metrics 30-day retention rate


Time to complete onboarding flow


Number of core features activated within 7 days


Guardrail Metrics Invited users (The number of referrals should at least stay the same compared with the control)


Support tickets related to onboarding confusion


Error rate on onboarding steps


#### Calculate your required sample size


The best way to ensure good decision making with experiments is to know how much data you need up front. Running an experiment without a[sample size calculation](https://docs.growthbook.io/using/experimentation-best-practices#sample-sizes) is one way to end up not knowing if you can trust your results or when to end an experiment. Most modern experimentation platforms include a power calculator. You'll need four inputs:


- **Baseline Metric Value** : Your current metric value, from your recent historical data. For conversion rates, this is a percentage; for continuous metrics like revenue or session duration, it's an average. In GrowthBook,[we can compute this for you on historical data](https://docs.growthbook.io/statistics/power#how-do-i-run-a-power-analysis) filtered down to your likely experiment population.
- **Minimum Meaningful Effect** : The smallest improvement worth shipping for; in other words, you don’t care to detect a smaller effect, because it wouldn’t be worth the extra sample size to ship.
- **Confidence Level** : Typically 95%
- **Statistical Power** : Typically 80%, meaning a 20% chance of missing a real effect.


The calculator will tell you how many units you need per variant. Divide by your average daily volume of that unit to get your required duration. That might be daily active users, daily email sends, or accounts, depending on what you're randomizing on. Make sure you're calculating based only on the population that meets your targeting criteria, not your total user base.


Many tests should run for at least two full business cycles, typically two weeks minimum, to account for day-of-week behavior patterns even if you reach your sample size sooner.


#### Designing for trustworthy results


Experiment implementation is a crucial part of running a clean causal experiment and learning what you actually set out to learn.


- Test one feature at a time so results can be attributed to a specific cause.
- Ensure random, equal traffic distribution between variants.
- Keep everything else identical between versions.
- Think upfront about which user segments might respond differently to the change and whether they should be tested separately.
- Account for novelty effect. Users sometimes behave differently simply because something is new, which can cause early results to look better than they are.
- Document the experiment in your log before launch, including hypothesis, metrics, targeting criteria, implementation details, and expected end date.


### Step 4: set up your experiment and falidate the implementation


Before you launch your experiment, validate that your experiment is configured correctly. Problems caught here are easy to fix. Problems caught after two weeks of bad data are not.


- Confirm the events you need are firing correctly and consistently across platforms and devices.
- Run an[A/A test](https://docs.growthbook.io/kb/experiments/aa-tests) if you're setting up a new experimentation platform or making changes to your assignment logic.
- Check that both variants are free from bugs and function as expected.


### Step 5: launch and monitor


Once your experiment is live, your job is mostly to leave it alone. The temptation to[check results early](https://docs.growthbook.io/using/experimentation-problems#peeking) is real, especially when there's pressure to ship, but acting on interim results is one of the most common ways teams produce conclusions they can't trust.


Monitor only for:


- **Technical Errors or Bugs** : If something is broken, stop the test and fix it.
- **Guardrail Metric Violations** : If an important metric is getting meaningfully worse, it may be worth stopping early regardless of significance.
- **Sample Ratio Mismatch** : An uneven traffic split is a signal that something is wrong with your assignment logic.


Everything else can wait until the test reaches its required sample size. If you need the flexibility to act on results before that point, enable sequential testing.


### Step 6: analyze results properly


When your experiment reaches its required sample size, resist the urge to declare a winner immediately. Good analysis goes beyond the binary question of whether the variant beats the control.


Some metrics require additional waiting time even after the experiment ends. For example, if you're measuring 7-day activation, you need to wait seven days after the last user was exposed before you can analyze that metric. Build this into your timeline upfront.


When it’s time to analyze the results:


- Confirm the experiment ran as designed and reached the sample size needed to power it properly.
- Check the confidence level against your predetermined threshold. A result that doesn't reach 95% confidence isn't automatically worthless. If a variant shows a 70% chance of being best with no meaningful guardrail violations and low implementation cost, many teams will ship it.
- Verify there was no[sample ratio mismatch](https://docs.growthbook.io/kb/experiments/troubleshooting-experiments#problem-2-srm-errors-traffic-imbalance-in-the-experiment) that could invalidate the results.
- Check secondary and guardrail metrics to confirm the variant didn't improve the primary metric while quietly harming something else.
- Analyze results by key segments to check whether the overall result is hiding meaningful differences between groups.
- Look at practical significance along with statistical significance. Statistical significance on its own doesn't tell you if this was a big win or a small win; you can learn a lot about what worked or didn't by looking at the lift directly, and considering how it compares to the cost of building and maintaining this feature.
- Document regardless of outcome. Losses are often where the most learning happens. Take the time to try to learn why your users behaved in a way you didn't expect.


### Step 7: implement and iterate


Every experiment produces an outcome worth acting on, even when your hypothesis is proven wrong.


- **If your new variant wins, fully implement it** and monitor post-launch performance. Use what you learned to sharpen the next hypothesis, a winning experiment often reveals opportunities for further improvement.
- **If your new variant loses, roll back to your control** . A losing experiment is still valuable. Analyze why your hypothesis was wrong, what the data suggests about user behavior, and whether a different approach is worth testing. Document the result so the same test doesn't get run again six months later by a different team.
- **If the result is inconclusive, iterate a few times with a learning mindset.** An inconclusive result usually means one of three things: the sample size wasn't large enough, the effect is smaller than your minimum detectable effect, or there genuinely isn't a meaningful difference between the variants.


## Advanced A/B testing strategies


Once the fundamentals are in place, these advanced techniques can create additional value as your program matures and the questions you're trying to answer get harder.


### CUPED


[CUPED (Controlled-experiment using pre-experiment data)](https://docs.growthbook.io/statistics/cuped) is a variance reduction technique that uses pre-experiment metric data to improve the accuracy of your results. By accounting for pre-existing differences between users before the experiment starts, it reduces the noise in your estimates, meaning you can detect smaller effects with the same traffic, or[reach the same level of confidence faster](https://blog.growthbook.io/cuped-for-faster-experimentation-in-growthbook/) .


GrowthBook's implementation extends CUPED with post-stratification, which uses user attributes like country or plan tier to further reduce variance by isolating the[treatment effect](https://www.growthbook.io/blog/a-practitioners-guide-to-treatment-effects-in-experimentation-ate-cate-itt-late-att-explained) from natural differences between groups. The more correlated your pre-experiment data and attributes are with the metric you're measuring, the more variance reduction you'll see.


The main requirement is that you have pre-experiment data for the metric you're testing. It works best for metrics that are frequently observed (engagement rates, session counts, revenue) and is less effective for new users or rare events where there's little pre-experiment history to draw on.


**Example** : Netflix reported CUPED[reduced variance by roughly 40%](https://www.kdd.org/kdd2016/papers/files/adp0945-xieA.pdf) for some key engagement metrics. Microsoft reported it was equivalent to adding[20% more traffic](https://www.microsoft.com/en-us/research/articles/deep-dive-into-variance-reduction/) for a majority of metrics on one product team.


### Quantile testing


Most A/B tests compare means across variants, which works well when the effect is evenly distributed across users.[Quantile testing](https://docs.growthbook.io/statistics/quantile) compares percentiles instead, making it the right tool when you care about what's happening at the extremes. A change that improves average page load time by 50ms might look neutral on a mean test while actually fixing a severe performance problem affecting your slowest 1% of users.


The main consideration is sample size. Extreme quantiles (P99, P99.9) require large samples to produce reliable estimates. It also works best when you have a clear hypothesis about which part of the distribution you're trying to move.


**Example** : An engineering team testing a backend optimization uses a P99 latency metric to confirm the change reduced worst-case load times by 7ms, even though the mean improvement was too small to detect.


### Multi-armed bandits


A[multi-armed bandit](https://www.growthbook.io/blog/guide-to-multi-arm-bandits-what-is-it-and-why-you-probably-shouldnt-use-it) is an adaptive experiment that shifts traffic toward better-performing variants as data comes in, rather than maintaining a fixed split throughout. Unlike a standard A/B test, which waits until the end to declare a winner, a bandit continuously reallocates traffic based on which variant is performing best on a single decision metric. GrowthBook uses Thompson sampling, a Bayesian algorithm that balances exploration (testing all variants) with exploitation (sending more traffic to the best performer).


Bandits work best when you have a clear single metric to optimize, five or more variants to test, and care more about minimizing exposure to poor-performing variants than understanding why each one performed the way it did. They're less suited to situations with long feedback loops, multiple goal metrics, or where statistical rigor matters more than speed.


**Example** : An ecommerce team testing five different product page layouts uses a bandit to automatically shift traffic toward the best-performing variant. This allows them to quickly capitalize on a winner during time-sensitive, days-long promotions like a Black Friday sale, while also reducing the number of users exposed to lower-converting layouts.


### Cluster experiments


Most experiments randomize at the user level, but some products require randomization at a coarser level of granularity. In B2B software, for example, you might need everyone at a company to see the same experience. Showing different variants to different users within the same organization would create confusion and contaminate results.[Cluster experiments](https://docs.growthbook.io/experimentation-analysis/cluster-experiments) solve this by randomizing at the group level (the organization, the school, the household) while still analyzing outcomes at the individual level.


The main challenge is that cluster-level randomization reduces your effective sample size. You're randomizing across a smaller number of clusters than individual users, which means you need more clusters to reach significance. GrowthBook supports cluster experiments natively, handling the statistical complexity of analyzing at a different level than you randomize through its statistics engine.


**Example** : A B2B SaaS team testing a new dashboard layout randomizes at the organization level so every user within a company sees the same variant, then analyzes individual user engagement to measure impact.


### Full-funnel testing


Most experiments measure a single metric at a single point in the user journey. Full-funnel testing measures the effect of a change across multiple stages, from initial conversion through to retention, revenue, and long-term engagement. This matters because a change that looks positive at the top of the funnel can have neutral or negative downstream effects that a single-metric test would miss entirely.


The main requirement is having metrics instrumented across the full user journey and enough traffic to detect meaningful differences at each stage. It also requires patience — downstream metrics like 30-day retention take time to manifest, which means full-funnel tests run longer than standard conversion tests.


**Example** : A team testing 7-day versus 14-day free trial lengths measures not just trial starts but 30-day conversion to paid, finding that the longer trial increased signups but reduced urgency to convert, producing a net negative revenue impact.


### Long-term holdouts


Individual experiments measure the impact of a single change.[Long-term holdouts](https://www.growthbook.io/blog/holdouts-deep-dive) measure the[cumulative impact](https://www.growthbook.io/blog/why-the-impact-of-a-b-testing-can-seem-invisible) of all your changes over time. A small group of users is withheld from new features and experiments for an extended period, typically a quarter, while the rest of the product moves forward. Comparing the holdout group to the general population reveals the true long-term value of everything you shipped, including any unexpected interactions between features that individual tests couldn't detect.


The main tradeoff is that a small percentage of users (typically around 5%) experience a degraded product for the duration.


**Example** : A product team runs a quarterly holdout and discovers that the cumulative lift from five experiments, each with a 1% lift, is only 3% relative to the holdout group because of diminishing returns.


### Incorporating research


A/B tests tell you what happened, but they can’t tell you *why* . Combining quantitative experiment results with supplemental research (user interviews, session recordings, surveys, usability testing) gives you both. A variant that wins on conversion but generates support tickets is a signal worth investigating. A variant that loses might reveal through user interviews that the hypothesis was right but the execution was wrong.


Supplemental research is most valuable at two points: before an experiment, to sharpen the hypothesis, and after an inconclusive or surprising result, to understand what the data couldn't tell you and you help generate your next hypothesis.


**Example:** A team runs an experiment on a new onboarding flow that produces an inconclusive result. User interviews reveal that users understood the new flow better but felt uncertain about committing without seeing the product first, leading to a new hypothesis worth testing.


## Common A/B testing mistakes (and how to avoid them)


Every[experimentation program can make mistakes](https://www.growthbook.io/blog/experimentation-program-mistakes-to-avoid) . Learning to recognize common experimentation pitfalls **** is the first step to not repeating them.


### 1. Running experiments without big enough samples


An underpowered experiment is one that doesn't have enough units to reliably detect the effect you're looking for. It happens when teams skip the power analysis and launch tests without knowing whether their population size or expected traffic is sufficient. Without enough data, you'll either get an inconclusive result or one that looks real but isn't stable enough to act on.


**Example** : Running tests on pages with fewer than 1,000 visitors per week.


**Solution** : Focus on the highest-traffic pages or make bolder changes that require smaller samples to detect.


### 2. Testing changes that are too small


A change that's too small to produce a detectable effect is a change that's too small to test. It happens when teams focus on incremental tweaks (like a slightly different button shade or a minor copy change) rather than changes that are likely to meaningfully affect user behavior. When these tests do reach significance, the effect size is often too small to justify implementing, and every underpowered test on a trivial change is a test you didn't run on something that could actually move your metrics.


**Example** : Testing the order of navigation menu dropdowns when users don't understand what your product does.


**Solution** : Match the boldness to your traffic volume. Smaller sample sizes need bigger swings.


### 3. Stopping tests early


Stopping a test before it reaches its required sample size is one of the most common ways teams produce results they can't trust. It happens when interim results look promising, and there's pressure to ship. The numbers seem to confirm the hypothesis, so stopping feels justified. The problem is that early data is noisier than final data, and a result that looks significant at day five may look very different at day twenty. Stopping early inflates your false positive rate, meaning you'll ship changes that don't actually work.


**Example** : Ending tests as soon as the p-value hits 0.05.


**Solution** : Predetermine the sample size and duration, and stick to them. If you need the flexibility to monitor continuously, enable[sequential testing](https://docs.growthbook.io/statistics/sequential) .


### 4. Ignoring external factors


External factors are events or conditions outside your product that affect user behavior during an experiment. It happens when teams run tests during atypical periods like a seasonal sale, a major product launch, or a news cycle, without accounting for how those conditions might skew results. A winning variant during an unusual period may reflect the context more than the change itself, and applying those results year-round can lead to poor decisions.


**Example** : Testing during Black Friday and assuming results can be replicated year-round.


**Solution** : Note external factors and retest important changes during normal periods before permanently implementing them.


### 5. Shopping metrics for significant results


Metric shopping is when teams run an experiment against many metrics and report whichever ones show significance after the fact. It happens when teams don't define their primary metric before the experiment starts, leaving the door open to interpret results selectively once the data comes in. The more metrics you test, the more likely you are to find a false positive by chance, and a result that emerges from fishing through metrics is not a result you can act on confidently.


**Example** : Testing 20 metrics, hoping one shows significance.


**Solution** : Choose your primary metric before starting. Treat others as directional insights rather than stable conclusions.


### 6. Shipping a winner without checking segment performance


Aggregate results can hide meaningful differences in how different groups of users respond to a change. It happens when teams declare a winner based on overall performance without[breaking results down by segment](https://docs.growthbook.io/using/experimenting#segmentation) . A new checkout flow might increase conversion overall but frustrate returning users who've built habits around the old one, or perform well on desktop while degrading the experience on mobile. Shipping without checking may help some users at the expense of others.


**Example** : The overall winner performs worse for important segments.


**Solution** : Always analyze results by key segments, like new versus returning users or mobile versus desktop, before implementing.


### 7. Ignoring implementation cost


Not all winning variants are worth shipping. It happens when teams evaluate experimental outcomes purely by metric lift, without accounting for the actual cost of building and maintaining the change. A variant that requires significant refactoring, introduces dependencies on other systems, or creates an ongoing maintenance burden may not be worth implementing even if the results are strong. The lift needs to justify not just the initial build but the long-term cost of owning the change.


**Example** : A lead categorization model can improve onboarding success, but implementing it requires rebuilding the underlying data model and all downstream dependencies.


**Solution** : Factor implementation cost into your hypothesis prioritization.


### 8. Optimizing for short-term metrics


A test can show an increase in conversion while masking longer-term damage. It happens when teams optimize for metrics that look good in a two-week experiment window without considering what happens to users afterward.[Dark patterns](https://www.growthbook.io/blog/dark-patterns-a-b-testing) that trick users into taking actions they might not otherwise take can boost immediate conversion rates while increasing refund rates, reducing retention, and eroding brand trust over time. A confused user and a genuinely converted user can look identical in the short term.


**Example:** An[edtech team](https://www.growthbook.io/industry/edtech) test that shortened a mandatory course tutorial showed a 20% gain in time-to-first-lesson completion, but a 7% decrease in the final exam pass rate. **‍**


**Solution:** Use[guardrail metrics](https://docs.growthbook.io/using/experimenting#metric-selection) to catch downstream damage. If a winning variant hurts retention or drives up support volume, it's not a real win.


### 9. Running one test and moving on


Experimentation compounds over time, but only if teams treat it as a continuous practice rather than a series of isolated events. The mistake happens when shipping a winner feels like the finish line. The variant performed better, the change ships, and the team moves on to the next project without asking what they learned or what to test next. A single experiment answers a single question, but the real magic comes from using that answer to sharpen the next hypothesis, and the next one after that.


**Example** : A new recommendation algorithm that increases platform engagement, but no one investigates which content types drove the increase to further refine it.


**Solution** : Create a regular testing cadence with iteration built in.


### 10. Not documenting results


Without a record of what was tested, what the hypothesis was, and what the outcome was, institutional knowledge disappears when people leave, and teams waste time re-running experiments that have already been answered. It happens when documentation is treated as an afterthought rather than part of the process.


**Example** : A variant everyone was confident would win loses. A few months later, a different team has the same idea and runs the same test.


**Solution** : Maintain a searchable experiment archive and share learnings broadly.


## How to build a culture of experimentation


Winning a single A/B test is straightforward. The harder work is building an organization where evidence, not opinion, drives decisions consistently across teams, product areas, and levels of seniority.


An[experimentation culture](https://www.growthbook.io/blog/why-a-b-testing-is-good-for-your-company-culture) means controlled experiments are the default way of resolving uncertainty. Companies with a strong experimentation culture, recognize that their win rate is only 20% and that they are terrible at predicting which features will win or lose. That insight creates humility and a determination to test everything. They recognize that the results of a test carry more weight than the instinct of the most senior person in the room, and losing an experiment is treated as useful information rather than a failure.


### What a strong experimentation culture looks like in practice


In his book[Experimentation Works](https://www.hks.harvard.edu/centers/mrcbg/programs/growthpolicy/experimentation-works-surprising-power-business-experiments) , Harvard Business School Professor Stefan Thomke identifies seven attributes that characterize organizations where experimentation is genuinely embedded. They're worth understanding not as a checklist but as a description of what the mature state actually looks like.


1. **A Learning Mindset:** Experimentation is treated as a continuous process, not a one-time validation. Most experiments won't produce dramatic results, and teams that have internalized this don't treat inconclusive results as wasted effort. They treat them as the cost of learning.
2. **Rewards Consistent With Values:** Teams are rewarded for running good experiments, not just winning ones. When compensation is tied to metrics that make experimentation difficult, or when people are punished for null results, the culture quietly dies regardless of what leadership says about it.
3. **Humility:** In a true experimentation organization, even the most senior person's assumptions get tested. Leadership's job shifts from making top-down calls to creating the conditions for good experiments and accepting what they find.
4. **Experiments Have Integrity:** Strict guidelines govern how experiments are designed and run. This means pre-registered hypotheses, appropriate sample sizes, and agreed-upon metrics before the experiment starts, not after the results come in.
5. **Tools are Trusted:** Experimentation only works if people trust what it produces. If teams routinely question the validity of results or find workarounds to avoid acting on them, the infrastructure exists, but the culture doesn't.
6. **Exploration and Exploitation are Balanced:** There's an inherent tension between running experiments to learn and shipping product to grow. Organizations that only exploit what they already know stop learning. Organizations that only explore never ship. Senior leadership has to manage that balance deliberately.
7. **Leadership Actively Promotes It:** Companies tend to become less innovative as they grow, as the distance between senior leadership and the teams doing the work increases. Experimentation cultures require leaders who actively champion the practice, not just endorse it in all-hands presentations.


### Where does your team sit on the experimentation maturity model?


Thomke and his colleagues describe five stages of experimentation maturity. These are a great way to honestly assess where your organization is and what it would take to move forward.


#### Stage 1: awareness


At the awareness stage,[leadership values experimentation](https://www.growthbook.io/blog/convincing-leadership-to-adopt-a-b-testing) , but no processes, tools, or infrastructure in place. Decisions are still mostly based on experience and intuition. If your team occasionally runs an experiment when a decision is particularly contested, this is probably where you are.


#### Stage 2: belief


At the belief stage, leadership accepts that a more disciplined approach is needed and starts investing in tools and dedicated teams. The impact on day-to-day decision-making is still minimal, but the direction is set.


#### Stage 3: commitment


At the commitment stage, experimentation becomes core to how the team operates. Some product decisions and roadmap calls now require data from experiments, and the impact on business outcomes is becoming measurable.


#### Stage 4: diffusion


At the diffusion stage, large-scale experimentation is recognized as necessary, and formal standards are rolled out across the organization, supported by tooling and training. Individual teams are no longer the bottleneck.


#### Stage 5: embeddedness


At the embeddedness stage, experimentation is fully democratized. Teams design and run their own experiments without central oversight, results are shared automatically across the organization, and the institutional memory of past experiments actively informs new ones.


When Harvard Business School's Baker Research Services[compared the stock performance](https://www.exed.hbs.edu/documents/building-culture-experimentation.pdf) of companies with strong experimentation cultures against the S&P 500 over ten years, those companies outperformed the index by a wide margin. The group included Amazon, Etsy, Facebook, Google, Microsoft, Booking Holdings, and Netflix, organizations that had spent years building the infrastructure and culture for large-scale experimentation.


## The future of A/B testing


Experimentation is evolving fast. The tools and techniques available today look very different from what existed five years ago, and the next five years will likely bring even more significant shifts. A few trends worth paying attention to:


### AI-powered testing


AI coding tools are accelerating development velocity in ways that are changing how product teams need to think about experimentation. When engineers can ship features faster, the volume of changes hitting production increases. More features shipping faster means more opportunities for something to hurt retention, conversion, or engagement before anyone catches it. Gradual rollouts and a rigorous experimentation practice matter more as shipping velocity increases.


There's also an entirely new category of things to test. Teams building AI-powered features like recommendation systems, content generation tools, and AI tutors face a challenge that standard A/B testing wasn't designed for. LLMs are non-deterministic: the same input doesn't always produce the same output, and measuring quality requires different metrics than measuring clicks or conversions. Testing whether one model prompt produces better learning outcomes than another requires an experimentation platform that can handle that kind of measurement.


GrowthBook's approach is to accelerate every step of the experimentation lifecycle from directly within the tools developers already use.[AI integrations](https://docs.growthbook.io/integrations/ai) built into the platform include automatic results summaries, hypothesis validation before a test launches, similar experiment detection using vector embeddings, metric definition generation, and SQL generation for data exploration. MCP integration lets you connect your own tools and agents directly to GrowthBook via the[MCP server](https://www.growthbook.io/platform/mcp-server) .


Learn more about[how to test AI](https://www.growthbook.io/blog/how-to-a-b-test-ai-a-practical-guide) with this practical guide.


### Real-time personalization


Traditional A/B testing delivers the same experience to everyone in a variant. The next evolution is moving beyond fixed variants toward delivering the optimal experience for each individual user in real time, based on their behavior, context, and predicted response. Multi-armed bandits are an early version of this idea, but the direction is toward much more granular personalization.


### Causal inference


As experimentation programs mature, teams are increasingly using advanced statistical methods to understand cause and effect more precisely, particularly in situations where traditional randomized experiments are difficult or impossible to run. Techniques like difference-in-differences, synthetic control, and instrumental variables are becoming more accessible to product and data teams.


### Cross-channel orchestration


Many experimentation programs are siloed by channel. A web team runs web experiments, a mobile team runs mobile experiments, and the combined effect of changes across both is rarely measured. The direction is toward experimentation infrastructure that can orchestrate and measure tests across web, mobile, email, and other touch points simultaneously.


### Privacy-first experimentation


Privacy regulations and the deprecation of third-party tracking are forcing experimentation platforms to adapt. The approaches gaining traction are those that minimize data movement, work with aggregated rather than individual-level data, and can operate within strict compliance requirements. Platforms that support[self-hosting](https://www.growthbook.io/platform/deployment-options) are well-positioned for this shift.


## How to get started with A/B testing


Getting started with A/B testing doesn't require a mature experimentation platform or a dedicated data science team, but the setup decisions you make early will either accelerate or constrain your program as it grows.


### Get your instrumentation right


Before you can run reliable experiments, you need confidence that the metrics you care about are being tracked correctly and consistently. This means checking that your event logging is complete, that events fire consistently across platforms and devices, and that your data pipeline is reliable.


Skipping this step is one of the most common reasons early experimentation programs produce results no one trusts. A test result is only as good as the data behind it, and discovering instrumentation gaps after a test has run is a frustrating way to learn that lesson.


### Start with one test


Pick a high-traffic surface where you have a clear hypothesis and a metric you can measure. Don't start with the most complex change or the most ambitious idea. Start with something where the feedback loop is short, the instrumentation is straightforward, and you have a reasonable chance of seeing a result.[Early wins help build organizational buy-in](https://www.growthbook.io/blog/convincing-leadership-to-adopt-a-b-testing) . Run the test for long enough to reach your required sample size, analyze the results honestly, and document what you learned regardless of the outcome.


Chances are, your team already has hypotheses worth testing. Look at support tickets, session recordings, and drop-off points in your funnel. If you want external inspiration, resources like[GoodUI.org](http://goodui.org/) and the[Baymard Institute](https://baymard.com/) publish evidence-based UX patterns that can serve as a starting point for simple but effective test ideas.


### Find a leadership sponsor


Experimentation programs that stick have a[senior champion](https://www.growthbook.io/blog/convincing-leadership-to-adopt-a-b-testing) : someone with enough organizational influence to protect the team's time, push back when results are inconvenient, and make the case for investing in the infrastructure. Without one, a single bad test result or a quarter of inconclusive experiments is enough to kill the program before it gets traction.


It’s ok if your sponsor isn’t technical, but they need to believe that making decisions based on evidence is worth the investment, and be willing to say so publicly when the HiPPO in the room disagrees with the data.


### Go deeper


One of the most useful things you can do early is learn from teams that have already built mature experimentation programs.


- **Trustworthy Online Controlled Experiments by Ronny Kohavi, Diane Tang, and Ya Xu** : The most rigorous and practical book on running experiments at scale, written by the people who built experimentation programs at Microsoft, Google, and LinkedIn.
- **Experimentation Works by Stefan Thomke** : This book takes a broader look at building an experimentation culture, grounded in research across dozens of organizations.
- [GrowthBook Docs](https://docs.growthbook.io/) **:** Detailed guides covering everything from getting started to advanced statistical methods, with a practical guide to scaling experimentation at your company.


### Join the community


Having access to people who have already solved the problems you're facing is one of the most underrated resources in experimentation, and the experimentation community loves to share knowledge and help each other out.


- [Trustworthy A/B Patterns](https://docs.google.com/document/d/1XVpFGkTH2l70mWiOYBHNnT-J1-Ewiko3/edit) **:** GrowthBook is partnering with industry pioneers Ronny Kohavi, Lukas Vermeer, and Jakub Linowski to offer e-commerce companies with over 1 million monthly active users free expert assistance in designing and executing high-impact A/B tests in exchange for the right to publish the results.
- [GrowthBook Slack](https://slack.growthbook.io/?ref=blog) : A free Slack community where you can ask questions, share learnings, and connect with other teams running experiments from those just getting started to those running robust programs at scale.
- [Test & Learn Community](https://testandlearn.community/) : A free community of over 2,000 practitioners across experimentation, product, analytics, and research. Members meet regularly on Zoom to discuss topics, hear from industry leaders, and help each other solve real problems.


## How to choose an A/B testing platform


The[experimentation platform you choose](https://www.growthbook.io/compare) now will shape your experimentation program for years to come. It determines what you can test, how fast you can move, and how much you can trust your results. And because experimentation infrastructure becomes deeply embedded in your codebase and data pipelines over time, switching platforms is expensive and disruptive enough that most teams avoid it, so it's worth getting right the first time.


### Technical fit and developer experience


The platform needs to work with how your team already builds. A tool that requires significant engineering lift to integrate, or doesn't support your tech stack, will create friction from day one and limit who can actually run experiments.


- **Target Use Cases:** Was this platform built for product and engineering teams or marketing-led CRO? The answer shapes everything from the SDK architecture to the statistical methods available, and a tool designed for visual editing and landing page optimization will quickly run into issues testing algorithms or server-side features.
- **SDK Coverage:** The platform needs to integrate cleanly into how your team already builds, without requiring significant backend engineering every time a new test is created. Look for SDKs that evaluate locally with no network requests (keeping performance impact minimal and ensuring experiments work regardless of connectivity), and that cover your full stack. GrowthBook offers[24+ SDKs](https://docs.growthbook.io/lib/) covering frontend, backend, mobile, and edge environments, working with virtually any stack.
- **Feature Flag Integration** : The best experimentation platforms combine[feature flags](https://www.growthbook.io/blog/what-are-feature-flags) and experiments in a single tool. This lets you use the same flag to run an experiment, do a phased rollout, and kill a change if something goes wrong, without switching between systems.
- **Integration Complexity:** How long does it take to instrument your first experiment? A good platform should have clear documentation and a quick-start path that doesn't require backend engineering for every new test.
- **Scalability** : Can it handle your traffic volume without degrading performance or requiring you to limit how much of your user base is exposed to experiments?
- **Environment and Release Management** : Does it support separate staging and production environments, and can you roll out changes incrementally without redeploying code?
- [AI and MCP integration](https://www.growthbook.io/platform/mcp-server) : Does the platform support AI-assisted workflows so your team can work smarter and faster, or support MCP so you can integrate your own tools and agents?


### Statistical rigor and data ownership


The platform needs to produce results you can actually trust, and that means being transparent about how the statistics work.


- **Statistical Transparency** : Can you see the methodology behind the results? Look for platforms that support both Bayesian and frequentist approaches, publish their stats engine openly, and don't hide their calculations behind proprietary black boxes.
- [Warehouse-Native Analysis](https://www.growthbook.io/platform/warehouse-native) : The best platforms let you analyze data directly in your existing warehouse (Snowflake, BigQuery, Redshift, Databricks) rather than requiring you to send data to a third-party system. This means your experiment data lives alongside your product data, you define metrics using SQL you control, and there's no duplicate data pipeline to maintain.
- [Managed Warehouse](https://www.growthbook.io/get-started) : If you don’t have your own data warehouse, some vendors offer a pre-configured data warehouse. This allows you to start out from day 1 with an industry-standard data warehouse without setting one up or creating your own data pipelines.
- **Metrics Definition and Governance:** Who defines the metrics and how? Look for platforms that let your data team define metrics centrally using your own data definitions, rather than forcing you to redefine them inside the tool.
- **Data Ownership** : When you stop using the platform, do you keep your experiment history and learnings? Proprietary platforms that hold your data hostage create switching costs that go beyond the tool itself.
- **Targeting and Segmentation** : Can you randomize at the level that makes sense for your product (user, account, organization, session) and analyze results by segment without the platform limiting how you slice the data?


### Security, compliance, and deployment options


Security and compliance requirements vary widely across industries, but the cost of getting this wrong is high regardless. Data residency issues, compliance violations, and PII exposure can all stem from choosing a platform that wasn't built with these constraints in mind.


- [Self-Hosting](https://www.growthbook.io/platform/deployment-options) : Can you run the platform on your own infrastructure? Cloud-only platforms create data residency issues for teams with strict compliance requirements and require you to send user data to a third-party system. Self-hosting gives you full control over where your data lives.
- **Privacy and PII handling** : Does the platform require you to send personally identifiable information to its servers to run experiments? Look for platforms that assign experiments locally, with no user data leaving your infrastructure.
- **Open Source vs Proprietary** : Open-source platforms allow you to audit the code, customize the platform to your needs, and avoid vendor lock-in, but they require engineering resources for maintenance.
- **Compliance and Regulatory Requirements** : If you operate in healthcare, financial services, education or other regulated industries, the platform you choose should support your compliance requirements out of the box.


### Accessibility and collaboration


Experimentation only scales when the whole team can participate. A platform that creates friction for non-technical users will limit how often experiments get run and who benefits from the results.


- **Ease of Use for Non-Technical Teams** : Can a product manager set up and launch an experiment without engineering support? Look for intuitive interfaces, clear result summaries, and workflows that don't require SQL or statistics knowledge to navigate.
- **Result Sharing and Reporting** : How easy is it to share experiment results with stakeholders? Look for shareable dashboards, exportable reports, and result summaries that translate statistical outcomes into plain language.
- **Experiment Documentation** : Does the platform make it easy to document hypotheses, decisions, and learnings in a way that's searchable and accessible to the whole team? A searchable experiment archive is one of the most valuable things an experimentation program can build over time.
- **Permissions and Governance** : As your program grows, you need the ability to control who can create, approve, and ship experiments. Look for role-based permissions and approval workflows that you can tailor to how your organization actually operates.


### Pricing and total cost of ownership


With experimentation platforms, the sticker price is rarely the full cost. How a platform charges you shapes how much you can experiment, and the wrong pricing model can quietly constrain your program as it grows.


- **Pricing Model** : Does the platform charge per event or based on traffic volume? These models create a direct conflict between running more experiments and controlling costs, often forcing teams to test on a fraction of their traffic to avoid overage fees. Look for predictable pricing that doesn't penalize you for growing.
- **Build vs Buy** : Are you better off[building or buying an experimentation platform](https://www.growthbook.io/blog/a-b-testing-platforms-build-vs-buy) ? Building an in-house solution gives you full control but most companies[underestimate the complexity](https://www.growthbook.io/blog/the-hidden-complexities-of-building-your-own-a-b-testing-platform) and risks of doing so. Most teams underestimate this cost until they're already committed, and the opportunity cost of those engineers not working on the product is rarely factored in.
- **Modular vs All-in-one Pricing** : Some platforms charge separately for server-side, client-side, and feature flag capabilities. What starts as one tool quickly becomes multiple SKUs with compounding costs.
- **Switching Costs** : What happens if you outgrow the platform or want to move? Proprietary data formats, locked-in experiment history, and deep SDK integrations all make switching painful. Factor this into your evaluation upfront rather than after you've committed.


### Why GrowthBook


GrowthBook is the warehouse-native feature flagging, experimentation, and product analytics platform built for product and engineering teams. It's used by over 3000 companies, from early-stage startups running their first experiments to enterprises processing billions of feature flag evaluations per day. Here’s why you should consider using GrowthBook:


- No per-traffic or per-event pricing, so you can run experiments on as much of your traffic as you want without watching costs balloon. Learn more about[GrowthBook pricing](https://www.growthbook.io/pricing) .
- Analysis runs directly on your[existing data warehouse](https://www.growthbook.io/platform/warehouse-native) (Snowflake, BigQuery, Redshift, Databricks), with no need to send data to a third-party system.
- The platform is[open source](https://github.com/growthbook/growthbook) , your data stays yours, and you can self-host if you need full control.
- [Feature flags](https://docs.growthbook.io/features/basics) and[experimentation](https://www.growthbook.io/products/experimentation) are unified in a single platform, so you're not juggling separate tools for rollouts and tests.
- The stats engine supports both[Bayesian](https://docs.growthbook.io/statistics/overview#bayesian-statistics) and[frequentist](https://docs.growthbook.io/statistics/overview#frequentist-statistics) approaches,[CUPED](https://docs.growthbook.io/statistics/cuped) , post-stratification,[sequential testing](https://docs.growthbook.io/statistics/sequential) , and advanced techniques like[cluster experiments](https://docs.growthbook.io/experimentation-analysis/cluster-experiments) and[holdouts](https://docs.growthbook.io/app/holdouts) .
- Built-in tools for experimentation culture and[deep insights](https://www.growthbook.io/products/product-analytics) : a searchable experiment archive, shareable dashboards, and an interface designed for the whole team, not just data scientists.
- With[24+ SDKs](https://docs.growthbook.io/lib/quickstart) covering frontend, backend, mobile, and edge environments, it works with virtually any stack.


You can[start for free](https://www.growthbook.io/get-started) and scale from there. The free tier gives you everything you need to run your first experiments, while the Enterprise plan adds advanced features like holdouts and the governance tools that mature programs need.


## Start A/B testing the right way


A/B testing doesn't replace judgment, but it gives judgment something solid to work with. The teams that get the most out of it aren't running the most experiments. They're asking sharper questions, defining better metrics, and building enough rigor into their process that results can actually be trusted.


That's harder to build than it sounds. But the organizations that do it stop having the same arguments about what to ship. They stop reverting changes based on noise. They stop leaving product decisions to whoever made the most compelling case in the last meeting.


In 2026, the companies pulling ahead are the ones replacing guesswork with evidence.


‍
