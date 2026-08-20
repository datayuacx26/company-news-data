---
schema_version: "1.0.0"
document_id: "1480ced7a70104cb24780eed257f6107752e9ca492b64a8f4d5c7f688d00cb63"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/ald-improvements"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:d986b8ed46c29de011c0662ec6e21874f8b95613a53dfafeae533235c5c454ce"
---

# Automatic language detection improvements: increased accuracy & expanded language support

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


At AssemblyAI, we're focused on providing the complete speech-to-text API that helps companies build powerful products from voice data. This not only means industry-leading[automatic speech recognition (ASR) accuracy](https://www.assemblyai.com/blog/announcing-universal-1-speech-recognition-model/) , but also means capabilities like[speaker diarization](https://www.assemblyai.com/blog/speaker-diarization-improvements/) ,[speech understanding via LLMs](https://www.assemblyai.com/blog/introducing-lemur-claude-3/) , and automatic language detection, that are necessary to truly get meaning from voice data.


Today, we're announcing updates to our ALD model: increased accuracy, expanded language support, and customizable confidence thresholds. These advancements allow you to build multilingual applications with higher accuracy and more control, which gives your customers better insights and experiences using your product.


## Increased accuracy & expanded language support


Automatic Language Detection (ALD) identifies the spoken language in an audio file before transcription begins, ensuring that multilingual applications function smoothly without manual language selection. Our latest update brings two significant improvements to our ALD model:


1. Expanded our language support from 7 to 17 languages in our Best Tier, adding 10 more languages including Chinese, Finnish, and Hindi among others.
2. Best in class accuracy in 15 of 17 languages when benchmarked against four leading model providers.


These improvements benefit applications like video subtitling, meeting transcription, and podcast processing. The result: more accurate language detection and more reliable, readable transcriptions across all supported languages.


### Accuracy that speaks volumes


To validate our ALD performance and provide you with confidence in its capabilities, we've put it through rigorous testing. Using the industry-standard[FLEURS](https://arxiv.org/abs/2205.12446?ref=assemblyai.com) benchmark, we've measured our model against four leading market providers.


These results translate directly into tangible benefits for your applications:


- **A single API:** Our single API supports 17 languages in Best Tier and 99 in Nano, simplifying your multilingual applications and reducing development and maintenance time.
- **Reliable transcripts:** With industry-leading accuracy in detecting the correct language, you'll spend less time troubleshooting language-related issues.
- **Enter new markets:** Consistent performance across all languages helps you to expand to new markets quickly, without extensive language-specific adjustments.
- **Better user experience:** High-accuracy across all supported languages ensures a better experience for your users.


## Customizable confidence thresholds


In the real world, one-size-fits-all rarely works. That's why we're introducing customizable confidence thresholds, giving you control over language detection.


Whether you’re building a customer service bot or automating international subtitles, you can now set minimum confidence levels to ensure only high-certainty transcriptions are processed. Here’s how it works:


- **Set your threshold:** Define a confidence score that meets your quality standards.
- **Get alerts:** Receive notifications if a language detection falls below your set threshold.
- **Control quality:** Handle low-confidence cases as you see fit, ensuring consistent output quality.


For instance, when automating multilingual call center transcriptions, you might set a high confidence threshold for language detection. This ensures that each call is transcribed using the correct language model, maintaining accuracy in customer interactions and facilitating precise analysis of call content. High confidence thresholds are crucial here to prevent misclassification that could lead to incorrect transcriptions and potentially misinterpreted customer feedback.


On the other hand, for less critical applications, like preliminary content categorization of user-submitted audio files, you might set a lower threshold. This allows you to capture a wider range of content and get a broad understanding of the languages represented in your data set, even if some detections are less certain. You can then use this initial categorization to guide further processing or manual review where needed.


Here’s how you can apply them in practice:


**High-accuracy language detection**


Automatically detect the language of an audio file, with best-in-class model accuracy.


**Visibility into detected language certainty**


Understand the confidence score for a detected language.


**Low-confidence workflows**


Set a minimum confidence threshold, rejecting transcription when language certainty falls short.


` import assemblyai as aai


aai.settings.api_key = "YOUR_API_KEY"
` *` config`*` = aai.TranscriptionConfig(language_detection=True)


transcriber = aai.Transcriber(` *` config`*` =` *` config`*` )
transcript = transcriber.transcribe("portuguese_interview.mp4")
` *` print`*` (transcript.json_response){
...
"language_code": "pt",
"language_confidence": 0.9893
...
}`


These examples show how easily you can integrate our ALD capabilities into your applications with a few lines of code. Here are some practical use cases:


1. **Global meeting transcription:** Accurately document multilingual discussions without manual intervention.
2. **Customer service analytics:** Analyze interactions across regions with precise language classification, enabling accurate sentiment analysis and trend identification.
3. **Adaptive voice assistants:** Create assistants that switch languages based on user input, improving natural language interactions.
4. **Podcast transcription:** Build platforms that accurately transcribe and index content in multiple languages, enhancing searchability and accessibility.


These scenarios showcase how you can leverage improved accuracy, expanded language support, and customizable confidence thresholds in your projects. With these capabilities, you're equipped to build robust, scalable solutions that effectively handle multilingual content.


## **Get Started Today**


For more detailed information about AssemblyAI’s ALD, check out our[docs](https://www.assemblyai.com/docs/speech-to-text/speech-recognition?ref=assemblyai.com#automatic-language-detection) .
