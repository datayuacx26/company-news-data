---
schema_version: "1.0.0"
document_id: "7e12d83f5c497d3326d94b628ff7f3795c7b9a5b3d8dca9e91ace2c44a0b50aa"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/166963330230"
published_at: "2017-10-31T12:34:14+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:50bd540e9ca8c8f9ecc31af2ddca381a1a748777fa2d17d31d656ff53acf3f1e"
---

# NASA’s Johnson Space Center transcends space and time with custom built bike tracking system

[NASA’s Johnson Space Center](https://www.nasa.gov/centers/johnson/home/index.html) campus is always bustling with people. With about 3,000 employees, 12,000 contractors, and a steady stream of interns and students making their way around the facilities at any given moment, efficient modes of transportation are a must … rockets and space shuttles aren’t exactly pragmatic! Enter their bicycle sharing program, tracked and powered by Estimote beacons.


It all started with a summer internship by an ambitious Computer Science student from Washington University in Saint Louis named David Lie-Tjauw. David was working alongside the sustainability team at NASA to identify issues and prototype unique solutions. With the mentorship of NASA leadership, David was able to flex his technical knowledge while gaining precious insights and education to building a fully workable product. After numerous brainstorming sessions, they honed in on Johnson Space Center’s free-range bike program. These over 300 bikes are available for NASA personnel to freely use. The problem? People had trouble actually finding them on campus! Thus, the project was born: what if employees could simply log in, and view the real-time locations of the bikes?


Here’s how it works. Each free-range bike is outfitted with an Estimote Proximity beacon, broadcasting the identifier of the respective bike. Stationed in every building at JSC is a Raspberry Pi, substituted for a smartphone, hard-coded with a permanent location tag and running a custom-built Android application that scans for nearby beacons. When a bike is parked at a building, the Raspberry Pi and Android app sees the beacon identifier, sending it to the online real-time database (in this case, they’re using Google Firebase). For example, a Raspberry Pi in Teague Auditorium would tell Firebase that bikes #12345, #54321, and #19283 are at Teague. The Raspberry Pi updates the database roughly every 30 seconds, so once a bike leaves a building, the Firebase database will reflect changes with each update. Finally, an easy-to-use website called BikeJSC reads the bike data from Firebase and visualizes it on a Google Map image of Johnson Space Center. As bikes move throughout campus, the website will update in real-time to accurately display the number of available bikes at specific buildings. NASA employees can use this website to find nearby bikes.


Most of our developers use smartphones, but it was clear off the bat that smartphones wouldn’t scale for this project… the campus is huge! Needing a smartphone for every building would be expensive and battery life would be killer, so David utilized Raspberry Pi’s, mini barebones computers with no screens to work alongside Estimote SDK. In order to successfully run Android applications on the Raspberry Pi, David used Android Things, a brand of Android OS specifically tailored for the Internet Of Things.


“Bike tracking at NASA JSC would not be possible without the use of the Estimote SDK. I would like to give a shout out to Estimote’s amazing engineers and developer community who have created extremely useful and comprehensive documentation for the Estimote SDK. Often times, the biggest barrier that keeps users from implementing any new technology is the availability of help and resources needed to figure out how to use such technology in the first place. Estimote has done a fantastic job on this end!” - *David Lie-Tjauw, lead project developer*


Often you hear in business that a foot in the door is everything. Well, what good is a foot in the door at NASA? We can’t help but humblebrag about influencing a NASA intern, ourselves. Plus, we’re hearing all sorts of ideas! David is a self admitted Estimote enthusiast. He’s already brainstorming new ways to integrate BLE tech and Estimote beacons in space exploration. For example, what if the bike tracking system could be applied by using Estimote beacons to track the location of objects in microgravity, aboard the International Space Station? The concept of contextual computing and its potential to revolutionize hardware and software mechanics can enable engineers at NASA to attack problems from entirely new perspectives.


Be sure to keep an eye on this[young talent](http://www.linkedin.com/in/david-lie-tjauw-2b2b53120) , and look out for the bikes next time you’re visiting NASA’s Johnson Space Center in Houston!


*For more information on the project, feel free to reach out toDavid Lie-Tjauw*


*Written byJess Anderson , Content Creator + Community Manager @ Estimote*
