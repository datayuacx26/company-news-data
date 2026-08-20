---
schema_version: "1.0.0"
document_id: "fb67bb31bb1cc3b0b912f82a4e2f5ca0c0acffffd3c69e3b2f02b7ff9cc5002c"
company_key: "yc-pinch"
company: "Pinch"
source_id: "yc-pinch-news-import-5efd69a4f207"
canonical_url: "https://startpinch.com/research/en/speech_translation_benchmark"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-23T20:26:36.653996+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:4e391a950edc0261c4f083b0366c2ae13569c9abf6af88cdfe985374ec23e2de"
---

# Speech Translation Benchmark

A five-metric benchmark of real-time speech translation systems, including DeepL, Soniox, GPT-RT, Hibiki, Palabra, and Pinch’s Relay-1.


## Intro


Real-time speech translation is hard, which makes good evaluation essential when choosing an API to build into your service. At Pinch we treat benchmarks as directional: they show us what’s working and where we still need to improve.


A real-time system has to balance latency against correctness, since improving one usually costs the other in a live conversation, whether in a meeting app or in person. Just as important is how natural the output sounds: the translation should blend into the conversation, not add friction. So evaluating a speech-to-speech system means looking at several angles at once, balancing speed, correctness, and naturalness against how well it preserves each speaker’s voice.


In this blog post, we explore the following metrics:


- **Translation quality:** COMET score comparing the system’s translated text against a human reference translation (` XCOMET-XL` ).
- **Intelligibility:** word error rate on the translated speech output against the system’s text (` ASR-WER` ).
- **Naturalness:** perceived quality of the output speech, scored by` UTMOSv2` .
- **Latency:** median time from input to first audio chunk out (` TTFA p50` ).
- **Speaker similarity:** cosine similarity between source and output speaker embeddings, computed with` WavLM` .


## Translation quality


Translation quality measures whether the system actually conveyed what the speaker said. Intelligibility (next section) tells us the output is understandable. A system can synthesize crystal-clear audio of a wrong translation, and the listener walks away confidently misinformed.


We measure translation quality with` XCOMET-XL` : we take the system’s translated text output and score it against a reference translation, using a learned metric trained to correlate with human judgments of translation adequacy. Unlike metrics like BLEU that compare surface word overlap, COMET-family scores reason about meaning, so they handle paraphrases and word-order differences without unfairly penalizing them.


## Intelligibility


Intelligibility measures whether the translated speech can be understood as words at all. If a listener can’t make out the words, the quality of the underlying translation is irrelevant.


We measure intelligibility with` ASR-WER` : we transcribe the system’s output audio with an ASR model (` whisper-large-v3` ) and compute the word error rate against the system’s own translated text, i.e. the input to its TTS. A low WER means the synthesized speech is clear enough that a downstream listener (human or machine) recovers the intended words. This metric isolates a class of TTS failures that quality scores like COMET can’t see, since COMET never hears the audio: dropped words, repeated syllables, or artifacts that mangle an otherwise-correct translation.


## Naturalness


Naturalness measures how pleasant and human-like the synthesized speech sounds. A system can be intelligible and accurate but still have robotic prosody, unnatural pauses, or a monotone delivery, all of which add cognitive load and wear on the listener over a long conversation.


We measure naturalness with` UTMOSv2` , a learned model trained to predict the mean opinion score (MOS) a panel of human listeners would give.` UTMOSv2` isn’t a perfect substitute for human evaluation (no automatic metric is), but it correlates well with listener judgments and scales to thousands of comparisons without a rater pool.


## Latency


Latency measures how long the system takes to start producing output after the speaker begins. In real-time translation this shapes whether the conversation works at all: if the translated audio arrives too late, speakers talk over each other, lose their turn, or stop trusting the system.


We measure latency with` TTFA p50` : the median wall-clock time from input audio reaching the system to the first chunk of translated audio coming back. We use the median rather than the mean to stay robust to outliers.


## Speaker similarity


Speaker similarity measures how closely the translated speech preserves the voice characteristics of the original speaker. In a multilingual meeting, listeners track who is talking by voice. If every translated speaker sounds the same, that signal disappears, and the conversation gets harder to follow even when the words are correct.


