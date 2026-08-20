---
schema_version: "1.0.0"
document_id: "09988390de04e3cd96ba3ad59a986abd17aca989c8cbb67e68439ca303b78c7e"
company_key: "yc-recall-ai"
company: "Recall.ai"
source_id: "yc-recall-ai-news-import-5dc65e725852"
canonical_url: "https://www.recall.ai/blog/how-to-get-a-transcript-from-microsoft-teams"
published_at: "2026-03-26T00:00:00+00:00"
first_seen_at: "2026-07-22T11:03:36.206412+00:00"
fetched_at: "2026-07-28T22:17:35.086279+00:00"
content_hash: "sha256:276cee2b118c6a09a0996d24968acca33e42015c9794425badb79a0af5bac4f6"
---

# How to get transcripts from Microsoft Teams programmatically

In this post, I’ll walk you through how to programmatically retrieve transcripts from Microsoft Teams using Recall.ai’s[Meeting Bot API](https://www.recall.ai/product/meeting-bot-api) .


I’ll also cover the[different transcription options](https://docs.recall.ai/docs/recallai-transcription) available in Recall.ai, so you can understand which option best fits your use case.


> You can find the full code for this tutorial in the[Microsoft Teams Transcription GitHub repository.](https://github.com/recallai/teams-transcript)


Watch the demo to see these capabilities in action.


[Demo Video](https://youtu.be/J7Gwc2nVhD8)


### How to get transcripts from Microsoft Teams: Different methods


There are several ways to retrieve transcripts from Microsoft Teams meetings. Some methods only support async transcription, while others support both real-time and async transcription.


> Async transcription is generated and accessible once the meeting ends, while real-time transcription is generated during the meeting.


Product Description


Recall.ai’s Desktop Recording SDK Enables developers to build desktop applications that detect and record meetings happening on a user’s computer.


Microsoft Teams’ Native Cloud Recording API Lets you retrieve and work with Teams meeting recordings.


Recall.ai’s Meeting Bot API Enables developers to create bots that can automatically join and interact with meetings across platforms like[Zoom,](https://www.recall.ai/product/zoom-recording-api)[Google Meet,](https://www.recall.ai/product/google-meet-recording-api) and[Microsoft Teams.](https://www.recall.ai/product/microsoft-teams-recording-api)


**Recall.ai’s Desktop Recording SDK**


- Supports both[real-time](https://docs.recall.ai/docs/bot-real-time-transcription) and[async transcription](https://docs.recall.ai/docs/async-transcription)
- Records without a bot in the meeting
- Provides speaker labels
- Works across all meeting platforms, including breakout rooms
- Supports in-person meeting recording
- Requires users to install a desktop application


**Microsoft Teams’ Native Cloud Recording API**


- Supports async transcription, no real-time transcription
- Recording must be initiated by a participant who is allowed to record (eg: an organizer or presenter) with a Microsoft 365 license and appropriate meeting policies enabled
- Transcript access limited to authorized meeting participants
- Only works on Microsoft Teams


**Recall.ai’s Meeting Bot API**


- Supports real-time and async transcription
- Provides speaker labels and 100% perfect[speaker diarization](https://docs.recall.ai/docs/perfect-diarization)
- Works across all meeting platforms, including breakout rooms
- Meeting bot appears in call


> Perfect speaker diarization means each spoken segment is assigned to the correct participant 100% of the time by tapping into separate audio streams per speaker. Each participant’s speech is captured and labeled with their actual name (not “Speaker 1”, “Speaker 2”).


If you’re looking for a solution that is easy to set up and needs capabilities like real-time transcription, accurate speaker attribution, and reliable cross-platform support, the **Meeting Bot API and Desktop Recording SDK** are strong choices to get transcripts from Microsoft Teams. In this article, I’ll focus on the **Meeting Bot API.**


Meeting Bot API (what I’ll be using in this article) Microsoft Teams’ Native Cloud Recording API Desktop Recording SDK


Async transcription Yes Yes Yes


Real-time transcription Yes No Yes


100% perfect diarization Yes No No


Speaker labels Yes Yes Yes


No host or participant permissions required Yes No Yes


Works across breakout rooms Yes No Yes


Meeting bot shows up in meeting Yes No No


Requires user to install desktop app No No Yes


### Prerequisites for using a bot to get Teams transcripts


For this tutorial, I am using the US West 2 region. Update the base URL to match your account’s region.


Region Base URL


US West 2 us-west-2.recall.ai


US East 1 us-east-1.recall.ai


EU eu-central-1.recall.ai


Asia ap-northeast-1.recall.ai


Before starting, make sure you have the following:


- [A Recall.ai API key](https://us-west-2.recall.ai/) (sign up for free)
- A backend server that can receive webhooks (I used[ngrok](https://ngrok.com/) for this tutorial)
- The[GitHub repository](https://github.com/recallai/teams-transcript) contains the full code for this tutorial, so you can see the results in action.


Update .env files with the keys you need:


```text
RECALL_API_KEY=YOUR_API_KEY
RECALL_REGION=YOUR_REGION
PUBLIC_BASE_URL=YOUR_NGROK_LINK


```


You need a total of three terminals running:


1. In the first terminal, start your ngrok tunnel:


```text
ngrok http 3000


```


1. In the second terminal, install the necessary requirements:


```text
npm install


```


1. Then run:


```text
node backend.js


```


If everything is set up correctly, you should see the message: **Server is running on[http://localhost:3000](http://localhost:3000/)**


### Step 1: Sending a bot to a Teams meeting


If you see the success message, you’re ready to send a bot to your Microsoft Teams meeting to record and transcribe the meeting.


Start a Microsoft Teams meeting, copy the meeting link, and paste it into the cURL command you’ll run to send the bot into the meeting.


```text
curl -X POST http://localhost:3000/bots/realtime \
-H "Content-Type: application/json" \
-d '{
"meetingUrl": "TEAMS_MEETING_URL_HERE",
"botName": "My Bot"
}'


```


Wait a few seconds, and you should see a bot requesting to enter the meeting.


### Step 2: Retrieving Teams transcript using async transcription


To set up async transcription, you’ll first need to configure your webhook endpoints. Then subscribe to the` recording.done` and` transcript.done` events.


> Recall.ai uses webhooks to notify your server when certain events happen. You can configure which events to subscribe to in the Recall.ai dashboard.


Async transcription begins when the meeting ends. Recall.ai signals this by sending a` recording.done` webhook event, which indicates that the recording has been successfully uploaded and all meeting data is available for retrieval.


This event includes the` recordingId` , which your server uses to call the` create_transcript` endpoint to start generating the transcript.


```text
const transcriptJob = await recallRequest(
`/recording/${recordingId}/create_transcript/`,
{
method: "POST",
body: JSON.stringify({
provider: {
recallai_async: {
language_code: "en"
}
}
})
}
);


```


When transcription is complete, Recall.ai sends a` transcript.done` webhook event. This event includes the` transcriptId` , which your server uses to call the` transcript` endpoint and retrieve the finished transcript.


```text
const transcript = await recallRequest(`/transcript/${transcriptId}/`, {
method: "GET"
});


```


The endpoint returns a transcript URL that you can use to retrieve the transcript.


### Step 3: Retrieving Teams transcript using real-time transcription


> Async transcription delivers results via **status-change webhooks** because it’s a post-processing job (you’re notified when the transcript is ready), whereas real-time transcription uses **real-time webhook endpoints** to stream transcript data live during the call.


To set up real-time transcription, update the cURL command you use to create the bot by configuring a webhook endpoint for real-time transcript events. Include the` transcript.data` and` transcript.partial_data` events, set the language to English, and use` prioritize_low_latency` mode.


```text
curl --request POST \
--url "https://RECALL_REGION.recall.ai/api/v1/bot/" \
--header "Authorization: Token RECALL_API_KEY" \
--header "accept: application/json" \
--header "content-type: application/json" \
--data '
{
"meeting_url": "YOUR_TEAMS_MEETING_LINK",
"recording_config": {
"transcript": {
"provider": {
"recallai_streaming": {
"mode": "prioritize_low_latency",
"language_code": "en"
}
},
"diarization": {
"use_separate_streams_when_available": true
}
},
"realtime_endpoints": [
{
"type": "webhook",
"url": "YOUR_NGROK_LINK/webhook/recall/transcript",
"events": ["transcript.data", "transcript.partial_data"]
}
]
}
}'


```


This setup allows you to see the meeting being transcribed in real time in your backend.


> By default, Recall.ai provides[built-in transcription](https://docs.recall.ai/docs/recallai-transcription) that supports multiple languages and word-level timestamps. This allows you to get started without integrating a separate transcription provider.


### Why use this method for getting Microsoft Teams transcripts


In this tutorial, I went over how to get real-time and async transcripts from a Microsoft Teams meeting using Recall.ai’s[Meeting Bot API.](https://www.recall.ai/product/meeting-bot-api/microsoft-teams) We also explored both real-time and async transcription options.


To summarize, here are the reasons for using the Meeting Bot API to receive transcripts from a Microsoft Teams meeting:


- Users are not required to install an app
- Works in breakout rooms and across all meeting types
- Does not require the user to be the meeting host or have special permissions to generate transcripts
- Supports both real-time and async transcription, with speaker labels and 100% perfect diarization


You can sign up for a free[Recall.ai account](https://us-west-2.recall.ai/) to try sending a meeting bot and retrieving transcripts in your own meetings. If you’re looking to use Recall.ai at scale, you can[book a demo](https://www.recall.ai/book-a-demo) with the team.
