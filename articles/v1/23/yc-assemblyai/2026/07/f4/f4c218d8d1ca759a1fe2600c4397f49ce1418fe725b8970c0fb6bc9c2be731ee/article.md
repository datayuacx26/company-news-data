---
schema_version: "1.0.0"
document_id: "f4c218d8d1ca759a1fe2600c4397f49ce1418fe725b8970c0fb6bc9c2be731ee"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/lemur"
published_at: null
first_seen_at: "2026-07-22T08:04:49.593567+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:def4b3f476aca4ee8385fa5dcce6f44cc38299b39f5bb75e6ea9e0dc6ddc600f"
---

# LeMUR: Now Available for Early Access

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


[Large Language Models](https://www.assemblyai.com/blog/the-full-story-of-large-language-models-and-rlhf/) (LLMs) are changing what users expect in every industry. However, it is still difficult to build[Generative AI](https://www.assemblyai.com/blog/introduction-generative-ai/) products centered around human speech because audio files present challenges for LLMs.


One key challenge with applying LLMs to audio files today is that LLMs are limited by their context windows. Before an audio file can be sent into an LLM, it needs to be converted into text. The longer an audio file is when transcribed into text, the greater the engineering challenge it is to workaround LLM context window limits.


LeMUR, short for Leveraging Large Language Models to Understand Recognized Speech, is our new framework for applying powerful LLMs to transcribed speech to solve this issue. With a single line of code (via our Python SDK), LeMUR can quickly process audio transcripts for up to 10 hours worth of audio content which effectively translates into ~150K tokens. By contrast, off-the-shelf, common LLMs are only able to fit up to 8K or ~45 minutes worth of transcribed audio within their context window limits.


To solve the complexity of applying LLMs to transcribed audio files, LeMUR essentially wraps a pipeline of intelligent segmentation, a fast vector database, and reasoning steps like chain-of-thought prompting and self evaluation as illustrated below:


**Fig. 1 — LeMUR's architecture enables users to send long and/or multiple audio transcripts into a LLM with a single API call.**


> **“LeMUR unlocks some amazing new possibilities that I never would have thought were possible just a few years ago. The ability to effortlessly extract valuable insights, such as identifying optimal actions, empowering agent scorecard and coaching, and discerning call outcomes like sales, appointments, or call purposes, feels truly magical.”**


Ryan Johnson, Chief Product Officer at CallRail


Explore LeMUR Today


Join thousands of developers and product teams leveraging LeMUR to access LLM capabilities for spoken data.


[Learn More](https://www.assemblyai.com/products/speech-understanding?ref=assemblyai.com)


## What LeMUR unlocks


### Apply LLMs to multiple audio transcripts


LeMUR enables users to get responses from LLMs on multiple audio files at once and transcripts up to 10 hours in duration, which effectively translates to a context window of ~150K tokens.


Without LeMUR


LeMUR


### Reliable & safe outputs


Because LeMUR includes safety measures and content filters, it will provide users with responses from an LLM that are less likely to generate harmful or biased language.


Without LeMUR


LeMUR


### Inject context specific to your use case


LeMUR enables users to provide additional context at inference time that an LLM can use to provide personalized and more accurate results when generating outputs.


Without LeMUR


LeMUR


### Modular, fast integration


LeMUR consistently returns structured data in the form of consumable JSON. Users can further customize the format of LeMUR’s output, to ensure responses are in the format their next piece of business logic expects (for example, boolean answers to questions). This eliminates the need for building custom code to handle the output of LLMs, making LeMUR just a few lines of code to practically bring LLM capabilities to users’ products.


### Continuously state-of-the-art


New LLM technologies and models are continually being released. AssemblyAI pulls in the newest breakthroughs into LeMUR and all of our ASR models to ensure users can build with the latest AI technology.


> “LeMUR works incredibly well out-of-the-box. It allowed us to focus on product instead of infrastructure. As a result, we were able to bring a transformative new product to market in half the time.”


Alexander Kvamme, Co-founder & CEO at Pathlight


### What you can build with LeMUR today


To start, LeMUR is focused on a set of flexible endpoints that can be used for multiple use cases. We’re expanding the number of use cases rapidly and would like to hear yours.


#### Question and Answer


```text
import   assemblyai.sdk   as   aai


transcript = assemblyai.transcribe(  "3_hour_customer_call.mp3"  )
question =   "In one sentence, what did the customer say was the best new feature?"


answer = transcript.lemur.question(question)
print(answer.text)
>>   "The customer said that having an online coach available during study hours was the best feature of the learning product."
```


#### Custom Summary


```text
import   assemblyai.sdk   as   aai


transcript = assemblyai.transcribe(  "poetry-superpowers.mp3"  )
summary = transcript.lemur.summarize(  "This is a popular TED talk."  ,
answer_format=  "one bullet point"  )
print(summary.text)
>>   ""  "
• A man describes growing up with dyslexia and struggling in school, finding his passion for writing and storytelling through discovering hip hop music, slam poetry, and comic books. He honed his craft by observing the world around him and finding inspiration in everyday people and places. He eventually made it to Broadway as a playwright, achieving his childhood dream of becoming a "  superhero  " through the power of storytelling.
"  ""
```


#### AI Coach


```text
import   assemblyai.sdk   as   aai


audio_files = [  "ultimate-django-course.mov"  ,   "django-quickstart.mp3"  ,
"python-django-beginners.mov"  ,   "starting-django.mp4"  ]


transcript = assemblyai.transcribe(audio_files)


coach_topic =   "What can the instructors do better to explain URLs?"
feedback = transcript.lemur.ask_coach(coach_topic)
print(feedback.text)
>>  ""  "To improve explaining URLs, the instructor could:
- Provide visual examples of URL structures and relationships.
- Verbalize the logic behind how URLs map to app structures and entity relationships.
- Highlight best practices for REST API URL design and how those apply to the concepts being explained.
"  ""
```


> **“LeMUR’s highly accurate and highly customizable framework has given us the essential tools needed to deliver the next evolution of GenAI experiences for project management.”**


Jon Johnson, CEO at AnchorAI


Experience AssemblyAI's Models


Test our Speech-to-Text, Sentiment Analysis, and Topic Detection models instantly with your own audio.


[Try AI Models Now](https://www.assemblyai.com/playground)