We measure speaker similarity with` WavLM` : we extract speaker embeddings from the source and translated audio using` WavLM` fine-tuned for speaker verification (` microsoft/wavlm-base-plus-sv` ), then compute the cosine similarity between them. A higher score means the output voice sits closer to the source in the embedding space. This metric is most meaningful for systems that attempt voice preservation (voice cloning).


## Benchmark Results


We ran the benchmark across a set of real-time speech translation systems available today, evaluating each on identical audio clips and reference translations with the five metrics defined above. The point isn’t a single ranking: it’s to show where each system sits across the five axes and where the real tradeoffs are.


The dataset is` kyutai/Audio-NTREX-4L` , a long-form multilingual speech translation set covering French, Spanish, Portuguese, and German into English, designed to evaluate speech translation models on multi-sentence utterances. The dataset is publicly available on[Hugging Face](https://huggingface.co/datasets/kyutai/Audio-NTREX-4L) .


In this benchmark we tested seven real-time speech translation systems. All models are accessed via their APIs, with the exception of Hibiki, which we host locally using the open weights.


- ` Soniox` , via the Soniox API ([reference](https://developers.deepl.com/api-reference/voice) )
- ` DeepL` , via the DeepL Voice API ([reference](https://developers.deepl.com/docs/getting-started/quickstart) )
- ` GPT-RT` , OpenAI’s gpt-realtime-translate ([reference](https://developers.openai.com/api/docs/models/gpt-realtime-translate) )
- ` Hibiki` , Kyutai’s open model ([reference](https://kyutai.org/blog/2026-02-12-hibiki-zero) )
- ` Palabra` , via the Palabra API ([reference](https://docs.palabra.ai/docs/quick-start) )
- ` Gemini` , via the Gemini 3.5 Live Translate API ([reference](https://ai.google.dev/gemini-api/docs/models/gemini-3.5-live-translate-preview) )
- ` Relay-1` , our own system, via the Pinch API ([reference](https://startpinch.com/docs/api-reference) )


The table below reports all five metrics for each system; arrows indicate whether higher or lower is better.


Metric Soniox DeepL GPT-RT Hibiki Palabra Gemini Relay-1


Translation quality ↑ 0.81 **0.84** 0.73 0.65 0.68 0.78 0.77


Intelligibility WER ↓ NA NA 0.11 0.07 **0.03** **0.03** 0.06


Naturalness ↑ NA NA 2.89 2.06 2.49 **3.64** 3.27


Latency ↓ NA NA **2289** 2434 7297 3269 3201


Speaker similarity ↑ NA NA 0.78 **0.96** 0.46 0.70 0.46


### Metrics breakdown


Alongside the five headline metrics, the tables below report a few auxiliary metrics that give a second reading on translation quality and latency. These aren’t our primary signals, but they make the comparison more robust and easier to check against numbers reported elsewhere.


-


Translation quality — COMET-DA: A reference-based neural score from the COMET family, trained to predict human Direct Assessment ratings. It returns a single regression score for adequacy, where XCOMET-XL adds fine-grained error-span detection on top. We report it as a second learned-metric view on the same axis.


-


Translation quality — BLEU: The classic n-gram overlap between the system’s translated text and the reference. BLEU rewards surface word matches, so unlike the COMET metrics it penalizes valid paraphrases and word-order changes. We include it as a familiar, interpretable baseline.


-


Latency — LongYAAL CU: A long-form latency metric from the average-lagging family (YAAL, extended to unsegmented audio). It measures the average delay behind an ideal emission policy: how long a listener waits for a piece of source content to surface in the translation across the whole stream. CU is the computation-unaware variant, which scores lag on the idealized schedule and ignores model compute time. It complements TTFA p50: TTFA is the wall-clock time to the first audio chunk, while LongYAAL CU captures sustained lag across the full utterance.


#### Text output systems


Metric Soniox DeepL


Translation quality — XCOMET-XL ↑ 0.81 **0.84**


Translation quality — COMET-DA ↑ 0.83 **0.85**


Translation quality — BLEU, norm ↑ 31.37 **36.22**


Latency — LongYAAL CU (ms) ↓ **1965** 2179


#### Text and Speech output systems


Metric GPT-RT Hibiki Palabra Gemini Relay-1


Translation quality — XCOMET-XL ↑ 0.73 0.65 0.68 **0.78** 0.77


Translation quality — COMET-DA ↑ 0.80 0.76 0.76 0.82 0.81


Translation quality — BLEU, norm ↑ **32.55** 30.06 29.20 31.31 30.08


Latency — LongYAAL CU (ms) ↓ 3136 3163 3823 **1839** 2877


Latency — TTFA p50 (ms) ↓ **2289** 2434 7297 3269 3201


Intelligibility — ASR-WER ↓ 0.11 0.07 **0.03** **0.03** 0.06


Naturalness — UTMOS ↑ 2.89 2.06 2.49 **3.64** 3.27


Speaker similarity — WavLM ↑ 0.78 **0.96** 0.46 0.70 0.46


## Conclusion


No system wins on every axis, which is the expected outcome for a problem that trades speed against correctness. The results sort into two groups with different jobs.


The text-output systems, Soniox and DeepL, lead on translation quality, with DeepL strongest at 0.84 XCOMET-XL and the better COMET-DA and BLEU scores. They also post the lowest computation-unaware latency. But they stop at text, so intelligibility, naturalness, and speaker similarity don’t apply. If your product only needs translated captions, these are the systems to beat on quality.


Among the speech-output systems the tradeoffs are sharper. Gemini is the most rounded: top naturalness (3.64), tied-best intelligibility (0.03 WER), and the highest translation quality in the group (0.78). GPT-RT has the lowest first-audio latency (2289 ms) but trails on naturalness and intelligibility. Hibiki preserves the source voice almost perfectly (0.96 speaker similarity) while scoring lowest on translation quality and naturalness. Palabra reaches the best intelligibility but pays with latency over 7 seconds, which is hard to sustain in a live conversation.


Relay-1 sits in the top group on the axes that shape a live exchange: second on naturalness (3.27), within 0.01 of Gemini on translation quality (0.77 vs 0.78), and competitive on intelligibility (0.06 WER) at mid-pack latency (3201 ms). It does not clone the source voice, which is why its speaker-similarity score (0.46) lines up with Palabra rather than Hibiki. For a meeting or conversation app where clear, natural, accurate output matters more than reproducing each speaker’s timbre, that’s the right set of tradeoffs to optimize for.


These numbers are a snapshot. The systems keep moving, the dataset covers four language pairs into English, and a different workload would reshuffle the rankings. We publish them to mark where Relay-1 stands today and where we go next.


## References


-


Radford et al. (2022). *Robust Speech Recognition via Large-Scale Weak Supervision* . arXiv:[2212.04356](https://arxiv.org/abs/2212.04356) . Code and models:[github.com/openai/whisper](https://github.com/openai/whisper) .


-


Guerreiro et al. (2024). *xCOMET: Transparent Machine Translation Evaluation through Fine-grained Error Detection* . TACL. arXiv:[2310.10482](https://arxiv.org/abs/2310.10482) . Model:[huggingface.co/Unbabel/XCOMET-XL](https://huggingface.co/Unbabel/XCOMET-XL) .


-


Baba, Nakata, Saito, Saruwatari (2024). *The T05 System for the VoiceMOS Challenge 2024: Transfer Learning from Deep Image Classifier to Naturalness MOS Prediction of High-Quality Synthetic Speech* . IEEE SLT 2024. arXiv:[2409.09305](https://arxiv.org/abs/2409.09305) . Code:[github.com/sarulab-speech/UTMOSv2](https://github.com/sarulab-speech/UTMOSv2) .


-


Chen et al. (2021). *WavLM: Large-Scale Self-Supervised Pre-Training for Full Stack Speech Processing* . arXiv:[2110.13900](https://arxiv.org/abs/2110.13900) . Model:[microsoft/wavlm-base-plus-sv](https://huggingface.co/microsoft/wavlm-base-plus-sv) .


-


Labiausse et al. (2026). *Simultaneous speech-to-speech translation without aligned data* . arXiv preprint arXiv:[2602.11072](https://arxiv.org/abs/2602.11072v1) . Dataset:[kyutai/Audio-NTREX-4L](https://huggingface.co/datasets/kyutai/Audio-NTREX-4L) .


-


Polák et al. (2025). Better Late Than Never: Meta-Evaluation of Latency Metrics for Simultaneous Speech-to-Text Translation. arXiv:[2509.17349](https://arxiv.org/abs/2509.17349)
