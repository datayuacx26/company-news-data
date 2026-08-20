---
schema_version: "1.0.0"
document_id: "e241723d3f7e86372d9d009266c663e7d2c2153a8ac926ee11b04ad54ebd34de"
company_key: "yc-unlayer"
company: "Unlayer"
source_id: "yc-unlayer-rss-50f115338a5a"
canonical_url: "https://docs.unlayer.com/changelog/12180-stable"
published_at: "2025-03-06T22:57:13+00:00"
first_seen_at: "2026-08-10T04:27:59.075255+00:00"
fetched_at: "2026-08-10T04:28:00.874964+00:00"
content_hash: "sha256:746f9815c5cebc9c54b5c29c92f70527735632e03c42d24c4452732b78f0de7c"
---

# 1.218.0

The following fixes and features are available on the **stable** channel:


## Features & Improvements


-


**Themes API:** Added API support for setting up custom editor themes beyond custom JS/CSS. You can pass a theme object in the initial config or call the *setTheme* method.[Learn More](https://docs.unlayer.com/builder/1.218.0/appearance/themes#custom-themes) .


-


**Carousel for Web Pages:** Added support for a carousel component in web pages, allowing users to display multiple images or content slides interactively.


-


**Menu Tool onClick Support:** Add support for onClick in the Menu Tool for Web and Popup display modes.


-


**Locked Rows:** Support marking Rows, Columns and Content as Locked, preventing any change to be made on them.[Learn More](https://docs.unlayer.com/builder/1.218.0/templates/permissions) .


## Bug Fixes


-


**$ Replacement Fix:** Fixed an issue where the $ symbol was incorrectly being replaced with "px" when *inlineStyles: true* was enabled.


-


**Custom Font Weight Fix:** Resolved an issue where custom font weights defined in object format were not applied correctly.


-


**Hide on Desktop:** Fixed an issue where the css got broken when *inlineStyle: true* was enabled


-


**Form Tool Missing in Builder** : Fix form tool not showing up in the list of available tools for some users


-


**Inconsistent white spaces:** Improve how white spaces are handled in the Text tool


-


**Countdown Timer Improvements:** Allow you to duplicate a template and edit the new Timer without affecting the Timer from the previous template


-


**Inbox Previews:** Ensure Inbox Previews correctly uses the *previewHtml* callback for rendering.


-


**Safe HTML with Merge Tags:** Fixed an issue where the merge tags moved around in the HTML structure when safeHTML was set to true.


-


**Image Quality Fix:** Fixed an issue where images uploaded to the Unlayer's servers were being displayed with low resolution on MacBooks with Retina Display.


-


**Menu Tool Export Error:** Fix undefined variable error that was happening in a few cases for the Menu tool on email mode, particularly when dropping a saved block or setting the separator option.
