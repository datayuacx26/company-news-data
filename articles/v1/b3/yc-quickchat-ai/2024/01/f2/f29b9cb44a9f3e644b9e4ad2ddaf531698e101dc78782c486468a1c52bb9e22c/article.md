---
schema_version: "1.0.0"
document_id: "f29b9cb44a9f3e644b9e4ad2ddaf531698e101dc78782c486468a1c52bb9e22c"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/problems-with-multilingualism-of-llms"
published_at: "2024-01-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:352fd499fbd7f6ea6958acf1756c98a6c4d7b3bc52ac92b912e57ea4f6406f84"
---

# Is ChatGPT a real polyglot? Problems with multilingualism of LLMs

You might think that advanced AI such as ChatGPT can easily handle conversations in any language but well… things aren’t always so straightforward. We work with international businesses every day and we’ve seen some weird and funny stuff that sometimes may be harmful to your business.


In this article, I’ll talk about the limitations of using tools like OpenAI’s ChatGPT in terms of multilingual capabilities, why things are the way they are, and what can you do about it.


If you prefer to watch the video version, head straight to YouTube:


## Using multilingual AI Agents in business


Let’s imagine you’re running an e-commerce business. It’s 2023 and using AI chatbots and assistants, for example, for automating customer support, is getting more and more common. You know, these little chats in the bottom right of a website that are sometimes actually useful? Yeah, they’re probably using artificial intelligence (or…they hired a mechanical Turk).


As your business grows and leaps into new international markets, you may want to alleviate the workload of your Customer Support team and have an AI assistant that’s a linguistic chameleon, able to speak in multiple languages like a native speaker.


Large Language Models (LLMs) support multiple languages, meaning the products that are based on them have picked up on a variety of human languages.


However, the performance of generative models deteriorates quickly in languages other than English. LLMs may struggle when tasked with low-resource languages, which have a smaller pool of data to draw from.


‍


## The reasons why ChatGPT speaks worse in non-English languages


### Availability of training data


That’s mainly because models behind tools like ChatGPT were trained on a ton of internet data, which has way more content in English compared to other languages.


The reason for this is deeply rooted in how LLMs work. The bigger the pool of data they are trained on, the better the model can analyze this data and recognize patterns in language use. For example, it learns how certain words are commonly used in different contexts, the way sentences are structured, and how meaning changes with different word combinations.


It can also impact the style and tone of the responses. In cases where a language has limited online data available, the model may adopt a tone which reflects that data. For instance, if during training Swahili data was scarce, and the only substantial source available online was a collection of “Terms of Service” documents, the model’s output in Swahili might mirror this formal style. This could result in responses that sound overly formal or technical, even in casual or everyday contexts. Such a tone mismatch can be confusing for native speakers.


‍


### Response speed


Let’s now move on to another aspect — the speed of responses. The need for speed. Everyone wants that, right? But if we want to speak about speed, we must understand how Large Language Models interpret and process text and what role tokenization plays in it.


‍


#### What is tokenization and how it influences speed


LLMs, to accurately understand language, use tokenization, which is the process of breaking down text into sub-word segments, so-called ‘tokens’, for processing. To illustrate how that works, let’s take the word ‘tokenization’. It can be split into ‘token-’, ‘-iz-’, and ‘-ation’. The words are split this way, so they can become building blocks that can be reused to build other words. We can re-use the building blocks of tokenization to build e.g. ‘ **token** -s’, ‘real- **iz** -e’, and ‘n- **ation** ’.


Why tokenize at all? Because there are too many words in existence, and too few letters to efficiently present language to the language model. The usefulness of the subword parts is determined by how frequently they occur in the training data and enables building more efficient representations, creating fewer but highly reusable tokens for constructing words, which reduces the need for creating additional tokens. As we said before, the majority of LLM training data is in English, and, as a result, an English sentence generally comprises fewer tokens compared to a sentence in German. In the end, processing a German sentence conveying the same meaning as an English sentence requires more time due to the increased number of tokens used.


