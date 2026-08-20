---
schema_version: "1.0.0"
document_id: "9abcd02321e12cd4af4dc236406b14d0ad947d358b3ddc31828f35ab8e30785c"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/multi-org-github-connect/"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T10:20:48.668154+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:3994bad422faf229217ec43b2d4ee86a6a84e234e5a5dfa999f379ffb73fe94d"
---

# Connect Multiple GitHub Organizations to One Promptless Account

# Connect Multiple GitHub Organizations to One Promptless Account


[← Back to Blog](https://promptless.ai/blog)


Promptless now supports connecting multiple GitHub organizations to a single account. If your engineering setup spans more than one GitHub org, you no longer need a separate Promptless account for each one.


## The problem


Section titled “The problem”


Most Promptless customers have one GitHub organization. But some teams have two or more, spanning combinations like an internal product org and an open-source org, separate orgs for different product lines, or a company org alongside an acquired team’s GitHub namespace. Until now, the only way to use Promptless across these was to create separate accounts and manage them separately. That meant duplicated trigger configuration, separate notification settings, and no unified view of documentation work across orgs.


## What changed


Section titled “What changed”


After you connect your first GitHub organization, a “Connect another GitHub Org” option appears in Settings. Connect as many additional orgs as you need using the same OAuth flow you used for the first.


Once connected, all repos from all your orgs appear in project dropdowns. Repos are prefixed with the org name (for example,` acme/docs` ) so you can tell them apart when multiple orgs have similarly named repositories. Each org’s GitHub integration is managed independently, so you can disconnect one org without affecting the others.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## Who benefits most


Section titled “Who benefits most”


This is most useful for teams that maintain documentation across multiple GitHub organizations for legitimate organizational reasons. The most common examples are a company that ships both internal tooling and an open-source SDK through separate GitHub orgs, a team that acquired another company and now maintains their GitHub org separately, or an engineering org that separated infrastructure and product into distinct GitHub namespaces.


If you currently manage multiple Promptless accounts to work around the single-org limit, consolidating them will give you a unified view of all your projects and triggers.


## How to set it up


Section titled “How to set it up”


Go to **Settings > Integrations > GitHub** . After your first org is connected, a **Connect another GitHub Org** button appears at the bottom of the GitHub section. Clicking it starts a standard GitHub OAuth flow. Once authorized, that org’s repos are immediately available in project dropdowns.


Each connected org appears as a separate entry in the integrations list. To remove one, click its disconnect button. Removing an org does not affect projects or triggers from your other orgs.


If you’re consolidating from multiple Promptless accounts and want to migrate existing project and trigger configuration, contacthelp@gopromptless.ai .


## More from the blog


- [Slack Notifications for Suggestion Outcomes](https://promptless.ai/blog/product-updates/suggestion-lifecycle-notifications) Product Updates


- [Edit Doc Collection Settings Without Contacting Support](https://promptless.ai/blog/product-updates/self-serve-doc-collection-editing) Product Updates


- [Comment @promptless on a PR to Request Documentation](https://promptless.ai/blog/product-updates/request-docs-via-pr-comments) Product Updates


[← Back to Blog](https://promptless.ai/blog)
