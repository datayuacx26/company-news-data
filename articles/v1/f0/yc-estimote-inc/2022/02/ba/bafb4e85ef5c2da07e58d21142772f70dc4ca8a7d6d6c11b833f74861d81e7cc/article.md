---
schema_version: "1.0.0"
document_id: "bafb4e85ef5c2da07e58d21142772f70dc4ca8a7d6d6c11b833f74861d81e7cc"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/676268055242883072"
published_at: "2022-02-15T15:37:55+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:3ab4d68e25302f69b19b43cac2404efee1d734467e5c77b04b1e203e79c0ae2f"
---

# Apple U1 API is finally open and our new UWB Beacons are compatible

Finally, smartphones can be really smart. The little computer in our pockets could connect to a server on the other side of the world, but it didn’t really know much about our surroundings or the context we were in…


That reality changed in 2013 when Apple released iOS7 and opened up the Core Location API. This allowed iPhones to detect the presence of low power Bluetooth beacons. Developers using APIs from Apple could estimate their signal strength and approximate proximity with an accuracy of 1 - 3m. It worked great indoors where GPS signals weren’t available.


Estimote was the first company to[release iBeacon-compatible devices back in 2013](https://href.li/?https://blog.estimote.com/post/57087851702/estimote-beacons-pre-order-launched-shipping-this) . Our iconic Bluetooth beacons and SDKs allowed thousands of developers to enhance their mobile apps with micro-location features.


Now, in August 2021, Apple released their new mobile operating system – iOS15 – and opened the Nearby Interactions API.


It allows iPhones to talk to nearby UWB beacons and compute distance with inch-level precision. In addition to distance, this API can also tell the direction/angle where the beacon is installed relative to the phone. This is a superior upgrade to location technologies embedded into modern phones and will create another wave of innovation.


**UWB BEACONS & IOS SDK**


And here we are again. We are the first company on the market to release UWB beacons by updating our iconic Proximity Beacons with a state-of-the-art UWB radio that is compatible with the Ultra Wideband U1-chip installed in Apple devices.


We are shipping them alongside an iOS Demo Application and an iOS SDK available on GitHub.


**HOW DOES IT WORK?**


Our new UWB Proximity Beacons, in addition to BLE radio, are equipped with a UWB chip and antenna. They run firmware compatible with the specification from FiRa Consortium. It’s the same UWB communication standard Apple implemented in their U1-enabled devices to make them interoperable with other UWB devices. The protocol uses a combination of Bluetooth and UWB to provide precise location/angle. When a mobile app using our SDK or a native Nearby Interactions API wants to get distance estimation, it first uses Bluetooth to detect nearby devices. Once Bluetooth communication is established, both the beacon’s UWB radio and the smartphone’s UWB modules wake up. The devices send radio signals to each other and compute very precise distance estimation. As there are multiple UWB antennas embedded into the phone, software can also determine which angle the UWB signals are coming from.


**WHAT IS THE DIFFERENCE BETWEEN UWB AND BLE?**


Both these wireless communication technologies are low-power and short-range and both are supported by modern smartphones. The main difference when it comes to location technology is the technique used to estimate the distance between the beacon and the phone.


**Bluetooth Low Energy (BLE)**
With Bluetooth, the longer the distance from the beacon, the weaker the signal received by 2.4GHz radio (RSSI) is. Based on that signal strength received by the phone, the mobile SDK can compute approximate distance with 1-5m precision, depending on the venue.


ALT


**Ultra Wideband (UWB)**


With UWB, the distance estimation technique is based on time of flight measurements. UWB Beacon and the phone talk to each other sending 3-6 GHz radio signals back and forth. Both the beacon and the phone measure precise time duration of that back-and-forth communication. Multiplying it by the speed of light (also the speed of radio waves), the SDK can very precisely compute distance estimations with an average accuracy of 10 cm (4 inches).


ALT


**WHAT ARE THE APPLICATIONS?**


One obvious application is the ability for a phone to search for lost items that are in range, but which location we don’t precisely know. Apple showcased this use-case with their FindMy app and AirTags launched in early 2021.


Another application is related to the superior security UWB communication provides. The only way to ensure that you are in front of your own car or apartment is to really be there physically. Because of the time of flight physics, it’s not possible to fake that signal and pretend you are in the car. This prevents “relay attacks” possible with other radio technologies. This “proof-of-presence” technology is already used for keyless car applications.


**GET YOUR UWB DEV KIT TODAY**


Other use-cases depend on you – developers with lots of great ideas about how to make your mobile apps more spatially aware, delivering magical experiences.


Start contributing to the contextual computing era and[buy your UWB Dev Kit with 3 UWB Proximity Beacons for $99 + shipping](https://order.estimote.com/buy/proximity_devkit_with_uwb_2022) directly on our website. Dev Kits are already shipping.


—
This blogpost was updated February 2022
