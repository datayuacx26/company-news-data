---
schema_version: "1.0.0"
document_id: "19a5e7dafcc19117968a5fba08887458ca55dd9afd0314d591a2862af1b9f0a9"
company_key: "yc-rally-uxr"
company: "Rally UXR"
source_id: "yc-rally-uxr-news-import-94fad56766fc"
canonical_url: "https://www.rallyuxr.com/post/rally-mcp-tool-calls-explained"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T11:37:35.353241+00:00"
fetched_at: "2026-08-04T11:44:39.476301+00:00"
content_hash: "sha256:0c3d769d273dbed3b07ec1bd0723d5f7310d8d700fbeb9c0fcd04d2deeca0211"
---

# Rally MCP Tool Calls, Explained

August 4, 2026


## Rally MCP Tool Calls, Explained


A complete reference for what Rally MCP can do and why we built it this way.


Contributors:


No items found.


‍ *Hack: Copy the URL of this blog post into your AI tool of choice and ask for the following prompt:*


*You are a research operations expert reviewing documentation for Rally MCP, a Model Context Protocol server for UX research. I am going to paste the full content of a reference article below. Using only the information in that article, generate a structured report that includes:*


- *A summary of Rally MCP's product philosophy and what differentiates it from other MCP tools*
- *A complete list of all tool calls, grouped by category*
- *For each tool call: its name and what it does*
- *A count of total tool calls, broken down by category*
- *A list of which tool calls are fully automatic and require no user prompt*


## Depth over breadth with a focus on recruitment.


Many MCP servers are built the same way: wrap an existing API with tool calls, and ship as many as possible. The result is a long list of capabilities that are technically functional but shallow in practice. Agents can call them, but they cannot do much with them.


Rally MCP is built around a different question. Not "what can we expose?" but "what do agents actually need to kick off research?"


That distinction matters more than it might sound. When you build for agents rather than for API coverage, you invest differently. Individual tool calls get deeper. They carry more context, enforce more rules, and handle more of the workflow autonomously so the researcher does not have to fill in the gaps. A tool call that recruits participants does not just pull a list. It checks eligibility, applies cooldown windows, validates consent status, and enforces the criteria your Research Ops team configured in Rally before a single invitation goes out.


That is where our investment goes. Not into the number of tool calls, but into how much each one can actually do. The result is a focused set of tools built specifically around recruitment and governance, the two areas where research programs most often break down and where the consequences of getting it wrong are highest.


What follows is the complete reference for every Rally MCP tool call and what it does.


Note: We are building new tools and improving existing ones every day. Reach out to us for an updated list.


## What always needs your approval


Regardless of which tools your assistant uses, a few things always come back to you:


- Proposed sends and a study's first outreach round wait for your approval in Rally.
- Starting paid external-panel recruitment requires your confirmation on Rally's cost sheet. Reviewing a quote doesn't reserve money or notify the panel.
- Some study settings lock once recruiting has progressed; your assistant will tell you when a change is no longer possible.


## Getting started


If you haven't connected Rally MCP, setup takes a few minutes. See the connection instructions in the help center. Questions about a specific capability, or something not behaving as described here? Reach out to support.


### Study design and setup


