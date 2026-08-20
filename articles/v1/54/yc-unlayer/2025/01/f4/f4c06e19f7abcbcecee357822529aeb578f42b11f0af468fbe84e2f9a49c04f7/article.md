---
schema_version: "1.0.0"
document_id: "f4c06e19f7abcbcecee357822529aeb578f42b11f0af468fbe84e2f9a49c04f7"
company_key: "yc-unlayer"
company: "Unlayer"
source_id: "yc-unlayer-rss-50f115338a5a"
canonical_url: "https://docs.unlayer.com/changelog/11900"
published_at: "2025-01-22T20:01:19+00:00"
first_seen_at: "2026-08-10T04:27:59.075255+00:00"
fetched_at: "2026-08-10T04:28:00.874964+00:00"
content_hash: "sha256:7e97d83356778f2fe7d7a5f1c72b84647028e769569454beaf9cbc0abc5abd59"
---

# 1.190.2

## Features & Improvements


-


**YouTube Live Thumbnail Support:** Added the ability for the Video tool to generate thumbnails from YouTube /live video URLs.


-


**Link Label in JSON:** The link editor now saves the label in addition to the value in the JSON, making sure the human readable label will always be available to be shown in the UI.


-


**Image Callback with ID Support:** The image callbacks now better support custom string IDs provided by the user.


## Bug Fixes


-


**Crash on Empty URL:** Fixed an issue where the Link Editor would crash if the href field was null.


-


**Merge Tag Preview Fix:** Improved merge tag previews in the document builder for accuracy.


-


**Mobile View Rendering Fix:** Stopped the builder from incorrectly switching to mobile view when resizing the browser window.


-


**Link Modal Text Overflow:** Fixed text overflow issues in the link modal.


-


**Outlook Rendering Fix:** Resolved rendering issues on Outlook mobile when minification is enabled for designs.


-


**Improved Styling Issues:** Resolved issues with the appearance and user interface of the comments section and inbox preview.


-


**Increased Text Spacing in Gmail:** Fixed excessive text spacing in Gmail for consistent typography.


-


**Text Direction in Heading & Button Tools:** Fixed text direction not applying in Heading and Button tools.


-


**Text Toolbar Overlapping Content:** Fixed toolbar blocking top lines of text while editing.


-


**RTL Text Rendering in Emails:** Fixed RTL text formatting issues in sent emails.


-


**Merge Tag Styling Fix:** Fixed an issue where text color and background color could not be changed for merge tags.


-


**Image Size in Outlook Mobile:** Fixed an issue where images appeared significantly larger than intended in Outlook mobile emails.


-


**Template Colors Update Fix:** Fixed an issue where outdated colors continued to appear in the template color selection after changes were made.


-


**Export Error Fix for Web Templates:** Resolved an issue where exporting HTML failed for certain web templates.
