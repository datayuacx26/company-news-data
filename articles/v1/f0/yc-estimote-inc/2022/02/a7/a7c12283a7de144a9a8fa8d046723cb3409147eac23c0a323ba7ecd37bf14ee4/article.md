---
schema_version: "1.0.0"
document_id: "a7c12283a7de144a9a8fa8d046723cb3409147eac23c0a323ba7ecd37bf14ee4"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/676268031578636288"
published_at: "2022-02-15T15:37:32+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:0d9cf4434613c94c8db67496135ccd1fd05f93cbb6a819df9e38751d6cfc8962"
---

# Programmable wearable to fight COVID

In 2018, we[launched the Estimote LTE Beacon](https://href.li/?https://blog.estimote.com/post/177348177680/estimote-lte-m-beacon) , the first beacon continuously connected to the cloud and fully programmable in JavaScript. It was designed to scan for assets tagged with BLE beacons and report their presence in the cloud. Some creative customers even adapted it to be a panic button for workplace safety solutions in a slightly modified form-factor of a pebble-shaped wearable.


Then, in 2019, we added UWB features and ML-based IMU technology - both exposed through JavaScript APIs. This updated product became known as an AI beacon.


Nobody could have predicted what would happen next…


At the end of 2019, our Chinese partners began informing us about a new, exponentially spreading virus. A few weeks later, our European customers alerted us that outbreaks occurred in their regions. Then, it finally arrived in the US. Our entire company swiftly moved to remote work and we shifted all our brainpower to work on creatively using IoT technology to fight Covid and its consequences. It was our mission and our duty.


Because the virus was so contagious, anyone who had close contact with an infected person contracted it almost immediately. Instead of Covid-testing everyone on the same floor, we thought a UWB wearable with inch-precise distance measurements could register who the actual potentially infected people were and only they needed to be quarantined. We even added support for vibration and a buzzer to warn employees against congregating at a close distance as defined by the CDC.


That is how the[Estimote Wearable](https://href.li/?https://techcrunch.com/2020/04/02/estimote-launches-wearables-for-workplace-level-contact-tracing-for-covid-19/) was born. It took us 2 weeks to write JavaScript code for social distancing/contact tracing and push it remotely to existing wearables over the LTE. This was the power of a programmable IoT platform. Working directly with customers and following CDC guidelines, we also put together a cloud-based dashboard for contact tracers.


Our customers loved it, and soon we were shipping tens of thousands of wearables to enterprises across the world to help their front-line workers with social distancing and contact tracing in a pre-vaccine pandemic.


The new form-factor was convenient for a wearable use-case. Since it had USB-C, this JavaScript programmable device could also serve as a gateway scanner registering the presence of other BLE or UWB Beacons.


This is why we have decided to sunset the[AI Beacon form-factor](https://href.li/?https://blog.estimote.com/post/636576024254185472/ai-beacon-brings-inch-level-positioning-with-uwb) and replace it with our Programmable Wearable as a platform for all our customers.


Here is the full specification which is onboard and supported by JavaScript API:


- UWB radio with two-way ranging and TDoA
- Bluetooth Low Energy advertising and scanning
- LTE-M/NB-IoT with MQTT support
- IMU (6 axis accelerometer/gyroscope) with Machine Learning
- 8MB Flash Memory with storage API
- Programmable LEDs and button
- Vibration motor and buzzer
- USB-C for charging


If you want to use our Programmable Wearable beyond workplace safety and contact tracing applications please contact our sales team.


—
This blogpost and the story was updated in January 2022.
