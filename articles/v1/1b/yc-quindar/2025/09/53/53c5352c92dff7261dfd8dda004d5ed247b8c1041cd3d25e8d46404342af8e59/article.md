---
schema_version: "1.0.0"
document_id: "53c5352c92dff7261dfd8dda004d5ed247b8c1041cd3d25e8d46404342af8e59"
company_key: "yc-quindar"
company: "Quindar"
source_id: "yc-quindar-rss-c6f333da4943"
canonical_url: "https://quindar.space/what-is-satellite-mission-management-mission-operations-explained/"
published_at: "2025-09-10T01:25:00+00:00"
first_seen_at: "2026-08-10T02:21:35.833292+00:00"
fetched_at: "2026-08-10T02:21:38.860446+00:00"
content_hash: "sha256:8fac4d7903ddaf9c02a7ee2dbca76725d6bdf58ec543d435021104b831a1b199"
---

# What is Satellite Mission Management? Mission Operations Explained.

Blog


September 9, 2025


nate-hamet


Historically, it has taken a large multidisciplinary team of aerospace engineers to operate a single satellite. Pioneers like[Katherine Johnson](https://www.nasa.gov/content/katherine-johnson-biography) and[Gene Krantz](https://www.nasa.gov/sites/default/files/files/Gene-Kranz-Bio.pdf) built the foundation and the first principles have generally remained the same since the 1960s.


Satellite operations for the National Oceanic & Atmospheric Administration (NOAA)


**But the game has changed.**


The space industry is shifting from companies launching a handful of satellites over a decade, to launching hundreds of satellites in just a few years. The traditional model of many engineers operating a few satellites simply doesn’t scale.


## What does it take to operate a satellite?


At a glance, an operations team needs to:


- Calculate the satellite orbit and accurately predict its position
- Avoid space debris and objects
- Create the terrestrial infrastructure to communicate with a wireless orbiting computer that is zooming around the Earth at 7.5 km/s
- Monitor the health of the satellite and its various subsystems
- Develop and execute procedures to operate the payload (e.g. a camera) or plan recovery and maintenance activities
- Analyze and troubleshoot infrastructure, antenna, and satellite anomalies
- Manage the personnel that glues this all together


**They need to do these tasks reliably every single day until the satellite is de-orbited.**


## What is satellite flight dynamics?


Flight dynamics answers three questions: where is the satellite now, where is it going, and how is it going to get there? Along the way, the satellite needs to maneuver around[27,000 (and growing) pieces of space debris](https://www.nasa.gov/mission_pages/station/news/orbital_debris.html) and other passing satellites.


Included in the flight dynamics responsibilities team is station keeping – the act of maintaining an intended orbit. The Moon, the Sun, an irregular shaped ellipsoid called Earth, and atmospheric drag all cause a satellite’s orbit to change from the idyllic textbook models.


To maintain its orbit, a satellite has thrusters it can fire but this activity takes accuracy and precision in not only its current and predicted location, but also the amount of time to burn down to the millisecond. Fuel is also a finite resource. While gas stations in space are becoming a near-term possibility, most satellites can only perform so many maneuvers before it begins to run low on fuel. This is the point at which it needs to be de-orbited which can quickly end a mission or significantly impact mission coverage.


## What is satellite mission planning?


A Low Earth Orbit (LEO) satellite at 450km takes around 90 minutes to completely orbit the Earth. An ops team needs to ensure it has scheduled a sufficient number of contacts, spaced apart optimally, to ensure the satellite is adequately executing its mission.


In the past, aerospace companies would need to build and operate their own antennas and place them strategically around the world to optimize the amount of contact time with their fleet. This takes years of design and integration and can literally feel like the show[Ice Road Truckers](https://en.wikipedia.org/wiki/Ice_Road_Truckers) , where certain geographic locations are only accessible when the ice roads are frozen but holes can only be dug when the ground is thawed. Today, ground stations are more like AirBnBs that can be reserved for a period of minutes, days in advance. These providers include[Atlas](https://atlasground.com/) ,[AWS](https://aws.amazon.com/ground-station/) , and[KSAT](https://www.ksat.no/ground-network-services/) to name a few.


But what if you needed to talk to dozens of satellites across a handful of antennas and providers? How do you carefully select which satellite to contact but try to evenly contact them to verify their state of health? What if other antenna customers reserved the contact that would have been most ideal for you? This is one of the many jobs mission planning would have to handle.


Another job of mission planning is to ensure that the mission tasks are being executed as efficiently as possible. Take the example of a fleet of Earth imaging satellites. If a customer wants in image of a given location as quickly as possible, the mission planning system needs to ask the following questions: which satellite is capable of taking an image based on satellite health constraints (power, mode, etc), which satellite will have access to that ground site at the given time, and which satellites are not occupied with other tasks at that time. This math is simple with one satellite and one user, but gets complicated with scale.


## What is satellite command & control?


Command and control, often known as C2, encodes satellite commands and decodes satellite telemetry. It’s the engine of the ops center.


Most satellites speak their own language and dialect depending on who built them. There are standard protocols such as[CCSDS](https://public.ccsds.org/default.aspx) ,[CSP](https://en.wikipedia.org/wiki/Cubesat_Space_Protocol) , and[HDLC](https://en.wikipedia.org/wiki/High-Level_Data_Link_Control) , but many manufacturers implement their own version (dialect) that ground systems need to be configured for.


The upload and download speeds of satellites is often VERY small. Unlike your internet speeds at home, even state-of-health space-to-ground links can be as small as 64 kbps on the uplink and 120 mbps on the downlink. This is about 100x slower than *basic* home internet packages and 4x slower than 3G technology!


One of the most complex operations for a mission operations team to perform is uploading a software patch. With only about 10 minutes of access time per contact, it can take multiple passes to complete an upload and verification of a software patch. This requires precise traceability and verification from custom ground software to understand where it left off when uploading. The last thing an operator wants to do is upload a corrupted image and jeopardize the mission.


## Who else is involved in mission operations?


Some of the other teams involved with operations support:


- **Facilities:** In charge of procuring office space to host the team and operations centers. Usually, two operations facilities in different geographic locations in case of a regional outage.
- **IT:** Provides network and security from the operations centers to the antennas.
- **Systems Engineers:** The experts on the satellite and mission design. They are relied heavily on during satellite troubleshooting and operation verification.
- **Spacecraft Operators:** Engineers who can operate the ground software to command and control a satellite.
- **Ground Support:** Experts in RF and space-to-ground protocols.
- **Software Developers:** Somebody must build or integrate the software that controls the satellite. This is a lifetime commitment to fix bugs and produce features for the operations team.
- **Management:** The name of the team is Mission Management, and it takes an organization to become a smooth operator.


## How much does satellite operations cost?


Software engineers are expensive. Satellite operations is a niche expertise and a 24/7 job. The space industry is growing and hiring talent is at high demand. Launches are becoming cheaper, and satellites are becoming less expensive to build so companies are building more satellites. Hiring an ops team and the software to operate even a few satellites can cost more than $6M a year.


## Why is satellite operations expensive?


- Human capital is expensive, and the team includes more than just personnel. It includes the facilities, software licenses, and compute infrastructure.
- Aerospace COTS software wasn’t made for scale and is, for the most part, not cloud native. It takes a team to create new software or integrate existing products.
- Mission management is just as much a software team as it is an aerospace team.


## When is satellite operations designed?


Immediately. A mission director should be the first hire and a software lead should be the second. They should be involved in the mission design **and** spacecraft design. Carol Shelby and Bruce McLaren not only drove their race cars, but they also gave input into the design because they were the ones in charge of operating them. Why wouldn’t you want to do the same with the operators of the spacecraft that need to be flown by a team for five or more years?


It also takes time to build the team and system. Hiring, architecting the ground network and software solutions, working with 3rd party providers, and software development can take many years. Bottom line, if you are not the expert in ops, it’s never too early to get an expert involved – especially when your mission counts on them.


## What are the common mistakes of mission management?


Here are some of the common mistakes that new space companies make prior to thinking about mission management and operations.


- 🤯 Integrating various COTS software that have never been integrated together
- 😱 Building the ops team with less than a year to launch
- 🙈 Not hiring an ops leader at the start of mission design
- 👾 Thinking ops needs to be built from scratch
- 💰 Completely outsourcing ops
- 👫 Hiring more operators to solve more problems
- 🛰 Purchasing a satellite before hiring an ops software lead
- 📡 Purchasing ground stations before hiring an ops software lead
- 👩🏻‍💻 Not having a solid development, staging, and production environment
- 📑 Not integrating manufacture test scripts into ops


## Summary


- Satellite operations is the tip of the iceberg for satellite mission management
- Traditional mission management requires a large team of aerospace and software engineers
- Most COTS software was not designed to operate the[upcoming satellite boom](https://payloadspace.com/euroconsult-releases-smallsat-industry-report/)
- It’s never to early to get an expert involved in your mission design
