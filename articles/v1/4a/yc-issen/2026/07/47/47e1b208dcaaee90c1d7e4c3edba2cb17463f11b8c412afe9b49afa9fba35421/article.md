---
schema_version: "1.0.0"
document_id: "47e1b208dcaaee90c1d7e4c3edba2cb17463f11b8c412afe9b49afa9fba35421"
company_key: "yc-issen"
company: "ISSEN"
source_id: "yc-issen-news-import-92b9053c251f"
canonical_url: "https://www.issen.com/blog/spaced-repetition-vocabulary-ai-conversation-practice"
published_at: "2026-07-27T15:06:12.934+00:00"
first_seen_at: "2026-07-28T00:39:09.576870+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:c66a749a19c013f1d4f5a611428f595d109dcb5c75d49f7b9bd38a14d21b50cb"
---

# Spaced Repetition and AI Conversation: Learn 1,000 Words a Month (July 2026)

Thirty-three new cards a day, 90% retention, FSRS doing the math in the background. The spaced repetition side of vocabulary learning is well-documented and genuinely effective. What's less talked about is what happens after the reviews: you've kept 1,000 words alive in your head, but pulling them out during an actual conversation is a separate skill that flashcards alone don't train. This post covers the full loop, from building your daily review schedule to closing the gap between recognition and real use.


**TLDR:**


-


Spaced repetition spaces reviews at widening intervals timed to your forgetting rate; a 2022 meta-analysis by Kim and Webb across 48 experiments found medium-to-large effects on second language retention, with longer intervals producing the strongest gains.


-


Hitting 1,000 words a month means 33 new cards per day, but the real load comes from the review queue: budget 45 to 60 minutes daily and expect 250 to 300 total cards per session by week two.


-


Sentence cards outperform isolated word cards because they carry article gender, case endings, and word order for free, so you rehearse grammar every time you rehearse the word.


-


Drilling 5,000 cards to 90% retention won't prepare you for a real conversation; Swain's Output Hypothesis (1985) argues that producing language under pressure is what moves words from recognition into actual use.


-


ISSEN builds SRS flashcards from your own voice conversations, so the review card surfaces the sentence you used the word in, not a wordlist someone else made.


## What spaced repetition is


Spaced repetition is a review schedule that spaces encounters with a piece of information at widening intervals, timed just before you would forget it. Each successful recall stretches the next interval. Each miss shortens it. A word you saw once becomes a word you own.


The mechanic sits on Hermann Ebbinghaus's 1885 forgetting curve, which showed memory decays sharply after initial learning and flattens with each successful review. Sebastian Leitner turned that into a practical system in the 1970s using physical flashcard boxes: cards you got right moved to slower-review boxes; missed cards dropped back to daily practice. Modern[AI language learning](https://www.issen.com/blog/what-is-ai-language-learning) has since built on these foundations in ways Leitner couldn't have anticipated.


For vocabulary, the bottleneck is not exposure. The real constraint is retention under decay.


## The science of the forgetting curve


Ebbinghaus's original curve put numbers on something every learner has felt:[Murre and Dros's 2015 replication](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4492928/) confirmed that around 60 to 70% of new material slips away within 24 hours if nothing pulls it back. The decay is steep at first, then flattens. Each timed review before the trace fades resets the slope, and the interval you can safely wait grows longer.


