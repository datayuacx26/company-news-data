---
schema_version: "1.0.0"
document_id: "37a003c07e1b62280ce72e64368ce03e0d06a9acc860a190d95d39752cd4ec88"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/advanced-contextual-text-formatting"
published_at: null
first_seen_at: "2026-07-24T08:06:00.112884+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:8db6a09b8faad86a7d5fd287670217aa634968807444a662dfc27fd8a010e767"
---

# Universal model improvements: Introducing advanced contextual text formatting for Spanish and German

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


Upgrade your multilingual transcriptions with formatting that native speakers prefer—now achieving 62.2% preference rates for Spanish and 54.5% for German.


## Multilingual transcription challenges


Getting the words right is only half the battle. Without proper punctuation, capitalization, and formatting, even accurate transcriptions can feel awkward and unprofessional to native speakers.


That's why we've upgraded our Universal Speech-to-Text model with advanced text formatting capabilities specifically designed for Spanish and German—two languages with unique formatting requirements that standard transcription services often overlook.


## Why contextual text formatting matters


Text formatting directly impacts how users experience and trust your transcriptions:


- **Enhanced Comprehension** : Proper punctuation placement guides readers through complex sentences effortlessly
- **Professional Credibility** : Language-specific formatting rules (like German noun capitalization) demonstrate cultural awareness
- **Natural Flow** : Contextual number formatting makes transcriptions feel authentically native
- **Improved Accessibility** : Grammar-aware formatting reduces cognitive load for all users


## Four breakthrough improvements that set Universal apart


