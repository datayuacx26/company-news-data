---
schema_version: "1.0.0"
document_id: "2132fa0d4f85919688582efa46ad880295b3c1f890c363d02454747cee2898e1"
company_key: "yc-unlayer"
company: "Unlayer"
source_id: "yc-unlayer-rss-50f115338a5a"
canonical_url: "https://docs.unlayer.com/changelog/1-455-0-2WMxc71T"
published_at: "2026-07-23T16:00:00+00:00"
first_seen_at: "2026-08-10T04:27:59.075255+00:00"
fetched_at: "2026-08-10T04:28:00.874964+00:00"
content_hash: "sha256:6bce144810931e34d97f7c32ba73eede09c83cf8b815a46613c23dc1d181ca3c"
---

# 1.455.0

The following fixes and features are available on the[stable](https://docs.unlayer.com/builder/latest/version-management#production-environment) channel:


## Features & Improvements


-


Upgrade the editor to React 19. See[examples](https://examples.unlayer.com/custom_tools/react-custom-tool/) .


-


Allow image upload callbacks to abort or return errors.


-


Support versioned runtime assets and` editorVersion` exports.


## Bug Fixes


-


Fix Collaboration mode missing from the Console editor.


-


Fix Heading and Button tools switching back to the old text editor.


-


Fix Timer tool failing to generate GIF when partial default values are passed via init method.


-


Prevent` {{url}}` ,` {{href}}` , and` {{custom}}` merge tags from being removed from link href attributes.


-


Fix Custom Links failing to resolve merge tags.


-


Fix Send Test Mail adding extra vertical spacing to paragraphs.


-


Fix inconsistent line heights in Outlook Desktop.


-


**AI Assistant** :


-


Keep prompt input below content tools.


-


Handle exhausted credits more clearly.


-


Fix malformed AI image editing responses and failover.


-


Additional fixes and improvements.
