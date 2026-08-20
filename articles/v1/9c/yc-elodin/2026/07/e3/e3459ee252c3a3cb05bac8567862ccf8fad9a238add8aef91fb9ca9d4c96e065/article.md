---
schema_version: "1.0.0"
document_id: "e3459ee252c3a3cb05bac8567862ccf8fad9a238add8aef91fb9ca9d4c96e065"
company_key: "yc-elodin"
company: "Elodin"
source_id: "yc-elodin-news-import-b1709095e6d7"
canonical_url: "https://www.elodin.systems/post/update-july-2024"
published_at: null
first_seen_at: "2026-07-25T02:35:46.271436+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:2e928160ca3bc45a443f2693c65548b12d69619a30e6b06706ceeb5c0e030ec6"
---

# Company Update | July 2024

July was a month of collaboration and innovation! We worked with our partners on rockets and satellites, added new features and updates to our platform, and laid the groundwork for some major product changes that we are moving to release in the coming months.
‍


##### **🚀 Join Our Team**


**‍** First off - We're looking for a full-time[Flight Software Engineer](https://www.elodin.systems/post/were-hiring---flight-software-engineer) to join our team! If you're passionate about working on cutting-edge flight software for drones, satellites, and rockets, we'd love to hear from you. (or your friends!)


‍[Apply Now](https://www.ycombinator.com/companies/elodin/jobs/VxHqDSh-flight-software-engineer)


‍


##### 📊 **3D Graphing in Elodin**


We're thrilled to introduce 3D Graphing to the Elodin platform! This powerful new feature allows you to visualize complex data in three dimensions, making it easier to analyze and interpret your simulation results. Whether you’re modeling drone flight paths or satellite orbits, our 3D Graphing tool will help you gain deeper insights and improve your design and testing processes. Dive into your projects with enhanced clarity and precision!


```text
# The entity id is required as the first positional argument.
world.spawn(el.Line3d(rocket))


# You can specify the color and width of the line.
w.spawn(el.Line3d(rocket, line_width=  5.0  , color=el.Color(  1.0  ,   0.0  ,   0.0  )))


# You can enable or disable perspective rendering,
# which makes the line scale with distance to the camera.
# This is enabled by default.
w.spawn(el.Line3d(rocket, perspective=  False  ))
```


‍


‍


##### **🔁 Replays!**


We’ve added support for a whole new workflow to Elodin,[replays](https://docs.elodin.systems/reference/replays) . Now you can record a simulation, save it, and play it back at a later time. We’ve also decoupled simulation recording from simulation playback, so now you can start inspecting a simulation before its completed simulating.


This work lays the groundwork for our next big feature: comparing replays. With Elodin today you can record simulations and real world data, but you can’t easily compare them to see how they differ. By letting users compare real world data, and simulating data they can make their simulation higher quality, find bugs in their flight software, and compare different tests!


[‍](https://www.ycombinator.com/companies/elodin/jobs/VxHqDSh-flight-software-engineer)


##### **🕹️ Ergonomic Improvements**


We’ve spent the last month hard at work making Elodin even more fun to work with! We reduced simulation startup times by 10x, which will make iterating on complex simulations much easier. We made Elodin simulations easier to read by reducing the lines of code required to create a graph or panel.We made it much easier to find entities, components, and graphs by adding searching and labeling.


You can find more details about the many improvements in our change log[Here.](https://docs.elodin.systems/updates/changelog)


##### ‍
**🌎 Real World Characterization**


[‍](https://www.ycombinator.com/companies/elodin/jobs/VxHqDSh-flight-software-engineer) A simulation is only as good as your understanding of the real world. As part of our mission to develop higher quality simulations, we have been building test-fixtures to characterize a VTOL drone. We used a[high quality thrust stand](https://www.tytorobotics.com/collections/all/products/series-1580-test-stand-bundle) to analyze the real world properties of drone motors to use in the simulation, and we leveraged a[classic technique](https://www.me.psu.edu/sommer/me481/notes_07_03.pdf) for calculating the real world moments of inertia to further improve the simulation. Using this new data, we'll advance our[Drone blog series](https://www.elodin.systems/post/tuning-a-drone-control-system-in-simulation) into the next chapter: VTOLs.


High fidelity thrust stand


Calculating the real world moments of inertia
