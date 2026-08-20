---
schema_version: "1.0.0"
document_id: "a9db9399773cc83803f7689a22085b14f1925eed4c1c9c21f381494c9dcdcbea"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/banking-and-capital-markets/impact-new-iso-20022-messaging-standard-payments-infrastructure"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:24038a81f7558cf8cf16deac76cc7db2b26640cde8648fb32ec1b1d04eeb0ec2"
---

# Impact of the new ISO 20022 messaging standard on payments infrastructure

The payments industry is constantly evolving and has been for decades. Where are we today and why? Let’s take a closer look in light of recent changes—namely, the adoption of the ISO 20022 payment messaging standard.


### From Swift payment messages to the emergence of ISO 20022


First, a brief review of the early days of payments. For this discussion, I’m fast-forwarding the history of payments to the beginning of digital payments, jumping over the period when the telex network was used to send electronic messages between businesses in the post-World War II period.


In 1973, the banking community agreed to digitize this telex infrastructure. It created the organization called Swift, which many people are familiar with, especially in the banking world.


The Swift network transmitted the first electronic message in 1977, using the then newly developed MT message standard. This standard mimicked the standard used to punch holes in the paper that transmitted data when using a telex machine.


You could argue that a lot has happened since 1977. We, for example, moved from X.25 to IP communication in the beginning of the millennium. At that time, we also introduced a new message standard dubbed MX, with X denoting XML as the primary structure for the message.


Despite this, we transport the vast bulk of all payments today using the very old MT standard. However, the banking community has now decided to actually replace the MT standard—not just add a new standard as in the past. The new standard chosen to replace MT is the MX standard, which uses ISO 20022 as its structure for core messaging.


It’s not only Swift payment messages that are about to be transformed to ISO 20022. Many local payment infrastructures around the globe also are adopting the ISO 20022 messaging standard. We definitely see this in the rapidly growing instant payment infrastructures worldwide. In addition, traditional bulk payments infrastructures are adopting this standard.


### ISO 20022 adoption challenges and options


So, all of a sudden, we have this new format in many (if not all) key payment infrastructures that banks are using. This has a number of advantages, but also a number of challenges that banks need to address internally.


Let’s take a closer look at these challenges and why it’s important to make a strategic long-term decision to address them.


### Back-office challenges and options


If we take a step back and look at the typical IT landscape within most banks, we see there are a number of back-office systems that send and/or receive payments. This is because a payment is involved with most bank products. The impact of the ISO 20022 messaging format might be that these internal systems, which are typically old, simply cannot handle the extended information that comes with ISO 20022, which is one of the main drivers behind its adoption. One other aspect is the pure size of the message, but we will come back to that a bit later in this post.


So, how do you address these challenges from a solution point of view? The first necessary step is an assessment of your internal systems to determine the impact the introduction of ISO 20022 will have on them. Following that, there are typically three main categories of possible solutions to consider:


1. Put a “black-box” in front of your back-office systems and shield them from the impact of ISO 20022.
2. Update your back-office systems so that they can take advantage of the new possibilities that ISO 20022 brings.
3. Buy new back-office systems that already have built-in support for ISO 20022.


It might be tempting to go with the first “black box” option. However, this option poses serious risks. The main risk is information loss, which, in turn, might negatively affect bank products and, in turn, customer relationships. These risks increase over time because competitors can access and leverage the lost information in their product offerings. In addition, eventually you will need to adopt the new standard anyway.


The second “update your systems” alternative will likely be the one that most banks choose. The advantage of this option is that you don’t have to replace all of your back-office systems at once.


The third “replace your systems” alternative most likely will be used by banks that have systems—or processes—that already have a number of deficiencies, and these deficiencies will be only more apparent over time.


### Messaging platform challenges and options


Let’s now move away from the back office and consider implications on other areas of the bank.


The next major impacted area of the bank is the bank’s messaging platform, which communicates with all of its internal systems, as well as with the outside world. Due to the historical success of Swift, external networks mostly have been Swift-based. As a result, in essence, there has been primarily only one external network used.


In recent years, however, we see more and more alternatives to Swift emerging, as well as the use of complementary networks. In addition, many central market infrastructures rely on preferred networks that are “non-Swift.”


Regardless of the external network used, ISO 20022 imposes new requirements on the messaging infrastructure of banks. Therefore, you need to be able to route based on price, speed, cost, resiliency and reachability by using multiple networks.


In addition to multi-network routing requirements, we also have the impact of the size of ISO 20022 messages on the messaging platform. As mentioned earlier, an ISO 20022 message is larger than the traditional MT message that most banks’ messaging platforms manage. Banks need to consider whether their existing messaging platforms can effectively route the much larger ISO 20022 messages. They also need to consider the costs and risks imposed.


### Conclusion


It’s clear that ISO 20022 will have a major impact on current bank infrastructures. We also know from the results of our[2020 CGI Client Global Insights](https://www.cgi.com/en/client-global-insights) that bank executives are concerned about the condition of their operational platforms and legacy infrastructures, especially as they continue to digitize and move to real-time payments. This concern is actually a top trend for 2020 cited by executives in both the retail banking and corporate and transaction banking sectors.


Back-office systems and messaging platforms have a tendency to live for a long time in most banks, so it’s important to consider the implications of ISO 20022 from a strategic long-term point of view. What changes should you make today to not only accommodate ISO 20022 requirements but strengthen your business for the future?


CGI has been at the forefront of payments for four decades, working on major payment evolutions like the establishment of Swift and, more recently, the development instant payment infrastructures.Contact me for a discussion on this topic or any other payments-related issue your organization is seeking to resolve.
