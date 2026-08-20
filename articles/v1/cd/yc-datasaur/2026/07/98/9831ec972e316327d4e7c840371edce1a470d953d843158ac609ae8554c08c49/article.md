---
schema_version: "1.0.0"
document_id: "9831ec972e316327d4e7c840371edce1a470d953d843158ac609ae8554c08c49"
company_key: "yc-datasaur"
company: "Datasaur"
source_id: "yc-datasaur-news-import-9dd599cb2858"
canonical_url: "https://datasaur.ai/blog/i-built-my-own-morning-podcast-with-three-free-tools"
published_at: null
first_seen_at: "2026-07-25T01:27:44.103227+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:cf5aa44d215285412949b19ebb214c9a9a26321aeffb60a10e0a6867e81feeba"
---

# I Built My Own Morning Podcast with Three Free Tools

There is a version of the future where your AI assistant reads you your morning briefing while you make coffee, catching you up on overnight messages, flagging what needs your attention, and priming you for the day ahead. That version of the future exists today. I built it over a weekend using three free, off-the-shelf tools.


No custom models. No expensive subscriptions. No engineering team. Just a bit of glue code and a clear idea of what I actually wanted.


## The problem: mornings are expensive attention


The first 30 minutes of a workday are some of the most cognitively valuable. Yet most of us spend them triaging: scrolling through Slack, scanning email, trying to reconstruct what happened overnight, and figuring out what matters most right now.


Reading takes focus. And focus, first thing in the morning, is a finite resource.


What I wanted was simple: something that could tell me what I needed to know, hands-free, while I was still in the kitchen. A morning podcast, but one that was actually about my day.


## The stack: three free components


The system I built has three layers, each handled by a different free tool.


- **Content:** OpenCode daily briefing skill connected to Slack and Gmail
- **Voice:** Microsoft Edge TTS
- **Delivery:** Spotify’s save-to-Spotify tool


## 1) Content: OpenCode daily briefing skill


The brain of the system is an OpenCode skill I configured to pull from my Slack channels and Gmail inbox. It surfaces time-sensitive overnight messages (things that need a response before the day gets going) alongside my top priorities for the day. The output is a clean, structured briefing: what happened, what needs attention, and what is on the agenda.


OpenCode’s integration with Slack and Gmail means the briefing is always current and always personal. It is not a generic news digest. It is my context, assembled automatically.


## 2) Voice: Microsoft Edge TTS


Once the briefing is generated, it needs to be read aloud. Microsoft’s Edge Text-to-Speech service handles this. It is free, high quality, and it produces natural-sounding audio from plain text. The voices are good enough that you stop noticing they are synthetic after a few listens.


Edge TTS takes the briefing text and converts it into an audio file, ready to be played back or saved.


## 3) Delivery: Spotify save-to-Spotify tool


The final piece is delivery. I use Spotify’s save-to-Spotify tool to push the generated audio into my Spotify library. That means my morning briefing shows up right alongside my playlists and podcasts. No separate app, no extra step. I just hit play.


## Why this works


The magic is not in any single component. It is in the combination.


Each tool does one thing well:


- OpenCode handles the intelligence (what to surface and how to structure it).
- Edge TTS handles the voice (turning text into something listenable).
- Spotify handles the delivery (putting the audio somewhere I already am every morning).


The result is a system that feels seamless precisely because it is built from parts that were already good at their jobs.


There is also something worth noting about the cost: zero. All three components are free. This is not a prototype that requires a budget to scale. It is a workflow anyone can replicate.


## What this points to


Voice interfaces have been promised for years. Siri, Google Assistant, and Alexa all gestured at this kind of ambient, context-aware assistance. None of them quite delivered, at least not for knowledge workers who need something more than weather updates and timers.


What has changed is the underlying intelligence layer. With tools like OpenCode able to reason over your actual communications (your Slack, your email, your calendar), the briefing can be genuinely useful rather than generic.


We are at a point where building your own version of what big tech promised is not only possible, it is practical. The components exist. The integrations exist. The only thing required is the willingness to put them together.


## Conclusion


My morning podcast is not a product. It is a workflow, one I assembled in a weekend from tools that already existed. But it has changed how I start my day in a meaningful way. I am less reactive, better informed, and I get to drink my coffee while I catch up.


If you have been waiting for an AI assistant that actually knows your context and can brief you on it, you do not have to wait anymore. The pieces are already there.


**Built with:** OpenCode (daily briefing skill + Slack + Gmail integration), Microsoft Edge TTS, Spotify save-to-Spotify tool.
