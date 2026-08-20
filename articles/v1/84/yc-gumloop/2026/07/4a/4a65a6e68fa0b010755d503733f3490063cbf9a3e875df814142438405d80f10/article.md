---
schema_version: "1.0.0"
document_id: "4a65a6e68fa0b010755d503733f3490063cbf9a3e875df814142438405d80f10"
company_key: "yc-gumloop"
company: "Gumloop"
source_id: "yc-gumloop-news-import-488d75a2156b"
canonical_url: "https://www.gumloop.com/blog/open-weight-vs-open-source"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-21T22:19:59.182624+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:b4cbb1f29e4cdee56947a7ecb910e2322455ce5f6f9d7d4827acae2d00295ff3"
---

# Open weight vs open source: Everything you need to know

Technically most of them are only “open weight” but after going down a rabbit hole on the topic, I’ve learned that while there is a very clear difference, most people do not care.


I’ll explain the difference between open weight and open source models as succinctly as possible. No AI fluff or slop of any kind used to write this!


## What are the key differences between open weight and open source?


Let’s compare AI models to cars for the sake of this analogy.


- An open weight model would be a car you are given and can take home. You can do whatever you like with it, tinker, repaint it, tune it to your liking and drive it freely.
- An open source model would be that same car for you to take home but it also comes with an industrial manual explaining how to build it from scratch, all the tools necessary for the build and the raw materials you’d need. You’d be able to make your own version of the car if you wanted to or use that manual as the ground-work for your own custom car.
- A closed model (all frontier models) would be comparable to an Uber. You pay per ride and you are merely borrowing its time, you do not own it whatsoever. You can barely control what music is playing on the radio let alone understand how the engine functions.


