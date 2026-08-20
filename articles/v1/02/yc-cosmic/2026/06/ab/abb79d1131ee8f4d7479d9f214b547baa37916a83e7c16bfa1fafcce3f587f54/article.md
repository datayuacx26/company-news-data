---
schema_version: "1.0.0"
document_id: "abb79d1131ee8f4d7479d9f214b547baa37916a83e7c16bfa1fafcce3f587f54"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/content-assignments-assign-team-members-to-objects"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:42f66dacf6c9ec615d37a4efd3ad06bb86768189dc10269dd2cf3e83e57af4f1"
---

# Content Assignments: Assign Team Members to Any Object

Cosmic now lets you assign a team member to any Object, so everyone can see who owns a piece of content without leaving the dashboard. Assignments live on the Object itself, show up in a new **Assigned to** picker in the sidebar, and can trigger an email to the person you assign.


If your team has been tracking "who's working on what" in a separate spreadsheet, you can retire that sheet now. Ownership lives right next to the content it belongs to.


## What's New


- **Assign any team member to an Object.** A new **Assigned to** picker sits right above History in the Object sidebar. Pick anyone with access to the Bucket and their avatar and name show who owns the content.
- **Optional email notification.** When you assign someone, you can send them an email letting them know, with an optional personal message. Opt out from the same dialog anytime. No email is sent when you assign work to yourself.
- **Add teammates without leaving the page.** Admins and managers can invite a new team member straight from the picker, then assign them on the spot.
- **Filter by assignee.** Filter your Objects list by **Assigned to** to see everything on one person's plate.
- **Full record keeping.** Every assignment stores who is assigned (), who assigned them (), and when ().
- **Available in the API.** The , , and fields are returned on Objects by default, and you can filter by (including and ) in queries.
- **Respects roles.** Contributors can see who an Object is assigned to but cannot change it.


## Why This Matters


Teams working with freelancers and multiple contributors usually end up tracking assignments somewhere outside their CMS: a spreadsheet, a project tool, a chat thread. That parallel system drifts out of date the moment content changes hands.


Putting assignment on the Object closes that gap. The source of truth for "who is responsible for this page" now lives on the page itself: visible to everyone, queryable through the API, and filterable in the dashboard.


## How It Works


1. Open any Object and find the **Assigned to** picker above History.
2. Select a team member with access to the Bucket.
3. Choose whether to email them, and add a short message if you like.
4. The assignment is saved to the Object and shows the assignee's avatar and name.


To find work fast, use the **Assigned to** filter on the Objects list. To build assignment into your own tools, read or query the new fields through the API.


## Try It


Open any Object, find the **Assigned to** picker above History, and pick a team member. Choose whether to email them, and your assignment is saved right on the Object. Use the **Assigned to** filter on the Objects list to see everything on one person's plate.


New to Cosmic?[Create your free account](https://app.cosmicjs.com/signup) , invite your team, and start assigning content owners in seconds.


Building your own tools? The , , and fields are returned on Objects by default and filterable in queries. See the[Objects API reference](https://www.cosmicjs.com/docs/api/objects) .
