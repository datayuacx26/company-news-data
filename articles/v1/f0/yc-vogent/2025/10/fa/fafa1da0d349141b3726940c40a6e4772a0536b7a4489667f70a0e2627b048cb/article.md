---
schema_version: "1.0.0"
document_id: "fafa1da0d349141b3726940c40a6e4772a0536b7a4489667f70a0e2627b048cb"
company_key: "yc-vogent"
company: "Vogent"
source_id: "yc-vogent-news-import-1ff82337d4a1"
canonical_url: "https://blog.vogent.ai/posts/voturn-80m-state-of-the-art-turn-detection-for-voice-agents"
published_at: "2025-10-17T00:40:13.565+00:00"
first_seen_at: "2026-07-22T19:07:51.499791+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:abae2ed8085b661ba66d18afe95a32a55ba727e9e1a7c3cd67f8a323e6844735"
---

# VoTurn-80M: State-of-the-Art Turn Detection for Voice Agents

[Repo](https://github.com/vogent/vogent-turn) ,[Model weights](http://huggingface.co/vogent/Vogent-Turn-80M) ,[Demo](https://huggingface.co/spaces/vogent/vogent-turn-demo)


# Understanding when to speak


What makes a conversation feel natural? Beyond the words we choose, it's the rhythm—knowing when to speak and when to listen. These split-second judgments happen effortlessly in human dialogue: we sense the difference between a thoughtful pause and a completed thought, between "I'm flying from San Francisco..." trailing off for more, and "I'm flying from San Francisco" as a final answer to a different question. This intuition is so fundamental to communication that we rarely notice it, yet it's one of the hardest problems for voice AI systems to solve.


## The Turn Detection Problem


At the heart of every voice assistant is a deceptively simple question: is the person done speaking? Get it wrong, and the consequences cascade. Interrupt too early, and you create frustration—the user must repeat themselves, breaking the conversational flow. Wait too long, and the interaction feels sluggish and unresponsive, undermining the promise of natural dialogue.


Current approaches to turn detection fall into two camps, each with fundamental limitations:


**Audio-only systems** analyze vocal patterns—pitch, pace, and intonation—to predict turn completion. When you ask "What are your departure and arrival airports?" and the user responds "I'm departing from San Francisco," the audio signal alone cannot distinguish between a pause before continuing ("...and arriving in New York") and a genuinely incomplete answer. The intonation might be identical, but the semantic context makes all the difference.


**Text-only systems** take the opposite approach, analyzing transcribed words and punctuation patterns. But strip away the audio, and you lose critical information. Consider the question "What is your patient ID number?" The responses "123 456" and "123 456 789" look nearly identical as text, but the speaker's intonation makes their intent unmistakable—one rises at the end, signaling more to come; the other falls with finality. Text-only systems also tend to overfit to punctuation marks, making them brittle across different speech-to-text providers with varying punctuation styles.


## A Multimodal Solution


Human conversation is multimodal by nature—we simultaneously process acoustic cues and semantic meaning. A robust turn detection system must do the same. We need models that can hear the rising question in "123 456" while also understanding that "I'm departing from San Francisco" is a contextually incomplete response to a question about two airports.


This is why we built our turn detection model from the ground up to combine both audio and text signals, creating a system that understands not just how you're speaking, but what you're saying and what the context demands. The result is a model that achieves 94.1% accuracy while running fast enough for real-time voice applications—because natural conversation cannot wait.


# Technical Implementation


Audio and text embeddings are fed into an ablated small language model (SmolLM-132 with only the first 12 layers/~80M params) with a linear classification head


## Model Architecture


Our turn detection model is built on a multimodal foundation that processes both acoustic and linguistic signals. The architecture combines audio feature extraction with a compact language model, enabling real-time inference while maintaining high accuracy.


### **Base Model Selection**


We selected SmolLM2-135M as our starting point for several reasons. First, efficiency is paramount for turn detection—the model must run fast enough to maintain conversational fluidity, ideally within single-digit milliseconds. Second, turn detection is a relatively constrained task compared to general language modeling, suggesting that a smaller, focused model might achieve comparable performance to larger alternatives. Third, SmolLM2's architecture provides a strong foundation for multimodal fusion while remaining computationally tractable.


However, even SmolLM2-135M contains more capacity than necessary for our task. Through systematic ablation experiments, we found that retaining only the first 12 hidden layers of the SmolLM2 checkpoint reduces the parameter count from 135M to approximately 80M with no measurable impact on turn detection accuracy. This depth reduction maintains the model's representational capacity while significantly improving inference latency.


### **Multimodal Fusion**


The model processes two complementary input streams:


*Audio features:* We extract acoustic representations using a pre-trained Whisper encoder, selecting up to the last 8 seconds of audio. The Whisper encoder outputs are projected into the language model's embedding space, yielding approximately 400 tokens that capture prosodic information—pitch contours, speaking rate, pauses, and intonation patterns that signal turn completion or continuation intent.


*Textual context:* The conversation transcript, including both assistant and user utterances, provides semantic context. This allows the model to understand whether a response is complete given the preceding question or statement.


The audio embeddings are prepended to the text sequence, allowing the language model to attend jointly across both modalities. This design enables the model to reason about questions like "Given the acoustic properties of this utterance and the conversational context, has the speaker finished their turn?"


### Dataset Construction


Training a turn detection model requires examples that capture the full spectrum of conversational complexity—not just clear turn boundaries, but the ambiguous cases where acoustic and semantic cues must be weighed together.


**Data Sources**


Our training data combines two complementary approaches:


*Human-collected data:* We recorded diverse conversational scenarios spanning multiple domains: customer service interactions, medical intake conversations, travel booking dialogs, and casual multi-turn exchanges. These recordings capture natural speech patterns including disfluencies, self-corrections, and contextual dependencies that synthetic data often misses.


*Synthetic data generation:* Inspired by Pipecat's data generation methodology, we systematically created challenging edge cases. We focused on scenarios where voice activity detection (VAD) alone would incorrectly signal turn completion:


* Multi-clause responses where the speaker pauses between independent clauses


* Disfluent speech with filled pauses ("um," "uh") and restarts


* List-like enumerations with natural pauses between items


* Incomplete responses to multi-part questions (e.g., providing departure city but not arrival city)


* Think-aloud patterns where speakers pause to formulate their next thought


The synthetic generation process ensures balanced representation across these challenging cases, preventing the model from relying too heavily on any single signal.


### **Annotation Strategy**


Each sample was labeled with binary turn completion status.


### **Inference Performance**


Latency is critical for turn detection. The model must process audio and make predictions fast enough that any delay is imperceptible to users—ideally under 10ms to maintain conversational naturalness when combined with other system components.


On an NVIDIA T4 GPU, the model achieves:


**Single inference (batch size 1):** ~7ms in fp16


**Batched inference (batch size 30):** no significant increase in latency, enabling efficient multi-session handling


These numbers represent end-to-end latency including audio encoding, model forward pass, and prediction extraction.


### **Quantization and CPU Inference**


The current model release is not optimized for CPU inference; we plan on training int8-quantized models to decrease latency on CPUs for more resource-constrained setups.


## Evaluation Results


### **Accuracy Metrics**


The model achieves:


**Accuracy:** 94.1%


**AUPRC (Area Under Precision-Recall Curve):** 0.975


### Limitations and Future Work


**Language Support:** The current model is trained exclusively on English conversational data. Turn-taking conventions vary across languages—for example, overlap patterns and acceptable pause durations differ between cultures. Multilingual training data will be essential for broader deployment.


**Domain Adaptation:** While we included diverse conversational scenarios, highly specialized domains (legal consultations, technical support, medical diagnosis) may exhibit domain-specific turn patterns. Few-shot adaptation techniques could enable customization without full retraining.


**Task Adaptation:** While we tried to best represent the types of tasks that would lead to false positives/negatives (e.g. spelling), representation of a broader spectrum of flavors of these tasks (e.g. spelling numbers vs. spelling websites, etc.) would better teach the model to recognize specific variations in cues.
