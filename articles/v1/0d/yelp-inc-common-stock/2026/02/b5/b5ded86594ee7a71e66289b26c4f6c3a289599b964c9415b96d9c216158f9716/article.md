---
schema_version: "1.0.0"
document_id: "b5ded86594ee7a71e66289b26c4f6c3a289599b964c9415b96d9c216158f9716"
company_key: "yelp-inc-common-stock"
company: "Yelp Inc."
source_id: "yelp-inc-common-stock-rss-c1f3492370c5"
canonical_url: "https://engineeringblog.yelp.com/2026/02/how-yelp-built-a-back-testing-engine-for-safer-smarter-ad-budget-allocation.html"
published_at: "2026-02-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:18:26.702617+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:c9107cf1d92f4964d1c057df6c6f7fed0b23e1b319b20fbc279232c12c861102"
---

# How Yelp Built a Back-Testing Engine for Safer, Smarter Ad Budget Allocation

# How Yelp Built a Back-Testing Engine for Safer, Smarter Ad Budget Allocation


- Samuele Mazzanti, Applied Scientist


- Feb 2, 2026


## Introduction


Modern advertising platforms are fast-paced and interconnected: even small adjustments can have ripple effects on how ads are shown, how budgets are spent, and the value advertisers get from their ad spend.


At Yelp, Ad Budget Allocation means splitting each campaign’s spend between on‑platform inventory (our website, mobile site, and app) and off‑platform inventory (the Yelp Ad Network). We optimize this split to meet advertisers’ performance goals while growing overall revenue. Due to the complexity of the budget allocation system and its feedback loop, even small changes can lead to unexpected system‑wide effects.


To help us safely evaluate changes, we developed a Back-Testing Engine. This tool allows us to simulate the entire Ad Budget Allocation ecosystem with proposed algorithm changes, giving us a preview of real-world effects before we run full A/B tests or launch new code. All simulations use aggregated campaign data, with no personal user information involved.


In this post, we’ll share why we built this Engine, explain how it works, and reflect on how it’s improving our decision-making process.


## What is a Back-Testing Engine?


A Back-Testing Engine allows us to simulate “what if” scenarios by applying alternative algorithms or parameters against historical campaign data. Instead of testing changes live, where mistakes could impact real budgets and advertisers, we can safely preview the effects of updates in a controlled environment.


For the Yelp Ad Budget Allocation team, this means virtually rerunning past campaigns with proposed allocation strategies and measuring outcomes like spend, leads, or revenue. This approach offers a key advantage over traditional simulation methods or “back-of-the-envelope” calculations using aggregate data, which often miss important day-to-day dynamics and interactions.


As our allocation logic and partner integrations have become more sophisticated, rapid and safe innovation has become essential. The Back-Testing Engine gives us the confidence to explore improvements, validate ideas, and iterate faster, while keeping advertiser trust and system performance front and center.


## How our Ad Budget Allocation system works


Yelp’s advertising system handles budget allocation for hundreds of thousands of campaigns each month. Advertisers typically set a monthly budget, but behind the scenes, our infrastructure makes daily decisions on how much to spend, and where.


In particular, a campaign goes through the following steps:


1. **Beginning of the day** : Our system calculates how much of the campaign’s budget to allocate that day, and how to split it between Yelp and our ad network based on the campaign’s goals.
2. **Throughout the day** : Once the budget is set, the campaign generates outcomes (such as impressions, clicks, and leads) as the day progresses. While we can’t directly control the number of these outcomes, we closely monitor them as the ad budget is spent.
3. **End of day** : Our system collects the day’s results and uses them to bill the campaign.


Importantly, each day’s budget decisions depend on the outcomes of previous days, so the system constantly adapts as new outcomes come in. This is a fundamental property to take into account for our Back-Testing Engine. In fact, even small changes can have cascading, system-wide impacts over the billing period.


Below is a visual example of this day-by-day process (for example, taking December 2025 as the billing period) for two campaigns:


###


Figure 1. Campaign journey


