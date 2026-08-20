---
schema_version: "1.0.0"
document_id: "18a8e56a7d94d60dfaab4884436c2c88601caf4c0a15ca6947afe183b3973ed1"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/announcing-universal-1-speech-recognition-model"
published_at: null
first_seen_at: "2026-07-22T08:04:49.593567+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:e797f1bfe1d4c5f766d34a5b124dd24cc5045112a7738f30c7f29950d26233cb"
---

# Introducing Universal-1

Today, AssemblyAI is launching[Universal-1](https://www.assemblyai.com/research/universal-1?ref=assemblyai.com) , our most capable and highly trained speech recognition model. Trained on over 12.5 million hours of multilingual audio data, Universal-1 achieves best-in-class speech-to-text accuracy, reduces word error rate and hallucinations, improves timestamp estimation, and helps us continue to raise the bar as the industry-leading Voice AI provider.


Universal-1 is trained on four major languages: English, Spanish, French, and German, and shows extremely strong speech-to-text accuracy in almost all conditions, including heavy background noise, accented speech, natural conversations, and changes in language, while achieving fast turn-around time and improved timestamp accuracy.


This is a chart of the Average Word Error Rate (lower WER is better) of AssemblyAI's Universal-1 model compared to Deepgram Nova-2, OpenAI Whisper Large-v3, Amazon, Microsoft Azure Batch v3.1, Google Latest-long by Language (English, Spanish, German, French).


In the last few years we've seen an explosion of audio data available online. This coupled with advances in AI technology have allowed organizations to unlock the value of voice data in ways that were previously impossible. As a result, organizations are building new products, services, and capabilities that serve millions of people around the world. By building on AssemblyAI’s Voice AI models, customers have built products that can summarize video calls with clear notes and action items, automate customer service experiences and help organizations understand the voice of their customers with insights from every customer interaction, and create apps that help teachers guide students more effectively as they learn to read.


With Universal-1 we sought to build on the industry-leading performance of our previous models, and designed this new model guided by the idea that accuracy of every word matters. In conversations with customers, it was clear that there was a need in the industry for a model that focused on the nuances of spoken language across accents, tone, dialect, faithfulness, and more. We hope the new capabilities of Universal-1 will help power the next generation of AI products and features built with voice data.


Accuracy is paramount when deciding which speech-to-text model to implement. AssemblyAI's Automatic Speech Recognition (ASR) model is best-in-class, and we are beneficiaries of the constant improvements they implement, like Universal-1. We provide lead intelligence to over 200,000 small businesses. If the transcriptions are not accurate, then the downstream intelligence our customers depend on will also be subpar — garbage in, garbage out.


Ryan Johnson, Chief Product Officer, CallRail


## Universal-1 ASR: Pushing the Boundaries of Voice AI


[Universal-1](https://www.assemblyai.com/research/universal-1?ref=assemblyai.com) accomplishes the following improvements:


**Accurate and robust multilingual speech-to-text**
Universal-1 represents another major milestone in our mission to provide accurate, faithful, and robust speech-to-text capabilities for multiple languages, helping our customers and developers worldwide build various Voice AI applications.


- Universal-1 achieves 10% or greater improvement in English, Spanish, and German speech-to-text accuracy, compared to the next-best commercial speech-to-text system we tested.
- Universal-1 reduces hallucination rate by 30% over a widely used open-source model, Whisper Large-v3, providing users with confidence in the results we deliver.
- Humans prefer the outputs from Universal-1 over Conformer-2, our previous generation model, 71% of the time when they have a preference.
- Universal-1 exhibits the ability to code switch, transcribing multiple languages within a single audio file.


This is a chart of consecutive error types per audio hour for AssemblyAI Universal-1 compared to NVIDIA Canary-1B, OpenAI Whisper Large-v3


**Precise timestamp estimation**
Word-level timestamps are essential for various downstream applications, such as audio and video editing. In conversation analytics and meeting transcription, accurate timestamps are crucial to enable speaker diarization to align speaker labels with recognized words.


- Word-level timestamps are essential for various downstream applications, such as audio and video editing as well as conversation analytics.
- Universal-1 improves our timestamp accuracy by 13% relative to Conformer-2.
- The improvement in timestamp estimation results in a positive impact on speaker diarization, improving concatenated minimum-permutation word error rate (cpWER) by 14% and speaker count estimation accuracy by 71% compared to Conformer-2.


**Efficient parallel inference**


- Effective parallelization during inference is crucial to achieve very low turnaround processing time for long audio files.
- Universal-1 achieves a 5x speed-up compared to a fast and batch-enabled implementation of Whisper Large-v3 on the same hardware.


## See it in action


Paul. It's okay. I'm here. I'm here. It's been a while since you've had one of those nightmares. Tell me, what was it about? It's only fragments. Nothing's clear. You've been fighting the Harkonnens for decades. Load. My family's been fighting them for centuries. Your blood comes from dukes and great houses. Here, we're equal. What we do, we do for the benefit of all. Well, I'd very much like to be equal to you. Maybe I'll show you the way. Deal with this prophet. Send assassins. Theodorother, he's psychotic. I see possible futures all at once. And in so many futures, our enemies prevail. But I do see a way. There is a narrow way through. My allegiance is to you. Do you believe me? This is a form of power that our world has not yet seen. The ultimate power. I want you to know I will love you as long as I breathe. You will never lose me as long as you stay who you are. Consider what you're about to do, Paul Atreides. Silence. This prophecy is how they enslave us. Journey. You are not prepared for what is done to come.


Entonces le digo yo a Martínez, Martínez, espérame right here cinco minutes que yo tengo que ir al toilet. Pero hay no idea lo que me iba a encontrar yo en ese toilet. Oye, te mando mamá, you cooking for me the sunny side up cuando tú sabes que a mí me gusta scramble. Emilito. ¿Number one, who told you que esto es para ti? En number dos, lo primero que tú dices en mi cocina es good morning. Ah, good morning, mami. Pues good morning, mamá. Good morning, mija. Así que no estoy en el toilet doing my business cuando escucho una woman screaming from el toilet de Alao. Mamá Sonny, side up for me, please. Sony, side up. Pero ya tú no eres vegetarian. No more lacto. Y aquí podemos ver a mi older sister que todos los días está cambiando el diet pensando que le estaban haciendo daño y boom. I can't believe my eyeball. Mami. El jefe Kissing in the mouth con Missy Martinez. Oh, my God. ¿Oye, quién me ayuda con algo de mi Instagram? I can't figure it out. Dame acá. Abuelita. ¿What is it? ¿Carolina? That's too la baby. Baja volumen, mi amor. Yo sospechaba algo porque ese jefe Eli's grabbing and touching all the girls en la oficina. Emilio, Mrs. Martinez no es ninguna santa, you know. Mamá, tú no puedes estar comiendo tu chorizo every morning. Habías hecho cáncer de colon. Emilio, sé something. ¿What? ¿Cómo que Emilio? ¿Qué falta de respeto es esa? You call me dad. ¿Abuelita, how? ¿Cómo es que tú tienes 100 likes en esta foto? Esa es mi people from bingo. Ay, my salud de colon ideal. So por favor, min, your own business. Carolina de volume. Wow, abuelita, tú eres una rockstar. ¿Can you like my post emily to bless the table? Yo bendije ayer, papá. Den tu lilianita. Thank you for all this comida que tu pones en nuestra family table. Bless the hands que prepararon la comida. Perdónanos por comer dis baby chicken huevos and forgive my papá Emilio for being so gossipy and chismoso. Amén. Amén. No, no, no, no puedo tomar café. No te hagas el sentido. No, no, no.


My name is Angelica Skyler Alexander Hamilton. Where's your family from? Unimportant. There's a million things I haven't. Just you wait. Just you wait. So this is what it feels like to match wit for someone at your level. What the hell is the catch? It's the feeling of freedom. Of seeing the light is Ben Franklin with the key and a kite. You see it, right? The conversation lasted two minutes, maybe three minutes. Everything we said in total agreement. It's the dream and it's a bit of a dance, a bit of a posture. It's a bit of a stance. He's a bit of a flirt. But I'm gonna give it a chance. I asked about his family. Did you see his answer? His hands started fidgeting. He looked askance. He's penniless. He's flying by the seat of his pants. Handsome boy, does he know it. Peach fuzz. Then he can't even grow it. Want to take him far away from this place? Then I turn and see my sister's face. And she is helpless. And I know she is helpless. And her eyes are just helpless. And I realize three fundamental truths at the exact same time.


Universal-1’s training data far exceeds the training data used for most existing speech-to-text models. This training data includes audio from non-native speakers, audio with heavy background noise, conversations involving multiple talkers held in various domains and settings, to better simulate how speech happens in the real world. Universal-1 also builds on our predecessor models, Conformer-1 and Conformer-2, to capture proper nouns and alphanumeric details with high accuracy.


We’re excited to see the impact that Universal-1 has on applications like:


- Conversational intelligence platforms that are now able to analyze vast amounts of customer data quickly, accurately, and reliably in order to surface critical voice of customer insights and analytics regardless of accent, recording condition, number of speakers, and more.
- AI notetakers that can now generate highly accurate and hallucination-free meeting notes to serve as the basis for LLM-powered summaries, action items, and other metadata generation with accurate proper noun, speaker, and timing information included.
- Creator tool applications that are now able to build AI-powered video editing workflows for their end-users leveraging precise speech-to-text outputs in multiple languages with low error rates and reliable word timing information.
- Telehealth platforms automating clinical note entry and claims submission processes with a high success rate leveraging accurate and faithful speech-to-text outputs, including rare words like prescription names and medical diagnoses, in adversarial and far field recording conditions.


## Improving the accuracy of Voice AI across languages


Trained on English, Spanish, German, and French data, Universal-1 is built to support the languages most often used by our customers and their end-users.


Today, Universal-1 is available in English & Spanish, with German and French being made available shortly. We will be adding additional language support within future Universal models over time.


## Best & Nano ASR Tiers: More Options to Build with AssemblyAI


Today, we’re also introducing our Best and Nano tiers to give you more options when building with Voice AI models from AssemblyAI depending on your budget, accuracy needs, and use case.


At AssemblyAI, we use a combination of models to produce your results. Our Best tier will house our most powerful and accurate models, including Universal-1. This tier is best suited for use cases where accuracy is paramount, and end-users will interact directly with the results generated from our models.


We are also introducing a Nano tier—a lightweight lower cost speech-to-text option available in many languages. Nano is best suited for use cases like search and topic detection or for use cases where accuracy is not paramount.


## What Comes Next for Universal-1


[Universal-1 is available via our API](https://www.assemblyai.com/dashboard/signup?ref=assemblyai.com) , and you can start building on it today. We’ll continue to improve our Voice AI models over time, so stay tuned for updates as we add new capabilities and languages to Universal-1.
