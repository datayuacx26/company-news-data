---
schema_version: "1.0.0"
document_id: "feadc0ad1a4f1aef5faba363e9c1ff650b4423198aece5ce88699dc83bca477f"
company_key: "yc-bland-ai"
company: "Bland AI"
source_id: "yc-bland-ai-news-import-c77b84ac3c61"
canonical_url: "https://www.bland.ai/blog/noise-cancellation"
published_at: "2026-06-11T18:55:00+00:00"
first_seen_at: "2026-07-21T10:41:07.173500+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:6208c34a1764b6b6c0c4ad4ae98beaf5d774243dbdce21ce1a0413b106532db5"
---

# The real world is loud. Now your AI agent won't care.

[Back to blog](https://www.bland.ai/blog)


# The real world is loud. Now your AI agent won't care.


Background noise is one of the sneakiest ways voice agents fail in the field. Bland's V2 noise cancellation filters it before the model hears it. 16% lower word error rate across 3,744 live calls.


[Reece](https://www.bland.ai/blog/author/reece) June 11, 2026


Updated June 16, 2026


2 min read


The world is loud. Callers aren't in controlled environments, and all that ambient chaos works against your voice agent.


Here's what happens. The transcriber picks up a side conversation and the agent responds to it. Environmental noise triggers a false interruption and cuts the caller off mid sentence. In loud enough conditions, transcription degrades so badly that the agent just stops talking. Even the best systems struggle, and at best it's awkward for the caller.


We just shipped V2 of Bland's noise cancellation. Here's what changed.


## The numbers#


We ran the updated system across 3,744 live production calls. With noise cancellation enabled, Word Error Rate improves by 16%.


Word Error Rate measures how accurately speech gets transcribed. The lower it is, the fewer mistakes. A 16% improvement means significantly fewer misheard words, fewer hallucinations, and fewer moments where the agent goes sideways because it heard something it shouldn't have.


## How it works#


The system sits upstream of the transcriber. It filters noise out of the audio before the signal ever reaches the model, so the model never has to deal with it in the first place. Cleaner input, better everything downstream.


## Turning it on#


Noise cancellation can be enabled in three places, depending on how broadly you want to apply it.


**Personas.** Enables it at the agent level. Every call a Persona handles has noise cancellation on.


**Send Call.** Enables it per call, when you want more selective control.


**Inbound phone numbers.** Applies it to every call coming in to a specific number.


## Worth enabling?#


The answer is an emphatic yes. A 16% improvement in Word Error Rate isn't a marginal gain. It's the difference between an agent that can handle a call from a busy street and one that stutters and stalls. Cleaner transcription means fewer hallucinations, fewer false interruptions, and better resolution on calls that would have gone sideways before.


The feature is live. Try it now[in Bland](https://app.bland.ai/) and let us know what you think.
