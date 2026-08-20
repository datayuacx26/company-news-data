---
schema_version: "1.0.0"
document_id: "9e005f04762073255c3dcbdd0970fbc70ae92eb0e4d27fe0ea157ca6f0166859"
company_key: "yc-recall-ai"
company: "Recall.ai"
source_id: "yc-recall-ai-news-import-5dc65e725852"
canonical_url: "https://www.recall.ai/blog/how-to-build-a-crm-for-wealth-management"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-18T02:04:21.990894+00:00"
fetched_at: "2026-08-18T02:04:24.336869+00:00"
content_hash: "sha256:ca0d55c5b2af0d2bbd7d7149080e65da705d6d06a45db587e5a06ae8003381df"
---

# How to build a CRM for wealth management

To build a CRM that automatically updates for wealth managers, you need an app that can turn meeting conversations into CRM updates, and use them to power downstream workflows. In this tutorial, I will show you how to build a desktop application that handles the entire process.


> If you’re an individual end user looking for a solution for yourself, an off-the-shelf notetaker might be the best solution.


## Features of a wealth management CRM


To build a CRM for wealth managers you must first build the functionality to automatically and accurately update after every client meeting. Without this ability, wealth managers need to manually enter or verify meeting details, creating friction that hurts product adoption.


For wealth managers to benefit from a desktop application that auto-updates a CRM, it needs four core capabilities:


**1. Capture and transcribe the meeting:** Accurately record the conversation and generate a transcript.


**2. Extract key insights, decisions, and next steps:** Identify information from the meeting that needs to be added to the CRM.


**3. Update the client’s record in the wealth manager’s CRM:** Map the information to the correct client.


**4. Power pre and post-meeting workflows:** Use the updated CRM data to trigger workflows such as pre-meeting briefs and follow up emails.


There are many downstream workflows you could build to support wealth managers, but in this tutorial, I will focus on pre and post-meeting workflows.


A post-meeting workflow closes the loop after a meeting with a client by generating an email draft you can send to your client that summarizes what was discussed, the agreed-upon next steps, and reminds them of the next scheduled meeting. A pre-meeting workflow helps wealth managers prepare for that next meeting. Because meetings with the same client may be weeks apart, wealth managers can spend hours getting back up to speed on the client’s context before doing outstanding work.


A pre-meeting brief is most commonly delivered to the wealth manager as an email or calendar event. It surfaces the relevant context without requiring wealth managers to dig through past notes, so they can immediately dive into the outstanding work.


Because meetings between clients can be weeks apart, wealth managers spend a lot of time rebuilding context. My sample app reduces that context-switching overhead, allowing wealth managers to manage more clients without sacrificing quality.


The desktop application I built with Recall.ai brings all of these capabilities together.


## See how a wealth management CRM works in action


