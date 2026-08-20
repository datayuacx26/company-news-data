---
schema_version: "1.0.0"
document_id: "14e5a1a84d2de103cd2ad0bba1de1f11df293710ed54023bded5769b2c2fa3bd"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c3d7277c32d9"
canonical_url: "https://www.assemblyai.com/blog/assemblyai-replicate"
published_at: null
first_seen_at: "2026-07-21T08:04:02.365197+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:eb71628ebcfd6ba7b2fbdc44b47bf782f9d8f5f2a17045838205733d6aad51ac"
---

# Building community and unlocking AI innovation

Back when AI was more commonly referred to as Machine Learning and long before AI was integrated into almost every product we use, Ben Firshman was thinking about ways to make it easier for researchers and software developers to share their findings and build with Machine Learning and AI.


His experience at companies like Docker, as well as prior frustrations with having to piece together findings from papers to rebuild Machine Learning models from scratch, spurred him to start a research community for Machine Learning researchers and developers. The community would be an all-in-one place for researchers and developers to “put their work inside a box” and more easily share their Machine Learning, or AI, models with other developers in the space, so they could continue to tinker with and build on those models.


This idea became the foundation for[Replicate](https://replicate.com/) , an open-source community for developers trying to “replicate” Machine Learning models. To help achieve this goal, the models shared on the Replicate platform had to be packaged in a way that let other developers implement the models for real-world tasks without having to rebuild the model based on the initial research paper — saving time, unlocking innovation, and allowing more opportunities for collaboration.


Since its founding in 2019, Ben has expanded Replicate to match the needs of its users' demands, hosting thousands of models contributed by the community, and building a tool suite to further support the open source community as the complexity—and demand—for AI products has skyrocketed.


Ben recently joined AssemblyAI founder and CEO Dylan Fox to discuss their shared experiences in building AI companies during this period of rapid AI transformation and adoption across every industry.


#### Here are a few takeaways from that conversation:


##### 1. Innovation in AI takes foresight and a little bit of luck


**Ben Firshman:** “We sort of worked with this community and built a tool for this community, and then Stable Diffusion happened. And that's when these text-to-image models really reached the masses. And it's where we were positioned perfectly as the place where these models were. It was a place where people were publishing these models, and where people were tinkering on these models and making variations of them as well, which was really the interesting bit about the open source community.”


“But **right at that time, loads of people wanted to build products out of these models** . People wanted to build image editors, they wanted to build AI avatar generators, they wanted to build generative games and all these kind of things just around the time of ChatGPT, where there was all this interest, and then **it was just this sort of perfect storm of the supply and demand that just helped us grow** .”


##### 2. Developers love building with AI because of how magical it can feel


**Ben Firshman:** “Partly it was just **the core technology was really magical. For the first time ever, you could write a description of something you want, and it just appeared instantly** . So most of the initial traction and the initial products that were being built were just people plugging Stable Diffusion into an existing application or building a new application out of it.”


##### 3. Unlocking AI innovation and user adoption starts with unlocking barriers to building with AI


**Ben Firshman:** “What's really interesting about Llama is that it was the first really capable Large Language Model, or within the same realm as the proprietary models like OpenAI's models. But it was tinkerable, and you could fine-tune it, and you could mess with the code, and you could plug it into other models and this kind of thing, which really caught the imagination of the open-source community.”


“So there was this sort of community of hackers, a bit like that early colab community with Stable Diffusion, who are tinkering on these models and making interesting variants of it, but it was encumbered by licenses, which meant you couldn't use it for commercial use. But **that caught the imagination of the hacker community because it felt naughty..** .”


“That community sort of grew around there, but it was non-commerical. It wasn't really good enough. And it wasn't until Llama 2, wherever Meta, kind of realized this was really interesting. And, oh, we should probably, we should probably make a version here that's not encumbered by all this, all this sort of research licenses. So that's when they made Llama 2, and **that's when it really took off, because Llama 2 was much better and it was possible to use in products as well** .”


##### 4. The stage of a company changes how they interact with AI


**Dylan Fox:** “How are people actually building successful products on top of all these 10,000 different models that are on Replicate?”


**Ben Firshman:** “One way to understand this is segmenting by different mediums and different stages of the company… And things startups are doing is quite different from enterprises. And we're primarily building for startups. And when we say startups, it's both actual startups, but also small teams inside large companies that are kind of behaving like startups. **These teams are having a lot of success building, like either whole products that are native to AI, or like particular point solutions to certain things inside the product** .”


“To give you a few examples of things that are really working with our customers, we have lots of people who are building consumer apps with Generative AI. So people like people who are building these virtual photo shoot applications, which are enormously popular, like some of our top customers at building these applications, where you let you sort of create your own photoshoots, people are building image editing apps which are either using Generative AI as like a starting point for an image, or as a way to edit images. **So there are these whole startups that are built around these new mediums. We also see it being mixed into existing products in a really successful way.** ”


##### 5. Fine-tuning LLM models is becoming reserved for specialized use cases


**Ben Firshman:** “ **It’s a bit more nuanced that. There are use cases of fine-tuned language models** . And to be clear, we see customers fine-tune language models, and in fact, people are deploying fine-tuned language models on Replicate. It's just not a sort of first class part of the product anymore.”


**Dylan Fox:** “What are those use cases where it does make sense?”


**Ben Firshman:** “Two things it works really well for is sort of similar to image models, like if you want it to get to be in a very particular style, like if you have like a house style for how you write or you wanted to learn, you know, a language or learn a program language or something like that… So a customer that is deploying fine-tuned models on Replicate for their use cases. They have a proprietary query language and they want to turn natural language into that query. **And for that it's almost like teaching it a style** .”


##### 6. It’s easy to build a beautiful prototype but it requires more duct tape and heuristics to build a truly robust product


**Ben Firshman:** “I think there's some interesting, interesting developments with multimodal models that do like whole things end to end. And I think that's super interesting. In reality, for a lot of these kind of complex, multimodal applications, there's almost always going to need to be some kind of duct tape involved. And **I think what we find when these models hit the real world, you can't just ship a big model as the product** . There's always going to be some duct tape and some heuristics and some filters and some massaging of the output to make it behave how you want. So however it pans out with how these models end up and how they behave, **there's always going to be a lot of duct tape and heuristic involved in building products..** .”


“...It's very easy to make a really good prototype. It's very easy to make something that looks really cool. But that is to some extent it's like most products, but to even more extreme extent, I think that is 10% the work. **The remaining 90% is getting it actually working in production as a sort of reliable, robust product** .”


##### 7. Developers are building new products with a clear problem in mind, a desire to incorporate specific technology, or both


**Ben Firshman:** “It's sort of similar to any building any kind of product, but I think it's just exacerbated by the fact that these systems are very complicated and people are not really sure how to, what to do with them. Like with any product, it's coming from two sides. It can be either technology driven or it can be sort of pulled by the problem.”


“So we see customers who are like, oh, we have this problem with our product. Let me see if I can try and use AI to solve this problem. And you sort of try a bunch of things, try different models, see if we can solve the problem, or, which is quite common, particularly for startups, and particularly with a lot of these new models that have come about in the past couple of years, there's this really cool piece of technology, and what new kind of products could we build with these, or what new kind of features rebuild an application that could be done with these? **And it's a mix of both. And both require iteration and experimentation.** ”
