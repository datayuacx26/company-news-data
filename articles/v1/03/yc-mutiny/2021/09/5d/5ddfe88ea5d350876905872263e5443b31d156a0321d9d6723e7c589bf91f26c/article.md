---
schema_version: "1.0.0"
document_id: "5ddfe88ea5d350876905872263e5443b31d156a0321d9d6723e7c589bf91f26c"
company_key: "yc-mutiny"
company: "Mutiny"
source_id: "yc-mutiny-rss-ceed54e2b595"
canonical_url: "https://engineering.mutinyhq.com/generating-personalized-website-headlines-using-gpt-3/"
published_at: "2021-09-30T06:37:32+00:00"
first_seen_at: "2026-07-24T11:48:32.930778+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:c5f1f087fcc8d082971f160c3c0558a8346fd9b8b671a1d4ea7c37bfd547bdab"
---

# Generating personalized website headlines using GPT-3

> Hi, I'm Alec, one of the engineers here at Mutiny, a company focused on helping marketers convert their web traffic using personalization. Over the past few months, I've been working on delivering our recently unveiled headline suggestions by using GPT-3. Over several blog articles, I'll walk you through our learnings from getting started with GPT-3, approaches on how to improve your completion quality, and techniques to build and grow your training dataset once your model reaches production. You can also check out our[ProductHunt](https://www.producthunt.com/posts/mutiny-3-0?ref=engineering.mutinyhq.com) announcement to try personalized headlines on your own website.


## Why we're doing this


Today, the best marketing teams need both strong analytical skills and creative thinking. They need analytical skills to effectively measure the impact of their programs and creative skills to make sure their message cuts through the noise. At Mutiny, we like to say that we can help with both the art and science of website personalization. We provide data-driven recommendations to help marketing teams focus their efforts where they'll have the biggest impact. And we show marketers what other companies have personalized in order to see success. But we realized that some marketers, after identifying an opportunity for website personalization, had a hard time transitioning from an analytical to a creative mindset. " *What content should I personalize in crafting my experience? What messaging speaks best to startups?"*


We decided to help marketers tackle this problem by providing suggestions of potential content they could use to speak to their audience.


> Using our existing dataset of successful headlines launched on Mutiny, we set out to generate personalized headlines to help guide our customers in the art of website personalization.


To accomplish this, we needed some sort of language model, which could generate coherent marketing copy. There are many language models available, but after consulting with several people familiar with the space, we decided to move forward with GPT-3.


## Getting started with GPT-3


GPT-3, which stands for Generative Pre-trained Transformer 3, is a language model developed and hosted as a service by OpenAI. Currently, it's under private beta, but we'll show you examples that can help you reproduce our results once you're able to get access.


As a general model, GPT-3 is capable of generating human-like text for a wide array of tasks, including writing a story, translating, completing analogies, answering trivia questions, and more. For those new to language models, it may have an unfamiliar interface. As programmers, we're used to structured data and explicit APIs that operate on that data. But GPT-3 is completely different. Instead, we must describe the task we want the model to complete entirely in words. For most applications, this means:


1. Serializing our structured data into a text description that defines the task we want GPT-3 to complete, called a prompt
2. Send our prompt to GPT-3, which respond with a text "completion"
3. Parse the completion back to structured data to extract the desired data


This interface seems like something you would expect from a chatbot instead of programming with a cutting-edge machine learning technology.


Once we overcame the initial hurdle of how to write a prompt, the simplest prompts we started with looked like:


```text
Write a website headline for Carta tailored for startup companies. Their current headline is "Equity. Simplified."
Headline:


```


Our prompt is simplistic and our initial completions were poor quality. We'll show you some example completions from this prompt so you can get a sense of how to improve the quality improves with our changes.


Here are some of the example completions we get from this prompt.


- "It's time to stop fundraising, start equipping"
- "How Equity's Changed. Why Carta?"
- "Trade"


As you can see, our initial quality is pretty terrible. The generated headlines are generic, buzz-wordy, and lack messaging that's particularly relevant for startups. Sometimes the model even fails to generate something that even resembles a headline.


At this point, we actually felt stuck. It seemed like there weren't adequate developer inputs to affect the completion. Our only option was to better describe the problem to a computer in words instead of writing detailed instructions in code. We had a lot of questions:


- How do we control what GPT-3 generates so I can reliably show the output to a user?
- How do we know what information GPT-3 needs to reliably complete its task?
- How do we know that the wording in my prompt is understandable to the model?


## Diving into how GPT-3 works


To answer some of these questions, let's first take a quick detour to discuss how GPT-3 works to better understand the completions it generates.


GPT-3 is a large neural network machine learning model that has been trained on almost 500 billion tokens of scraped internet data, books, and Wikipedia content.


When generating a completion from a prompt, GPT-3:


1. Parses the prompt and extracts relationships between words to try to understand what you're asking for.
2. Repeatedly generates the next token based on the last token it's seen. The token is probabilistically selected from all possible tokens, where the probability of each token is influenced by both the training corpus and the prompt.
3. Continues to generate the next token until it generates a stop sequence\[1\] or it reaches a specified token limit.


---


1.


A stop sequence is a configured sequence of characters that tells the model that it has reached the end of the completion and should return it↩︎


