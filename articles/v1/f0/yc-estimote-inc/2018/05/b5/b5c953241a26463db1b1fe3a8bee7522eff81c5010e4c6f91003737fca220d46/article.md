---
schema_version: "1.0.0"
document_id: "b5c953241a26463db1b1fe3a8bee7522eff81c5010e4c6f91003737fca220d46"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/174145298605"
published_at: "2018-05-22T13:12:48+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:3aadb9cd755038ced8f7ca19f09d1efff74688af1d0f80cec2f880d0d69ecf19"
---

# Proximity reinvented - launching the next-gen Proximity beacons

We are thrilled to announce our launch of the next generation in our core product - the world’s best Proximity beacons. They are the final element to our proximity stack which we have continued to innovate and completely reinvent over the last two years.


### OS for the physical world


At Estimote, we create an Operating System for the physical world. A software environment where developers can programmatically access the location of people and objects. Then, they can use this data to build next-generation contextual apps for smart spaces such as offices, transport centers, and hospitals.


The key for every OS is reliable user input and a consistent user experience. For computer software, it was the keyboard and mouse position; for phones, it was multi-touch. For real-world software, it’s precise user location.


We spent the last 2 years perfecting just that - reliable enter/exits for a single region represented by a Bluetooth beacon.


When Apple announced Core Location API with its iBeacon implementation, and Google followed up with Nearby API, both were designed for a generic Bluetooth device acting as a beacon.


At Estimote, we took the full-stack approach. We designed our own beacon hardware/firmware as well as the Proximity SDK[iOS](https://github.com/Estimote/iOS-Proximity-SDK) /[Android](https://github.com/Estimote/Android-Proximity-SDK) to be optimized specifically for reliable enter/exit events. We also integrated it with Cloud APIs to make it easy to scale and secure beacon deployments.


### New Proximity Beacons


We launched our original Proximity beacons four years ago. We were the first ever company to ship iBeacon-compatible devices. We didn’t expect them to become the world’s most popular beacons for developers, and power the largest commercial deployments around!


Today, we are announcing the new generation of Proximity Beacons. They come with many hardware and software innovations, and are available to buy right away.


Read below the long list of all the hardware and software improvements.


**Long-lasting batteries**
The new Proximity beacon have twice as much battery capacity. We switched to two AA batteries that last 3 years on default settings, or 2 years for a 100ms iBeacon packet. When configured to advertise every 2000ms, they can last more than 7 years.


**Bluetooth 5 chip**
The new Bluetooth 5 spec is not fully adopted across mobile devices yet, but our beacons are ready for the update. The SoC we use is Bluetooth 5 compliant and can be upgraded over-the-air as soon as major phone manufacturers implement it as part of their Bluetooth stack.


**Planet-friendly enclosure**
New Proximity beacons have the same durable silicone enclosure made from silica (sand), which is non-toxic and 100% recyclable. It’s great for indoor and outdoor use, able to survive temperatures from -30°C to +55°C and is water-resistant (equivalent to IP67).


**Improved adhesive**
Each beacon has nanotechnology smart tape that can be repositioned for the first 40 minutes and will stick forever after that. For unusual surfaces such as an old brick wall or wood, there is a small mounting hole on the back. Simply use a screw or nail on the wall, and the beacon enclosure will fit over it perfectly.


**Other hardware upgrades in the new Proximity Beacons**


- New 3-axis accelerometer supporting flip-to-sleep mode during prototyping
- Light Sensor detecting Bright/Dark environment
- Temperature sensing capability
- RGB-color LED for configuration feedback and AR VLC
- New PCB IFA antenna tuned for S-Band for performance


Each beacon is a little computer with a 32-bit chip. It runs embedded software called firmware. New beacons have the latest version supporting **a bunch of new features** - you can read more below.


**Reliable Proximity packets**
All the beacons we manufacture are configured by default to use a set of our own Bluetooth advertising packets. These custom packets ([Estimote Monitoring](https://community.estimote.com/hc/en-us/articles/360003252832-What-is-Estimote-Monitoring-) schema), alongside[Proximity SDK](https://github.com/Estimote) , are optimized for the best enter/exit reliability.


**Multiple packets support**
Customers who optimise more for interoperability rather than reliability can enable other popular packets such as iBeacon or Eddystone. New Proximity beacons can broadcast all these packets at the same time, if that’s what you want to do. Keep in mind, this will drain a bit more battery.


**Secure Estimote Monitoring**
To[prevent attackers from piggybacking](https://blog.estimote.com/post/172115262320/presence-verification-and-security-is-more-refined) on your beacons and to comply with GDPR regulations, our intelligent firmware rotates and encrypts advertised data. This is patented technology and only provisioned mobile SDKs with Cloud access can decode it.


**Cached Estimote Monitoring**
For scenarios when users’ phones don’t have a constant Internet connection, it is possible for the mobile app to cache enough data to keep monitoring for beacon interactions while being offline for some time.


**Proximity SDK**
In the past, we used to have one giant SDK for both proximity and fleet management. We have decided to separate Proximity SDK and have written it from scratch for iOS and Android.


The new SDK is optimized for the best reliability and ease-of-use. It leverages our experience with hundreds of deployments and, thanks to signal filtering, allows you to more accurately detect the users the moment they interact with your beacon. With the software-defined range, you can set when the event is triggered in code instead of relying on connecting to a beacon and tweaking the broadcasting settings.


**Fleet Management SDK**
We do still innovate with the previous SDK; its main objective now is remote fleet management. Use it for any apps that will authenticate and connect to beacons, change their settings or upgrade the firmware. You can also use this SDK for collecting telemetry data such as battery status.


**App Templates**
To get started with our SDK, simply log in to our Cloud and go to[Apps](https://cloud.estimote.com/#/apps/add) . Once you select the preferred language (Obj-C/Swift or Java/Kotlin) a ready-made app with our SDK, API token, and beacon identifiers will be downloaded as a ZIP file. Just compile it to have your first proximity app.


**Demo Apps**
To experience both SDKs in action, download Estimote app from the Apple App Store or Google Play. Try[Proximity demo](https://blog.estimote.com/post/166210528555/try-our-improved-proximity-experience-in-5-minutes) to experience improved reliability. Under the “Configuration” tab with the radar icon, you can still connect and change beacon settings. All of this can be re-created inside your very own app using our SDKs.


**Development Mode**
All manufactured beacons are automatically assigned to a Cloud account where we store a copy of their configuration. Previously, only the beacon owners (typically the person who purchased them) were allowed to change any settings. New Proximity beacons are shipped in “Development mode” meaning that a team of developers can freely play with them and tweak any settings. However, only the true owner can “transfer their ownership” or secure them for the final deployment.


**Cloud Attachments**
Historically, beacons broadcasted static identifiers. Many of our customers used different database architectures to map these identifiers into contextual data served to nearby users. In order to simplify app development, we have therefore introduced the new[Cloud Attachments API](https://blog.estimote.com/post/168674042150/estimote-cloud-attachments-api-store-context-in) . Each beacon that we store in the cloud can also store a JSON object. The object can be easily accessed via the mobile API that is part of our SDK, so you can keep your tags, location data, descriptions or any other data assigned to beacons there.


You can always shoot us a note tocontact@estimote.com if you have any questions or if you are interested in bulk quantities.
