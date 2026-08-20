---
schema_version: "1.0.0"
document_id: "1c0c1d67aa3c5cc3b86f18df2f2539bfff834b37227b0f77ef34f5346892fea8"
company_key: "yc-alex"
company: "Alex"
source_id: "yc-alex-news-import-06f85188604b"
canonical_url: "https://www.alexcodes.app/blog/vibe-cloning-cal-ai"
published_at: "2025-06-17T00:00:00+00:00"
first_seen_at: "2026-07-21T05:43:46.556958+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:3b345ae6eab771d896ac9fb10c70a4b79e51bcfeb3668c1a18b39fa2a69f4b5f"
---

# Vibe-Cloning Cal AI In A Couple Hours With Alex

(written by the intern)


[Cal AI](https://calai.app/) 's pretty cool. It's an app that uses AI to estimate the calories and macronutrients (carbs, fat, and protein) in your meals—just take a photo and it figures out the amounts and logs them for you.


I'm too lazy to estimate my meal's calories every time, I'd rather have AI do it.


Only issue is that Cal AI's kinda expensive ($9.99 per month or $29.99 per year).


I estimated the API cost of a month of meal tracking with gpt-4o (accounting for system prompt, structured output prompt length, image input cost, and output cost) and it came out to $0.38.


So...clone time.


I'm also too lazy to write this clone myself, so I had Alex do it.


After opening Alex and Xcode,


I started with this prompt:


o3 one-shot most of it, but there were a few errors. I was also too lazy to fix those errors, so I just clicked


in Alex and had Sonnet 4 do it for me. After some more prompts:


and clicking that button a few more times, I ended up with this:


Works great, **26.3 times cheaper than Cal AI.**


I didn't write a single line of code.


If you want to try it, here's a complete prompt for o3 or Sonnet 4:


> Clone Cal AI (calorie, not calendar). Be fully functional. Track calories, carbs, fat, and protein. Allow for manual amount adding, but focus on the camera AI feature. Stick to system UI design. Follow best practices.
>
>
> Use the OpenAI API with SwiftOpenAI. Search up SwiftOpenAI, and implement structured output as shown in the GitHub README.
>
>
> Use SwiftData for storage of logs.


**Download Alex and give it a shot! 👆**


You can also check out the source code[here](https://www.alexcodes.app/blog/%22https://github.com/AlexSideBar/VibeCalAI%22) .
