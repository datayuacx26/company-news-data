---
schema_version: "1.0.0"
document_id: "c72c4a47249087ebf8136dfc51c1f2eccc035989c58e147c5d3d8e2aadd1f0c7"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/multiplayer-stack-visionary-realms-mmorpg-pantheon"
published_at: "2025-06-02T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:40.457159+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:16df3a983c447358b76f9f31db874cd0a493bcecea848a65c40dca03abf17d0a"
---

# The multiplayer stack behind MMORPG Pantheon: Rise of the Fallen

Finding your own path is at the core of gameplay in *[Pantheon: Rise of the Fallen](https://www.pantheonmmo.com/)* – players can go anywhere, climb anything, forge new routes, and follow their curiosity to find adventure. It’s not that different from how its creators, Visionary Realms, approaches building this MMORPG – they’re doing it their own way.


Transporting players to the fantasy world of Terminus, *Pantheon: Rise of the Fallen* harkens back to classic MMOs, where accidental discovery wandering through an open world and social interactions with other players are at the heart of the game experience.


Creating any multiplayer game is a challenge – but a highly social online game at this scale is an epic quest. We sat down with lead programmer Kyle Olsen about how the team is using Unity to connect players in this MMORPG fantasy world.


**So what makes *Pantheon: Rise of the Fallen* unique compared to other MMO games?**


It’s definitely the social aspect. **** You have to experience the world and move through it naturally. **** It can be a bit more of a grind in a way, but it I think connects you more to your character, to the game, and the world instead of just sort of teleporting everywhere and joining LFG systems or just being placed in a dungeon. You learn the land a bit better, you have to navigate and you use your eyes more than just bouncing around like a pinball from objective to objective, following quest markers and stuff. It’s more of a thought game.


**How are you managing synchronization between the player experience and specific world instances?**


We have our own network library we built for the socket transport layer called ViNL. That’s the bread and butter for all of the zone communications, between zones and player to zone. SQL server in the back end, kind of standard stuff there. But most of the transports are handled by our own network library.


Pantheon: Rise of the Fallen by Visionary Realms


**How do you approach asset loading for this giant world?**


We’ve got a step where we bake our continents out into these tiles, and we’ve got different backends that we can plug into that. We’ve got one that just outputs standard Prefabs, and we’ve got one that outputs[subscenes](https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/conversion-subscenes.html) that we were using before[Unity 6](https://unity.com/releases/unity-6) , and then we’ve got actual full-on Unity scenes that you can load additively, so you can choose how you want to output your content. Before Unity 6, we had moved away from Prefabs and started loading the DOTS subscenes and using that, built on BRG.


We also have an output that can render directly to our own custom batch render group as well, just using scriptable objects and managing our own data. So we’ve been able to experiment and test out the different ones, and see what yields the best client performance. Prior to Unity 6, we were outputting and rendering the entire continent with subscenes, but with Unity 6 we actually switched back to using Prefabs with Instantiate Async and Addressables to manage everything.


We’re using the[Resident Drawer](https://unity.com/roadmap/2602-gpu-resident-drawer) and[GPU occlusion culling](https://discussions.unity.com/t/gpu-dynamic-upload-culling/1542638) , which ended up yielding even better performance than subscenes and our own batch render group – I’m assuming because GPU occlusion culling just isn’t supported by some of the other render paths at the moment. So we’ve bounced around quite a bit, and we landed on Addressables for managing all the memory and asset loading, and regular Instantiate Prefabs with the GPU Resident Drawer seems to be the best client-side performance at the moment.


**Did you upgrade to Unity 6 to take advantage of the GPU Resident Drawer, specifically?**


Actually, I really wanted it for the occlusion culling. I wasn’t aware that only certain render paths made use of the occlusion culling, so we were attempting to use it with the same subscene rendering that we were using prior to Unity 6 and realizing nothing’s actually being culled. So we opted to switch back to the Prefab output to see what that looked like with the Resident Drawer, and occlusion culling and FPS went up.


We had some issues initially, because Instantiate Async wasn’t in before Unity 6, so we had some stalls when we would instantiate our tiles. There were quite a few things being instantiated, but switching that over to Instantiate Async after we fixed a couple of bugs we got rid of the stall on load and the overall frame rate was higher after load, so it was just a win-win.


**Were there any really remarkable productivity gains that came with the switch to Unity 6?**


Everything I've talked about so far was client-facing, so our players experienced those wins. For the developer side of things, the stability and performance of the Editor went up quite a bit. The Editor stability in Unity 6 has gone up pretty substantially – it’s very rare to actually crash now. That alone has been, at least for the coding side, a huge win. It feels more stable in its entirety for sure.


**How do you handle making changes and updates without breaking everything?**


We build with[Addressables](https://unity.com/how-to/simplify-your-content-management-addressables) using the labels very heavily, and we do the Addressable packaging by labels. So if we edit a specific zone or an asset in a zone, or like a VFX that’s associated with a spell or something like that, only those bundles that touch that label get updated at all.


And then, our own content delivery system, we have the game available on Steam and our own patcher, and those both handle the delta changes, where we’re just delivering small updates through those Addressable bundles. The netcode requires the same version to be connected in the first place, so the network library side of that is automatically handled in the handshake process.


Pantheon: Rise of the Fallen by Visionary Realms


**What guidance would you give someone who’s trying to tackle an MMO game or another ambitious multiplayer project?**


You kind of start small, I guess. It's a step-by-step process. If you’re a small team, you You start small. It's a step-by-step process. If you’re a small team, you can’t bite off too much. It’d be completely overwhelming – but that holds true with any larger-scale game, not just an MMO.


Probably technology selection – making smart choices upfront and sticking to them. It’s going to be a lot of middleware and backend tech that you’re going to have to wrangle and get working well together, and swapping to the newest cool thing all the time is not going to bode well.


**What’s the most exciting technical achievement for your team with this game?**


I think that there aren’t many open world MMOs, period, that have been pulled off in Unity. We don’t have a huge team, and we're making a game that is genuinely massive, so we have to focus on little isolated areas, develop them as best we can, and then move on and get feedback.


The whole package together is fairly new grounds – when there is an MMO, it needs to feel like an MMO in spirit, with lots of people all around, doing their own thing. And we’ve pulled that off – I think better than pretty much any Unity MMO ever has. I think we can pat ourselves on the back for that.


***Get more insights from developers on[Unity’s Resources page](https://unity.com/resources)* *and here on the[blog](https://unity.com/blog) . Check out*[Pantheon: Rise of the Fallen](https://store.steampowered.com/app/3107230/Pantheon_Rise_of_the_Fallen/) *in Early Access on Steam.***
