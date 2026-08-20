---
schema_version: "1.0.0"
document_id: "d5c34fea96d5593d6bcbf77e0363676115d497b410ad7238abfd781982fb271c"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/636576024254185472"
published_at: "2019-08-06T11:50:04+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:565c5212dacd3cad2797add6695c5405558e09be5a71f0d139949585da51fe44"
---

# AI Beacon brings inch-level positioning with UWB radio and inertial module you can teach

In September 2018[we announced our LTE Beacon](https://blog.estimote.com/post/177348177680/estimote-lte-m-beacon) . It was the first Bluetooth beacon with low-power cellular connectivity, fully programmable in JavaScript.


It quickly became an amazing platform for rapid prototyping of IoT apps. Our customers programmed it to act as Bluetooth gateways that can scan for other beacons, as GPS trackers, as panic buttons, and more.


With embedded Bluetooth 5 and GPS it enabled both indoor and outdoor vehicle or asset tracking applications, with an average accuracy of a few meters.


## New beacon and inch-level tracking with UWB


Today we are pleased to announce a new version of our programmable beacon.
It has an additional Ultra Wideband radio (UWB). It allows sub-10-cm location accuracy between two UWB-enabled devices using time-of-flight technology.


To push the positioning precision even further, we have added an intelligent Inertial Measurement Unit (IMU). It has an on-chip Machine Learning Core, which makes the beacon teachable. That’s why we call our new beacon the AI Beacon.


## The AI Beacon and gesture monitoring with an intelligent IMU


In addition to a 3-axis accelerometer and a 3-axis gyro, the inertial module has Machine Learning Processor baked into the sensor itself. It allows the beacon to be trained to count steps or detect specific motion gestures such as bump, free fall, pick up, tilt or any other gestures fueled by inertia measurements.


The IMU is always turned on and consumes very little power while the other components are turned off. Once a gesture is detected, the AI Beacon wakes up the other components and performs pre-programmed actions—e.g., sends an alert over LTE or NB-IoT. This event-based approach can massively reduce power consumption of the entire beacon. Power consuming radios such as GPS, cellular or UWB can be activated only when a specific gesture is detected.


## The full specification of Estimote AI Beacon


The new AI Beacon from Estimote is packed with state of the art IoT technologies.
It has all the key wireless chips and sensors you need to build the next generation apps for the physical world.


##


## Key technologies available on AI Beacon:


- LTE-M / NB-IoT / EGPRS modem
- Global cellular connectivity, active out of the box
- GPS / Galileo / GLONASS / BeiDou / QZSS GNSS
- Bluetooth 5 radio
- UWB radio
- 64 MHz ARM ® Cortex™ M4 CPU with FPU
- 16 Mb flash memory
- 3-axis MEMS accelerometer
- 3-axis MEMS gyroscope
- Programmable button
- Programmable RGB LEDs
- Rechargeable lithium-ion battery
- USB-C


## Indoor Location using UWB positioning


For outdoor tracking the AI Beacon can use GPS signals or cell tower triangulation.
For indoor location it can use nearby Bluetooth beacons for precision down to a few meters, or UWB beacons for inch-level precision.


The UWB radio will support two different modes. In the first one, at least three AI Beacons should be connected to power using USB-C and installed as anchors in the room corners. The fourth AI Beacon can be mobile and attached to a vehicle, e.g., a forklift. It will keep ranging nearby UWB anchors and, based on received signals, will compute its own position in real-time. This precise position can be sent to the cloud using LTE, or processed by the micro-app installed on the AI Beacon.


In this scenario both anchors and the vehicle beacon should be connected to power using USB-C, since the UWB radio will be consuming a significant amount of power.


There is also another scenario where at least four UWB anchors are installed in the corners and powered with USB-C. This time, the mobile AI Beacon can be programmed to act as a low-power UWB Tag. It can be attached to an asset and will keep sending tiny UWB signals. Powered UWB anchors on the walls nearby will pick up the signals and relay them to our cloud to compute the precise position of the tag. Then, you can subscribe to receive real-time location updates from our cloud API.


In this scenario, UWB Tags can last years on a single charge. This setup is perfect for high-precision asset tracking, e.g. pallets or animals.


## Programming the AI Beacon with Web IDE and JavaScript


Just like the LTE Beacon, the AI Beacon is programmable in JavaScript using[Estimote Web IDE](http://cloud.estimote.com/) . All you need is an internet browser with Web Bluetooth support. The beacon exposes a large collection of JavaScript functions (an API) to control all the radios and sensors. When it is time to deploy, Estimote Cloud will compile the code into a binary, and flash it to the beacon over Bluetooth or cellular connection.


The new AI Beacon features, UWB positioning and smart IMU, are programmable in the very same way, with UWB and IMU functions joining the beacon’s JavaScript API.


If you want to see the process of programming the beacon in action, watch our 4-minute video-tutorial. In the video, we build an app that sends a text message with Twilio whenever the button on the beacon gets pressed:


[https://www.youtube.com/embed/UvTvzO5YstQ](https://www.youtube.com/embed/UvTvzO5YstQ)


## IoT app templates


With all the radios and sensors included in the AI Beacon, and its rich JavaScript API, there is a lot to explore. To make this easier, we have built many IoT app templates to show _what_ you can do with the beacon, and also _how_ to do it.


They range from simple code snippets to help learn the basics, to more complete examples with beacon (micro-app) code, backend (cloud) code, and integrations with 3rd-party APIs.


The templates are well-commented, explaining the APIs and functions they use, and recommending some best-practices. They are also designed to be easily tweakable, so that you can adapt them to your needs, or even mix-and-match code snippets from multiple templates.


## IoT prototyping platform and Estimote Software


We believe the new AI Beacon from Estimote with its rich wireless technologies will be an outstanding tool for rapid prototyping of different Internet of Things apps.


If you want us to help with your idea, we are also more than happy to allocate our engineers and engage in Proof-of-Concept/Pilot projects—contact our Customer Success team to learn more about PoC packages.


Once you are ready with your prototype, we can also help with volume manufacturing and custom form-factor products that use the same software stack, but are optimized for specific use-cases.


Contact our Customer Success Team by emailing business@estimote.com.
