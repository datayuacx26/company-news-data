---
schema_version: "1.0.0"
document_id: "04fc4b3ae38163a1469143a42c31a761919acfe13f4126939d14087aff17f9ca"
company_key: "yc-subsets"
company: "Subsets"
source_id: "yc-subsets-news-import-30668a98a978"
canonical_url: "https://www.subsets.com/blog/cancellation-flow-experiments-pause-discount-downgrade"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T03:28:33.070528+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:3791520f37fab2d5995f69a469f7e809291ab8e9c1f331425c3df7f636e95c00"
---

# Cancellation-flow experiments: pause, discount, & downgrades

A cancellation offer only tells you something useful when it is matched to the reason a subscriber is leaving. A pause, discount, and downgrade can all reduce cancellations, but they do not solve the same problem.


A subscriber clicks cancel, sees an offer someone designed eighteen months ago, and the team calls it done. The save rate looks fine on a dashboard, so nobody asks the hard question: "fine compared to what?"


That's the gap in subscriber lifecycle that most businesses don’t address. Teams optimize the cancel flow for saves, but not retained revenue. The two aren't the same thing, and the difference shows up three months later when the "saved" subscribers churn anyway.


The focus of lifecycle teams should be on whether the cancellation offer matched the signal of the subscribers and whether that match resulted in retained revenue after the cancellation moment passed.


## The three offers


Pause, discount, and downgrade get treated as interchangeable levers in the same save screen. However, they're not similar at all. Each one answers a different reason someone is leaving, and testing them against each other only makes sense once you're honest about what each is built to solve.


1. **Pause** : fits a subscriber whose life changed and their opinion of the product is unchanged. A pause paired with a check-in message partway through is ideal for this case as it re-anchors the subscriber in what they were getting.[Pause flow-specific mechanics](https://www.subsets.com/blog/pause-flows-to-turn-cancellation-intent-into-retained-subscribers) include re-entry timing, expiration check-in, etc. as well.
2. **Discount** : fits price objections, and only price objections. If someone cancels because the product stopped being useful, 20% off doesn't fix that. It just delays the same cancellation by a billing cycle or two, at a lower price the whole time.
3. **Downgrade** : fits subscribers paying for more than they use. Oversized plan, unused seats, a tier they upgraded into during a promo and never needed. Downgrade keeps the relationship at a price that matches actual usage instead of forcing an all-or-nothing choice between full price and gone.


The mistake is offering all three as a menu and letting the subscriber self-select, or worse, showing the same single offer to everyone regardless of why they're leaving. Match the offer to the reason, or the offer does nothing but slow the dashboard down.


## Starting with the signal


Before testing pause, discount, or downgrade, the more useful question to answer is which subscribers should see which offer based on what is already known about them.


The signals usually include engagement trend, tenure, lifecycle stage, billing history, price point, plan-to-usage fit, and the stated or inferred cancellation reason from the cancel-flow survey.


When mapped against those signals, next steps become clear. The decision tree matters more than any single offer. Testing pause, discount, and downgrade against one broad cancellation audience pools together subscribers the offers were never designed for.


## Save rate is the first read


A save-screen click shows whether a subscriber accepted an offer. But it does not show whether they would have stayed anyway, whether they used the product afterward, or whether they were still active after the next billing cycle.


> The stronger question is which offer creates incremental retained revenue for the subscriber condition it was meant to address.


That requires a control group. Some subscribers see the matched offer, a comparable group does not, and both groups are tracked on retention and revenue at 30, 60, and 90 days. For this to be implemented, lifecycle growth and retention teams require the capabilities to[segment audiences](https://www.subsets.com/predictive-audiences) based on their behavior, create journeys for different segments identified, and launch the relevant experiments.


[Daily Mail](https://www.subsets.com/customer-stories/how-dailymail-boosted-30-day-engagement-by-14-percent-with-subsets) ran concurrent lifecycle experiments across trial conversion, activation, re-engagement, renewal, and pricing, with automated tracking of retention, engagement, and revenue. The publishing news giant has experienced results that include a 14% engagement lift and a 32% lift in customer lifetime value from price testing.


[Matas](https://www.subsets.com/customer-stories/how-matas-increased-retention-by-29-with-subsets) tested a predictive retention flow for dormant, high-risk subscribers showing moderate activity in the 60 days before going inactive. Against a control group over 30 days, the treatment group showed a 29%-point higher retention rate, 39% higher web engagement, and a 20% increase in orders placed. Matas has since launched 10 predictive retention flows built on this approach.


Both examples point to the same operating principle: the lift comes from matching the offer or flow to a defined subscriber condition and measuring it against a control. The offer alone is not the full experiment. The audience, signal, timing, and downstream measurement decide whether it should scale.


## **What to measure**


A cancellation-flow experiment should measure what happens after the subscriber accepts or rejects the offer. Useful metrics include:


- Save-screen acceptance rate, by offer and signal segment
- 30, 60, and 90-day retention versus control
- Retained revenue per subscriber entering the cancel flow
- Product usage after accepting the offer
- Repeat cancellation behavior
- [Upgrade/downgrade rate](https://www.subsets.com/glossary/metrics/upgrade-downgrade-rate)


These metrics separate a short-term save from a subscriber relationship that actually holds.


## **Key takeaway**


Pause, discount, and downgrade are responses to different subscriber signals. The experiment that matters is not simply which offer wins. It is which offer is right for this subscriber, and whether that offer holds up over 90 days against a control.


[Book a demo](https://www.subsets.com/demo) to see how Subsets helps teams route cancellation-flow offers by signal and measure what improves retained revenue in the long term.
