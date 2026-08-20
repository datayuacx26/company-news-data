---
schema_version: "1.0.0"
document_id: "d89511fe1d28d3a342b905123ba3723407d0dea71ad773ff67d8a9a3cd0308e1"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/goalert-now-available-in-open-source"
published_at: "2019-06-11T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:99f53bcfa08303be0513dd159a53efc6068d6583f184ba967e4fe45d309fc94b"
---

# GoAlert - Now Available in Open Source

Open Source GoAlert Is Now Available At


[https://github.com/target/goalert](https://github.com/target/goalert)


At Target, we've standardized on GoAlert as the recommended on-call scheduling and notification product. In February, we


[introduced GoAlert](https://tech.target.com/2019/02/25/introducing-goalert.html) and how the opportunity to create it materialized. Through our investment in this system, we've realized the following benefits:


Reduction In Licensing and Maintenance Costs


Before GoAlert, Target paid for multiple on-call scheduling products. We had a tiered model where teams that required a full-featured, on-call scheduling and notification product used a robust, high-priced, commercial product. Other teams used another less-expensive commercial product with limited features. When we looked at the return on investment of continuing to use commercial products versus leveraging an existing open source product or building our own, the choice was clear. We built GoAlert.


Simplified Engagement Process


Reducing from many on-call products to one streamlined and unified our on-call engagement process. This simplification was possible because GoAlert scales well in both cost and stability aspects. Since GoAlert is easy to use, adoption took off.


Increased Engineer Engagement


Many engineers at Target care deeply about the success of the open source community. They are inspired by each other's open source contributions and want to work on teams that give back to open source. From the beginning, we built GoAlert to be an open source product. If that final product was valuable and feedback positive, we wanted the option to contribute the product to the open source community. As a result, it was relatively easy to recruit highly-skilled engineers to join the team.


GoAlert - Now An Open Source Project


You, too, can now benefit from using GoAlert as we've released it to the open source community via the permissive Apache 2.0 license. We think that GoAlert users will enjoy it because:


- On-call schedule management is extremely easy and flexible. You can define a rotation of people, shift duration, and handover time.


- Overrides allow you to make manual schedule adjustments.


- Escalation Policies ensure that someone on your team is notified of every alert.


- Every user can define their own contact methods and decide if they want an SMS or voice call notifications, or both.


- Users can acknowledge and close alerts from their mobile device via a simple, one-character SMS reply.


- The user interface looks great on your mobile device.


We think that administrators will love GoAlert because it’s easy to run and maintain. The application runs in a single Docker container connected to a PostgreSQL database.


Production Use


Internally, we run the same version of GoAlert in production that we are open sourcing today. This means that GoAlert has already been used in production by 3,500+ customers and, since the initial production deployment more than two years ago, it has successfully processed well more than 1.5 million alerts.


How to Get Started


To get started with GoAlert, check out the README at


[https://github.com/target/goalert](https://github.com/target/goalert) . In just a few minutes, you can have GoAlert up and running for yourself.


Target's Open Source Contributions


Target believes in contributing to open source for a number of reasons. A few reasons include:


- Target uses a ton of open source technologies and gains a lot of value from the community. We want to contribute back to open source to help ensure community health.


- Many of Target's engineers want to contribute to open source projects. These engineers often want to help the community, demonstrate their passions and skills publicly, and desire feedback from the community. Since these engineers can contribute to open source as part of their normal work, it helps keep these engineers engaged and happy, which helps with retention.


- In the IT Industry, there are so many interesting problems that need to be solved. Working publicly with the community is an efficient way to solve problems. We don't want to solve a problem that someone else already addressed.


One of Target's Most Used Operations Products Open Sourced


Target has made numerous


[open source contributions](https://opensource.target.com/) to libraries and has created a few products itself. GoAlert is a flagship Infrastructure & Operations product for Target. It's a full featured, end-to-end product that we believe can meet the needs of many different people and teams.


Additional Information


- Visit


[GoAlert.me](https://goalert.me/)
- Slack:


[#goalert](https://gophers.slack.com/messages/CJQGZPYLV/) on


[Gopher Slack Community](https://gophersinvite.herokuapp.com/)
- Email:


Support@GoAlert.me


## RELATED POSTS


### GoAlert - Your Future Open Source, On-Call Notification Product


By Adam Westman, February 25, 2019


A few years ago, Target started a journey to move into a product-based organization with dedicated, durable, full-stack teams.