Our enhanced model is based on the[Universal all-neural architecture](https://arxiv.org/abs/2501.05948) that we introduced last year for English and trained on a large text corpus for target languages. This yields the following advantages for our model.


### 1. Precision punctuation that respects language nuances


Our enhanced model is now a fully trained end to end neural network that doesn't just understand punctuation logic, but also the cultural and grammatical context it operates in:


- **Spanish-specific punctuation** : Automatic insertion of inverted question marks (¿) and exclamation points (¡) exactly where native speakers expect them
- **Intelligent comma placement** : Context-aware decisions that reflect natural speech patterns
- **Period identification** : Sophisticated algorithms that accurately locate periods and parse sentence boundaries


**Measurable impact** : Our punctuation accuracy shows significant improvements with relative Punctuation Error Rate reduced by 15.3% for Spanish (from 30.8% to 26.1%) and 19.2% for German (from 22.9% to 18.5%).


##### Real-world Spanish punctuation examples:


**New Model:**


```text
"Vale, pues el palo este también está suelto,
como veis, imagino que esto lo pusieron pues
no sé la verdad porque suelto esto no hace nada"
```


**Old Model:**


```text
"Vale pues el palo este también está suelto
como veis imagino que esto lo pusieron pues
no sé la verdad porque suelto esto no hace nada"
```


### 2. Culturally-aware capitalization


Each language has its own capitalization DNA, and Universal respects these differences:


- **German noun recognition** : Nouns properly capitalized according to German grammar rules—from "der Tisch" to "die Universität"
- **Smart proper noun detection** : Advanced identification of names, places, and organizations across both languages
- **Context-sensitive capitalization** : Intelligent decisions based on sentence structure and meaning


**Proven accuracy** : Casing performance metrics demonstrate our model's precision with F1 scores improving to 89.5% for Spanish (5.5% relative improvement) and 95.8% for German (4.9% relative improvement).


##### Real-world German capitalization examples:


**New Model:**


```text
"Herman, in der Nacht, wenn Summer Grill. Ja, Einzahl Grill.
Was ist Einzahl? Heilige. Ja, fang an mit M an, aber da gibt es ein anderes Wort dafür.
Heilige Mama. Nein, italienisches Wort. Maria, Wurscht, was soll's. Madonna ist einerwiesen und springt
auf die Gegend. Krass, richtig."
```


**Old Model:**


```text
"Herman in der nacht wenn summer grill ja einzahl grill
was ist einzahl heilige ja fang an mit m an aber da gibt es ein anderes wort dafür
heilige mama nein italienisches wort maria wurscht was soll's madonna ist einerwiesen und springt
auf die gegend krass richti
```


### 3. Natural number formatting that feels native


Numbers should appear exactly as native speakers write them:


- **Contextual representation** : Knows when to spell out "mil" versus using "1000" in Spanish contexts
- **Cultural conventions** : Follows regional preferences for number formatting
- **Seamless integration** : Numbers flow naturally within the text, enhancing readability


**Breakthrough ITN performance** : This is where our model truly shines. Inverse Text Normalization (ITN) improvements show dramatic gains:


- **Spanish** : ITN-WER reduced by 44.5% (from 46.1% to 25.6%)
- **German** : ITN-WER reduced by 70.3% (from 54.2% to 16.1%)


These represent a fundamental breakthrough in accurately formatting numbers, dates, and proper nouns within context.


#### **Real-world number formatting examples:**


##### Spanish number improvements:


**New Model:**


```text
"Mil gracias, de verdad, mil gracias por seguirme. Estoy muy feliz,
estoy muy contento porque ya somos más de 800 mil personas"
```


**Old Model:**


```text
"1000 gracias, de verdad, 1000 gracias por seguirme. Estoy muy feliz,
estoy muy contento porque ya somos más de 800000 personas"
```


##### German number and time formatting:


**New Model:**


```text
"Viertel Teelöffel Pfeffer und ein halber Teelöffel Oregano. So, das waren alle Zutaten.
Die vermischen wir jetzt nur noch 10 Sekunden lang mit Linkslauf auf der Stufe 3. Ja,
und fertig ist die Pizzamasse. Schaut euch das mal an."
```


**Old Model:**


```text
"Viertel Teelöffel Pfeffer und ein halber Teelöffel Oregano so das waren alle Zutaten
die vermischen wir jetzt nur noch 10 s lang mit Linkslauf auf der Stufe dritte ja
und fertig ist die Pizzamasse schaut euch das mal an."
```


### 4. Grammar-first formatting logic


Being trained on a large text corpus, our model thinks like a native speaker:


- **Structural awareness** : Understands complex German sentence structures and Spanish syntactic patterns
- **Holistic formatting** : Makes decisions based on entire sentences, not just individual words
- **Cultural authenticity** : Formats text the way educated native speakers actually write


## Chosen by humans


In blind human evaluation studies comparing against our previous formatting model, native speakers consistently chose Universal's formatting:


- **Spanish transcriptions** : Preferred 62.2% of the time
- **German transcriptions** : Preferred 54.5% of the time


**Validation across industry benchmarks** : Our improvements aren't just visible in our internal tests—they're validated on widely-recognized open-source datasets including EUROPARL and MLSUM, ensuring consistent performance across diverse content types from parliamentary proceedings to news summaries.


## Seamless integration, immediate impact


The best part? These improvements are already live:


- **Zero integration effort** : Works with your existing API implementation
- **No performance impact** : Same lightning-fast processing speeds
- **No additional cost** : Premium formatting included at standard pricing
- **Automatic enhancement** : All Spanish and German transcriptions instantly upgraded


## See the difference for yourself


Don't just take our word for it. Experience how Universal transforms your Spanish and German transcriptions:


1. **Try it instantly** : Upload audio to our[Playground](https://www.assemblyai.com/playground) and see the results
2. **Test with your content** :[Use your existing integration](https://www.assemblyai.com/dashboard/login) —improvements apply automatically
3. **Start free** : New users get $50 in credits to explore Universal's capabilities


Ready to deliver world-class multilingual transcriptions?


Our Universal model supports over 99 languages. Whether you're building for Spanish and German speakers today or planning global expansion tomorrow, Universal scales with your ambitions.


[Sign Up For Free](https://www.assemblyai.com/dashboard/signup/?ref=assemblyai.com)
