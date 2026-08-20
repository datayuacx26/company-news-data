---
schema_version: "1.0.0"
document_id: "10071b48a3f5734cbe182a4a7f5e6c89a11bde2be6f004271a222702e7296b7d"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/how-to-evaluate-speech-recognition-models"
published_at: null
first_seen_at: "2026-07-24T08:06:00.112884+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:817049b1a93c988c7f9ac23926c09bdb995a4b68df3f73473c5f6ac92b02868b"
---

# How to evaluate speech recognition models

For years, evaluating speech-to-text models meant running word error rate against a clean dataset and declaring a winner. Simple. Reproducible. And increasingly wrong.


I'm going to make a claim up front and spend the rest of this post backing it up: the way most teams evaluate ASR in 2026 tells you almost nothing about how a model will behave in production. Not because the metrics are bad, but because they're incomplete, and because the reference transcripts underneath them are usually broken.


Here's the story that convinced us. When we launched Universal-3 Pro back in 2025, a handful of customers reported that their internal benchmarks showed the new model performing *worse* than the model it replaced. Our own testing said the opposite. So we dug in. What we found changed how we think about evaluation entirely, and it's the reason this guide exists.


The problem was never word error rate itself. The problem is that WER is one piece of a much bigger picture. It treats every word as equally important, it inherits every flaw in your ground truth, and it can't measure the things that matter to the LLMs and voice agents that now consume most transcripts. As Voice AI applications get more sophisticated, your evaluation has to get more sophisticated too.


