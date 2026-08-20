---
schema_version: "1.0.0"
document_id: "6493bfa72112a4cdaefe189eab9c8f87787357a50ad49990b35852dabf64e3b7"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/new-integration-noiseaware"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:e16d4e9bdc0cd705502a8faa9883d1deda1323b5a31f97bb7fdfe7d1ed73df70"
---

# New Integration: NoiseAware

We are releasing our NoiseAware sensor integration. You can now connect NoiseAware sensors to your Seam workspace and dashboard to receive elevated noise-level incidents.


To try it out, check out our[Get Started with NoiseAware](https://docs.seam.co/latest/device-guides/get-started-with-noiseaware-sensors) Guide. You can also


## What is NoiseAware


[NoiseAware](https://noiseaware.com/) makes a noise-level sensor to help short-term rental hosts protect their assets. Their products come in a couple of form factors, mainly to handle placement of sensors both indoors and outdoors.


## Why we Added NoiseAware


Many short-term rental applications using Seam have asked us for a NoiseAware integration. Truth be told, we had already started working on it, and those requests only injected urgency into those efforts.


In terms of use cases, we expect NoiseAware to deliver similar value as our Minut integration. In our Minut release, we wrote the following:


We have been told that while noise sensors successfully pick up noise incidents, the notification process to alert hosts and guests can still be manual. For example, if an Airbnb host received a notification about one of their properties experiencing an incident, they would often have to manually look up the guest’s contact information in their reservation software (“PMS”) to notify them.


Those reservation software applications already have the guest’s contact info and can directly notify the guest via email or text. The missing link is an integration between noise sensors and the reservation software.


NoiseAware already has a handful of direct integration in a few reservation systems. However, there are many PMSs that still lack this support. Since a few of those PMSs use Seam, they will now also be able to easily tap into NoiseAware’s functionality, increasing the number of PMSs that NoiseAware’s customers can use in conjunction with their devices.


Now with Seam, an STR application can now receive noise events from Seam, and automatically notify the relevant parties based on specific conditions such as reservations, time of day, locations, and more.


## How Does it Work


An application can now prompt a user to connect their NoiseAware account just like they do with their[Minut](https://www.minut.com/) account.


Once linked to an application, all the NoiseAware devices will be retrieved. When a noise disturbance is detected, Seam will issue a webhook to the application. The payload of this noise event is standardized across noise sensor brands for ease of integration and handling.


For example, an application like[Hospitable](https://hospitable.com/) will receive a webhook from Seam after a noise event is detected. Hospitable can then look up the guests and notify them about the noise disturbance.


## Getting Started


You can log into the[Seam console](https://console.seam.co/) and start connecting NoiseAware devices today. Note that in sandbox mode, Seam approximates the OAuth flow to Noise Aware. You will see a small banner at the top indicating as such.


## Testing Your Integration


Similarly as some of our other sensor integrations, you can use the trigger feature to let you test your code. High noise threshold will begin appearing in the event stream.


To test against a real device, we suggest invite a lot of friends over to your office, buying a few pizzas, and having a good time to celebrate the launch of your integration 😀


Good luck and ping us atsupport@getseam.com if you have any questions.
