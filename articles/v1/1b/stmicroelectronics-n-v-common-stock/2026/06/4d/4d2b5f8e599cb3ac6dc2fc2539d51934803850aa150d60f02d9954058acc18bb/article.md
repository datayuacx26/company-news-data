---
schema_version: "1.0.0"
document_id: "4d2b5f8e599cb3ac6dc2fc2539d51934803850aa150d60f02d9954058acc18bb"
company_key: "stmicroelectronics-n-v-common-stock"
company: "STMicroelectronics N.V."
source_id: "stmicroelectronics-n-v-common-stock-rss-811b8a53447b"
canonical_url: "https://blog.st.com/point-one-navigation/"
published_at: "2026-06-09T15:57:54+00:00"
first_seen_at: "2026-07-24T02:26:40.894294+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:49fb2dac849990d43eec66bf8a2e160a03032882e9e708c3670efd7b6b165713"
---

# Point One Navigation: They started with TeseoV, Their new TeseoVI solution rivals $50,000 systems

**[Point One Navigation](https://pointonenav.com/) , a member of the[ST Partner Program](https://www.st.com/content/st_com/en/partner/partner-program/partnerpage/point-one-navigation.html#) , will showcase an updated version of its demo at the[AutoSens](https://auto-sens.com/usa/) tradeshow, opening on June 9 in Detroit.** Building on what the company showed at CES 2026 last January, Point One Navigation will present a precise positioning system with real-time kinetic and inertial engines fully integrated into the ST demo car. The Point One Navigation[TeseoVI](https://www.st.com/content/st_com/en/campaigns/teseo6-quad-band-gnss-receivers-for-autonomous-driving-and-asset-tracking-industrial-applications.html) -based solution runs in the vehicle in real time. Let us explore how Point One Navigation arrived at this point, how it began using ST technologies, and the lessons the company learned along the way.


## How it started?


### Learning to ship fast


Point One Navigation learned that shipping fast helped them understand markets better


Interestingly, Point One Navigation shared that they started their ST journey years ago with[TeseoV](https://blog.st.com/teseo/) modules featuring an[STM32H7](https://www.st.com/en/microcontrollers-microprocessors/stm32h7-series.html) MCU and ST inertial sensors. It allowed them to work on their positioning engine, learn how to use our GNSS receiver, and collaborate with partners who would integrate these modules into their designs. **Put simply, it was about shipping a solution that would work for established leaders.** Too often, teams get bogged down trying to do too much, too quickly, and too early. By working with modules with ST devices, Point One Navigation gained the expertise it needed to grow strategically and meaningfully.


### Mastering feature development


By working on real-time kinematics and dead reckoning on a TeseoV and an STM32H7, Point One Navigation learned to use the ST platform more effectively. **For instance, it understood the importance of running its application in a tightly coupled solution, but also that it had limits around the amount of information it could process at once.** Real-time kinematic uses GNSS signals and local data points to provide precise location estimates. However, that requires the ability to process more data than a traditional GNSS positioning system and to compute these data points in real time. Similarly, the dead reckoning system relies on sensor data to fill in for incomplete or absent satellite information.


### Evolving from module to bespoke solutions


Emboldened by this experience, Point One Navigation decided its solution would benefit greatly from the[quad-band capability of the TeseoVI](https://blog.st.com/teseovi/) and could be further optimized by working at the chip level rather than the module level. **Put simply, the demo shown at CES 2026 of their Atlas Duo standalone reference system stood on the shoulders of years of experience with previous ST devices and solved the limitations encountered previously.** That’s why the company was able to[obtain results](https://pointonenav.com/news/live-demo-ces-2026-with-st/) previously available only on systems that cost multiple times more and offer unique features, such as the ability for RTK and inertial engines to talk to each other.


## How’s it going?


### Learning to fail early


A TeseoVI


One lesson Point One Navigation learned as they evolved from TeseoV modules to bespoke TeseoVI systems is the importance of failing early. Indeed, the company was successful because it rapidly discovered what worked and what didn’t, allowing it to offer solutions for modules and then unique features in its Atlas Duo application. Similarly, its engineers recommend that customers quickly identify the edge cases that could prevent them from obtaining accurate positioning data. Will a truck drive through an area with little to no coverage? Will assets use containers that could block signals? **By failing early, and learning from it, engineers can ship a more robust solution in the long term.**


### Understanding what customers are not saying


Another lesson Point One Navigation shared with the ST Blog is its ability to understand its customers’ needs, especially what they are not saying out loud. For instance, one client was using a positioning system sold by a competitor with very mixed results. Point One Navigation realized that the system had been built for cars but was used on a drone that required altitude information. **By shipping early and building strong expertise with ST devices, Point One Navigation not only came up with Atlas Duo but also developed a way to fully understand what their customers really needed, rather than relying on haphazard assumptions.**


### Creating object lessons


Finally, a third lesson is that proof of concepts are object lessons. P **oint One Navigation shared that, by using a TeseoVI, an ST MPU or MCU, and ST sensors, the proof-of-concept serves to educate the customer rather than being a disposable design.** In fact, the company explained that most of the time, its teams simply get a development kit from ST, attach it to a Raspberry Pi, and send the whole thing to a customer who only needs to run a few lines of code to see what it can do. The proof of concept thus becomes a tool for managing expectations and teaching customers to understand what they truly need.


- [Learn more about Point One Navigation](https://pointonenav.com/)
