---
schema_version: "1.0.0"
document_id: "35b97bb4b67423a2a2221c3a314fc82407d8db1b7262e490b27d0a87b61a0c94"
company_key: "yc-fogbender"
company: "Fogbender"
source_id: "yc-fogbender-news-import-46cf0bf99c36"
canonical_url: "https://fogbender.com/blog/fogbender-msteams-integration"
published_at: "2022-09-05T00:00:00+00:00"
first_seen_at: "2026-07-21T20:41:04.212025+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:42190cf9bdd6d0d21aa59a6e0c6245aaa778315570ef8311b21e929d9dbce3a1"
---

# Introducing the Fogbender Customer-side Microsoft Teams Integration

What are the benefits of company-wide team messaging channels over, say, email?


In short - access. Any current or future employee has access to the information in a company-wide messaging channel. For email, this is only true for those individuals or groups explicitly listed as recipients of a message \[0\]\[1\].


While there are countless scenarios where the “need to know” restriction to information is beneficial to running complex organizations, one area where it’s pretty much never the case is customer support.


A company C that signs a multi-year deal with a cloud computing vendor V has little reason to shield support conversations with V’s agents from its employees, and every insentive to do the opposite: to ensure smooth knowledge transfer between departing and incoming colleagues and to facilitate knowledge sharing in general.


This is exactly why company C might invite vendor V’s agents as guests to a dedicated Microsoft Teams channel and require - sometimes contractually - that this channel serves as the default information condiuit between company C and vendor V.


Leaving aesthetic considerations aside (vendor V might be team Slack, etc), the main issue for vendor V in this scenario is that Microsoft Teams is not a customer support platform, making standard customer support things - like assigning conversations to specific agents, associating conversations to tickets in Jira, sharing internal notes, receiving dropped ball notifications, and the like - are impossible.


Fogbender - our product - *is* such a customer support platform, but, until today, even if vendor V offered support through Fogbender to its customers, a request by a customer to use Microsoft Teams instead would have negated all the benefits afforded by Fogbender to the vendor.


Today, we’re announcing a Fogbender-Microsoft Teams integration that bridges the gap.


\[0\] - If it appears difficult to keep track of threads in Microsoft Teams, Slack, Facebook, or any other threaded messaging system, consider the threading model of email: it’s a graph that with a single limitation on expansion: can’t go back in time - anything else is fair game.


\[1\] - Some companies take the group idea seriously. For some pre-Slack email wisdom from Stripe check out[https://stripe.com/blog/scaling-email-transparency](https://stripe.com/blog/scaling-email-transparency)
