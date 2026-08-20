---
schema_version: "1.0.0"
document_id: "b3351310492d146a8426a8b4d12d99069305ca11a38f54b32e8fe68867015a37"
company_key: "yc-recall-ai"
company: "Recall.ai"
source_id: "yc-recall-ai-news-import-5dc65e725852"
canonical_url: "https://www.recall.ai/blog/how-to-build-a-zoom-notetaker"
published_at: "2025-09-22T00:00:00+00:00"
first_seen_at: "2026-07-22T11:03:36.206412+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:77c4f4a380ab3d6b4b2aa58ac32c9e9875dd82a5e951f971ae3aaffabd7335a4"
---

# How to build a Zoom notetaker

**Tl;dr** : I built a Zoom notetaker that joins calls, transcribes conversations, and generates AI-powered summaries. It’s completely customizable, and you can find the source code in[this Github repository](https://github.com/recallai/zoom-notetaker) .


AI notetakers are ubiquitous these days, with companies offering specialized meeting bots that cover every industry and niche imaginable. These are fantastic products for the most part. But sometimes you need a feature that doesn't exist yet, or you just prefer having complete control over your tools, and an out-of-the-box solution just doesn't cut it. When you build your own meeting notetaker, you get to make all the decisions about how it behaves, what it captures, and how it presents information back to you.


In this article, I built my own Zoom notetaker using the[Recall.ai Zoom Meeting Bot API](https://www.recall.ai/product/meeting-bot-api/zoom) , and I'm going to show you exactly how I did it.


## What is a Zoom notetaker?


At its core, a Zoom notetaker is a bot that joins your meetings as a participant, records the conversation, and automatically generates notes, and optionally summaries and action items from what was discussed. A great notetaker is a product in its own right, but it also becomes the data engine for other products. The same conversation data can, for example, update CRM fields after a sales call or feed a medical scribe that writes notes to the EHR and flags patterns for clinician review. These scenarios go beyond taking notes, but they’re powered by the same foundation: conversational context.


> Why not just record the meeting? There are several key differences between a notetaker and just hitting "record" in Zoom.
>
>
> With a notetaker, your app can capture meeting data in real time and immediately after the call. By contrast, Zoom recordings often take as long as the meeting itself to process before you can access the transcript or video.
>
>
> A notetaker also removes human error. There’s no need to rely on someone remembering to press “record.” This is crucial not only if your product is a notetaker, but also if your product depends on meeting data.
>
>
> And finally, a notetaker works across meetings regardless of who the host is or what Zoom plan they’re on, giving you consistent, reliable access to meeting data.


## A preview of what we’re building


Here's what my finished notetaker looks like in action:


[Demo Video](https://www.loom.com/share/3023db7d01754a4ca6636247e95ad750)


This is the workflow of the application:


1. User pastes a Zoom meeting URL into the web app
2. Notetaker bot automatically joins the Zoom call as a participant
3. Bot records and transcribes the meeting
4. When the call ends, the transcript (with speaker names, emails, and timestamps) gets sent to OpenAI for summarization
5. OpenAI returns a structured summary with speaker names and timestamps linked to important moments from the call
6. Users can view the summary, full transcript, and recording in the web app


The end result is a dashboard where you can see all your completed meetings with AI-generated summaries that capture the important stuff: who said what, the decisions that were made, and what happens next. You can even click on different parts of the summary to jump directly to that moment in the video recording. By the end of this article, you’ll understand how I built this, and how you can change this code to suit your own use case.


## Zoom notetaker architecture


The frontend is a Next.js app, and the backend uses Next.js API routes to handle bot creation and process webhooks.


For the actual meeting bot and transcription piece, I used the Recall.ai[Zoom Meeting Bot API](https://www.recall.ai/product/meeting-bot-api/zoom) , which handles all the complexity of joining meetings and capturing transcripts for us. To generate structured summaries from these transcripts I used the Vercel AI SDK with OpenAI.


For simplicity, all data gets stored on the local file system, though you could easily swap this out for your database of choice.


## Step 1: Extracting transcript data from the meeting


This is where many DIY meeting bot projects fall apart. Getting clean, accurate transcripts from Zoom meetings is genuinely hard if you're building without using a meeting bot API. I’ve gone through the process of[building a Zoom bot from scratch](https://www.recall.ai/blog/how-to-build-a-zoom-bot) , and in my experience, delegating this work to a third party is almost always the better option. For this project, I’ll be using the[Recall.ai Zoom Meeting Bot API](https://www.recall.ai/product/meeting-bot-api/zoom) to power my bot.


Using the Recall.ai, the only information I need to provide is the meeting URL. Now I can create a bot using a single API call:


```text
//@title zoom-notetaker/app/api/send-bot/route.ts
const response = await axios.post(
"<https://us-east-1.recall.ai/api/v1/bot/>",
{
meeting_url: meetingUrl,
// configure the bot to transcribe the meeting
recording_config: {
transcript: {
provider: {
recallai_streaming: {},
},
},
},
},
{
headers: {
accept: "application/json",
"content-type": "application/json",
Authorization: `Token ${RECALL_API_KEY}`,
},
}
);


```


This single API call is highly configurable. I can use it to set the bot’s name and image, send chat messages, and more. For example, in this request, I’m telling Recall to use their transcription service (` recallai_streaming` ), which means we get access to a structured transcript that includes participant names, emails, and timestamps for each word as soon as the meeting ends. I can also get this data in real time using webhooks, but that won’t be necessary for this project.


Once the bot is created, it automatically joins the meeting and starts recording. The bot appears as a regular participant, but it handles all the technical complexity of capturing high-quality audio, video, and transcripts.


### Handling webhook notifications


Webhooks from Recall.ai will be crucial for notifying my application about the state of my meeting bots. Before I can receive webhooks for this bot, I need to configure them in the[Recall.ai dashboard](https://us-west-2.recall.ai/dashboard/webhooks) . For local development, I’m using Ngrok to expose my server to the internet so Recall.ai can send webhooks to it.


To set this up in the dashboard, go to the` Webhooks` tab in the sidebar and click` Add Endpoint` . Enter your webhook URL in the` Endpoint URL` field, then scroll down to` Subscribe to Events` and select` bot.done` . Click` Create` to save it.


There are many events I could listen for, but the` bot.done` event is the only one required for this use case. It signals that the meeting has ended and the recording / transcript is ready to be downloaded. When I receive that event, I can fetch the transcript data and send it through my AI summarization pipeline.


```text
//@title zoom-notetaker/app/api/webhook/route.ts
export async function POST(request: NextRequest) {
const { data, event }: WebhookPayload = await request.json();
const botId = data?.bot?.id;
const eventName = event;


*// Find the meeting by botId*
const meeting = allMeetings.find((m) => m.botId === botId);


const response = NextResponse.json({
message: "Webhook received and processing started",
botId,
event: eventName,
});


processWebhookAsync(meeting.id, botId, eventName, data);
return response;
}


```


> Respond to webhooks immediately!
>
>
> You should respond to webhooks immediately with a 200 status once you've received them, then do any processing asynchronously. Webhook providers (including Recall.ai) will retry failed requests, so if your AI summary generation takes too long, the provider may assume the request failed and resend it. This causes duplicate webhook processing, which could result in generating multiple summaries for the same meeting.


### Fetching the transcript


After I receive the` done` webhook, I know that the transcript is available. At that point, I can fetch the latest information for the bot from the Recall.ai API using the[Retrieve Bot](https://docs.recall.ai/reference/bot_retrieve) endpoint. The transcript is located in the` transcript` object in the` media_shortcuts` field of the response from this endpoint. In the data field, you'll see a` download_url` . You can use this URL to download the raw meeting transcript. Here’s an example of an excerpt:


```text
[
{
"participant": {
"id": 1,
"name": "Aydin",
...
},
"words": [
{
"text": "This is a test meeting",
"start_timestamp": {
"relative": 0,
"absolute": "2025-09-10T23:59:05.927163Z"
},
"end_timestamp": {
"relative": 2.3542128,
"absolute": "2025-09-10T23:59:08.281376Z"
}
}
]
},
...
]


```


## Step 2: Creating a meeting summary


The raw meeting transcript is useful, but often people also want a more condensed way of understanding a conversation. A 45-minute meeting transcript might be 8,000 words of people talking over each other, and we want to extract the important decisions from the noise.


This is where the processing with LLMs becomes crucial. I’m using the Vercel AI SDK with OpenAI to transform raw transcripts into structured summaries.


### Getting structured outputs with the AI SDK


The Vercel AI SDK has some really nice features for ensuring you get consistent, structured outputs from language models. Instead of hoping the LLM returns properly formatted JSON, you can define schemas that guarantee the structure you need:


```text
//@title zoom-notetaker/lib/openai.ts
const meetingSummarySchema = z.object({
participants: z.array(participantSchema),
content: z.array(summarySegmentSchema),
});


const summarySegmentSchema = z.discriminatedUnion("type", [
z.object({
type: z.literal("text"),
content: z.string(),
}),
z.object({
type: z.literal("participant"),
content: z.string(),
participantId: z.string(),
}),
z.object({
type: z.literal("timestamp_link"),
content: z.string(),
timestamp: z.number(),
}),
]);


```


This approach is much more reliable than trying to parse free-form LLM outputs. You define the exact structure you want (meeting summary with participants, content segments, timestamps), and the SDK validates against this schema and ensures the model returns data in that format.


```text
//@title zoom-notetaker/lib/openai.ts
const { object } = await generateObject({
model: openai("gpt-4o"),
schema: meetingSummarySchema,
prompt: `Analyze this meeting transcript and create a comprehensive summary...`,
});


```


With this schema in place, the prompt can focus entirely on the content of the summary. It generates detailed outputs where participant names are highlighted with colors and key moments link back to specific timestamps in the video. You can check out the full prompt in the[GitHub repo](https://github.com/recallai/zoom-notetaker) .


### Prompt engineering for meeting summaries


I kept the prompt engineering relatively simple, but there are a few key elements that make a big difference for meeting summarization:


**Speaker context** : The prompt includes the full participant list so the model can properly attribute statements and decisions to specific people. There are several ways you could expand upon my prompt to use even more of the data that you get from Recall.ai. For example, if you wanted to analyze who spoke during a meeting and for how long, you could adjust the prompt to use the participant list along with the transcript with speaker names. That way, you can easily see who contributed, who didn’t, and how much each person spoke. You can even follow up after the call by sending emails directly to participants using the per-participant emails included in the transcript.


**Structural guidance** : The prompt specifically asks the LLM to identify the key decisions made, the action items assigned and the important discussions


**Timestamp integration** : Because I get timestamps per word, I am able to instruct the model to include timestamp links for important moments, making the summary interactive with the video recording.


**Length constraints** : Meeting summaries can easily become as long as the original transcript if you're not careful. I give the model clear guidance about conciseness while preserving important details.


Building against a common interface like the Vercel AI SDK allows you to test different providers (OpenAI, Anthropic, Google, etc.) and models so that you can see which ones work best for your specific use case without refactoring your app.


### Handling very long meetings


For most meetings, a single pass through the LLM works fine. But if you're dealing with particularly long meetings, you might run into token limits or find that the model struggles to synthesize everything effectively.


In these cases, you can implement a multi-pass approach:


1. **Chunk the transcript** into smaller segments (15-20 minutes long)
2. **Summarize each chunk** individually with a prompt focused on extracting key points
3. **Combine the chunk summaries** and run a final summarization pass to create the overall meeting summary


This approach helps ensure that important details from early in the meeting don't get lost when the model is trying to process hours of conversation. It's more complex to implement, but for very long meetings, it can produce much better results than trying to cram everything into a single prompt.


## Step 3: Displaying summaries in a web app


Now that we have a solution for creating a transcript from a Zoom meeting and summarizing it, we just need to save these results and display them in a UI so that users can send bots to calls and view the generated summaries.


This means building a simple web interface where users can paste meeting URLs, a database or file system to store the meeting data and summaries, and some way to display the results with the video, transcript, and AI-generated summary. The[GitHub repository](https://github.com/recallai/zoom-notetaker) includes a complete Next.js implementation that handles all of this, but you could build the same functionality with any web framework and database of your choice. The key pieces are the bot integration and AI summarization we've walked through. The rest is building a standard web app around those core features.


## Other options for building a Zoom notetaker


The approach I've outlined here uses the[Recall.ai Meeting Bot API](https://www.recall.ai/product/meeting-bot-api) to handle the meeting bot infrastructure because it’s the fastest and most reliable way to extract this data from meetings. However, there are other ways to build meeting notetakers if you want to explore all your choices.


If you’re feeling ambitious, I documented[how to build a Zoom bot from scratch](https://www.recall.ai/blog/how-to-build-a-zoom-bot) using browser automation. This approach gives you absolute control over your meeting bot, but adds a ton of additional complexity in the form of browser selectors, handling UI changes, managing Docker containers, and all the edge cases that come with scraping a dynamic web application. It's a fun learning exercise, but definitely not something I'd recommend for production.


For more practical use cases, we’ve written an article about[all of the ways to get Zoom transcripts](https://www.recall.ai/blog/7-apis-to-get-zoom-transcripts-a-comprehensive-guide) which goes through all of the form factors and explores options with varying levels of effort.


Lastly, you could also try Recall.ai’s[Desktop Recording SDK](https://www.recall.ai/product/desktop-recording-sdk) , which allows you to get all the raw data we’ve been discussing without having a bot in the call. To explore both form factors, check out our[Notetaker API](https://www.recall.ai/product/notetaker-api) page.


## Final thoughts


Once I got the initial data ingestion setup working (i.e. the API call to create a bot, the webhook handler, the basic data flow), I essentially didn't have to touch that part of the code again for the rest of the project.


The real time sink was getting the AI summarization right. I spent way more time wrestling with the prompt than any other part of the system, trying to force the LLM output into something actually useful. This is one reason that I'd definitely recommend starting with a library like the Vercel AI SDK for structured outputs rather than trying to parse raw text responses.


In practice, if you're building a meeting notetaker using a service like Recall.ai, the AI analysis layer is where you'll spend most of your development time. That's what makes using an API like[Recall.ai](https://www.recall.ai/product/meeting-bot-api/zoom) valuable: you can skip the ingestion complexity and focus on the differentiator, which is the quality of your analysis.


If you’re interested in trying out my notetaker or customizing it for your own use case, the complete source code is available on[GitHub](https://github.com/recallai/zoom-notetaker) . You can[sign up for a free Recall.ai account](https://us-west-2.recall.ai/auth/signup) to test the notetaker in action during your next meeting.
