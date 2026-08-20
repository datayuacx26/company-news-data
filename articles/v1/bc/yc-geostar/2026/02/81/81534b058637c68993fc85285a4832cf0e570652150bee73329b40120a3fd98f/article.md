---
schema_version: "1.0.0"
document_id: "81534b058637c68993fc85285a4832cf0e570652150bee73329b40120a3fd98f"
company_key: "yc-geostar"
company: "Geostar"
source_id: "yc-geostar-news-import-4466b2e9c01d"
canonical_url: "https://www.geostar.ai/blog/the-words-ai-cant-stop-using-vocabulary-lexical-patterns"
published_at: "2026-02-27T00:00:00+00:00"
first_seen_at: "2026-07-24T09:40:06.971335+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:2891672e21d3865d69b6439397529fbb516564cf0daa6a8961cb69231a64d80a"
---

# The Words AI Can't Stop Using: Vocabulary & Lexical Patterns

At[Geostar](https://www.geostar.ai/) , we produce content for dozens of clients across industries, from SaaS and e-commerce to law firms and home services. Every piece we write has to sound like *that client* , not like a language model. So when AI-generated text started flooding the web with identical vocabulary and rhythms, we needed to understand exactly what was happening at a linguistic level.


We analyzed over 186,000 articles across five major AI models (ChatGPT, Claude, Gemini, Grok, and DeepSeek), mapping the vocabulary, sentence structures, and rhetorical patterns that distinguish machine-generated text from human writing. The[full research data is published on our site](https://www.geostar.ai/resources/ai-writing-patterns-research-data) , with breakdowns by model, detection method, and pattern category. This article is an overview of what we found, what it means for content strategy, and why we built this research into how we work.


## Why We Ran This Analysis


Our core business is[generative engine optimization](https://www.geostar.ai/what-is-geo) : helping brands become visible and citable in AI search engines like ChatGPT, Perplexity, Google AI Overviews, and Claude. We track citation rates across those platforms for every client.


What we noticed: content that carries AI vocabulary patterns performs worse as a cited source. AI search engines evaluating which pages to reference can distinguish between original expert writing and text that mirrors the same patterns the models themselves produce. If your article sounds like it was written by the same system doing the citing, you have a structural disadvantage.


That observation pushed us to quantify the problem. We needed hard data on exactly which words, phrases, and structures function as AI fingerprints, both to protect our clients' brand voices and to give our[full-service agency](https://www.geostar.ai/agency) team clear, measurable standards for content quality.


## The 654% Word


The word "delve" surged 654% in academic papers after November 2022. That alone is striking, but what really captured our attention: 46% of all historical uses of "delve" in PubMed appeared in just 15 months. A systematic analysis of 26,657 words appearing more than 100 times annually in biomedical publications revealed 379 style words with elevated frequencies in AI-processed text \[1\].


The contamination is broad. "Underscore" jumped from roughly 3% to 30% of papers between 2022 and 2025. "Showcasing" increased ninefold. And these words travel together: "delve" and "underscore" co-appeared in 98.8% of flagged papers. That kind of clustering does not happen when human authors make independent vocabulary choices.


The pattern extends beyond academia. At least 20-30% of academic papers published in 2024 include machine-generated text. Meanwhile, 9.1% of US newspaper articles contain AI content, and an estimated 54% of LinkedIn long-form posts follow AI formulas. In our own analysis of marketing content, the saturation is even higher in certain verticals.


## The Signature Word List


Our research cataloged dozens of words that function as reliable AI markers. These cluster into distinct semantic categories that reveal how language models approach word choice.


| Category | Examples | Pattern | | --- | --- | --- | | Corporate buzzwords | "leverage" / "synergy" / "streamline" | Mid-formal business register regardless of context | | Empowerment verbs | "foster" / "harness" / "catalyze" | Action-oriented but vague | | Aspirational adjectives | "seamless" / "vibrant" / "unparalleled" | Inflated language that adds no specificity | | Grandiosity terms | "paradigm" / "game-changer" | Exaggerated importance claims | | Academic tells | "underscore" / "meticulous" / "pivotal" | Academic tone applied to non-academic contexts |


The common thread is a pull toward the mid-formal register. AI models flatten language. Fiction, blog posts, and technical documentation all converge toward the same dense, noun-heavy, academic-adjacent prose. We call this register leveling, and it is one of the most consistent markers of AI involvement in any text. Research confirms that instruction-tuned models converge on a particular noun-heavy, informationally dense style that does not match genre conventions familiar to human audiences \[2\].


For content teams, the signature word list works as an audit tool. Run your existing library through these categories. If multiple hits cluster in a single page, that page carries the same linguistic fingerprint as millions of other AI-processed pages competing for the same citations.


Worth noting: many of these words are fine in isolation. "Optimize" is standard marketing vocabulary. "Comprehensive" describes a thorough guide. The signal comes from clustering. When four or five signature words appear in the same 500-word stretch, the probability of AI involvement jumps sharply.


## Why Every AI Writes This Way


The root cause is Reinforcement Learning from Human Feedback (RLHF), the training method that transforms base language models into conversational assistants. During RLHF, human evaluators rate model outputs. Models learn to maximize those ratings. The problem is that evaluators consistently reward the same things:


-


**Elevated vocabulary** gets higher ratings. "Utilize" scores better than "use." "Commence" beats "start." Models learn to reach for the fancier option every time.


-


**Formal register** gets rewarded. Authoritative, complete-sounding responses earn more points. Over time, every output converges on the same mid-formal academic tone.


-


**Structured output** gets positive feedback. Lists, bullet points, and organized formatting earn higher evaluator ratings, so models impose structure even when prose would work better.


This creates a feedback loop. Over successive training rounds, the vocabulary preferences of a small group of annotators get amplified into the default voice of a system used by hundreds of millions of people. Our analysis confirmed what researchers found independently: 28 of 32 known overrepresented AI words appear only after instruction tuning, not in base models \[3\]. The base model writes with vocabulary diversity much closer to human norms. Instruction tuning creates the distinctive AI voice.


This is why brand voice preservation requires more than running text through a language model and editing a few words. At Geostar, every piece of client content goes through editorial review specifically targeting these RLHF-induced patterns, because the models will reintroduce them if you aren't watching.


## Each Model Has Its Own Fingerprint


Not all AI text sounds the same. Our analysis of 186,000+ articles revealed that individual models carry distinct signatures, detectable with 93-98% accuracy in pairwise comparisons against human writing. We published the[complete model-by-model breakdown](https://www.geostar.ai/resources/ai-writing-patterns-research-data/models) in our research data.


| Model | Signature Vocabulary | Structural Tells | Sycophancy Rate | | --- | --- | --- | --- | | ChatGPT | "delve," "landscape," "intricate" | Groups of three, em dashes at 10x earlier rates | 56.71% | | Claude | "nuanced," "measured," "quiet truth" | Prose over lists, context before answer | 57.44% | | Gemini | Simpler terms ("sugar" over "glucose") | Skips preambles, jumps to answers | 62.47% (highest) | | Grok | Informal, internet-native, sarcastic | No consistent template, closes with jokes | 0% | | DeepSeek | "comprehensive," "crucial," "below is" | Compact, step-by-step analytical structure | 0% |


For companies trying to get cited by AI search systems, this fingerprinting matters. Each model evaluates source content through its own lens. Understanding those lenses helps you produce content that reads as authoritative to the specific systems your audience uses. This is central to how we approach GEO for our clients. We don't optimize for one model. We optimize for the full landscape.


## The Structural Tells Beyond Vocabulary


Beyond individual words, AI models produce distinctive phrase-level and structural patterns. Our[patterns analysis](https://www.geostar.ai/resources/ai-writing-patterns-research-data/patterns) documented the most common formulaic structures:


-


**Binary contrast** : "It's not X. It's Y." repeated across paragraphs


-


**Setup phrases** : "Here's why:", "Here's what:", "The truth is:"


-


**Rule of three** : Three adjectives, three list items, three examples in compulsive repetition


-


**Dramatic starters** : "This changes everything." "Nobody's talking about this."


-


**Empowerment closers** : "By understanding X, you can Y."


These structures appear so reliably that we use them as primary detection features. Five or more signature words combined with two or more formulaic structures reaches 99% probability of AI involvement. The punctuation layer adds further signal: em dash usage increased tenfold between GPT-3.5 and GPT-4o, AI models almost never omit the Oxford comma, and sentence length clusters tightly around 25 to 27 words.


That sentence-length uniformity is measurable through "burstiness," the ratio of standard deviation to mean sentence length multiplied by 100. Human writing scores above 50. AI-generated text scores below 30. Count the words in five random sentences and run the calculation. The gap is wide and consistent.


When millions of pages use the same words in the same sentence rhythms with the same structural patterns, no individual page stands out. From a GEO perspective, that sameness is a liability. AI search engines selecting sources to cite have every reason to prefer distinctive, expert-authored content.


## The Co-Evolution Problem


AI vocabulary patterns are already shifting. The most flagged terms began decreasing after March 2024, once they were publicly identified as AI markers. But swapping individual words does not fix the underlying problem. The deeper patterns remain intact:


-


Sentence-length uniformity (every sentence landing between 25 and 27 words)


-


Formulaic paragraph structures (the same hook-explain-transition loop on repeat)


-


Flat register across the entire piece (no shifts between casual and technical)


-


Absence of genuine rhythm variation (no short punchy sentences breaking up longer ones)


Detection accuracy using structural features remains high because the signals are architectural, not lexical. New words fill the same structural roles. The register stays flat. The burstiness stays low.


There is also reverse contamination. People who regularly interact with AI outputs begin unconsciously mirroring AI vocabulary patterns in their own writing and speech. We see this regularly in client audits: teams that adopted AI writing tools early often have libraries where newer content sounds indistinguishable from AI output, even when a human wrote it. This is one of the reasons we built systematic[detection methods](https://www.geostar.ai/resources/ai-writing-patterns-research-data/detection) into our quality control process.


## What This Means for Geostar, and for Every Brand Publishing Content


We ran this research because it directly affects our work. When we produce content for a client, it has to carry that client's voice, not a model's voice. Preserving brand voice at scale, across dozens of clients with distinct tones, audiences, and vocabularies, means we need to know precisely which patterns to intercept before publication.


The data also informs how we advise clients on their broader content strategy:


1.


**Audit for signature patterns.** Run your existing content through the signature word lists and structural checks. Clusters of AI tells put your pages at a disadvantage against content that reads as genuinely human.


2.


**Write with vocabulary diversity.** AI flattens language into a narrow band. Vary sentence length, mix registers, and use precise language grounded in actual experience.


3.


**Invest in editorial voice.** Subject matter experts who have opinions and use field-specific language naturally avoid most AI tells. AI drafting tools, without strong editorial oversight, erode that voice over time.


4.


**Understand model-specific evaluation.** Each AI search platform runs a model with its own biases. Building content with structured data and schema markup helps AI systems parse your content accurately regardless of which model is evaluating it.


5.


**Measure before you publish.** Burstiness (sentence-length variation) and type-token ratio (vocabulary diversity) are simple to calculate and highly diagnostic. Run these checks before publication.


AI search engines have known, measurable vocabulary biases. Content that avoids those biases reads as more original, more authoritative, and more worthy of citation. We see this across every client vertical we work in, and we expect it to intensify as AI-generated content continues to flood the web.


The brands that win in AI search will be the ones whose content sounds like it was written by humans with genuine expertise. Because it was. If you want to see where your brand stands,[book a free audit](https://www.geostar.ai/contact) .


## Frequently Asked Questions


### What are the most common AI-generated words to watch for?


The highest-signal terms are "delve," "underscore," "meticulous," "multifaceted," "pivotal," "seamlessly," "leverage," and "holistic." Finding three or more of these in a single piece of content strongly suggests AI involvement.


### Can AI detection tools reliably identify AI-written text?


Detection based on linguistic features alone reaches 93-98% accuracy in pairwise comparisons, and our research found 97% accuracy using word choice and sentence rhythm. The most effective approach combines vocabulary analysis, sentence-length variation (burstiness), and structural pattern recognition rather than relying on any single metric.


### Do different AI models produce different writing styles?


Yes. Each major model carries distinct vocabulary and structural signatures. ChatGPT favors the signature academic vocabulary. Claude avoids those words but gravitates toward literary alternatives. Grok uses informal vocabulary with humor. These fingerprints are detectable with high accuracy and published in our research data.


### How does AI vocabulary affect content performance in AI search?


Content saturated with common AI vocabulary patterns competes at a disadvantage because it reads as undifferentiated. AI search engines evaluating source authority can distinguish between original expert content and text that mirrors the same patterns the models themselves produce. Brands that invest in authentic voice see higher citation rates across platforms we track.


### What is "register leveling" and why does it matter?


Register leveling is the phenomenon where AI flattens all writing into the same mid-formal, academic-adjacent tone regardless of context. Blog posts and casual writing end up sounding like academic papers. This uniformity makes AI-influenced content less distinctive and less valuable as a cited source.


### Is replacing individual AI words enough to fix the problem?


No. Swapping one flagged word for a synonym addresses a surface-level tell but leaves deeper patterns intact. Sentence-length uniformity and formulaic structures persist. Effective remediation requires addressing vocabulary, rhythm, structure, and voice together, which is why our content process treats AI pattern removal as architectural, not cosmetic.


## References


\[1\] Kobak, D., Gonzalez-Marquez, R., Horvat, E., Lause, J. "Delving into LLM-assisted writing in biomedical publications through excess vocabulary." Science Advances, 2025.[https://www.science.org/doi/10.1126/sciadv.adt3813](https://www.science.org/doi/10.1126/sciadv.adt3813)


\[2\] Reinhart, A., Markey, B., Laudenbach, M. et al. "Do LLMs write like humans? Variation in grammatical and rhetorical styles." PNAS, 2025.[https://www.pnas.org/doi/10.1073/pnas.2422455122](https://www.pnas.org/doi/10.1073/pnas.2422455122)


\[3\] Juzek, T., Ward, C. "Word Overuse and Alignment in Large Language Models: The Influence of Learning from Human Feedback." BIAS 2025, ECML PKDD.[https://arxiv.org/abs/2508.01930](https://arxiv.org/abs/2508.01930)
