---
schema_version: "1.0.0"
document_id: "38a463fc157062acb923ec2f79bd933260904bf9586810418b681b456b1ca698"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/media-gets-smarter-replace-files-hard-deletes-and-safer-sharing"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:c9b12352493108c477e6425da6f084f46faadf91c21e8bb4b593caabb9952afc"
---

# Media Gets Smarter: Replace Files, Hard Deletes, and Safer Sharing

Media has always been first-class in Cosmic, but two common workflows deserved an upgrade: replacing a file in place, and actually cleaning up files when you delete them. This update ships both, plus new safety rails for media that is shared across Buckets.


## Replace Media In Place


You can now replace the file behind any media item without breaking anything that already references it.


- **Same URL, same id.** Upload a new file (or save edits from the built-in image editor) and every Object, page, and link that points to this media instantly picks up the new file. No rewrites, no broken references.
- **Works from the editor.** Crop, rotate, resize, or apply any edit from the image editor and choose "Replace original" to save it over the existing file.
- **CDN is flushed for you.** Cosmic automatically purges the CDN caches after a replace, so your site and your viewers see the new file right away instead of a stale copy.


This is the workflow teams have been asking for: fix the image on the live page, not in a new upload with a new URL that you then have to swap into every Object manually.


## A Real Delete, Not Just a Hidden File


Deleting media from the Media Library now does what you would expect it to do.


- **The file is removed from storage.** Not hidden, not orphaned. Gone.
- **CDN caches are purged.** The media URL stops serving the file immediately after deletion.
- **Object references are left to you.** Deleting media does not rewrite Objects or Metafields that still reference the old URL. Cosmic flags this in the delete confirmation so you can clean up references on your terms.


Deleting a whole Bucket now does the same cleanup across every file unique to that Bucket, so spinning down a sandbox or throwaway Bucket leaves no storage behind.


## Safer Sharing Across Buckets


When you import a Bucket, clone from a template, or install a prebuilt app, the underlying media files can be shared with other Buckets. That saves space and keeps everything fast, but it also means a careless edit in one place could overwrite a file someone else depends on.


We added two guardrails:


- **Replace is blocked when you don't have access to every Bucket using the file.** If a media item is shared with another Bucket you can't see, the Replace action is disabled and Cosmic tells you exactly why. You can't accidentally overwrite files owned by another team.
- **Delete is scoped to your Bucket.** If a file is shared with other Buckets, deleting it from yours only removes the reference in your Bucket. The file stays available everywhere else that uses it. The delete confirmation shows you how many other Buckets share the file before you commit.


Both dialogs now also tell you how many shared Buckets you do and do not have access to, so the numbers always line up.


## Try It


Open any Bucket, go to **Media** , click into any file, and look for the new **Replace** action. Save an edit from the image editor with "Replace original" and watch the CDN update in place. Delete a file you no longer need and confirm it is really gone.


These updates are live for every Cosmic user, on every plan. Read the Media documentation for the full reference.