Our Back-Testing Engine is designed to replay this daily process using historical data and simulated changes together, helping us forecast the effects of changes before we ever touch production systems.


## System overview


The Back-Testing Engine is built from eight interconnected components, each playing a distinct role in the simulation process:


1. **Parameter search space** : Defines the parameters and values to explore.
2. **Optimizer** : Selects the most promising candidates to test.
3. **Candidate** : Represents a specific set of parameter values to be tested (one value for each parameter).
4. **Production repositories** : Mirror production code (e.g., budgeting, billing).
5. **Historical daily campaign data** : Actual historical data used for simulation.
6. **Machine-learning (ML) Models for clicks, leads, etc.** : Predict daily outcomes such as impressions, clicks, and leads.
7. **Metrics** : Store main KPIs for each candidate.
8. **Logging and visualization** : Collects and displays all results.


The diagram below shows how they interact during the simulation process.


###


Figure 2. System architecture


Below, we break down each component in more detail.


### Component 1 - Parameter search space \[YAML file\]


To run a back-test, we first define which parameters we want to tune or evaluate. These might include algorithm choices, thresholds, or weights, all specified in a YAML file—a human-readable format widely used for configuration.


The file includes:


- A **date range** for the simulation.
- A **run name** to identify the test.
- The **search space** for each parameter: allowed values or intervals.


For example, suppose our budget allocation system currently uses a standard allocation approach, but we want to experiment with a new method called Algorithm X. We’re also interested in tuning a constant (called parameter Alpha) which we believe will impact allocation performance, with reasonable values ranging between -10 and +10.


To run this back-test for December 2025, we’d configure the YAML file as follows:


```text
date_interval  :
-    '  2025-12-01'
-    '  2025-12-31'


experiment_name  :    '  algorithm_x_vs_status_quo'


searches  :
-    search_type  :    '  scikit-opt'
minimize_metric  :    '  average-cpl'
max_evals  :    25
search_space  :
allocation_algo  :    skopt.space.Categorical(['status-quo', 'algorithm_x'])
alpha  :    skopt.space.Real(-10, 10)


```


Once this configuration is set, the optimizer can begin exploring different combinations of these parameters during the simulation.


### Component 2 - Optimizer \[Scikit-Optimize\]


An optimizer is necessary to select the best candidates to back-test.


For this purpose, we use the Python library Scikit-Optimize. The optimizer (Bayesian in this case) is designed to extract the most promising candidates that aim at minimizing a given metric (the one that is defined in the YAML file).


To efficiently explore the parameter space, our Back-Testing Engine uses an optimizer: specifically, the Scikit-Optimize library. The optimizer’s goal is to propose parameter combinations (candidates) that are likely to improve a chosen metric, defined in the YAML file as` minimize_metric` , in this case` average-cpl` (cost per lead).


The process begins with the optimizer suggesting an initial candidate, which is typically a random sample since no prior data exist. For example, the first candidate might be` {'allocation_algo': 'status_quo', 'alpha': 3.53}` . The Engine simulates this candidate and returns its performance metrics. In turn, the optimizer uses this feedback to select the next candidate, learning from previous results to propose combinations more likely to optimize the target metric.


This iterative loop continues until a specified number of candidates (` max_evals` in the YAML file, in this case 25) have been evaluated.


Scikit-Opt search is just one possible search strategy. Other strategies are supported, and specifically:


- **Grid search** : All the possible combinations of parameter values are back-tested. This approach requires limiting the number of values to be tested, as the number of possible combinations grows quickly. For instance if we have a parameter with 5 values, another parameter with 3 values, and a third parameter with 10 values, the total number of candidates would be 5 × 3 × 10 = 150.
- **Listed search** : Each candidate is directly specified by the user in the YAML file.


Note that for all kinds of search except Scikit-Opt, the optimizer doesn’t really act as an optimizer but just a wrapper that yields the next candidate to try.


### Component 3 - Candidate


As we have seen, each candidate is a specific combination of parameter values. This component is a key-value dictionary. In the example above (see *Component 2 - Optimizer \[Scikit-Optimize\]* ), *Candidate #1* is a dictionary:` {'allocation_algo': 'status_quo', 'alpha': 3.53}` .


