---
schema_version: "1.0.0"
document_id: "be74888d1ca7634bd8c36d73b27c633866c01f5adf38fd8fa5884deeb8884951"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/slam-1-public-beta"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:a6169f8c3bb41daf8ae222059fc2df091d9ba2644f632ed3dedafe2b2cd2c042"
---

# Slam-1 now in public beta: the most powerful prompt-based Speech Language Model to unlock real world outcomes

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


Slam-1 is now in public beta and[ready for you to use today](https://www.assemblyai.com/docs/getting-started/slam-1) .


Slam-1 represents a fundamental shift in speech recognition technology as the world's most powerful Speech Language Model specifically designed for speech-to-text tasks. By combining the powerful reasoning capabilities of large language models with specialized audio processing, Slam-1 doesn't just recognize speech—it understands it. This breakthrough enables you to build powerful products and end-user experiences with accuracy levels previously achievable only through custom model development.


With Slam-1, you get an accurate speech-to-text solution that's customizable via prompts for your specific transcription needs – without the traditional cost, complexity, and time investment of custom model development, so your team can build better, ship faster, and drive tangible results for your users.


## Superior accuracy that humans prefer


Even without any customization, Slam-1 delivers exceptional results compared to our[industry-leading Universal model](https://www.assemblyai.com/universal-2/) . In side-by-side blind tests, two-thirds of human evaluators consistently preferred Slam-1 transcripts for their accuracy, readability, and proper formatting. Furthermore, Slam-1 has over a 72% human preference rating over other recent models like Deepgram’s Nova-3 model.


This human preference translates directly to business value. Better transcripts mean fewer support tickets, higher user satisfaction, increased engagement, and improved retention. For customer-facing applications, transcript quality that humans prefer can significantly enhance your product experience and drive monetization.


**Side-by-side human preference test results between Slam-1 and two other models, evaluated by two external vendors. Only samples where at least two-thirds of the human raters agreed on their ratings were included.**


### Why users prefer Slam-1: more accurate, better formatted, and business-critical detail capture


This strong user preference is backed by concrete improvements in metrics that directly impact transcript readability and usability. When compared to Universal on the below key quantitative metrics, the Slam-1 improvements are clear:


- **Accuracy for key entities:** Reduces error rate in alphanumerics, addresses, emails, and numbers by 12%, 41%, 37%, and 25%, respectively.
- **Formatting accuracy** : 27% reduction in formatting errors (17.2% vs 23.6%) ensuring proper capitalization, punctuation, and spacing.
- **General accuracy** : Maintains the industry-leading accuracy achieved by our Universal model on average WER of 7%, measured on a very diverse data set comprising 205 hours of audio.


***Comparison of Slam-1 and Universal in terms of WER and FWER.***


These improvements are especially noticeable in challenging audio conditions and with specialized terminology, where Slam-1's language understanding capabilities help it make better predictions about what was said.


For instance, in a noisy conference call where a speaker mentions "XeroSync's API integration with ServiceNow," traditional models might produce "zero syncs APR integration with service now," while Slam-1 correctly captures the product name, technical terms, and proper capitalization. The enhanced formatting and accuracy deliver more reliable transcripts increasing performance across downstream workflows.


## Fine-tune and customize Slam-1 to your industry-specific application


Slam-1 is customizable for specific industries and use cases with minimal effort. Rather than spending months developing custom models or implementing complex post-processing rules, Slam-1 offers customization approaches that give you unprecedented control over your transcription results. Check out our[Getting Started Guide](https://www.assemblyai.com/docs/getting-started/slam-1) .


### Provide key terms for Slam-1 to focus on


Whether you need to capture medical terminology (like "myocardial infarction"), technical product names (like "CloudGuard SSO"), legal citations (like "Duran v. Peabody Coal Company"), or industry-specific acronyms, Slam-1's multi-modal architecture understands the semantic meaning and context of the terminology you provide to correctly place focus words in the transcript.


Unlike legacy custom vocabulary, Slam-1 doesn't just recognize these exact keywords and phrases—it comprehends their contextual relevance, understands related terminology, and improves transcription accuracy throughout the entire document.


***By understanding semantic contextual information, Slam-1 is able to capture uncommon words unlike normal Speech-to-Text models, but avoid overoptimizing for rare words as legacy custom vocabulary models do.***


By providing a list of domain-specific terms and phrases you activate Slam-1's contextual understanding capabilities around those concepts. This creates a semantic context awareness that improves recognition not just of the exact terms you specify, but also related terminology, variations, and contextually similar phrases. Functionally this represents fine-tuning per audio file at runtime, allowing you to achieve transcription accuracy previously only possible with custom model development.


***Slam-1 reduces missed entity rate (MER) by 66% with key term prompting. MER is computed as the number of incorrectly transcribed or missed key terms relative to their total occurrence count in the reference transcription*** *.*


For applications requiring specialized terminology recognition, Slam-1's contextual understanding capabilities deliver unprecedented accuracy across diverse domains and can handle up to 1,000 such words.


### Medical doctor consultation:


With a language model's general understanding ability of medical domains, Slam-1 powers medical scribe applications with breakthrough accuracy. By reducing missed entity rate errors by up to 66%, it ensures comprehensive records for continuity of care while helping healthcare organizations capture billable services accurately, decrease denial rates, and reduce compliance risks.


```text
import   requests
import   time


base_url =   "https://api.assemblyai.com"
headers = {  "authorization"  :   "<YOUR_API_KEY>"  }


# select Slam-  1   and specify your audio file
data = {
"audio_url"  :   "https://assembly.ai/sports_injuries.mp3"  ,
"speech_model"  :   "slam-1"  ,
"keyterms_prompt"  : [  "differential diagnosis"  ,   "myocardial infarction"  ,   "hypertension"  ,   "Wellbutrin XL 150mg"  ,   "lumbar radiculopathy"  ,   "bilateral paresthesia"  ,   "metastatic adenocarcinoma"  ,   "idiopathic thrombocytopenic purpura"  ]
}
# submit the transcription request
response = requests.post(base_url +   "/v2/transcript"  , headers=headers, json=data)
```


### Legal proceeding:


For legal proceedings, providing "motion for summary judgment" helps Slam-1 better understand related legal procedures and terminology even if they weren't explicitly included in your prompt.


The higher accuracy in FWER and MER in legal settings translates to reduced review time for transcripts, faster case preparation, and improved searchability of case documentation. Law firms can process depositions and court proceedings more efficiently, allowing attorneys to focus on case strategy rather than correcting transcription errors. The improved capture of case citations and legal precedents also enhances the quality of legal research and discovery processes.


```text
import   requests
import   time


base_url =   "https://api.assemblyai.com"
headers = {  "authorization"  :   "<YOUR_API_KEY>"  }


# select Slam-1 and specify your audio file
data = {
"audio_url"  :   "https://assembly.ai/sports_injuries.mp3"  ,
"speech_model"  :   "slam-1"  ,
"keyterms_prompt"  : [  "motion for summary judgment"  ,   "voir dire"  ,   "amicus curiae"  ,   "Duran v. Peabody Coal Company"  ,   "punitive damages"  ,   "declaratory relief"  ,   "substantive due process"  ,   "Harnett County Superior Court"  ]
}
# submit the transcription request
```


### Sales discovery call:


Similarly, including "service-level agreement" in a sales context improves recognition of related contractual and technical terms throughout the conversation.


For revenue intelligence platforms, this enhanced accuracy delivers tangible downstream advantages. With a 41% improvement in capturing email addresses, 37% better accuracy for contact information, and 27% reduction in formatting errors, sales teams can extract more reliable customer data from calls.


Capturing critical details like pricing ($499 vs $4.99), technical specifications, and follow-up commitments results in more actionable conversation intelligence, better coaching opportunities, and more accurate sales forecasting—directly impacting win rates and customer trust.


```text
import   requests
import   time


base_url =   "https://api.assemblyai.com"
headers = {  "authorization"  :   "<YOUR_API_KEY>"  }


# select Slam-1 and specify your audio file
data = {
"audio_url"  :   "https://assembly.ai/sports_injuries.mp3"  ,
"speech_model"  :   "slam-1"  ,
"keyterms_prompt"  : [  "SaaS implementation timeline"  ,   "service-level agreement"  ,   "XeroSync Pro"  ,   "DataVault Enterprise"  ,   "RBAC permissions"  ,   "CloudGuard SSO"  ,   "quarterly business review"  ,   "FlexiScale pricing tier"  ,   "ThreatShield API"  ,   "RapidDeploy v4.2"  ,   "annual subscription model"  ,   "SecureVault integration"  ]
}
# submit the transcription request
```


These examples demonstrate how Slam-1's contextual understanding transforms transcription across critical business functions. By simply providing relevant terminology, organizations achieve accuracy levels previously possible only through expensive custom model development.


Whether capturing medical terminology, legal citations, or sales conversations, Slam-1 adapts to your specific domain with minimal effort—delivering superior results where accurate speech recognition directly impacts business outcomes.


## Slam-1’s multi-modal advantage


What makes Slam-1 uniquely powerful is its multi-modal architecture that processes audio and language simultaneously. Unlike traditional speech recognition models that focus solely on audio-to-text conversion, Slam-1 brings true language understanding to speech transcription.


Built and optimized specifically for speech-to-text tasks, Slam-1 integrates seamlessly with high-demand features like speaker diarization, timestamp prediction, and multichannel transcription—allowing you to simply drop it in as a replacement to immediately improve accuracy across your existing workflows.


These capabilities deliver measurable business outcomes:


1. Reduced post-processing costs and engineering effort
2. More reliable downstream analytics and automation
3. Improved end-user experience with higher quality transcripts
4. Faster time-to-insight from spoken content


By combining a higher baseline accuracy with powerful customization capabilities, Slam-1 represents the next evolution in Voice AI technology—transforming how organizations work with speech data in applications where accurate transcription directly impacts business results.


## Looking forward


Over the coming weeks, we plan to ship new capabilities for Slam-1. Here are two exciting features currently in development.


### 1. Provide contextual information about the recording


Beyond key term lists, Slam-1 can incorporate contextual information about the recording through a description. This allows the model to understand the broader context of your audio file and make more intelligent transcription decisions. You can provide up to 1,500 words of contextual information, giving the model rich background knowledge about the content, participants, domain, and purpose of the recording.


**Note: this parameter is still in development and doesn't currently have any affect on transcripts**


For example, for legal transcription, this contextual understanding could look like:


```text
import   requests
import   time


base_url =   "https://api.assemblyai.com"
headers = {  "authorization"  :   "<YOUR_API_KEY>"  }


# select Slam-1 and specify your audio file
data = {
"audio_url"  :   "https://assembly.ai/sports_injuries.mp3"  ,
"speech_model"  :   "slam-1"  ,
"prompt"  :   "This is a deposition in the case of Smith v. Acme Corporation, a product liability lawsuit involving an alleged defect in the XJ-5000 power tool that resulted in severe lacerations to the plaintiff's right hand. The deposition will involve questioning of Dr. Elizabeth Chen, an orthopedic surgeon who treated the plaintiff's injuries."
}
# submit the transcription request
```


With this context, Slam-1 will better understand the legal proceeding's nature, the relevant parties, and likely specialized terminology – resulting in more accurate transcription of case details, medical testimony, and legal procedures without requiring you to explicitly list every technical term.


### 2. Ask Slam-1 to capture disfluencies, emphasis, and sentiment


Soon Slam-1 will offer you control over how speech is represented in transcripts. You'll be able to customize the level of detail and style through simple instructions that match your specific needs.


For conversation intelligence platforms analyzing sales calls, for example, this future capability will enable rich insights:


```text
import   requests
import   time


base_url =   "https://api.assemblyai.com"
headers = {  "authorization"  :   "<YOUR_API_KEY>"  }


# select Slam-1 and specify your audio file
data = {
"audio_url"  :   "https://assembly.ai/sports_injuries.mp3"  ,
"speech_model"  :   "slam-1"  ,
"prompt"  :   "Capture disfluencies and repetitions. Identify questions versus statements. Preserve emphasis when speakers stress particular words."
}
# submit the transcription request
```


This level of control allows conversation intelligence platforms to analyze not just what was said, but how it was said – capturing hesitations that might indicate uncertainty, preserving emphasis that highlights key points, and distinguishing questions from statements for better conversation flow analysis.


Additional upcoming features will include emotion detection, more advanced promptability with better instruction following, and various deployment options including our API, deploying on your own servers, and via select inference partners.


## Choosing the right model for your needs


### Slam-1


Slam-1 is priced at $0.37/hour—the same as our Universal model—making it accessible while allowing you to quickly customize the model to your specific context without requiring model retraining or complex engineering. Volume discounts are available for large workloads which you can[contact us about here](https://www.assemblyai.com/contact/) **.**


Whether you're transcribing healthcare consultations, legal proceedings, sales calls, or technical discussions, Slam-1 can adapt to capture the terminology and nuances that matter most to your use case, so you can derive more accurate insights, reduce post-processing costs, and deliver higher-quality results to your end users.


**When to use Slam-1:**


- You need superior accuracy for English-language content (Slam-1 is English-only at this time)
- You work with specialized terminology or domain-specific content that benefits from contextual understanding and prompting
- You need higher reliability when capturing specific entities (names, products, technical terms)
- You prioritize transcript quality and formatting over processing speed and can accept slightly higher latency


### Universal


**When to use Universal:**


- You need broader language support beyond English
- Processing speed and low latency are your top priorities
- You're working with general content that doesn't contain specialized terminology
- You need high-volumes and scale with the lowest custom rates


Both models can be used within the same workflow for different content types, allowing you to select the optimal model based on language requirements, accuracy needs, and turnaround time requirements for each specific use case.


## Get started with Slam-1 today


The public beta of Slam-1 is currently accessible through our[standard API endpoint](https://www.assemblyai.com/dashboard/signup) . Getting started is simple - you'll make requests to the same https://api.assemblyai.com/v2/transcript endpoint using your current API key.


The only change you need to make is to include the speech_model parameter with a value of "slam-1" as shown in the code example below:


```text
import   requests
import   time


base_url =   "https://api.assemblyai.com"
headers = {  "authorization"  :   "<YOUR_API_KEY>"  }


# select Slam-1 and specify your audio file
data = {
"audio_url"  :   "https://assembly.ai/sports_injuries.mp3"  ,
"speech_model"  :   "slam-1"  ,
"keyterms_prompt"  : [  "<YOUR_KEY_TERMS>"  ,]
}


# submit the transcription request
if   response.status_code !=   200  :
print  (  f"Error:   {response.status_code}  , Response:   {response.text}  "  )
response.raise_for_status()


transcript_response = response.json()
transcript_id = transcript_response[  "id"  ]


# poll for the result
polling_endpoint =   f"  {base_url}  /v2/transcript/  {transcript_id}  "
while     True  :
transcript = requests.get(polling_endpoint, headers=headers).json()
if   transcript[  "status"  ] ==   "completed"  :
print  (transcript[  "text"  ])
break
elif   transcript[  "status"  ] ==   "error"  :
raise   RuntimeError(  f"Transcription failed:   {transcript[  'error'  ]}  "  )
else  :
time.sleep(  3  )
```


Try Slam-1 today and experience the next evolution in speech recognition technology. To explore more check out our[Getting Started Guide](https://www.assemblyai.com/docs/getting-started/slam-1) .
