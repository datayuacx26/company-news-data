---
schema_version: "1.0.0"
document_id: "c4f80d850d3d2e5a45690ad97d8ee68320a95c42e4e9b3dc621999a5269943db"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-13cc9c892e31"
canonical_url: "https://about.roblox.com/newsroom/2026/07/how-in-game-reporting-works-on-roblox"
published_at: "2026-07-15T12:00:00+00:00"
first_seen_at: "2026-07-23T23:14:00.046707+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:cdfbed679fbdd410f0c317b47fd4853a00e1170234609c4de41162bf6a7e58f5"
---

# How In-Game Reporting Works on Roblox

Share


[Engineering](https://about.roblox.com/newsroom?filter=engineering)


# How In-Game Reporting Works on Roblox


The Tech Behind Capturing Detailed Reports in Dynamic 3D Environments


By


bc Wong


Published


Jul 15, 2026


- Roblox processes 274 million daily avatar updates, and that’s just one type of user-generated content that we moderate for safety violations.1
- Player reports are a crucial part of identifying violations early, and we want to make it easy for players to report anything they see that they believe is inappropriate or against our policies.
- Because violations in dynamic, user-generated 3D content can be challenging to capture, we leverage ray casting, data model cloning, and an overhauled reporting UI to help players identify and report specific objects with the data and visual evidence necessary for us to take action.


At peak, the Roblox platform processes 274 million daily avatar updates.1 Reporting and reviewing abuse in this type of dynamic, user-generated 3D content is challenging because it doesn’t always leave the explicit trail that static content like a text log or a social media post does. In this blog post, we describe our approach to capturing the data and visual context necessary to act on player reports, from our initial ray casting approach for identifying abusive avatars, to our current system, which integrates with the Roblox data model so that players can precisely report any 3D game object.


Player reports are a crucial part of identifying potential policy violations early, and we recently[updated our reporting flow](https://about.roblox.com/newsroom/2026/07/major-updates-in-game-reporting-tools) to make it easier for players to immediately report or block anything that makes them feel uncomfortable or unsafe. Player reports now result in the automatic removal of over 19,000 policy-violating avatars per month and serve as an important early signal for detecting inappropriate games.2


## The Challenge of Capturing Dynamic Content


Since dynamic, user-generated 3D content can be combined and used in different contexts, simple descriptions or screenshots don’t always provide the necessary data for us to take action. Static content like social media posts, forum comments, and videos act as permanent documentation of an issue, making it easy to both report and review the evidence. Text and voice chat are documented on Roblox, but other types of user-generated content don’t leave such an explicit trail. Images can be loaded from elsewhere on the platform, dynamic interactions might not be documented, and players sometimes combine individually innocuous clothing items to form an outfit that’s inappropriate.


In order to capture policy violations in dynamic situations, a reporter first needs a way to identify specific elements within a game. Second, the identification mechanism needs to provide object IDs for the reported content so that the correct object can be evaluated.


An example where layering two separate shirts, one with the letters “CK” on the sleeve and one with the letters “FU” on the torso, results in profanity that could evade moderation systems. Generating an ID for this avatar makes it possible to report and review.


## Detecting Abusive Avatars in 3D Space


Our previous reporting tool relied primarily on chat log evidence. Reports didn’t carry any visual information, leaving large gaps in understanding. When we began to work on a reporting tool that could capture more context around other types of violations, we focused first on a type of object that every player interacts with: avatars. Players can customize avatars to represent themselves on Roblox, and capturing information about an avatar meant we’d be able to associate violations with specific players even if there was no paper trail in chat. A system that could effectively identify avatar data would also be an important foundation for other types of objects in the future.


Potential solutions included taking screenshots or background recordings to supplement reports with more evidence. We started testing with screenshots because they were an easy way to gather insights, even if they didn’t capture all possible violations. We quickly ran into a limitation: A 2D image didn’t help us distinguish a player’s avatar from other avatars and NPCs in 3D space. We couldn’t always accurately identify the user ID of the potential violator when avatars overlapped.


We needed to figure out how to extract more information so we could turn a 2D image taken at a single point in time into actionable evidence.


We settled on ray casting as an efficient method to isolate relevant information in 3D space and represent it in a 2D image at the moment of a report. When a player opens a report, we first gather a list of players in the session and filter out any who aren’t in the viewport. Then we use ray casting to determine the bounding boxes for each player avatar. At the same time, we automatically save the Roblox engine’s frame buffer content into a screenshot. This way, a simple 2D screenshot can be paired with enough spatial data about the 3D world to differentiate the potential violator from other avatars and the surrounding environment. Ray casting allows us to determine bounding boxes in 3.5 milliseconds on average. Slower methods would have required an awkward pause, disrupting the experience.


An illustration of how ray casting helps determine which region in the 2D screenshot belongs to which avatar.


A screenshot of a game with bounding boxes around avatars and tighter convex hulls that pinpoint where the avatar is on-screen.


From there, other contextual information about a potential policy violation can be gathered asynchronously. Today, the system automatically triggers this mechanism when a player starts a report.


## Highlight Mode


Next, we needed an intuitive reporting interface so players could quickly identify violating content. We updated our reporting interface with a Highlight Mode to let players highlight offending items and annotate their reports with more information. When a player clicked on an offending item, we would cast a spread of rays in a circle around the player’s cursor. If they selected a player’s avatar, we could capture all the necessary metadata to review their report and act. In internal user research, Highlight Mode was well received, including by younger players (ages 9-13) who were now more easily able to highlight a problem and successfully complete a report.


Highlight Mode allows players to highlight what they want to report using the in-game report UI (e.g., the green circle around the duck avatar).


With this initial version of the new reporting tool, we only captured IDs for avatars. If a player selected something else, we wouldn’t capture an object ID. We could examine reports for insights and greater context, reach out to developers, and decide what to do next.


But we envisioned a mechanism that allowed players to precisely highlight any item in a game and capture its data directly in their report.


## Data Model Capture for Generic Content Types


While our circular ray casting approach worked well for differentiating the limited number of avatars or ads within a player’s viewport, it wasn’t scalable to every object in a game. Consider the environment below:


In a dense environment where dozens of objects like rocks, shrubs, and flowers overlap and animate, ray casting in a circle wouldn’t reliably identify a single flower or shrub. We needed to allow players to highlight objects with more precision, and we needed a way to capture the data associated with any object in a game.


Roblox’s engine already stores a representation of the 3D world in its[data model](https://create.roblox.com/docs/projects/data-model) , but the entire data model for a game can be gigabytes in size. To optimize for speed, we created a cloning mechanism (a mechanism we first developed for[Translation Feedback](https://devforum.roblox.com/t/introducing-translation-feedback/2935975) ) that clones only the key objects needed to render a scene. Once we have this cloned version of the data model, we can then use Highlight Mode to drop the reporter into the cloned world where the player can precisely highlight what they want to report in 3D.


Highlight Mode now clones key objects in the game’s data model and allows players to directly highlight the object they’re reporting.


This approach gives the player more control over what they’re reporting and gives us the metadata required to evaluate many more content types. We also automatically attach a screenshot to the report to help with the moderation review.


## Data Integrity


One key question we’ve asked since the start of the project is whether we should capture evidence on the client’s device or on the game server. Roblox supports client-side scripts that may update only the client state, and not on the server. In addition, some physics effects are nondeterministic, which means a server-side capture might not show exactly what the reporter experienced. We decided on client-side capture to ensure that the reporter can capture a faithful representation of what they’re seeing, but that comes with a trade-off: A bad actor could potentially gain access to a client and fabricate evidence.


To mitigate this, we verify accuracy with a variety of signals and weed out inaccurate client representations. As an analogy, meteorologists can produce accurate weather forecasts even if they aren’t receiving accurate readings from every weather station.


## Future Work


Thanks to these improvements and the ability to capture richer visual information, the system is now automatically taking down over 19,000 policy-violating avatars per month and has become an important signal for detecting inappropriate games. We’ve also seen strong adoption, with approximately 19% of eligible reports including visual annotations.2


We’re already at work on the next phase of improvements, developing a way to detect dynamic violations (such as emotes combined with player movement) that cannot be determined by point-in-time snapshots. Later this year, we plan to enable capturing a time sequence of the scene being reported. As Roblox continues to provide creators with the tools to build richer in-game interactions, our safety team will continue to promote a safe environment for all players.


*We’d like to thank Rayan Hussain, Ryan Liu, Bridget Daly, Agatha Kielczewski, Alex Leavitt, Ying Liao, and Andrew Xu for their work on this project.*


1 *Based on data from H1, 2025.*


2 *Based on data from March 1 to April 1, 2026.*
