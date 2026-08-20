---
schema_version: "1.0.0"
document_id: "23280b777b89c9e0cde890ce3ad1cf41d272f52657ba7bfa564c3cf529a3f444"
company_key: "ceva-inc-common-stock"
company: "CEVA Inc."
source_id: "ceva-inc-common-stock-rss-438e9c80ad28"
canonical_url: "https://www.ceva-ip.com/blog/one-key-for-every-door-how-aliro-extends-the-uwb-digital-key-beyond-the-car/"
published_at: "2026-07-20T06:40:43+00:00"
first_seen_at: "2026-07-20T23:21:57.347824+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:7b9bcbd737e9688be8239eeb703b5bf670f0fc5f3144c49f3d705918718d6ce7"
---

# One Key for Every Door: How Aliro Extends the UWB Digital Key Beyond the Car

5 min read


You may already know this feeling. You walk up to your car with your phone still in your pocket, and it unlocks as you reach for the handle. You get in, press start, and drive off without ever touching a key. When you walk away, the doors lock themselves behind you. You have shared that key with your partner, handed a temporary one to the valet last week, and the car already knew it was you approaching: your seat, mirrors, and climate were set before you sat down. \[1\]


That is a modern smartphone digital car key, and the technology behind it is Ultra-Wideband (UWB).


**From the driveway to every doorway**


Now extend that same experience to every other door in your life.


Walk up to your front door and it unlocks. Pull into the driveway and the garage opens. Leave, and everything locks behind you. Share access with family the same way you share a car key, and hand a plumber or a houseguest a credential that only works this afternoon.


The change is even bigger away from home. In offices, hotels, hospitals, and on campuses, the plastic access badge and the hotel room card are overdue for replacement. A phone credential is not only more convenient, it is far easier to manage: security teams can grant a specific person access to specific doors on a specific schedule, revoke it instantly if a phone is lost, and keep a clean record of who entered and when. Better security and better management, from a device everyone already carries.


For any of this to work, though, two things have to be true. The wireless link has to be genuinely secure, and it has to work across different phone brands and different lock brands, not just within one manufacturer’s own ecosystem.


**The foundation: secure ranging in the PHY**


