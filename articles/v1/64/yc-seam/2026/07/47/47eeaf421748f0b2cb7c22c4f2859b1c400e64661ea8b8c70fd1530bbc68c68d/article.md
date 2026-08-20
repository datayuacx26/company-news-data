---
schema_version: "1.0.0"
document_id: "47eeaf421748f0b2cb7c22c4f2859b1c400e64661ea8b8c70fd1530bbc68c68d"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/new-integration-2n-intercoms"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:5680fe9b41be12ccf49e416b257c629b1ff66cff47127cb43176a6e135de681f"
---

# New Integration: 2N Intercoms

Today we are announcing the release of our 2N intercom integration. This now lets you connect 2N intercoms and access systems to Seam and use our API + dashboard to control them. You can unlock doors, edit resident directory, and retrieve entry events. Most 2N intercom models come with a built in camera which we will release as part of our broader Universal Camera API later this summer.


## Resources:


- Check out our[Getting Started with 2N Guide](https://docs.seam.co/latest/device-guides/get-started-with-2n-intercoms)
- View the full[list of supported 2N Devices](https://www.seam.co/manufacturers/2n)


## What is 2N


A subsidiary of Axis Communications, 2N manufactures and sells intercom and access control systems. The IP Verso and Solo models are arguably their most popular intercom products, which you’ll encounter at many buildings in San Francisco or New York.


## How Does it Work


Seam’s 2N integration goes through the My2N cloud. You will need credentials to My2N as well as the on-prem unit you are trying to connect with.


To start, create a new Connect Webview and select 2N as the provider. Then, enter your My2N cloud credentials. Seam will then retrieve all your devices. You will then be presented with a list of all these devices. Select the devices you want to connect. Seam will then present a login screen for each project as on-prem device will have its own login credentials.


Once your devices are connected, you’ll be able to unlock the door, edit residents, retrieve events, and eventually tap into the camera feed of the device. Check out our[Getting Started with 2N Guide](https://docs.seam.co/latest/device-guides/get-started-with-2n-intercoms) for more info.


## What’s Next


As mentioned, we’re going to expand this integration over time to include additional features like the ability to tap into the video feed. If you have any specific feature request or are running into issues, please contact us atsupport@getseam.com .
