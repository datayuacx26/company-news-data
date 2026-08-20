---
schema_version: "1.0.0"
document_id: "4228b6594c4f5e411e0c5c968b9a43b0fd845bd80c1949e1a9f56409f2827803"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/polymath-perspectives-roscon2025"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:a73ec5bc05b8ab7359beba76d1562ac0af1626a4270e1be30ba06e7f7ed83316"
---

# Polymath Robotics Blog | Polymath Perspectives from ROSCON2025 in Singapore

Troy Gibb and Emerson Knapp, two of Polymath’s Staff Engineers, attended ROSCon 2025 in Singapore a few weeks ago.


Here are their takeaways & hot takes from the conference - as jotted down by the gentlemen themselves!


## Themes & Takeaways


- Build and Infrastructure


- Was on full display as one of the pieces a) talked about in official presentations and b) had a lot of discussions (including a “Birds of a Feather” group discussion session coordinated by Emerson)
- Hottest items:


- nix


- Troy’s hot take: So many good chats on this one! Although we didn’t run into many folks using NixOS in their prod fleets.
- Might be worth reaching out to Yuyuan Yuan at ZettaScale, who was an avid evangelist
- Emerson notes:


- [https://github.com/lopsided98/nix-ros-overlay](https://github.com/lopsided98/nix-ros-overlay) - uses[https://github.com/ros-infrastructure/superflore](https://github.com/ros-infrastructure/superflore) to generate Nix definitions for ros distros
- [https://github.com/wentasah/ros2nix/](https://github.com/wentasah/ros2nix/) - generate Nix definitions from your ROS package.xml
- [https://github.com/clearpathrobotics/nix-ros-base](https://github.com/clearpathrobotics/nix-ros-base) - likely very outdated


- [pixi](https://pixi.sh/latest/)


- Pixi is being presented as one of the major package managers for the ros ecosystem going forward. It is now the default package manager for the windows build of ros2, and if we had to guess, will start eating its way into linux distros at some point soon. The dev team for pixi (prefix.dev) was evangelizing pixi hard with multiple talks.


- Troy’s hot take: Pixi on the whole seems like a great way to isolate runtimes without containers and has some lightweight conventions on top of standard ros (pixi.toml), however for full OS configuration, I’m still personally more of a fan of NixOS.


- Built on Conda
- ROS integration is via[https://robostack.github.io/GettingStarted.html](https://robostack.github.io/GettingStarted.html)


- balena


- Troy had a great conversation with Guillaume Doisy, VP at Dexory, who waxed eloquent about belana usage on their fleet of 100+ robots. Incremental upgrades was a feature he called out (even though it’s container based). Otherwise didn’t hear much about it at the conference.
- [https://docs.balena.io/learn/deploy/delta/](https://docs.balena.io/learn/deploy/delta/)


- [https://mender.io/solutions/fleet-management](https://mender.io/solutions/fleet-management)


- Emerson: I’ve been interested in this for a long time. Not mutually exclusive with any of the package management stuff, provides an A/B rootfs delta update mechanism that might replace/simplify use of CM tools like Ansible/Chef. If it’s quick and easy to slap a clean image on there, why modify things?
- Someone said they were using it just for the fleet management portion right now, which makes me think that the feature doesn’t 100% suck, worth looking at more.


- Testing


- RTest ([https://github.com/Beam-and-Spyrosoft/rtest](https://github.com/Beam-and-Spyrosoft/rtest) ) is a unit test stub for running deterministic ros2 unit tests in C++. It works by mocking out the underlying pub/sub libraries in ros2


- Troy’s hot take: worth experimenting with. The folks at Spyrosoft developed this with a client, so it is battle tested.


- replay_testing


- Completely floored the entire conference. People loved it so much they walked out early
- But actually, had a couple of people come up to chat more about it, and stated their intent to use it.


- Simulation


- Some folks from Nvidia, Gazebo and[Robotec.ai](http://robotec.ai/) presented a new simulation standard:[https://github.com/ros-simulation/simulation_interfaces](https://github.com/ros-simulation/simulation_interfaces)


- This standard is meant to reduce friction between using different simulators— currently they target Isaac sim, Gazebo and O3DE.
- Troy’s Hot Take: Definitely worth investigating this framework more! If we have a common layer between these frameworks, the migration cost will be minimal. We can start to experiment more with tools like O3DE without having to change everything


- “Evaluation of ROS 2 Simulators 2025”


- The speaker here presented some benchmarks on the common simulators. TL;DR: Isaac Sim performed extremely poorly in all benchmarks


- Gazebo


- Didn’t see anything worth writing home about


- Data


- [https://docs.roboto.ai](https://docs.roboto.ai/) is building really cool tooling to run analyses on robotics datasets (e.g. bunch o bags). After the record & offload portion is solved - this may answer some next steps of “so how do we do anything with all that data?”


- Tools


- [Cloudini](https://github.com/facontidavide/cloudini) seems like a good package to trial run. Promises up to 50% compression of point clouds, and has an extension in Foxglove for its custom data type


- Emerson’s Hot Take: The core algorithm that Davide’s research led to is actually really simple! (that’s great) But, I discussed this with him and he fully indexed on covering the “general pointcloud” use case. However he admitted that for “structured point clouds” like we get raw from the sensor, video encoders may very well be a better choice! He invited me to consider implementing that as an option or plugin for Cloudini. That’s fine but maybe a whole “framework” is actually overkill if we just want to apply a transformation to a topic.


- [https://github.com/nobleo/ros2_fmt_logger](https://github.com/nobleo/ros2_fmt_logger) is a promising take at a more ergonomic logging API for ROS 2
- [https://github.com/sxyazi/yazi](https://github.com/sxyazi/yazi) looks like really nice terminal-based filesystem explorer
- [https://github.com/nerfstudio-project/viser](https://github.com/nerfstudio-project/viser) is a visualizer someone recommended
- [Roboflow](https://roboflow.com/)


- @Troy Gibb got a verbal promise to get a free trial of this tool!


- RMW - Zenoh


- TBD - Troy missed all these talks for the testing track!
- It’s looking really promising, performance seems great and that’s all we really care about.
- Discovery server solves discovery problems (can fall back to multicast discovery if needed)


- Fast-DDS also has a discovery server now
- Cyclone doesn’t have onue


- CycloneDDS maintainers seem AWOL, no new development happening there
- Zenoh not supported on Humble, probably just won’t be. We could try if we wanted, or use it as yet another point for Jazzy


## Our Favorite Talks


[https://roscon.ros.org/2025/#program](https://roscon.ros.org/2025/#program)


- Day 1


- Hosted BoF session, didn’t do much else other than networking


- Day 2


- ROS Project Update
- On Use of Nav2 Route Server


- Highlighted video of Polymath doing tomato harvesting in production!


- Replay Testing


- Troy was real cool up there 😎


- Rtest - Reliable ROS 2 Unit Tests Framework


- seems promising, we have a lot of flaky tests ourselves, the core patterns encourage it
- good opportunities for integration with replay testing!


- Zenoh Strikes Back


- excited to try rmw_zenoh. seems ready for primetime - we should be able to use it in Jazzy. I don’t think Humble support is impossible, but it’s not there.


- Day 3


- **Infrastructure Project Update**


- The ROS infrastructure is the unsung hero of the whole project. Always good to hear from the maintainers and know at least high level what’s going on.
- He mentioned this great article:[https://openssf.org/blog/2025/09/23/open-infrastructure-is-not-free-a-joint-statement-on-sustainable-stewardship/](https://openssf.org/blog/2025/09/23/open-infrastructure-is-not-free-a-joint-statement-on-sustainable-stewardship/)


- **Our Ten Most Common ROS Questions Answered**


- Super informative high level talk for anybody who uses ROS 2 - thanks Kat 🙂


- **Solving ROS Package Management With Pixi and Robostack**
- **Escape Velocity: Smarter, Cleaner ROS 2 Launch Patterns**


- This one was like crazy good, I don’t know how this guy is such a genius


- **Evaluation of ROS 2 Simulators 2025**


- Spoiler: Isaac did the worst by far


- **Introducing the New ROS Simulation Standard**


- Promising but early stage, maybe not much we can actually use today


- **Introducing Rclrs: The ROS 2 Client Library for Rust**


- good to see the language landscape expand


Want to watch later because we missed them:


- **Rmw_what? Implementing the ROS 2 Middleware Interface**
- **From DDS to Zenoh: Migrating the Dexory Autonomy ROS Stack—configuration, Performance, and External Integration**
- **Unlock Ffmpeg in ROS 2: A Flexible Audio/video Pipeline With Ffmpeg_pipeline and Ros2_control**
- **Smarter Vision Pipelines for ROS 2: Compress, Transport, and Sync at Scale**
- **Help Me With the Bags: Quick & Easy ROS 2 Data Handling**
- **Simplifying Diagnostics: A Ready to Use Robot Webserver**
- **ROS 2 Logging Subsystem and Alternative Syslog Implementation**


## Polymath’s Sessions


### **Emerson - coordinated group discussion “Deployment & Launch Tooling”**


This was a 1 hour “Birds of a feather” (BoF) session, which is just an unstructured group discussion on a topic of shared interest. This one had 30-40 people, but turned into more of an impromptu panel with 5-6 most vocal participants, Emerson moderating, with others chiming in occasionally with questions or thoughts.


My high level takeaway: We still have a long way to go building the bridges between DevOps and robotics as robotics matures into an industry, and there remain many unknowns where common/best practices will someday exist. How can we leverage the good parts from the cloud world where they can manage thousands or millions of server instances? What tools do we need to extend, replace, or modify to meet the realities of robotics cases where some core assumptions are different? Thinking about intermittent connectivity, long-lived server instances that can't necessarily be fungibly replaced, the spatial issues of sensor/device intrinsics and extrinsics, heterogeneous fleets in general, among many more issues.


Some of the topics that came up:


- How do your applications start on robot? Common themes were:


- SystemD services running launchfiles or individual nodes
- Containers & some form of composition, but we're many of us feeling disillusioned with containers as they end up being used for the on-robot use case


- Some talk about launchfiles


- folks iterated their confusion with the Python API and I sold them on my talk haha, I tried not to give that whole conversation in this discussion and steer it to other topics I wasn't going to spend 20 minutes lecturing on later
- began a very promising conversation on "launch rendering", such as automatically translating a launchfile into a SystemD service tree instead of running the processes directly


- What's the payload you deploy to your robot?


- discussed incremental updates because payload sizes are really big and you feel this more when trying to scale
- Containers by default are not good at this because layers don't match well with dependency trees
- Discussed therefore strategies around on-device apt caching, use of Nix, Pixi, rootfs binary diff tools (a la Mender), container image diff (which Balena now offers)


- Some digression into "how to keep track of what you're deploying to and the differences between them" a.k.a. "fleet management" (overloaded term I try to avoid)


- consensus is there is no clear winner solution for robots still but many tools exist to solve parts of the problem
- one team was using Mender's fleet manager
- Polymath is looking into Netbox to discover and catalogue our inventory as our manual processes start to scale-fail
- deploy mechanism - push vs pull. Ansible, Chef CM vs NixOS declarative vs Mender or Jetson full-rootfs OTA overwrite
- How to manage global default configuration, vs fleet/model configuration overrides, vs individual device overrides (calibrations, tunings, identity, connected device ports & serial numbers, etc, etc). No clear solutions, lots of partial internal solutions


### **Troy - “Replay Testing”**


‍


### **Emerson - 2 min Lightning Talk “Monobranch strategy for ROS package maintenance”**


[SLIDES HERE](https://docs.google.com/presentation/d/13sUJCI8zbZ78CgphJXy46bgtFr3wVj_zsoUXvHgQq2o/edit?usp=sharing)


‍


### **Emerson - “Escape Velocity: How to write better launchfiles”**


[SLIDES HERE](https://docs.google.com/presentation/d/1eKRxB8y3y0lEe-YeB6OUsu0nGAy6tnfVKjGfAzVUPak/edit?usp=sharing)


‍


## Photos!


‍


Emerson with his fellow members of the ROS 2 Project Management Committee


‍


As many members of the Open Source Robotics Foundation group of Project Management Committees as we could gather


‍


How many badge ribbons can you reasonably collect?


‍


Troy’s groundbreaking Testing Pyramid framework


‍


Bouldering meetup after the first day of the conference


‍


Troy and Emerson on a Jurassic adventure in the Cloud Forest dome at the Gardens by the Bay


‍


Emerson spends a day with elephants in Thailand before the conference


‍


‍


Robots sighted in the wild


‍


‍
