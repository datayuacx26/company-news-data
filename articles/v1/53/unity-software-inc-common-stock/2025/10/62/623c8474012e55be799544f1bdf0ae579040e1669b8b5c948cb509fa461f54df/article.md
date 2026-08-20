---
schema_version: "1.0.0"
document_id: "623c8474012e55be799544f1bdf0ae579040e1669b8b5c948cb509fa461f54df"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/bye-sweet-carole-animation-fluidity-player-inputs"
published_at: "2025-10-09T00:00:00+00:00"
first_seen_at: "2026-08-03T18:06:18.177744+00:00"
fetched_at: "2026-08-05T03:48:44.436222+00:00"
content_hash: "sha256:c1373229fdda24fc29c16a889962c4573baf107cd3356070d76471ee66637d60"
---

# Balancing animation fluidity with player inputs in Bye Sweet Carole

**[Bye Sweet Carole](https://store.steampowered.com/app/2428980/Bye_Sweet_Carole/) ( *launching today) combines classic animated princess aesthetics with horror and the macabre, all brought to life using traditional animation techniques. We interviewed Chris Darill, writer and director at Little Sewing Machine and creator of the* Remothered *series, and lead programmer Alvaro Martinez from Dreams Uncorporated, to learn about the techniques and challenges of making hand-drawn graphics for a real-time environment in Unity.***


Chris Darill describes *Bye Sweet Carole* as a tribute to the animated films he grew up with in the 1990s (as well as their infamously difficult spinoff games). “I’m a faithful supporter of traditional animation,” he says. “ *Bye Sweet Carole* is difficult to define because each chapter is different, but it’s like a mix of old school classic horror games like *Clock Tower* and animated point-and-click games like *Dragon’s Lair* and *The Secret of Monkey Island* .”


This ambitious title features a huge number of traditionally animated assets and visuals, each hand-drawn by a team of 11 professional artists. Lead programmer Alvaro Martinez estimates that around 95% of the art assets in the final game are hand-drawn, with the remaining 5% produced by[shaders](https://docs.unity3d.com/6000.2/Documentation/Manual/materials-and-shaders.html) and[post-processing](https://docs.unity3d.com/6000.2/Documentation/Manual/post-processing-and-full-screen-effects.html) effects.


“Nearly everything in *Bye Sweet Carole* is in real-time, too. It’s pretty rare to find a game in this style with this many frames,” adds Chris. “It might be one of the most complicated games to animate in history!”


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


### Identifying the challenge


Creating a game in this visual style relies on detailed frames and smooth transitions. However, early tests revealed a significant disconnect between graphics and gameplay. While the[animations](https://docs.unity3d.com/6000.2/Documentation/Manual/AnimationSection.html) for different actions looked great, the corresponding[inputs](https://docs.unity3d.com/6000.2/Documentation/Manual/Input.html) didn’t feel natural, resulting in a sluggish and unresponsive experience. Balancing visual fluidity with input responsiveness quickly became a key challenge for the team to solve.


*Bye Sweet Carole* features a few different playable characters at different points in the game. In terms of animations, the main protagonist, Lana Benton (who can turn into a rabbit), is the most complicated, with a full suite of actions and animations both for her human and rabbit forms. The problem was blending all of these different animations these smoothly during transitions between different movements.


“We’re doing animations alternating between 15 and 24 fps, so if you just abruptly transition from one animation to another, it looks really bad,” says Chris. “At the same time, when we used really long animation blends, our gameplay just didn’t feel responsive.”


Bye Sweet Carole | Little Sewing Machine | Dreams Uncorporated | Maximum Entertainment


### Enter the matrix frame


For a game that combines tense platforming and hide-and-seek mechanics – where one wrong move can mean game over – this perceived lag was unacceptable. The team’s creative solution was to develop a system around what they call “the matrix frame.” They began by adapting Lana’s walk and run cycles to include a few similar-looking frames that could act as a bridge between movements. Within these cycles, they designate a specific frame (the matrix frame) that can serve as a universal starting point for other actions.


Under this new system, if the player hits the jump input while Lana is running, the game doesn’t wait for the run cycle to complete. Instead, it jumps to the nearest matrix frame, initiating the jump action from that specific point. This allows the animation to “originate” from a clean, predictable position, drastically reducing perceived input lag for the player. This technique allows for the kinds of abrupt, responsive actions players might need to make in a horror game, without compromising on the visual style.


### Scoping out production


*Bye Sweet Carole* ’s unwavering commitment to traditional animation meant the team needed to establish a rigorous, frame-by-frame workflow for asset creation. Chris outlined how a character animation typically comes together:


**• Roughs/storyboards:** Initial sketches to define the action


**• Cleanup and tweening:** Refining keyframes and adding frames for smoothness


**• Coloring:** Applying the final color palette


**• Exportation:** Preparing the files to be imported into the[Unity Editor](https://unity.com/download)


Asset exportation proved to be the biggest hurdle. For cutscenes with a[fixed camera](https://docs.unity3d.com/6000.2/Documentation/Manual/Cameras.html) , the process was straightforward, but the game’s many dynamic character animations proved more complicated. Each frame had to be exported with a central pivot point to ensure the character moved correctly within the gameworld. This was a meticulous process, especially for larger playable characters like Mr. Baese who had to be cut into pieces and reassembled around the pivot point in-Editor.


Animation concepts for Mr. Kyn, the main antagonist of Bye Sweet Carole


### Tackling performance and optimization


*Bye Sweet Carole* is launching simultaneously on desktop and consoles. Achieving a 60 fps target on most platforms with extremely high-resolution assets required disciplined optimization. Alonzo outlined some key techniques:


**Leveraging[Sprite Atlases](https://docs.unity3d.com/6000.2/Documentation/Manual/sprite/atlas/atlas-landing.html) :** Sprites are organized and packed into atlases based on function. For example, each of Lana’s walking animations have their own atlas. (Alvaro notes that this is standard practice, but was critical for managing memory and ensuring fast rendering with Unity’s 2D Renderer).


**Aggressive[profiling](https://docs.unity3d.com/6000.2/Documentation/Manual/performance-profiling-tools.html) :** Memory overflow was a common problem. Deep profiling helped identify and fix CPU/GPU bottlenecks and memory leaks.


**Asset compression and streaming:** Each Sprite Atlas’s compression algorithm and format is carefully considered. Sound assets are loaded and unloaded as needed using[asset streaming](https://docs.unity3d.com/6000.3/Documentation/Manual/StreamingAssets.html) to minimize their memory footprint.


One of Lana's many sprite sheets in-Editor


### Paying homage to the classics


By balancing technique and creativity with rigorous technical optimization, Little Sewing Machine and Dreams Uncorporated successfully executed on Chris Darill’s vision, blending the nostalgia of classical animation with the snappy and responsive gameplay players expect today.


“I’m really lucky to have been able to work with such talented professionals,” says Chris. “At the beginning, I wasn’t sure this crazy idea would work, but I’m really proud of everyone involved and what we’ve made.”


**Bye Sweet Carole *is[out now](https://store.steampowered.com/app/2428980/Bye_Sweet_Carole/) on desktop and consoles. Explore more Made with Unity projects on our official Steam Curator page. Read more stories from developers on the Unity Blog and Resource Hub.***
