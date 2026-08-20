---
schema_version: "1.0.0"
document_id: "2dff366b7068b1c4c985259f55b6c2b35ff833a3d34c0d473b9ee38b97db13a2"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/customer-feedback-analytics-rfp-requirements"
published_at: null
first_seen_at: "2026-07-22T16:27:08.884527+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:2d93ed645c432a4635b8433db2c426e8ee1913507fa797cae7717fcc3222e0f4"
---

# What to Require in a Customer Feedback Analytics RFP: 9 Requirements That Matter

Most RFPs for a customer feedback or text analytics platform ask the wrong questions. They collect a[feature checklist](https://www.getthematic.com/insights/must-have-features-in-feedback-analysis-software) , sit through four polished demos on the vendor's cleanest sample data, and score everyone 4 out of 5. Six months later the taxonomy has drifted, no one can explain how a theme was built, and the "insights" cannot be traced back to a single customer comment. The requirements that would have caught this were never in the document.


The nine requirements below are the ones that separate a platform you can stake a decision on from one that looks good in a demo. Thematic recommends writing each as a testable requirement with a pass or fail bar, not a capability you tick yes or no. The single most important one is the last: require a proof of value run on your own feedback data before you sign anything. A vendor's accuracy on their data tells you nothing about accuracy on yours.


Below is the full list, what to ask for under each, and how to turn the list into an RFP you can actually score.


## The 9 requirements at a glance


1. **Measurable accuracy on your own data.** Precision, recall, and F1 on a benchmark run with your feedback, not the vendor's.
2. **Themes that trace to the raw comments.** Every insight links back to the verbatims behind it.
3. **A taxonomy your team can edit and govern.** You refine themes without a consultant or a full retrain.
4. **One consistent set of themes across every source.** Surveys, tickets, reviews, and calls share the same taxonomy.
5. **Themes linked to the metrics you already report.** The platform shows which themes move NPS or CSAT.
6. **Time to insight measured in days, not quarters.** No long training period before you get value.
7. **Connectors and an open API for your existing stack.** It fits your tools instead of replacing them.
8. **Full data portability and a clean exit.** You can export everything and leave without penalty.
9. **Enterprise security, compliance, and AI governance.** SOC 2, GDPR, data residency, SSO, and a documented AI governance posture.


## 1. Accuracy you can measure on your own data


Require the vendor to run an accuracy benchmark on a sample of your feedback and report precision, recall, and F1, not a single marketing accuracy number. Precision is how often the themes the platform assigns are correct. Recall is how much of the relevant feedback it catches. F1 is the harmonic mean of the two, and it is the better measure for customer feedback because feedback data is class-imbalanced: a few themes are huge and most are small, which is exactly the case where raw accuracy misleads, per[Google's machine learning documentation](https://developers.google.com/machine-learning/crash-course/classification/accuracy-precision-recall) .


Set a realistic bar. Human analysts agree with each other on sentiment only about 80 to 85 percent of the time, according to[text analytics firm Lexalytics](https://www.lexalytics.com/blog/sentiment-accuracy-baseline-testing/) , so that range is the practical ceiling for any automated system. A vendor promising 99 percent accuracy is either measuring something trivial or not measuring at all. Thematic's position is that a single accuracy percentage tells you little; the requirement is a benchmark on your data, with the metric broken out, against a stated human agreement baseline.


## 2. Themes that trace back to the raw comments


Require that every theme, score, and summary can be traced to the specific customer comments behind it. This is the difference between an insight a skeptical CFO can interrogate and a[black-box output](https://www.getthematic.com/insights/auditable-transparent-ai-feedback-analytics) no one can defend. When the platform tells you "delivery complaints rose 12 percent," you should be able to click through to the exact verbatims that make up that number.


This requirement is also where AI governance is heading. The[NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) names "explainable and interpretable" and "accountable and transparent" as core characteristics of trustworthy AI. The[EU AI Act, Article 13](https://artificialintelligenceact.eu/article/13/) , requires that high-risk AI systems be transparent enough for the people using them to interpret the output correctly. Thematic is built so that every theme traces to comment-level evidence, with audit trails. Ask specifically about summaries generated by large language models: require a citation back to source comments for each claim, so a fluent-sounding but invented insight cannot slip through.


## 3. A taxonomy your team can edit and govern


Require that your own team can edit themes without opening a support ticket, hiring a consultant, or triggering a full model retrain. The theme structure is the part of the system your analysts live in, and it will need to change as your business changes. If every change routes through the vendor's professional services team, you do not own your taxonomy; you rent it.


Ask what happens when a theme is wrong. A strong platform lets an analyst merge, split, rename, or[refine a theme](https://www.getthematic.com/insights/customize-ai-themes-customer-feedback) through a no-code editor and keeps a human in the loop, so the AI learns from the correction while the analyst stays in control. Thematic provides a no-code theme editor for exactly this. The demo-time test: hand the vendor a theme you know is miscategorized in their sample and ask them to fix it live, in front of you, without engineering help.


## 4. One consistent set of themes across every source


Require that surveys, support tickets, app reviews, call summaries, and social posts all resolve to the same taxonomy, so "billing problem" means the same thing in a support ticket that it means in an NPS verbatim. Most tools can[ingest multiple sources](https://www.getthematic.com/insights/integrate-feedback-sources-analytics-platform) . The hard part, and the requirement worth testing, is whether those sources are aligned to one consistent set of themes or simply dumped into the same dashboard under different labels.


The evidence to demand is a live example of cross-source consistency. A major enterprise-software company funneled surveys, support chats, community posts, and social into one Thematic taxonomy through its API, so feedback from every channel was deduplicated and aligned to the same themes before it reached product teams; it chose Thematic after more than a year of evaluating tools. Scale matters here too: a three-person insights team at a national home-improvement retailer analyzed more than 20,000 comments a month from 84 stores under one taxonomy. Ask the vendor to show two different sources resolving to the same theme.


## 5. Themes linked to the metrics you already report


Require the platform to connect themes to the outcome metrics your leadership already tracks, such as net promoter score (NPS), customer satisfaction score (CSAT), or customer effort score (CES), and to show[which themes actually move the score](https://www.getthematic.com/insights/link-feedback-themes-nps-csat-drivers) . Counting how often a topic appears tells you what is loud. Linking a theme to its impact on the score tells you what matters, which is a different and more useful thing.


This is the capability that turns a feedback tool into a budget-defense tool. Vodafone New Zealand used Thematic to analyze tNPS survey verbatims, saved 60 hours every month on the analysis, and posted a double-digit tNPS increase over nine months after acting on what the themes surfaced. Require the vendor to demonstrate the link from a specific theme to a specific movement in your metric, not just a word cloud sized by frequency.


## 6. Time to insight measured in days, not quarters


Require a clear answer on time to first insight, and be skeptical of any platform that needs months of taxonomy building or model training before it produces anything. Legacy text analytics often carried a long, consultant-led setup. Modern platforms discover themes from the feedback itself and produce usable output in days.


The bar is concrete. A US not-for-profit health system used Thematic to deliver themed reports from open-text employee engagement feedback to all 250 of its departments in a single three-day sprint, saving more than 160 hours, roughly 10,000 dollars, per reporting cycle. Ask the vendor how long until you see themes on your own data, and require the proof-of-value timeline to match the answer.


## 7. Connectors and an open API for your existing stack


Require that the platform connects to the tools you already run rather than asking you to replace them. Most enterprise CX teams are not looking to rip out[Qualtrics](https://www.getthematic.com/insights/use-qualtrics-with-thematic-and-power-bi) , Medallia, or their data warehouse. They want a better analytics layer on top. So the requirement is native connectors to your feedback sources and BI tools, plus an open API for anything custom.


A New Zealand vehicle-glass repair company with more than 60 locations collected NPS feedback in Qualtrics, used Thematic to discover themes and tie them to NPS impact, and pushed the results into Power BI, so the whole business worked from one view without changing how feedback was collected. Ask for the connector list in writing, and ask specifically whether the platform reads from and writes to your warehouse, such as Snowflake, BigQuery, or Databricks.


## 8. Full data portability and a clean exit


Require a documented exit before you sign the entry. Ask exactly how you get your data out, in what format, on what timeline, at what cost, and how deletion is certified. This is the requirement buyers most often skip and most often regret, because vendor lock-in is quiet until the day you want to leave.


A defensible acceptance bar: you can export all raw feedback, themes, and metadata in a documented open format, within a defined window, with no egress fees, and receive certified deletion of your data on termination. You own your data and your taxonomy, not the vendor. If a vendor cannot describe its export path plainly, treat that as a finding, not a footnote.


## 9. Enterprise security, compliance, and AI governance


Require the security and governance evidence up front, scored, not deferred to a legal review after selection. At an enterprise, this requirement usually decides whether a deal can close at all, so it belongs in the RFP, not the redlines.


Ask for the concrete artifacts: SOC 2 Type II, General Data Protection Regulation (GDPR) compliance, data residency options, single sign-on (SSO), role-based access control (RBAC), audit logging, and data retention and deletion controls. For the AI itself, ask how the vendor maps to recognized frameworks. The NIST AI Risk Management Framework and its Govern, Map, Measure, and Manage functions, and[ISO/IEC 42001](https://www.iso.org/standard/42001) , the certifiable international standard for AI management systems, are the two most useful reference points. A vendor that can speak to these has thought about governance; a vendor that cannot has not.


## How to turn these nine into an RFP you can score


A list of requirements only works if you score it consistently. Split the nine into must-have and nice-to-have for your situation, assign each a weight that sums to 100, and define[what evidence clears each requirement](https://www.getthematic.com/resource/text-analytics-rfp-guide) so scoring is about proof, not impression.


Requirement Priority Evidence that clears it


Measurable accuracy on your data Must-have Precision, recall, and F1 from a benchmark on your feedback


Themes trace to raw comments Must-have Click-through from any number to its source verbatims


Editable, governed taxonomy Must-have A theme fixed live, in the demo, with no engineering help


One taxonomy across all sources Must-have Two different sources resolving to the same theme


Themes linked to your metrics Must-have A theme shown driving a specific NPS or CSAT movement


Time to insight in days Nice-to-have A stated timeline to themes on your data, honored in the pilot


Connectors and open API Must-have Written connector list plus warehouse read and write


Data portability and exit Must-have Documented export format, timeline, cost, and deletion terms


Security and AI governance Must-have SOC 2, GDPR, SSO, and a mapped NIST or ISO 42001 posture


Then add the clause that makes the whole document real: a mandatory proof of value on your own data before signature. Require each shortlisted vendor to run the accuracy benchmark, the cross-source consistency test, and the live theme edit on a sample of your feedback, scored against the same bar. A platform that resists testing on your data is telling you something.


## The short version


Require nine things in a customer feedback analytics RFP: measurable accuracy on your own data, themes that trace to the raw comments, an editable and governed taxonomy, one consistent set of themes across every source, themes linked to your reported metrics, fast time to insight, connectors and an open API, full data portability, and enterprise security with AI governance. Score them as weighted must-haves with defined evidence, not a yes or no checklist. Then insist on a proof of value on your own feedback before you sign. The vendor that welcomes that test is usually the one worth choosing.
