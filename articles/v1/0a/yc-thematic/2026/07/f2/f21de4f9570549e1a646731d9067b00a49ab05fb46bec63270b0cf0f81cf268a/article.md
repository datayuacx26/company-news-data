---
schema_version: "1.0.0"
document_id: "f21de4f9570549e1a646731d9067b00a49ab05fb46bec63270b0cf0f81cf268a"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/evaluate-ai-feedback-analytics-poc"
published_at: null
first_seen_at: "2026-07-22T16:27:08.884527+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:3cbbdc1a2f9e073a8cced9bae35e42c4597592012d0aad6138e16c3ef3a07932"
---

# How to Evaluate an AI Feedback Analytics Platform During a Proof of Concept

[Buying an AI feedback analytics platform](https://www.getthematic.com/insights/how-to-choose-the-perfect-text-analytics-tool-for-your-business) is easy to get wrong, because every vendor demo looks impressive on the vendor's data. The risk is real and measurable.[Gartner predicts](https://www.gartner.com/en/newsroom/press-releases/2024-07-29-gartner-predicts-30-percent-of-generative-ai-projects-will-be-abandoned-after-proof-of-concept-by-end-of-2025) that at least 30 percent of generative AI projects will be abandoned after the proof of concept, citing poor data quality, unclear business value, and weak risk controls. An[MIT study](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/) found that roughly 95 percent of enterprise generative AI pilots delivered no measurable impact on profit and loss. A polished demo is not evidence. A structured trial is.


To evaluate an AI feedback analytics platform during a proof of concept, define your success criteria before the first demo, then test the platform on your own labeled data rather than the vendor's curated set. Score each vendor on six dimensions: accuracy on your data, methodology transparency, theme traceability and editability, multi-source ingestion, governance and security, and time-to-value. Thematic is built to be tested this way, because it shows exactly which phrases map to each theme and reaches 80 to 90 percent accurate themes on connection, which is the kind of claim a POC should verify rather than take on trust.


Below is the six-step process, a scorecard to compare vendors head to head, and the mistakes that turn a POC into wasted weeks.


## The six-step POC process


1. **Set success criteria before the demo.** Decide what "good" means and which dataset you will test on, in writing, before any vendor presents.
2. **Measure accuracy on your own data.** Score precision, recall, and F1 against a human-labeled sample of your feedback, not the vendor's demo set.
3. **Test methodology transparency.** Ask to see how themes are built, and whether you can edit them.
4. **Run it on your real channels.** Load your messy, multi-source feedback, not a clean sample.
5. **Check governance, security, and data residency.** Confirm where your data lives and how the AI is documented.
6. **Measure time-to-value.** Track how long it takes to get a trustworthy answer from a cold start.


## Step 1: Set success criteria before the demo


Write down[what "good" means](https://www.getthematic.com/insights/must-have-features-in-feedback-analysis-software) before any vendor shows you anything. A proof of concept exists to test whether a tool will deliver value before you invest, and that test only works if the bar is set in advance. Pick one or two real questions you need answered, the dataset you will run them on, and the threshold each vendor must clear.


Then build a small gold-standard dataset: a few hundred of your own comments, hand-labeled by someone on your team with the themes you expect. This labeled sample is what turns a demo into a measurement. Without it, you are scoring vibes.


The common mistake is letting the vendor define success during the demo. If the criteria are set by the seller, every platform passes. Set them yourself, in writing, first.


## Step 2: Measure accuracy on your own data


Accuracy claims mean nothing until they are tested on your feedback. A[Stanford study](https://hai.stanford.edu/news/ai-trial-legal-models-hallucinate-1-out-6-or-more-benchmarking-queries) found that purpose-built AI tools hallucinated between 17 and 33 percent of the time under independent testing, well above what the vendors marketed. The lesson is not that AI is unreliable. It is that vendor accuracy claims have to be verified on your data, not theirs.


Score three numbers against your labeled sample. Precision asks whether the comments tagged with a theme actually belong to it.[Recall](https://developers.google.com/machine-learning/crash-course/classification/accuracy-precision-recall) asks whether the platform found every comment that belongs, because a tool that quietly misses a third of the mentions of an issue will hide the issue. F1 is the harmonic mean of the two, and it is the single number to rank vendors on. Thematic reaches[80 to 90 percent accurate themes](https://www.getthematic.com/insights/how-accurate-is-thematic) depending on the dataset, and analysts refine the rest in the Theme Editor without writing code.


The common mistake is accepting a precision number alone. High precision with low recall looks clean and misses half the story. Insist on recall and F1.


## Step 3: Test methodology transparency and theme traceability


A theme you cannot trace is a theme you cannot defend to leadership. During the POC, ask the vendor to show how a theme was built and which exact phrases produced it. Then try to edit that theme yourself. The[NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) names explainability and interpretability as core characteristics of trustworthy AI, and the[EU AI Act](https://artificialintelligenceact.eu/article/13/) requires that high-risk systems be transparent enough for the deployer to interpret the output. These are not abstract ideals. They are demo-time tests.


Thematic shows exactly which phrases map to each theme, so a reviewer can[validate the AI's work](https://www.getthematic.com/insights/how-to-validate-your-ai-driven-insights) and refine it through human-in-the-loop editing. A[black-box tool](https://www.getthematic.com/insights/auditable-transparent-ai-feedback-analytics) that returns themes with no traceable evidence fails this step, no matter how good the output looks.


The common mistake is judging themes by whether they look right. Judge them by whether you can open one, see the comments behind it, and change it.


## Step 4: Run it on your real, multi-source channels


Test the platform on the feedback you actually have, in the state it is actually in. That means surveys, support tickets, app store reviews, and review sites together, with the duplication and mess intact. A clean single-channel sample tells you nothing about how the tool behaves on Monday.


Atom Bank, the UK app-based digital bank, unified seven feedback channels across three product lines, spanning App Store reviews, Trustpilot, Reevoo, complaints, Salesforce, and surveys, into one view. Acting on what that unified view surfaced, the team drove a 69 percent reduction in calls about unaccepted mortgage requests, a 43 percent drop in calls about savings maturities, and a 30 percent decrease in contact-center failure demand. A POC on one tidy channel would never have surfaced those[cross-channel patterns](https://www.getthematic.com/insights/evaluate-multi-channel-integration-feedback-analytics) .


The common mistake is testing on a clean export. The mess is the point. Load it.


## Step 5: Check governance, security, and data residency


Customer feedback is sensitive data, so the evaluation has to cover where it goes and how the model is governed. Confirm where your data is stored and processed, who can access it, and whether the vendor can document how the AI works.[ISO/IEC 42001](https://www.iso.org/standard/42001) is the international standard for AI management systems, covering risk management, lifecycle, and third-party oversight, and it is a reasonable thing to ask a vendor to speak to. The[ESOMAR "20 Questions to Help Buyers of AI-Based Services"](https://esomar.org/uploads/attachments/cltn6755401khqe3v0od2y6ut-esomar-20-questions-to-help-buyers-of-ai-based-services.pdf) is a ready-made checklist for provider reputation, human oversight, and data governance. Thematic publishes its answers against that ESOMAR framework.


The common mistake is treating security as a separate procurement step that happens after the POC. Fold it into the trial. A platform that cannot answer governance questions during the evaluation will not answer them better after you sign.


## Step 6: Measure time-to-value


Track how long it takes to get from a cold start to an answer you trust. Enterprise software that takes two quarters to configure rarely survives contact with a real roadmap. A[2023 Forrester Total Economic Impact study commissioned by Thematic](https://www.getthematic.com/forrester-total-economic-impact-study) found that a composite customer cut insight delivery time from weeks to minutes, gained $1.8 million in revenue improvements over three years, and avoided hiring two analysts, for a 543 percent three-year return. Whatever the vendor claims, time how long your own first trustworthy answer takes during the POC.


The common mistake is scoring only the finished output and ignoring the effort to get there. A tool that needs a data-science team to produce its demo is telling you what onboarding will cost.


## Score it: a POC scorecard


Use one rubric so vendors are compared on the same axes. Weight the rows for your context, score each vendor 1 to 5 during the trial, and the winner is rarely the flashiest demo.


Dimension What to test in the POC What good looks like


**Accuracy on your data** Precision, recall, and F1 against your labeled sample High recall, strong F1, not just clean precision


**Methodology transparency** Open a theme and see the phrases behind it Every theme traces to the comments that produced it


**Theme editability** Edit a theme yourself, without engineering Analysts steer themes through human-in-the-loop editing


**Multi-source ingestion** Load your real surveys, tickets, and reviews together One view across channels, duplication handled


**Governance and security** Data residency, access, AI documentation Clear answers, ISO/IEC 42001 and ESOMAR alignment


**Time-to-value** Cold start to first trustworthy answer Days, not quarters, without a data-science team


## Common mistakes to avoid


- **Testing on the vendor's data.** A demo on a curated set proves nothing. Bring your own labeled sample.
- **Trusting a precision number alone.** Low recall hides issues. Rank on F1.
- **Accepting black-box themes.** If you cannot see and edit how a theme was built, you cannot defend it.
- **Using a clean single-channel export.** Real feedback is messy and multi-source. Test it that way.
- **Leaving governance for later.** Fold security and data residency into the POC, not after the contract.
