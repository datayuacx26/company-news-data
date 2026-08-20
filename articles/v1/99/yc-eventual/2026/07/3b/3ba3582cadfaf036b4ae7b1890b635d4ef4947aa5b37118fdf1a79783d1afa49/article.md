---
schema_version: "1.0.0"
document_id: "3ba3582cadfaf036b4ae7b1890b635d4ef4947aa5b37118fdf1a79783d1afa49"
company_key: "yc-eventual"
company: "Eventual"
source_id: "yc-eventual-news-import-1ca85739ec11"
canonical_url: "https://www.eventual.ai/blog/what-if-data-alone-wont-solve-robotics"
published_at: "2026-07-29T08:00:00+00:00"
first_seen_at: "2026-07-29T10:21:32.744501+00:00"
fetched_at: "2026-07-29T10:21:34.349058+00:00"
content_hash: "sha256:f07584ca25e2feb807e7d5b6510b91a489482c9e33ec9869637ec45cd456a1cf"
---

# What if more data alone won't solve robotics? Ken Goldberg on why good old-fashioned engineering still matters

> "Large data solved vision, large data solved language, therefore large data will solve robotics. I believe that will be true. But my question is, when?"
>
>
> --[Ken Goldberg, MIT Robotics Seminar](https://www.youtube.com/watch?v=EtK5es0jVM4&t=317)


In an[earlier post](https://www.daft.ai/blog/the-data-wall-in-physical-ai) , we argued that the limiting factor in physical AI is data. However, Ken Goldberg thinks data alone won't be enough.


Goldberg is a UC Berkeley robotics professor who co-founded[Ambi Robotics](https://www.ambirobotics.com/) (over 100 million packages sorted in production) and created[Dex-Net](https://berkeleyautomation.github.io/dex-net/) , one of the foundational grasping systems in modern robotics. In a[talk at MIT](https://www.youtube.com/watch?v=EtK5es0jVM4) , he argued that good old-fashioned engineering - modularity, algorithms, metrics - is still valuable, even as the field moves toward end-to-end learning.


## The 100,000 year gap


Goldberg compares training data across domains in human-equivalent terms.[π0](https://www.pi.website/blog/pi0) is trained on roughly one year of human teleoperation time. Qwen is trained on the equivalent of about 100,000 years of human reading time.


And unlike language or vision, as Goldberg puts it,["nothing's on the internet"](https://www.youtube.com/watch?v=EtK5es0jVM4&t=381) for robotics - there are no examples available. Every trajectory requires a real robot in a real environment. The dimensionality is harder too - a robot with two arms and dexterous hands can have[50 to 100 degrees of freedom](https://www.youtube.com/watch?v=EtK5es0jVM4&t=377) .


## If data were enough, Tesla would be beating Waymo


Tesla has collected roughly[9 billion miles of driving data](https://www.youtube.com/watch?v=EtK5es0jVM4&t=463) - about 500 times more than Waymo. But Waymo's miles-to-disengagement metric is[far ahead](https://www.youtube.com/watch?v=EtK5es0jVM4&t=475) (at the time of the presentation).


As Goldberg puts it: "if data were the only ingredient," this gap shouldn't exist.


## Good old-fashioned engineering


So if data alone isn't enough, what else is needed? Goldberg's answer is what he calls good old-fashioned engineering: modularity, algorithms, and metrics. Modular systems let you swap out components as better ones come along. Algorithms give you something you can reason about and fix. Metrics let you measure progress.


[Waymo is a good example](https://www.youtube.com/watch?v=EtK5es0jVM4&t=536) . Its self-driving system is full of filters, procedural elements, and sensing modules - it's data plus good old-fashioned engineering.


## Where do we get the data?


Given the data gap we discussed earlier, Goldberg asks:["where are we going to get this data?"](https://www.youtube.com/watch?v=EtK5es0jVM4&t=2218) He surveys the main approaches:


**Simulation** works for[flying robots and locomotion](https://www.youtube.com/watch?v=EtK5es0jVM4&t=2235) - you can train in sim and transfer to real hardware. But for[grasping and manipulation, it does not transfer](https://www.youtube.com/watch?v=EtK5es0jVM4&t=2305) . Locomotion has one dominant force vector: gravity. Manipulation involves deformations, friction, and interpenetrations between bodies that cause force discontinuities.


**World models** don't solve it either - Goldberg shows examples where[video generation produces physically impossible grasps](https://www.youtube.com/watch?v=EtK5es0jVM4&t=2404) that look plausible at first glance.


**Human teleoperation** is[the way most people are going right now](https://www.youtube.com/watch?v=EtK5es0jVM4&t=2454) , and it's showing promise.


Goldberg's own approach is what he calls the data avalanche.


## The data avalanche: ship first, learn second


So if you can't collect enough data before building, and simulation won't give it to you, where does the data come from? Goldberg's answer is what he calls the[data avalanche](https://www.youtube.com/watch?v=EtK5es0jVM4&t=2752) : build a working system using engineering plus some learning, ship it, then let production generate the training data.


[Ambi Robotics followed this path](https://www.youtube.com/watch?v=EtK5es0jVM4&t=2613) . Their package sorting system uses Dex-Net for grasp planning, but also traditional motion planning, PID control, and a deep learning warm-start for the motion planner. Good enough to deploy. Four years later, those systems have generated 22 years worth of real production picking data. They used it to train a new model that handles cases the old system couldn't - like picking up bags with wrinkles that fool suction cups - and they're seeing scaling effects as data increases.


Goldberg argues this is also what Waymo and Boston Dynamics are doing - gradually replacing engineered modules with learned ones as production data accumulates.


The full talk is worth watching:[Ken Goldberg: Good Old-Fashioned Engineering Can Close the 100,000 Year "Data Gap" in Robotics](https://www.youtube.com/watch?v=EtK5es0jVM4) .


## Learn more


If you want to learn more about physical AI, feel free to check out our[newsletter](https://topicqueue.substack.com/) . If you're a machine learning engineer getting started with physical AI, we're building a tool for multimodal model training called[MultiBase](https://www.eventual.ai/multibase) .
