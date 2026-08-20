---
schema_version: "1.0.0"
document_id: "7973fdd8e0a317e9b806c762f4f4318becea6cc99f3210d6a9cd26848a146b5b"
company_key: "yc-sapling-ai"
company: "Sapling.ai"
source_id: "yc-sapling-ai-rss-bcde5e160aa9"
canonical_url: "https://sapling.ai/devblog/chatgpt-phrases/"
published_at: "2023-10-18T00:00:00+00:00"
first_seen_at: "2026-07-25T22:15:27.632672+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:51e073fb6a93257b62151aa13a06bc9c768e9e2f6608528fae1b5ec3fe8c7562"
---

# ChatGPT's Favorite Phrases

## Introduction​


Since the release of OpenAI's ChatGPT in November of 2022,[hundreds of millions](https://www.reuters.com/technology/chatgpt-sets-record-fastest-growing-user-base-analyst-note-2023-02-01/) of users have used it to generate billions of tokens per day.


ChatGPT, like other large language models (LLMs), has a distinctive style, and in the initial months of its release would frequently use the phrase "as a large language model" as a precursor to its responses.


Since then, this tic appears to have been[reduced](https://www.reddit.com/r/ChatGPT/comments/1576v41/finally_chatgpt_is_no_longer_going_to_say_as_a/) , but led us to wonder: what other phrases are strongly associated with ChatGPT's style, but not with the writing of humans? Beyond these statistics, what does this tell us about its training?


## Method​


Our approach was to take question-answer (QA) data with responses by both humans and by ChatGPT. We then determine the phrases that *frequently occur in ChatGPT responses but infrequently occur in human responses* . While this can be done by manually inspecting outputs and making observations, we instead form all possible subsequences of words in the texts (up to length 5) and count the frequency of each.


### Data​


To perform an empirical analysis of ChatGPT's favorite phrases, we used the[HC3 dataset](https://huggingface.co/datasets/Hello-SimpleAI/HC3) . This dataset contains ~24K examples of human and AI-generated responses to prompts from Reddit, WikiQA, StackExchange, and other sources of question-answer pairs.


We note that this is just one snapshot of responses, and while we may expect the human styles and phrasings to hold steady (at least over the course of a few years), LLMs such as ChatGPT are evolving and changing by the week; the datasets described may need to be updated.


### n-grams​


After some simple preprocessing of the data, we computed the 5-grams in each split (human, AI) in the dataset. This resulted in 6,036,620 human n-grams and 3,631,482 ChatGPT n-grams: the first difference we observe is that human-generated text remains more diverse than AI-generated text.


We would expect certain[n-grams](https://github.com/orgtre/google-books-ngram-frequency/blob/main/ngrams/5grams_english.csv) to appear frequently in both splits (` at the end of the` ,` in whole or in part` ) but others to appear much more frequently in human-generated text and others to appear much more frequently in AI-generated text. More precisely perhaps, we would not expect LLMs to generate specific sequences of text due to the methods in which they've been trained and finetuned with human preferences.


## Results​


Based on the analysis above, here are the top 20 phrases most indicative of **human-generated text** :


Phrase Log Probability Delta


the end of the trading -2.3243


put a lot of strain -2.0574


what you want to achieve -2.0332


to give you a basic -2.0118


for the most part its -2.0069


a bit of a mystery -1.9875


i am not a medical -1.9823


is a matter of personal -1.9750


a few of the many -1.9646


i hope that helps let -1.9407


it is up to each -1.9109


the money in the account -1.9042


if you are a nonresident -1.8966


the ratio of the circumference -1.8936


to do this is by -1.8752


are some of the main -1.8651


to the united states constitution -1.8348


the tip of the \[redacted\] -1.8265


come up with a plan -1.7752


as a matter of law -1.7748


the best of my ability -1.7566


the length of the side -1.7447


the energy that is released -1.7409


the result of a combination -1.7016


there is also the risk -1.6976


Here are the top 20 phrases most likely to appear in **ChatGPT-generated text** and not in human-generated text:


Phrase Log Probability Delta


<s> there are a couple 2.9191


its also important to think 2.3774


is always a good thing 2.1334


it is also important that 2.0646


it is important to know 2.0253


can vary depending on what 2.0024


i hope this helps </s> 1.9463


you may want to check 1.9204


<s> another reason is because 1.9133


i hope that helps </s> 1.8907


best course of action is 1.8803


it is generally considered impolite 1.8401


it is important to study 1.8371


to keep in mind is 1.8360


i hope this helps you 1.8343


a good idea to go 1.8296


<s> there are a lot 1.8218


by a variety of methods 1.8128


<s> there are many theories 1.8114


to keep in mind the 1.8098


one way to think of 1.8061


it can be difficult if 1.7998


a good idea to put 1.7873


be able to determine that 1.7555


is important to remember however 1.7528


Aside:` <s>` and` </s>` are special tokens indicating the start and end of a text. The number following each n-gram is the difference in log-probability between that text appearing in human vs. ChatGPT-generated text.


## Observations​


There are clear patterns in the phrases associated with ChatGPT:


- Emphasizing important points (` it is important...` ,` is always a good thing` , ...)
- Expressing uncertainty (` can vary depending` ,` you may want to check` , ...)
- Presenting multiple options and viewpoints (` its also important to...` ,` it is important to remember...` , ...) Many of these patterns are likely a result of the human examples and preferences that ChatGPT has been tuned on.


It's more difficult to immediately identify patterns in the human n-grams. Perhaps a slightly lower degree of formality (` to give you a basic` ,` for the most part its` ) and also topics such as medical and financial advice where LLMs can be more strict and formulaic given the gravity of the topic.


## Conclusion​


The phrases we've identified demonstrate patterns in ChatGPT's responses that are a reflection of the data that was used to train it. But how strong a signal are they of whether a text was AI-generated or not?


AI-generated content is proliferating, and it can be helpful to have surefire "signatures" (such as` as a large language model` ) that a text was produced by an LLM. But this is only effective in cases where the user was sloppy and left these telltale signs in the generated text.


Many of tried to create **watermarks** that indicate whether an image is AI-generated. These properties can also be put in an image's metadata to indicate its provenance. However, the same is much harder to bake into easily editable text. Some researchers have proposed probabilistic watermarks based on[partitioning vocabularies](https://twitter.com/tomgoldsteincs/status/1618287665006403585) ; however, these require the participation of all major LLM developers.


Interested in other, up-to-date approaches for flagging AI-generated text? Try Sapling's regularly updated[AI detector](https://sapling.ai/ai-content-detector?utm_source=sapling.ai&utm_medium=documentation&utm_campaign=apidocs) and[contact us](https://sapling.ai/contact?utm_source=sapling.ai&utm_medium=documentation&utm_campaign=apidocs) if you're interested in using it for your business.