### Component 4 - Production repositories \[Git Submodules\]


To support accurate back-testing, our Engine uses the same code as production by including key repositories (like Budgeting and Billing) as Git Submodules. This lets us simulate current logic or proposed changes by pointing to specific Git branches.


For example, to test a new budgeting algorithm, we add it on a separate branch, configure the Back-Testing Engine to use that branch, and run simulations. This setup enables our tests to closely match production and allows us to validate code changes in a controlled environment before rollout.


### Component 5 - Historical daily campaign data \[Redshift\]


For the back-test, the system needs to retrieve historical campaign and advertiser data from Redshift, limited to the selected simulation period (e.g., December 1–31, 2025). This data is relevant because:


- The budgeting logic may vary depending on specific campaign attributes.
- These attributes also serve as input features for the ML models (see *Component 6 - ML models for clicks, leads, etc. \[CatBoost\]* ), improving the accuracy of predicted outcomes.


All data is ingested at the campaign and date level to match the granularity of our production environment.


### Component 6 - ML models for clicks, leads, etc. \[CatBoost\]


Once daily budget allocations are set (see *Component 4 - Production repositories \[Git Submodules\]* ) and campaign characteristics are loaded (see *Component 5 - Historical daily campaign data \[Redshift\]* ), the next step is to estimate each campaign’s outcomes, such as impressions, clicks, and leads. Accurately predicting these results is challenging because:


1. These outcomes depend on external systems we don’t directly control (e.g., partner ad networks).
2. There is intrinsic randomness in user behavior, such as whether someone chooses to click on an ad.


To address this, we leverage ML models (specifically, CatBoost) trained to predict expected impressions, clicks, and leads based on daily budget and campaign features.


Using a non-parametric ML approach, instead of making simplistic assumptions (e.g. constant cost per click), allows us to accurately capture complex effects such as diminishing returns on budget, resulting in simulations that more closely reflect real-world behavior.


Using the same ML models for all candidates promotes fair comparisons. To further improve reliability, we monitor these models to prevent overfitting, checking that performance is consistent between training and hold-out datasets.


Because our models output average expected values (not integers), we apply a Poisson distribution to simulate integer outcomes. This approach captures the randomness seen in live systems.


Note: The use of ML models to predict counterfactual outcomes means this is not a pure back-testing approach, but rather a hybrid that combines elements of both simulation and back-testing.


### Component 7 - Metrics


For each candidate, we track a set of metrics that are important indicators of campaign performance or economic result for Yelp, for instance these could be per-campaign average cost-per-click, average cost-per-lead, Yelp margin, etc. These metrics are calculated from the raw simulation results for each campaign and day, including daily budgets, impressions, clicks, leads, and billing.


As already mentioned (see *Figure 1. Campaign journey* ), the raw simulation results of each candidate are obtained from “replaying” each campaign for each day. Such simulation process works as follows:


- **Beginning of the day** : The Engine, using the Budgeting repository (configured with the candidate parameters), determines each campaign’s daily budget and allocates spend across channels.
- **Throughout the day** : ML models predict the campaign’s impressions, clicks, and leads based on the allocated budget and campaign features.
- **End of day** : The Billing repository (configured with the candidate parameters) computes each campaign’s billing using the simulated outcomes and candidate parameters.


This process is repeated for each campaign and for each day in the period.


At the end, we aggregate these raw results into summary metrics, stored as key-value pairs for each candidate (e.g.,` {'avg_cpc': 1.39, 'avg_cpl': 18.48, 'margin': 0.35}` ). These global metrics make it easier to compare candidates.


### Component 8 - Logging and Visualization \[MLFlow\]


For every candidate, we log both the input parameters and the resulting metrics to MLFlow, which runs on a remote server.


This setup offers two main advantages:


- **Centralized collaboration** : All experiment results are stored in one place, making it easy for developers and applied scientists to access, review, and share findings.
- **Effortless visualization** : MLFlow’s built-in tools allow users to quickly compare and visualize candidate results without extra coding, streamlining analysis, and decision making.


