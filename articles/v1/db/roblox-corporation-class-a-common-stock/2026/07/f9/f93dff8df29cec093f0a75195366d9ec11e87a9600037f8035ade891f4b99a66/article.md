---
schema_version: "1.0.0"
document_id: "f93dff8df29cec093f0a75195366d9ec11e87a9600037f8035ade891f4b99a66"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-13cc9c892e31"
canonical_url: "https://about.roblox.com/newsroom/2026/07/creating-responsive-cheat-resistant-games-roblox-server-authority"
published_at: "2026-07-09T12:00:00+00:00"
first_seen_at: "2026-07-23T23:14:00.046707+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:7bce312da69b10d5ec3191a70237332e4f9990da1bda1d9efcfb3a0dbcf36b8b"
---

# Creating Responsive, Cheat-Resistant Games With Server Authority

Share


[News](https://about.roblox.com/newsroom?filter=news)[Product](https://about.roblox.com/newsroom?filter=product)


# Creating Responsive, Cheat-Resistant Games With Server Authority


How Server Authority Levels the Playing Field


By


Bryan Nealer


Published


Jul 9, 2026


We’re introducing Server Authority with client prediction and rollback on Roblox to give creators a powerful new tool for creating hyper-responsive, secure games out of the box. Roblox is inherently multiplayer and multi-device. Server Authority helps games feel responsive and fair across devices by ensuring that any change to game state happens quickly and consistently.


Server Authority has enormous benefits for both competitive and social games on Roblox, but building the tech from scratch is complex and often time-prohibitive. Roblox’s Server Authority solution provides a state-of-the-art foundation for realistic physical interactions, robust networking infrastructure, and anti-cheat systems that prevent client-side exploits so creators can focus on tuning gameplay and creating content rather than building custom solutions.


## The Benefits of Server Authority


Server Authority makes the server the source of truth for game logic and simulation so that no single client can change the outcome for other players until it’s validated. Server Authority is essential for competitive games, but it also improves physical interactions like vehicle collisions or ball physics, and protects from client-side cheats like flings, wall clips, or speed hacks, making it useful even outside of multiplayer environments.


Because it takes time for client inputs to reach the server, Server Authority alone could lead to a sluggish client-side experience while each client waits for validation before executing an action. We mitigate this with client-side prediction and rollback. Each player’s client acts on the player’s actions immediately to maintain a smooth experience, and these actions are treated as predictions until validated with the server. In cases where the client and server state don’t match, the client seamlessly rewinds to the last known good server state and resimulates to catch up. Learn more about how Server Authority works in our[DevForum post](https://devforum.roblox.com/t/full-release-ship-fair-and-competitive-games-with-server-authority/4727993) .


## Server Authority in Action, Featuring ThoughtSpinnr


Roblox creator ThoughtSpinnr is using Server Authority to prototype multiple games, including a space game that pushes the boundaries of Roblox’s physics engine:


“With Server Authority, I get way more control and freedom over my games,” said ThoughtSpinnr. “It raises my quality bar, and it’s such a generational leap for what we can do as creators.”


ThoughtSpinnr’s space game features a real-time action combat system and a custom character system powered by Roblox’s modular character controllers. “The real-time combat system would be dead in the water without Server Authority,” said ThoughtSpinnr. “In a multiplayer combat system, enemies and their state must be owned by the server and replicated down to clients to ensure consistency. If the game waits for a response from the server to validate combat actions, the game will feel highly sluggish. If the game applies changes locally, the version of the enemy’s state on my client will slowly but inevitably drift out of sync with the enemy’s state on the server. That’s not good in either scenario.”


Introducing additional players and mechanics like abilities can compound these issues. Server Authority with rollback and resimulation can achieve responsiveness and consistency simultaneously. “With Server Authority, rollback, and resimulation, these problems are much, much easier to tackle,” said ThoughtSpinnr. “Combat can be fully simulated in its entirety on the client, with each client running hit, stun, and ability logic immediately, and then the client can be rolled back and resimulated if any mispredictions occur.”


ThoughtSpinnr already has plenty of other ideas for Server Authority. “Think of the most annoying problem you face with characters in your game. Now think of it disappearing at the click of a button,” said ThoughtSpinnr. “Cheating? Basically gone. Latency? Drastically reduced. Responsiveness? Instant. Why wouldn’t you turn it on?”


Server Authority unlocks new dimensions of gameplay on Roblox by providing creators with a state-of-the-art foundation for building responsive, consistent, and secure gameplay. Server Authority reflects our commitment to giving creators sophisticated, modern tools for building the next generation of games on Roblox.
