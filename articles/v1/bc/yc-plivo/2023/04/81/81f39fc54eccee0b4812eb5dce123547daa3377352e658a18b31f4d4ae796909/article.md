---
schema_version: "1.0.0"
document_id: "81f39fc54eccee0b4812eb5dce123547daa3377352e658a18b31f4d4ae796909"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/why-transcribe-calls/"
published_at: "2023-04-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:d683fe867428e3bf0dfa0f691986e07891cec7df7c7a0a0b36b109566bfc37a5"
---

# Why You Should Transcribe Your Calls

Plivo’s Voice API includes a feature you may not be taking full advantage of: call transcription.


Call transcription is the process of converting recorded phone conversations into written words. It can help you in a number of ways. With a transcript, anyone can search or visually scan call conversations instead of listening to an entire recording — a process that’s more efficient than voice call monitoring. With phone call transcription you can:


1. **Improve agent performance** — Managers and agents can search and organize transcribed calls based on outcomes, and learn from them to create best practices documents or knowledge base articles. Transcripts can help you improve grading, quality monitoring, and agent and team performance. They can also help managers develop personalized training objectives for agents to improve their performance over time. Meanwhile, sales agents can learn from transcripts to improve their customer service, as well as identify cross- and upsell opportunities and identify effective ways to move leads further along the pipeline.
2. **Reduce training time** — You can share transcriptions among reps and during the new-hire onboarding process. They can serve as example scripts that illustrate what a rep should or shouldn’t say to a customer.
3. **Do post-facto keyword analytics** — Transcripts make it easy to analyze which keywords appear in call conversations and how frequently, so you can learn what your customers are saying and use that information to make better decisions. For instance, if you can understand what aspects and features of a product customers are happy or unhappy about, you can better prioritize efforts to improve the product.
4. **Increase accessibility** — For the hearing-impaired, transcripts make absorbing information easier. Inclusive practices such providing transcripts are not only good business, they may also be the law. For instance, in the US, Section 504 of the Rehabilitation Act of 1973 says, “No otherwise qualified individual with a disability in the United States . . . shall, solely by reason of her or his disability, be excluded from the participation in, be denied the benefits of, or be subjected to discrimination under any program or activity receiving Federal financial assistance . . . .” The Office for Civil Rights enforces Section 504 in programs and activities that receive federal financial assistance from the Education Department, including public school districts and institutions of higher education. If your business has a presence in education, transcriptions may be required.
5. **Enhance customer experience** — For contact centers especially, customer satisfaction is crucial, and the best way to deliver on customer satisfaction is to have customer insights. Call transcripts unlock those insights, giving you an easy-to-review format and high-level perspective, which is especially useful when you have a lot of calls covering a number of subjects. Call transcription makes it simple to review information and discern trends, and identify instances where you should go back and listen to specific calls for more context.


## The price is right


You might be willing to pay a lot to gain all of the benefits of speech-to-text, but with Plivo you don’t have to. Our speech transcription service is surprisingly inexpensive. Plivo charges[less than a penny per minute](https://plivo-webflow.webflow.io/voice/pricing) of recorded speech —[a fraction of what most cloud communication services charge](http://plivo-webflow.webflow.io/blog/call-transcription-price-reduction) — which makes it a trivial decision to say yes and gain all of the benefits.


However, before you transcribe any calls, make sure that doing so is legal in your locale. Some jurisdictions ban not only transcribing calls but even recording calls, or require informed consent by some or all parties in a conversation. Consult your friendly neighborhood lawyer.


## How to transcribe calls


Now that you know why to use speech-to-text transcription, let’s talk about how: Simply provide an additional parameter to the[Record Call API](https://www.plivo.com/docs/voice/api/call/record-calls/) when you initiate a call. The[transcription_url parameter](https://www.plivo.com/docs/voice/api/call/record-calls/#start-recording-a-call) defines the webhook URL to which the transcription should be posted. After the call completes, Plivo will post the transcribed text to the URL you specify, along with other parameters about the recording, as you can see on our API documentation page.


It’s really that simple and that inexpensive. If you’re already using Plivo’s[Voice API](https://plivo-webflow.webflow.io/voice) but haven’t yet tried transcription, think about places where it might come in handy. If you’d like to get started,[sign up for a free trial account](https://console.plivo.com/accounts/register/) and see all that[Plivo](https://www.plivo.com/) can do.
