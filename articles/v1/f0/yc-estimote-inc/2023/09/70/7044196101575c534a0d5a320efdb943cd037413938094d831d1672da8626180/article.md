---
schema_version: "1.0.0"
document_id: "7044196101575c534a0d5a320efdb943cd037413938094d831d1672da8626180"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/728317898359259136"
published_at: "2023-09-13T04:07:53+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:0ecf9f7ccac6caab62e45dea81868b6fbc7f26d571148e0ddf75d71f08c0b08d"
---

# Why Apple&rsquo;s 2nd Gen UWB Chip is Exciting?

With over 10 years of experience in spatial and location technologies, including Bluetooth, Ultra Wideband, and Cellular IoT, we at Estimote are thrilled about the newly announced iPhone 15 and its upgraded second-generation Ultra Wideband chip.


In this blog post, we’ll explain why we’re excited, we will try to forecast the potential use-cases of this second-gen chip, and highlight the opportunities for developers to create groundbreaking context and location-aware apps.


**SPATIAL AWARENESS**


Many people aren’t aware, but every iPhone from the iPhone 11 onwards has a hidden super-power. It can measure the distance and orientation to nearby phones or other compatible devices. Think of it as an invisible string connecting your phone to objects with AirTags attached, a HomePod, or even your car.


This super-power is made possible by the first generation Ultra Wideband chip (U1 chip). This chip is a small radio transmitter that sends, receives, and processes tiny radio signals. These signals are so low-power that they’re almost indistinguishable from the background noise in a broad range of frequencies (5-9 GHz). That’s why it’s called Ultra Wideband, or UWB for short.


Whenever a nearby device communicates with an iPhone using UWB, the phone calculates the duration of this interaction. The greater the distance, the more time it takes for the radio signal to make the round trip. The U1 chip then multiplies this travel time by the speed of light to determine the inch-level distance to nearby phones or tags.


iPhones equipped with the UWB chip use this distance data to create magical user experiences. For instance, when you AirDrop files to friends close by or search for your misplaced keys, your iPhone directs you to the corresponding AirTag. And when you walk into your living room your music can seamlessly start playing on your HomePod. These are just a few examples of what the U1 chip offers, and there’s so much more on the horizon.


**LOWER POWER UWB CHIP**


The new iPhone 15 as well as new Apple Watch Series 9 feature a second-generation UWB chip. Reports indicate that a primary difference with that model is the shift in the chip manufacturing process from 16nm to 7nm. This simply means that the transistors have become smaller. As a result, they use less energy and switch more quickly, leading to enhanced performance, greater range, and new possibilities.


Why is power consumption so crucial? Reduced power usage can result in a superior user experience and introduce entirely new use-cases and applications. For instance, with the new UWB chip, new AirTags when upgraded could last up to 2-3 years on a single coin battery, as opposed to the current 1-year lifespan with the U1 chip.


Having more transistors also means stronger computing power, better signal processing, and more effective noise filtering. This is why Apple also announced a 3x improvement in UWB range and unveiled a new precision finding feature for the Find My app to locate friends around.


**UNTRACKED INDOOR LOCATION**


Another potential future application of the lower power UWB chip might involve a technique named Downlink TDoA. This stands for “time difference of arrival,” a new standard championed by the FiRa organization. It lets UWB-enabled phones passively receive signals from UWB beacons or anchors.


Imagine your car in a tunnel where GPS is unavailable. UWB beacons in the tunnel transmit radio signals to your phone. Your phone captures these signals, and using the time difference of their arrival, it can calculate the exact position of the car inside the tunnel. With Downlink TDoA, beacons don’t collect any data from to the phone. The precise location is determined solely by the UWB chip on the phone. This approach is seen as safer and more privacy-centric than traditional real-time location systems (RTLS). As a result, it’s often referred to as Untracked Indoor Positioning.


This innovative, privacy-centric way of determining indoor positioning could also be applied in airports, malls, museums, and more. It could effectively deliver what the iBeacon promised years ago, but with inch-perfect accuracy and superior security.


To make such experiences possible, a low-power UWB processor like the second-gen chip is essential. The phone would need to constantly detect UWB signals from nearby beacons, and it’s crucial that this happens efficiently.


**SPATIAL COMPUTING AND VISION PRO**


There’s also speculation that the UWB chip may eventually interact with Apple’s new smart glasses. This makes a lot of sense. Currently, Vision Pro determines its spatial orientation in a room using computer vision and image processing. Using the UWB chip and radio signals for this purpose would use order of magnitude less power. Preserving power could allow Apple to design more compact and lighter smart glasses in the future.


**FUTURE APPLICATIONS OF UWB**


Ultra Wideband technology has many more uses than just distance positioning. It was originally developed for military use in radars. Essentially, UWB signals sent from the phone could reflect off nearby objects and return to the UWB chip. By examining these reflections phone can identify the shape, orientation, or movement of objects.


**RADAR AND HEALTH MONITORING**


There are many research papers where UWB radar is used to count people in a room or even detect human heartbeats. Given Apple’s focus on health & fitness, it’s entirely possible for future iPhones equipped with UWB on your nightstand to monitor heart rate or breathing. Non-contact monitoring could be useful for elderly individuals living alone or young children. It could identify falls or irregular heartbeats and notify caregivers or family members. This might not be implemented on the second-gen processors, but could be expected from the future UWB revisions and the new software.


**HANDS-FREE PAYMENTS**


Another potential use for an improved UWB chip could be in payments. While NFC has been popular for close-range payments, UWB could transform the entire experience. Imagine approaching a payment terminal, and without needing to pull out your phone or move it near the terminal, the payment is processed securely. This technology might lead to genuinely hands-free shopping. Customers could just grab items and leave the store. With devices powered by the UWB chip communicating with other UWB-enabled tags and payment terminals, the items someone has would be detected, and the cost automatically taken from the associated account, all while ensuring security through spatial authentication.


Beyond shopping, UWB could also make peer-to-peer payments easier. Instead of sifting through apps, just being nearby could prompt a payment screen on your iPhone, asking if you want to split the bill, recognizing your friend’s device by its spatial position.


**ACCESS CONTROL AND HANDS-FREE AUTHENTICATION**


Finally, Apple’s new second-gen UWB chip, can revolutionise the way we access and interact with spaces. Imagine arriving at a hotel. As you approach your room, the door recognises your smartphone and automatically unlocks without you having to fumble for a keycard or even touch the door.


Workplaces, especially those that involve machinery or secure data access, can benefit from hands-free authentication. Consider a research laboratory with multiple machines and computers. Instead of using passwords, keycards, or fingerprint scans, devices equipped with UWB sensors could detect the presence of an authorised UWB chip-carrying employee and unlock automatically.


The incorporation of the second generation UWB chip into a broader range of devices could dramatically transform the way we interact with the world, making our experiences smoother, more intuitive, and more secure. From hotels to workplaces, the possibilities for hands-free access and seamless interactions are vast and exciting.


If you are interested to learn more about UWB and differences between BLE and UWB see our[other blog-post about our UWB Beacons](https://href.li/?https://blog.estimote.com/post/676268055242883072/apple-opens-u1-api-and-supports-our-uwb-beacons) and try Nearby Interactions API and our SDK building next-gen mobile apps.
