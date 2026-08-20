---
schema_version: "1.0.0"
document_id: "1d43cea632c2892f5903c022721d5fd3235d7bef48f7fc1e578b861aee8d2746"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-26fc52334fc9"
canonical_url: "https://forum.newsblur.com/t/ask-ai-to-make-sense-of-the-news/13374"
published_at: "2026-01-16T23:35:46+00:00"
first_seen_at: "2026-07-25T16:10:33.287397+00:00"
fetched_at: "2026-07-28T22:23:30.670356+00:00"
content_hash: "sha256:404be0e8c9fe13ec468f7cae5837b325906e1ba13ca5e0b39d1d7bd895e23b16"
---

# Ask AI to make sense of the news

[samuelclay](https://forum.newsblur.com/u/samuelclay)


January 16, 2026, 11:35pm 1


```text
<p>Sometimes I open a story and realize I’m late to the party. Maybe it’s the fourth article in a saga I haven’t followed, maybe it name-drops people I should know, or maybe it’s dense and I just want the gist. Ask AI fills in those gaps. You can ask about any story in NewsBlur and get quick answers from the model you prefer.</p>


```


### How it works


Select a story and click Ask AI in the story toolbar. You’ll see quick options for getting information about the story.


### Not interested? Turn it off


I know AI features aren’t for everyone. If you’d rather not see Ask AI at all, you can hide it completely. Go to **Manage → Preferences → Stories** and toggle off “Show Ask AI button.” The button disappears from your toolbar and you’ll never be bothered by it again. NewsBlur has always been about giving you control over your reading experience, and this is no different.


### Presets for common questions


At the top of the menu, you can summarize the story in three flavors: a single sentence, bullet points, or a full paragraph. Below that are presets I use most often:


- **What’s the context and background?** — Fills in backstory you might have missed
- **Identify key people and relationships** — Who are all these names and how do they connect?
- **What are the main arguments?** — For opinion pieces or debates, cuts to what each side is saying
- **Fact check this story** — Flags claims that look worth verifying


Or just type whatever you’re curious about.


### Speak your question


See that microphone button? Click it and say your question out loud. NewsBlur uses OpenAI’s Whisper to transcribe your voice, so you can keep reading without typing. Handy when you’re on the couch and don’t want to pull out the keyboard.


### Pick your model


Different models have different strengths. NewsBlur lets you pick from:


- **Claude Opus 4.5** — The default, most capable model from Anthropic
- **GPT 5.2** — OpenAI’s latest
- **Gemini 3 Pro** — Google’s offering
- **Grok 4.1 Fast** — xAI’s speedy option


NewsBlur remembers your choice across sessions and devices. If a reply feels off, re-ask with a different model — they each have their own style.


### Keep the conversation going


After the first answer, ask follow-ups. NewsBlur keeps the conversation context so you can dig deeper or ask for a clarification. It’s meant to be a back-and-forth, not a one-shot.


### Your data stays yours


Your stories and questions are sent to the AI provider you choose to generate a response, but that’s it. NewsBlur doesn’t use your reading data or questions to train models or for any other purpose. The AI providers’ standard privacy policies apply, but NewsBlur itself treats your Ask AI usage the same as any other part of your reading experience — it’s your data, not ours.


### Availability


Ask AI is live on the web. Here’s how many questions you get:


- **Premium Archive** : 100 questions per day
- **Premium + Free** : 1 question per week


Premium Archive limits reset daily at midnight in your local timezone. Premium and free limits reset weekly on Sunday.


If you’re not yet a Premium Archive subscriber, you can[upgrade on the web](https://www.newsblur.com/?next=premium) to get the full Ask AI experience along with unlimited story archiving and full-text search.


As always, I’d love to hear what you think on the[NewsBlur forum](https://forum.newsblur.com/) . If you have ideas for new preset questions or ways to make Ask AI more useful, let me know.


---


This is a companion discussion topic for the original entry at[https://blog.newsblur.com/2026/01/16/ask-ai/](https://blog.newsblur.com/2026/01/16/ask-ai/)


[yayadrian](https://forum.newsblur.com/u/yayadrian)


January 17, 2026, 12:05am 2


Cool to see new features and looking forward to testing this more.


two questions I’m interested to know


1.


Why is it using the biggest and more expensive models?


Seems like this level of question should be suitable for Sonnet, GPT 5.2-mini, Gemini 3 Flash


2.


would you consider letting users provide their own API key?


I totally understand this doesn’t encourage users to archive plans but interested in your views.


keep the improvements coming. Hope 2026 is a great year for NewsBlur


[samuelclay](https://forum.newsblur.com/u/samuelclay)


January 17, 2026, 12:16am 3


Why not use the best models? I don’t expect usage of this to be high enough for it to matter. I’ll adjust if it does. I have a bunch of charts in grafana watching usage.


BYOK, if anything, will only be allowed on the Premium Archive tier. But it would only be to get around the 100 questions/day limit, and I don’t see that happening. I might adjust things, but I just don’t see it as being an issue. Not yet, anyhow.


I do use the smaller models for story categorization, which is an upcoming feature in the browser archive feature that’s currently in testing:[Archive Extension: Browser history capture with AI-powered search by samuelclay · Pull Request #2030 · samuelclay/NewsBlur · GitHub](https://github.com/samuelclay/NewsBlur/pull/2030)


1 Like


[bitprophet](https://forum.newsblur.com/u/bitprophet)


January 17, 2026, 12:23am 4


Thanks for launching this with the ability to turn the button off! Hopefully that heads off the worst of the vitriol…


I’m on Team No Slop, Please myself – but as long as you don’t end up running NB into the ground in order to facilitate this feature area (which seems unlikely, obviously) then eh, whatevs.


As always, thanks for NewsBlur!


2 Likes


[bogorad](https://forum.newsblur.com/u/bogorad)


January 17, 2026, 11:32am 5


A for effort! Otherwise … I’ll keep using[my plugin](https://github.com/bogorad/openrouter-summarizer) . With an API key from google’s ai-studio, the usage can be literally FREE (although I currently prefer GPT-5.2-codex as the best summary engine; I wrote[a compasiron runner](https://github.com/bogorad/llm-summary-compare) ; and run it periodically to make sure I’m using the best model).


1 Like
