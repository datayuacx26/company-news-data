---
schema_version: "1.0.0"
document_id: "d6a84828df5437b0197c11a57c093d822e6cd317730f52574a46d13512dfc434"
company_key: "yc-struct-ai"
company: "Struct"
source_id: "yc-struct-ai-rss-e2c56fab48fe"
canonical_url: "https://struct.ai/articles/design-sre-slos-slis-budgets/"
published_at: "2026-07-12T05:10:56+00:00"
first_seen_at: "2026-07-27T05:30:38.706787+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:6bc781330726a93dae4a175037f1b59340e97174aba79ab01bb07dc9bed5808b"
---

# How to Design SRE SLOs, SLIs, and Error Budgets

*Written by: Nimesh Chakravarthi, Co-founder & CTO, Struct*


## Key Takeaways for SRE SLO Design


-


SLOs convert vague uptime goals into measurable reliability contracts by defining numeric targets and error budgets over fixed windows.


-


SLIs are calculated as good events divided by total events, with common types including availability, latency, throughput, and correctness.


-


Error budgets quantify allowable unreliability (for example, 0.1% of a 30-day window equals roughly 43 minutes) and drive release and incident decisions.


-


Realistic SLO targets come from historical data by setting the goal slightly above the 5th-percentile worst performance observed over 90 days.


-


