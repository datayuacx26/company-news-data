---
schema_version: "1.0.0"
document_id: "981197660b4bccaf90390d551206c21a51af2715781473da41353ebc07791cf1"
company_key: "yc-glide"
company: "Glide"
source_id: "yc-glide-news-import-b50025bbdd3a"
canonical_url: "https://www.glideapps.com/blog/data-interview-mark-probst"
published_at: "2025-06-12T12:00:00+00:00"
first_seen_at: "2026-07-21T21:51:19.848750+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:79e5f0d4f452890453bd72085c253855c602e60758f08d1cd9cf8f2ea06867be"
---

# Data has gravity: A conversation with Glide Co-Founder Mark Probst

*Glide’s Jack Vaughan has been interviewing members of the team about their areas of expertise and the larger ideas around no-code, AI, and building custom business software. Today, he interviewed Glide Co-Founder*[Mark Probst](https://www.linkedin.com/in/markprobst/) *on the most important core piece of any app—data. They discuss how the initial structure of your data makes such a huge difference in how your business software develops and how effective, scalable, and stable it will be. Here is their conversation.*


**Jack:** I’ve heard you say before that data has gravity. Can you elaborate on what that means?


**Mark:** People say data has gravity. It’s the idea that the bigger your dataset gets, the harder it is to move. Not just because it’s physically large, though it often is, but because everything else in your system depends on it. You’ve got code, automations, and assumptions all tied to that data’s structure. If you change the data, all of those things need to change too.


**Jack:** So it’s not just about size?


**Mark:** Right. It’s about structure, too. You can’t just snap your fingers and change the shape of your data. If your business is running 24/7, you don’t really get “off” time to refactor. Even if you shut things down to migrate, you still have to make sure the new structure works, the new code works, and you’ve got a rollback plan in case things break.


**Jack:** Let’s talk about structure. How should data be shaped?


**Mark:** That depends. There’s always a tension between shaping data for internal convenience versus shaping it around how users think. In many cases, you do want to optimize for performance and system integrity. Then you build an interface that maps to the user’s mental model.


Take Figma, for example. If you looked at their database, it wouldn’t resemble the canvas you see on screen at all. And that’s intentional—your mental model isn’t necessarily how computers work best.


**Jack:** What about normalization? That seems to come up a lot.


**Mark:** Normalization is where you try not to store the same data in multiple places. Like, if someone changes their name, you update it once instead of a thousand times. That’s great for consistency.


But it comes with a cost: performance. If you want to show someone’s name on a comment, you have to go look it up. Sometimes, especially in no-code environments, it’s easier to just store the name with each comment. That’s called denormalization. It’s convenient. Until you need to change it everywhere, that’s when you say, “Ah, I wish I had normalized this.”


**Jack:** How does this apply to no-code tools like Glide?


**Mark:** In tools like Glide, you feel this trade-off. Denormalized data is easier to work with at first, but as you scale, things get messy. You end up creating lookups of lookups just to get basic info.


**Jack:** You’ve mentioned vector databases and semantic search. How do those fit in?


**Mark:** That’s a newer, really exciting area. Vector databases let you search by meaning instead of keywords. You can use them for things like Retrieval-Augmented Generation (RAG), where a language model finds relevant information and uses it to answer a question.


But honestly, as a builder, you often don’t need to know how the vectors work. We use Pinecone, but through a layer that just lets us upload files and ask questions. All the technical stuff happens behind the scenes. It’s like traditional databases: you don’t need to know how indexing works to use them.


**Jack:** So, how important is data when building software?


**Mark:** You can’t build software without thinking about data. Every app you use—your calendar, your smart lock, your bank—exists to show, collect, or manipulate data. Even if the software does something physical, like unlocking a door, it’s still reading and writing data to make decisions. That’s why, especially in no-code, you have to start with the data. Interface without data is just decoration.


Learn how to build a data dashboard


[Without any code](https://www.glideapps.com/blog/build-a-no-code-dashboard)


Create a Glide app in five minutes, for free.


[Sign Up](https://os.glideapps.dev/?utm_source=blog&utm_campaign=Data+has+gravity%3A+A+conversation+with+Glide+Co-Founder+Mark+Probst)


[Jack Vaughan](https://www.glideapps.com/blog/author/jack-vaughan)


Jack Vaughan is the Senior Video Producer and Educator at Glide. Based out of Edinburgh, Scotland he has a passion for Software and Motion Design and produces a[podcast](https://bento.me/jackvaughan) where he interviews experts and inspirational figures in the field.


### Related Articles


[AI engineering is the next wave after no-code: Here is how business leaders can get started](https://www.glideapps.com/blog/no-code-to-ai)[Gideon Lahav](https://www.glideapps.com/blog/author/gideon-lahav) 28 Jul 2026


[How to build custom software for e-commerce operations with AI](https://www.glideapps.com/blog/build-ecommerce-software)[Wren Noble](https://www.glideapps.com/blog/author/wren-noble) 27 Jul 2026


[What is AI citizen development? A guide for 2026](https://www.glideapps.com/blog/ai-citizen-development)[Wren Noble](https://www.glideapps.com/blog/author/wren-noble) 24 Jul 2026
