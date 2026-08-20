---
schema_version: "1.0.0"
document_id: "5b4d38df375f3f1e4b2de556ff12e11da6ab1b2d159cf6cfee2bf5e9c46a9d94"
company_key: "yc-dayflow"
company: "Dayflow"
source_id: "yc-dayflow-rss-659ae9f20797"
canonical_url: "https://www.dayflow.so/blog/adhd-time-blindness-app/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.227507+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:ff299a8c0c2df49f1907668945912c1bfdb0ade0d700411d3fd45cb124e8c52d"
---

# An app for ADHD time blindness: making lost hours visible

Time blindness - the ADHD experience of hours passing without any felt sense of their passing - has a cruel interaction with every productivity tool that asks you to log things. A timer you have to remember to start is precisely the kind of prospective-memory task that ADHD makes hard. The tool that is supposed to help with time awareness demands time awareness to operate.


The fix is a tracker that requires nothing from you: no timers, no tags, no check-ins. It just watches, and at the end of the day it shows you where the hours went.


## What time blindness actually costs


It is 6pm and you cannot account for the day. You know you were busy - you never stopped - but the visible output is small and your memory offers three fragments. The usual response is self-blame (“I wasted the whole day”), which is often factually wrong: the day went to real things - a rabbit-hole that was actually research, four context switches someone else caused, an hour of helping a teammate - that left no trace in memory.


You cannot fix a day you cannot see.


## An automatic journal makes the day visible


[Dayflow](https://www.dayflow.so/) is a free, open-source app for Mac built around exactly this. It records your screen at one frame every 10 seconds and has AI write a readable story of your day: what you did, when, for how long - in plain sentences, not charts. There is nothing to start, stop, tag, or categorize. At 6pm, instead of a fog, you get a timeline you can scroll.


Three things about that experience matter specifically for ADHD:


1. **It is retrospective, not prospective.** All the demands on your memory are removed. The journal happens whether or not you remember it exists.
2. **It replaces judgment with information.** “Where did my day go” gets a factual answer. Often the answer is kinder than the fog: the day went somewhere real.
3. **It makes the invisible switches visible.** Context switches are the tax ADHD pays all day; seeing 14 of them laid out is the first step to negotiating with them.


## Privacy, because a screen recorder should earn trust


An app that watches your screen has to be trustworthy. Dayflow is open source (MIT license, 6,600+ stars on GitHub - you can read the code) and 100% local: recordings and the timeline database live on your Mac and never leave it. With a local AI model via Ollama or LM Studio, not even the analysis leaves your machine. There is no account and no cloud.


## Honest limitations


Dayflow only sees the Mac. Time on your phone - a real factor in most vanished hours - is invisible to it, as is anything away from the keyboard. It is an awareness tool, not a treatment; it pairs with strategies from an ADHD-informed clinician rather than replacing them. And it does not block or nag: if you want in-the-moment interruption, a[blocker is a different tool](https://www.dayflow.so/blog/deep-work-review/) .


## Try the 6pm test


Install[Dayflow](https://www.dayflow.so/) (free, macOS 14+, about 100MB of RAM), let it run for one normal day, and look at the timeline at 6pm. For most people with time blindness, seeing an actual account of the day - not a guess, a record - is the single most useful thing a piece of software has done for the problem.