(If you want to dive deeper into tokenization, don’t miss[this article](https://www.quickchat.ai/post/tokens-entropy-question) ).


How does that affect speed? The more tokens the model has to process in reading and generating the response, the slower the pace of the interaction. This means that, on average, conversations in languages other than English will take longer.


‍


## What it means for business use cases


You may still argue that conversational AI systems such as ChatGPT are doing fine and you’ll probably be right. For a personal user engaging in basic conversations with the AI system in their native language, the quality and speed are often sufficient for continued use. Even when mistakes arise, e.g. the agent makes a grammatical error, they can be easily overlooked because nothing of significance depends on it.


But it becomes a hurdle when the stakes are higher and LLMs are directly interacting with your clients or users. Would you risk your customers using the model, knowing that it might perform worse in some languages than others? Increase the response time? Confuse your prospects?


You’d probably have second thoughts.


‍


## Potential solutions — make your AI Agent fluent in non-English languages


So what can you do about it? Seasoned Googlers know it’s usually better to search in English and then just translate whatever you need to your native language.


And you probably do the same when using ChatGPT. Have you noticed somehow better, more natural and less generic responses compared to the other languages? Has that convinced you to stick just to English?


If you want to generate 10 headlines for your new blog post, you’ll probably find that it’s more efficient to do it in English and then translate them, taking into consideration your unique business and cultural context.


Let’s now put it in the context of LLMs and try to do something similar there. We could use ChatGPT in the language where we are guaranteed to have the most coverage, which is English. Later insert an additional translation layer, in the form of specialised translation models, between the actual LLM and the user utilizing the AI Agent. That way we can ensure reliability for all languages and gain more control over the translations.


Another advantage relates to what really makes your custom AI Agent stand out from a general-purpose tool like ChatGPT — the specific information you feed it through a Knowledge Base. Your company’s knowledge base could be any kind of document regarding your products or services that you can make the LLM use and thanks to that provide it with your specific context and knowledge. For example, you can use your product descriptions as a knowledge base, making sure your AI Agent gives correct and specific information when asked.


But here’s the tricky part. If your Assistant needs to handle multiple languages, the information you feed it also has to be in those languages. Imagine your business is in 20 different countries, each with its own language. That means you’d need 20 separate Knowledge Bases.


That’s a big ask and doesn’t really scale well in practice. We feel you, and adopting a different approach, in which you use that additional translation layer, allows you to provide the knowledge base only in one language, English, but you’ll still sound like a pro in every other language that you run your business in.


Neat right? I wish it got easier from there, but if you’re building AI solutions yourself, you still need to remember a few things.


‍


## LLMs’ struggle with brand names


Let me use an example. Let’s take the heavy industry and English-to-Polish translation. Polish energy experts use the term “kocioł” to refer to what in English is a “burner” - a critical component in their energy systems. However, a direct translation might mistakenly render “kocioł” as “oven” in English, which is a common translation, but incorrect in this specific context, and it will make you look like a newbie.


Since these models are trained on broad datasets that may not pick up on specialized or industry-specific language, they can struggle with accurately translating terms that have different meanings in different contexts. The model might correctly translate “kocioł” as “oven” in a culinary context but fails to recognize that in the context of energy systems, the correct translation should be “burner”.


Great, let’s take another, less heavy example. In Germany, there’s a museum called Kupferstichkabinett, the literal translation of which could be copper engraving cabinet but even the Museum itself translates it as Museum of Prints and Drawings.


You definitely use your own jargon, as every business and industry does, and we build features like[Custom Translations](https://www.quickchat.ai/post/product-update-custom-translations) to give you control over this aspect.


The second thing is ensuring that your brand’s unique elements – such as product names, taglines, sound natural and consistent, no matter where in the world they’re heard.


Let’s take the example of the Ford ‘Fiesta’ to illustrate how machine translations might struggle with brand names, especially when they overlap with common words. Imagine you’re translating a car review about the Ford Fiesta into Spanish where ‘fiesta’ means ‘party’ or ‘celebration’. AI systems that rely on general datasets, might not recognize ‘Fiesta’ as a car model name and instead translate it as ‘party’.


But I have a confession to make: I used the example of a well-known brand to make it easy to understand but AI is smart and will probably translate popular brand names correctly. But what about 99% of companies whose names are not that popular and don’t appear in training datasets very often? Like our company, or maybe the company you work at? The odds are not on our side.


‍


## Conclusion


While LLMs are a whiz at many tasks, they aren’t always polyglots. especially in the business world. You need to account there for the varying speed and quality of responses when using these AI Systems in different languages. Low reliability, longer response time and higher cost (due to the increased numbers of tokens used in less popular languages) of multilingual handling may get your company in trouble. You need to be sure that the AI tools you use are correct every time and don’t discourage or — in the best case — confuse your customers.


‍
