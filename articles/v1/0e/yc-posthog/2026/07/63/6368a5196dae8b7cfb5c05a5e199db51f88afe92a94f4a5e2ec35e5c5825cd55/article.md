---
schema_version: "1.0.0"
document_id: "6368a5196dae8b7cfb5c05a5e199db51f88afe92a94f4a5e2ec35e5c5825cd55"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/replay-vision"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-06T14:07:50.182055+00:00"
fetched_at: "2026-08-06T14:07:55.105595+00:00"
content_hash: "sha256:85378486b224da46a80e2ce1ea87810bc79fc5445538659347adfd1ed424623b"
---

# Replay Vision watches the session recordings you never will

# Replay Vision watches the session recordings you never will


- [Sara Miteva](https://posthog.com/community/profiles/35224)


Jul 31, 2026


- [Product updates](https://posthog.com/blog/product-updates)


,
- [AI](https://posthog.com/blog/ai)


,
- [Session replay](https://posthog.com/blog/session-replay)


,
- [Self-driving](https://posthog.com/blog/self-driving)


#### Contents


-
-
-
-
-


Somewhere in your session recordings is the moment a user rageclicked a dead button and walked out. There was no error thrown, and nobody filed a ticket – most users don't bother. They just leave. Unless you watch every session one by one, you'll never know it happened.


[Replay Vision](https://posthog.com/replay-vision)


, live today, is our AI layer for[session recordings](https://posthog.com/session-replay)


that finds where users are struggling. It's like a detective in your product, looking for clues on your behalf.


After it finds all clues, it actually solves the mystery and sends the cracked case in the form of a PR directly to your[Inbox](https://posthog.com/docs/self-driving/inbox)


. This is how[self-driving](https://posthog.com/docs/self-driving)


works: once it detects a UI inconsistency, the fix is automatically drafted, and all you need to do is review and merge.


Before we asked anyone to point Replay Vision at their product, we pointed it at ours. It cracked two cases in a week – same MO both times: the user saw no error, the logs pointed at the wrong suspect, and the only honest witness was the recording.


##


Case one: the button that only looked clickable


On the Web Analytics installation health tab, a line reading "Complete the PostHog installation to start seeing events" looked like the thing to click. It was plain text. The real button sat next to it under a different label and opened docs in the same tab, which unloaded the app and ended the recording, so the whole page basically played dead.


A scanner reading those sessions flagged the dead click. Following the trail, it also caught the page firing a throttled refresh on every load, throwing 24,740 rate-limit errors in 30 days without ever returning fresher data, so the one thing our error tracking had plenty of was the symptom nobody was actually hitting.


The fix relabeled the button, opened docs in a new tab, and stopped the refresh storm. Merged:[posthog#67643](https://github.com/PostHog/posthog/pull/67643)


.


##


Case two: the search that swore the event didn't exist


The taxonomic filter is the picker you use to choose events and properties across insights, dashboards, and experiments. Search it for an event you know exists, and every so often it insisted: "No results." Broaden the search and the event strolled right back in, like it had somewhere to be. Two recordings about 16 hours apart caught users typing an exact event name, getting nothing, and second-guessing themselves.


A 60-second cache had pinned one transient empty result and kept serving the blank, while the "No results" state showed before the real answer had landed. The fix stops caching empty results and holds the loading state until the fresh answer arrives. Merged:[posthog#67499](https://github.com/PostHog/posthog/pull/67499)


.


##


Neither case was cracked by a human


Both PRs above reached us the same way:


1. A scanner spotted the struggle in a recording.
2. That finding became a signal in our[Inbox](https://app.posthog.com/inbox)


.
3. A background agent investigated it and opened a pull request.
4. It landed in our inbox for review. We read both, and merged them.


That's the[self-driving loop](https://posthog.com/self-driving)


, and Replay Vision is what plugs session recordings into it: the witness that used to need a human watching is now something the loop can act on.


##


Point a scanner at your sessions


Here's how it works: You set up a scanner, aim it at the sessions you care about, and it works one job on each, on demand, or on a schedule.


- **Monitor** – answer a yes/no question, like "did this user hit an error and quietly give up?"
- **Scorer** – rank sessions by frustration, so you watch the 10 that matter, not the 10,000 that don't.
- **Classifier** – tag what each user was trying to do.
- **Summarizer** – write up what happened in your product's words: "went through onboarding, got stuck requesting parental consent."


Every observation links back to the exact moment in the recording, so you can watch the evidence before you act on it. And because scanners watch the rendered screen, they catch problems that never throw an error: spinners that never resolve, buttons that only *look* clickable but aren't actually clickable.


##


Or just ask


You can also ask your agents to set up your scanners for you or tell you what users are doing from[Slack](https://posthog.com/slack)


,[PostHog Desktop](https://posthog.com/desktop)


, or your editor via the[PostHog MCP](https://posthog.com/docs/replay-vision/mcp)


:


Set one up and see what it'll cost:


```text
Set up a scanner that flags mobile users who abandon checkout, and estimate what it'll cost me per month.
```


[Try it with PostHogAI](https://app.posthog.com/#panel=max:Set%20up%20a%20scanner%20that%20flags%20mobile%20users%20who%20abandon%20checkout%2C%20and%20estimate%20what%20it%27ll%20cost%20me%20per%20month.)


Run one on a recording you're already looking at:


```text
Run a frustration score on this recording and tell me where it dropped.
```


[Try it with PostHogAI](https://app.posthog.com/#panel=max:Run%20a%20frustration%20score%20on%20this%20recording%20and%20tell%20me%20where%20it%20dropped.)


Read back what a scanner has already found:


```text
What are the top three issues my dead-ends scanner found on the pricing page this week? Show me an example recording for each.
```


[Try it with PostHogAI](https://app.posthog.com/#panel=max:What%20are%20the%20top%20three%20issues%20my%20dead-ends%20scanner%20found%20on%20the%20pricing%20page%20this%20week%3F%20Show%20me%20an%20example%20recording%20for%20each.)


Point one at the users you're losing:


```text
Summarize the last session of every user who churned this month and give me your best guess why.
```


[Try it with PostHogAI](https://app.posthog.com/#panel=max:Summarize%20the%20last%20session%20of%20every%20user%20who%20churned%20this%20month%20and%20give%20me%20your%20best%20guess%20why.)


Put one on an experiment:


```text
Create a scanner that watches my checkout experiment and tells me how users behave differently in each variant.
```


[Try it with PostHogAI](https://app.posthog.com/#panel=max:Create%20a%20scanner%20that%20watches%20my%20checkout%20experiment%20and%20tells%20me%20how%20users%20behave%20differently%20in%20each%20variant.)


Behind each one, the agent calls the Replay Vision MCP tools, drafts or runs the scanner, and reads the findings back.


[Open Replay Vision](https://app.posthog.com/replay)


and put a scanner on a flow you care about. It'll watch the footage you were never going to.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/context-warehouse) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
