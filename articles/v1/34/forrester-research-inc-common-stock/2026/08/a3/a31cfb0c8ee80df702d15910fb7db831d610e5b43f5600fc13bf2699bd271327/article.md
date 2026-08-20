---
schema_version: "1.0.0"
document_id: "a31cfb0c8ee80df702d15910fb7db831d610e5b43f5600fc13bf2699bd271327"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/rules-of-the-road-what-flocks-71-misread-rate-teaches-us-about-third-party-ai-risk/"
published_at: "2026-08-05T21:51:17+00:00"
first_seen_at: "2026-08-05T22:04:44.588872+00:00"
fetched_at: "2026-08-05T22:04:46.136083+00:00"
content_hash: "sha256:e72a507d5ddd76618ff7988239160d5455ea47ff97dc91bea23ea0896dee3228"
---

# Rules Of The Road: What Flock’s 71% Misread Rate Teaches Us About Third-Party AI Risk

Roseville, California just provided a case study in what happens when organizations confuse “AI-powered” with “AI-verified.” A Business Insider investigation found that Flock Safety’s AI license plate readers misread plates in[71% of the alerts](https://www.businessinsider.com/flock-camera-misread-license-plate-reader-california-roseville-police-2026-7) sent to Roseville police over two years, incorrectly flagging vehicles as stolen or linked to a felony. That number isn’t a rounding error or a software bug. It reflected a failure to validate, monitor, and govern AI performance in a real operating environment.


**Oversight that depends on willpower isn’t a control**


AI governance sets intent. AI risk management is execution. Roseville had a policy requiring officers to verify alerts before taking action. The issue was not the absence of a control. It was a failure to enforce it. A dispatch supervisor got so used to a known-bad match that she admitted, “It’s easier at this point we have it memorized,” instead of escalating it. A control on paper became a workaround in practice.


Another Flock[case in Minnesota](https://www.thedrive.com/news/inside-the-flock-dragnet-how-systemic-errors-led-to-police-ambushing-me-for-no-reason) highlights the consequences of these types of control failures. A family was stopped by multiple police vehicles after the system generated a faulty match. Different circumstances, same pattern: confidence in automation without verification. Law enforcement is not the only ones learning this lesson. Organizations that don’t verify third-party AI model performance in their environment realize the risks after it’s too late. And the stakes go beyond a false alarm.


**Vendor Accuracy Claims Rarely Survive Contact With Reality**


Flock reports that its cameras read license plates with 96% accuracy under optimal conditions. Roseville’s experience demonstrates the limitation of relying on that figure alone. Performance measured in controlled testing does not guarantee performance in production.


That gap between 96% accuracy and 71% inaccuracy matters because AI performance is contextual. Accuracy depends on the environment, data quality, operating conditions, and use case. A vendor’s benchmark reflects how a model performed in its testing environment, not your real-life environment. Organizations that treat those numbers as interchangeable create blind spots before deployment even begins.


AI outcomes are not portable across contexts. That’s why the[NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) separates “mapping risk from measuring performance.” Every deployment environment requires its own validation baseline.


## **Third-Party AI Risk Becomes Your Risk**


Replace a license plate with a resume, loan application, or healthcare claim and the pattern remains the same. The[Earnest Operations](https://www.mass.gov/news/ag-campbell-announces-25-million-settlement-with-student-loan-lender-for-unlawful-practices-through-ai-use-other-consumer-protection-violations) settlement over AI-driven lending decisions and the ongoing litigation involving[Workday’s](https://www.shrm.org/topics-tools/news/technology/workday-ai-lawsuit-wake-up-call-hr) AI hiring technology reflect a broader reality when organizations discover how AI behaves in production after consequences emerge.


This is the third-party AI risk many companies underestimate and that contracts don’t transfer away. Vendor claims may create confidence, but accountability remains with the organization deploying the technology. Without continuous validation and monitoring, a vendor’s error becomes your consequence.


## **What Risk Management Must Do Now**


The technology created the risk signal. The governance failure came from missing verification, escalation, and accountability practices. Two years of unmanaged errors were not a technology problem alone. They were a risk management problem. To avoid a similar outcome, risk pros must:


- **Validate vendor claims in production conditions.** Test AI against your own data, operating environment, and edge cases before scaling deployment.
- **Require human verification for high-consequence decisions.** The greater the potential impact, the stronger the validation requirements should be.
- **Formalize escalation requirements.** Define reportable AI errors, establish review thresholds, and audit whether teams are escalating issues or working around them (or worse, hiding them).


- **Expand third-party risk assessments** . Ask vendors whether their accuracy claims have been independently audited, require evidence, and treat a refusal or vague answer as a material risk finding.


- **Make error-reporting a contractual obligation.** Define what counts as a reportable error, set a threshold that triggers vendor and internal review, and audit whether your team escalates or quietly works around problems.
- **Track state AI verification mandates.** Human-in-the-loop requirements for high-stakes[AI are emerging state by state](https://www.forrester.com/report/navigate-the-patchwork-of-us-ai-regulations/RES180500) , and building the control now costs less than retrofitting it under a deadline.


The lesson from Rossville is simple: In third-party AI deployments, operating reality always wins over vendor promises. If you are a Forrester client,[schedule a guidance session](https://www.forrester.com/inquiry) to get tailored insights and guidance for your third-party AI risk management program.


Share


-
-
