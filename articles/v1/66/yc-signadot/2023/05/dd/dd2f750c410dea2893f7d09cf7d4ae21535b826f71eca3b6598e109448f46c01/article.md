---
schema_version: "1.0.0"
document_id: "dd2f750c410dea2893f7d09cf7d4ae21535b826f71eca3b6598e109448f46c01"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/is-serverless-for-everyone/"
published_at: "2023-05-16T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:63f3bf2fe18449b945b6b5e432d1cee3d3e768c9b3c38ef01e8643b8f35b6862"
---

# Is Serverless *Really* For Everyone?

*A snake for the ‘scaling’ problems Serverless is supposed to resolve*


So, the backlash has come. A few years ago, when you said the word ‘monolith,’ the next most likely phrase was ‘migrating to microservices.’ Now some teams are talking about how moving *away* from \[microservices to a more monlithic approach to a codebase has some advantages:


After this[Prime video](https://www.primevideotech.com/video-streaming/scaling-up-the-prime-video-audio-video-monitoring-service-and-reducing-costs-by-90) story was shared by Kelsey Hightower, product manager Max Andrews had some choice words about the usability of serverless functions.


What starts as a very broad statements that Serverless isn’t really good for large data apps, becomes quite specific: the issue is cold starts with large data load times. Both of these problems seem controllable: cold starts can be handled with[provisioned concurrency](https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html) , and a data-intensive app doesn’t necessarily mean loading a bunch of data at Lambda start time.


Hightower makes a great rebuttal:[Serverless is more than FaaS](https://twitter.com/kelseyhightower/status/1654244131210919936?t=0h-LdJkYwt67hzK6aAaUCA&s=19)


The clean scaling of Serverless doesn’t have to imply that you’re using serverless as your main compute platform, or that your serverless workload needs to scale to zero.


The very smart Marco Rogers makes a great point that while scaling was a superpower of serverless at the beginning, that may no longer be a huge[differentiatior:](https://twitter.com/polotek/status/1654244980624596992)


This forces us to confront the question: what is Serverless truly good for? An ability to scale up and down is hugely important but it’s not everything. While Amazon Prime may be celebrating their moving away from serverless, that smooth scaling sure would[have helped Netflix recently](https://twitter.com/Kotaku/status/1648087020311392260) .


However serverless is more than just scaling![(As pointed out by Kelsey Hightower)](https://twitter.com/kelseyhightower/status/1654246388908310528) .


The core concept of Serverless, that all your code runs in a single codified environment, is an incredible strength and has the potential to improve how your code runs. But that’s not everyone’s experience, and that standardization isn’t forward as one of Serverless’s key benefits.


So is serverless for everyone? Yes, in some application. Will all compute run serverlessly in 2025? No. But all teams *will* have a use for the technology. Further, Serverless is *much more* than AWS Lambda. Both Cloud Run and Vercel are showing real potential as new, very popular platforms for running code.


Serverless is a powerful technology that can provide a lot of benefits, but it may not be the best choice for every application. As the backlash against serverless grows, it’s important to remember that there are use cases where it’s the perfect fit. The key is to carefully evaluate your specific needs and requirements before making a decision.


## The elephant in the room: Dev and testing


I﻿n my own experience with serverless, the real problem was actually the ‘development loop’ or the time from writing code to trying it out. Waiting those crucial five minutes for a function to update killed developer velocity. While Signadot isn’t a tool aimed at serverless developers, we have built something that massively increases dev velocity by letting you share a single cluster for testing.[Try it out today!](https://www.signadot.com/)


‍
