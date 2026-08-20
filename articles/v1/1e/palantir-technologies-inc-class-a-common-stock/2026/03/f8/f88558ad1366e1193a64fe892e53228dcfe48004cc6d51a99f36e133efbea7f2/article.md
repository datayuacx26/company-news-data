---
schema_version: "1.0.0"
document_id: "f88558ad1366e1193a64fe892e53228dcfe48004cc6d51a99f36e133efbea7f2"
company_key: "palantir-technologies-inc-class-a-common-stock"
company: "Palantir Technologies Inc."
source_id: "palantir-technologies-inc-class-a-common-stock-rss-f87a89a6619a"
canonical_url: "https://blog.palantir.com/frontend-engineering-at-palantir-redefining-real-time-map-collaboration-8845ebb928d1"
published_at: "2026-03-26T16:38:29+00:00"
first_seen_at: "2026-07-20T03:31:10.454667+00:00"
fetched_at: "2026-07-28T22:17:33.975741+00:00"
content_hash: "sha256:3abca0f7d85f82c76e217d67e88bbddbf381fdef6f3808d8bb57d538a64be412"
---

# Frontend Engineering at Palantir: Redefining Real-Time Map Collaboration

# Frontend Engineering at Palantir: Redefining Real-Time Map Collaboration


## How we built lightweight, real-time map collaboration for teams operating at the edge.


