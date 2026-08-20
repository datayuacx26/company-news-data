---
schema_version: "1.0.0"
document_id: "6ef6214d7764903ff7c600c36deb4388ca1c0a241596461c3583b404fd83d4eb"
company_key: "yc-codecrafters"
company: "CodeCrafters"
source_id: "yc-codecrafters-news-import-01a1b304c75c"
canonical_url: "https://codecrafters.io/blog/writing-for-developers"
published_at: "2025-04-15T00:00:00+00:00"
first_seen_at: "2026-07-21T14:04:10.550467+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:e16f53f0a8680307aa7d18f3db7e9d11a7540287b0eff069bf3557859a44cf6b"
---

# Writing for Developers

Developers have a high bar for content.


Today, we're publicly releasing the internal guidelines we use to write for our audience of developers.


## Know Your Reader


Always picture one specific person as your target reader.


Understand the expertise level, pain points, and motivations of this reader.


This is important to ensure a high degree of relevance. One post can't serve everyone.


If you want to cover multiple audiences for a topic, break it into a series of posts.


## Best Practices


After GPT, people (and least of all, developers) lack the patience to go through unnecessary text.


Avoid long sentences (anything over 30 words). Attention spans are fragile.


Write like you speak, with simple words and clear, natural sentences.


Avoid unnecessarily complex words like "akin" and "delve". The subject of your writing is complex enough. Don't make your reader spend their limited energy on figuring out the meaning of your words.


What language would you use when speaking to a friend? Use those same words when writing.


❌ Bad Example ✅ Good Example


Utilize the appropriate syntax to achieve the desired outcome. Use the right syntax based on your needs.


The techniques discussed henceforth will enhance your understanding of low-level programming. Here are some techniques to help you get better at low-level programming.


We shall now delve into the intricacies of memory management. Let's break down how memory management works.


…but don't over-optimize. A touch of creative writing has its place, it can add rhythm and personality to your words.


Avoid forcing short formal phrases that you'd typically use in emails and internal documents. (Honestly, don't even use them there.)


Uphold grammar, maintain flow, complete your sentences.


❌ Bad Example ✅ Good Example


Tool provides value. Helps teams collaborate. Documentation is okay. This tool makes team collaboration smoother, and its documentation is clear and easy to follow.


Why this framework works? 1. Fast performance 2. Easy to set up 3. Scales well This framework is fast, easy to set up, and scales well. It's a solid choice.


Again, the rule of thumb is to write like you're speaking to your audience on stage.


Many blogs use up 10-50% of the writing on setting up the introductory context. Skip all of that, get to the point. If you've identified your reader correctly, they will already have the context.


For example, if you're writing a guide on Docker deployment, skip sections like "What is Docker?" or "Why use Docker?". Open with 2–3 sentences (perhaps about why this guide was necessary), then dive straight into the steps.


Be precise. Don't make vague statements.


❌ Bad Example ✅ Good Example


We'll learn some advanced Rust features. We'll learn about ownership, borrowing, and lifetimes, which are core concepts in Rust.


This approach doesn't always work. This approach fails when you try to mutate shared data across threads.


Don't spell out what the reader can infer or judge on their own.


❌ Bad Example ✅ Good Example


This article will enhance your understanding of Rust's ownership model. This article explains Rust's ownership model.


This guide is perfect for beginners and experts. This guide starts with the basics and moves into advanced topics.


Here is a really cool hack to achieve X. Here is a hack to achieve X.


Once you have a draft, review and trim anything that doesn't add value.


## Content-Blogging-specific advice


Aim to inspire creativity and curiosity. You don't always need to spell everything out. Nudge your reader to think in interesting ways and explore things on their own.


Just enough direction to spark ideas, without spoonfeeding.


Software developers are already subject matter experts. Don't over-explain things.


For hard and confusing topics, the best articles are simple, fun, and easy to read, without watering down the technical depth.


Whenever possible, bring something fresh that other blogs miss.


Introduce angles that are fresh or overlooked.


If you don't understand parts of your topic, that's not a bad thing, necessarily. Admit it.


Take extra care to get terminology right. Mistakes will cost you credibility.


Typos, mixing up casing e.g "Git hub" or "Javascript", mixing up asynchronous and concurrent, or calling Rails or React a programming language are all red flags.


It's probably a nitpick, but avoid saying "coding" which can come off as amateurish. Stick to "writing code" or "programming".


Writing from personal experience is extremely valuable. Developers appreciate insights from those who have actually dealt with the topic.


You can talk about how you built, used, learned, or fixed something.


Reading others' experiences can spark ideas readers can use to solve problems or build something.


In these kind of posts, write about the messy process, successes and failures.


