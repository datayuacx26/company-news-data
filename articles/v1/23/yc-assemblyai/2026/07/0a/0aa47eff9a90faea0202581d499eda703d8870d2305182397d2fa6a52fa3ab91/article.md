---
schema_version: "1.0.0"
document_id: "0aa47eff9a90faea0202581d499eda703d8870d2305182397d2fa6a52fa3ab91"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/universal-2-conversational-intelligence"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:7801692bdf9be90755d10b604f2a361c855ccc15e6ecfe98f8d8469721dc37ac"
---

# Universal in Action: Transforming Conversational Data Across Industries

Conversational Intelligence is a rapidly growing field, with a compound annual growth rate estimates generally around 25% over the next decade, depending on the source. This high growth rate is driven, in part, by the fact that Conversational Intelligence has the potential to be *dramatically* transformed by recent advances in AI, and especially Voice AI. There is a wealth of information in conversational data that was once either too time consuming or costly (or both) to extract, if it was even possible.


Projected Conversational Intelligence market size ([source](https://www.businessresearchinsights.com/market-reports/conversation-intelligence-platform-market-102311?ref=assemblyai.com) )


Now that these advances Voice AI have made it possible, in just a few years, to extract this information in a robust and cost effective way, Conversational Intelligence companies are rushing to build rich user experiences on top of these AI capabilities. In fact, our recent[AI insights report](https://www.assemblyai.com/reports/2024-insights-report?ref=assemblyai.com) found that **the top use-case for AI was in augmenting customer experiences** . Whether this manifests as AI-augmented customer support flows or AI-powered telemedicine platforms, the common thread is that AI is being used to make conversations more **efficient** , more **effective** , and more **valuable** .


Recently, we released[Universal-2](https://www.assemblyai.com/universal-2?ref=assemblyai.com) - a best-in-class Speech-to-Text model that makes huge strides in the accuracies relevant to practical applications. While Universal-2 maintains its predecessor's position as best-in-class on canonical metrics like[Word Error Rate (WER)](https://www.assemblyai.com/blog/how-to-evaluate-speech-recognition-models/#evaluation-metrics-for-speech-recognition-models) , it was built to address the gap between[how accuracy is usually measured](https://www.assemblyai.com/blog/universal-2-delivers-accuracy-where-it-matters/) and what actually matters where the rubber meets the road for end-user applications. In other words, Universal-2 was built specifically to power practical use cases rather than focusing on incremental improvements to academic metrics. Let's see how Universal-2 can make (and has already made) an impact in Conversational Intelligence verticals ranging from sales coaching to healthcare administration.


## Accurate alphanumerics


One type of linguistic entity of particular importance to real world use cases is **alphanumerics** - strings of text composed of numbers and letters. At first this may sound like an obscure thing to focus on, but once you start looking for them you'll find alphanumerics everywhere in modern applications - customer IDs, cell phone numbers, email addresses, all of the essential means by which we identify and contact each other *are* alphanumerics. While many providers fail to consider, measure, or report alphanumeric performance, Universal-2 was specifically designed to accurately capture such alphanumeric strings.


Here we can see a side-by-side comparison of[Universal-1](https://www.assemblyai.com/blog/announcing-universal-1-speech-recognition-model/) and Universal-2 when transcribing[this audio file](https://storage.googleapis.com/aai-web-samples/number_pattern.mp3?ref=assemblyai.com) . We can see that Universal-2 accurately transcribes this challenging sequence of words and numbers:


Universal-1's output (red) compared to Universal-2's output (green), differences are highlighted. Universal-2 displays an ability to accurately capture alphanumerics.


Here's the code to run this transcription yourself with Universal-2 - you can get an API key to run this transcription for free[here](https://www.assemblyai.com/dashboard/signup?utm_source=website&utm_medium=blog&utm_campaign=universal-2-in-action) . You can also try uploading a file to our[Playground](https://www.assemblyai.com/playground?ref=assemblyai.com) to see how Universal-2 performs on your own files without needing any code.


*` # you'll need to \`pip install assemblyai\` in a terminal firstimport`*` assemblyai` *` as`*` aai


aai.settings.api_key = "YOUR_API_KEY"


transcript = aai.Transcriber().transcribe("https://storage.googleapis.com/aai-web-samples/number_pattern.mp3")


` *` print`*` (transcript.text)
`


And the benefits of improved alphanumerics performance are not just theoretical.[CallRail](https://www.callrail.com/?ref=assemblyai.com) is a leader in call tracking, focusing on improving customer experiences by augmenting the process with AI. They rely on Universal-2's alphanumerical accuracy to improve their troubleshooting efficiency and lower time-to-resolution for their customers.


In another industry, a fast-growth telehealth startup is working to modernize mental health care by expanding access to mental health treatment. They rely on Universal-2's accurate transcription of personal details and medical codes to efficiently and accurately manage their healthcare administration, reducing errors in patient records and expediting insurance claim processing so they can help people struggling with mental health at scale in an affordable way. And, as always, they can see our security certifications in our[trust portal](https://app.vanta.com/assemblyai/trust/7n80syl8zln1bn1qm3x8eg?ref=assemblyai.com) to know that their patients' information is being responsibly handled.


## Faithful proper nouns


In addition to alphanumerics, proper nouns are of particular significance in industry use-cases. In contrast to alphanumerics, it's easier to immediately see why faithfully transcribing proper nouns is important to businesses - it's the difference between transcribing your own company's name accurately or not, or getting a prospective customer's name right or not. Again, despite being highly important to useable transcripts, the performance of Speech-to-Text models on these important linguistic entities is often not reported.


While Universal-2's predecessor, Universal-1, was competitive with the state-of-the-art on this front, a central goal of ours when building Universal-2 was pushing proper noun performance further. **Universal-2 achieves top-of-the-line proper noun accuracy** . See the results yourself for[this audio file](https://storage.googleapis.com/aai-web-samples/proper_nouns_example.mp3?ref=assemblyai.com) :


Universal-1's output (red) compared to Universal-2's output (green), truncated for brevity. Differences are highlighted. Universal-2 displays an ability to accurately capture proper nouns.


Here's the code to run this transcription yourself - again you can get an API key for free[here](https://www.assemblyai.com/dashboard/signup?utm_source=website&utm_medium=blog&utm_campaign=universal-2-in-action) if you want to run it.


aai.settings.api_key = "YOUR_API_KEY"


transcript = aai.Transcriber().transcribe("https://storage.googleapis.com/aai-web-samples/proper_nouns_example.mp3")


` *` print`*` (transcript.text)
`


Universal-2's upgrade to proper noun performance impacts a huge number of industry verticals. For example, another of our customers[Jimminy](https://jiminny.com/?ref=assemblyai.com) is building a leading sales conversational intelligence platform, so accurate transcription of proper nouns, whether they be contact names, company names, or something else, is absolutely critical. Universal-2's impact has been substantial - from refining sales strategy to closing deals, Universal-2's accurate identification of product names and client details has led to more effective sales training.


## Proper formatting and casing


The raw output of Speech-to-Text models is not suitable for use cases in which humans will directly interact with the transcript, as is often the case in Conversational Intelligence applications. In such cases the raw transcripts should be passed through formatting and casing models to map a technically-accurate but abrasive transcript into a useable transcript that is suitable for human reading, not just machine processing. Look at the difference in readability between the two outputs below, despite them having the same WER, for[this](https://callhounds.com/sample-call-center-recording/?ref=assemblyai.com) public file:


**No formatting and casing:**


` call is now being recorded good afternoon elkins builders yeah hi i'm calling speak to someone about building a house and a property i'm looking to purchase oh okay great let me get your name what's your first name please kenny and your last name lindstrom it's l i n d s t r o n thank you and may i have your callback number it's six one zero two six five one seven one five that's six one zero two six five one seven one five yes and where is the property that you're looking for an estimate on it's in westchester i haven't purchased the land yet i'd like to see if i could get an estimate or have them take a look at it before i do okay no problem is there a good time to reach you with this number or is that at any time that's my cell phone if they could call me back today that would be great okay no problem i'll pass your message along and somebody should be getting back to you this afternoon great thank you so much you're welcome and thank you for calling elkins builders bye
`


**With formatting and casing:**


` Call is now being recorded. Good afternoon. Elkins Builders. Yeah, hi. I'm calling speak to someone about building a house and a property I'm looking to purchase. Oh, okay, great. Let me get your name. What's your first name, please? Kenny. And your last name? Lindstrom. It's L, I, N D S T R O N. Thank you. And may I have your callback number? It's 610-265-1715. That's 610-265-1715. Yes. And where is the property that you're looking for an estimate on? It's in Westchester. I haven't purchased the land yet. I'd like to see if I could get an estimate or have them take a look at it before I do. Okay, no problem. Is there a good time to reach you with this number or is that at any time? That's my cell phone. If they could call me back today, that would be great. Okay, no problem. I'll pass your message along. And somebody should be getting back to you this afternoon. Great. Thank you so much. You're welcome. And thank you for calling Elkins Builders. Bye.
`


If we add in[Speaker Diarization](https://www.assemblyai.com/docs/speech-to-text/speaker-diarization?ref=assemblyai.com) (or in this case potentially[Channel Diarization](https://www.assemblyai.com/blog/multichannel-speaker-diarization/) ) on top of this, the difference in readability becomes even more stark - look at the difference in clarity between the transcript below and the transcript with no formatting and casing:


` Speaker A: Call is now being recorded. Good afternoon. Elkins Builders.
Speaker B: Yeah, hi. I'm calling speak to someone about building a house and a property I'm looking to purchase.
Speaker A: Oh, okay, great. Let me get your name. What's your first name, please?
Speaker B: Kenny.
Speaker A: And your last name?
Speaker B: Lindstrom. It's L, I, N D S T R O N. Thank you.
Speaker A: And may I have your callback number?
Speaker B: It's 610-265-1715.
Speaker A: That's 610-265-1715.
Speaker B: Yes.
Speaker A: And where is the property that you're looking for an estimate on?
Speaker B: It's in Westchester. I haven't purchased the land yet. I'd like to see if I could get an estimate or have them take a look at it before I do.
Speaker A: Okay, no problem. Is there a good time to reach you with this number or is that at any time?
Speaker B: That's my cell phone. If they could call me back today.
Speaker A: That would be great. Okay, no problem. I'll pass your message along. And somebody should be getting back to you this afternoon.
Speaker B: Great. Thank you so much.
Speaker A: You're welcome. And thank you for calling Elkins Builders.
Speaker B: Bye.
`


Beyond being easier on the eyes, formatting can have a big impact on downstream processing. Transcripts are just the *start* of the processing pipeline in modern applications - additional processing by e.g. LLMs is what powers many user-facing features. What happens when the *input* to this pipeline isn't properly formatted?


The technical answer is that these errors propagate down the pipeline to cause downstream errors, but let's borrow the joke stapled to every English teacher's door across the country to illustrate the point:


Okay, that may be a bit of an extreme example, but the underlying principle holds. Punctuation is not just a convenience to human readers - it's an **intrinsic part** of the meaning of language. While it may *seem* like a nice-to-have, it contains critical information that is best not thrown out.


And the same applies with casing. Properly casing a transcript is the difference between saying "she passed the BAR" and "she passed the bar" - is she an attorney or a pole vaulter? While this is a simple example, it again highlights the point at large. Incorrect casing has the potential cause errors in user-facing features, and these errors could be the difference between, for example, a successful meeting and a missed connection in a sales intelligence platform.


Universal-1 was already a best-in-class model for formatting and casing, but we again sought to, and did, push performance even further with Universal-2 - check out the results below:


Universal-1's output (red) compared to Universal-2's output (green), differences are highlighted. Universal-2 displays an ability to properly format challenging strings of words.


Universal-2 prevents` website@traverofcharleston.com` from being directed through downstream processing for emails and potentially causing errors, instead transcribing it properly as` website at travelerofcharleston.com` .


Here's the code to run this transcription yourself:


aai.settings.api_key = "YOUR_API_KEY"


transcript = aai.Transcriber().transcribe("https://storage.googleapis.com/aai-web-samples/formatting.flac")


` *` print`*` (transcript.text)
`


Universal-2's formatting and casing improvements are already making an impact with Conversational Intelligence companies.[Fireflies](https://fireflies.ai/?ref=assemblyai.com) builds a collaboration platform that helps businesses track information and transform conversations into actionable data. It can automatically transcribe calls and meetings, summarize them to distill the essential information for the future or those who couldn't make it, and makes all of this information searchable and analyzable


Formatting and casing is crucial here, not only to make reading easier, but for understanding the semantics of what's being said. When you are ingesting hundreds or thousands of hours of conversations for analysis, these finer details matter. Universal-2's ability to capture these details is part of what allows Fireflies to help its customers, for example, avoid missed action items.


Learn more about Fireflies' journey here:


## [#](https://www.assemblyai.com/blog/universal-2-conversational-intelligence/#try-universal-2) Try Universal-2


Universal-2 is generally available today and is the default model used for English files sent to our API. You can check out our[Playground](https://www.assemblyai.com/playground?ref=assemblyai.com) to test Universal-2 (as well as our[Audio Intelligence models](https://www.assemblyai.com/docs/audio-intelligence?ref=assemblyai.com) and[LeMUR](https://www.assemblyai.com/docs/lemur?ref=assemblyai.com) ) in a no-code way. We also have integrations with various no-code or low-code tools like[Make.com](https://www.assemblyai.com/docs/integrations/make?ref=assemblyai.com) and[Activepieces](https://www.assemblyai.com/docs/integrations/activepieces?ref=assemblyai.com) , so try Universal-2 out there if those tools are part of your workflow.


Universal-2 is easy to integrate directly into your application, too. Get started with our[SDKs](https://www.assemblyai.com/docs/?ref=assemblyai.com) to transcribe in 4 lines of code:


*` import`*` assemblyai` *` as`*` aai


transcriber = aai.Transcriber()
transcript = transcriber.transcribe("path/to/your/file.wav")


` *` print`*` (transcript.text)
`


Then head over to our[Docs](https://www.assemblyai.com/docs/?ref=assemblyai.com) to learn more about our models, check out some[sample code](https://www.assemblyai.com/docs/guides?ref=assemblyai.com) , or take a look at our[API reference](https://www.assemblyai.com/docs/api-reference/overview?ref=assemblyai.com) . Alternatively, get inspired on our[YouTube channel](https://www.youtube.com/c/AssemblyAI?ref=assemblyai.com) or[Blog](https://www.assemblyai.com/blog/) .


## Final words


Universal-2 makes substantial improvements in the accuracies relevant to real-world use cases like Conversational Intelligence, accuracies that are often overlooked in traditional evaluation pipelines. Learn more about the details of Universal-2 in our[Universal-2 research post](https://www.assemblyai.com/research/universal-2?ref=assemblyai.com) , or compare it to other models on our[benchmarks](https://www.assemblyai.com/benchmarks?ref=assemblyai.com) page. Alternatively check out our blog to see[how Universal-2 compares to Whisper](https://www.assemblyai.com/blog/comparing-universal-2-and-openai-whisper/) .
