---
schema_version: "1.0.0"
document_id: "8773d21776031a4f129650af9fb81848eb4f0a128adc4131eb0d4b169cf169ca"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c3d7277c32d9"
canonical_url: "https://www.assemblyai.com/blog/summarize-meetings-llms-python"
published_at: null
first_seen_at: "2026-07-21T08:04:02.365197+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:4a392d178b6fb376b2db43fc25da462a9096d5808e979b1b45f424becaf7f8e0"
---

# How to summarize meetings with LLMs

In today's remote-first world, organizations conduct millions of virtual meetings daily, but crucial information often slips through the cracks. Important decisions get **forgotten** , action items go **untracked** , and valuable insights remain **buried** in recordings that nobody has time to review. These problems create a massive efficiency gap—[76% of organizations](https://www.assemblyai.com/2025-research-report) now embed conversation intelligence into the majority of their customer interactions, yet most still struggle to extract meaningful intelligence from internal meetings.


In this tutorial, you'll learn how to use AssemblyAI's[LLM Gateway](https://www.assemblyai.com/docs/llm-gateway/overview?ref=assemblyai.com) to automatically capture and analyze your meetings, allowing you to turn hours of conversations into structured summaries, clear action items, and actionable insights - all powered by large language models.


## **What you need to get started**


To summarize meetings with AI, you need two things: a speech-to-text API to transcribe the audio, and an LLM to analyze the transcript. AssemblyAI's[LLM Gateway](https://www.assemblyai.com/docs/llm-gateway/overview?ref=assemblyai.com) combines both in a single workflow.


Get your AssemblyAI API key[here](https://www.assemblyai.com/signup?ref=assemblyai.com) . The LLM Gateway may incur additional costs, so ensure your account has billing enabled.


Install the Python SDK:


```text
pip install -U assemblyai
```


## **Transcribe meeting audio with speech-to-text**


To generate a meeting summary, you first need to get a transcript of the meeting audio. Create a file named main.py and add the following code. This script will import the necessary libraries, configure your API key, and transcribe the meeting audio.


While we set the API key inline here for simplicity, you should store it securely as an environment variable in production code and never check it into source control.


```text
import   assemblyai   as   aai
import   requests
import   sys


# Set your API key
aai.settings.api_key =   "YOUR_API_KEY"


# URL of the meeting audio to be transcribed
MEETING_URL =   "https://storage.googleapis.com/aai-web-samples/meeting.mp3"


# Configure the transcriber
transcriber = aai.Transcriber()


# Transcribe the audio file
transcript = transcriber.transcribe(MEETING_URL)


# Check for transcription errors
if   transcript.status == aai.TranscriptStatus.error:
print  (  f"Transcription failed:   {transcript.error}  "  , file=sys.stderr)
sys.exit(  1  )
```


## **Generate a meeting summary with the LLM Gateway**


With the transcript ready, you can analyze it using AssemblyAI's **LLM Gateway** . Create a structured prompt that defines exactly what information to extract:


### **Write an effective summarization prompt**


```text
prompt =   ""  "
Analyze this meeting transcript and provide a structured summary with the following:


1. Meeting Overview
- Meeting date and duration
- List of participants (if mentioned)
- Main objectives discussed


1. Key Decisions
- Document all final decisions made
- Include any deadlines or timelines established
- Note any budgets or resources allocated


1. Action Items
- List each action item with:
* Assigned owner
* Due date (if specified)
* Dependencies or prerequisites
* Current status (if mentioned)


1. Discussion Topics
- Summarize main points for each topic
- Highlight any challenges or risks identified
- Note any unresolved questions requiring follow-up


1. Next Steps
- Upcoming milestones
- Scheduled follow-up meetings
- Required preparations for next discussion


ROLE: You are a professional meeting analyst focused on extracting actionable insights.


FORMAT: Present the information in clear sections with bullet points for easy scanning.
Keep descriptions concise but include specific details like names, dates, and numbers when mentioned.


If any of these elements are not discussed in the meeting, note their absence rather than making assumptions.
"  ""  .strip()
```


### **Send the transcript to the LLM Gateway**


Now, send the transcript text and your prompt to the LLM Gateway. This requires a separate API call. Add the following code to the end of your main.py file to send the request and print the summary:


```text
# Prepare the request   for   the LLM Gateway
llm_gateway_payload = {
"model"  :   "claude-sonnet-4-6"  ,  # A powerful and current model
"messages"  : [
{
"role"  :   "user"  ,
"content"  : f  "{prompt}\n\nTranscript:\n{transcript.text}"
}
],
"max_tokens"  :   2048  ,
"temperature"  :   0.0
}


try  :
# Send the request to the LLM Gateway
response = requests.post(
"https://llm-gateway.assemblyai.com/v1/chat/completions"  ,
headers={  "authorization"  : aai.settings.api_key},
json=llm_gateway_payload
)
response.raise_for_status()  # Raise an exception   for   bad status codes


result = response.json()
print(result[  'choices'  ][  0  ][  'message'  ][  'content'  ])


except requests.exceptions.RequestException   as   e:
print(f  "Error calling LLM Gateway: {e}"  , file=sys.stderr)
sys.exit(  1  )
```


**Tip:** For lower-cost processing, you can swap claude-sonnet-4-6 with claude-haiku-4-5-20251001, which offers near-instant responses at a lower price point. See the model table below for all available options.


Execute this script in your terminal by running python main.py. This will transcribe the meeting audio, analyze the transcript, and generate a structured meeting summary based on your prompt. The output will be printed to your console. For example, here is an example output for the example file used above:


```text
Here  's a structured summary of the meeting transcript:


1. Meeting Overview
- Date: February 18, 2021
- Participants mentioned: Eric Johnson, Sid, Lily, Mac, Christopher, Steve, Craig, Christy, Rob
- Main objectives: Engineering key review, discussing KPIs, metrics, and organizational changes


2. Key Decisions
- Break up the engineering key review into four department key reviews
- Implement a two-month rotation for department reviews
- Change the R&D wider MR Rate KPI to track percentage of total MRs that come from the community
- Measure S1/S2 SLO achievement based on open bugs rather than closed bugs


3. Action Items
- Lily: Work with Mac to transition to new community contribution KPI
- Mac: Provide an update on the Postgres replication issue in next week'  s infra key review
- Mac/Data team: Develop   new   measurement   for   average open bugs age
- Mac/Data team: Adjust metrics to measure percentage   of   open bugs within SLO
- Christopher: Continue monitoring narrow MR Rate and expect rebound   in   March


4.   Discussion Topics


a) Department Key Reviews
- Proposal to split engineering review into development, quality, security, and UX
- Two-month rotation proposed to avoid adding too many meetings


b) R&D MR Rate Metrics
- Confusion about current R&D wider MR Rate calculation
- Decision to simplify and track percentage   of   MRs   from   community


c) Postgres Replication Issue
- Lag   in   data updates affecting February metrics
- Need   for   dedicated computational resources and potential database tuning


d) Defect Tracking and SLOs
- S1 defects at   80  % SLO achievement, S2 at   60  %
- Spike   in   mean time to close   for   S2 bugs noted


e) SUS (Satisfaction) Metric
- Smallest decline   in   Q4 compared to previous quarters
- Cautious optimism about trend, but continued monitoring needed


f) Narrow MR Rate
- Currently below target but higher than previous year
- Expectation to rebound   in   March after short February and power outages   in   Texas


5.   Next Steps
- Implement   new   department key review structure
- Monitor effects   of   changes to KPI measurements
- Continue focus on improving security work prioritization
- Expect potential temporary jump   in   SLO achievement   as   backlog is cleared


Note  : Specific due dates   for   action items were not mentioned   in   the transcript.
```


### **Choose the right LLM model**


The LLM Gateway supports models from leading providers. Check our[docs](https://www.assemblyai.com/docs/llm-gateway/overview?ref=assemblyai.com) for the most up-to-date list.


Model Provider Best for


claude-sonnet-4-6 Anthropic Complex analysis, balanced performance


gpt-5 OpenAI Coding and agentic tasks


gemini-2.5-pro Google Deep reasoning over complex problems


claude-haiku-4-5-20251001 Anthropic Near-instant responsiveness, lower cost


gpt-oss-120b OpenAI Open-weight, self-hosting option


Access LLM Gateway models


Start calling leading LLMs through a single API with your AssemblyAI key. Pick the model that fits your latency and cost needs for meeting summaries.


[Get API key](https://www.assemblyai.com/dashboard/signup)


## **Customize your meeting summaries**


You can tailor the LLM's output by changing the prompt. Tell it what to extract, how to format it, and what to prioritize. Here are some examples:


### **Focus on action items**


```text
action_items_prompt =   ""  "
Review the transcript and extract all action items:
- Who is responsible
- What needs to be done
- When it's due
- Current status


Format as a clear, bulleted list.
"  ""
```


### **Generate a technical discussion summary**


```text
technical_prompt =   ""  "
Analyze this technical discussion and provide:
- Technical decisions made
- Architecture changes approved
- Dependencies identified
- Technical debt noted
- System constraints discussed


Include specific technical details mentioned.
"  ""
```


### **Create a project status report**


```text
status_prompt =   ""  "
Generate a project status report including:
- Overall project health
- Milestones completed
- Upcoming deadlines
- Blocking issues
- Resource needs
- Risk assessment
"  ""
```


## **Add speaker-aware summaries with diarization**


A basic transcript treats an entire meeting as one wall of text. That's fine for a rough summary, but it loses the most valuable context—who said what.[Speaker diarization](https://www.assemblyai.com/docs/speech-to-text/speaker-diarization) labels each segment of speech by speaker, so your LLM can generate per-person summaries, extract individual action items, and identify who owns each decision.


### **Enable speaker diarization**


Turn on diarization by setting speaker_labels=True in your transcription config. The API will automatically detect and label each speaker in the audio.


```text
config = aai.TranscriptionConfig(speech_models=[  "universal-3-pro"  ], speaker_labels=True)
transcript = transcriber.transcribe(MEETING_URL, config=config)
```


The response now includes an utterances array, where each utterance contains a speaker label, the spoken text, and start/end timestamps.


### **Format diarized transcripts for LLM analysis**


Raw utterance objects aren't ideal for an LLM prompt. You'll get better results by formatting them into a clean, speaker-labeled transcript string first.


```text
# Build a speaker-labeled transcript string
speaker_transcript =   ""
for   utterance   in   transcript.utterances:
speaker_transcript += f  "Speaker {utterance.speaker}: {utterance.text}\n"
```


Now pass this formatted transcript to the LLM Gateway with a prompt designed to extract per-speaker contributions.


```text
diarized_payload = {
"model"  :   "claude-sonnet-4-6"  ,
"messages"  : [
{
"role"  :   "user"  ,
"content"  : (
"You are an expert meeting analyst. Given the following "
"speaker-labeled meeting transcript, provide:\n"
"1. A brief overall meeting summary (2-3 sentences)\n"
"2. Per-speaker summaries of each person's key contributions\n"
"3. Action items with the responsible speaker assigned\n\n"
f  "Transcript:\n{speaker_transcript}"
)
}
]
}


response = requests.post(
"https://llm-gateway.assemblyai.com/v1/chat/completions"  ,
headers={  "authorization"  : aai.settings.api_key},
json=diarized_payload
)


summary = response.json()[  "choices"  ][  0  ][  "message"  ][  "content"  ]
print(summary)
```


The LLM now has the context it needs to attribute decisions, questions, and commitments to specific speakers—something that's impossible with a plain transcript. You can adjust the prompt to focus on whatever matters most: decisions made, disagreements raised, or follow-up tasks per person.


Try speaker diarization in the playground


Upload a meeting recording and see speaker-labeled transcripts in seconds. No code required.


[Open playground](https://www.assemblyai.com/playground)


## **Handle errors in production**


This workflow involves two API calls—one to the Speech-to-Text API and one to the LLM Gateway—and both can fail independently. Network issues, invalid file formats, or authentication problems can occur at either stage.


The AssemblyAI Python SDK raises an AssemblyAIError for transcription problems. The LLM Gateway call (made via requests) can raise an HTTPError.


### **Transcription errors**


The SDK's transcribe() method can fail silently by returning a transcript with an error status. Always check transcript.status before proceeding to the LLM step.


### **LLM Gateway errors**


The LLM Gateway uses standard HTTP status codes. Call response.raise_for_status() to catch 4xx and 5xx errors, then handle them in a try...except block.


```text
import   assemblyai   as   aai
import   requests
import   sys


aai.settings.api_key =   "YOUR_API_KEY"
prompt =   "Your summarization prompt..."


try  :
# Step   1  : Transcribe the audio
transcript = aai.Transcriber().transcribe(  "https://path/to/your/file.mp3"  )


if   transcript.status == aai.TranscriptStatus.error:
print(f  "Transcription failed: {transcript.error}"  , file=sys.stderr)
sys.exit(  1  )


# Step   2  : Analyze   with   LLM Gateway
llm_gateway_payload = {
"model"  :   "claude-sonnet-4-6"  ,
"messages"  : [{  "role"  :   "user"  ,   "content"  : f  "{prompt}\n\nTranscript:\n{transcript.text}"  }]
}


llm_response = requests.post(
"https://llm-gateway.assemblyai.com/v1/chat/completions"  ,
headers={  "authorization"  : aai.settings.api_key},
json=llm_gateway_payload
)


# Check   for   LLM Gateway errors
llm_response.raise_for_status()


# ... proceed   with   processing the response
print(llm_response.json()[  'choices'  ][  0  ][  'message'  ][  'content'  ])


except aai.errors.AssemblyAIError   as   e:
print(f  "A transcription API error occurred: {e}"  , file=sys.stderr)
sys.exit(  1  )
except requests.exceptions.RequestException   as   e:
print(f  "An LLM Gateway API error occurred: {e}"  , file=sys.stderr)
sys.exit(  1  )
```


This pattern catches errors at three levels: network failures via the try...except block, transcription errors via transcript.status, and LLM Gateway errors via raise_for_status().


## **Build a voice agent for meeting intelligence**


Everything you've built so far processes recordings after the fact. But what if your meeting assistant could participate in real time—answering questions, tracking action items, and surfacing insights as the conversation happens? That's what the[Voice Agent API](https://www.assemblyai.com/docs/voice-agents/voice-agent-api) enables.


The Voice Agent API is a single WebSocket connection that handles the full pipeline: speech understanding, LLM reasoning, and voice generation. It's built on Universal-3 Pro, so you get the same transcription accuracy you've been using throughout this tutorial—but now in a real-time, interactive loop. One API, one connection, $4.50/hr flat rate.


Here's a minimal WebSocket setup that configures a meeting intelligence agent:


```text
import   asyncio
import   json
import   websockets


VOICE_AGENT_URL =   "wss://agents.assemblyai.com/v1/ws"
YOUR_API_KEY =   "YOUR_API_KEY"


async     def     meeting_agent  ():
async     with   websockets.connect(
VOICE_AGENT_URL,
extra_headers={  "Authorization"  :   f"Bearer   {YOUR_API_KEY}  "  }
)   as   ws:
# Configure the agent for meeting intelligence
await   ws.send(json.dumps({
"type"  :   "session.update"  ,
"session"  : {
"system_prompt"  :   "You are a meeting assistant. Summarize discussions and track action items in real time."  ,
"output"  : {  "voice"  :   "Mark"  }
}
}))


asyncio.run(meeting_agent())
```


From here, you'd stream microphone audio into the WebSocket and receive spoken responses back. The agent hears the conversation, understands context through the LLM, and responds with synthesized speech—all through that single connection. You can register tools via JSON Schema to let the agent pull in data from past meetings, query your knowledge base, or push action items to your project management system.


Check out the[Voice Agent API docs](https://www.assemblyai.com/docs/voice-agents/voice-agent-api) to go deeper on tool calling, turn detection configuration, and building production-ready meeting agents.


Build your first voice agent


Get started with real-time meeting intelligence using the Voice Agent API. One WebSocket connection handles transcription, reasoning, and speech.


[Get API key](https://www.assemblyai.com/dashboard/signup)


## **Best practices for meeting summarization**


### **Prompt engineering**


Prompt structure directly impacts analysis quality. Treat prompts as precise API specifications for the LLM.[A key advantage](https://www.assemblyai.com/blog/build-speech-ai-apps-with-lemur-llms) of this approach is the ability to define exactly what you need in natural language, allowing the LLM to handle the complex implementation.


Essential prompt elements:


- Clear formatting requirements (bullet points, sections, tables)
- Specific categories of information to extract
- Instructions for handling uncertainty or missing information
- Guidelines for maintaining consistent terminology
- Requirements for level of detail and technical depth


You can check out our crash course on prompt engineering if you're unfamiliar:


### **Audio quality**


Transcription accuracy directly affects LLM analysis quality. As[industry research confirms](https://www.assemblyai.com/2025-research-report) , "if the words are wrong, the outcomes are too"—poor audio creates cascading errors through the entire pipeline.


Audio optimization checklist:


- Use high-quality microphones or headsets rated for voice clarity
- Ensure all participants are in quiet environments with minimal echo
- Test audio settings before important meetings
- Record locally when possible to avoid internet connectivity issues
- Request that participants mute when not speaking


### **Meeting structure**


Structured meetings create recognizable patterns for LLM processing, and consistent protocols improve extraction accuracy.[Industry surveys show](https://www.assemblyai.com/2025-research-report) more than 70% of teams using conversation intelligence report measurable increases in end-user satisfaction.


Meeting structure requirements:


- Begin with a clear agenda shared in advance
- Start with brief participant introductions
- Use consistent terminology for decisions and action items
- Designate specific times for questions and discussion
- End with a verbal summary of key points


Remember to use our[Speaker Diarization](https://www.assemblyai.com/docs/speech-to-text/speaker-diarization?ref=assemblyai.com) service in your transcripts to automatically separate out speakers and attribute sentences accordingly, too.


Remember that these practices work together - good audio quality with poor meeting structure, or well-structured meetings with unclear prompts, will still result in suboptimal outcomes. You will find the most success in implementing these best practices as a cohesive system.


## **Frequently asked questions about meeting summarization with LLMs**


### **How do I debug transcription accuracy issues with poor audio quality?**


Check source audio for noise, echo, or overlapping speakers first. Enable Speaker Diarization for multi-speaker recordings to improve LLM context accuracy.


### **What are the performance differences between LLM models for meeting summarization?**


Models trade off reasoning quality, latency, and cost. Use claude-sonnet-4-6 for balanced performance, gemini-2.5-pro for deep reasoning, or claude-haiku-4-5-20251001 for near-instant responses at lower cost.


### **How do I handle very long meetings that exceed LLM token limits?**


Use map-and-reduce: chunk long transcripts into 15-minute segments, summarize each chunk individually, then combine summaries with a final prompt.


### **What's the best approach for real-time vs batch meeting processing?**


Use batch processing for post-meeting summaries or streaming transcription API for real-time analysis. Stream transcript chunks to the **LLM Gateway** for live insights during meetings.


### **How do I optimize costs when processing meetings at scale?**


Implement caching to avoid duplicate processing, optimize prompt length to reduce token costs, and batch requests where latency permits.


‍


‍