Let's look at this in action. When we prompt GPT-3 with **"San Francisco,"** , based on tokens that commonly follow in the corpus, GPT-3 generates: **" CA 94105"** . Looks like a lot of proud tech companies are putting their ZIP code on their website :)


However, when we prompt GPT-3 with **"If You're Going to San Francisco,"** it responds with **" Be Sure to Wear Some Flowers In Your Hair (1967)"** , quoting the song by Scott McKenzie.


Even though the two prompts ended the same way, GPT-3 responded very differently. This illustrates how tokens in the prompt greatly affect the probabilities that GPT-3 uses when considering the next token.


## Engineering the problem into a prompt


Now that we understand some of the decisions that GPT-3 is making, it's time to use our prompt to ensure it generates meaningful headlines. As we saw in the initial example, we can include text instructions describing the task it is meant to accomplish. But we can also include additional metadata it needs to generate these headlines. One crucial piece of information that the model is missing in its corpus is what Carta does as a company. When we include it, the next iteration of our prompts looks like:


```text
Write a website headline for Carta tailored for startup companies based on the description.


Description: Carta is an equity management platform to help private companies manage their cap tables, get 409A valuations, complete tender offers, and more.
Headline:


```


Here are our results:


- Carta - The Equity Investment Tracking Tool
- The ultimate equity management tool for private companies
- Equity Management Simplified - Share Trading Made Easy
- Modernize Your Capital Structure With Carta
- Streamline company management with Carta
- Technically-Aware Service for All of Your Equity Needs
- Professional Equity Management Services Made Easy
- Carta is the Future of Equity Management
- The Cap Table and Equity Management Expert
- Finding the Right Fit - The first, comprehensive equity management platform for private companies


In this iteration, the quality and reliability of the generations are improved. What we've started doing is encoding the headline writing process in the prompt. After all, in order to generate a headline for a company, GPT-3 must know what the company does.


Based on the previous section, we also know that including the description causes it to be more likely to produce a headline relevant to what the company does. Without it, GPT-3 just generates the most likely tokens from the corpus.


We've started breaking down the steps involved in writing a good headline by including the company description. Maybe there are additional steps involved before we arrive at a good headline. What if we highlighted a product use case for startups?


```text
Write a website headline for Carta tailored for startup companies based on the description and use case.


Use case: Startup companies need a fast, easy way to issue equity to their initial hires.
Headline:


```


Here are some highlights:


- Carta: Startups' Easy Equity Management Platform
- Make an easy first hire with Carta
- Get Equity To Your Employees Quick


## Teaching a generalist to specialize


So far, we've prompted GPT-3 to generate headlines using only an instruction and some metadata, such as a company description. But the prompt does not leverage any of Mutiny's dataset of existing successful personalized headlines. Instead of telling GPT-3 the steps it needs to take to solve the problem, we can just have it learn by example.


```text
Write a website headline for Carta tailored for startup companies based on the example descriptions and headlines.
===
Example Description: DocSend is a secure document sharing platform that allows document owners to share content, see read receipts, collect e-Signatures, and more.
Example Headline: The Secure Document Sharing Platform for Startups
===
Headline:


```


The` ===` sequence is a stop sequence we use to signify the end of an example. Based on the prompt, GPT-3 is likely to generate the stop sequence at the end of a headline, which will cause it to stop generating and return the completion.


Here are our improved headlines:


- Simplify Equity Management for Your Startup
- The Easy-to-Use, All Inclusive Equity Management Platform for Startups
- Your go-to equity management solution for startup companies
- 5 Must-Have Tools for Private Company Equity Management
- Streamline Equity Management for Startups with Carta
- Equity Management Made Easy for Your Startup
- A Full Suite of Equity Management Tools and Services for Startups
- Optimize Your Equity Today with Carta
- The Solution for Private Company Equity Management
- We Make Equity Management Easy


The results look vastly improved just from a single example! Not only do the headlines feel more real, but they also feel more contextual to startups. By including examples, we're also instructing GPT-3 on what it looks like to solve our specialized task—to generate contextual headlines from a company description.


Before we wrap up, here are some considerations when including example data:


1. GPT-3 can infer information about what is important to startups from our examples. If an example headline for startups mentioned being easy to set up, GPT-3 might be able to generate a headline for Carta like: "Issue equity in minutes, not months".
2. We can end up constraining the diversity of our completions if our examples are too narrow. When asking GPT-3 for a poem, if we only give haikus as examples, it's unlikely it will ever write us a sonnet.


## Continue iterating


At this point, we've generated respectable headlines with a few key steps:


1. Describe your task in words in your prompt.
2. Break down logical steps and provide key information.
3. Include examples to show GPT-3 what we're looking for.


Hopefully, these concepts are enough to get you started in your journey with GPT-3. Doubtless, there will still be more tinkering and experimentation to do. In future posts, we'll talk about additional strategies to systematically improve final completion quality, how to bootstrap an example dataset, and discuss our approach towards experimenting with different prompts.


So far, Mutiny has gotten warm feedback on our headline suggestions, but we're only getting started. If you're interested in using cutting-edge machine learning technology to enable marketers,[we'd love to hear from you](https://www.mutinyhq.com/careers?ref=engineering.mutinyhq.com) . If you have thoughts on our approach, or experience with GPT-3, please reach out. Until next time, happy prompting!
