---
schema_version: "1.0.0"
document_id: "aeb385f60598e9dbb86352cedaa5cb78141ce5c75161f0f1de5bf60ee4e7269a"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/inside-the-survival-kids-multiplayer-network-infrastructure"
published_at: "2025-09-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:a6f0430160275277599cd60ae0642045c64f104a87bce0c5840f882219610872"
---

# Inside the Survival Kids multiplayer network infrastructure

This summer,[Survival Kids](https://www.konami.com/games/survival_kids/) launched as a day-one release for[Nintendo Switch™ 2](https://www.nintendo.com/us/gaming-systems/switch-2/?srsltid=AfmBOoouO_4cokZE_DwyBl65xyxsJF4y6LciVSMZ4n82A7uh06qa44zq) . The game was built entirely on[Unity 6](https://unity.com/releases/unity-6) , marking Unity’s first-ever end-to-end development project, working closely with publisher partner KONAMI.


Developing for a new platform on Day 1 is a huge challenge, but the small internal team that built this project included seasoned Unity developers, many of whom have been working in Unity and on games for decades. This blog is part of an ongoing series diving into how the game was made, how this work fueled Unity’s commitment to[production verification](https://unity.com/releases/unity-6/production-verification) , plus lessons other Unity gamedevs can take and apply to their own projects.


This is the first instalment of an ongoing behind-the-scenes series digging into team lessons from working on *Survival Kids.*


*Nintendo Switch is a trademark of Nintendo.*


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


*Survival Kids* was built by a very small team within Unity. The core group was about 10 developers of various disciplines (artists, engineers, and designers). At our peak, we were around 20 as people from other Unity teams came onboard. For example, Steven, our rendering engineer, worked with us a lot, but he wasn’t always on the project.


As a small team, we had some advantages, though. The engineers were vastly experienced – most of us have been writing games for 20-odd years, mostly in the AAA space, so we’ve learned a lot of lessons and we’ve made a lot of mistakes. And of course we’re really experienced in Unity because most of us have been here for some time.


Some of us have also worked on customer projects as part of Unity support teams like Professional Services/Accelerate Solutions, now[Unity Studio Productions](https://www.gamesindustry.biz/how-unity-developed-its-first-game-in-20-years-as-a-nintendo-switch-2-launch-exclusive) . We advise customers on how to optimize their projects and even embed with project teams to work alongside them and help solve their hard technical problems, so we’re quite well-versed in the mistakes that studios often make and how to fix them. Working on *Survival Kids* , we could architect the project and put it on the right path from the start because we knew where all the pitfalls would be, and that saved us a lot of time and resources.


Today, I want to dig into the game’s network architecture. We used Unity to drive multiplayer networking, and *Survival Kids* offers players a number of different ways to play the game, all from the same networking base. So let’s dive into how this came together, and hopefully some of this can help you in your projects, too.


Gameplay in Survival Kids


*Survival Kids* can be played a few ways: single player, local co-op, and online with friends. On the[Nintendo Switch™ 2](https://unity.com/solutions/nintendo-switch) , players can also use GameShare to stream the video to another Nintendo Switch 2 or even an original Switch, then play multiplayer with someone on the TV or a device, which is really cool.


We wanted our setup to drive all of that and other combinations. For example, you could have two players playing split screen on one television that’s connected to another two players playing split screen on a different TV – so four players using two devices. That flexibility was something that we really wanted to design into the architecture to enable play in lots of different ways.


To do this, we decided on[Netcode for Entities](https://docs.unity3d.com/Packages/com.unity.netcode@1.6/manual/index.html) . Once we’d pitched the concept for *Survival Kids* to KONAMI, we went straight into prototyping to find the fun for our[multiplayer game](https://unity.com/solutions/multiplayer) . We used an existing project as a launch point, one that I’d written previously as a proof of concept for how we could use Netcode for Entities as a backend network, then write a[GameObject](https://docs.unity3d.com/6000.1/Documentation/ScriptReference/GameObject.html) layer on top of it to take advantage of Prefabs and animations. Not everyone on the team had experience working with Entities, so we decided to use GameObjects and[MonoBehaviour](https://docs.unity3d.com/6000.1/Documentation/ScriptReference/MonoBehaviour.html) together.


We also wanted to keep the gameplay logic in GameObjects and MonoBehaviours because they make it really easy to prototype – this setup lets you throw things together and write scripts and download scripts off the internet or use[Asset Store](https://assetstore.unity.com/) packages for prototyping. We wanted that fast iteration and freedom, but we also liked that Netcode for Entities gave us a performant network layer. I’d already used it on a few customer projects and personal research projects, so I knew that its quality level could drive the level of gameplay we wanted.


When we first started, about three years ago,[Netcode for GameObjects](https://docs-multiplayer.unity3d.com/netcode/current/about/) existed, but it still lacked a few of the features we wanted, especially client-side prediction. With client-side prediction, if there’s ever a lag between the server and client, the client predicts what the server is going to do and does it instantly – so players’ controls feel responsive even when there’s lag. You don’t have to wait for the server to tell you that a player has moved or what have you – you’re already doing it. That’s something that Netcode for Entities had from the start.


For prototyping, we basically grabbed a project we already had and jumped in. We started with simple things – picking up objects, chopping down trees – and gradually, we started fleshing out what some of the gameplay would be. We were still prototyping, so we didn’t really worry about code quality too much. We were trying to find the fun and looking at our game pillars, including “survival for everyone.” We wanted a survival game, but we didn’t want it to be super hard or punishing – we were trying to distill what’s really fun and exciting about this genre.


We asked ourselves: What do people love about crafting and resource gathering? What don’t they care about? That helped us define how players get resources, how they move them from one place to another, how they do crafting. We figured that all out by prototyping and iterating quite quickly using GameObjects and MonoBehaviours.


Because we started from that little proof-of-concept demo, we could connect by internet address, right from the word go. It was possible to connect using a computer IP, but we also used Unity’s[Relay](https://unity.com/products/relay) service, which lets you host a game on a Relay server in the cloud. With Relay, anyone can join that game using a join code, and people can connect from home or the office without a VPN or known IP. That meant that we could get into a rhythm of weekly playtests – and we were doing them at work and on our home networks, which let us stress-test our network architecture alongside the gameplay with all kinds of different connection speeds. In the end, we kept Relay in production.


We tried to stay as close to the publicly released packages as possible. If we found a bug in one of the packages, we’d identify it, bring the package locally, and try to fix it. Sometimes we’d go to Slack after and message Unity’s Netcode team to explain the problem and our fix so they could take that and do the PRs – and sometimes get it into the final version. We weren’t involved in the fix necessarily, but by working in a production environment, we found some issues that they hadn’t yet (although sometimes they already had a better fix than whatever we’d come up with, or they’d tell us we’re using it wrong).


Gameplay in Survival Kids


Because we developed this way, remotely through Relay, we didn’t add an offline mode until later, close to release. The offline mode doesn’t open up any network sockets, and it uses something called an in-process driver. It effectively behaves like it’s a network, with a server and a client, but they execute in the same process and communicate with one another. Instead of sending it through the network, they send it directly to the client. It’s called an in-process connection, and it’s very fast because you don’t have to wait for actual bytes to travel across the network, but it goes through all of the same flow as our gameplay does.


Working this way, we didn’t need to code a different version – this is our single-player mode *and* our multiplayer mode. Single player and offline are still a network game, it’s just that we don’t use the network – it all just happens internally.


This basically meant that we had one code architecture that we could use everywhere. The cost of that, though, is that when you’re hosting or on single player, you’re simulating the server and the client, creating a performance challenge to run both at the same time. With dedicated servers, a server might go off and live in a server farm somewhere, so that all you need is what’s called the client, which makes it all look nice and respond to whatever the server’s communicating. But on single-player, since we’re simulating, the game has to do both and can’t just sit off on a dedicated server somewhere.


That ended up being one of our biggest performance challenges, optimizing so that the server and client could sit in the same game, in the same frame, and still hit our 60 frames per second target at a good resolution. That target was really important to us.


***Check out the other instalments of our blog series deep dive into*** **Survival Kids** ***production:***
***-***[" Graphics and rendering tips from](https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids)[Survival Kids"](https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids)
-["Level layout and terrain workflows in](https://unity.com/blog/level-layout-and-terrain-workflows-in-survival-kids)[Survival Kids "](https://unity.com/blog/level-layout-and-terrain-workflows-in-survival-kids)
-["Inside the Survival Kids multiplayer network infrastructure"](https://unity.com/blog/inside-the-survival-kids-multiplayer-network-infrastructure)


***To learn more about projects made with Unity, visit the***[Resources](https://unity.com/resources) ***page.***
