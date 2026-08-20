---
schema_version: "1.0.0"
document_id: "c71701c6bce9bba8a2f7c8236d7c2269ac374e42fb09757cd056d09a9e53c267"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/playtonic-yooka-replaylee-a-fresh-coat-of-paint"
published_at: "2025-11-26T00:00:00+00:00"
first_seen_at: "2026-08-03T18:06:18.177744+00:00"
fetched_at: "2026-08-05T03:48:44.436222+00:00"
content_hash: "sha256:7bacb3d67cfd68b2dc1d372e98f349ae1b90f129d0090af06ae5a8fcb4709343"
---

# Yooka-Replaylee: A fresh coat of paint

***In this guest post, Playtonic concept artist Phoebe Wiggin offers a detailed look at the art and technical process behind*[Yooka-Replaylee](https://store.steampowered.com/app/2448020/YookaReplaylee/) *. Using[Unity 6](https://unity.com/download) , the team focused on evolving the game's visuals while carefully preserving the character and design of the 2017 original.***


*Yooka-Laylee* first launched almost a decade ago, built in Unity 5.4.1 by a small team of former Rare developers as an homage to classic 3D platformers like *Banjo-Kazooie* . Now, over eight years later, Playtonic have returned with *Yooka-Replaylee* , a retelling of the dynamic duo’s daring adventure.


Unlike a typical remaster, *Replaylee* involved rebuilding much of the game from the ground up in a far more modern engine. Moving the project into[Unity 6](https://unity.com/download) (6.0.46 to be specific) meant revisiting every shader (the original release relied heavily on Shader Forge, which is no longer supported). Our artists rebuilt them using[Amplify](https://assetstore.unity.com/packages/tools/visual-scripting/amplify-shader-editor-68570?srsltid=AfmBOoq71j1WK2iYQEwN8dQJ2iXgNKtbpKwfwbQEPtdfiFaqH21QKYuX) and Unity’s[Shader Graph](https://docs.unity3d.com/6000.2/Documentation/Manual/shader-graph.html) , supported by a custom tool created in-house to remap legacy Shader Forge parameters to their new equivalents. This preserved as much of the original material setup as possible and saved a huge amount of development time.


Texturing also stepped up a level, with the team leaning far more on Adobe Substance 3D Painter and Substance Designer than they were able to during the 2017 release. Working as a larger team with updated tools, *Yooka-Replaylee* has been given a fresh coat of paint for both returning players and newcomers alike.


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


### 3D art and lighting improvements


*Yooka-Laylee* 's rendering received a complete overhaul. We ported from the Built-in Render Pipeline to the[Universal Render Pipeline (URP)](https://docs.unity3d.com/6000.2/Documentation/Manual/urp/urp-introduction.html) as it gave us much more power to add some of the new graphical effects we incorporated, such as our drop shadow system, interactive fog, snow and water, volumetric effects, and grass. It also allowed us to take advantage of[Visual Effect Graph](https://docs.unity3d.com/6000.2/Documentation/Manual/VFXGraph.html) , all coming together to make *Yooka-Replaylee* feel more alive and dynamic.


Yooka-Replaylee | Playtonic Games


We used[lightmaps](https://docs.unity3d.com/6000.2/Documentation/Manual/Lightmappers.html) , and a mixture of baked and real-time lighting as it offered a good mix of performance and fidelity. This enabled us to achieve realistic[specular](https://docs.unity3d.com/6000.2/Documentation/Manual/shader-NormalSpecular.html) highlights on[materials](https://docs.unity3d.com/6000.2/Documentation/Manual/Materials.html) from the real-time lighting, and moving shadows on animated objects and materials like moving platforms and trees.


Yooka-Replaylee | Playtonic Games


Talking to technical artist Harry Bentley about the improvement in shaders and performance, he stated that, “For me at least, the grass was probably the biggest technical project – we started off using RenderMeshInstancedIndirect, and went through a lot of iterations working on improving performance, then eventually ended up using the[BatchRenderGroup API](https://docs.unity3d.com/6000.2/Documentation/Manual/batch-renderer-group.html) , which is what we're currently using, and uses the Unity[Jobs system](https://docs.unity3d.com/6000.2/Documentation/Manual/job-system-jobs.html) .


We break up the areas of the level that use grass into a grid, then subdivide it until we have small squares that we use for[culling](https://docs.unity3d.com/6000.2/Documentation/Manual/OcclusionCulling.html) – only rendering the grid cells that are in the camera frustum. We also cull based on distance from the camera, and the shader also fades out the material to the same distance so you don't see any harsh pop-in.”


Side-by-side comparison of Yooka-Laylee (2017) and Yooka-Replaylee (2025) | Playtonic Games


### Props and easter eggs


Q.U.I.D.S – *Yooka-Replaylee* ’s in-game currency – were originally designed as silver coins. Back then, the classic Quill collectables were always gold, so the Q.U.I.D.S needed a contrasting colour to make them feel like their own thing. As development went on, Quills were updated to use unique colours per level, which freed us up to make the Q.U.I.D.S a bold gold instead. The switch massively improved how readable they were in gameplay. This prop is a little nod to that early silver design.


Yooka-Replaylee | Playtonic Games


### New areas


We created a whole new introduction for *Yooka-Replaylee* . This allowed us to take advantage of an opportunity to add narrative elements to both the environment and cutscenes using Unity’s[Timeline](https://docs.unity3d.com/6000.2/Documentation/Manual/com.unity.timeline.html) tools. Being able to mix 2D assets with 3D animation and camera effects really pushed the quality of our introductory cutscene.


Yooka-Replaylee | Playtonic Games


Cave scenes call for a bunch of atmospheric lighting and fog, setting the scene for a fun adventure. 3D artists worked with game designers to create a mystical-feeling tutorial cave, bringing this environment to life by injecting some narrative- and gameplay-friendly props. Working in a modular fashion with kits of custom rocks, foliage, fencing and décor, a whole new area was created.


Yooka-Replaylee | Playtonic Games


### Concept art and new enemies


With new enemies replacing the old Corplet Drones and updated visuals for Bizzy Bees, *Yooka-Replaylee* brings the enemy variety forward to a more modern audience with cleaner silhouettes enhancing the gameplay experience. Taking inspiration from other popular platforming titles, we focused on expanding the world of Quack’s Q Corp inventions and the Capitalist Bee theme. It was essential to make new enemies feel like they simply belonged in this universe and added an element of fun.


Concept art from Yooka-Replaylee by Playtonic Games


Concept art from Yooka-Replaylee by Playtonic Games


### Character art


One of the most significant upgrades players will notice in this remake is the visual improvements we’ve made to the characters throughout the game ( *Laylee has fur now?!* ) Improved textures and rigging also help our characters feel more vibrant, but making this all happen was no easy task!


To shed some light the thought process behind some of these changes, I spoke to character artist and Rare alumnus Steve Mayles about his work in *Yooka-Replaylee* .


**“What was the biggest change in terms of character art?”**


The addition of fur is probably the most noticeable change (although it is only on four characters, it’s front-and-center on Laylee). This adds some extra surface variety and really lifts the perception of quality. I admit to groaning inwardly when a furry bat was requested, as I knew it would be a bit of a pain, but it’s been more than worth it!


Original character design for Yooka-Laylee by Playtonic Games


Updated character design for Yooka-Replaylee by Playtonic Games


General changes across all characters include a higher polygon count and brand-new, higher resolution textures. I didn’t take a ‘one size fits all’ approach; some characters needed more work than others. Character A might only need some extra edge loops adding if the existing topology was sound, while Character B might need whole sections rebuilding. I treated each character individually. In general, *Yooka-Replaylee* ’s characters have three times the number of polygons than in the original game – our new hi-res textures deserve the extra geometry! UV’s were completely redone, and nothing was used from the old textures, with everything created from scratch in Substance Painter.


Add to this a more robust shader with a variety of new settings to tweak to add extra vibrancy and our cast of characters is looking better than ever!


Yooka-Replaylee | Playtonic Games


**“What was the most challenging part of updating pre-existing characters?”**


There are quite a lot of characters for one person to remaster, so it was important to keep an eye on how long each one would take. This was actually the first time I’ve worked on characters I didn’t create myself, so it was an interesting process – I wasn’t sure how each model would look until I loaded them up in Maya. Fortunately, the original characters were already great designs, so there was no need to move them too far away from their original incarnations – just a tweak here and there in the proportions and design as I saw fit.


Where I could, I tried to keep proportions the same so if animations needed to be reused, we’d have that option. You’ll see from playing the game most characters got lovely new animations, but at one point that wasn’t a given.


Whereas the original models followed the usual hi-res to low-res baking process, I didn’t have time (or the desire!) to create hi-res models for each remastered character and found I didn’t need to. Some pieces of geometry had a hi-res bake, but most didn’t – I found with the extra polygons and texturing flexibility of Substance Painter I could re-use the low-res to bake maps and apply details with great results.


Original character design for Yooka-Laylee's friendly vendor, Vendi


Updated character design for Yooka-Replaylee's friendly vendor, Vendi


**”Who was your favourite character to remake and why?”**


Any character where I could make a difference that players would appreciate! While adding fur isn’t a favorite activity of mine, it really transformed the look of the Yeti minion, and that was really satisfying to see the end result of.


I enjoyed working on Schell as she is the only real human character, so something a bit different, but I wouldn’t want to work on human characters all the time (it would get dull very quickly). I’m lucky to get to work on such a wide variety of wacky characters, where each character is an exciting new challenge.


It's hard to choose a favorite, but I’ll go with the Alien Minion. He’s such a simple, fun design! The original was constrained by having to share a skeleton and animations with the other minions, but the remastered version benefits from a unique skeleton and animations to give him his own personality. Plus, he’s got a big exposed squishy brain… I’d like to see more exposed brains in character design, please!


Alien Minion from 2017's Yooka-Laylee (left) and 2025's Yooka-Replaylee (right) | Playtonic Games


### Final thoughts


With the recent release of *Yooka-Replaylee* , we are delighted to see fans of the lizard/bat duo exploring the world and noticing all the changes we made to improve the player experience. *Yooka-Replaylee* is out now on PC and the latest generation of consoles.


***Read more stories from the community on the[Unity Blog](https://unity.com/blog) , and explore additional content on the[Resource Hub](https://unity.com/resources) . Follow our[official Steam Curator page](https://store.steampowered.com/curator/45049063-Made-With-Unity-Official/) for updates on the latest games made with Unity.***
