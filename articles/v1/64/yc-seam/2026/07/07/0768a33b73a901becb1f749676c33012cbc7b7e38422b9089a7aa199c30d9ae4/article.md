---
schema_version: "1.0.0"
document_id: "0768a33b73a901becb1f749676c33012cbc7b7e38422b9089a7aa199c30d9ae4"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/wyze-integration"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:9e10ca8dab00802546a201917d3707845d902a78dc8c1f1de6daf635d376fee2"
---

# Wyze Integration

Seam is excited to release our official[Wyze integration](https://www.seam.co/manufacturers/wyze) ! You can now connect and control Wyze locks to create access codes for temporary or permanent access.


Resources:


- Check out our[Wyze API Documentation](https://docs.seam.co/latest/device-guides/wyze-locks)
- View the full list of supported[Wyze Locks](https://www.seam.co/manufacturers/wyze)


## About Wyze


Based in Seattle, WA, Wyze is an up and coming smart home brands with many device types such as cameras, thermostats, and of course, smart locks. Their team produced a really[fun & interesting video](https://www.youtube.com/watch?v=64syJcn9Ias) on what it took to build their locks. We encourage you to check out[their entire Youtube channel](https://www.youtube.com/@wyze/videos) for some insights into what it takes to ship consumer hardware at scale.


## Connecting a Wyze Device to Seam


To connect a device to Seam, you will first need to generate an API key in the Wyze online portal. Once you have this key, use our connect webview flow to import it into Seam, along with your account credentials. Once completed, your devices should appear in your Seam workspace. Note that the Wyze Lock Bolt is not supported as it is a bluetooth only device.


## Getting Started


Go to the Seam Developer Console and click on “+ Add Device”, select Wyze, and follow the onboarding steps. Once connected, make sure to check out our` locks` and` access_code` capability API documentation for controlling Wyze devices.


If you have any questions, ping us atsupport@getseam.com
