---
schema_version: "1.0.0"
document_id: "53349c5547b7e7817b456a2ab83b1b7f27f13dccaa0b8ad36072d70221d272ee"
company_key: "yc-unlayer"
company: "Unlayer"
source_id: "yc-unlayer-rss-50f115338a5a"
canonical_url: "https://docs.unlayer.com/changelog/12780"
published_at: "2025-07-28T21:42:24+00:00"
first_seen_at: "2026-08-10T04:27:59.075255+00:00"
fetched_at: "2026-08-10T04:28:00.874964+00:00"
content_hash: "sha256:1660a6116988040741cbf759630611cc8317da44a03aec97d5da2a95225c595b"
---

# 1.278.0

The following fixes and features are available on the **stable** channel:


## Features & Improvements


-


**Header and Footer** : Add header and footer support to multiple editors


-


**Collapsed Property Groups by default** : Allow property groups to be collapsed by default in custom tools ([See docs](https://docs.unlayer.com/builder/tools/custom/create#property-group) )


-


**Link Editor** : Improve input handling for URLs with spaces


-


**Disable Merge Tags Autocomplete:** Ability to disable the Merge Tags Autocomplete Menu Trigger by passing "null" to the config ([See docs](https://docs.unlayer.com/builder/latest/dynamic-content/merge-tags#autocomplete-menu-trigger) )


-


**Blocks** : Improve loading feedback when a block is created, deleted, or updated


-


**Translations:** Add some missing translations


-


**Accessibility** : Apply ARIA labels to exported HTML of some built-in tools


## Bug Fixes


-


**Fix Image Special Characters Support:** Fix support for uploading and searching images with special characters in the filename


-


**Fix Merge Tags Autocomplete:** Fix shortcut "{" not triggering the Merge Tags Autocomplete Menu when the "mergeTags" callback was set


-


**Merge Tags** : Fix merge tags with spaces getting encoded in text links


-


**React flushSync:** Fix Preview modal support for calling React flushSync method inside tool exporters
