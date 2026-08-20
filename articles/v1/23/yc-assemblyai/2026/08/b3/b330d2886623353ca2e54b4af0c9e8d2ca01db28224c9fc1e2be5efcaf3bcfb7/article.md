---
schema_version: "1.0.0"
document_id: "b330d2886623353ca2e54b4af0c9e8d2ca01db28224c9fc1e2be5efcaf3bcfb7"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c3d7277c32d9"
canonical_url: "https://www.assemblyai.com/blog/contextual-awareness-in-universal-3-5-pro-realtime"
published_at: null
first_seen_at: "2026-08-19T11:49:15.223909+00:00"
fetched_at: "2026-08-19T11:49:16.837282+00:00"
content_hash: "sha256:8177784a052b72d953694d9e94c3738c28b947f5a3174fce61d2e9b663c5c8cf"
---

# Contextual Awareness in Universal-3.5 Pro Realtime

# Introducing Contextual Awareness in Universal-3.5 Pro Realtime


Speech-to-text models have historically treated every utterance in isolation. They hear audio, they transcribe audio, and they forget everything the moment the next chunk arrives. But real conversations don't work that way—and neither should transcription.


With Universal-3.5 Pro Realtime, we've made a fundamental leap in contextual awareness. The model can now use information about *what* is being discussed, *who* is speaking, and *what was said moments ago* to dramatically improve transcription accuracy—especially in the noisy, unpredictable conditions where voice agents actually operate.


There are two major capabilities behind this: **contextual prompting** and **conversation context** (also called agent context). Let's walk through both.


## Contextual prompting: tell the model what it's listening to


Universal-3.5 Pro Realtime now accepts a natural-language prompt describing the audio content—and uses it to improve accuracy within that domain.


The key insight: **the more specific your prompt, the better the accuracy.** Consider three prompts for the same medical call, in increasing order of detail:


1. ` Medical consultation call`
2. ` Cardiology consultation about chest pain symptoms`
3. ` Cardiology consultation between Dr. Smith and elderly patient regarding chest pain, ECG results, and medication adjustment for hypertension`


Each level of specificity gives the model more information to make accurate predictions about what's actually being spoken. A vague prompt helps; a detailed one helps a lot more.


### Key terms, now with context


Key terms prompting has long been one of the most effective ways to boost accuracy on domain-specific vocabulary—product names, people's names, industry jargon. Pass the term, and the model boosts its likelihood of transcribing it correctly. It works extremely well.


But key terms alone are devoid of context. If you add` Klebanoff` as a key term, the model knows that terminology will appear in the audio—but it doesn't know whether it's a person's name, a product, or a company. That means it can misfire on acoustically similar phrases. Say something like *"take the club and cough"* —semantically close to "Klebanoff"—and a context-free key term can get incorrectly applied.


With contextual prompting, you can now tell the model what the key term actually *is* :


` The user's name is Zachary Klebanoff.`


Now the model transcribes "My name is Zachary Klebanoff" correctly—and when someone says "take the club and cough," it doesn't blanket-apply the key term despite the acoustic similarity, because it understands the context in which the term should appear.


### Dynamic prompt updates, mid-call


Here's where it gets powerful for real-time applications: **you can update the prompt mid-stream through the API.**


Imagine a voice agent handling customer service calls for a bike shop. Your initial prompt might be:


` Voice agent transcribing bike shop customer service calls.`


Then Lance Armstrong calls in and says his bike tire popped. Your application can make a tool call mid-conversation and update the prompt to:


` The caller's name is Lance Armstrong and his bike tire is popped.`


The model now has fresh, caller-specific context to transcribe the rest of the conversation more accurately—all without interrupting the stream. Prompts stop being static configuration and become a live channel for feeding your model everything your application learns as the call unfolds.


## Conversation context: the model remembers the conversation


The second major capability is **conversation context** (or agent context): the model now retains previous speech-to-text transcriptions within the session, and you can also pass in spoken messages from the other side of the conversation—like a voice agent's LLM-generated TTS responses.


Why does this matter? Because conversations are predictable in ways single utterances aren't:


- If the previous turn was *"What is your email?"* —the model now expects an email address next.
- If the previous turn was *"Would you like to make that a large?"* —the model expects a yes/no response.


Importantly, **the model doesn't over-bias on this context.** If the agent asks for an email and the caller says something completely different, the model handles it just fine. Think of it the way a human listens: if I ask for your email, I *expect* an email—but I can still understand you if you change the subject. The context is a nudge, not a constraint.


Speech-to-text transcripts are retained in the model session automatically. For voice agent use cases, you can additionally pass your agent's LLM-generated responses directly to the model, giving it the full two-sided picture of the conversation.


### The results


On voice agent datasets, we've measured a significant reduction in word error rate with agent context enabled—and we're continuing to train the model to leverage this context even more effectively, so expect these numbers to keep improving.


## Where this shines: bad audio, real conversations


The most striking demonstration of these features is in poor audio conditions. When the signal degrades, a context-free model has little to fall back on. A context-aware model can lean on the situational logic of the conversation.


In a food-ordering demo—with deliberately terrible audio—the model correctly transcribed menu items like "kelp shake" because the prompt included the menu and the agent context established what was being ordered.


In a more intensive customer-support scenario, the full toolkit came together:


1. **Initial prompt:**` You are an audio transcriptionist for ZeroCorp.`
2. The caller identifies herself as **Dara Okafor** from the Northwind account → a **dynamic prompt update** adds` The caller's name is Dara Okafor.`
3. A background **tool call** looks up her account and discovers she recently purchased a product called **Ciro Flow** → the configuration updates again, so the model expects that product name.
4. The agent asks for her email → **conversation context** primes the model, and` dara.okafor@northwind.com` is transcribed cleanly.
5. When she mentions the "Ciro Flow Halo tier and the Vantix add-on," the model nails product names it would otherwise have no way of knowing—because the application fed it that knowledge mid-call.


Every piece—contextual prompting, key terms with context, dynamic updates, and conversation memory—compounds to keep transcription accurate through a realistic, messy, multi-turn call.


## See it in action


Watch this walkthrough as a video on YouTube.


## Getting started


Contextual prompting is available today in our playground and API, and conversation context (including agent context for LLM-generated responses) is available through the API for streaming use cases. Dynamic mid-call prompt updates are API-only for now.


If you're building voice agents, this combination of contextual prompting and context carryover is designed for you: your application already knows who's calling, what they bought, and what the agent just said. Now your speech-to-text model can know it too.


Questions, or interested in trying it out? Reach out to our team—we'd love to hear what you build.
