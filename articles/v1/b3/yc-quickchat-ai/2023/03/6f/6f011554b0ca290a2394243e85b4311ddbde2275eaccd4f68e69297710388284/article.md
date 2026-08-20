---
schema_version: "1.0.0"
document_id: "6f011554b0ca290a2394243e85b4311ddbde2275eaccd4f68e69297710388284"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/gpt-4-multimodal"
published_at: "2023-03-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:4b0e31a0a4a6cdb3d27cb7dec42c05061121b891665ca137db610016ec347b64"
---

# GPT-4: How Multimodal Learning Takes Us Closer to Human-level Performance

As you delve into the world of artificial intelligence, you may have stumbled upon OpenAI’s[ChatGPT](https://quickchat.ai/post/chat-gpt/) , a language model that has left many in awe of its abilities. Perhaps you find yourself intrigued, wondering how its successor, GPT-4, could possibly top the remarkable performance of ChatGPT. But here’s a secret: it’s not just about the number of parameters anymore. The real game-changer lies in the new, exciting advancements that will push the boundaries of what we thought was possible with AI.


## What is GPT?


For the title of this article to make sense, let’s take a small detour and briefly explore how GPT, or **Generative Pretrained Transformer** works. This should give us a better understanding of what makes them so powerful and how we have to make a major leap forward to make them even better. GPT-3 in particular is an LLM, or **Large Language Model** that has been trained on a vast amount of text data. Although this particular is a language model, it is just a neural network architecture that could work on a plethora of data, as we will learn in this article.


### Generative


Its primary objective is to predict the likelihood of each[token](https://quickchat.ai/post/tokens-entropy-question/) in its vocabulary, given the input text. Essentially, it attempts to predict the most probable next piece of text based on what has been seen previously. The process is autoregressive in nature, meaning that the model appends the predicted token to the existing text and generates another token until a token is generated, signalling the end of the generation process. There is not much more to it. If you write A cat has four , clearly the next most probable word is legs, and you can be almost certain about this without ever seeing a cat - just by reading some books.


Obviously, this is just a basic explanation of how this works - recent improvements like RLHF is what made the ChatGPT so *smart* . **Reinforcement Learning for Human Feedback**[1](https://quickchat.ai/post/gpt-4-multimodal/#fn:1) is a technique used in natural language processing and machine learning to train language models to generate text that meets human preferences. It involves integrating human feedback into the training loop of the model to fine-tune its output such that it *looks* good to the users. GPT-3 was introduced in 2020, but it took another two years to *align* the generation to make it look smart. Nonetheless, it is still just generating the most probable tokens, one by one.


‍


### Transformer


Sure, that’s the “Generative” part of GPT. But what is a “Transformer” and how do we “pre-train” it?


A **Transformer** is a type of neural network architecture that has been around since 2017 and is a basis for pretty much all recent breakthroughs in the Machine Learning area. It is widely used for natural language processing tasks. It uses a self-attention mechanism to capture long-range dependencies between words in a sentence or sequence. The self-attention mechanism employed by Transformers allows them to compute the probability of predicting the next word in a sentence by attending to the most relevant parts of the input sequence. By computing the weighted sum of the relevant elements in the sequence, Transformers can predict the most likely next word, even if it has never been seen before. This is because the attention weights assigned to each word pair change as the context changes, allowing the model to capture the dependencies between words in a flexible and dynamic manner. Interestingly, Transformers are not limited to processing natural language data. If you can represent any type of data as a *word* (such as image patches, code snippets, or protein sequences), you can use a Transformer to compute their representation effectively.


Transformers are incredibly powerful for both practical and theoretical reasons. One of the main theoretical benefits of Transformers is their ability to handle much larger context windows than previous neural network architectures like Recurrent Neural Networks (RNNs). This means that Transformers can take in more text or input data at once and still maintain the ability to understand the relationship between different elements in the input. In contrast, RNNs have to process input data piece-by-piece, which can result in forgetting important information from the beginning of the sequence and over-attention to recent data. With the larger context window offered by Transformers, the model can better understand the relationships between words and pieces of information over a longer sequence of text, leading to more accurate predictions and better performance overall. This is a significant theoretical improvement that has contributed to the remarkable success of large language models like GPT-3.


The practical gains are more prosaic - Transformers are great because of their highly *parallelizable* architecture. This means that they can be trained very efficiently on large-scale hardware, such as multiple GPUs or TPUs, allowing for much faster training times and the ability to handle much larger models with more parameters. In the case of GPT-3, the ability to parallelise training across thousands of GPUs was a major factor in its success and the ability to create such a massive model with 175B parameters. This allowed for faster and more efficient training, as well as the ability to handle a vast amount of data to learn from.


### Pretraining


The pretraining procedure plays a significant role in the *intelligence* capabilities of these models. One key factor in this is the recently popularised technique of **self-supervised** learning, which enables models to acquire a vast amount of information. We will show this on the[BERT\[2\]](https://quickchat.ai/post/gpt-4-multimodal/#fn:2) architecture which was the first major model to use Transformers.


BERT uses self-supervised pretraining to learn critical information about text. Prior to this, most models tend to be trained in a supervised manner using a dataset consisting of pairs of data - the information to learn and its corresponding label. The model predicted the label and was penalised for each error it made. While this approach works well for small datasets and simple tasks, natural language is incredibly complex, and obtaining a labelled dataset of sufficient size is challenging. Additionally, we face a practical limit in that labels must be coherent for the model to learn effectively. Providing arbitrary labels and hoping that the model will learn from them is not feasible. That essentially means that we cannot utilise multiple datasets


- it is best if it is one huge dataset with a single type of label.


Self-supervised learning addresses this problem by reducing the reliance on labelled data. To understand how this works, let’s use an analogy in the context of a child learning to predict a missing word in a sentence. In supervised learning, the child would predict the missing word, and the teacher would verify if the prediction is correct or not. However, this learning process would be limited by the speed of the child’s prediction, as well as the availability of the teacher’s knowledge (i.e., labels).


‍


#### Self-supervision


To improve this, imagine that there is no teacher, and the child is left to play and learn on their own. Instead of receiving a text with missing words and relying on the teacher to verify their prediction, the child takes any text they find and simply covers one of the words without seeing it. They then make their best guess at the missing word and uncover it to check if they were correct.


This method is not as efficient as having a good teacher, but it allows the child to learn in a *self-supervised* way. Similarly, self-supervised learning allows machine learning models to learn from unlabelled data by using a variety of techniques to predict missing information, such as masked language modelling, where the model predicts missing words in a sentence, or contrastive learning, where the model learns to differentiate between similar and dissimilar examples.


The benefits of self-supervised learning are two-fold. First, the model (or child) needs a comprehensive understanding of the semantic meaning of the text to predict the missing word, which implicitly requires them to learn to *understand* the text, yielding the best results in the word prediction task. However, this is not enough to solve most tasks, unless we simply want to predict a missing word. Still, it serves as a pre-training step or a foundation that aligns the model’s knowledge better, requiring only a *fine- tuning* step on the target dataset.


‍


#### Limits of current pretraining


We can use the vastness of the internet to train our model to *understand* text. Any text we find on the internet can be used for training, which provides a nearly ‘unlimited’ source of data. However, as ridiculous as this may sound, we have already almost hit the limit of what text is available. Most Large Language Models have been trained on Wikipedia, all books, websites, and anything else the companies like OpenAI had their hands on, over and over, and there is little else for it to read. While different tasks can be used to keep learning, reading the same data repeatedly has its limitations


- imagine reading a book back-to-front because you have done the normal way too many times already. Therefore, finding new ways to acquire meaningful training data remains a crucial area of research to improve the performance of these large models.


*Immersive movies speak to us a lot better than just words* ‍


### How GPT-4 changes pretraining


After the extensive introduction, we can finally focus on the topic of this post: how to enable models to keep learning and improve their understanding of the world. To illustrate this concept, let’s think about it from the perspective of a child. Reading is undoubtedly beneficial for children, but if it’s the only thing they do 24/7, we would be concerned about their overall development. We would encourage them to engage in other activities, such as playing in the garden, listening to music, or even watching goofy cartoons where Tom chases Jerry.


In the world of machine learning, this type of learning is referred to as **multimodal** learning. It involves using different types of information or *modalities* to learn. To better understand this concept, we can compare it to experiencing a movie. Watching a movie without sound or listening to a movie without visuals does not provide us with the full experience. We could try doing one after the other - watching a movie and then listening to it separately which would obviously not be very efficient or entertaining. However, in machine learning, even this type of learning things sequentially is not currently viable due to the **catastrophic forgetting** phenomenon. Models must learn everything at once to make sense of it, similar to watching a movie with sound - and as you can imagine this has not been possible at large scale - up until now.


### Move towards other modalities


For years, it has been known that using multiple modalities is the way to go in machine learning. However, it’s important to start with the easiest modality, which is typically text. The reason for this is that our vocabulary is surprisingly limited, with most people using no more than 35,000 words in day-to-day communication[3](https://quickchat.ai/post/gpt-4-multimodal/#fn:3) , and the typical sentence containing only around 10 words. In contrast, images have millions of pixels, with each pixel having 256 shades of red, green, and blue. This complexity is difficult to account for, even in the context of the masking task that our kid had to solve. It’s not clear to humans what could be under a patch of, say, 100x100 pixels - there are millions of potentially correct answers, making it challenging to properly grade machine learning models for their responses.


However, recent breakthroughs in diffusion models, such as[DALL-E](https://openai.com/product/dall-e-2) or[Stable Diffusion](https://stability.ai/blog/stable-diffusion-public-release) , have shown that we can work around these challenges and achieve impressive results even in the visual domain. In addition, new models are emerging for other modalities, such as music generation, GIF generation, text-to-speech models, speech-to-speech models, and more.


If we look at how social media has evolved, we see a similar trend towards more immersive platforms that incorporate multiple modalities. Twitter and Facebook started with text, Instagram added images, TikTok introduced videos, and now Meta is betting big on the even more complex platform of the *Metaverse* .


Machine learning models are a few modalities behind the Internet world, and this makes sense because they require large amounts of data and the hardware needs to catch up before we can train them to perform complex tasks like humans do on the internet. However, to make a truly groundbreaking change, we should aim to combine the knowledge and get the full experience of *watching a movie* . If we are able to combine the pre-training tasks for all modalities and learn the attention between a text, video, cartoon and a song about a cat, we get a lot more **intelligence** than if we only were able to read a book.


## Conclusion


It’s time we make the AI master what we have always been good at - merging multiple signals into a coherent representation. By bringing the multi- modality to the masses, GPT-4 is making a huge step forward.


Does that mean that GPT-4 is at Human-level intelligence? Obviously **not** and this article is not arguing for it. Not to mention that the physical world is still a mystery to the AI. However, we see that AI is a lot closer than we were imagining even a couple of years ago - we can create[AI Assistants](https://quickchat.ai/post/chatgpt-api-announcement/) that use better grammar than most people, can be compassionate and help us in day-to- day tasks. AI can draw paintings that we thought only true artists could. But it wasn’t able to connect the dots between these two - and this is now changing.


At[Quickchat AI](https://quickchat.ai/) we have been working hard on exploring what is possible with Large Language Models for years, and we believe we have a good understanding of their capabilities. We are launching new features every week for AI Agents which are already revolutionising how brands interact with their customers. So we are incredibly excited to explore the many ways GPT-4 will change our lives. If you are also interested and passionate about this, do not hesitate to see our[careers page](https://www.quickchat.ai/careers) as we have many technical and non- technical positions open.


1. [OpenAI](https://openai.com/blog/chatgpt) has a great explanation how they have used RLHF to develop ChatGPT.[↩︎](https://quickchat.ai/post/gpt-4-multimodal/#fnref:1)
2. [BERT](https://arxiv.org/pdf/1810.04805.pdf) has been a huge success and is one of the main reasons that Transformers have took off. The masked language learning that it uses was not new, but it was the first architecture that could take a full advantage of it.[↩︎](https://quickchat.ai/post/gpt-4-multimodal/#fnref:2)
3. The Oxford Dictionary has over[150,000 entries](https://wordcounter.io/blog/how-many-words-are-in-the-english-language) , but most English native speakers use only 20,000–35,000 words. The total number of words in usage is hard to estimate but certainly much higher than that.[↩︎](https://quickchat.ai/post/gpt-4-multimodal/#fnref:3)


‍