Acknowledging weaknesses and blind spots will build trust. Ignoring or dismissing them will raise questions on your understanding or integrity.


Make your blog authentic and relatable by being transparent and objective.


If you don't have personal experience, you can try to research and present some insights from others' experiences.


There are varying views on the inclusion of humor and memes. We welcome programming humor, you don't need to play safe.


You can visit r/ProgrammerHumor to get inspired.


Avoid edgy humor or commentary on completely unrelated topics. You don't want to unnecessarily offend your audience. Or lose credibility trying to look funny.


For programming posts specifically, include working code. Link a GitHub repository. This makes your article more useful to the reader.


Finally, remember that all of the above are guidelines. You should always keep them at the back of your mind, but don't be afraid to break a rule when you feel strongly.


If everyone wrote the same way, it'd be boring. Your unique style will make your writing more valuable in the sea of SEO-optimized, generic content.


## Presentation


Use paragraph spacing after every few sentences. This makes the blog easy to digest and retains the reader's attention.


This style ensures that less readers lose the will to keep reading until the end.


Add images, GIFs, or videos every 1–2 screen scrolls.


Visuals should have intrinsic value, and not just serve as text breaks. Use them to add insight or commentary that deepens the conversation.


Use captions to add further to the conversation instead of just describing visuals.


Maintain a design style consistent with the rest of your website pages and graphics.


## Native English Tests


Unnatural language can sometimes slip into writing, especially for non-native speakers.


Use tools like[Hemingway App](https://hemingwayapp.com/) and[Grammarly](https://www.grammarly.com/) to receive ideas to improve your drafts.


These apps give helpful suggestions, but remember to use your own judgement in the end.


You can also use ChatGPT. Enter a prompt like the one below.


*"Rewrite these sentences to how a native English speaker would have written them. Don't lose the main message or deviate too much from my writing. Just tweak the parts that don't sound like a native English speaker speaking."*


## No AI-generated content


Developers won't take you seriously if they sense your writing was produced by LLMs.


Avoid GPT-generated content. In fact, we recommend that you *proactively* avoid writing styles that are now associated with AI:


- "X is not only Y, it is Z."
- Overuse of em dashes ( — )
- Cliche openings like "In today's X world…"


Use tools like Quillbot's AI detector and aim for 0% detection.


## Promotions


You should only promote a product or service in your blog if and where it adds real value to the conversation and benefits readers.


Avoid inducing any forced bias. Developers will detect it instantly and reject your blog.


## What to Write?


No developer has ever said, "I'd love to read a list of 50 Python packages."


Focus on real problems developers face where they


1. don't understand. Make complex things simple for them.
2. get confused. Help them do the right thing.
3. waste too much time. Give them insights that save effort.


Here are few themes where content is always welcome. We have linked an example for each.


- [Compare tools, techniques, approaches](https://smalldatum.blogspot.com/2025/03/at-what-level-of-concurrency-do-mysql.html)
- [Share effective ways to learn](https://learning-rust.github.io/docs/overview/)
- [Build software while learning internals](https://zserge.com/posts/6502/)
- [Explain how large systems work](https://medium.com/@abhinavcv007/bittorrent-part-1-the-engineering-behind-the-bittorrent-protocol-04e70ee01d58)
- [Share insights you discovered](https://buttondown.com/jaffray/archive/the-case-of-a-curious-sql-query/)
- [Solve common developer headaches](https://michael.stapelberg.ch/posts/2025-02-27-debug-hanging-go-programs/)


## Final Thoughts


Writing is hard. Your first drafts will need several revisions. Ideas will change as you write them down for the first time. That's normal. Your writing skills will develop with practice.


Over time, you'll realize that good writing both comes from and sharpens your clarity of thought.


It also forces you to improve your understanding of the topic you're writing about.


Finally, if we've inspired you,send us your articles . We'd love to read your work and share it with our community of top developers.


## Resources


Paul Graham's short musings on writing are full of insights:


- [Write Simply](https://www.paulgraham.com/simply.html)
- [Writing, Briefly](https://www.paulgraham.com/writing44.html)
- [Putting Ideas into Words](https://paulgraham.com/words.html)
- [Writes and Write-Nots](https://www.paulgraham.com/writes.html)
- [How to Write Usefully](https://www.paulgraham.com/useful.html)


[Hacker News guidelines](https://news.ycombinator.com/newsguidelines.html) provide a general idea of what kinds of posts work well there. Additionally, this[HN thread](https://news.ycombinator.com/item?id=34127742) shares tips straight from the community.


You can also check out Phil Eaton's[list of great non-corporate tech blogs](https://eatonphil.com/blogs.html) or this[round-up of the best engineering blogs from dev teams](https://draft.dev/learn/engineering-blogs) .
