---
schema_version: "1.0.0"
document_id: "bc6ab8965218ef8f5b52bd6fd72701230b105a131c844fdfcc842e4a77d23851"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/user-roles-are-now-available-in-apollo-studio"
published_at: "2020-09-01T20:17:18+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:3794e8ff13b1700138216ba3078bc2661f563b55391ba9cc736a25891c9a5c79"
---

# User roles are now available in Apollo Studio

**tldr;** at the request of many (and just because we really needed it), you can now assign specific roles to members of your organization in Studio on paid accounts. Please emailsupport@apollographql.com if you’re interested.


For years, every user in every organization in Studio has been able to operate with full admin privileges, by default. While we’ve enjoyed the days of small teams and extended trust, it’s time to get a bit more serious. Studio is at the point where organization admins need control over what permissions their users have. We’ve heard your calls and have answered. We can now turn on roles access for paid accounts on request (emailsupport@apollographql.com if you’re interested). Organization admins can now assign one of these five roles to each member of their account:


- **Billing Manager** — management access to organization-level configuration and billing. Can also remove members (but not invite them). No access to graphs.
- **Consumer** — view-only access to the organization’s graph *schemas* , but not metrics. Can also execute queries in the Explorer. Intended for application developers building clients against a graph.
- **Observer** — view-only access to the organization’s graph data, such as schemas and metrics. Can also execute queries in the Explorer. Intended for back-end developers who are *not* authorized to make changes to graphs.
- **Contributor** — management access to the organization’s graphs, but not to members or organization-level configuration. Intended for back-end developers who are authorized to make changes to graphs.
- **Admin** — management access to the entire organization, including its members, graphs, and configuration.


Ther full user permissions table can be found in our[documentation for user roles](https://www.apollographql.com/docs/studio/org/members/) .


Roles are currently only available at the organization level, but we have it in our sights to introduce them to graphs as well, sooner rather than later. We’d appreciate[any and all feedback](https://docs.google.com/forms/d/10C4_ETX5Xb60RfnJjBC2xBVK9gjr6A644AYKA-27T-E/viewform) you may have for us on these five roles and what you need from permissions control in general in Studio 📬. Thanks for coming on this journey with us! We look forward to hearing from you. As always, happy querying.


Written by


Danielle Man


[Read more by Danielle Man](https://www.apollographql.com/blog/author/dman)
