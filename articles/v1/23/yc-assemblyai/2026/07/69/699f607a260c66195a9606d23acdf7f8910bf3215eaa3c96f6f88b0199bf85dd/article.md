---
schema_version: "1.0.0"
document_id: "699f607a260c66195a9606d23acdf7f8910bf3215eaa3c96f6f88b0199bf85dd"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/universal-english-german-spanish"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:10725a3ef49aabce5958e1afba072dc75fa95a545b468ffdae42445d0c703838"
---

# Universal speech-to-text model leads in English, German, and Spanish

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


While the industry continues to focus on broad accuracy metrics, real business value lies in capturing the critical details that power conversation intelligence applications: proper nouns that maintain their meaning, alphanumerics that validate correctly, and formatting that conveys intended meaning across languages.


Today, we deliver further improvements to our best-in-class Universal speech-to-text model family for three industry-critical languages: English, German, and Spanish. This is an improvement over our[October 2024 Universal release](https://www.assemblyai.com/universal-2?ref=assemblyai.com) in terms of latency, accuracy, and language coverage.


The chart below shows Universal's standard error rate compared to the leading models on the market for English, German, and Spanish, demonstrating Universal's leading accuracy across all three languages:


Average word error rate (WER) across languages for several providers.[WER](https://www.assemblyai.com/blog/how-to-evaluate-speech-recognition-models/#word-error-rate) is a canonical metric in speech-to-text that measures typical accuracy (lower is better). Descriptions of our evaluation sets can be found in our October release[blog post](https://www.assemblyai.com/research/universal-2?ref=assemblyai.com) .


Additionally, these improvements to accuracy are accompanied by significant increases in processing speed. Our latest Universal release achieves a **27.4% speedup in inference time** for the vast majority of files (at the 95th percentile), enabling faster transcription at scale.


Universal goes beyond standard benchmarks to[solve the "last-mile" challenges](https://www.assemblyai.com/blog/universal-2-conversational-intelligence/) in speech recognition—the critical details that make transcripts truly useful for business applications. In addition to the standard accuracy, language coverage, and processing speed improvements for all covered languages outlined above, today's Universal upgrades bring further improvements on these last-mile challenges, especially for English audio.


## English "last-mile" improvements


While traditional speech-to-text evaluations focus primarily on word error rate, real-world applications demand more: accurate company names that support agent escalation workflows, properly formatted emails that enable automated follow-ups, and consistent handling of product mentions that power sales analytics.


Building on Universal's already best-in-class English performance, today's upgrades bring significant improvements to these last-mile challenges. Our enhanced model captures and formats important entities like names and email addresses more faithfully than existing solutions on the market, backed by[comprehensive performance testing](https://www.assemblyai.com/blog/how-to-evaluate-speech-recognition-models/) . Here are some of the highlights:


- **Proper nouns:** A 12.5% relative improvement in proper noun accuracy (PNER) from 15.06% to 13.17%, ensuring correct capture of names, brands, and companies
- **Accented speech:** A 5% relative improvement in performance on accented English speech (WER) from 11.4% to 10.8%, delivering better performance across diverse speaking styles


The chart below compares Universal to several other speech-to-text models across a variety of metrics which together constitute a holistic measure of model performance. In contrast to the focus only on Standard ASR accuracy of typical univariate analyses, the suite of measurements displayed below evaluates how well a model handles **key linguistic entities** that are critical in real-world use cases. Each value represents an error rate, so **lower is better** .


Comparative error rates across speech recognition models, with lower values indicating better performance. Descriptions of our evaluation sets can be found in our October release[blog post](https://www.assemblyai.com/research/universal-2?ref=assemblyai.com) .


The chart displays, for each model:


- **Standard ASR:** Classical word error rate (WER) measurement, which probes general accuracy (ASR = Automatic Speech Recognition)
- **Proper noun accuracy** : Proper noun error rate (PNER), which is a metric invented by our research team to probe model performance specifically on proper nouns
- **Alphanumeric accuracy:** Alphanumeric error rate, measured by WER, which probes model performance on alphanumeric strings like telephone numbers and email addresses
- **Accented speech accuracy:** Accented speech error rate, measured by WER, which probes model performance on accented English speech


Universal's strong performance across the entire suite of metrics demonstrates a robustness across practical use cases.


For example, **contact centers** rely on accurately capturing caller information like phone numbers and email addresses, whether it be for inbound sales leads or customer service calls. Universal's strong performance on alphanumerics indicates that these important features are faithfully captured in call transcripts.


Other areas like **sales coaching** benefit from Universal's strong performance on proper nouns, ensuring that entities like names, companies, products, and locations are accurately captured. This accuracy is crucial not only for tactical insights like analyzing customer interactions, tracking competitive mentions, and measuring brand awareness; but also to the bedrock fundamentals of building genuine relationships through attention to detail.


And Universal's[strong text formatting accuracy](https://arxiv.org/abs/2501.05948?ref=assemblyai.com) yields highly readable transcripts that are important to any application - check out an example below ([source audio](https://www.youtube.com/watch?v=U2tZAz9dUjo&ref=assemblyai.com) ).


```text
Welcome to another edition of Traveler TV. Today we  're at the Arthur Ravenel Jr Bridge,
located here. It opened in 2005 and is currently the longest cable stayed bridge in the
Western Hemisphere. The design features two diamond shaped towers that span the Cooper river
and connect downtown Charleston with Mount Pleasant. The bicycle pedestrian paths provide
unparalleled views of the harbor and is the perfect spot to catch a sunrise or sunset. To
walk or bike the bridge, you can park on either the downtown side here or on the Mount
Pleasant side in Memorial Waterfront Park. To learn more about The Arthur Ravenel Jr. Bridge
and other fun things to do in Charleston, SC. Visit our website at travelerofcharleston.com
or download our free mobile app exploring Charleston SC.
```


## How to use Universal for Speech-to-Text


You can try Universal immediately in our[Playground](https://www.assemblyai.com/playground?ref=assemblyai.com) - just submit an English, German, or Spanish audio file and select your features to see Universal's results on your data.


You can also use Universal through our API. Sign up to[get your free API key](https://www.assemblyai.com/dashboard/signup?ref=assemblyai.com) , and then call our API in your preferred language and environment. For example, here is how you can use Universal in Python with our[Python SDK](https://github.com/AssemblyAI/assemblyai-python-sdk?ref=assemblyai.com) . Universal is the default model, and it can be explicitly set by specifying the best model tier. Additionally, you can directly set a language_code, or use our[Automatic Language Detection](https://www.assemblyai.com/blog/universal-english-german-spanish/) :


```text
# pip install assemblyai
import   assemblyai   as   aai


aai.settings.api_key =   "YOUR_API_KEY"
audio_file =   "https://assembly.ai/sports_injuries.mp3"


config = aai.TranscriptionConfig(
language_code=  "en"  ,    # "de", "es"
speech_model=aai.SpeechModel.best
)


transcriber = aai.Transcriber()
transcript = transcriber.transcribe(audio_file, config)


print  (transcript.text)
```


You can use any publicly-accessible remote file, or a local file. In addition to using[our SDKs](https://www.assemblyai.com/docs?ref=assemblyai.com) , you can directly call our API:


```text
curl -X POST https://api.assemblyai.com/v2/transcript \
-H   "Authorization: your-api-key"   \
-H   "Content-Type: application/json"   \
-d   '{"audio_url": "https://assembly.ai/sports_injuries.mp3"}'
```


Additionally, you can specify any of our other features as usual, like[speaker labels](https://www.assemblyai.com/docs/speech-to-text/speaker-diarization?ref=assemblyai.com) . Check out our[Docs](https://www.assemblyai.com/docs?ref=assemblyai.com) to learn more about using Universal and our other available features, or check out our[benchmarks](https://www.assemblyai.com/benchmarks?ref=assemblyai.com) page for a more detailed metrics breakdown.
