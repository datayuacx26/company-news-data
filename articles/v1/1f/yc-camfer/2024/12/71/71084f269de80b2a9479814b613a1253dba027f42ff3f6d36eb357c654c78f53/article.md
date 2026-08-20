---
schema_version: "1.0.0"
document_id: "71084f269de80b2a9479814b613a1253dba027f42ff3f6d36eb357c654c78f53"
company_key: "yc-camfer"
company: "camfer"
source_id: "yc-camfer-news-import-eadafe2b34d8"
canonical_url: "https://camfer.dev/blog/our-problems/"
published_at: "2024-12-10T00:00:00+00:00"
first_seen_at: "2026-07-24T23:30:43.881527+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:1461c8ea3f18e2be7b83197259e4180d66415f353f5d8655dec2e3e624a579f3"
---

# Things We're Thinking About

Hi! Keaton here. We have the privilege of working on some really cool problems at camfer. Here are a few of the things we’re thinking about.


## Spatial Reasoning


LLM’s today have a tenuous grip on how objects exist/behave in 3D space. Is this a foundational limitation of the models themselves? Or are they just bottlenecked by the amount of relevant tokens they see during training? Camfer is a high-conviction bet on the latter.


We’ve made a ton of progress so far trying out different ways of encoding spatial information. Soon, we want to build a semantic search engine for the physical world - where finding similar designs is as easy as cosine similarity in a well-structured latent space.


## Post-training


We’re thinking a lot about how to get the most out of foundation models. Beyond just instilling an understanding of 3D CAD, how can we train them to perform tasks autonomously and reliably? How can we overcome the limitations of supervised fine-tuning? How can we practically scale RL for LLMs on long-horizon tasks?


Most of building agents is unproven in production; canonical answers and best practices are unestablished. This is the time to try out kooky ideas and fail fast. Many of the research papers we read establish their approach on smaller models and demonstrate their results hold with increasing parameter counts. We have the compute to scale these ideas all the way. Exponentials and emergence are incredibly powerful!


## Integrations


We want camfer to be at the engineer’s fingertips at all times. The reality of this means we need to deeply integrate with the industry-standard CAD platforms.


We’re building bridges between these complex systems and our AI models. From the customer’s perspective, it should just work (and hopefully be magical too). But we still have some open questions. How does the CAD UX evolve in this new ecosystem? How can we solve our problems while working with the black boxes of different platforms? We believe this constrained problem space breeds innovation.


## Non-parametric 3D Models


There’s an entire world of non-parametric 3D representations - point clouds, STL files, STEP files, and more.


We’re developing methods to learn from these diverse representations. We want universal 3D understanding, regardless of how the geometry is represented. How will this play with our parametric models? We’re still figuring this out.


## Join Us


If you’re the type of person to get excited about this, reach out (you can literally just email us:hiring@camfer.dev )! We feel the same way :).


One last thing I’ll add. There’s a[great essay](https://kortina.nyc/essays/the-emperor-has-no-clothes-there-is-no-santa-claus-and-nothing-is-rocket-science/) by Andrew Kortina where he talks about (among other things) technological determinism. About the feeling that, if you don’t build it, someone else will. That maybe the work is just meaningless.


The point is that, when *you* are the one building it, *you* get to decide what it looks like. There are a million ways for AI and CAD or humans and agents to be brought together. Will it isolate engineers and suck the fun out of their jobs? Or will it empower them to do more, build more, innovate more? If you feel strongly about this, there’s no better place to be heard. And hopefully, we’ll all have some fun building it along the way.
