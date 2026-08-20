---
schema_version: "1.0.0"
document_id: "e345ec3c567a68df995fb5fe2b7f5d79b4938a83ef51491982fea1c1e0b58be3"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/suggestion-lifecycle-notifications/"
published_at: "2026-05-25T00:00:00+00:00"
first_seen_at: "2026-07-22T10:20:48.668154+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:bafa9b07f83263a40726b5a5394751f36ff08c58c60ce5180f5dab8ce51b7338"
---

# Slack Notifications for Suggestion Outcomes

# Slack Notifications for Suggestion Outcomes


[← Back to Blog](https://promptless.ai/blog)


Promptless now posts a Slack follow-up when a suggestion resolves. When the underlying docs PR is merged or closed, or when the suggestion is rejected or archived, Promptless replies in the Slack thread where it originally announced the suggestion. The feature is opt-in. An admin enables it once in Organization Settings.


This closes a feedback loop that was previously missing. Suggestions in Promptless move through a review cycle, but the outcome wasn’t surfaced anywhere outside the dashboard or the associated docs PR.


## The problem


Section titled “The problem”


When Promptless generates a suggestion, it often touches people beyond whoever kicked it off. A developer opens a PR, Promptless generates documentation suggestions for the changes, and then a technical writer reviews those suggestions in the dashboard. Or a content team member triggers a documentation task via a Slack message, and suggestions get distributed across several reviewers.


In either case, there was no feedback loop. When a suggestion was merged, the people watching the original Slack thread didn’t see the outcome. When a suggestion was rejected, the person who triggered the review didn’t know why or when. The only way to track outcomes was to check the dashboard directly or watch the docs PR.


For teams with a steady cadence of suggestions across multiple repositories, this meant either ignoring outcomes entirely or building a manual tracking habit that didn’t scale.


## What changed


Section titled “What changed”


Admins can now turn on **Suggestion Status Updates** in Organization Settings. When the toggle is on, Promptless posts one concise reply in each Slack thread where it originally announced a suggestion. Follow-ups fire when a docs PR is merged, when a docs PR is closed without merging, when a suggestion is rejected from the dashboard, or when a suggestion is auto-archived for staleness.


Delivery is best-effort. The lifecycle transition itself always completes. If the saved Slack thread for a suggestion is missing or no longer reachable, Promptless logs the skip and moves on.


There is a single setting at the organization level. No per-member opt-in is required. Once an admin enables it, every suggestion the org resolves from then on gets a follow-up in its thread.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## Who benefits most


Section titled “Who benefits most”


Teams whose suggestions get reviewed across more than one surface. If a writer triages in the dashboard while developers and PMs watch in Slack, the dashboard side already knew when a suggestion resolved. The Slack side didn’t, until now. The reply lands in the same thread, so anyone who was paying attention to the original suggestion sees the outcome without needing dashboard access.


The same applies to suggestions triggered from Slack in the first place. Whoever started the thread, or anyone tagged into it later, sees how it landed.


## How to use it


Section titled “How to use it”


1. Go to **Organization Settings** in the Promptless dashboard.
2. Find the **Suggestion Status Updates** section.
3. Check **Post Slack follow-ups when suggestions are resolved** and save.


Follow-ups will start appearing the next time a suggestion in your org transitions to merged, closed, rejected, or archived. They go into the suggestion’s existing Slack thread, in whichever channel Promptless originally used to announce it.


## More from the blog


- [Connect Multiple GitHub Organizations to One Promptless Account](https://promptless.ai/blog/product-updates/multi-org-github-connect) Product Updates


- [Edit Doc Collection Settings Without Contacting Support](https://promptless.ai/blog/product-updates/self-serve-doc-collection-editing) Product Updates


- [Comment @promptless on a PR to Request Documentation](https://promptless.ai/blog/product-updates/request-docs-via-pr-comments) Product Updates


[← Back to Blog](https://promptless.ai/blog)
