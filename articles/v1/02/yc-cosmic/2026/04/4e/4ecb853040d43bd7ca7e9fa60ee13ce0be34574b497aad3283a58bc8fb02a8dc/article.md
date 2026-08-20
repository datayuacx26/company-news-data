---
schema_version: "1.0.0"
document_id: "4ecb853040d43bd7ca7e9fa60ee13ce0be34574b497aad3283a58bc8fb02a8dc"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/promote-content-move-objects-between-buckets-in-one-click"
published_at: "2026-04-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:6b0bdf615632b36075dc321e130ce8a2ec0244a15403633c001376a1341ae488"
---

# Promote Content: Move Objects Between Buckets in One Click

Ever needed to push content from staging to production without exporting, importing, and hand-fixing references? Promote Content does exactly that. Select Objects in one Bucket, pick a target Bucket in the same Project, and Cosmic handles the rest: schemas, media, Object references, and revisions included.


---


## What It Does


Promote Content copies selected Objects from one Bucket to another Bucket in the same Project. It is built for real-world content workflows: staging to production, production back to staging for testing, seeding a sandbox Bucket, or cloning campaign content across environments.


- **Entry points:** Objects table bulk action, row three-dot menu, and single Object detail view
- **Four-step wizard:** Target, pre-flight report, options, confirm
- **Idempotent:** re-run a promotion anytime. Cosmic updates the same target Objects and reuses previously copied media.


## Smart Pre-Flight Report


Before anything is written, Cosmic runs a dry-run and shows you exactly what will happen:


- Objects to create, update, or skip in the target
- Object types that will be created or modified, with a per-type breakdown of Metafield changes
- Referenced Objects that match in the target (by type, slug, and locale)
- Referenced media that needs to be copied vs. already present in the target


No surprises. If a schema change is destructive, you will see it before you commit.


## Media That Just Works


Media referenced by promoted Objects is copied into the target Bucket automatically. This includes:


- File and Files Metafields
- Object thumbnails
- Embedded media URLs in rich text, Markdown, and HTML content


Cosmic tracks which media has already been copied, so re-promoting the same content does not create duplicates or re-upload existing files.


## Object Reference Remapping


Object references in your Metafields (single Object, multiple Objects, parent, and repeater fields) are remapped to the matching Objects in the target Bucket, matched by type, slug, and locale. If a referenced Object does not exist in the target, you can either:


- **Cascade:** promote it alongside in the same run, or
- **Leave unresolved:** surface it in the pre-flight report for manual review


## Options You Control


**Conflict strategy** (when a matching Object already exists in the target):


- : overwrite the target Object
- : insert a fresh Object with a unique slug
- : do nothing and flag it in the result


**Schema strategy** (when the target Object type differs from the source):


- : add any missing Metafields to the target schema. Non-destructive.
- : make the target schema exactly match the source. Requires typing to confirm, because this can delete data on existing Objects.
- : block the promotion entirely if schemas differ.


**Status override:** preserve the source status, or force all promoted Objects to or .


**Include referenced media:** toggle media copying on or off.


## Revisions on Every Step


Every created, updated, or cascaded Object gets a new Object revision in the target Bucket. You can always audit what changed and roll back to any prior state.


## Try It Now


Open any Bucket, go to Content, select one or more Objects, and click **Promote** in the bulk action bar. Or use the three-dot menu on any Object row or detail view. Pick your target Bucket, review the pre-flight report, and ship it.


Promote Content is available now on all Buckets. Read the[Buckets documentation](https://www.cosmicjs.com/docs/dashboard/buckets#promote-content) for the full reference.
