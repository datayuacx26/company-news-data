---
schema_version: "1.0.0"
document_id: "21d47c66bda0a3b0bb95abcc8a5bf62e0a7dccdfc6c69bafa90f8176e84a7fbc"
company_key: "yc-recall-ai"
company: "Recall.ai"
source_id: "yc-recall-ai-news-import-5dc65e725852"
canonical_url: "https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints"
published_at: "2026-01-22T00:00:00+00:00"
first_seen_at: "2026-07-25T20:39:05.667365+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:a5214290f28fa9fb87ab313f7016e815a6a77ad3d3a1263c4fc466a0e8e6e281"
---

# Understanding Real-Time Media Streams' Behavior and Constraints

If you implement RTMS correctly, you will get real-time audio, video, transcripts, and event metadata with very low latency. That’s the upside, and it’s substantial.


The part that surprises teams is that starting RTMS depends on a number of user-related reasons – how the Zoom meeting was set up, host permissions, RTMS app settings, and more.


This article is meant to help PMs, CTOs, and engineers forecast the real user experience of your RTMS app. If you are looking for a primer on RTMS, check out[What is Zoom RTMS?](https://www.recall.ai/blog/what-is-zoom-rtms) Throughout this article, we’ll refer to end users simply as users.


[Testing RTMS Behaviors Video and Demo](https://youtu.be/2xUYTC5KPTk)


Before diving into the conditions surrounding RTMS, let’s define some common terms.


- **Participant** - An individual who is present in the Zoom meeting.
- **Host** - The manager of the meeting. Hosts do not need to be present in the meeting. When in the meeting, the host can take various actions that others can not such as mute participants, admit participants from the waiting room, and allow in-meeting apps to run.
- **App Owner** - The participant who brings and starts the Zoom app.
- **Zoom Admin** - The individual responsible for organization-wide settings. This person is typically a member of the IT team at the organization that they are the admin for.


## A quick way to tell if RTMS will work for a meeting


The table below shows the primary conditions that determine whether RTMS will start, pause, stop, or fail in a given meeting. Each column represents a specific requirement or constraint. Each row represents a real-world meeting scenario.


This table is intended to give you a brief overview of when RTMS will get meeting data and when it will not. If a particular scenario matters to your use case, you can jump to the linked section below for a deeper explanation or check out the[appendix](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#appendix) to see how each scenario is created.


Scenario Result


**Scenario 1** :[Host’s organization](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#how-the-host-organization-impacts-rtms) disables RTMS **No RTMS data** Host’s organization must allow for RTMS


**Scenario 2** : Host’s[client version](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#zoom-client-version-requirements) is not up-to-date **No RTMS data** Meeting host’s client version must be 6.5.5+


**Scenario 3** : App owner’s[client version](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#zoom-client-version-requirements) is not up-to-date **No RTMS data** App owner’s client version must be 6.5.5+


**Scenario 4** : Meeting starts,[host is not present](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#host-presence-and-meeting-start-authority) and host approval to start RTMS is required **No RTMS data** May need to resend request in order to get data once the host joins


**Scenario 5** :[Host is not present](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#host-presence-and-meeting-start-authority) and the meeting is not allowed to start without host presence **No RTMS data** Auto-start can’t run if the host doesn’t start the meeting


**Scenario 6** : Host has set RTMS app to[auto-start](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#rtms-app-auto-start-behavior) and allowed meeting to start without host presence **Meeting data available through RTMS**


**Scenario 7** :[Host denies](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#host-approval-and-data-requests) RTMS app **No RTMS data** If host approval is required, apps cannot get data via RTMS until the host approves


**Scenario 8** : Host and app owner are present, host must approve the request for RTMS,[host approves](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#host-approval-and-data-requests) the request for RTMS **Meeting data available through RTMS**


**Scenario 9** : Host and app owner are present, host’s organization has RTMS enabled, all Zoom clients support RTMS, host[doesn’t require approval](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#rtms-app-auto-start-behavior) of RTMS app **Meeting data available through RTMS**


**Scenario 10** :[App owner never joins](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#rtms-app-owner-presence) and does not have their RTMS app set to[auto-start](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#rtms-app-auto-start-behavior) **No RTMS data** RTMS cannot start without auto-start or someone manually enabling it


**Scenario 11** :[App owner leaves](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#rtms-app-owner-presence) the meeting **No RTMS data** With app owner no longer present, app stops collecting data


**Scenario 12** : App owner moves to[breakout room](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#breakout-rooms) **No RTMS data** RTMS stops when app owner moves to breakout room


**Scenario 13** :[Meeting is handed off](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#how-rtms-can-pause-stop-and-resume-during-a-meeting) from host with RTMS enabled to a host with RTMS disabled **Meeting data available through RTMS** The meeting owner's rules govern. A new host's organization rules do not supercede the original host


**Scenario 14** :[Meeting is handed off](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#how-rtms-can-pause-stop-and-resume-during-a-meeting) from host with RTMS disabled to a host with RTMS enabled **No RTMS data** The meeting owner's rules govern. A new host's organization rules do not supercede the original host/owner


RTMS has a few prerequisites that must be satisfied before the meeting begins, like the host’s organization settings and supported Zoom client versions for both meeting host and app owner. If those prerequisites aren’t met other conditions become irrelevant and RTMS can’t start. Once those pre-meeting conditions are met, RTMS behavior is controlled by in-meeting factors such as host approval and host presence, which determine whether RTMS starts, pauses, resumes, or stops.


## How the host organization impacts RTMS


RTMS behavior is governed primarily by the meeting’s host organization. The host organization is the Zoom workplace that is hosting the meeting. While the host role may be transfered throughout the course of a meeting, when we refer to host organization, we refer to the original meeting host.


If the host’s Zoom organization admin keeps RTMS enabled, RTMS can work regardless of participant settings. If the host organization disables RTMS, no participant setting, OAuth scope, or API call can enable it. This host-centric model is intentional and shapes when RTMS works reliably in real-world meetings.


[How to set RTMS as zoom admin](https://youtu.be/XEN7AjA8hrU)


Throughout the rest of this article, nearly every instance of RTMS starting or not starting maps back to this host-governed model.


### What this means for your users


Your users may find that RTMS is allowed to capture meeting data in some environments, and isn’t allowed to receive meeting data in other environments, even when the app relying on RTMS is built correctly. That inconsistency usually maps to one thing: Are users joining meetings hosted by their own organization, or by external organizations?


- Internal-first meetings (sales enablement, support, recruiting inside one organization) tend to be a good fit, because the host of the meeting tends to be someone within your Zoom organization.
- External meetings (users joining customer meetings, partner meetings, community calls) will have more variability when using RTMS alone, because the host of the meeting could be someone outside your Zoom organization, and Zoom RTMS may not be set up in someone else’s Zoom organization.


> Note: If the hosting Zoom organization is a free Zoom account, RTMS should work, as it’s enabled by default and cannot be turned off.


## Zoom client version requirements


The host and the user of the app that relies on RTMS must both have a Zoom client version that is at least version 6.5.5+. If the host is on an unsupported or outdated client, RTMS will not start even if admin settings are correct.


Some client-related failures are effectively silent from the end-user point of view. There is no messaging when RTMS is unavailable for a particular meeting. This is why, if you decide to use RTMS, you should consider how the handle your app not receiving data after the request to start the app has been sent.


## Host presence and meeting start authority


Whether the host is present affects RTMS in two related but distinct ways: whether the meeting itself can start, and whether RTMS is allowed to begin streaming.


> In Zoom, users can set meetings to start with or without the host. In the event that the meeting is only allowed to start with the host and the host never joins, there is no meeting to pull data from.


Host presence is required for RTMS only when[host approval](https://www.recall.ai/blog/understanding-zoom-rtms-behavior-and-its-constraints#host-approval-and-data-requests) is required. In meetings where host approval is required, the host must be present in the meeting to grant RTMS access. If the meeting has started without the host and host approval is required, RTMS cannot begin streaming. In this case, the app owner must wait for the host to join and then resend the RTMS request. When host approval is not required, the host does not need to be present for RTMS to start so long as the meeting has started.


## RTMS app auto-start behavior


Auto-start allows RTMS to begin without the app owner manually triggering the app, but it only helps in specific scenarios.


If host approval **is required** , auto-start does not bypass it. RTMS will wait for approval even if the meeting is already running.


Auto-start can enable RTMS to start regardless of if the host joins when:


- the meeting is allowed to start without the host, and
- host approval is **not** required to start RTMS.


Auto-start can be particularly useful if the host wants to record the meeting using an app that relies on RTMS, they cannot attend the meeting, and they have auto-start on for their RTMS app and the meeting is set to start regardless of whether the host joins.


## Host approval and data requests


Some meetings require the host to approve apps that request real-time meeting content.


When enabled:


- RTMS remains paused until approval is granted
- if approval never comes, RTMS may never start


Approval applies to the **entire request** . There is no way for the host to approve sharing specific pieces of data requested such as transcript or audio.


If your app requests audio, video, and transcript, and the host is uncomfortable sharing any part of that request, the host denies the request and RTMS produces no data.


Requesting more media types increases the likelihood of denial simply because there are more pieces of data that the host may object to sharing.


## RTMS app owner presence


In some cases, RTMS can start and continue without the app owner actively participating.


The app owner’s presence matters in cases where the app owner joins the meeting and then leaves the meeting. In that case RTMS stops when the app owner leaves the meeting.


If the app owner never joins the meeting and the app starts using auto-start then the app will run without the app owner’s presence.


## Breakout rooms


As of January 2026, RTMS does not capture breakout room content.


When a meeting enters breakout rooms, RTMS stops streaming for the duration of those sessions. RTMS may resume once the meeting returns to the main room, but any content that occurred inside breakout rooms is unavailable.


If your product depends on capturing breakout sessions, such as workshops, training sessions, or interview loops, RTMS will not work.


## Participant organization settings


Participant organization settings do not affect RTMS behavior. If a participant does not want to be recorded they should leave the meeting when RTMS is enabled to avoid having their data captured. This design ensures that real-time access policies are applied consistently across the meeting rather than negotiated participant by participant.


## How RTMS can pause, stop, and resume during a meeting


RTMS access is not decided only at the start of a meeting. Even after streaming begins, the host and host organization can still change or revoke access. As a result, based on host actions, host transitions, or changes in sharing permissions, RTMS can pause, stop, or resume during a meeting.


For product design, host approval should be treated as a condition that can change over time. Therefore, products using RTMS should consider how to capture data or alert users that the meeting data is no longer being captured when host approval changes.


In addition to approval at startup, both the host and the RTMS app owner can stop sharing real-time meeting content at any point during the meeting. When this happens, RTMS may pause or stop mid-meeting, and may resume later if sharing is re-enabled.


### What can stop RTMS during a meeting


Trigger What users see RTMS behavior Product implication


Host disables sharing RTMS stops Stream ends Handle interruption


Host re-enables sharing RTMS resumes New session Support restart


Host handoff within same organization, sharing allowed Nothing Continues to stream Normal lifecycle


RTMS app owner leaves meeting RTMS stops App session ends App owner presence required


To users, the changes listed above can result in RTMS suddenly stopping or restarting. This behavior is expected and reflects the host’s ongoing control over real-time access.


RTMS sessions cannot be resumed after a disconnect. Any data not captured during interruptions is permanently lost. Your app must wait for a new` meeting.rtms_started` event before reconnecting.


The scenarios above are not edge cases. They are the result of the host-governed permission model that RTMS operates under.


## A planning framework for PMs and CTOs


Before committing to RTMS as your only real-time path, decide which world you live in:


**World A: Mostly internal or controlled meetings**
You can standardize admin settings, client versions, and approval policy. RTMS can deliver a consistently strong user experience.


**World B: Many external meetings with unknown hosts**
Expect variability. RTMS can still be valuable, but your product experience needs:


- Clear messaging (e.g. “This meeting doesn’t allow RTMS”)
- Other capture methods to fall back to. Recall.ai supports[RTMS](https://www.recall.ai/product/rtms) as well as other capture methods so that you can access meeting data regardless of video conferencing platform or edge case.


In many cases, layering approaches can ensure that you get data no matter the use case.


## Conclusion


If you’re interested in learning more about[Zoom RTMS](https://www.recall.ai/blog/what-is-zoom-rtms) , you can check out our article on Zoom RTMS or chat with a member of our team. If you want to learn more about all of the options available for capturing meeting data, you may be interested in Recall.ai’s[Meeting Bot API](https://www.recall.ai/product/meeting-bot-api) or[Desktop Recording SDK.](https://www.recall.ai/product/desktop-recording-sdk)


## Appendix


The following table expands on the table at the beginning of the blog. Here we explore each individual flag that creates the scenario on the left and leads to the result featured on the right.


**Yes** = the condition is true, **No** = the condition is false, **Any** = condition has no impact on results.


Scenario Host Organization has RTMS Enabled Host has Supported Zoom Client App Owner has Supported Zoom Client Host Present RTMS Auto-Start Enabled Meeting Allowed w/o Host Host Approval Required for RTMS Host Approves RTMS RTMS App Owner Present In Breakout Room Participant Organization has RTMS Enabled Result


**Scenario 1** : Host’s organization does not allow RTMS No Any Any Any Any Any Any Any Any Any Any **No RTMS data** Host’s organization must allow for RTMS


**Scenario 2** : Host’s client version is not up-to-date Yes No Any Any Any Any Any Any Any Any Any **No RTMS data** Meeting host’s client version must be 6.5.5+


**Scenario 3** : App owner’s client version is not up-to-date Yes Yes No Any Any Any Any Any Any Any Any **No RTMS data** App owner’s client version must be 6.5.5+


**Scenario 4** : Meeting starts, host is not present and their approval is required. Yes Yes Yes No Any Yes Yes Any Any Any Any **No RTMS data** May need to resend request in order to get data once the host joins


**Scenario 5** : Host is not present and the meeting is not allowed to start without them Yes Yes Yes No Any No Any Any Any Any Any **No RTMS data** Auto-start can’t run if the host doesn’t start the meeting


**Scenario 6** : Host has pre-authorized sharing and allowed meeting to start without them Yes Yes Yes No Yes Yes No Any Yes No Any **Meeting data available through RTMS**


**Scenario 7** : Host does not approve RTMS Yes Yes Yes Yes Any Any Yes No Yes Any Any **No RTMS data** If host approval is required, apps cannot get data via RTMS until the host approves.


**Scenario 8** : Host and app owner are present, must approve the request for RTMS, approves the request for RTMS Yes Yes Yes Yes Any Any Yes Yes Yes No Any **Meeting data available through RTMS**


**Scenario 9** : Host and app owner are present, host’s organization has RTMS enabled, Zoom clients support RTMS, host doesn’t require approval of app Yes Yes Yes Yes Any Any No Any Yes No Any **Meeting data available through RTMS**


**Scenario 10** : App owner never joins and does not have their app set to auto-start Yes Yes Yes Yes No Any Any Any No Any Any **No RTMS data** RTMS cannot start without auto-start or someone manually enabling it


**Scenario 11** : App owner leaves the meeting Yes Yes Yes Yes Any Any Yes Yes Yes -> No Any Any **No RTMS data** With app owner no longer present, app stops collecting data


**Scenario 12** : App owner moves to breakout room Yes Yes Yes Yes Any Any Any Yes Yes Yes Any **No RTMS data** RTMS stops when app owner moves to breakout room


**Scenario 13** : Meeting is handed off from host with RTMS enabled to a host with RTMS disabled Yes -> Yes Yes Yes Yes Any Any Any Any Yes Any Any **Meeting data available through RTMS** Meeting owner governs and allows RTMS


**Scenario 13** : Meeting is handed off from host with RTMS disabled to a host with RTMS enabled No -> No Any Any Any Any Any Any Any Any Any Any **No RTMS data** Meeting owner’s organization governs and does not allow RTMS
