---
schema_version: "1.0.0"
document_id: "9d02ae39cd2c024326270d953be4cd4c3d6ff360dae288b8ff92968f735eb3f1"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/streaming-keyterms-prompting"
published_at: null
first_seen_at: "2026-07-24T08:06:00.112884+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:978423e8613dac547d6db13fa329bf1bf8a34a1356950165ca46d7aac0621af8"
---

# Introducing Keyterms Prompting to Streaming STT: Never miss the words that matter most

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


Boost transcription accuracy for your critical vocabulary - product names, people, and industry terms - in real-time.


Voice agents stumble on "Baconator." Meeting transcriptions butcher executive names. Medical scheduling bots confuse "Dr. Rodriguez" with "Dr. Rogers."


These aren't edge cases - they're daily failures that frustrate users and erode trust in voice AI.


Today, we're launching Keyterms Prompting for Universal-Streaming: a simple, powerful way to ensure your most important words are transcribed perfectly. Just pass a list of up to 100 custom terms, and watch accuracy soar for the vocabulary that matters to your business.


[\[Try Keyterms Prompting now →\]](https://www.assemblyai.com/playground)


## The challenge with domain-specific vocabulary


Every industry has its language. Restaurants have menu items. Healthcare has doctor names and medical terminology. Tech companies have product names and internal jargon. These domain-specific terms - often unique spellings, proper nouns, or specialized vocabulary - present a natural challenge for even the best speech recognition system.


The impact compounds in real-time streaming scenarios:


- Voice agents mishear orders, leading to incorrect orders and frustrated customers
- Meeting transcriptions mangle participant names, making notes unsearchable
- Medical schedulers book appointments with the wrong doctors
- Customer service bots fail to understand product names that customers reference


Until now, achieving high accuracy for domain-specific terms meant choosing between expensive solutions that still missed critical words, or complex workarounds that barely worked. While other providers charge premium prices for partial accuracy, we deliver superior recognition at just $0.04/hour (added to our $0.15/hour base rate). Accurate transcription of your critical vocabulary shouldn't break your budget.


## Best-in-class accuracy for real-time transcription


We tested Keyterms Prompting across real-world streaming sessions containing domain-specific vocabulary. The improvements are significant:


Universal-Streaming with Keyterms Prompting achieves 21% better accuracy on average compared to Deepgram Nova-3 when transcribing critical domain-specific terms. See the missed entity rate comparison (MER) below. Lower is better.


*Testing was conducted on diverse datasets, including general vocabulary from LibriSpeech with named entities, medical conversations from real healthcare interactions, and complex medical domain terminology from specialized clinical settings.*


What this means for your voice agents:


- Fewer repeated questions to clarify misheard terms
- Higher task completion rates on first attempt
- More natural conversations without frustrating corrections


## Real-world impact across industries


### Food service: Perfect order accuracy starts with perfect transcription


For food service and similar platforms, menu accuracy isn't a nice-to-have - it's essential. A misheard "Whopper" becomes a "Wrapper," turning a satisfied customer into a support ticket.


With Keyterms Prompting, food ordering voice agents can boost accuracy for:


- Menu items specific to each restaurant
- Customization options ("extra crispy," "no pickles")
- Restaurant-specific terminology


**Result:** Fewer order errors, reduced remakes, and happier customers who get exactly what they ordered.


### Healthcare: Every name and term matters


When healthcare scheduling agents mishear doctor names, patients show up for appointments with the wrong physician. It's not just inconvenient - it's unprofessional and potentially dangerous.


Keyterms Prompting ensures accurate capture of:


- Doctor and staff names
- Medical specialties and procedures
- Clinic-specific terminology


**Result:** Accurate appointment booking, fewer clarification calls, and maintained trust in automated healthcare systems.


### Meeting intelligence: Make every conversation searchable


Note-taking applications know that meeting transcriptions are only valuable if they're accurate. When participant names and company jargon are mangled, notes become unsearchable and unreliable.


With dynamic Keyterms Prompting, meeting tools can:


- Boost participant names at the start of each meeting
- Include company-specific terms and acronyms
- Adapt vocabulary for different meeting contexts


**Result:** Searchable, professional documentation that captures exactly what was said, by whom.


The example demonstrates how providing key terms (the names Mason, Skye, and Chase) helps the model correctly distinguish between people's names and identical common words in a restaurant ordering scenario.


## Pricing built for scale


At $0.04/hour, Keyterms Prompting delivers enterprise-grade accuracy at a fraction of the cost—67% less than alternatives. Combined with our base streaming rate, you save 65% total while ensuring accurate transcription of your critical terms. Our pricing scales with you: built to enable growth, not restrict it. From 100 to 10,000+ hours monthly, get the vocabulary precision you need without the enterprise price tag.


## Simple integration, powerful results


Utilizing Keyterms Prompting is straightforward. Add a keyterms_prompt parameter to your streaming request with up to 100 terms you want boosted. Each term can be up to 50 characters - perfect for names, products, menu items, or industry-specific vocabulary.


1. Identify your critical terms - Menu items, doctor names, product vocabulary
2. Add the parameter - Include keyterms_prompt in your streaming config
3. See immediate results - Enhanced accuracy on your specified terms


The feature integrates seamlessly with all our SDKs and is available in your in-app playground for testing.


```text
YOUR_API_KEY =   "YOUR-API-KEY"      # Replace with your actual API key


CONNECTION_PARAMS = {
"sample_rate"  :   16000  ,
"format_turns"  :   True  ,    # Request formatted final transcripts
"keyterms_prompt"  : json.dumps([  "Keanu Reeves"  ,   "AssemblyAI"  ,   "Universal-2"  ])
}
API_ENDPOINT_BASE_URL =   "wss://streaming.assemblyai.com/v3/ws"
API_ENDPOINT =   f"  {API_ENDPOINT_BASE_URL}  ?  {urlencode(CONNECTION_PARAMS)}  "


```


The enhancement applies both before, and after turn completion, ensuring your critical terms are captured accurately without impacting streaming latency. It's that simple - no model retraining, no complex configuration, just immediate accuracy improvements where you need them most.


## Get started today


Your customers expect voice AI to understand the words that matter to them - their doctor's name, their favorite menu item, their company's products. Keyterms Prompting ensures you never let them down.


Three ways to get started:


1. [Test in the Playground:](https://www.assemblyai.com/playground) Try Keyterms Prompting with your audio and vocabulary and see the difference immediately
2. [Get started with our docs:](https://www.assemblyai.com/docs) Complete implementation guide with code examples for all SDKs
3. [Talk to our team:](https://www.assemblyai.com/contact/support) Get personalized recommendations for your use case


Start building with Universal-Streaming


Start building with Universal-Streaming and create voice agents that feel natural, responsive, reliable, and genuinely helpful.


[Get Started Now](https://www.assemblyai.com/dashboard/signup)


‍