The[Open Source Initiative](https://opensource.org/) (the same nonprofit that defined what “open source” means for regular software) put out its Open Source AI Definition at the end of 2024. The short version is that a model is only open source if you’re free to use, study, modify and share it, and to make that actually possible the creators have to hand over the training code, a detailed description of the training data and everything else a skilled person would need to rebuild an equivalent model from scratch, not just the finished weights. When following this definition, most open source models you’ve heard of violate the definition and should be called open weight as a result. They’re still technically “open” models but not open source.


The easiest way to place any model yourself is to ask two questions.


“Can you download the model weights and run it yourself”. If you can, it’s an open model. If you can't, it's a closed model.


“Alongside the weight, have you been given everything I need to recreate this model?”. If you can, it’s open source. If you can’t, it’s only open weight.


Most companies on earth are “renting their intelligence” from the frontier labs rather than owning it.


## Properties of an open weight model


The[open source models](https://opensource.org/ai/open-weights) are released with all of the code, training data and architecture details you would need to retrain the model alongside the model’s “weights”.[Open weight models](https://www.gumloop.com/blog/open-weight-ai-models) are only released with the weights with none of the secrets around how those weights were achieved.


Since the weights of both models are publicly available, anyone can take those weights and run the model on[their own infrastructure](https://www.gumloop.com/blog/open-source-ai-agents) . This is great news because it’s allowing people to run AI models themselves instead of forcing everyone to use models from the big AI labs (OpenAI, Anthropic, Gemini).


## Properties of an open source model


An[open source model](https://www.ibm.com/think/topics/open-source-llms) comes with the entire recipe, not just the finished dish. Alongside the weights you get the training code, the architecture details and a description of the data it was trained on, which is everything you’d need to retrain it or build your own version from the ground up. Going back to the car analogy, this is the model that ships with the industrial manual, the tools and the raw materials in the trunk.


That level of openness is rare. It’s expensive to train a model and even more painful to document and release every ingredient behind it, so the models that truly qualify tend to come from research-driven efforts rather than companies protecting a commercial edge. Projects like OLMo, Eleuther AI and BLOOM are some of the few that clear the bar.


## Why are frontier models still the most popular?


Everyone in the AI community is itching to play around with[the latest tech](https://artificialanalysis.ai/leaderboards/models) so using anything that is even remotely outdated is unappealing to enthusiasts. On top of pure preference, everyone is fiercely competing in the tech community, any edge over competition is necessary so most people default to leasing their intelligence at a large premium.


If we consider the car analogy, the frontier models are supercars that you can rent from an exotic car dealer. You pay an extremely high premium and you need to give it back when you’re done but for a brief moment you have the best product the industry can offer.


Open models used to be so far behind the frontier that people would scoff at them like they’d scoff at a rusty old Honda Civic beside a Porsche.


These open models have been improving so quickly recently however that they’re increasingly turning heads. Especially when people see the wildly affordable price tag. Every new open model release is getting harder to tell apart from the super cars.


## Why do people release open models at all?


It is a bit confusing why a company would ever train a model and then just put it out into the world for free. Two of the fastest growing companies in human history (Anthropic and OpenAI) have made generational businesses by doing this research work and then keeping the instruction manual very secret.


It may seem like charity on the surface but there is always some financial motivation behind this that was needed to justify spending millions to train the models. The motivation varies greatly.


- **Destabilizing competition:** One of the biggest suspected reasons organizations are releasing open models is to knock leaders off of their perch. If an AI lab can’t confidently compete against the big players to win market share, they can destabilize their lead by giving away their product for free. A common assumption is that Chinese AI labs are aggressively working on open models in order to slow the progress of their American counterparts. Many of these labs have even been accused of directly using frontier models in order to generate data to train their own copy-cat models.
- **Commoditizing the complement:** Some large organizations, like Meta, have released impressive open models to “commoditize the complement”. A complement is something that can be used alongside your core product. Meta generates its revenue almost entirely with ads but the business that serves those ads will undoubtedly use billions in AI. If they can help turn those models into a cheap, interchangeable commodity, they never have to depend on a frontier lab, or pay its premium, to power the thing that actually makes them money.
- **Free sample strategy:** Some labs will release smaller open models as a way to draw people in but then sell their more powerful models via API. Companies like Mistral have done this in the past.
- **Ecosystem:** Some companies are taking the Android playbook of creating an open ecosystem of builders and tinkerers around their core offering. This lets them build a community while being able to steer the overall direction with every subsequent release.
- **Free R&D:** Once an open model is released, the community takes it and runs with it. They will explore quantized versions of it, find bugs, red team it and find novel optimizations. It’s a very efficient way to crowd source creativity for future improvements. This is especially appealing if the model the company originally created isn’t challenging any benchmark leaders.
- **Brand Marketing:** Climbing a benchmark is an undeniable way to get recognition. If a model you create can beat existing leaders you’ll become a household name overnight (Deepseek experienced this rise to fame). It’s significantly easier to dethrone open models on benchmarks compared to closed ones as well.
- **Talent:** Similarly to the brand marketing angle, if you’re a household name, talented people will hear about what you’re up to and consider working for you. Talented people tend to follow other talented people and what better way to signal talent density than creating a competitive open model.
- **Regulatory positioning:** The EU in particular has lighter restrictions on models using free or open source licenses so it can be used as a means of avoiding compliance burden.
- **Genuine Mission:** Some companies truly have a greater mission. This is much less common since training models is so expensive that a business plan normally has to form to fund the training. Projects like OLMo, Eleuther AI and BLOOM fall into this category.


## Where are these open models coming from?


Let’s go over some of the most popular open models and break down how open they actually are, who created them and what their motivation was.


Model Created by How open is it? Why they open-sourced it


GLM MIT


Zhipu AI (Z.ai), a Beijing lab spun out of Tsinghua University MIT


Open weights under MIT: self-host, fine-tune, and use commercially with no caps. Training data stays private. Adoption is the business model. Free frontier-grade coding models win developers, and the paid API and enterprise deals ride on that install base.


DeepSeek MIT


DeepSeek, a Hangzhou lab funded by the High-Flyer hedge fund MIT


Open weights under MIT, plus unusually detailed technical reports explaining how the models were trained. A research lab at heart. Publishing everything attracts talent, and it proved frontier reasoning could be trained at a fraction of US lab budgets.


MiniMax Apache 2.0 / custom


MiniMax, a Shanghai startup best known for consumer AI apps Apache 2.0 / custom


Open weights. Earlier releases used Apache 2.0 and MIT; the newest models ship under MiniMax's own license. A consumer company buying developer credibility: small, cheap models tuned for coding and agents put its name in front of builders.


Gemma Apache 2.0


Google DeepMind Apache 2.0


Open weights. Early Gemma versions carried a custom Google license with usage restrictions; Gemma 4 moved to Apache 2.0. A sanctioned taste of Gemini tech: small models you can run anywhere, with Google Cloud waiting when you outgrow them.


Llama Custom license


Meta Custom license


Open weights only. The Llama Community License caps very large companies, restricts some uses, and disqualifies it as open source by OSI's definition. Commoditize the model layer. If strong models are free, no rival can own the platform, and the ecosystem standardizes on Meta's stack.


Qwen Apache 2.0


Alibaba's Qwen team Apache 2.0


One of the most open model lineups at every size, nearly all under Apache 2.0 with no usage caps. Every Qwen deployment seeds demand for Alibaba Cloud, and the sheer range of sizes makes it the default base model for fine-tuners.


Notice the pattern? Every single one of these is open weight, not open source.


### Is all the data from open models going to China?


Not unless you’re choosing a version of the model that is hosted in China!


The beauty of the models being open is that anyone can host them. Some of the fastest growing startups in the United States are inference providers that host these models and serve them via an API. Companies like Fireworks, Together or Base10 have created businesses around efficiently hosting these open models in their own data centers.


If for some reason, you choose to use the official Deepseek API, you are however sending your data to China so be conscious of the inference provider you use.


## Why is open weight so much more popular than true open source?


When looking at the most viral open models, almost all of them are open weight rather than open source. There are actually so many reasons to release as open weight that I’m a bit surprised anyone is releasing true open source models.


- **The blueprint is the moat:** unless the model is extremely impressive, its weights aren’t that valuable on their own. New models will come out next week that challenge whatever performance previously impressed you. All of the secretive work that went into discovering those weights is worth billions of dollars however.
- **Open weight meets most needs:** Most companies or privacy conscious individuals only care about being able to run the models on their own infrastructure and being able to fine tune models for their specific use cases. Open weight provides both these freedoms.
- **The market doesn’t know the difference:** The very fact that I wrote this article to explain the difference between open weight and source is evidence in itself. Making the model open weight will get most of the glory open sourcing it would have provided with a fraction of the hassle.
- **The blueprint will be used by very few:** Showing people how to rebuild the model is great for the scientific community but few people can truly use those blueprints. To truly put them to use would cost tens of millions of dollars most likely. This is the same reason people aren’t too upset with car manufacturers for not also providing the industrial manual for how to build the car, most people don’t have a factory to build it in.
- **Legal seppuku:** It’s pretty widely accepted that training large models requires tons of data scraped off the internet. This data is often in a grey zone or outright illegally trained upon. There have been legal cases between every frontier lab and owners of data that have proven some sort of data theft was required to create their models. Most frontier models used to be able to recite entire Harry Potter books from memory for example. To share the data is to share the evidence of theft.


## Why are open models gaining so much popularity?


There are 3 main reasons why the topic seems to have entered the mainstream.


- **Cost:** open models can be hosted by anyone. This means end users (like companies who need AI in their product) can host the models themselves. This also means any company can host them and sell access via an API. This API business is extremely competitive so they’re selling them for as little as they feasibly can in order to grow their market share. Being able to get the same performance for 80% less cost is an inherently viral recipe.
- **Sovereignty:** The idea of “owning your AI” is becoming more important with every industry these large labs are trying to subsume. The reality is that millions of companies have been sending their data directly to the lab that will try to crush them in the future with a competing product. Having control over where your data goes and cutting any reliance on the labs is enticing.
- **Rapid improvement:** It is legitimately hard to tell the difference between the best open models and the top lab models for most use cases these days. Getting a better price and having confidence in your future without any downsides is the core reason we’re seeing a surge in interest.


## So does the difference actually matter?


Technically, yes. Open source and open weight are not the same thing, and now you know exactly why. Practically, for almost everyone, the honest answer is not really. If you can download the weights, run the model on your own hardware and fine tune it for your use case, you already have the freedom you were after. The full recipe is a beautiful thing for the scientific community, but very few people have the millions of dollars and the data centers it would take to actually cook from it.


What does matter is the direction things are heading. Open models are getting cheaper, better and harder to tell apart from the frontier every month, and the idea of owning your intelligence instead of renting it only gets more appealing. Whether the label says “open weight” or “open source”, the more interesting story is that you no longer have to rent a supercar to keep up. You can park a very fast one in your own garage.
