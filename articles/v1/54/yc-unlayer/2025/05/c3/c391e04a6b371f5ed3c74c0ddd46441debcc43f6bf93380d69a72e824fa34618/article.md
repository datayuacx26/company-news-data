---
schema_version: "1.0.0"
document_id: "c391e04a6b371f5ed3c74c0ddd46441debcc43f6bf93380d69a72e824fa34618"
company_key: "yc-unlayer"
company: "Unlayer"
source_id: "yc-unlayer-rss-50f115338a5a"
canonical_url: "https://docs.unlayer.com/changelog/12480-stable"
published_at: "2025-05-07T21:17:45+00:00"
first_seen_at: "2026-08-10T04:27:59.075255+00:00"
fetched_at: "2026-08-10T04:28:00.874964+00:00"
content_hash: "sha256:ebcbeccc90ac48ab60069a5a9fa38cf8540354f19b70cbb02abb0fcde3508c1c"
---

# 1.248.0

The following fixes and features are available on the **stable** channel:


## Features & Improvements


-


**Accessibility Improvements:** Improved accessibility of the editor and the exported html, e.g. by adding a role="presentation" to the table elements and allow passing a custom title ([See docs](https://docs.unlayer.com/builder/1.248.0/accessibility) )


-


**Custom Placeholder URL:** Add ability to change image placeholder URL ([See docs](https://docs.unlayer.com/builder/1.248.0/tools/image#placeholder) )


-


**Big List Performance:** Improve performance of big lists when using Link Types


-


**Sort Link Types:** Add new *sortOptions* config option to the Link Types API ([See docs](https://docs.unlayer.com/builder/1.248.0/link-management/link-types#input-field-parameters) )


-


**Form Font Family:** Add ability to change font family of form labels, buttons and input fields


-


**Small Screens Responsiveness:** Improve editor responsiveness on smaller screens


## Bug Fixes


-


**Link Type onClick:** Fix urls being stripped after double slashes (//) when inside a Link Type onClick handler


-


**Blocks Endpoint Big Payload:** Improve support for bigger responses in the blocks api endpoint


-


**Broken Zip Files:** Fix issue where unlayer releases had corrupted images inside of them


-


**Wrong OnPrem Version:** Fix issue where the editor was trying to load a different version than it should when using the offline bundles (zip files)


-


**Stock Images Aspect Ratio:** Fix wrong aspect ratio in some stock images


-


**Fix Timer RGB Color Issue:** Fix countdown not being generated if a RGB/RGBA color was passed instead of a HEX color


-


**Fix Image Size Increasing:** Fix image editing increasing the size of images


##