- build_study: Creates a new study or edits an existing one (draft or live). Handles name, description, type, incentive, consent, scheduler, public-link, and survey-provider settings. Some fields lock progressively once recruiting has started (incentive once anyone's invited, duration once interviews are booked, and so on). Your assistant will flag when a change is no longer possible.
- build_screener: Writes or revises the qualifying screener questions for an interview or unmoderated-test study, before it's live. Survey studies don't have screeners.
- build_survey: Writes or revises a survey study's own questions in Rally's native builder. If the study's questions are handed off to Qualtrics, SurveyMonkey, Typeform, or a custom link instead, this doesn't apply.
- search_survey_connectors: Checks which external survey tools (Qualtrics, SurveyMonkey, Typeform) are connected and available, and looks up a specific survey to link into a study.
- map_question_properties: Auto-maps screener or survey answers onto person profile properties so responses are saved to the person record, not just the submission. Maps to existing properties only; it doesn't create new ones.
- get_consent_forms: Lists the consent forms available in the workspace, for attaching to a study.
- search_properties: Looks up person properties (built-in and custom) in the workspace, including PII/PHI flags. Useful for finding the right field for filtering or personalization.
- get_study: Fetches full configuration for one study (status, incentive, audience, recruiting progress, and the current screener if built), including in-progress draft edits not yet reflected on the live study.
- search_studies: Lists studies across the workspace with filters for owner, team, type, and status; each row includes recruiting progress.


### Recruitment planning


- check_recruiting_feasibility: Checks whether a recruiting criterion can be met from the existing Rally panel, and recommends recruiting from the panel, importing a list, or using a paid external panel.
- build_recruiting: Builds the invitation (or sign-up-link) plan for a study before it goes live: who gets contacted, the send cadence, and a message preview. Doesn't add anyone by itself; people must already be added to the study first.
- start_recruiting: Takes the study live once you've approved the recruiting plan. Nothing sends until you separately approve the first round in Rally.
- build_external_panel_audience: Configures who a paid external panel (Respondent or User Interviews) should recruit: audience type, targeting criteria, and participant time commitment.
- search_external_panel_targeting_options: Looks up what an external panel provider can target and the exact option values it accepts, so targeting criteria are built from real options rather than guessed.
- review_external_panel_launch: Shows the final itemized cost quote for an external-panel study right before launch. Can't launch anything itself; you confirm and reserve funds on Rally's cost sheet.


### People and participants


- search_people: Browses and filters the workspace people pool by natural language, and builds the saved selection ("view") that recruiting plans use.
- search_populations: Looks up existing named populations (panels) in the workspace by name.
- create_population: Creates a new population to import people into or to power public sign-up links.
- import_people: Imports a list of people (pasted or structured rows) directly into a chosen population, or checks the status of an import already in progress. Runs as a background job, not instant.
- import_people_from_file: Starts an import of a staged CSV file into a population. Runs as a background job, not instant.
- add_people_to_study: Adds a saved selection or specific people to a study (draft or live). Sends no invitations by itself.
- search_participants: Lists and filters the participants enrolled in one study by status or lifecycle stage (invited, screening, pending incentive, etc.).
- get_study_participant: Full detail on one participant: status, screener progress, approval state, consent signature, quota group.
- get_person: Full profile for one person in the workspace pool, including notes and past participation ratings.
- append_note_to_participant: Adds a note to a participant's record. Only appends; never replaces or clears existing notes.


### Email and outreach


- draft_message: Composes a one-off ad-hoc message (e.g. a thank-you note) for a group of participants on a live study, without touching the study's saved templates.
- preview_message: Renders a configured template or a draft exactly as a participant would see it. Read-only, no send.
- send_message: Proposes sending an invitation or custom message to specific participants. Always goes through your in-app review first; nothing sends until you approve it in Rally.
- edit_study_message_template: Edits one of a study's saved, permanent email templates (invitation, screener invite, booking, incentive, etc.). Locks once the study is live.
- list_study_message_templates: Lists a study's configured email templates (invitation, screener invite, booking, reminders, incentive) with whether each is enabled and when it was last edited.
- get_study_message_tokens: Returns the merge tokens (participant name, links, custom properties) available for a given study email, so copy stays personalized rather than hardcoded.
- list_message_campaigns: Lists a study's discrete email sends with delivery counts (sent, delivered, opened, bounced) per campaign.
- pause_message_campaign / resume_message_campaign: Temporarily halts or restarts one specific email campaign without affecting the rest of the study.
- cancel_message_campaign: Permanently drops a campaign's pending invitations. Cannot be undone.
- search_email_sender_addresses: Lists the personal and team sender addresses available for a study's outbound email, for choosing who a message appears to come from.
- get_participant_email_activity: Shows one participant's full email history in a study: every email sent, delivery outcome, and confirmed opens/clicks.
- get_study_email_activity: Reports a study's overall email performance (sent, delivered, opened, bounced) broken down by email type.


### Screening and qualification


- set_screener_qualification: Manually sets or overrides a participant's screener qualification (qualified / not qualified). Applies immediately since it's easily reversible.
- decide_external_panel_applicants: Approves or rejects applicants awaiting review on an external-panel study. Approving auto-invites them at the provider. Always previews before committing.


### Scheduling and interviews


- search_interviews: Lists interview sessions across studies, or for one study, filtered by date, host, or state. The basis for calendar and "what's on today" questions.
- get_interview: Full detail on one interview booking: attendees, status, and (if recorded) the observer-room link, AI summary, and recording status.
- get_interview_brief: Generates a prep brief for a scheduled interview: participant background, relevant history, suggested questions, session tips.
- search_availability: Looks up a study's open interview slots across configured hosts, for a given date range.
- schedule_interview: Books a new 1:1 interview for a participant already in the study, with the requesting researcher as host. Always previews before committing.
- reschedule_interview: Moves a scheduled interview to a new time, keeping the same host. Always previews open slots before committing.
- cancel_interview: Cancels a scheduled interview booking and notifies everyone involved. Doesn't apply to group or multi-session interviews.
- get_my_availability: Shows your (or another host's) default interview hours, caps, and calendar-conflict handling.
- update_my_availability: Changes your own interview hours, one-off date overrides, or scheduling caps (weekly/daily limits, calendar-conflict handling). Applies to every study you host, not just one.
- mark_attendance: Records whether a participant attended or no-showed a specific interview session. Always previews before committing.
- change_interview_host: Changes who's hosting an already-booked interview, preserving the time. Always previews before committing.
- add_interview_guest: Adds a live-call guest or a silent observer to a scheduled 1:1 interview.


### Incentives


- log_custom_incentive: Records a manually-fulfilled incentive (swag, credits) as sent for one or more participants. Rally tracks it but doesn't pay or deliver it; that happens outside Rally. Always previews before committing.
- resend_incentive: Re-sends the redemption email for a cash/gift-card incentive. Doesn't apply to custom incentives or externally-recruited studies.
- cancel_incentive: Cancels an unsent or unclaimed incentive.


### Reading results


- get_raw_transcript: Retrieves the full spoken transcript of a recorded interview, with speaker labels and timestamps, for quoting or detailed analysis.
- search_study_responses: Returns a study's screener or survey submissions with question-level answers.


### Managing a study's life


- pause_study / resume_study: Temporarily halts or restarts a whole study's recruiting; participation already underway keeps going.
- close_study: Permanently stops recruiting on a study. This is one-way and cannot be undone.


### Workspace and identity


These aren't part of any single study's lifecycle. They operate at the workspace level, across studies.


- get_current_user: Returns your own profile (name, email, role, and team memberships) and is how your assistant resolves "my studies" or "my team."
- search_pending_decisions: Lists what's awaiting your resolution on a study (outreach rounds, sends), with a link to review and decide in Rally. No MCP tool can approve these on your behalf.


Already a Rally customer?[Connect to Rally MCP in under 5 minutes.](https://help.rallyuxr.com/articles/5139669410-rally-mcp)


Evaluating Rally MCP and want to see governance in action across your research workflow?[Book a demo with our team.](https://www.rallyuxr.com/demo)


No items found.


Supercharge your Research Ops with Rally


Rally’s Research Ops Platform enables you to do better research in less time. Find out how you can use Rally to empower your teams to talk to their users, without disjointed tooling and spreadsheets. Explore Rally now by[setting up a demo](https://www.rallyuxr.com/demo) .


[Back to all blogs](https://www.rallyuxr.com/blog)
