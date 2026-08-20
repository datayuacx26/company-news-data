---
schema_version: "1.0.0"
document_id: "7407131f915c98023c03c22ea41e2a734f0bd3553a5f505954fe51e4f57b08f3"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/169263723575"
published_at: "2018-01-04T14:00:00+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:34f4d41f21b788f07b61d6f9a0d680fb9368166e35fcd22d8d64feb944d5063f"
---

# The most reliable Proximity SDK is now available for Raspberry Pi using Android Things

Recently, we announced our Proximity SDK, which offers both a reliable beacon-based proximity experience and an easy-to-use API. Proximity SDK can be used for both iOS and Android devices, however for some[very interesting use cases](http://blog.estimote.com/post/166963330230/nasas-johnson-space-center-transcends-space-and) this is not enough. We’ve found out that you want to build exciting real world software applications using low-cost computers, just like Raspberry Pi. In this great example of Internet of Things communication, central nodes (e.g. Raspberry Pi) gather data from low power nodes (beacons) about their Proximity (and build simple object detection mechanisms — a basic asset tracking solution) or Telemetry information (using beacons’ built-in sensors). You used to build those use-cases using generic BLE scanning libraries. With the introduction and increasing popularity of AT, this changes. We’re happy to announce that our latest and most robust Proximity SDK is now officially supported on embedded platforms.


Yes - this means that you can now efficiently scan for Estimote Beacons on Raspberry Pi 3.0, or NXP i.MX7D.


**Why is this new solution better?**


First of all, you get the most reliable Proximity SDK with a great API. We are currently fully supporting this SDK as a part of the general Proximity Android SDK — you can expect all reliability improvements and new features of our stack to be fully covered on Raspberry Pi. Proximity SDK makes it simple to build beacon-based proximity experiences, and helps you get started really fast.


Secondly, you no longer need embedded development skills to create new beacon-based apps for the IoT. You can use standard Android developments tools, programming languages (Kotlin, Java) and toolchain (Android Studio) to write new apps and install them on your development board.


**How does it work?**


The core of this solution is Android Things. Android Things is an operating system for embedded development platforms. The most popular among several development boards certified for Android Things is Raspberry Pi 3 — a low-cost, small, single-board computer equipped with a Bluetooth Low Energy chip. Using the Estimote Proximity SDK API for Android (Things) you are able to create an application in Android Studio. Your application is scanning for beacons using signals transmitted from them either to detect proximity or read sensor data. You can later use that extracted real world information to build a complete Internet of Things project.


**What do you need to do?**


To use it, all you need to do is install Android Things on your[development board](https://developer.android.com/things/hardware/developer-kits.html) and run our[Proximity SDK Android Things sample app](https://github.com/Estimote/Android-Proximity-SDK/tree/master/example/AndroidThingsApp) on it. To help you get started really fast, we have prepared[a detailed tutorial](http://developer.estimote.com/sensors/android-things/) (some really handy shortcuts included :)).


Share your thoughts on our[forums](https://forums.estimote.com/) ordrop us an email