This guide walks through modern speech recognition evaluation end to end: the core ASR evaluation metrics, the ones almost nobody measures (diarization accuracy and code-switching), the ground truth problem that quietly corrupts benchmarks, and the practical tools to run it all correctly. If you want the fundamentals first, our[speech-to-text overview](https://www.assemblyai.com/products/speech-to-text) covers the basics.


For reference, the current AssemblyAI flagship for pre-recorded audio is Universal-3.5 Pro, released July 7, 2026 at $0.21/hour. I'll use it as the running example, and I'll flag where the older Universal-3 Pro story is historical.


## Part 1: The core ASR evaluation metrics


### Word error rate: the foundation


Word error rate is still the industry standard, and it earned that spot. It's simple, it's reproducible, and it gives you one number to compare across vendors. But you have to understand what it actually measures before you trust it. For a deeper primer, see[Is word error rate useful?](https://www.assemblyai.com/blog/word-error-rate)


WER is the percentage of words that differ between a reference transcript (your ground truth) and the model's output. It counts three error types:


- **Insertions** (hallucinations): words the model added that weren't spoken
- **Deletions** : words that were spoken but the model missed
- **Substitutions** : words the model got wrong (transcribing "Cadillac" as "cataracts")


The formula:


```text
WER = (Insertions + Deletions + Substitutions) / Total Words in Reference × 100
```


A quick example:


- Reference: "The quick brown fox jumps over the lazy dog" (9 words)
- Output: "A quick brown fox jumped over the lazy dog" (9 words)
- Errors: 2 substitutions ("The"→"A", "jumps"→"jumped")
- WER: (2 / 9) × 100 = 22.2%


The catch that trips up most teams: before you calculate anything, both transcripts have to be normalized. Lowercase everything, strip punctuation, spell out numbers, standardize contractions. "don't" and "do not" are semantically identical but score as an error if you skip this step. We recommend the open-source[Whisper Normalizer](https://github.com/openai/whisper/blob/main/whisper/normalizers/english.py) for English and the Basic normalizer for everything else.


One number worth anchoring on: Universal-3.5 Pro posts a mean English WER of 5.6% (median 4.9%), measured across 26 public datasets, 80,000+ files, and 250+ hours of audio. That last part matters more than the number. A WER figure from a single curated dataset is marketing. A WER figure across dozens of messy real-world datasets is a forecast. Always ask which one you're looking at.


### Semantic WER: beyond word-for-word


Here's where it gets interesting. Traditional WER assumes every word carries equal weight. But in modern Voice AI, transcripts usually feed an LLM, not a human reader. An LLM doesn't care whether the transcript says "cannot" or "can't"; it extracts the same meaning either way. Traditional WER penalizes that harmless difference just as hard as a genuine error.


Semantic WER fixes this. Instead of comparing strings, it uses a reasoning model to judge whether the *meaning* survived:


1. A reasoning model receives both the model transcript and the reference
2. It judges whether they convey the same information
3. Accuracy is scored on semantic equivalence, not exact string match


An example traditional WER gets wrong:


- Reference: "The patient is on hydrochlorothiazide for hypertension"
- Output: "The patient is on HCTZ for hypertension"
- Traditional WER: ~20% error (one substitution)
- Semantic WER: 0% error (HCTZ is the standard abbreviation, meaning fully preserved)


For voice agents and any LLM-driven pipeline, Semantic WER is often the more honest metric because it measures what actually moves downstream performance. Our post on[how accurate speech-to-text is in 2026](https://www.assemblyai.com/blog/how-accurate-speech-to-text) shows how Semantic WER stacks up across providers.


### Missed Entity Rate: the words that actually matter


WER treats "um" and "hydrochlorothiazide" as equals. Your business does not. Missing a filler word is harmless. Missing a drug name in a clinical note is a safety problem.


Missed Entity Rate (MER) measures accuracy specifically on high-value tokens: drug names and dosages, proper nouns and company names, procedures and diagnoses, numbers, dates, addresses, and any domain-specific terminology you care about. Rather than averaging error across a whole transcript, it isolates the words that decide whether your application works.


**Real-world example, Medical Mode.** AssemblyAI's Medical Mode is a +$0.15/hour add-on (domain="medical-v1") that cuts Missed Entity Rate on clinical terms by roughly 20%. Our internal benchmark:


Model MER on medical entities


AssemblyAI Medical Mode ~4.9%


Deepgram Nova-3 Medical ~7.3%


That gap looks small until you translate it. It's meaningfully fewer missed drug names per clinical transcript, and in healthcare that's the line between a documentation tool clinicians tolerate and one they actually trust. Medical Mode is available under a Business Associate Addendum (BAA) for teams handling protected health information.


### Character error rate (CER): when whole-word scoring falls apart


Word error rate assumes words are the right unit. For a lot of the world's audio, they aren't.


Character error rate (CER) measures accuracy at the character level: insertions, deletions, and substitutions of individual characters divided by the total characters in the reference. It's the metric of choice when word boundaries are ambiguous or nonexistent, which is exactly the case for languages like Mandarin, Japanese, Thai, and Korean where "words" aren't neatly space-delimited. It's also useful for scoring heavily inflected languages, and for spelling-sensitive content like codes, IDs, and alphanumeric strings where a single wrong character matters.


The tradeoff: CER is less intuitive for English, where a one-character slip and a completely wrong word both register as "errors" without telling you which happened. Rule of thumb, use WER for English and other space-delimited languages, reach for CER when whole-word scoring stops making sense, and report both when you're evaluating a multilingual model.


### cpWER: how to actually measure diarization


Most ASR evaluation guides stop at WER and its cousins. That's a real gap, because a huge share of production audio is multi-speaker, and none of the metrics above tell you whether the model got the speakers right. You can score a perfect WER and still produce a transcript that's useless because it attributed the customer's words to the agent.


The metric for this is cpWER, concatenated minimum-permutation word error rate. It works like this: concatenate each speaker's words, try every possible mapping between reference speakers and predicted speakers, and take the permutation that minimizes total WER. The result is a single number that captures transcription accuracy *and* speaker attribution at the same time. Lower is better. If the model splits one speaker into two, merges two into one, or swaps who said what, cpWER punishes it. Plain WER shrugs.


We optimized Universal-3.5 Pro's diarization directly for cpWER, and it's the most accurate diarization we've shipped. Here's how it compares (lower is better):


Model Average cpWER


AssemblyAI Universal-3.5 Pro 30.17


ElevenLabs Scribe v2 35.26


Gladia 36.87


Deepgram Nova-3 (English) 37.92


If your product is conversation intelligence, contact center analytics, meeting notes, or anything with more than one person talking, cpWER should be a first-class metric in your evaluation, not an afterthought. A model that wins on WER and loses on cpWER will still frustrate your users.


### Code-switching and multilingual accuracy


Here's a dimension almost nobody benchmarks and a growing share of real audio demands: what happens when a speaker switches languages mid-sentence? "Necesito el report para el meeting de las tres" is a normal utterance for millions of bilingual speakers, and most models fall apart on it, either forcing everything into one language or garbling the transition.


Universal-3.5 Pro handles code-switching natively across 18 languages. On our code-switch benchmark it posts a 7.69% average normalized WER across five language pairs, measured specifically on code-switched audio rather than clean monolingual samples. That distinction is the whole point: a model can score beautifully on separate English and Spanish test sets and still break the moment a speaker mixes them in one breath.


If you serve bilingual or multilingual users, build a code-switching set into your evaluation. Grab real audio where speakers switch languages naturally, and score it separately from your monolingual benchmarks. It's one of the fastest ways to find out whether a model actually works for your users or just works in the demo.


### LLM-as-a-judge


For the highest-confidence read on quality, use a reasoning model as a judge. This shines when subjective quality matters and a single number won't capture it.


The process:


1. Present two anonymized transcripts side by side, one from your preferred model, one from a competitor
2. Include the reference transcript as ground truth
3. Ask the model which transcript is higher quality for your specific use case
4. Calculate win rates across a representative sample


We use current Claude models for this, Claude Opus 4.8 or Claude Sonnet 5 with extended thinking, depending on how much depth the judgment needs. *\[VERIFY current model\]*


This approach earns its keep for medical transcription (clinical accuracy over grammar), legal transcription (phrasing has consequences), and accessibility (clarity and formatting affect real usability). The upside: you're measuring real-world usefulness instead of gaming a benchmark. The tradeoff: it's slower and more expensive than automated metrics, so use it to validate, not to run on every file.


See These Metrics on Your Own Audio


Run your files through the Playground to see live transcripts, confidence scores, and formatting—including Medical Mode—before you write a line of benchmarking code.


[Try playground](https://www.assemblyai.com/playground)


## Part 2: The ground truth problem


Now the part nobody wants to talk about: the biggest source of evaluation error usually isn't the model. It's your ground truth.


Remember those Universal-3 Pro customers whose WER went *up* ? When we pulled their reference files apart, we found the human transcripts were riddled with systematic errors and inconsistencies, and the benchmark was penalizing the newer model for being *more* accurate than the humans it was measured against. We published the full investigation in[Why your WER benchmark might be lying to you](https://www.assemblyai.com/blog/new-word-error-rate-wer-benchmark) , and I'd read it before you run a single benchmark. (Note that this was the Universal-3 Pro launch; the current flagship is Universal-3.5 Pro, but the lesson is timeless.)


### Why ground truth quality matters


A customer records audio, has a human transcribe it, and calls that transcript "truth." You then measure every model against it. But human transcribers make mistakes too. They mishear words and get them consistently wrong, they normalize speech inconsistently ("alright" versus "all right"), they miss medical abbreviations, they include filler words at random, and they disagree on capitalization, punctuation, and numbers.


When the reference is wrong, a better model scores worse, because it transcribes what was actually said instead of what a tired human thought they heard.


**The Universal-3 Pro case study (historical).** At launch, some customers saw WER climb 2 to 3 points. The reference files were the culprit. One example:


- Reference (should have been): "Patient allergic to penicillin"
- Human ground truth: "Patient allergic to penicillium" (transcriber misheard)
- Model output: "Patient allergic to penicillin" (correct)
- WER verdict: model marked wrong, despite being right


### Semantic equivalence issues


Even without outright errors, ground truth degrades through subjective formatting choices: "healthcare" versus "health care," "alright" versus "all right," "can't" versus "cannot," "12" versus "twelve." Each one can swing WER by points while adding exactly nothing to real quality.


### How to fix ground truth quality


This is what the Truth File Corrector is for. It lives in the AssemblyAI dashboard and it's free. It:


1. Detects semantic equivalence issues across your reference file
2. Flags likely transcriber errors using language models
3. Suggests corrections grounded in domain knowledge
4. Lets you review and approve every change before you re-run


The payoff: your benchmark measures model quality instead of transcriber noise. In practice, this single step often reveals that 5 to 15% of a "clean" reference file was unnecessary inconsistency.


The Truth File Corrector is live in your dashboard, and the[open-source Benchmarking SDK](https://github.com/AssemblyAI/benchmark-sdk) is on GitHub. Both are free.


See Our Benchmarks in the Open


We publish Universal-3.5 Pro's numbers across 26 standard datasets, updated with every release, so you can reproduce them instead of taking a single curated figure on faith.


[View benchmarks](https://www.assemblyai.com/benchmarks)


## Part 3: A practical benchmarking framework


### Step 1: Prepare representative audio


Pick 10 to 20 files that look like your real traffic: a mix of accents and dialects, a range of audio quality (clean studio, conference calls, noisy field recordings), genuine domain terminology, and varied speaker counts with real overlapping speech. Don't benchmark only on pristine audio. Your models will do worse in production, and you want numbers that predict that, not numbers that flatter it.


### Step 2: Build ground truth with quality controls


**Option 1 (recommended).** Transcribe with Universal-3.5 Pro, then have domain experts correct only clear errors. It's far faster than transcribing from scratch and your baseline quality is already high.


**Option 2.** Hire professional transcribers, and tell them explicitly to transcribe what was *said* , not to "clean up" grammar, using industry-standard formatting for your domain.


Either way, upload a sample to the[Playground](https://www.assemblyai.com/playground) first to see live transcripts, confidence scores, and formatting before you write any code.


### Step 3: Normalize both transcripts


```text
from   whisper_normalizer.english   import   EnglishTextNormalizer


normalizer = EnglishTextNormalizer()
reference_normalized = normalizer(reference_transcript)
ai_normalized = normalizer(ai_transcript)
```


### Step 4: Calculate multiple metrics


Don't stop at WER. Run at least three, and add cpWER if your audio is multi-speaker:


```text
import   jiwer


# Traditional WER
wer = jiwer.wer(reference_normalized, ai_normalized)


# Error breakdown
output = jiwer.compute_measures(reference_normalized, ai_normalized)
substitutions = output[  'substitutions'  ]
deletions = output[  'deletions'  ]
insertions = output[  'insertions'  ]
```


### Step 5: Run the Truth File Corrector


1. Upload your reference file to the dashboard
2. Run it through the Truth File Corrector
3. Review flagged inconsistencies
4. Approve or edit the corrections
5. Re-run your evaluation against the cleaned reference


### Step 6: Benchmark streaming and async separately


Streaming and pre-recorded models optimize for different tradeoffs, so evaluate them independently. More on both below.


## Part 4: The ASR evaluation metrics at a glance


Metric What it measures Best for Limitations


Word error rate (WER) % of words wrong, missing, or added General benchmarking Treats all words equally


Semantic WER Whether meaning is preserved Voice agents, LLM pipelines Slower, needs a reasoning model


Missed Entity Rate (MER) Accuracy on high-value words Medical, legal, technical Requires defining key terms


cpWER Transcription + speaker attribution Diarization, multi-speaker audio Needs speaker-labeled reference


Character error rate (CER) Character-level accuracy Non-space-delimited and multilingual audio Less intuitive for English


Code-switch WER Accuracy on mixed-language speech Bilingual and multilingual products Needs code-switched test data


LLM-as-a-judge Overall subjective quality Regulated, human-reviewed use Subjective, expensive


Ins/Del/Sub rates Breakdown of error types Diagnosing model failures No single comparable number


**My recommendation.** For most teams, WER plus MER on your domain terms is the right primary pair. Add Semantic WER if transcripts feed an LLM. Add cpWER the moment you have more than one speaker. Add code-switch WER if your users are multilingual. Reserve LLM-as-a-judge for regulated work where subjective quality is the whole ballgame.


## Part 5: Real-world evaluation patterns


### Enterprise medical transcription


MER on clinical terminology is your north star. Primary: MER on drug names, procedures, and diagnoses. Secondary: WER and Semantic WER. Clean your ground truth with the Truth File Corrector, and split streaming (ambient scribes) from async (batch documentation). Medical Mode's ~4.9% MER on clinical entities *\[VERIFY\]* comes from training specifically on the terminology these workflows live and die on.


### Voice agent transcription


For agents passing transcripts to an LLM, weight Semantic WER heavily and watch your deletion rate, because a hang is worse than a harmless substitution. Benchmark on real customer conversations with overlapping speech, and measure accuracy at the latency you'll actually deploy at.


One eval consideration specific to agents: context. Universal-3.5 Pro Realtime accepts the agent's own question as input via agent_context, and feeding it that context cut WER by 10.2% across 20,000 voice agent audio files in our testing. If you're benchmarking a streaming model for an agent, test it both with and without context, because the version you ship will have context and the version you benchmark naively won't.[Choosing an STT API for voice agents](https://www.assemblyai.com/blog/choosing-a-stt-api-for-voice-agents) has the full framework.


### Contact center analytics


Balance transcription accuracy against speaker attribution and Speech Understanding. Primary: WER on critical information (names, issues, account numbers) and cpWER for who-said-what. Secondary: sentiment and emotional tone preservation. Run it on real calls with real background noise and accent diversity, never on scrubbed samples.


## Part 6: The Benchmarking SDK and dashboard tools


The[AssemblyAI Benchmarking SDK](https://github.com/AssemblyAI/benchmark-sdk) is free and open source. It automates WER with the Whisper Normalizer, MER on your domain terms, error-type breakdowns, batch evaluation across models, and CSV/JSON export.


```text
pip install assemblyai-benchmark-sdk


python -m aai_benchmark \
--reference truth.txt \
--hypothesis model_output.txt \
--language en \
--metrics wer,mer,semantic_wer
```


The Truth File Corrector is built into the dashboard. No setup, free for everyone.


## Part 7: Streaming vs. async benchmarking


### Pre-recorded (async) evaluation


Async models get time to process fully, so optimize purely for accuracy. Score WER on full final transcripts against normalized, corrected ground truth, and split your audio by scenario. Universal-3.5 Pro reaches 5.6% mean WER (4.9% median) across 26 English datasets precisely because it can apply multiple passes and correction layers that a real-time model can't. It also brings native code-switching across 18 languages and contextual prompting. Universal-3 Pro remains available as a pinnable snapshot if you need reproducibility across a long-running evaluation.


### Streaming evaluation


Streaming trades some processing headroom for speed, so measure both dimensions:


- **TTFT** (time to first token): how fast the first word comes back
- **TTCT** (time to complete turn): how long to finalize an utterance
- **Accuracy on final transcripts:** WER on completed turns, not partial hypotheses
- **Deletion rate:** the one to watch, because a dropped word makes the conversation hang


The current streaming flagship is Universal-3.5 Pro Realtime, released June 23, 2026 at $0.45/hour base. The number I'd point you to is its 6.99% pooled WER on Pipecat's open STT benchmark, which is built from real voice agent conversations rather than read speech. For context, that benchmark puts Deepgram Flux at 15.58%, ElevenLabs Scribe v2 at 9.76%, and Google Chirp3 at 9.04%. Universal-3.5 Pro Realtime also posts a 15.31% entity error rate on that set. (Universal-3 Pro Streaming remains a pinnable snapshot.)


Two things matter about that benchmark. First, it's open, so you can reproduce it. Second, it's built from actual agent audio, not clean recordings, which is why the spread between models is so much wider than on a tidy academic dataset. When you evaluate streaming, don't compare on pure accuracy in the abstract. Compare accuracy at the latency you'll actually run at. An async model's WER is meaningless if your product needs a sub-300ms response.


## Final words: evaluation is a system, not a step


Most teams treat evaluation as a gate you pass through once before launch. It isn't. It's a system you run continuously, because your audio drifts, your use cases expand, and your ground truth quietly rots.


Here's the new insight I'll leave you with, the one that took us a model launch and a lot of confused customers to internalize: the metric you optimize for is a decision about who your product serves. Optimize for WER alone and you've quietly decided that every word matters equally and every speaker is the same person, which is true for almost no real product. Add cpWER and you're deciding that speaker attribution matters. Add code-switch WER and you're deciding that bilingual users count. Add Semantic WER and you're deciding to serve the LLM that reads your transcript, not the human who never will. Your metric stack *is* your product strategy. Choose it on purpose.


**Quick action steps:**


- Run evaluation on at least 10 representative files across multiple models
- Calculate WER plus a domain metric (MER for medical, deletion rate for agents), and add cpWER for multi-speaker audio
- Fix your ground truth with the Truth File Corrector before you trust any number
- Benchmark streaming and async separately, at the latency you'll deploy at
- Test code-switched audio separately if you serve multilingual users
- Use Semantic WER whenever transcripts feed an LLM


Run Your First Benchmark Today


Get a free API key, transcribe your representative files with Universal-3.5 Pro, and clean your ground truth with the free Truth File Corrector before you trust any number.


[Sign up free](https://www.assemblyai.com/dashboard/signup)


## Frequently asked questions


### How do you evaluate STT models?


Evaluate speech-to-text models on representative audio using multiple metrics, not one. Start with word error rate on normalized transcripts, add Missed Entity Rate on the domain terms you care about, and add cpWER if your audio has more than one speaker. Use Semantic WER when transcripts feed an LLM, and character error rate (CER) for non-space-delimited or multilingual audio. Critically, validate your ground truth first (bad reference transcripts are the number one cause of misleading results), then benchmark streaming and async models separately because they optimize for different tradeoffs.


### What metrics measure an ASR model?


The core ASR evaluation metrics are word error rate (WER), which counts insertions, deletions, and substitutions; Semantic WER, which judges whether meaning is preserved; Missed Entity Rate (MER), which scores accuracy on high-value words like drug names and account numbers; character error rate (CER), which works at the character level for languages without clear word boundaries; and cpWER (concatenated minimum-permutation word error rate), which measures transcription and speaker attribution together for diarization. LLM-as-a-judge adds a subjective quality read for regulated use cases. For voice agents, deletion rate and latency metrics (TTFT, TTCT) belong alongside accuracy.


### What is word error rate and why does it matter?


Word error rate is the percentage of words that differ between a reference transcript and a model's output: (insertions + deletions + substitutions) / total reference words × 100. Lower is better. It matters because it's reproducible, two people scoring the same audio should get the same number, and it gives you a single comparable figure across vendors. But it's not sufficient on its own. It treats every word equally and inherits every error in your ground truth, which is why modern evaluation pairs it with metrics like Semantic WER, MER, and cpWER.


### What is cpWER and when should I use it?


cpWER (concatenated minimum-permutation word error rate) measures transcription accuracy and speaker attribution at the same time. It concatenates each speaker's words, tries every mapping between reference and predicted speakers, and reports the WER of the best-matching permutation. Use it whenever your audio has multiple speakers (contact centers, meetings, interviews, conversation intelligence), because standard WER can look great even when the model attributes words to the wrong speaker. Universal-3.5 Pro averages 30.17 cpWER, ahead of ElevenLabs Scribe v2 (35.26), Gladia (36.87), and Deepgram Nova-3 English (37.92).


### What factors affect speech-to-text accuracy?


**Audio:** background noise, microphone quality, compression, echo and reverberation. **Speaker:** accent, dialect, pace, clarity, overlapping speech. **Content:** vocabulary complexity, proper nouns, numbers and dates, and language mixing (code-switching). Model choice matters too, Universal-3.5 Pro handles diverse accents, noise, technical terms, and code-switching across 18 languages far better than older models, but only if you're actually running it and evaluating it on audio that looks like your real traffic.


### Does real-time transcription sacrifice accuracy for speed?


Less than it used to. Universal-3.5 Pro Realtime posts 6.99% pooled WER on Pipecat's open benchmark of real agent conversations, well ahead of Deepgram Flux (15.58%), ElevenLabs Scribe v2 (9.76%), and Google Chirp3 (9.04%), while delivering the low latency voice agents need. The tradeoff isn't "accuracy versus speed" anymore, it's accuracy at a given latency point. Feeding the agent's own question via agent_context cut WER another 10.2% across 20,000 voice agent files, so test streaming models with the context you'll actually give them in production.


‍
