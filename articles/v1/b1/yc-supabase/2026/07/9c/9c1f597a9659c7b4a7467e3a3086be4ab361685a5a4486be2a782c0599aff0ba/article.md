---
schema_version: "1.0.0"
document_id: "9c1f597a9659c7b4a7467e3a3086be4ab361685a5a4486be2a782c0599aff0ba"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/47812-deprecation-notice-supabase-supabase-js-will-require-typescript-5-0"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:0999cf7f587543bb77b23c5d10472ca6c30a0b4886cf8afb720f130466dae735"
---

# Deprecation notice: @supabase/supabase-js will require TypeScript 5.0+

Starting with a **minor release on or after January 31, 2027** ,` @supabase/supabase-js` and the packages it bundles (` postgrest-js` ,` auth-js` ,` realtime-js` ,` storage-js` ,` functions-js` ) will require **TypeScript 5.0 or later** . TypeScript 4.7–4.9 will no longer be tested or supported.


Consistent with our existing[Support Policy](https://github.com/supabase/supabase-js#support-policy) , raising the minimum TypeScript version ships in a **minor release and is not considered a breaking change** , the same way we handle end-of-life Node.js versions.


## Why#


- Our type declarations currently target a TypeScript **4.7** floor (May 2022, ~4 years old) — roughly a dozen releases and two major lines behind current TypeScript.
- Holding the 4.7 floor blocks us from using modern type features that materially improve the SDK's types —` const` type parameters, the` satisfies` operator, and (later)` NoInfer` for the postgrest-js query-builder generics.
- It also pins transitive tooling (e.g.` zod` ) to older releases.


## What you need to do#


- **On TypeScript 5.0 or newer:** nothing. You're already covered.
- **On TypeScript 4.7–4.9:** upgrade your project's TypeScript to` >= 5.0` before the release above. TypeScript 5.x has been stable since March 2023.


## Timeline#


- **Now:** advance notice (this post); our SDKs continues to emit 4.7-compatible types.
- **January 31, 2027 (sunset date):** TypeScript 4.7–4.9 support ends. A minor release on or after this date raises the floor to TypeScript 5.0. The last version supporting TypeScript 4.7 will be called out in the release notes and the README once that release is cut.


Questions and concerns welcome below.
