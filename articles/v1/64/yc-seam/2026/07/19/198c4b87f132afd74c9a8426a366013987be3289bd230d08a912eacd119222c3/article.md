---
schema_version: "1.0.0"
document_id: "198c4b87f132afd74c9a8426a366013987be3289bd230d08a912eacd119222c3"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/new-integration-ttlocks"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:457de041e30b8e3f5172869a438aadb0223fca63d29f30d616295155f02341cc"
---

# New Integration: TTLocks

The Seam TTLock integration is now live. This integration lets you connect any TTLock smart lock and use the Seam API and Dashboard to control these devices. Create access codes, remotely unlock them, read battery levels, and more.


- Check out our[Getting Started with TTLock Guide](https://docs.seam.co/latest/device-guides/get-started-with-ttlock-devices)
- View the[full list of supported TTLock devices](https://www.seam.co/manufacturers/ttlock)


## What is TTLock?


Based out of Hangzhou, China, TTLock has historically been a supplier of electronics for the smart lock industry, offering both PCBA and white-labeled solutions. In more recent years, TTLock has begun selling its own brand of door locks and provides a companion iPhone and Android app. These locks are often sold under various localized brands, but the underlying hardware is the same.


Due to its affordable price point, TTLock devices are currently gaining in popularity, especially in emerging markets where affordability is important. TTLock retails some of its smart lock devices for as low as $30-40 per unit. TTLock devices also come in a large variety of form factors, making them ideal for a diverse set of doors and built environments.


## How Does it Work?


As TTLock devices have gained popularity and the Seam user base has expanded rapidly outside the United States, we've received many support messages asking for TTLock support. As of today, Seam now officially supports TTLock devices.


Our TTLock integration supports both lock/unlock functions as well as access code programming when available on the device. Note that remote unlock must first be enabled in the TTLock app before Seam is able to call out this function. You will also be able to retrieve battery level and entry events for most devices.


To connect a TTLock account, create a Connect Webview and request TTLock as the auth provider. Once connected, your app will be able to retrieve all TTLock devices tied to the account and begin performing actions against them.


## What's Next?


The universe of TTLock devices is broad and rich in form factors. Our EE-lab testing will continue to focus on testing each device and ensure that our sandbox virtual device replicas are consistent with the real device behavior. Next, we are also monitoring the reliability of TTLock device communication and will invest in additional logic to ensure a reliable API experience.


If you have any questions, ping us atsupport@getseam.com
