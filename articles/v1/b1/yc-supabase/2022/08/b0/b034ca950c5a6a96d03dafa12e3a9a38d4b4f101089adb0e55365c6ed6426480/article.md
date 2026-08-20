---
schema_version: "1.0.0"
document_id: "b034ca950c5a6a96d03dafa12e3a9a38d4b4f101089adb0e55365c6ed6426480"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/launch-week-5-one-more-thing"
published_at: "2022-08-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:a23c291f05997d8a6e3f9bd9115078aadd9e92d64d8b6324a27cccfc87eef156"
---

# One more thing

Let's be honest, it's never just one more thing. Here are six more things that you need to know this Launch Week.


---


## Supabase Vault#


Today we're announcing[Supabase Vault](https://supabase.com/blog/supabase-vault) , a Postgres extension for managing secrets and encryption inside your database. Vault is a thin usability-layer on top of[pgsodium](https://github.com/michelp/pgsodium) .


Vault is not available on the Supabase Platform yet. This release is intended to gather user feedback.


[Read more](https://supabase.com/blog/supabase-vault) .


---


## Auth UI#


Auth UI was previously released as a single` <Auth/>` component in the[@supabase/ui](http://github.com/supabase/ui) library. We recognized it was one of the most popular aspects of the library and developers were installing the library to use the Auth component alone.


From today, you can install it[from its own NPM package](https://github.com/supabase-community/auth-ui) :


`
_10


npm install @supabase/auth-ui-react


`


We've decoupled this library from our own styling system and you are now able to ( *deep breath...* ): create your own theme, use a pre-existing Supabase theme, extend a theme using token variables, create multiple variations of themes (and then also extend them with variables), use CSS classes and also use any inline CSS styles.


This is all done with[stitches.js](https://stitches.dev/) (or more specifically, the[framework agnostic API](https://stitches.dev/docs/framework-agnostic) ) to handle variants of components and the CSS properties.


A new feature in Auth UI is custom labels. You can now override any of the text labels—very useful for multiple language support.


Our initial release supports React, but we have prepared the repo for other frameworks. This release is a small step towards a larger feature—“ *Hosted Auth Pages* ”. The wide range of customization will become useful for developers to tweak their Hosted Auth pages to reflect their brand personality.


[View the docs](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui) .


---


## Dashboard permissions#


We've released granular permissions for the Supabase Dashboard. Dashboard permissions allow you assign roles to your team and to restrict what they can do.


We're starting with 3 roles—Owner, Administrator, and Developer—which you can edit in the Org settings of the Dashboard. There's a tooltip in the header of the members table, which elaborates on the individual permissions available for each role.


[Read more](https://supabase.com/docs/guides/platform/access-control#manage-team-members) .


---


## JSON schema validation#


We've released[pg_jsonschema](https://github.com/supabase/pg_jsonschema) , a new Postgres extension for the Supabase Platform.


` pg_jsonschema` adds support for[JSON Schema](https://json-schema.org/) validation for` json` and` jsonb` data types, built with Rust. You can read the blog post, which explains some of the scenarios where this is useful.


[Read more](https://supabase.com/blog/pg-jsonschema-a-postgres-extension-for-json-validation) .


---


## pg_graphql v0.4.0#


We've released a new version of` pg_graphql` , our GraphQL extension for Postgres. This release adds support for` json` /` jsonb` types, the` in` filter, and introspection for much larger schemas (10x larger than before). We've fixed a few bugs too.


You can read the[release notes](https://github.com/supabase/pg_graphql/releases/tag/v0.4.0) and[documentation](https://supabase.github.io/pg_graphql/) , and start using` pg_graphql` on Supabase today.


[Read more](https://supabase.com/blog/graphql-now-available) .


---


## Multi-Factor Authentication#


We've opened early-access signups for Multi-Factor Authentication (MFA). To be clear—this is for MFA for[Supabase Auth](https://supabase.com/auth) , which you can use to build additional layers of security in your applications (and not MFA for our Dashboard, which we will also release soon).


If you're interested, sign up and we'll grant your projects early access to the MFA API endpoints.


[Sign up](https://airtable.com/shrjcc9zi0RduHLIx) .