UWB earned its place in access control because of how it measures distance. Instead of guessing proximity from signal strength, which is easy to fake,[UWB](https://www.ceva-ip.com/product/ceva-waves-uwb/) times how long a pulse takes to travel between two devices and derives a precise distance from it.


The IEEE 802.15.4z standard made that measurement trustworthy by building security directly into the physical layer. Its secure ranging cryptographically protects the timing exchange, so the system can confirm that your phone is truly next to the door and not being spoofed from across the street. That is what defeats the relay attacks that have long plagued older keyless-entry systems.


That covers the first requirement: a link you can trust. But a secure radio, on its own, is not an ecosystem. To deliver the second requirement, interoperability at scale, the industry needed specifications built on top of the standard.


**CCC and FiRa: turning UWB into interoperable products**


That is where the consortiums come in. The Car Connectivity Consortium (CCC) took the digital car key vision and turned it into a real, interoperable specification built on UWB, which is why you can already use one phone across many different car brands. We covered that journey in more depth in our earlier piece on[UWB digital keys](https://www.ceva-ip.com/blog/uwb-digital-keys-and-the-quest-for-greater-range/) .


In parallel, the FiRa Consortium drives broad UWB interoperability and opens up new use cases, such as DL-TDoA for seamless indoor navigation, which we explored in our piece on[indoor positioning beyond GNSS](https://www.ceva-ip.com/blog/from-driveway-to-checkout-seamless-indoor-navigation-powered-by-uwb/) . Different scope, same secure UWB foundation.


What was still missing was a standard aimed squarely at doors.


**Enter Aliro**


In February 2026, the Connectivity Standards Alliance (CSA), the same organization behind the Matter smart-home standard, released the Aliro 1.0 specification. \[2\]


Aliro is both a communication protocol and a credential standard for access control. The protocol defines how a phone and a lock talk to each other over the air. The credential standard defines the digital key itself: its format, and how it is issued and verified. Together, they are what let a phone from one manufacturer work with a lock from another, ending the fragmentation that has held mobile keys back across homes, offices, universities, and hospitality venues. It even works in places with no network coverage, such as underground garages and elevators. Security is built on asymmetric cryptography, and the standard supports several transports: NFC for a tap, Bluetooth LE for longer-range interaction, and Bluetooth LE together with UWB for a seamless, hands-free walk-up.


Aliro complements Matter. Both come from the CSA, but they aim at different parts of the problem, Matter for general smart-home connectivity, and Aliro for the digital key itself.


**The wallets are on board**


A standard becomes a market when the phones support it, and Aliro already has commitments from Apple, Google, and Samsung. Apple pioneered UWB-based Home Key years before Aliro existed, \[3\] and both Apple and Google achieved Aliro 1.0 certification immediately following the specification’s release. \[4\]\[5\] Meanwhile, Samsung offers Digital Home Key in Samsung Wallet, unlocking compatible locks by tap or by UWB proximity. \[6\] With these three major wallet providers on board, lock makers finally have the confidence to build at scale.


**Bringing it back to silicon**


All of this has to be implemented somewhere, and that somewhere is the UWB subsystem inside the phone, the lock, the reader, or the wearable.


Ceva-Waves UWB is compliant with both IEEE 802.15.4z and its next generation, IEEE 802.15.4ab, and is aligned with FiRa, CCC Digital Key, and CSA Aliro requirements. \[7\] It includes hardware-accelerated encryption, authentication, and key management, along with robustness features such as strong Wi-Fi interference suppression. Beyond that baseline, Ceva also builds additional proprietary security hardening into the IP, giving product teams extra confidence against real-world attacks, not just minimum certification requirements.


**Why this matters for SoC vendors**


The practical payoff is that none of this has to be engineered twice. Because the same secure UWB layer underpins cars, indoor positioning, and access control, one qualified UWB core can target them all: automotive digital keys and in-vehicle anchors, precise positioning and tracking, and Aliro-ready locks, readers, phones, and wearables.


That interoperability work is already done inside the Ceva-Waves UWB, and SoC vendors that utilize the IP get it out of the box. The core already speaks FiRa, CCC Digital Key, and Aliro, cutting design risk and shortening time to market across both automotive and access-control product lines.


**One key for every door**


So picture it again, only bigger. The same key in your pocket starts your car, opens your front door and your garage, badges you into the office, and unlocks your hotel room. It is shared with your family and handed, for an afternoon, to a guest. One credential, one secure technology, working everywhere you need to go.


**References**


\[1\] Apple Developer, “Explore UWB-based car keys” (WWDC):[https://developer.apple.com/videos/play/wwdc2021/10084/](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdeveloper.apple.com%2Fvideos%2Fplay%2Fwwdc2021%2F10084%2F&data=05%7C02%7CHadas.Amir-Hirshenzon%40ceva-ip.com%7C39296d1ddbe544549be208dee1b98ea5%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639196384271973306%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=LvZmnttBSkEq33lQlOAQQ7gWEGQQEwRaNvXAiZR3CnQ%3D&reserved=0)


\[2\] Connectivity Standards Alliance, “Introducing Aliro 1.0: A Unified Standard to Transform the Access Control Ecosystem”:[https://csa-iot.org/newsroom/introducing-aliro-1-0-a-unified-standard-to-transform-the-access-control-ecosystem/](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fcsa-iot.org%2Fnewsroom%2Fintroducing-aliro-1-0-a-unified-standard-to-transform-the-access-control-ecosystem%2F&data=05%7C02%7CHadas.Amir-Hirshenzon%40ceva-ip.com%7C39296d1ddbe544549be208dee1b98ea5%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639196384271995080%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=3Urlux0h6MwJsziGlpXw%2FpISYjd8TZbB0SXTQ5BPWnA%3D&reserved=0)


\[3\] Apple Newsroom, “iOS 18 makes iPhone more personal, capable, and intelligent than ever”:[https://www.apple.com/newsroom/2024/06/ios-18-makes-iphone-more-personal-capable-and-intelligent-than-ever/](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.apple.com%2Fnewsroom%2F2024%2F06%2Fios-18-makes-iphone-more-personal-capable-and-intelligent-than-ever%2F&data=05%7C02%7CHadas.Amir-Hirshenzon%40ceva-ip.com%7C39296d1ddbe544549be208dee1b98ea5%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639196384272015820%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=6aTbzb%2FBTjeQszJSn%2BmVJPZrEdggjy%2BYoQz%2F%2B9ry1LI%3D&reserved=0)


\[4\] Connectivity Standards Alliance, “Apple’s Platforms” (certified product listing):[https://csa-iot.org/csa_product/apples-platforms/](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fcsa-iot.org%2Fcsa_product%2Fapples-platforms%2F&data=05%7C02%7CHadas.Amir-Hirshenzon%40ceva-ip.com%7C39296d1ddbe544549be208dee1b98ea5%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639196384272043649%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=36AGg7Son9OLInPSNNOjsbSaFXGrVqAhKDulUZY1W6A%3D&reserved=0)


\[5\] Connectivity Standards Alliance, “Google Wallet” (certified product listing):[https://csa-iot.org/csa_product/google-wallet/](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fcsa-iot.org%2Fcsa_product%2Fgoogle-wallet%2F&data=05%7C02%7CHadas.Amir-Hirshenzon%40ceva-ip.com%7C39296d1ddbe544549be208dee1b98ea5%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639196384272074664%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=9U%2F5IKbtM52Ca55I4AP%2FYXD46yp4U2NTFIB9ihcmaHs%3D&reserved=0)


\[6\] Samsung Newsroom, “Samsung Wallet Launches Digital Home Key for Smart Door Locks”:[https://news.samsung.com/global/samsung-wallet-launches-digital-home-key-for-smart-door-locks](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fnews.samsung.com%2Fglobal%2Fsamsung-wallet-launches-digital-home-key-for-smart-door-locks&data=05%7C02%7CHadas.Amir-Hirshenzon%40ceva-ip.com%7C39296d1ddbe544549be208dee1b98ea5%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639196384272095284%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=zZZ6Dj3Yy0%2BLlbrcn5w4fRmC03RIwZunt1HLw5YOtO0%3D&reserved=0)


\[7\] Ceva, “Ceva-Waves UWB”:[https://www.ceva-ip.com/product/ceva-waves-uwb/](https://www.ceva-ip.com/product/ceva-waves-uwb/)