[Struct instantly investigates](https://cal.com/deepanm/struct-demo) every SLO breach and delivers a correlated root-cause report before engineers open their laptops.


## SLI SLO Examples for Core User Journeys


Mapping user journeys to SLI types is the first concrete step. Each SLI type measures a different dimension of user experience: availability answers “did it work?”, latency answers “was it fast enough?”, throughput answers “did we process the expected volume?”, and correctness answers “was the result accurate?”. The table below shows how to translate each dimension into a measurable formula.


SLI Type


User Journey Example


Formula


Typical SLO Target


Availability


User can log in


Successful HTTP responses ÷ Total HTTP responses


99.9%


Latency


Checkout page loads


Requests served < 300 ms ÷ Total requests


95% under 300 ms


Throughput


Data pipeline processes events


Events processed per second ÷ Expected events per second


98%


Correctness


Payment amount matches order total


Correct responses ÷ Total responses validated


99.99%


User-journey mapping uses a cross-functional session with product, engineering, and support. The team reviews support tickets, product analytics, and existing runbooks together. They produce a prioritized list of journeys ranked by customer impact, each paired with an SLI type and formula. The trade-off is scope: mapping every journey at once creates instrumentation debt, while starting with the two or three highest-revenue paths delivers faster value.


[See how Struct correlates SLI signals across Datadog, Prometheus, and cloud logs into a single investigation timeline.](https://cal.com/deepanm/struct-demo)


## SLI SLO Error Budget Calculation in Practice


A 30-day window contains 43,200 minutes. For a 99.9% availability SLO, the error budget is 0.1% of that window.


Parameter


Value


SLO target


99.9%


Measurement window


30 days (43,200 min)


Allowed bad minutes


43.2 minutes


Budget consumed after 20-min outage


46.3% (20 ÷ 43.2)


Remaining budget


23.2 minutes


The SRE team owns this calculation. They use raw SLI time-series data and the agreed SLO target to compute a live error-budget gauge that updates continuously. The trade-off is window length: shorter windows such as 7 days react quickly to transient spikes, while longer windows such as 90 days smooth noise but delay feedback.


The following Prometheus recording rules compute error-budget consumption in real time:


```text
# Record the 30-day availability SLI - record: job:sli_availability:ratio_rate30d expr: | sum(rate(http_requests_total{code!~"5.."}[30d])) / sum(rate(http_requests_total[30d])) # Record remaining error budget as a fraction - record: job:error_budget_remaining:ratio30d expr: | 1 - ( (1 - job:sli_availability:ratio_rate30d) / (1 - 0.999) )
```


A Grafana dashboard for this setup contains three panels. First, a time-series graph of the rolling SLI ratio with the SLO threshold drawn as a horizontal reference line. Second, a stat panel showing remaining error-budget minutes with conditional coloring, green above 50%, yellow from 20% to 50%, and red below 20%. Third, a bar chart of error-budget consumption by service or endpoint to highlight the largest contributors.


Struct reads your Prometheus and Grafana data automatically when a breach fires, so engineers skip the dashboard-hunting step entirely.


## How to Set Realistic SLO Targets


Deriving targets from historical data prevents aspirational SLOs that immediately exhaust their error budgets. Teams pull 90 days of SLI data, identify the 5th-percentile worst performance, and set the initial SLO 5–10 percentage points above that floor. This approach gives the team room to improve without declaring a breach on day one.


Stakeholders are the SRE lead, the product manager, and the engineering manager. The SRE lead brings 90-day SLI time-series exports and incident logs, while the product manager provides customer-impact reports that highlight which journeys matter most. Together they decide measurement window length, SLI aggregation method, and the initial target value. The primary trade-off is ambition versus achievability: a target set too high burns the error budget during normal deployments, while a target set too low provides no reliability signal.


Measurement window selection follows a practical rule. Most teams use a 28-day rolling window because it aligns with monthly business reviews, avoids calendar-month edge effects, and provides enough data to smooth weekly traffic patterns without hiding multi-week degradation trends.


## Error Budget Policy and Escalation Tiers


An error budget policy converts a numeric budget into explicit engineering actions. Without a written policy, teams debate in real time whether a breach warrants a feature freeze, which often costs more time than the breach itself. The policy below defines four graduated response tiers, each with a clear budget threshold, status label, required action, and escalation owner.


Budget Remaining


Status


Required Action


Owner


> 50%


Healthy


Normal release cadence, continue planned work


Engineering team


20–50%


Caution


Reduce risky deployments, increase monitoring coverage


SRE lead


5–20%


Warning


Freeze non-critical releases, prioritize reliability work


Engineering manager


< 5%


Exhausted


Full feature freeze, all hands on reliability remediation


VP of Engineering


The SRE lead and the engineering manager jointly own the policy document. They use the SLO target, the error-budget calculation, and the release calendar as inputs. The result is a signed policy document referenced in the team’s incident runbook and sprint planning process. The trade-off is rigidity versus flexibility: a strict policy enforces reliability discipline but can block urgent product work, while a flexible policy preserves velocity but weakens the SLO’s credibility as an engineering contract.


## Burn Rate Alerts for SRE Teams


Burn rate measures how fast the error budget is being consumed relative to the rate that would exhaust it exactly at the end of the measurement window. A burn rate of 1.0 means the budget will be exactly depleted at window end, while a[burn rate of 14.4 on a 30-day window consumes 2% of the error budget per hour and exhausts the full budget in 50 hours](https://www.slo.foo/) .


The standard multi-window, multi-burn-rate alerting pattern uses two alert tiers:


```text
# Tier 1: Fast burn, page immediately (2% budget in 1 hour) - alert: HighBurnRate expr: | ( job:error_budget_remaining:ratio30d offset 1h - job:error_budget_remaining:ratio30d ) / 0.02 > 1 for: 2m labels: severity: page annotations: summary: "High burn rate detected, 2% budget consumed in 1h" # Tier 2: Slow burn, ticket (5% budget in 6 hours) - alert: SlowBurnRate expr: | ( job:error_budget_remaining:ratio30d offset 6h - job:error_budget_remaining:ratio30d ) / 0.05 > 1 for: 15m labels: severity: ticket annotations: summary: "Slow burn rate, 5% budget consumed in 6h"
```


The monitoring architecture for this pattern flows in a simple chain. Application metrics emit to Prometheus, recording rules compute SLI ratios and budget-remaining values, and Alertmanager evaluates burn-rate alert rules. Pages route to PagerDuty for Tier 1 and to a Slack channel for Tier 2, where Struct intercepts the Slack alert and begins automated investigation immediately.


Alert configuration is owned by the SRE or platform engineer. They use the Prometheus recording rules from the previous section and the error-budget policy thresholds as inputs. The outputs are two active alert rules with distinct severity labels and routing. The trade-off is sensitivity versus noise: tighter burn-rate thresholds catch degradation earlier but generate more pages during normal traffic spikes, while looser thresholds reduce noise but risk slower detection. Struct receives your burn-rate alert and delivers results before manual triage begins.


## Start Small With a Single SLO Implementation


The fastest path to a working SLO framework is a single critical user journey shipped in one sprint. The following checklist covers the full rollout sequence:


1.


Identify the one user journey with the highest revenue or customer-impact risk.


2.


Define the SLI type, such as availability, latency, throughput, or correctness, and write the formula.


3.


Instrument the SLI in Prometheus or your existing observability stack.


4.


Pull 90 days of historical SLI data and set an initial SLO target.


5.


Calculate the 30-day error budget in minutes.


6.


Write the error budget policy document and get sign-off from engineering management.


7.


Deploy the Prometheus recording rules and Grafana dashboard.


8.


Configure Tier 1 and Tier 2 burn-rate alerts in Alertmanager.


9.


Connect Struct to your Slack alerting channel to automate first-pass investigation.


10.


Run a 30-day pilot, review budget consumption in the next sprint planning session, and add a second user journey.


Team roles follow a clear split. The SRE or platform engineer owns steps 1 through 8, engineering management owns step 6 sign-off, and the full team participates in step 10 review. This division of ownership allows the SRE to move quickly on instrumentation while management handles policy alignment in parallel. The trade-off is coverage versus speed: starting with one journey ships faster but leaves other critical paths unmonitored until the next sprint cycle.


## How SLOs Fit Into Engineering Operations


SLI and SLO outputs feed four downstream engineering processes. In alert triage, burn-rate alerts replace threshold-based noise with budget-aware signals, so on-call engineers respond to meaningful degradation rather than transient spikes. In incident coordination, the error-budget policy determines whether a breach triggers a severity-1 response or a next-business-day ticket. In post-incident reviews, budget consumption data quantifies the reliability cost of each outage and informs prioritization of reliability work in the next sprint. In release decisions, the error budget policy gates deployments automatically with a simple rule: no budget, no release, which removes subjective debate from go or no-go calls.


Participants across these processes include on-call engineers, SRE leads, engineering managers, and product managers. They use live SLI dashboards, error-budget gauges, and the written policy as shared inputs. The outputs are incident tickets, post-mortem action items, and sprint backlog adjustments. The trade-off is process overhead: a fully integrated SLO framework adds review steps to sprint planning and incident response, but eliminates the far more expensive ad hoc debates that occur without one.


## Measurement and Continuous Improvement


SLOs require a defined review cadence to remain accurate as systems and traffic patterns evolve. A practical schedule is a weekly error-budget consumption review in the SRE team standup, a monthly SLO target review against the previous 30-day SLI baseline, and a quarterly user-journey audit to add new journeys or retire obsolete ones.


Baseline setting follows the same historical-data method used during initial target derivation. As the team ships reliability improvements, the 90-day SLI floor rises, and the SLO target should be tightened accordingly. The SRE lead owns weekly and monthly reviews, and the engineering manager owns quarterly audits. They use SLI time-series exports, incident logs, and the error-budget policy as inputs. The outputs are updated SLO documents and revised Prometheus recording rules. The trade-off is stability versus accuracy: frequent target revisions keep SLOs calibrated but create administrative overhead, while infrequent revisions reduce overhead but allow SLOs to drift away from actual system behavior.


## Common Pitfalls and Practical Best Practices


**Over-complication:** Teams that define 20 SLOs in the first sprint instrument everything but enforce nothing. Start with one or two SLOs and expand only after the review cadence is established.


**Weak ownership:** An SLO without a named owner defaults to being everyone’s responsibility, which means no one enforces the error budget policy. Every SLO document must list a primary owner by role.


**Insufficient documentation:** SLI formulas stored only in engineers’ heads create knowledge gaps during incidents and onboarding. Every SLI formula, SLO target, and policy threshold belongs in a version-controlled runbook.


**Lack of policy enforcement:** An error budget policy that is never invoked is a decoration. Engineering managers must visibly enforce the feature-freeze threshold at least once for the policy to carry weight in future release decisions.


**Accepted best practices:** Use rolling measurement windows rather than calendar windows to avoid end-of-month budget resets. Instrument SLIs at the load balancer or API gateway layer to capture user-facing behavior rather than internal service health. Review SLO targets after every major architecture change. Automate first-pass investigation of every burn-rate alert to prevent alert fatigue from eroding on-call discipline, eliminating the dashboard-hunting step mentioned earlier.


## Frequently Asked Questions


### What minimum team and tooling maturity is required to implement SLOs?


Teams with basic Prometheus and Grafana instrumentation in place can often ship a working SLO framework in a few weeks. The minimum tooling requirement is a metrics pipeline that emits request counts and error counts, a way to query those metrics such as Prometheus, Datadog, or an equivalent tool, and an alerting channel such as Slack or PagerDuty. Teams without any existing observability should instrument one endpoint before attempting SLO design.


### How do SLOs integrate with an existing Datadog or cloud-native monitoring stack?


Datadog natively supports SLO creation through its SLO management UI, where teams define monitor-based or metric-based SLOs directly against existing monitors or metric queries. For AWS CloudWatch users, CloudWatch Metrics Math can replicate the SLI ratio formula, and CloudWatch Alarms can approximate burn-rate alerting. Prometheus-based stacks use recording rules and Alertmanager as described in this guide. Struct integrates with all of these stacks and reads their outputs automatically when a breach alert fires, delivering the same sub-5-minute investigation mentioned earlier.


### How long does a full SLO rollout take for a team with no prior SRE practice?


A realistic timeline for a team starting from zero is one sprint to instrument the first SLI and set the SLO target, a second sprint to deploy the error budget policy and burn-rate alerts, and a third sprint to run the first monthly review and add a second user journey. Teams with existing Prometheus instrumentation and a written runbook can compress this to a single sprint. The most common delay is the policy sign-off step, which requires engineering management alignment and can add several business days.


### What happens when telemetry coverage is limited or logging quality is poor?


Limited telemetry constrains SLI accuracy but does not block SLO implementation. Teams with sparse logging should start with availability SLIs derived from HTTP status codes at the load balancer, which require minimal instrumentation. Correctness and throughput SLIs require richer telemetry and should be deferred until logging coverage improves. Investing one sprint in structured logging and trace ID propagation before defining SLOs pays dividends in both SLI accuracy and automated investigation quality.


### How should junior engineers be involved in on-call rotations once SLOs are in place?


SLOs and burn-rate alerts give junior engineers a clear, objective signal about incident severity, which removes the need for tribal knowledge to assess blast radius. The error budget policy document tells them exactly what action to take at each threshold. Automated first-pass investigation tools like Struct provide a correlated root-cause report before the engineer begins manual triage, giving newer team members a reliable starting point for every alert. This combination allows engineering managers to safely add junior engineers to on-call rotations without requiring senior engineer escalation for every incident.


## Conclusion: Ship Your First SLO Framework This Sprint


The path from no SLOs to an enforceable error-budget policy follows a clear sequence. Teams define SLIs for one critical user journey, set a target from historical data, calculate the 30-day error budget, write and sign the policy, deploy Prometheus recording rules and burn-rate alerts, and establish a monthly review cadence. Each step has a named owner, concrete inputs, and a measurable output. The entire framework can ship in one sprint.


The remaining gap between a working SLO framework and a high-functioning on-call operation is the investigation step. Every burn-rate alert still requires someone to open Datadog, search CloudWatch, correlate trace IDs, and identify the root cause, which consumes 30 to 45 minutes per incident and scales poorly as the team grows. Struct closes that gap by automatically investigating every SLO breach the moment the alert fires, delivering a correlated root-cause report and suggested fix while engineers focus on remediation.


[Set up Struct in under 10 minutes and let AI handle the first-pass investigation of every SLO breach your team ships this sprint.](https://cal.com/deepanm/struct-demo)
