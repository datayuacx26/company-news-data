---
schema_version: "1.0.0"
document_id: "c350d372c0b38788889d650a9abce01fb585d668eeed90082d0ab73317e83710"
company_key: "yc-luel"
company: "Luel"
source_id: "yc-luel-news-import-6e1b6cc0a46e"
canonical_url: "https://www.luel.ai/blog/the-tts-gap-and-why-were-building"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-18T01:26:46.028870+00:00"
fetched_at: "2026-08-18T01:26:48.460011+00:00"
content_hash: "sha256:19448447994919b388415920a6e81a6fe8bad6342d1dcc661db92e78a002ec7a"
---

# The TTS gap — and why we’re building

There are roughly 7,000 human languages. Commercial text-to-speech serves a sliver of them: Google’s cloud catalog advertises 380-plus voices across 75-plus languages and variants, and one widely used voice platform’s public model list runs to 74 languages. Whichever catalog you pick, the arithmetic lands in the same place — about one percent of the world’s languages have a voice a product team can actually ship. The other ninety-nine percent are silent.


This post is about that gap — and how we close it at[Luel Labs](https://www.luel.ai/lab) : what “low-resource” really means once you look at the data, why voice — not text — is the interface that matters most for exactly these communities, and what we’re building to ship voices into the silence.


## The gap, in numbers


The research picture is unusually consistent. When Meta launched its Massively Multilingual Speech project in 2023, its own framing was that speech technology covered roughly 100 languages. MMS pushed open research TTS to 1,107 — a real landmark — but with one single-speaker model per language, trained on an average of 32 hours of New Testament readings. That’s the state of the art for most of the world: the gap has moved from *no model exists* to *no product-grade voice exists* .


Languages spoken vs. languages with a voice Documented languages (UNESCO)


8,324


Open research TTS (Meta MMS)


1,107


Commercial catalogs (typical)


74


UNESCO’s Atlas documents 8,324 languages; Meta’s MMS opened research TTS to 1,107 with a single-speaker voice each; a widely used commercial catalog lists 74. The sliver is where products ship; the rest is silent.


Who that gap hurts is documented, not hypothetical. Stanford HAI’s 2025 research calls it systematic exclusion: language AI works beautifully for the world’s 1.5 billion English speakers and degrades as data thins, taking economic and educational opportunity with it. A March 2026 benchmark study puts it in one line — current models “systematically underserve” low-resource language communities. UNESCO’s standing estimate is that fewer than 2% of languages have any real online presence, a figure that explicitly counts text-to-speech among the missing services — and of the 8,324 languages its Atlas documents, around 40% are at risk, with one disappearing, on average, every two weeks.


## What “low-resource” actually means


Start with what the word hides. “Low-resource” sounds like a euphemism for “few speakers,” and it isn’t: a language with tens of millions of speakers can be low-resource while a language a fraction of its size sits comfortably in the commercial catalogs. The resource in question is data — recorded, transcribed, clean, single-speaker audio, and text that matches how people actually talk — and for most languages it barely exists.


Two examples make the point better than any definition. Bhojpuri is spoken by around 55 million people in India and Nepal — and a 2025 survey of the entire South Asian research literature found four papers on it. Four. It also sits outside the 22 scheduled languages that anchor India’s official language programs, so the national platforms largely pass it by. Hausa is bigger still: West Africa’s great lingua franca, roughly 94 million speakers, a tonal language written in two different systems — the Latin-based Boko and the Arabic-derived Ajami. One language, two orthographies, pitch that changes word meaning: everything a text-to-speech front-end must resolve before it can say a single word, and very little curated data to learn it from.


Scale of a different kind fails too. Arabic’s split between the written standard and the spoken dialects means the data that exists is overwhelmingly the register nobody grows up speaking: the team behind Qatar’s Fanar platform states flatly that dialectal Arabic remains significantly underrepresented in language models, and the UAE’s Falcon-Arabic gives the same diglossia rationale. Hundreds of millions of speakers — and the everyday spoken language still counts as low-resource.


This is the terrain we chose deliberately. Our first voices at Luel span the whole spectrum. Masri — the Egyptian Arabic of some 111 million people, the most widely spoken vernacular in the Arabic world, and precisely the spoken register that generic “Arabic” systems don’t cover. Darija — the Moroccan Arabic of roughly 40 million, a spoken-first language with no settled standard, written in two competing scripts. Tibetan — where a single, thousand-year-old written standard serves several often mutually unintelligible spoken dialects, so a voice model must decide whose voice the text gets. And Dzongkha — the national language of Bhutan, with around 640,000 speakers, among the smallest national languages on Earth. Two vernacular giants no mainstream product speaks, and two preservation-critical Himalayan languages: between them, nearly every hard problem in low-resource synthesis.


## Why voice, and not just text


The case for closing the gap with voice, rather than with more text, starts with who text excludes. UNESCO’s count is that 739 million adults worldwide still cannot read and write — nearly one in ten, two-thirds of them women. For that population, every text-first product is a locked door, and a voice interface is the handle. The exclusion compounds precisely where the language gap is widest: the regions with the most low-resource languages are also, broadly, the regions where literacy is lowest — so the people most likely to be failed by text are also the people whose languages the voice industry hasn’t built.


Then there’s register and modality. Many of the world’s most-spoken tongues are spoken far more than they are written — Arabic’s everyday dialects being the canonical case — and for a language that lives primarily in the mouth and the ear, audio is its native modality. A voice interface isn’t an accessibility add-on there; it is the interface.


And accessibility is not a niche. The WHO counts at least 2.2 billion people with near or distance vision impairment; screen readers, audiobooks, and spoken navigation all run on text-to-speech — and they only include you if the voice speaks your language.


## How Luel bridges the gap


Everything above reduces to one bottleneck: data. Not text scraped from the web — recorded, transcribed, quality-checked speech from native speakers, in languages where no convenient corpus exists. That is the thing the market never built. It’s the thing we did.


Luel’s data engine is a global network of more than 750,000 contributors completing audio, video, and speech tasks. Because the network reaches all parts of the world, we can collect speech from the communities that actually use a language — the dialects as they are really spoken, in the accents and registers the catalogs skip — and shape that collection to what a voice model needs, rather than making do with whatever audio happens to exist.


Volume alone doesn’t make a voice; quality does. Every contribution passes through our QA pipeline before it comes anywhere near a training set, so the corpora behind our models are clean, verified, native speech — not found audio of uncertain provenance. Contributors everywhere, quality control between them and the model: that combination is what turns “low-resource” into “enough.” It is the engine behind our Masri, Darija, Tibetan, and Dzongkha voices, and the reason we can keep aiming at the languages the market skipped.


The text-to-speech gap is enormous, well-documented, and — with the right data engine — newly tractable. That’s why we’re building.


New voices and public benchmarks continue on[Luel Labs](https://www.luel.ai/lab) .
