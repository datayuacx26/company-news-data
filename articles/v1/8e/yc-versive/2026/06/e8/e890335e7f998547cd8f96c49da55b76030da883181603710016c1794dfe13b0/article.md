---
schema_version: "1.0.0"
document_id: "e890335e7f998547cd8f96c49da55b76030da883181603710016c1794dfe13b0"
company_key: "yc-versive"
company: "Versive"
source_id: "yc-versive-news-import-a5070f4382ee"
canonical_url: "https://getversive.com/blog/june-2026-smart-logic-surveys"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-22T18:46:56.164928+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:84914e38ba4673926a7582c6745fd7ca6eb3f141d4834ba9a7202360158066a8"
---

# Smarter skip logic, survey studies, and background AI tests

[Back to updates](https://www.getversive.com/blog)


June 18, 2026 •


Product Updates


### Smarter skip logic, survey studies, and background AI tests


AI-powered skip logic for open-ended questions, a new survey study type with its own pricing, AI tests that run in the background, richer exports, and faster dashboards.


---


A roundup of what we've shipped recently: smarter branching logic, a new way to run surveys, AI tests you can walk away from, and a batch of export and performance improvements.


### Skip logic that understands open-ended answers


Skip logic has always been easy to set up for structured questions: if a participant picks option B, send them to question 5. But open-ended answers didn't have a good equivalent, because there's no fixed option to match against.


Now there is. **Smart conditions are available on open-ended questions** : describe the condition in plain language, like "the participant mentions pricing as a frustration," and the AI evaluates each participant's actual answer to decide where they go next. You can branch a study based on what people *say* , not just what they select.


We also **improved the skip logic warnings** in the study editor, so it's clearer when a rule can't be reached or logic conflicts with the question order.


### Survey studies


Some studies don't need an AI interviewer. Sometimes you just want to run a straightforward survey. There's now a dedicated **Survey** mode with its own, more affordable completion pricing, separate from your interview quota.


Surveys support all the structured question types, skip logic (including the new smart conditions), and every conversation mode. And because survey completions are metered separately, they never eat into your interview credits.


Survey plans are rolling out gradually.[Contact us](https://www.getversive.com/cdn-cgi/l/email-protection#b4dcddf4d3d1c0c2d1c6c7ddc2d19ad7dbd9) if you'd like them enabled for your workspace.


### Top up credits and see your billing history


Two related billing improvements:


- **Buy credits anytime** : if you run out of interview or survey completions mid-cycle, you can now purchase additional credits for the current billing period straight from the Billing page, no plan change needed.
- **Billing history** : a new page listing your invoices and one-time purchases, each with a downloadable invoice or receipt.


### AI tests run in the background


AI tests now run **asynchronously** . Kick off a test, close the tab, and come back whenever you like. Results update in real time as they come in, in a unified results view. If a run hits a snag, we retry it automatically, and you're only billed for runs that succeed.


Website tests also got an **extended mode** that allows up to 100 steps, for testing longer, more complex flows.


### Screener responses in your exports


If you recruit participants through panels, their screener responses are now included in **CSV exports** and in **individual and bulk transcript downloads** , so the context you screened on travels with the data instead of living only on the transcript page.


### Faster where it counts


Insights and dashboards load noticeably faster, and the study Launch tab now opens instantly instead of showing a loading screen.


### For developers


- A new **study metrics endpoint** returns completion counts, quota, and status for a study, useful for gating embedded studies server-side.
- The Embed SDK's **` complete` event** now fires for conversational and voice interviews, not just surveys.


---


### Get in touch


We're always building. If you have any questions or feature requests, reach out to us at[\[email protected\]](https://www.getversive.com/cdn-cgi/l/email-protection#234b4a63444657554651504a55460d404c4e) . Want to get started?[Sign up today](https://www.getversive.com/signup) and start running research in minutes.


---


Eric Li


, Co-Founder, Versive
