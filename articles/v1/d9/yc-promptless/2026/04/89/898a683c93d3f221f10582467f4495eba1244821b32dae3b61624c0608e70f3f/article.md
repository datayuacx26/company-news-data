---
schema_version: "1.0.0"
document_id: "898a683c93d3f221f10582467f4495eba1244821b32dae3b61624c0608e70f3f"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/self-serve-doc-collection-editing/"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T10:20:48.668154+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:62d66b1d1212056f9ddb72d4b941c46a5315807cd540125474ba3bb8fae8d30a"
---

# Edit Doc Collection Settings Without Contacting Support

# Edit Doc Collection Settings Without Contacting Support


[← Back to Blog](https://promptless.ai/blog)


Doc collection settings can now be edited directly from the Promptless dashboard. If you need to update your docs framework, config path, published URL, Vale config, or Doc Detective settings, you can do it yourself. No support request, no waiting.


## The problem


Section titled “The problem”


Setting up a Promptless doc collection involves choosing which documentation framework you’re using, where the framework config lives in your repo, what URL your published docs are served from, whether you’re using Vale for linting, and whether Doc Detective is enabled.


Most teams get this right during onboarding. But documentation setups change. A team might reorganize their repo and move the config file. They might migrate from one docs framework to another. They might enable Vale or Doc Detective after the initial integration. Their published URL might change after a domain migration.


Until now, every one of those changes required a support request. You’d open a ticket, describe what needed to change, and wait for someone on our team to update it. Depending on timing, that could mean days before Promptless could generate accurate suggestions again. For teams running multiple doc collections, a single infrastructure change could mean several tickets, handled in sequence.


The friction was out of proportion to the task.


## What changed


Section titled “What changed”


There’s now an edit button on each doc collection card in the dashboard. Clicking it opens the collection configuration form with your current settings pre-filled. You can update:


**Docs framework.** If you’ve migrated from Docusaurus to Starlight, or from GitBook to Mintlify, update the framework setting so Promptless knows how to parse and write to your docs.


**Config path.** Promptless uses your framework’s config file to understand your doc structure. If you’ve reorganized your repo and the config file moved, update the path here.


**Published URL.** Promptless uses your published URL when generating links and references in documentation. Update this if your domain changed or you moved to a subdomain.


**Vale config.** If you added Vale linting after initial setup, or if your Vale config location changed, update it here so Promptless respects your style rules.


**Doc Detective settings.** Doc Detective support can be enabled or adjusted on existing collections without starting over.


Changes take effect immediately for new suggestions.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## Who benefits most


Section titled “Who benefits most”


Teams that set up Promptless early and have since changed their docs infrastructure benefit most. Common scenarios include framework migrations, repo reorganizations, adding Vale or Doc Detective after the initial integration, and domain changes.


It also helps teams that caught a mistake in their initial setup. If the config path was wrong from the start, suggestions might have been targeting the wrong location. That’s now a one-minute fix.


For teams running multiple doc collections across several repositories, this is more significant. Any infrastructure change that previously required several sequential support requests can now be handled in a single session.


## How to use it


Section titled “How to use it”


Open the Promptless dashboard and go to your project settings. Find the doc collection you want to update and click the edit button on the collection card. Update the fields and save.


The form uses the same fields as initial setup. If you have an open support request to update a collection setting, you can close it and make the change yourself.


## More from the blog


- [Slack Notifications for Suggestion Outcomes](https://promptless.ai/blog/product-updates/suggestion-lifecycle-notifications) Product Updates


- [Connect Multiple GitHub Organizations to One Promptless Account](https://promptless.ai/blog/product-updates/multi-org-github-connect) Product Updates


- [Comment @promptless on a PR to Request Documentation](https://promptless.ai/blog/product-updates/request-docs-via-pr-comments) Product Updates


[← Back to Blog](https://promptless.ai/blog)
