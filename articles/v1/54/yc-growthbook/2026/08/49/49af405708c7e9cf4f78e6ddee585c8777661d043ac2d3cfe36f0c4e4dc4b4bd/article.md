---
schema_version: "1.0.0"
document_id: "49af405708c7e9cf4f78e6ddee585c8777661d043ac2d3cfe36f0c4e4dc4b4bd"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/sample-ratio-mismatch"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-08T04:32:57.295548+00:00"
fetched_at: "2026-08-08T04:32:58.291424+00:00"
content_hash: "sha256:8cb5e2f53200bc27d8deb3d7c3f8dd0397d6e012e8a35920726442831b493141"
---

# Sample Ratio Mismatch (SRM): How to Identify the Root Cause and Decide Whether to Restart Your Experiment

The experiment you spent weeks making the case for is finally live. It runs its planned two weeks, the treatment comes out ahead, and you open the results to write up the win. At the top of the page sits a warning: sample ratio mismatch detected.


That sample ratio mismatch (SRM) warning means the observed traffic split doesn't match the one you configured, and until you know why, you can't trust the[experiment results](https://docs.growthbook.io/app/experiment-results) .


Diagnosing an SRM is much harder than detecting it. The root cause can sit anywhere from assignment to how the data gets processed and analyzed. This article covers **the 5 types of SRM and how to prevent them** , then walks through the investigation step by step: how to isolate the cause and how to decide if the experiment can be saved.


## What is sample ratio mismatch, and why does it matter?


A sample ratio mismatch is a statistically significant gap between the traffic split you configured and the split your experiment produced. If you implemented a 50/50 test and the realized allocation is far enough from 50/50 that chance can't explain it, you have an SRM. For example, if a 50/50 test with a few thousand users in each arm comes back 45/55, that gap is far too large to be chance.


Even the most mature experimentation programs experience SRMs. For example, roughly[6% of experiments at Microsoft](https://exp-platform.com/Documents/2019_KDDFabijanGupchupFuptaOmhoverVermeerDmitriev.pdf) had SRM issues and[LinkedIn has reported rates around 10%](https://arxiv.org/abs/1808.00114) for some kinds of experiments.


Random assignment is what makes an[A/B test](https://www.growthbook.io/blog/what-is-a-b-testing) valid. It guarantees the groups are comparable, so a metric difference can be attributed to the treatment rather than to who ended up in each group. Whatever caused the mismatch added or removed users non-randomly, which introduces bias.


For example, a slower treatment loses users who bounce before the exposure event fires. Those impatient users drop out of the treatment group, but their counterparts stay in the control group. The treatment group is left with more patient, engaged users than the control, so its metrics look better even if the treatment didn’t have an effect. The result is a[false positive](https://www.growthbook.io/blog/avoid-false-positives-high-velocity-experimentation) .


A[chi-squared goodness-of-fit test](https://docs.growthbook.io/app/experiment-results#experiment-balance-srm) detects an SRM by comparing the observed unit counts to the expected ones and returning a[p-value](https://www.growthbook.io/insights/p-value-explained) , the probability of seeing an imbalance this large or larger if the samples were randomized correctly. GrowthBook runs this[check automatically](https://docs.growthbook.io/app/experiment-results) on every experiment. The test tells you an SRM exists. The rest of this guide is about finding out why.


## The 5 types of sample ratio mismatch


Every SRM traces back to one of 5 types. This grouping is based on the taxonomy from[a 2019 study of experiments run by four companies](https://exp-platform.com/Documents/2019_KDDFabijanGupchupFuptaOmhoverVermeerDmitriev.pdf) , which classified SRM root causes by the experiment stage where they enter. The diagnostic steps below tell you which type you have.


### 1. Assignment SRMs


With assignment SRMs, the randomization assigns users to variations in the wrong proportions, before any data is collected. Causes include a biased randomization function, eligibility criteria applied unevenly across variations, a hash attribute (the identifier used for bucketing) that's empty for some users, and 2 concurrent experiments whose bucketing isn't independent (users from one arm of the first test cluster into one arm of the second). Running an[A/A test](https://docs.growthbook.io/kb/experiments/aa-tests) on a new experiment surface catches most of these before a real experiment depends on them.


### 2. Exposure logging SRMs


When users are assigned correctly, but not all of them get recorded, you have an exposure logging SRM. This is one of the most common types of SRM, because assignment often happens server-side while the tracking event (in GrowthBook, the SDK's \`trackingCallback\`) fires client-side. Because bots, ad blockers, and users who bounce before a page loads often prevent tracking code from executing, some users are assigned to an experiment but never recorded. This creates a data imbalance (SRM) if the experiment’s variations cause these issues at different rates. For example, a slower variation loses more users to bounces before the tracking event can fire. Firing the exposure event from the backend, before any variation-specific code runs, is[the most reliable way to prevent this](https://docs.growthbook.io/kb/experiments/troubleshooting-experiments) .


### 3. Analysis and filtering SRMs


The data is collected correctly, but the analysis drops users unevenly, usually through a filter such as an activation metric, which limits the analysis to users who reach a certain event. If the treatment changes who reaches that event, the filter removes more users from one arm than the other. Choosing activation metrics the treatment can't influence prevents most of these.


### 4. Data processing SRMs


The exposure data is right and the analysis is configured correctly, but rows for one variation get dropped or duplicated in the pipeline, by a faulty join, a deduplication step, or bot filtering. Monitoring how many units each join and filter drops per variation catches these early.


### 5. Interference SRMs


Someone or something acts on the running experiment itself. A variation's traffic percentage gets changed mid-run, an internal team assigns itself into a variant, or an attacker submits injection strings through a form field that gets recorded in your telemetry, corrupting events in whichever variation they were assigned to. Alerts on changes to running experiments prevent most interference from your own team.


## How to identify the root cause of an SRM


Identifying the root cause of an SRM is a process of systematically ruling things out. Each step below eliminates one or more causes or points directly at the culprit. Go through each step in order, because each result tells you what to look for in the next step.


### Step 1: Confirm the SRM is unexpected


Not every SRM warning points to a data-quality problem. There are 2 configuration issues you can try and rule out right away.


#### Check that your metric matches the assignment grain


An SRM warning can be caused by metric choice rather than the experiment. A metric needs to be built at the grain you randomized, meaning its denominator counts the same unit, and it uses the same identifier.


Sessions per user, for example, is fine in a user-randomized experiment, because the denominator is users, and users are what the randomization balanced. Conversion rate per session is not, because its denominator is sessions, a unit the randomization didn't balance. A variation that changes how often visitors return generates more sessions in one arm. The session counts diverge for a legitimate reason, but the divergence still triggers an SRM warning.


The same thing applies to identifiers. Randomization only balances the specific identifier you use for hashing. If your metric relies on a different identifier, it may count units that weren’t part of the original randomization. For example, if you randomize by user ID but your metric tracks anonymous ID (common for pre-login events), a single user could map to multiple anonymous IDs. Since these IDs aren’t guaranteed to split evenly across variations, your groups will be imbalanced.


In either case, you need to rebuild the metric on the unit you actually randomized and rerun the analysis.


#### Check whether the experiment was altered mid-run


Editing the assignment ratio or targeting, or starting a new phase without re-randomizing, can also cause the imbalance on its own. If that's what happened, there are 2 solutions, and the choice depends on the data collected before the change:


- **End the analysis at the point of the change:** Choose this option when the truncated experiment still satisfies your experimental design. Cutting an experiment short can violate the experimental design in ways that bias or weaken the result. For example, ending mid-week introduces day-of-week bias, an incomplete business cycle changes which users you capture, and a smaller sample can leave the test underpowered.
- **Re-randomize and restart the experiment:** This is the only solution when the data collected before the change doesn't satisfy the experimental design.


If the metric matches the assignment grain and the experiment wasn't altered mid-run, the imbalance is unexplained. Move on to the next step.


### Step 2: Determine whether the SRM is isolated or systemic


An SRM can be systemic or specific to one experiment. It's systemic when other live experiments show SRM warnings at the same time, or when an[A/A test](https://docs.growthbook.io/kb/experiments/aa-tests) (an experiment that serves the same experience to both arms) shows one. Systemic problems live in infrastructure shared across experiments, such as the assignment system, the tracking code, or the data pipeline. Comparing experiments identifies systemic problems much faster than debugging a single experiment. GrowthBook runs the same SRM check on every experiment, so checking your other recent experiments for warnings only takes a few minutes.


- **Several experiments show an SRM at once:** The cause is likely in shared infrastructure, not in any single experiment, so start the investigation there. The 2019 study describes[a bug in Microsoft's assignment service](https://exp-platform.com/Documents/2019_KDDFabijanGupchupFuptaOmhoverVermeerDmitriev.pdf) , which randomized users into 1,000 buckets that each held 0.1% of traffic. The bug gave the control one bucket fewer than the configured split called for, so every 50/50 test on the platform ran at 49.9/50. An A/A test surfaced it.
- **Only this experiment shows an SRM:** The cause is local to this experiment's setup, data, or treatment. Continue to the next step.


### Step 3: Read the severity and direction of the SRM


Severity describes how far the observed split sits from the one you configured, and direction describes which arm is missing units. In GrowthBook, the[Health Tab's balance check](https://docs.growthbook.io/app/experiment-results) shows observed and expected units side-by-side for each variation so you quickly identify both.


- **The imbalance is small but statistically significant:** The cause is likely narrow, like a bug that affects a specific browser or a brief tracking outage.
- **The imbalance is large:** Causes this big tend to be structural, like a redirect that fails for a whole variation or a tracking event missing from one code path.
- **The treatment arm is short:** Treatment users are being lost before they're counted. This usually means an exposure or performance problem, like a slower variation where users bounce before the tracking event fires.
- **One arm is at or near zero:** The exposure event or trigger isn't firing for that arm at all, and there is probably a telemetry problem that needs to be fixed.


### Step 4: Pinpoint when the SRM started


The timing of the imbalance separates causes that were present at launch from causes that arrived later. GrowthBook's[Health Tab](https://docs.growthbook.io/app/experiment-results#health-tab) plots units by the date they were first exposed.


- **Present from day one:** Assignment or exposure logging broke before anyone entered the experiment.
- **Appears mid-run:** Something changed partway through the experiment. Check for things like a targeting change, a delayed variation start, or a change to the data source behind the experiment.
- **Strongest on day one, then fades:** A rollout effect is usually the cause here. For example, one variation starting a few hours late or cached pages serving the old experience until the cache expires. Users who joined the experiment during that window end up in one arm disproportionately, but the gap closes as more units accumulate.


### Step 5: Segment the data to localize the SRM


Many SRMs can be localized by comparing data: the filtered analysis against the unfiltered exposure counts, and the population broken down by segment. Use what you learned in the previous investigation steps to decide where to look first.


- **The SRM appears only in the triggered or filtered view, not in the unfiltered exposure counts:** The filter is likely the cause and the problem is in the analysis.[Activation metrics that sit downstream of the treatment](https://docs.growthbook.io/kb/experiments/troubleshooting-experiments) are a common culprit, because a slower or heavier variation can reduce how many users reach the activation event.
- **The imbalance originates in one dimension:** The cause is something that varies with that specific population rather than the experiment as a whole. For example, if one browser has an SRM while the rest are fine, you probably have an implementation bug in that browser. GrowthBook's[Health Tab](https://docs.growthbook.io/app/experiment-results#health-tab) makes it easy to break units down by the dimensions you’ve defined.


Most dimensions have typical causes to check for:


- **Browser:** Ad blockers or tracking-prevention features block the exposure event in that browser.
- **Device:** An SDK bug affects one app version, or slower devices lose users before the event fires.
- **Region:** Consent requirements suppress tracking for some users, or bot traffic concentrates in one geography.
- **Channel:** A campaign link sends users directly into one variation, bypassing randomization.
- **New vs. returning users:** An imbalance concentrated among returning users points to a feedback loop, like a treatment that changes how often users return and re-enter the experiment.
- **Login status:** Signing in mid-experiment switches a user from an anonymous ID to a user ID, which can hash into a different variation, so the user is re-assigned, double-counted, or removed by the multiple-exposure filter, which excludes users who were seen in more than one variation.


The login-status case only skews the split when the treatment changes how often users sign in, like a checkout that requires an account. With an uneven split like 90/10, the multiple-exposure filter compounds the loss. A re-evaluated user from the small arm usually lands in the large arm and gets removed, while large-arm users mostly re-hash into the same arm and are never flagged.


Localizing the SRM also tells you which subject matter expert in your company to reach out to, because they can often explain what happened faster than further analysis will. For example, if there are more organic search visitors in one variation of a[URL redirect test](https://docs.growthbook.io/app/url-redirects#sample-ratio-mismatch-srm-warnings-on-url-redirect-tests) , your SEO manager may know that Google indexed that variation's URL, so searchers land on it directly, bypassing randomization. Or if an imbalance is limited to one app version, the lead engineer can quickly check if a recent release affected exposure logging.


### Step 6: Compare performance and engagement metrics


Comparing performance and engagement across the arms separates a treatment-driven difference from an instrumentation one. In GrowthBook, if you track load time or errors as[guardrail metrics](https://docs.growthbook.io/app/metrics) , they will be visible alongside your goal metrics in the results table.


- **The treatment arm is short with worse load times, or long with better ones:** The treatment's performance change is altering who gets recorded, causing selection bias. A variation that adds 800ms of load time loses its most impatient users before the exposure event fires, while one that gets faster gains users instead. Either way, the two groups are no longer comparable.
- **Engagement per user differs between arms:** The direction tells you who is missing. A higher average in the short arm means less-engaged users are being lost, which points to bounces or tracking loss. A lower average means the most-engaged users are being removed, which points to filtering, like a bot detector catching heavy users.


### Step 7: Inspect the data pipeline


If nothing upstream explains the imbalance, the cause is likely in the data pipeline itself. Between the raw tracking events and the results table, the data passes through steps that join, filter, and deduplicate it, and any of those steps can drop or duplicate rows for one variation. To locate the faulty step, count the units per variation after each one. The first step where the split becomes imbalanced is the one introducing the SRM. In GrowthBook, the[experiment assignment query](https://docs.growthbook.io/kb/experiments/troubleshooting-experiments) defines how units are counted, so start there.


- **Your own count doesn't match the platform's:** Counting units per variation directly from the raw events table checks every filter, join, and date window your experiment platform's queries apply. If your count is balanced and the platform's isn't, the bug is in those queries. If your count is imbalanced too, the loss likely happened before the warehouse, which points back to exposure logging.
- **The split is balanced entering a step and skewed after it:** The imbalance is entering at that point in the pipeline, so inspect the step's logic directly. For example, if the raw exposure events are balanced but the counts skew after a join that stitches anonymous IDs to user IDs, that join is dropping or duplicating rows for one variation. This is the login-status pattern from Step 5, seen at the query level.
- **Records that failed to join skew toward one arm:** An inner join drops rows with no match on the other side, and nothing in the output indicates it happened. If the dropped rows come disproportionately from one variation, that asymmetric loss is likely the cause of your SRM. Filters deserve the same scrutiny. Count how many units each filter excludes from each variation and check whether the exclusions are balanced.
- **The identifier in your queries doesn't match the hash attribute your SDK randomizes on:** The units being counted are not the units that were assigned. This is the same mismatch from metric check in Step 1, seen from the warehouse side instead. GrowthBook's[Multiple Exposures warning](https://docs.growthbook.io/app/experiment-results#multiple-exposures) will usually appear alongside the SRM warning when it happens.


## How to decide whether to restart your experiment


Once you've identified the cause of your SRM, there are 3 possible outcomes: recover the result, restart the experiment, or salvage a directional insight. Which outcome applies depends on whether the correct data exists and whether the treatment itself changed which units were counted.


### Recover the result


If the raw events were logged correctly and a processing step created the SRM, you can fix that step, rerun the analysis, and keep the result. A bot filter that removed real users, a faulty join, or a misconfigured analysis filter all fall into this category.[The 2019 study](https://exp-platform.com/Documents/2019_KDDFabijanGupchupFuptaOmhoverVermeerDmitriev.pdf) describes a case just like this where an SRM was traced to a bot filter that had removed the most-engaged treatment users. Once the filter was corrected, the experiment showed a statistically significant win, and no rerun was needed.


### Restart the experiment


If the correct data was never captured, or the treatment changed the underlying population, no reprocessing brings the lost units back, and you have to restart. An exposure event that never fired for one arm, or a slower variation that lost users from one arm faster than the other, both leave you with groups that aren't comparable. Before you restart, fix the root cause first and re-randomize with a new salt (the value that seeds the hashing function). In GrowthBook, that means[creating a new phase](https://docs.growthbook.io/kb/experiments/carryover-bias) with re-randomization or starting a fresh experiment, so users from the first run are independently reassigned instead of carrying their old assignments forward, which would introduce carryover bias. You’ll also want to restart when the imbalance is severe and the cause remains unidentified because with an unknown bias direction, neither a win nor a loss can be trusted.


### Salvage a directional insight


If the cause is unrelated to user characteristics (like an issue with how units were allocated to buckets), or external and time-bounded (like a one-day bot spike that doesn't coincide with the movement in your metrics), you can sometimes treat the result as directional evidence. Document your reasoning before making decisions on any SRM-affected number so readers know it’s directional.


## SRM diagnosis table


Every row below maps to a step or decision above, so you can either follow the investigation in order or jump to the observation that matches what you're seeing.


**Observation** **Likely cause** **What to do**


Multiple experiments show SRMs at the same time, or an A/A test shows one Systemic issue in shared infrastructure: assignment, tracking, or the data pipeline Debug the shared infrastructure rather than the experiment


One arm has close to zero units The exposure event never fired for that arm Fix the instrumentation and restart. The data was never captured


SRM is present from day one with a correct configuration Assignment or exposure logging broke at launch: a biased randomization function, uneven eligibility, an empty hash attribute, or lost tracking events Fix the assignment or logging issue and restart


SRM appears mid-experiment A mid-run change to the assignment ratio, targeting, or data source End the analysis at the change if the truncated experiment still satisfies the design. Otherwise, re-randomize and restart


SRM is only in the triggered or filtered view, not in the unfiltered exposure counts Analysis: an activation metric or filter downstream of the treatment If the raw events were logged correctly, fix the filter and recover. Otherwise, restart


SRM is confined to one segment, such as a browser, device, region, channel, login status, or returning users Tracking that depends on browser features, a failing redirect, caching, a campaign link that forces one variation, an identifier switch at sign-in, or a treatment that changes how often users return Fix for that segment. Recover if the raw events were captured, or restart if they weren't


The treatment arm is short with worse load times, or long with better ones The treatment's performance change altered which units were counted Restart. The groups aren't comparable


The split is balanced in the raw events but skewed after a processing step, or your own count disagrees with the platform's Data processing: a join, deduplication, or filter bug Reprocess the corrected data and recover


The imbalance traces to a brief external event that doesn't coincide with the metric movement An external or user-independent cause, such as a one-day bot spike Salvage a directional insight and document your reasoning


The imbalance is severe and no root cause was found Unknown Restart. The result is not trustworthy in either direction


## How GrowthBook helps you find and fix SRMs


GrowthBook's[experimentation platform](https://www.growthbook.io/products/experimentation) automates several of the steps in this investigation. Every experiment gets an automatic[SRM check](https://docs.growthbook.io/app/experiment-results) , so you learn about an imbalance without having to run the test yourself. The[Health Tab](https://docs.growthbook.io/app/experiment-results#health-tab) covers 2 of the quickest steps directly: traffic over time to see when the imbalance started, and traffic by dimension to identify which segment is affected. A[Pre-Exposure Bias Check](https://docs.growthbook.io/app/experiment-results#pre-exposure-bias-check) flags when the groups already differed before the experiment started, and[Multiple Exposures detection](https://docs.growthbook.io/app/experiment-results#multiple-exposures) surfaces the hash-attribute mismatches that often cause SRMs. These run alongside a larger set of[data quality checks](https://docs.growthbook.io/statistics/overview#data-quality-checks) that run on every analysis. When you've found the cause of an SRM, the[troubleshooting guide](https://docs.growthbook.io/kb/experiments/troubleshooting-experiments) provides guidance on how to address the most common ones, and because the[stats engine is open source](https://github.com/growthbook/growthbook/tree/main/packages/stats) , you can read exactly how any check is computed.


If you want these health checks running on your experiments by default,[try GrowthBook for free](https://www.growthbook.io/get-started) .


‍
