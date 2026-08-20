---
schema_version: "1.0.0"
document_id: "2c587147fd1b864ba4af7c049c15921a5fbcc08f0851c60849f5fbe3fd5129d7"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/split-screen-and-gameshare-networking-in-survival-kids"
published_at: "2025-09-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:7e11f84b78365753588a15da8897edfb5d356485d1c3cd1e48881b4fffc09e4d"
---

# Split-screen and GameShare networking in Survival Kids

This summer, Unity released its first game, created in close collaboration with publisher partner KONAMI.[Survival Kids](https://www.konami.com/games/survival_kids/) is a fun-filled update to the classic kids’ game that launched as a day-one[Nintendo Switch™ 2](https://www.nintendo.com/us/gaming-systems/switch-2/?srsltid=AfmBOoouO_4cokZE_DwyBl65xyxsJF4y6LciVSMZ4n82A7uh06qa44zq) title.


The game was built entirely on[Unity 6](https://unity.com/releases/unity-6) , so the dev team was working with new software toward launching the game on a new platform – a huge challenge. On top of that, the game can be enjoyed in a variety of network configurations, so the small Unity team working on the project had to build a robust multiplayer architecture that would support these options.


Check out the[first instalment](https://unity.com/blog/inside-the-survival-kids-multiplayer-network-infrastructure) of the multiplayer networking story for *Survival Kids* , where we share how the fundamentals behind the game’s network architecture came together. This post expands on this base to show how the team built the game’s split screen and Nintendo Switch 2[GameShare capabilities](https://www.nintendo.com/us/gaming-systems/switch-2/features/gameshare/?srsltid=AfmBOortjz89Jr5beKAI3oUwkMoz2TgxAK9ahfZZhMvn3_6U5XMoU4wn) .


*Nintendo Switch is a trademark of Nintendo.*


Gameplay in Survival Kids


After we’d solved a lot of the problems in the game’s network architecture, we started to think about how we were going to do split screen, which isn’t supplied out of the box in[Netcode for Entities](https://docs.unity3d.com/Packages/com.unity.netcode@1.6/manual/index.html) . This was a different challenge. With split screen, there’s got to be more than one player, but those players belong to a client.


Netcode for Entities assumes that there’s one player per client – if there’s a separate game, with a separate console connecting to it, then it has one player. When that changes and there are actually two players or three players, there’s no way to send the input up for each individual player. They have to be sent up as one.


We effectively created a virtual input player that nobody can see. It’s totally invisible, but it collects all the input for all the local players, up to four of them (although in the end we didn’t do four-player split screen). It manages all the input that comes in, and then it sends all that input up the server every frame.


In the game, players don’t manage their own input. The imaginary virtual input player tells them what the input is for a frame. Previously, Netcode for Entities assumes that a player is responsible for getting its input and using its input to do all its movement, but here there’s this other player that doesn’t do any movement but holds all the input for everything else.


Split screen was the main challenge from a network point of view. To avoid having a multiple cameras problem, we started by having a second player that would run around while the camera stayed with the first player. That came together pretty quickly, but then we encountered other problems, like how to set up a second camera? How to keep one camera on the left of the screen and the other on the right side of the screen? We had to solve UI problems, too, because there’s quite a bit of UI that only one player can see. For example, if one player is in front of a log, they would see a little prompt button that says, “Hey, press X to pick up this log,” but of course you don’t want the other player to see that.


We had to figure out how to hide the UI so that if the other player is nearby, they won’t see it. We used layers for that, but our fix related more to UI than to the network. We had decided that we ultimately wanted to lock the game to two split-screen players for a better gameplay experience – even if it’s on a big screen, there can only be two local players. We could do four on a split-screen internally, and we kept that going for quite a while because it was a great way of stress-testing performance, since every player adds a bit more processing, a bit more rendering, another player to simulate.


Gameplay in Survival Kids


One of the features during development for Nintendo Switch 2 is GameShare. You’re effectively sending a video feed to another console – really, it’s just split screen from a network perspective – except the system sends one camera to another console instead of rendering it on a screen.


Our four-player split screen was the basis for how we approached GameShare mode. We could connect as many players as we want as long as the performance is okay and we can stream video to that console. The main reason we didn’t want to do four-player split screen was just about screen size, really. Unless you have a massive TV, it’s really hard to see the windows – but if you have your own console, the video can stream over to that.


We pushed hard to differentiate from our two-player split screen mode so we could support an extra third player in GameShare. You can have a host and two guests while still offering players a great experience and smooth performance. We weren’t willing to lower our standards on that, but we were still able to use the split-screen architecture to power GameShare.


One really helpful feature that we added was a debug command. We have a dev menu, so you can press a button, call up the menu, and then type commands into it. This was handy because it let us run loads of debug stuff – it’s all compiled out of the final game, so of course nobody could do that in the final game that people buy and play. But one of the modes that we had in split screen was that you could duplicate the main player – this let you have a split screen where one controller runs both players. It was a great way to test the split screen without needing to have loads of controllers around, and this made it easier to test.


The split screen setup also effectively ran all the normal networking code that we did. Since the players were separate from each other, the server would send information to show how the online game works. But it’s also possible to test whether code worked in multiplayer mode without connecting a player to another client by firing up split screen mode with another controller in the Editor to play there. There’s no need to do a new build since it’s possible to test the code on split screen as a proxy for a normal online game.


There were another two Unity tools that we found really useful, although we didn’t use them until right at the end of the project. Unity 6 includes new[Multiplayer Play Mode](https://docs.unity3d.com/Packages/com.unity.multiplayer.playmode@0.1/manual/index.html) tools, which enables us to test without a separate player build.


Opening the Editor, it takes over an hour to do a clean player build because there’s so much art and other information, so testing code with a remote player means waiting at least that long. It’s not particularly good for iterating. But Multiplayer Play Mode enables you to effectively spin up another window, like another virtual version of the Editor, and connect like that.


Netcode for Entities also has Play Mode tools to simulate bad network connections. You can specify and simulate a specific level of ping – say, a 300-millisecond ping, a really horrible round trip to simulate what it would be like to play with a friend who tethered their phone to their laptop in an airport and connected to the game that way. Then you can test that in the Editor to find out how laggy or unstable it is. Sometimes that doesn’t work on a network connection that’s losing data and dropping packets, and we could simulate that easily.


This testing happened all the time. For a while, we had a rule that nobody was allowed to play in the Editor with the simulator turned off – everyone had to play with some kind of simulated lag, since none of our players were going to play on a perfect connection. That way, we could never fool ourselves into believing that a super high-speed office broadband was representative.


In the end, all of this testing paid off – we were able to deliver a smooth, performant game at 60 fps across really different networks and multiplayer setups. Since the game's release a few weeks ago, we've seen players continuing to engage online through[Lobby](https://unity.com/products/lobby) and[Relay](https://unity.com/products/relay) , hopefully enjoying a seamless and robust gaming experience, regardless of their home network conditions.


***Check out the other instalments of our blog series deep dive into* Survival Kids *production: -***[" Graphics and rendering tips from Survival Kids"](https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids)
-["Level layout and terrain workflows in Survival Kids "](https://unity.com/blog/level-layout-and-terrain-workflows-in-survival-kids)
-["Inside the Survival Kids multiplayer network infrastructure"](https://unity.com/blog/inside-the-survival-kids-multiplayer-network-infrastructure)


***To learn more about projects made with Unity, visit the[Resources](https://unity.com/resources) page.***