> You can clone the[app’s repository](https://github.com/recallai/wealth-management-crm-sample-app) to follow along or fork it to build on top of this sample app. It uses the Desktop Recording SDK and includes the configurations needed to get started and take the SDK into production.


Though you can build the desktop app without the help of agents, I built this desktop application usingRecall.ai’s MCP. If you want to build your own desktop application from scratch, we recommend starting with the MCP. It can help you get a working prototype up and running in less than an hour.


The desktop application captures and transcribes each meeting, extracts key insights, and uses them to update the CRM. The updated CRM data then creates a pre-meeting brief for the next client meeting and a post-meeting email draft summarizing the conversation. Both appear as to-do items in the desktop application’s calendar. The meeting capture and transcription layer are powered by Recall.ai’s Desktop Recording SDK.


If you’ve already built a desktop application, you can integrate Recall.ai into your existing application to get meeting context using one of Recall.ai’s form factors.


## Using Recall.ai to build a CRM for wealth management


Recall.ai offers two methods for capturing meeting data: the Desktop Recording SDK and the Meeting Bot API.


Desktop Recording SDK Meeting Bot API


Recording experience The end-user downloads a desktop application that can record meetings without a bot joining. A bot automatically joins the meeting, so the end user does not need to take any action.


In-person meetings Supported Not supported


Visibility in meeting Not visible Visible using a bot


Both capture methods provide audio recordings, video recordings, and either speaker-labeled[transcripts in real-time](https://www.recall.ai/blog/realtime-transcription-api) or once the meeting ends. These meeting artifacts give you the data to build a wide range of automations with either capture method. Because wealth managers often have in-person meetings, I chose to build the app using the Desktop Recording SDK..


Wealth managers care about minimizing data retention because they work with sensitive client information. Recall.ai provides controls for both the Desktop Recording SDK and Meeting Bot API that allow developers to limit what meeting data is retained. For example, settingvideo_mixed_mp4: null limits meeting capture to the audio needed for transcription. Once transcription is complete, the` transcript.done` webhook notifies the application. The app can thendelete audio recordings and transcripts once the CRM is updated by calling the delete audio recording endpoint and transcript endpoint. Both capture methods also offer BAA options to help financial companies meet data-protection obligations and compliance posture, so the choice between them really comes down to form factor preference.


> When using the Desktop Recording SDK, make sure your application complies with the recording consent laws in both your location and any end user’s location. Obtain any required consent before recording.


### Setting up Recall.ai’s Desktop Recording SDK


1. Install and initialize the SDK


```text
//@title Install the package on your desktop application
npm install @recallai/desktop-sdk


```


```text
//@title Initialize your base URL
import RecallAiSdk from "@recallai/desktop-sdk";


RecallAiSdk.init({
apiUrl: RECALL_API_URL,
});


```


On macOS, there’s one additional step: request the necessary system permissions from your end users.


```text
//@title Request systems, microphone and screen recording permissions from end user
RecallAiSdk.requestPermission("accessibility");
RecallAiSdk.requestPermission("microphone");
RecallAiSdk.requestPermission("screen-capture");


```


1. Create the Desktop SDK upload through a separate backend


To avoid accidentally exposing your API keys, make sure you implement a separate backend to make calls to the Recall.ai API.


```text
//@title server/recall-gateway.mjs - Building a separate backend to make the sdk-upload
const body = await this.#request("/api/v1/sdk_upload/", {
method: "POST",
headers: { "Content-Type": "application/json" },
body: JSON.stringify({
metadata: { recall_wealth_meeting_id: meetingId },
recording_config: buildDesktopSdkRecordingConfig({ captureVideo })
})
});


```


1. Automatically detect meetings


Users are prompted to record without opening the app first. This results in more meetings being captured, which keeps the CRM up to date.


```text
//@title electron/services/recall-desktop.mjs - Automatic meeting detection
this.sdk.addEventListener("meeting-detected", (event) => {
const meeting = safeWindow(event.window);
if (!meeting) return;
this.detectedMeetings.set(meeting.id, meeting);
this.emit({ type: "meeting-detected", meeting, status: this.status() });
});


```


Once the meeting gets uploaded and transcribed, the` recall.upload_completed` webhook fires. At that point, the meeting data is ready to be parsed, processing can run on the data using the LLM of your choice, the CRM is updated, and automations are triggered.


The CRM and automations built on top of meeting data are where product differentiation happens. Fortune 500 companies and startups use Recall.ai for the meeting capture layer so they can focus their engineering resources on those experiences.


## Building vs buying a CRM for wealth management


The stakes for getting the capture layer right are high because the accuracy of CRM updates and other automations depends on it. Reliable capture starts with producing a clear and accurate audio recording of the meeting. That is difficult because system and microphone audio are captured through separate paths and must be synchronized into a single recording. If not synchronized properly, streams will drift or fall out of sync resulting in unusable audio.


The way to capture system and microphone audio also varies across operating systems, browsers, and meeting apps, each with its own APIs, permissions, and restrictions. The complexity of the capture layer increases the risk of poor-quality recordings, which can cause parts of the conversation to be missed or transcribed incorrectly. These failures lead to inaccurate CRM updates and automations.


The capture layer also needs to attribute each part of the conversation to the correct speaker. Without proper speaker attribution, the right insight can get assigned to the wrong person, making the CRM unreliable. Relying on system audio captures all participants into a mixed stream, so the capture pipeline needs to distinguish between speakers and map speech to the correct participant, adding another layer of challenge when building from scratch.


High-quality audio and accurate speaker-labeled transcripts need to remain consistent across devices, operating systems and browsers. For example, one wealth manager joins a Zoom meeting on an older version of macOS using the browser, while another joins through the native Zoom app on macOS. Each environment introduces its own edge cases, and the number of scenarios to support grows with the user base. Operating system APIs and permissions change over time, making meeting capture an ongoing maintenance problem rather than a one-time implementation.


Relying on Recall.ai doesn’t come at the expense of less control over meeting data. As mentioned earlier, developers can choose to record only audio and delete both the audio and transcript once the CRM or any additional automations has been triggered. Granular control over data retention helps meet the data sensitivity requirements of wealth managers and their clients.


Having worked with thousands of customers, Recall.ai has encountered a wide range of scenarios, many of which only surface in production, and built its capture infrastructure to handle them reliably. Production readiness also requires the desktop app to work across real-world user environments that the Recall.ai’s Desktop Recording SDK is built to handle.


### Why companies like Nitrogen and Wealthbox use Recall.ai to grow their business


Outside of all of the issues with getting the capture layer right, wealth managers may switch microphones mid-meeting, experience connectivity issues, or use older devices that aren’t running the latest software.


If a wealth manager’s AirPods run out of battery mid-meeting, active microphone selection keeps the conversation captured as they switch microphones. Automatic meeting and mute detection help recordings start and stop at the right times and avoid capturing speech while the user is muted. This is especially important in wealth management, where sensitive or off-the-record conversations may happen while muted or immediately after a meeting ends and shouldn’t be captured.


Wealth managers can experience unexpected interruptions during a meeting, whether from connectivity issues, device problems, or a laptop running out of battery or power. Recall.ai continuously uploads meeting data while the recording is in progress, so if a wealth manager closes their laptop, loses power, runs out of battery, or their internet connection, meeting data gets preserved.


Teams also choose Recall.ai because of its broad device support. Older Macs can introduce additional audio issues when a wealth manager joins a meeting without headphones. Audio playing through the speakers can create echo, making it harder to maintain a clean system and microphone audio stream. Recall.ai’s Desktop Recording SDK solves for echo cancellation across macOS versions and other edge cases.


All these kinds of issues affect product reliability and adoption. Addressing them in-house takes months of engineering work, delays time to market, and pulls resources away from the features that differentiate your product.


The work also doesn’t stop after production. Users upgrade devices, operating systems change, and platform APIs evolve. A capture layer that works today can break later if those changes aren’t continuously supported. That ongoing maintenance becomes a significant strain on engineering bandwidth. Companies like Notta have moved to Recall.ai to avoid maintaining the capture layer themselves.


Using Recall.ai helps teams get to production faster and keeps engineering resources focused on the product experiences that create a competitive advantage.


If you are interested in building on meeting data with Recall.ai, you can[sign up for a free account](https://www.recall.ai/signup) or[book a demo](https://www.recall.ai/book-a-demo) .


## Appendix


### Building with the MCP


To get started with[Recall.ai’s MCP](https://docs.recall.ai/docs/docs-mcp) , first you need to connect it with your coding agent. The MCP gives the agent access to Recall.ai’s documentation and implementation context while it builds the integration.


Once connected, give your coding agent a prompt in this format:


```text
Read the Recall.ai MCP onboarding guide, then help me build an app as follows.
I want to create a desktop meeting recorder for [ICP].
The ideal flow is:
- Listen to and record the meeting
- [Do X]
- [Do Y]
- [Do Z]
Use Recall.ai’s Desktop Recording SDK for meeting capture.
Set up a separate backend for all Recall.ai API calls so the Recall.ai API key is never exposed in the desktop client. The desktop application should request an upload token from the backend before starting a recording.
On macOS, request the required recording permissions from the end user as part of onboarding. Request accessibility and microphone, plus system-audio for audio-only recording or screen-capture when capturing video.


```


After you provide the prompt, the MCP will generate a sample app with the appropriate Recall.ai configuration. The same approach also works for Meeting Bots.


### Getting only audio recording


For[audio-only capture](https://docs.recall.ai/docs/audio-only) on the Desktop Recording SDK, set the appropriate fields:


```text
//@title Sample code snippet
const recordingConfig = {
audio_mixed_mp3: {},
video_mixed_mp4: null,
};


```


## Deleting transcription once CRM is updated


For deleting meeting data once the CRM is updated, listen to the[transcript.done](https://docs.recall.ai/docs/async-transcription) webhook:


```text
//@title Sample of webhook implementation
app.post("/webhooks/recall", async (req, res) => {
const event = req.body;


if (event.event === "transcript.done") {
const recordingId = event.data.recording.id;


const transcript = await getTranscript(recordingId);


// Update CRM and trigger automations
await updateCRM(transcript);


// Delete meeting data after processing succeeds
await deleteRecording(recordingId);
}


res.sendStatus(200);
});


```


## Frequently asked questions about a financial CRM


#### What is a CRM for wealth management?


A wealth management CRM stores and organizes client information. The CRM stays up to date and power workflows such as meeting preparation and client follow-ups.


#### Why do you need a CRM for wealth management?


Wealth managers often manage many clients with different needs and may go months between meetings. An up-to-date CRM gives them a reliable record of each relationship so they can quickly recall previous conversations.


#### What are existing CRM for wealth management options in the market?


Existing CRM options for wealth management include Jump, Wealthbox, and Nitrogen. If you’re building a product for wealth managers rather than using an off-the-shelf solution, Recall.ai can provide the meeting capture infrastructure while you build the CRM and workflows that differentiate the product.


#### How do you build a CRM for wealth management from scratch?


At a high level, you need to capture and transcribe client meetings, extract relevant information from the transcript, update the appropriate client record, and use that information to trigger downstream workflows. Recall.ai provides the meeting capture layer through its[Desktop Recording SDK](https://www.recall.ai/product/desktop-recording-sdk) and[Meeting Bot API](https://www.recall.ai/product/meeting-bot-api) .


#### Should you buy or build a CRM for wealth management?


[Financial advisors](https://www.recall.ai/solutions/financial-advisors) looking for a solution just for themselves may be better served by an off-the-shelf notetaker. Developers building a[Granola AI alternative](https://www.recall.ai/blog/granola-ai-alternatives) with multiple functionalities may want to use Recall.ai for meeting capture, rather than building and maintaining the capture layer themselves. Without Recall.ai, developers would otherwise need to get[access to system audio](https://www.recall.ai/blog/how-to-get-access-to-system-audio) and stitching together multiple[local recording APIs](https://www.recall.ai/product/local-recording-api) and[macOS APIs](https://www.recall.ai/blog/macos-screencapture-api) , including[AVFoundation](https://www.recall.ai/blog/av-foundation) ,[ScreenCaptureKit](https://www.recall.ai/blog/how-to-use-screencapturekit-to-record-a-meeting) , and[Core Audio Taps](https://www.recall.ai/blog/core-audio-taps) .


#### What is a desktop meeting recorder?


A desktop meeting recorder is a[botless recorder](https://www.recall.ai/blog/how-i-built-a-botless-meeting-recorder-from-scratch) that captures meetings directly from the end user’s computer instead of sending a bot into the call. To[build a desktop recorder](https://www.recall.ai/blog/how-to-build-a-desktop-recording-app) , Recall.ai’s Desktop Recording SDK supports Windows and macOS and can capture audio, video, meeting metadata, and[multilingual transcription](https://www.recall.ai/blog/code-switching-and-language-identification) . It can also capture in-person meetings.


#### What is a meeting bot?


A meeting bot is a visible participant that joins a virtual meeting to capture meeting data. Recall.ai’s Meeting Bot API can capture audio, video, transcripts, and metadata from platforms such as Zoom, Google Meet and[Microsoft Teams](https://www.recall.ai/blog/how-to-get-a-transcript-from-microsoft-teams) .


#### Should I use a desktop meeting recorder or a meeting bot?


The choice comes down to the recording form factor preferred. The Desktop Recording SDK doesn’t require a bot to join the meeting and captures in-person meetings. The Meeting Bot API records meetings without requiring users to install a desktop application.
