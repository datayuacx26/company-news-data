---
schema_version: "1.0.0"
document_id: "7fd237b64dfb324c1d57dd4c1d6c12cd08f247b2fe4b9a8028770b6134f37e53"
company_key: "yc-quindar"
company: "Quindar"
source_id: "yc-quindar-rss-c6f333da4943"
canonical_url: "https://quindar.space/what-is-satellite-command-and-control/"
published_at: "2025-09-10T01:25:00+00:00"
first_seen_at: "2026-08-10T02:21:35.833292+00:00"
fetched_at: "2026-08-10T02:21:38.860446+00:00"
content_hash: "sha256:9ca6b937d1a164b14413ad827755dbec6634acee94278b7143babe525acfb10b"
---

# What is Satellite Command and Control?

Blog


September 9, 2025


nate-hamet


## How to Connect to a Satellite


Connecting to a satellite is analogous to how your computer connects to Wi-Fi to receive information from a server in a data center. Your computer is the satellite and the modem/router is the ground antenna and the server that your computer connects to is mission control. The server may have a new software update for your computer, and this is no different than when mission control needs to update the software on a satellite.


Satellite networks are conceptually similar to Wi-Fi device networks


How do we receive updates on our computer?


1. Connect to Wi-Fi
2. Receive a notification of a new software update
3. Accept and begin the software update
4. If there is a break in the Wi-Fi connection, pick back up where you left off when you are back online


Now let’s sprinkle in the complexities of satellite software updates. Your mobile device is now orbiting the earth at 7.5 km/s (~22x the speed of sound) just like a satellite, your Wi-Fi router antenna is now 4.5 meters wide and can track a satellite in the sky, and you are still connecting to the same server. You are now only able to talk to your computer every 90 minutes for about 10 minutes in duration. That’s cutting it close for fetching your OS update, so maybe you need a few passes to complete the process. Thankfully, you use[Quindar](https://quindar.space/post/optimizing-satellite-contact-schedules#viewer-fhce1) to calculate precisely when your flying computer will be in view of your antenna, and you use our partner antenna network to connect wirelessly. The only configuration you have to make is the number of times you want to talk to your flying computer within a 24-hour period.


## What Network Protocol Does a Satellite Use?


Unfortunately, satellites don’t use Wi-Fi, and without getting into the details of why, let’s just say they need a license to fly (there’s a joke in there somewhere). Plus, a satellites wireless communications standard,[CCSDS](https://public.ccsds.org/) , was created almost two decades before Wi-Fi in 1982. You don’t need a license to use your Wi-Fi, which wasn’t even invented until 1999!


Let’s continue to work backwards in time. When a computer connects to a server, it uses a protocol called[TCP/IP](https://en.wikipedia.org/wiki/Internet_protocol_suite) which was originally developed by the United States Department of Defense back in 1982.[Bob Kahn](https://en.wikipedia.org/wiki/Bob_Kahn) , the godfather of TCP/IP, based its design from his work at[SATNET](https://en.wikipedia.org/wiki/SATNET) – yup, a satellite packet network project that started in the early 1970’s. The typical protocol used in satellite networks, CCSDS, was first adopted around the time of TCP/IP in 1981.


So…


1. Satellites are flying computers 💻


2. The foundation to satellite operations is stuck in the 1980’s 😢


Do you know of a server and modem that speaks CCSDS/IP? Probably not, but there are a handful of commercial products that exist with this capability. They are attached directly to ground antennas. But don’t sweat the technical details, Quindar obfuscates all the network and application integration complexity from users – similar to what your Internet Service Provider (ISP) does in order to connect users to millions of servers worldwide.


## What is Satellite Telemetry?


Do you know how to run a diagnostic check on your computer? Probably not unless you work for the Geek Squad or are just a geek like us. Satellite operators (the Geek Squad for satellites) need to check the diagnostics of a satellite every time they connect. It’s hard to tell a satellite to snap a picture if it’s not running *nominally* . If we made satellite lingo bingo, the word nominal would be the free space – we use it all the time to say a system is working as intended.


Satellites typically transmit their real time and historical state-of-health data precisely when they are in view of an antenna. It is then up to the satellite geek squad to analyze if everything is okay. How do we tell a satellite to transmit health data? We command it, of course!


A satellite’s flight computer, sensors, actuators, and payload generate hundreds of data points a second. Operators will get a sampling of those during a contact from real time telemetry and get the full set when back-orbit telemetry is downloaded. An operations team, with help from its C2 system, will process hundreds of thousands of telemetry points to discern if the satellite’s health is nominal or if there has been an anomaly. Numerical limits and operator-run recovery procedures may suffice for a small number of satellites, but that solution will limit performance as your constellation grows. In a follow-on blog, we’ll show how Quindar is addressing telemetry analysis and satellite recovery at scale.


## How Do You Command a Satellite?


A ground contact schedule is usually uploaded to a satellite once a day. With[Quindar](https://quindar.space/post/optimizing-satellite-contact-schedules#viewer-fhce1) , we calculate the exact moments and duration of time a satellite will be in view of our partner antennae. These are also the precise timestamps during which a satellite will transmit health data to mission control and the operations team will upload a schedule and perform other real time commanding. When all systems are green, the schedule will focus around getting the most out of the satellite payload. If the satellite is off nominal, then all commands will focus on recovery. When in doubt, refer to the flowchart below:


A simplistic flow of high level satellite operations


## Summary


Quindar manages C2 as a service and provides the ability to deploy multiple test environments in parallel.


Tracking, contacting, receiving telemetry, and commanding a satellite can take a company years to build the infrastructure, integrate software, test, and verify nominal operations. Quindar abstracts this all from the user. When onboarding, input details about the data format of your satellite protocol, along with how often you want to connect to your spacecraft. Quindar connects you to your satellite at user configurable times. We also provide a test environment to simulate operations and the ability to connect to satellite test beds, including your lab bench, simulators and flatsats.


‍
