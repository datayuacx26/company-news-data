---
schema_version: "1.0.0"
document_id: "9f472c76ff2a78bc83812e7c7f91fd8e83a680c4e1d7117e89fcceb43b1e0191"
company_key: "yc-dayflow"
company: "Dayflow"
source_id: "yc-dayflow-rss-659ae9f20797"
canonical_url: "https://www.dayflow.so/blog/is-dayflow-safe/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.227507+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:9a6815ee474e87c4c56ffa8b39e1e72b4d728078a3e039730917db1841479e5a"
---

# Is Dayflow safe? What a screen-recording time tracker can and can't see

Short answer: yes - and you should not take our word for it, because you don’t have to. Dayflow is open source under the MIT license, so every claim below is verifiable in[the public source code](https://github.com/JerryZLiu/Dayflow) (6,600+ stars on GitHub).


An app that records your screen deserves a skeptical look. Here is exactly what Dayflow does with what it sees.


## Where the recordings live


Dayflow captures your screen at about one frame every 10 seconds and stores those recordings, plus the timeline database built from them, locally on your Mac. There is no Dayflow cloud, no account, and no sign-up. Delete the app and its data folder and the record is gone.


## What leaves your Mac (and what never does)


The one nuance worth understanding is AI processing. Dayflow needs a language model to turn screen activity into readable summaries, and you choose where that model runs:


- **Local mode (Ollama or LM Studio):** analysis happens on your Mac. Nothing - not recordings, not summaries, not metadata - leaves your machine. This is the configuration to pick if “airgapped from everyone including us” is your bar.
- **Bring your own key (Gemini, ChatGPT, or Claude):** periodic screenshots are sent to that provider for analysis under your own API agreement with them. Recordings still live only on your Mac.
- **Dayflow Pro:** the same as bring-your-own-key, except Dayflow’s key does the work so there is no setup.


That choice is yours at setup and reversible at any time in settings.


## What Dayflow can see - and what it can’t


With macOS screen-recording permission, Dayflow sees what is on your screen while it runs, which is what makes the timeline honest. It does not log keystrokes, does not read files, does not capture your camera or microphone, and stops seeing anything the moment you pause it or quit. Sensitive session? Pause it. It is your journal, not a compliance tool - there is no admin, no employer dashboard, and no one to report to. That difference is the entire subject of[time tracking without surveillance](https://www.dayflow.so/blog/time-tracking-without-surveillance/) .


## How the claims stay honest


Closed-source trackers ask you to trust a privacy policy. Dayflow’s architecture is inspectable: the recording pipeline, the storage paths, and the network calls are all in the open repository, and a community of thousands of developers has eyes on it. “Audit the code yourself” is a real option, and for the rest of us, the fact that anyone can is the deterrent that matters.


## The practical footprint


Safety also means “safe to leave running”: Dayflow uses roughly 100MB of RAM and under 1% CPU, because one frame every 10 seconds is a trickle, not a video stream. Battery impact is negligible - the[full battery breakdown is here](https://www.dayflow.so/blog/screen-recording-battery-impact/) .


If your question was “is it safe to let this thing watch my screen,” the honest summary is: it watches locally, it forgets nothing but shares nothing, and the code that proves it is public.
