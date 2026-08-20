---
schema_version: "1.0.0"
document_id: "7c0b3c7d740e0bb1ebb96442260886ce73f1daa46c67f4e38e7d84c92273e183"
company_key: "yc-retell-ai"
company: "Retell AI"
source_id: "yc-retell-ai-news-import-48ab15cc20a2"
canonical_url: "https://www.retellai.com/blog/retell-asr"
published_at: "2025-12-15T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:02.485527+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:cb255fb4af59b49fb65b43985884be49a10db5957e6f176cbb0cad065385bacd"
---

# Smarter Understanding, Clearer Calls: Retell’s ASR Just Got a Major Upgrade

At the core of understanding people correctly and having natural conversations lies the role of automatic speech recognition (ASR), which is critical for any[AI call bot](https://www.retellai.com/blog/ai-call-bots) .


Telephonic conversations are inherently challenging from a speech recognition perspective. Poor connectivity, background noise, and various dialects and accents make understanding a caller’s words difficult.


That’s why we’ve rolled out a major upgrade to Retell’s Automatic Speech Recognition (ASR) engine, bringing sharper transcription, stronger intent detection, and more reliable call outcomes across seven widely used languages.


This means better accuracy, crystal-clear transcripts, and higher call completion rates.


## What’s New in Retell’s Upgraded ASR Engine


Retell’s new ASR (text-to-speech) now supports ***22+ new languages*** including:


- Afrikaans
- Arabic
- Azerbaijani
- Bosnian
- Welsh
- Persian
- Filipino
- Galician
- Hebrew
- Croatian
- Armenian
- Icelandic
- Kazakh
- Kannada
- Macedonian
- Marathi
- Nepali
- Slovenian
- Serbian
- Swahili
- Tamil
- Urdu


This brings our total language count to 50+, bringing us one step closer to making content accessible in any language. The addition of these languages opens up vast possibilities for businesses to reach new audiences; Arabic alone is spoken by 450 million, Persian 130 million and Urdu 250 million.


These languages are made available as part of our new ASR. You can try out OpenAI TTS to build your next[voice agent](https://www.retellai.com/blog/connect-any-ai-voice-agent-to-mcp-with-retell-ai-mcp-node) with Retell. You can also clone your voice and convert to any of our 50+ languages.


This extensive language support enables businesses to effectively engage with a global audience.[Try Retell AI today and see it in action.](https://www.google.com/url?q=https://dashboard.retellai.com/?_gl%3D1*jkcbjo*_gcl_au*MTQzMzY2MjcwLjE3NjIzMjA1MDA.&sa=D&source=editors&ust=1765736253502429&usg=AOvVaw2yqI66rkIbdTzli0Eb5uyh)


### How to Use Retell’s Updated Languages?


Enhancing your AI bot's ability to communicate in multiple languages is a powerful way to improve user experience.


With Retell AI, you can enable this multilingual capabilities in a few simple steps:


### Step 1: Access the Global Settings


Navigate to the Agent Dashboard and select the bot you want to configure. Click on the Global Settings menu on the right-hand side.


### Step 2: Select Voice and Language


In the Voice and Language section under Global Settings, click the dropdown menu to explore available languages.


Choose the desired language for your bot. For example, selecting Spanish (Latin America) will apply this voice and language setting to the bot.


### Step 3: Customize Conversation Flow


After selecting the language, return to the Conversation Flow editor and ensure all messages are accurately translated for the target audience.


For example, in the Greetings node, the bot might say:


*“Hola, soy Anna, una representante de inteligencia artificial que llama desde la organización Retell Healthcare en una línea grabada…” (when Spanish is selected).*


Confirm that every conversation node—including user prompts and responses—consistently matches the selected language.


These multilingual voice flows can also be set up within an AI-powered IVR system, enabling callers to navigate menus and reach the appropriate department in their preferred language.


### Step 4: Test the Multilingual Capability


Use the Test option in Global Settings to simulate a conversation and verify that the bot responds smoothly in the selected language. Review both voice and text outputs to ensure accuracy and consistency.


This configuration can also support advanced use cases, such as an AI appointment setter, where the bot confirms dates, times, and other details while naturally speaking the customer’s preferred language.


Tips for an Effective Multilingual Setup


- **Voice Selection:** Choose a voice that aligns with the audience’s region to improve familiarity and engagement.


- **Multilingual Mode:** In regions with bilingual or multilingual users, enable multilingual mode so the bot can switch between languages seamlessly.


By following these best practices, your AI bot can communicate clearly with a broader audience, improving accessibility and delivering a more inclusive customer experience.


## How This New ASR Works In Retell?


Real-time transcription is often a tradeoff between latency and accuracy.


When you optimize for speed, you get the lowest latency but a higher chance of errors due to less context. When relying on results with more context, you risk waiting longer after the user stops speaking.


Retell offers two transcription models:


- Optimize for speed (Fast & Accurate modes)


- Optimize for accuracy (Accurate mode)


Even though we've found that the optimize for speed mode and optimize for accuracy mode have similar WER (Word Error Rate). The real difference lies in the slightest details like number, date, or address.


By optimizing our acoustic modeling pipeline, refining language-specific phonetic dictionaries, and improving real-time decoding, Retell now delivers dramatically lower Word Error Rates (WER) in both Accurate mode and Fast & Accurate modes.


### Sharper Accuracy in European Languages (Accurate Mode)


For German, French, Italian, and Polish, we cut Word Error Rate by 7–10 points.


These were already strong languages in our Accurate mode. Still, the new modeling architecture significantly reduces the standard error types we observed in real customer calls, like accent-driven phoneme swaps, background-noise distortions, and gender/number agreement mistakes.


#### What this means for voice automation:


- Clearer transcripts even in noisy environments


- More reliable intent capture for complex, multi-sentence responses


- Fewer repair turns (“Sorry, could you repeat that?”)


- Higher first-call resolution for support, sales, and service workflows


Language Word-Average WER Call-Average WER What This Improvement Means


German 0.1944 0.1971 Misheard consonants and accent variance errors drop noticeably.


French 0.2665 0.2552 Reduces noise sensitivity and improves handling of liaison and nasal vowels.


Italian 0.1781 0.2457 Smoother, natural-sounding call transcripts.


Polish 0.1733 0.1688 Better recognition of consonant clusters and inflections.


### Major Breakthroughs in Asian Languages (Fast & Accurate Modes)


For Chinese (Mandarin), Malay, and Hindi, the gains are even bigger: WER improvements of 15–25 points.


These languages have historically been challenging for ASR due to tonal dynamics (Mandarin), code-mixing (Malay), and accent diversity (Hindi). The upgraded engine now handles these complexities far more intelligently.


#### What this unlocks:


- Significantly better real-time understanding even at Fast mode speeds


- Accurate recognition of tonal changes, dialect variations, and mixed-language usage


- More natural turn-taking without delays or misinterpretations


- Stronger performance in call-center-style phone audio — the toughest ASR environment


Language Word-Average WER Call-Average WER What This Improvement Means


Malay 0.2623 0.2988 Fewer tone-confusion errors and better handling of rapid speech.


Hindi 0.3010 0.3150 Big gains in code-mixed speech (Malay + English), with better real-time clarity.


Mandarin 0.2605 0.2636 Drastically improving call transcription stability across accents.


## What This Means: Cleaner Input → Smarter Output


The new ASR engine reduces the mismatch between what callers *say* and what the AI *thinks they said* . With lower WER, our LLM-powered reasoning engine receives clearer text, enabling:


- More precise intent recognition
- Fewer conversational breakdowns
- Smoother, faster resolutions
- More human-like agent behavior


This upgrade doesn’t just improve transcription — it elevates the entire[voice automation](https://www.retellai.com/blog/best-call-center-voice-ai-tools-for-automating-conversations) experience.


‍