[Palantir](https://palantir.medium.com/?source=post_page---byline--8845ebb928d1---------------------------------------)


8 min read


·


Mar 3, 2026


--


Press enter or click to view image in full size


### [About This Series](https://blog.palantir.com/all?topic=palantir-frontend)


*Frontend engineering at Palantir goes far beyond building standard web apps. Our engineers design interfaces for mission-critical decision-making, build operational applications that translate insight to action, and create systems that handle massive datasets — thinking not just about what the user needs, but what they need when the network is unreliable, the stakes are high, and the margin for error is zero.*


*This series pulls back the curtain on what that work really looks like: the technical problems we solve, the impact we have, and the approaches we take. Whether you’re just curious or exploring opportunities to join us, these posts offer an authentic look at life on our Frontend teams. Find all Frontend Engineering blog posts*[here](https://blog.palantir.com/all?topic=palantir-frontend) *.*


*In this blog post, Leon, a Full Stack engineer based in NY, shares how building Gaia’s Follow Along mode — his first frontend project at Palantir — shifted his perspective on frontend engineering, while diving into the unique challenges of enabling real-time collaboration at scale.*


Before joining Palantir, I thought frontend engineering was mostly about adding pixels, pushing them around, and making things look nice. While that’s certainly a component of frontend development, it’s nowhere close to the complete picture of what frontend engineering looks like here. At Palantir, frontend engineers are deeply involved in solving complex, novel problems that demand creativity, technical rigor, and close collaboration across teams.


My perspective didn’t change overnight — it changed through first-hand experience. Gaia Follow Along Mode was one of my first projects at Palantir, and it perfectly captured just how different frontend engineering is here.


## Gaia Follow Along Mode


[Palantir Gotham](https://www.palantir.com/platforms/gotham/) is Palantir’s platform for integrating, analyzing, and operationalizing complex data in mission-critical defense and intelligence environments. Used by American and allied forces worldwide, Gotham enables teams to collaborate seamlessly, share situational awareness, and make informed decisions — even in challenging operational contexts.


At the heart of Gotham’s geospatial capabilities is Gaia, our collaborative map application. Gaia enables teams to analyze and share geospatial data in real time, providing a common operating picture across distributed environments. The Follow Along feature in Gaia allows users to “follow” another user’s actions — tracking their cursor, zooming, panning, and selection activity as it happens.


This feature is particularly valuable in defense and other mission-critical environments, where teams are often distributed across remote bases and must operate under severe bandwidth constraints and high network latency. Traditional screen sharing is often impractical or even impossible in these settings; even when video teleconferencing is available, the resulting screen shares are low resolution or laggy. Operational users need a lightweight, reliable way to share situational context — one that doesn’t consume precious bandwidth, is resilient to latency, and doesn’t require high-end connectivity.


Follow Along is a feature that addresses the shortcomings of traditional screen sharing by transmitting only essential state (such as map position, cursor location, and other select information), and sending updates only as changes occur, rather than continuously streaming every frame of the screen. This approach enables real-time collaboration without the heavy network load of true screen sharing.


Press enter or click to view image in full size


On the surface, it might look like a simple feature, but building it required thoughtful engineering across multiple dimensions:


## Get Palantir’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**Real-time collaboration:** Developing a real-time collaborative feature introduced several unique challenges.


- **Shared state:** We needed to reliably synchronize user actions across multiple clients, ensuring that every interaction was reflected almost instantly for all participants. To achieve this, Follow Along leverages an internal shared-state streaming framework, along with libraries such as Redux, to track and update specific map and selection properties in real time. Each client subscribes to updates for the relevant artifact, and only users with the appropriate permissions can participate.
- **Ordered updates:** To guarantee consistency, we introduced a monotonically increasing update index for cursor events, ensuring updates are processed in order and preventing jumpy or inconsistent cursor movement for followers.
- **Follower chaining and control:** We had to handle follower chaining and prevent circular follows (where users could end up following each other in a loop), and ensure seamless hand-offs when a follower wanted to take control.
- **Disconnection handling:** We had to gracefully handle disconnections. For example, if the leader disconnected, the Follow Along session would automatically end for all followers. However, to account for poor network connectivity and intermittent disconnections, we implemented a timeout that waits 15 seconds after the leader disconnects before ending the session for the follower.


**Performance:** Our users are often deployed in austere environments, where bandwidth is low, latency is high, and compute and hardware resources are limited. To maintain a smooth and responsive experience, we made careful optimizations to minimize resource usage and maximize reliability.


- **Selective data streaming:** Given the restrictive network bandwidth, we had to carefully scrutinize the data included in our updates. While our maps can support large-scale data, we intentionally designed the API to transmit only essential, lightweight information — such as user info, viewport, cursor location, and selection state — across the network.
- **Event throttling:** We were selective not only about what data we sent, but also how frequently we sent it. For example, we processed only every third cursor event, reducing update frequency by two-thirds. This optimization helped maintain smooth cursor movement and minimized frequent re-renders that could degrade performance on devices with limited hardware, while also reducing the impact of network latency on the user experience.
- **Selective state synchronization:** Updates are only stored and synced across clients when a user is actively being followed, to prevent unnecessary work and synchronization overhead — especially important in low-bandwidth environments.


**Domains:** Building for maps presented unique challenges that required domain-specific solutions.


- **Consistent cursor positioning:** For example, we can’t rely on pixel coordinates to share cursor location because clients may have different screen sizes and aspect ratios. Instead, we use geo-coordinates (latitude and longitude) as the source of truth for the cursor position to ensure everyone saw the cursor in the correct location.
- **Contextual UI hiding:** When in Follow Along mode, many map controls and accessory side panels are hidden for the follower. These are disabled to avoid distraction and to prevent accidental interaction by followers.
- **Viewport synchronization:** We had to ensure that each follower was able to see the entire viewport of the leader. The leader’s viewport of the map can vary based on various factors, such as open panels, screen size, and aspect ratios. To achieve this, we introduced a bounding box that tracks the top-left and bottom-right geo-coordinates visible on the leader’s screen. For each follower, we would then scale and center their viewport to fit this bounding box, ensuring that the entire area visible to the leader was also visible to every follower, regardless of differences in device or screen size.


Press enter or click to view image in full size


> Example of viewport synchronization: the blue box on the right represents the leader’s visible map area. Both the leader’s (left) and follower’s (right) views are centered on the same geo-coordinate. Even though the follower’s visible view of the map differs from the leader, the follower’s viewport is scaled and adjusted to ensure the entire area visible to the leader is also visible to the follower.


In short, doing this kind of work meant building a deep understanding of both the tech stack — from React and Redux to internal frameworks for distributed state management — and domain-specific areas like geospatial rendering and real-time data streaming.


It also required developing a strong empathy for user pain points and the reality of how and where our products are used. We worked closely with designers and product managers to ensure the features built truly addressed real-world collaboration challenges.


## An Unexpected Success Story


We built Gaia Follow Along mode as a bit of a bet — it wasn’t a feature users explicitly requested, instead, we engineered a solution based on the real pain points and feedback from operational users in the field. Initially, we enabled it at the request of a single user during a recent military exercise — almost as an experiment. To our surprise, it quickly became the #1 “tech win” of the event. Users found it incredibly impactful for secure, real-time collaboration on maps, and adoption spread rapidly as more teams discovered its value. The overwhelmingly positive feedback prompted us to share it across the company, and soon, other teams began to request demos and training.


Since then, Gaia Follow Along has evolved significantly, shaped by continuous user feedback from the field. We’ve added features like seamless hand-offs, allowing presenters to transfer their followers to another user, and customization options such as hiding usernames alongside the shared cursor. In subsequent projects, we made further optimizations. For example, some critical state is now passively streamed before a user starts following, ensuring that followers are immediately up-to-date when they join. Other state is only streamed once clients begin following, further reducing unnecessary data transmission. We have also scaled the system to support hundreds of concurrent users following the same leader, enabling large teams to collaborate in real time without sacrificing performance or reliability. Each new iteration was driven by feedback from people actually using the tool in high-stakes, collaborative environments. Each iteration required thoughtful engineering to meet evolving needs.


Press enter or click to view image in full size


> Example of a mock from a subsequent project to improve leader-follower hand-off and follow interactions. A great level of detail goes into each change and each small part of an overall feature.


Even though I no longer work directly on this feature, seeing it win the hearts of users and continue to evolve remains incredibly rewarding. It’s a testament to how frontend engineering at Palantir isn’t just about building user interfaces — it’s about delivering transformative experiences that truly matter to our users.


Gaia Follow Along mode is just one small glimpse into the vast landscape of frontend projects happening at scale across the company. If you’re passionate about building innovative and complex solutions from the ground up — solutions that have real-world impact — you’ll find no shortage of fascinating problems to tackle here at Palantir.


*If this sounds like the kind of project and impact you’re interested in, check out our open roles today:*[https://www.palantir.com/careers/open-positions/](https://www.palantir.com/careers/open-positions/) *. Our most applicable frontend postings are the “Web Application Developer” roles. We’re also hiring for these two specific roles right now:*[Software Engineer — Core Interfaces](https://jobs.lever.co/palantir/cf76738e-3030-42fa-92ac-a9446df956fc) *(Palo Alto), and*[Software Engineer — Defense Applications](https://jobs.lever.co/palantir/f7dbfdf1-0bb1-4c11-ac15-6a139cee3410) *(DC).*