## Insights & Learnings


Since adopting the Back-Testing Engine, we’ve seen clear improvements in the accuracy, speed, and safety of our experimentation. Here are the key ways it’s changed our workflow and decision-making.


### The impact on our experimentation process


Before the Back-Testing Engine, we’d typically test algorithmic changes by running A/B experiments. We’d split campaigns into control and treatment groups, measuring results and assessing risk after the fact.


While statistically sound, this approach has major limitations in our setting:


- **Limited data** : We experimented at the advertiser (not user) level, so sample sizes were relatively small for some effects to be detected.
- **Slow results** : Since most advertisers set monthly budgets, we had to wait one month to fully measure the effect of an A/B test.
- **High risk** : Mistakes or unintended consequences could have affected real advertisers.


The Back-Testing Engine changes this dynamic. Instead of relying solely on A/B tests, we can affordably and safely simulate a wide range of changes using historical data. This allows us to quickly filter out less ideal candidates and focus A/B tests only on the most promising ideas, preserving A/B testing for final validation rather than discovery.


###


Figure 3. How back-testing fits into our experimentation workflow


### Operational benefits


The introduction of back-testing has provided several additional advantages:


- **Faster productionization** : By allowing teams to implement changes directly in dedicated Git branches and immediately simulate their impact, we’re able to move promising ideas into production much more quickly. This effectively blurs the line between prototyping and production, streamlining our workflows.
- **Improved collaboration** : Scientists and engineers can now work side-by-side with production code, turning experiments into reusable, production-ready artifacts, rather than disconnected notebooks.
- **Increased prediction accuracy** : Our ML-driven simulations provide more realistic estimates of the business impact of each change, capturing complexities, like varying cost per click and cost per lead at different budget levels, that simplistic estimates often miss.
- **System fidelity** : By replaying the daily budgeting process, our Engine closely mirrors real-world operations, avoiding naive extrapolations and making results far more trustworthy.
- **Early bug detection** : Running simulations across a broad set of real data helps us catch code bugs or edge cases that would be tricky to find with unit tests alone.


Overall, the Back-Testing Engine acts as both a safety net and a launchpad, empowering us to explore, evaluate, and improve our ad system with confidence.


### Caveats, risks, and limitations


While back-testing brings significant benefits, it’s important to acknowledge its limitations:


- **Not a perfect predictor** : Back-testing relies on historical data and model assumptions, which may not capture major shifts in user, market, or partner behavior.
- **Risk of overfitting to history** : Relying too heavily on historical simulations could bias development toward optimizations that perform well on past data, potentially limiting innovation.
- **ML model dependency** : The accuracy of this methodology depends heavily on the quality and generalizability of the underlying ML models.


Being aware of these caveats helps us use back-testing more effectively, complementing it with A/B tests and real-world monitoring to ensure robust, reliable improvements.


## Conclusion


The introduction of our Back-Testing Engine has transformed the way we experiment and optimize Ad Budget Allocation at Yelp. By leveraging production code and historical data, we can evaluate changes safely and efficiently, enabling faster iteration and more informed decision-making. This approach has reduced the risks associated with live experimentation, improved collaboration between teams, and provided a more accurate picture of the impact any proposed update can have on our ad ecosystem.


While there are limitations, such as reliance on historical data and ML model accuracy, acknowledging these caveats ensures that back-testing remains a reliable tool in our experimentation toolkit. Throughout this process, we ensure that all campaign simulations use aggregated, anonymized data, prioritizing the privacy of our users and advertisers.


Altogether, the Back-Testing Engine has proven to be both a safety net and an accelerator, empowering our team to drive continuous improvement and deliver greater value to advertisers.


[Tweet](https://twitter.com/share)


### Join Our Team at Yelp


We're tackling exciting challenges at Yelp. Interested in joining us? Apply now!


[View Job](https://www.yelp.careers/us/en/c/engineering-jobs?lever-source=engineering_blog)


[Back to blog](https://engineeringblog.yelp.com/)
