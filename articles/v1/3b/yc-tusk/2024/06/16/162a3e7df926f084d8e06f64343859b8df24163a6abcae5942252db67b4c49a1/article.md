---
schema_version: "1.0.0"
document_id: "162a3e7df926f084d8e06f64343859b8df24163a6abcae5942252db67b4c49a1"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/introducing-our-figma-integration"
published_at: "2024-06-27T15:32:14+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:7b3a9d613b4bf9e95e751ab734b69daf82777f9ab135ee92d4bad3e1bc70d2ee"
---

# Introducing our Figma Integration

# Tusk's Figma Integration


I'm not at Figma Config but I *am* in Figma configuring things around. Excited to announce that we've released Tusk's much-awaited Figma integration 🎨✨


Your browser does not support the video tag.


## How It Works


Most cross-functional product squads today create frontend tickets with a link to their Figma design. We've heard our customers say that they want Tusk to reference their mock-ups when generating code without having to copy-and-paste images into their ticket descriptions.


Add a Figma link to your ticket and Tusk will use your design mock-ups as context to figure out which existing component to use/modify. If the agent can't find an existing component in the repo, it will generate a custom component.


## Getting Started


1. Connect your Figma account to Tusk via the Settings page in our web app
2. While creating an issue, copy the link from a Figma frame to your clipboard
3. Paste the Figma link into your issue description
4. Add the “Tusk” label to the issue
5. Tusk will automatically extract the image context from the Figma frame when reasoning
