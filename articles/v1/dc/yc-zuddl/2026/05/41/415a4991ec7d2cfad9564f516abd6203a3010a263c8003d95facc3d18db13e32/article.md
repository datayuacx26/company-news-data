---
schema_version: "1.0.0"
document_id: "415a4991ec7d2cfad9564f516abd6203a3010a263c8003d95facc3d18db13e32"
company_key: "yc-zuddl"
company: "Zuddl"
source_id: "yc-zuddl-rss-52104e166c84"
canonical_url: "https://www.zuddl.com/blog/file-uploads-smarter-check-in-and-attendee-led-cancellations"
published_at: "2026-05-11T09:49:12+00:00"
first_seen_at: "2026-07-26T06:39:34.720848+00:00"
fetched_at: "2026-07-28T22:08:49.880649+00:00"
content_hash: "sha256:7d382668584600b712721d980c919e3a7a1179a93007a16cfcf3ca2d0b27f1c0"
---

# April Product Pulse: File Uploads, Smarter Check-In, and Attendee-Led Cancellations | Zuddl Blog

## **Scan Any Badge and Check In Event Attendees**


Enable Third-party Event Badge Scanner in your check-in settings, and the Zuddl onsite app can scan any physical badge - from any event. OCR reads the attendee's name and available details. The app matches that against your field event registrations in real time.


Match found - check-in completes in one tap. No match - the scanned data pre-fills the registration form. Staff review, submit, and the attendee is processed as a walk-in on the spot.


**What this changes:**


- Attendees no longer need a Zuddl-issued QR code to check in
- Walk-in processing at third-party events becomes fast and structured
- Manual name lookup - the slowest, most error-prone step at any door - is removed


*Current release: Online mode only. Kiosk mode and offline support are in progress.*


## **Collect Documents During Registration At Ease**


Organizers can now add a file upload field to any registration flow. Choose the allowed file types - JPG, PNG, or PDF - set a size limit between 1 MB and 10 MB, and the field is live. Attendees upload during registration. Files appear immediately in People > Attendees, approval queues, and pending registration tables - from the moment the form is submitted.


For teams running approval flows, this matters more than it might seem. Files arrive with the registration, not after a follow-up thread.


**What this changes:**


- Document collection becomes part of registration, not a separate process after it
- Approval decisions can be made with complete information from day one
- Operations teams manage files directly in the platform, without external links or shared drives


*Available for all event types, live now.* *Note: Files are not included in CSV exports or sent via integrations.*


## **Let Attendees Cancel Registrations By Themselves**


Cancellations often happen in response to a calendar invite or an email to the organizer. Capacity stays reserved. Reporting stays stale.


Organizers can now enable self-cancellation with a single toggle. Attendees cancel from a link in their confirmation email, calendar invite, or from the modify registration page. They select a reason, confirm, and access is revoked immediately. Cancelled registrations appear in People > Attendees under a dedicated status filter.


**What this changes:**


- Cancellations no longer require organizer intervention to process
- Capacity is released in real time, not with a delay
- Reporting accuracy improves without additional manual work


*Note: Available only for non-ticketed flows.*


## **Improvements**


#### **Slack Notifications, Configured Per Event**


**‍** Each event can now send Slack notifications to its own dedicated channel - or inherit the org-level one. Notifications can be disabled per event, and types (Registered, Attended, Pending for Approval) toggled individually. When the org-level channel changes, events using it update automatically. Events with a custom channel are unaffected.


#### **Network Pre-Checks Before You Go Live**


**‍** The Enter Studio screen now runs connectivity and audio/video checks before the session starts. Issues surface immediately with troubleshooting guidance. If the organizer proceeds with an unresolved issue, a warning indicator stays visible on the affected control throughout the session.


#### **Auto-Enable HubSpot for Every New Event**


HubSpot can now activate automatically for every new or duplicated event. New events inherit the global configuration. Duplicated events carry over the source event's field mappings. No per-event setup required.


#### **AI-Powered CSV Column Mapping**


When AI is enabled, the platform auto-maps CSV headers to the expected fields before review. Confident matches are applied. Uncertain fields are flagged as Needs Review. Unrecognized headers are marked Skipped. The review step stays - the manual mapping before it doesn't.


#### **Salesforce Campaign Search by Name**


**‍** Campaigns in the Salesforce integration can now be searched and selected by name. No switching tabs to look up IDs - though search by ID still works for teams that prefer it.
