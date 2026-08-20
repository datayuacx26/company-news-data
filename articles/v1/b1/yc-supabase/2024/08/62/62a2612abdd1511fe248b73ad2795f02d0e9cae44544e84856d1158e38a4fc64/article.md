---
schema_version: "1.0.0"
document_id: "62a2612abdd1511fe248b73ad2795f02d0e9cae44544e84856d1158e38a4fc64"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/platform-access-control"
published_at: "2024-08-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:3cbceb4dadc22c9781e787508bfa29564227a6521f6ba5fb0f04deda5951c972"
---

# Introducing New Platform Access Control

At Supabase, we're constantly striving to provide the tools developers need to build secure, reliable applications. Our latest update focuses on an area that's critical to both security and reliability: Platform Access Control.


We're excited to announce the rollout of our new granular access control features which allows giving users access to specific projects instead of the entire organization.


### Why Platform Access Control matters#


Managing who can access what within your project isn't just a convenience — it's essential for maintaining security and ensuring that your software development lifecycle (SDLC) is followed and availability guarantees are met. While Supabase already provides a robust data security framework through Row-Level Security (RLS), we recognized a gap when it came to managing platform-level access. Our new Platform Access Control feature fills that gap by offering Role-Based Access Control (RBAC) to the Supabase platform and management APIs.


### Granular control at your fingertips#


With Platform Access Control, Supabase now offers a way to manage permissions at the both the organization and project levels.


A user can either have permissions assigned for the whole organization or for specific projects. The roles remain the same as before:


- **Owner:** Full control over everything
- **Administrator:** Similar to the Owner role with some restrictions on update organization settings, transferring projects and modifying owners.
- **Developer:** Cannot modify any settings but has full access to modifying the content like updating the database, uploading storage objects, etc.
- **Read-Only:** Ideal for stakeholders or team members who need visibility into the project without the ability to make changes.


For a more exhaustive list of actions allowed for each role, check out the[access control docs](https://supabase.com/docs/guides/platform/access-control#organization-scoped-roles-vs-project-scoped-roles) .


### Unlocking the full potential of your team#


With these new features, Supabase is making it easier than ever to ensure that every team member has the right level of access. By assigning specific roles, you can reduce the risk of accidental changes, streamline workflows, and maintain a high level of security across your projects. If you're part of a growing team, consider upgrading to an Enterprise Plan to take full advantage of these powerful new tools.


### Get started#


To start using the new Platform Access Control features, check out our updated documentation[here](https://supabase.com/docs/guides/platform/access-control) .
