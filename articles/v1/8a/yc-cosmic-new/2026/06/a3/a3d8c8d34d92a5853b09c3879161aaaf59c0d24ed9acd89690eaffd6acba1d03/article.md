---
schema_version: "1.0.0"
document_id: "a3d8c8d34d92a5853b09c3879161aaaf59c0d24ed9acd89690eaffd6acba1d03"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/agents-can-now-manage-content-blocks"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:3c923215dbf1178cf6665fd6d4b93a18701311b3df7c05b97ca794c711ab4dc3"
---

# Agents Can Now Manage Content Blocks

Team Agents can now manage your reusable **Content Blocks** . Until now an Agent could reference a block inside rich-text content with a token, but it could only use blocks you had already created by hand. Now an Agent can define the block too: create a new reusable block, update an existing one, or delete one it no longer needs, all from a normal chat or a scheduled run.


This closes the loop on rich-text content. When an Agent is writing a post and wants a reusable call to action, a disclaimer, or a product card, it can create that block once and reference it everywhere, instead of stopping to ask you to set it up in Bucket settings first.


## What's New


- **Four new content tools.** Agents with the **CMS write** capability get , , , and , alongside their existing object and content-type tools.
- **Create blocks on the fly.** An Agent can author a block as rich text (Markdown), plain text, or raw HTML. The shortcode is derived from the title, just like in the dashboard, so the block immediately works as a token.
- **Discover before referencing.** lets an Agent check which block shortcodes already exist so it references real blocks and avoids creating duplicates.
- **Admin-gated writes.** Editing blocks lives in Bucket settings, so creating, updating, or deleting a block requires bucket-admin access, the same bar a person clears to manage blocks by hand. An Agent can never do something its owner could not.
- **Live progress in chat.** Block actions stream their own status while they run, for example "Creating content block..." and "Deleting content block...", so you can see exactly what the Agent is doing.
- **Instantly live everywhere.** When an Agent saves a block, the change is reflected through the endpoint right away, so your front end renders the new block without a manual cache clear.


## Why This Matters


Reusable blocks are how you keep long-form content consistent: one call to action, one disclaimer, one product card, reused across hundreds of records and restyled from a single component. The piece that was missing was letting an Agent own that workflow end to end. Now an Agent can both define the building blocks and assemble the content with them, so "write the post and add our standard CTA" is a single request instead of a setup step plus a writing step.


## How It Works


1. Open a Team Agent with the **CMS write** capability (team agents have it on by default) and make sure its owner has bucket-admin access.
2. Ask for a block in plain language. The Agent creates it in your Bucket and confirms the shortcode.
3. Reference it from any rich-text field. The Agent drops the token into your content and it renders wherever that content is published.


Example chat:


```text


```


Notes:


- Block writes require bucket-admin access. If an Agent's owner is an editor or developer, the Agent can but will be told it needs admin access to create, update, or delete one.
- Shortcodes are immutable once created, so updating a block keeps every existing reference resolving. Deleting a block means content that referenced it will no longer render it.
- and are reserved for inline Object embeds and cannot be used as block shortcodes.