The empirical version is a 2022 meta-analysis by Kim and Webb covering 48 experiments and 3,411 participants, which found[medium-to-large effects on second language learning](https://www.researchgate.net/publication/358406370_The_Effects_of_Spaced_Practice_on_Second_Language_Learning_A_Meta-Analysis) from spaced practice, with longer intervals producing the strongest gains on delayed retention tests. Same-day cramming quietly evaporates by the following week. The broader picture of[effective language learning techniques](https://www.issen.com/blog/most-effective-language-learning-techniques-ai) shows spaced practice as one pillar among several.


## How SRS algorithms schedule your reviews


Under the hood, every spaced repetition app runs the same math: how long can we push the next review without letting the card slip?


The classic answer is SM-2, published by Piotr Wozniak in 1987. It multiplies the current interval by an ease factor that shrinks when you miss a card. Simple, but the same schedule for every learner.


Anki's current default is FSRS, the Free Spaced Repetition Scheduler. It tracks three variables per card and fits a personalized memory model from your own review history:


-


Difficulty: how hard the card is for you in particular


-


Stability: how long the memory lasts before decay


-


Retrievability: the predicted probability you can recall it right now


FSRS schedules the next review when retrievability drops to a target you set (90% is the common default), so intervals stretch or contract based on your actual forgetting rate. As of February 2024, FSRS is[supported natively across all Anki clients](https://faqs.ankiweb.net/what-spaced-repetition-algorithm) : desktop, AnkiWeb, AnkiMobile on iOS, and AnkiDroid.


## Anki for language learning: free, cross-platform, and highly configurable


Anki is the tool most committed learners land on, and it stays free where it matters. The desktop app runs on Windows, Mac, and Linux; AnkiWeb syncs decks across devices; AnkiDroid is free on the Play Store. The exception is iOS: AnkiMobile is $24.99 USD, which funds the wider ecosystem.


Setting


Default for 1,000/month


Why


New cards/day


33


One month of intake to hit target


Reviews/day


200 to 300


Absorbs the tail from prior weeks


Scheduler


FSRS


Personalizes intervals to your recall


Desired retention


0.9


Balances workload and accuracy


Learning steps


1m 10m


Quick early reinforcement


After 400 to 1,000 reviews of history, run Tools > FSRS > Optimize to fit parameters to your own forgetting rate. Recheck every couple of months.


## Why sentence cards outperform isolated word cards


A naked card pairs a foreign word with its English gloss and nothing else. You learn that "Schlussel" means "key," then in a real sentence you hesitate over der, die, or das, and whether you need Schlussel, Schlussels, or Schlusseln.


A sentence card carries that information for free. The word arrives inside its article, case ending, and word order, so you rehearse the grammar every time you rehearse the word. For German, Russian, or Japanese, that packaging is the difference between recognition and production.


Paul Nation's work at Victoria University of Wellington argues[deliberate vocabulary learning from word cards](https://www.cambridge.org/core/books/learning-vocabulary-in-another-language/deliberate-vocabulary-learning-from-word-cards/74EB1B4830C8A6C6A9E1490BCA444AD3) is efficient but incomplete. Cards build the foundation. Meaning-focused input and output convert it into words you can pull out mid-conversation.


## How to build a spaced repetition schedule for 1,000 words a month


The math is simple. 1,000 words divided by 30 days is 33 new cards per day. The trap is the review queue: at 90% retention, each new card generates 8 to 10 reviews before intervals stretch past 30 days.


A realistic week two load, once the pipeline is full:


Day


New


Reviews


Total


Mon


33


220


253


Wed


33


260


293


Fri


33


270


303


Sun


33


260


293


Budget 45 to 60 minutes a day. If the queue climbs past 350, pause new cards for two or three days and let reviews drain. Do not drop retention below 0.85 to catch up; you will spend the recovered time relearning leeches.


To project your load before committing, plug your new-card rate into Anki's built-in FSRS simulator (Tools > FSRS > Simulate) or a spreadsheet tracking date, new cards, and rolling reviews at your target retention.


## Active recall vs. passive review


Retrieval is the whole engine. Cover the answer, force the word out of your head, and only then flip the card. Re-reading the front and back until it feels familiar gives you a warm sense of recognition that collapses under a delayed test.


The Again, Hard, Good, Easy buttons only work if you rate accurately. Hard means you recalled it slowly under strain. Again means you missed it. Pressing Hard on a blank teaches the algorithm the wrong stability curve, and your intervals drift out of sync with your actual forgetting rate. The leech pile grows from there.


## Why SRS alone won't make you fluent


SRS is a retention tool. It keeps a word alive in your head, and that is the whole promise. Recognition and fluent spoken recall run on different circuits, and the flashcard trains only the first one.[Solo language speaking practice](https://www.issen.com/blog/practice-speaking-alone) builds the second circuit separately. You can drill 5,000 cards to 90% retention and still freeze when a cashier asks a follow-up, because pulling a word under real-time pressure is a separate skill built separately, and[fear of speaking a foreign language](https://www.issen.com/blog/how-to-speak-foreign-language-without-fear) compounds that gap.


Grammar has the same problem. Recognizing that a subjunctive is correct on a card does not teach your mouth to reach for it mid-sentence. Case endings, particles, article gender: these become automatic through production reps, not repeated recognition. Cards give you the raw material. Talking converts it, which is why[your language routine needs speaking practice](https://www.issen.com/blog/why-your-language-routine-needs-speaking-practice) built in from the start, not added as an afterthought.


## How AI conversation practice completes the SRS cycle


Merrill Swain's[Output Hypothesis](https://files.eric.ed.gov/fulltext/EJ1095572.pdf) argues that producing language under conversational pressure forces you to notice the gap between what you meant to say and what came out. Reading a card labels a word as familiar; reaching for it in a sentence, missing the ending, and hearing yourself correct it: that process moves the word from recognition into production.


The historical block has been access. A conversation partner willing to sit through your broken B1 attempts at 6am does not exist for most learners.[AI speaking practice](https://www.issen.com/blog/how-ai-speaking-practice-works) removes the scheduling and the flinch of stumbling in front of another human. If Anki keeps your 1,000 words alive, daily conversation forces them into your mouth, and that is exactly what[AI language tutoring for speaking fluency](https://www.issen.com/blog/ai-language-tutoring-speaking-fluency) is built to deliver.


## ISSEN: Conversation-Anchored SRS in a Single Session


Where a standalone Anki deck starts from a wordlist someone else made, ISSEN's flashcards get built from your own voice conversations, a design that separates the[best AI language tutors for conversation practice](https://www.issen.com/blog/best-ai-language-tutors-conversation-practice) from simple chat tools. When a word surfaces mid-session, the review card carries the sentence you used it in, the topic, and the situational context. Retention and production close in one workflow.


Our CEO Mariano used contextual SRS to learn 2,200 Kanji in 90 days and 2,000 Japanese words in 80 days, under two hours of study per day (per Mariano's personal account).


-


The AI voice tutor drives the exchange and adjusts vocabulary and pace to your level in real time


-


No push-to-talk, so lock-screen sessions on a walk or commute count as real reps


-


Available on iOS, Android, and web with a 10-minute free trial. You can also compare it against the[best language learning apps for speaking](https://www.issen.com/blog/best-language-learning-apps-speaking) to find the right fit.


## Final thoughts on spaced repetition as a language learning system


Spaced repetition gives your vocabulary somewhere to live between conversations. The algorithm handles the scheduling; your job is accurate ratings, consistent reviews, and enough speaking practice to make the words feel automatic. Think of Yuna, a Korean nurse who moved to Canada last year aiming to pass her RN licensing interview within three months, and drilled 1,200 medical and workplace terms to 90% retention in Anki, only to freeze in her first staff meeting when a doctor asked a follow-up she hadn't rehearsed. The vocabulary was there; the output circuit wasn't. What changes that is daily conversational pressure: sentences under real-time conditions, corrections in context, and review cards tied to what you actually said. That is the direction the field is moving, and the tools for it are already available. When your deck is ready for a real test,[try ISSEN free for 10 minutes](https://issen.com/) and see how many of those words you can actually pull out in real time.


## FAQ


### What's the difference between Anki's FSRS algorithm and the older SM-2 for language learning?


FSRS tracks three variables per card (difficulty, stability, and retrievability) and builds a personalized memory model from your own review history, so intervals stretch or contract based on how *you* forget, not a generic curve. SM-2 applies the same ease-factor formula to every learner regardless of individual recall patterns. For a spaced repetition schedule targeting 1,000 words a month, FSRS produces meaningfully better retention per review minute once you have 400 to 1,000 reviews logged and run the optimizer.


### Can I build a spaced repetition schedule for 1,000 words a month without burning out on reviews?


Yes, but the math requires planning before you start. At 90% desired retention, 33 new cards per day generates 200 to 270 reviews per day by week two, so budget 45 to 60 minutes daily and use Anki's built-in FSRS simulator to project your queue before committing. If reviews climb past 350, pause new cards for two or three days; dropping your retention target below 0.85 to catch up costs more time relearning lapsed cards than it saves.


### How do I use Anki spaced repetition settings for sentence cards instead of isolated word cards?


Set your learning steps to 1 minute then 10 minutes, keep desired retention at 0.90, and cap new cards at 33 per day to hit the 1,000-word monthly target. The deeper change is card format: build sentence cards that carry the word inside its article, case ending, and word order so every review rehearses grammar alongside vocabulary. This matters especially for German, Russian, or Japanese, where a word divorced from its morphology is only half the information you need.


### Why does drilling a spaced repetition vocabulary app to 90% retention still leave you freezing mid-conversation?


Recognition and real-time spoken recall are separate skills trained by different kinds of practice. A spaced repetition app trains you to identify a word when it appears on a screen; pulling that same word out of memory while a native speaker is waiting for your answer runs on a different cognitive circuit built through output reps under conversational pressure. Merrill Swain's Output Hypothesis describes this precisely: producing language forces you to notice the gap between what you meant to say and what came out, and that noticing is what moves a word from passive recognition into active use. Vocabulary apps supply the raw material; conversation converts it.


### Anki spaced repetition vs ISSEN for vocabulary retention: which should I use?


Use both, for different jobs. Anki's spaced repetition schedule keeps words alive between conversations through timed retrieval practice, which the 2022 Kim and Webb meta-analysis across 48 experiments confirmed produces medium-to-large effects on delayed retention. ISSEN's AI voice tutor then forces those same words into real-time spoken production, and generates flashcards anchored to the sentences you actually used during the session, so the review card carries the original conversational context and not an isolated gloss. The two tools close different gaps in the same cycle: Anki handles retention under decay; spoken conversation practice handles the transfer from recognition to fluency.
