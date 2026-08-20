---
schema_version: "1.0.0"
document_id: "fe657c1a624a62cf3f227b1d518615fd3dce749c72474353bafce7944f57d4a1"
company_key: "yc-mattermost"
company: "Mattermost"
source_id: "yc-mattermost-rss-3d807a20d23e"
canonical_url: "https://mattermost.com/blog/when-the-shift-changes-the-prompt-stays/"
published_at: "2026-07-02T18:19:12+00:00"
first_seen_at: "2026-07-20T23:24:03.492829+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:46b94df7e7d0ddccb78163da44861a4d15151bf908f035ad2129f0e74698a0d3"
---

# When the Shift Changes, the Prompt Stays

It’s 15:55 at a regional water-treatment plant. The outgoing shift supervisor at plant-north has fifteen alarms in various states of acknowledgment, a maintenance window open on a chlorine-dosing pump, two open work orders, and a pending OEM service call. The incoming supervisor walks in at 16:00 and starts asking questions. Somewhere in between, a handover brief gets written, sometimes as a careful log, more often as a rushed paragraph the next supervisor has to translate into context.


That 15-minute gap, three times a day, is where institutional knowledge either carries forward or gets lost.


The supervisor who works out exactly how to brief the next shift keeps that recipe in their head, in a personal notes file, or in a SharePoint page nobody syncs to anymore. When they rotate out, the recipe goes with them.


Custom prompts in[Mattermost Agents V2](https://mattermost.com/blog/introducing-mattermost-agents-v2-the-next-evolution-of-intelligent-workflows/) close that gap. What one supervisor tunes once, every supervisor runs identically. It’s stored in the on-prem Mattermost database, standard across the team, and pinned in the agent sidebar next to the channel, so it’s one click away at end of shift.


If you used Mattermost Agents in 2025, the buttons above each new chat were ours: fixed, generic, scaffolding. The pinned buttons in V2 come from your team and reflect what the work actually requires.


## **One prompt, one click, end of shift**


The supervisor opens the message composer in` #ops-plant-north` , picks **AI Actions → Custom prompts → Manage prompts** , clicks **Create new** , and writes the template once:


```text
{{.Time}} - End-of-shift handover for {{.Channel}}.


From the last 8 hours of channel activity, fill in this template. Keep every heading, and write "None" under any section with nothing to report.


Active alarms
- <alarm id> (<severity>) - <what it is>. <status and acknowledgment notes>


Open work orders
- <work order id> - <description>. Owner: <name>. <status; expected resolution>


In-progress maintenance windows
- <location> - <scope>. Window ends <time>. <approvals obtained>


Action items for incoming shift
- <action item>


Safety / compliance notes
- <note> (e.g. regulatory hold-points, permit-required work)
```


They name it “Shift Handover Brief,” mark it Public so every supervisor in the workspace can use it, and pin it to the agent sidebar that opens beside the channel. From there, end of shift is a single click. The agent reads the channel history, fills in the template from it (with` {{.Time}}` resolving to the current UTC timestamp and` {{.Channel}}` to` ops-plant-north` ), and produces the brief in the same structure every shift, every supervisor, every time.


Here’s what the filled-in brief looks like in` #ops-plant-north` (illustrative):


```text
Thu, 30 Apr 2026 23:55:00 UTC - End-of-shift handover for ops-plant-north.


Active alarms
- AL-2241 (high) - Tank 2 turbidity above setpoint. Acknowledged at 22:12; samples pulled, awaiting lab.
- AL-2244 (medium) - Pump P-201A bearing temp warning. Acknowledged at 23:20; mechanical paged.


Open work orders
- WO-7733 - Replace chlorine-dosing pump impeller. Owner: J. Reyes. In progress; expected close 01:30.
- WO-7741 - Recalibrate intake flowmeter. Owner: T. Patel. Parts on order.


In-progress maintenance windows
- Pump-house Bay 3 - chlorine dosing maintenance. Window ends 01:00. Permit-to-work signed by chief operator.


Action items for incoming shift
- Verify Tank 2 turbidity sample results; restore alarm if normal.
- Confirm WO-7733 closure before reopening Bay 3 to automated control.


Safety / compliance notes
- Permit-required work in Bay 3 active until 01:00. Lockout-tagout in place.
- Daily DPD residual log due before EOS, pending T. Patel.
```


The incoming supervisor reads it, posts a thread acknowledgment, and is operational in under five minutes. The outgoing supervisor doesn’t retype the framing. The chief operator, reviewing later, sees three handover briefs in the same format: anomalies easy to spot, patterns easy to query.


## **The same prompt, different operators**


Custom prompts matter in high-stakes environments because a team’s institutional muscle memory finally has somewhere durable to live. Which caveats apply on a given reporting line. Which alarms are routine and which get escalated. Which standing context every analyst needs at the top of every session. Today that lives in a senior person’s head, a personal notes file, or a SharePoint page nobody syncs to. Tomorrow it lives in a template the whole team runs identically.


Take an all-source intelligence analyst on an analysis team on a classified network. Every intelligence-summary drafting session starts with the same four sentences pasted from a personal document: classification marking, originating element, reporting period, standing caveats. Five to eight times a shift. When the analyst rotates out, their replacement spends days finding and validating the right framing. Two analysts on the same team can drift into different caveat language because they copied the boilerplate at different times.


A senior analyst writes the framing once as a custom prompt and marks it Public, and the whole team works from identical language until the team lead edits it. Same mechanism as the shift-handover prompt: a Public template stored centrally, pinned per analyst, rendered with the current context when it’s run.


A threat-detection team has the same shape of problem: a stale SharePoint page, half the team working from an outdated version of the framework they map their detections to. The tech lead replaces the SharePoint page with a custom prompt. When that framework publishes a new version, they edit the prompt once, and every analyst picks up the new framing the next time they run it. No document churn, no re-pinning, no version drift. The default path, pin once and click once, is also the only path. There’s no wrong way left to use it.


Three different missions, the same workflow change.


## **What actually changed**


Back at` plant-north` , it’s 15:55 again. The supervisor closes a maintenance ticket, clicks **Shift Handover Brief** , and the brief renders with the timestamp, the channel, the alarms, and the open work orders. The incoming supervisor scrolls down at 16:00 and is operational in under five minutes.


What changed is that a habit is now durable. The handover recipe survives the shift change, the supervisor’s next rotation, and their replacement learning the role. The institutional knowledge of plant-north isn’t stuck in one supervisor’s notes file anymore, and every supervisor runs the same brief.


## **Try it**


Open the message composer in the channel where the work actually happens. Choose **AI Actions → Custom prompts → Manage prompts** . Write down the framing you keep retyping at the start of every shift, every analysis session, every report. Mark it Public if your team will use it. Pin it. Then watch what happens at the next shift change, the next rotation, the next time a framework version increments.


For the full reference (variables, sharing rules, pinning behavior, and rendering semantics) see the[Custom Prompts documentation in the Mattermost Agents docs](https://docs.mattermost.com/end-user-guide/agents.html#use-custom-prompt-templates) .


## Read More Platform Articles


## Open source news, right in your inbox


## Thanks for subscribing!
