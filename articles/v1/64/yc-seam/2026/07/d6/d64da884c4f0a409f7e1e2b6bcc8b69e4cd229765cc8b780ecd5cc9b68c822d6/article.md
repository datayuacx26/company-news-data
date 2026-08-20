---
schema_version: "1.0.0"
document_id: "d64da884c4f0a409f7e1e2b6bcc8b69e4cd229765cc8b780ecd5cc9b68c822d6"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/new-integration-kwikset-smart-locks"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:25b30673ff09e991dcbb839e81c6624594b92ed609c5d01b327c9347e867897b"
---

# New Integration: Kwikset Smart Locks

We are excited to announce the official release of our Kwikset Halo locks integration. This integration allows you to connect any Kwikset Halo smart lock and use the Seam API and Dashboard to control the devices, create access codes to let people in, unlock the door, and receive events.


**Seam-Kwikset Resources:**


- Check out our[Getting Started with Kwikset Guide](https://docs.seam.co/latest/device-guides/get-started-with-kwikset-wifi-locks)
- View the[full list of supported Kwikset devices](https://www.seam.co/manufacturers/kwikset)


## What is Kwikset Halo


Kwikset is one of the most popular brands of smart locks available, and Seam already supports many Kwikset Z-Wave and Zigbee devices through its integrations with various hubs. However, the Kwikset Halo line of devices was introduced more recently and does not require a third-party hub. Instead, it connects directly to Wi-Fi and was not supported by Seam until today.


## How Does it Work


As Kwikset Halo devices have grown in popularity, we've received numerous requests to add support for these devices. Seam now officially supports Kwikset Halo.


This integration supports both lock/unlock operations as well as access codes. You will also be able to retrieve battery levels and entry events. Note that there are a few edge cases regarding MFA and code naming due to some limitations of the Halo systems, which we've flagged for you in the[getting started guide](https://docs.seam.co/latest/) .


To connect a device, you can create a Connect Webview by requesting Kwikset as the provider. Note that this is different from Z-Wave Kwikset devices, where you'd set the provider as the Z-Wave hub brand you use to connect the device.


Once connected, your application will be able to retrieve all Kwikset devices tied to the account and begin performing actions against them.


## What's Next


We're going to continue expanding the feature set covered by this integration. We will likely also contribute it back into Home Assistant and other platforms since they currently lack some of the integration features provided by Seam. If you have any questions, feel free to contact us atsupport@getseam.com
