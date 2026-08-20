---
schema_version: "1.0.0"
document_id: "7dc429b6260e621bbfab9d23b76d344904849884bbb54d762e68097ba14bfb2e"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/building-102-million-spell-combinations-prefab-variants-rhell"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:b11d4d2b63c8f3b0b9abafcfd88b0f3fc328ea984c3a86459d274e131968a418"
---

# How solo dev SlugGlove built 102 million spell combinations in Unity for Rhell: Warped Worlds & Troubled Times

***How does a solo developer create a video game with 102 million potential spell combos?*[Rhell: Warped Worlds & Troubled Times](https://store.steampowered.com/app/3247030/Rhell_Warped_Worlds__Troubled_Times/) *is a semi-open-world RPG where players chain magical runes together to cast spells on objects and manipulate their qualities to solve puzzles.***


***Creating this complex system would pose a significant challenge even for a large development team; solo developer[SlugGlove](https://www.youtube.com/c/SlugGlove) looked at it as another puzzle to solve. We sat down with SlugGlove to discuss their journey from animation to programming, their approach to architecting and testing* Rhell *’s magic system in Unity, and their best advice for managing scope and avoiding overwhelm.***


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


## From animation to coding in C#


SlugGlove didn’t set out to be a programmer; their background is in 3D animation and sprite work. While working on the final animation project for their degree, they realized they didn’t want to create an animated short – they wanted to bring their work to life in a video game instead.


SlugGlove picked up Unity and began experimenting while learning C#. For artists who might be intimidated by the idea of making the leap a game engine, they emphasize the user-friendliness of Unity’s[built-in animation suite](https://docs.unity3d.com/Manual/AnimationSection.html) .


“I was using Blender at the time, which I personally find very user-friendly,” says SlugGlove. “Transferring from this to Unity, I was basically making clips and arranging them with the Animator Controller. It was so much easier than traditional animation. If other animators are looking for a solution to test their animations in, Unity is very easy to pick up and figure out.”


What started as a portfolio project led to a couple of experimental games before development began on *Rhell: Warped Worlds & Troubled Times* .


Rhell’s animations in the Animator Controller (top) and in game (bottom)


## Building a combination-based spell system


*Rhell* ’s core hook is its modular magic system. Players can combine up to five runes at a time in Rhell’s spellbook, resulting in millions of potential interactions. Rather than creating unique[ScriptableObjects](https://docs.unity3d.com/Manual/class-ScriptableObject.html) or[prefabs](https://docs.unity3d.com/Manual/Prefabs.html) for every possible combination, SlugGlove found an elegant solution in integers and[prefab variants](https://docs.unity3d.com/Manual/PrefabVariants.html) .


### The integer approach


Rather than making each rune a complex asset, the game simply reads runes as numbers. “Runes themselves aren’t actually a separate asset in the project,” SlugGlove explains. “It’s more like, you press button one, and that is saved as an integer. It goes from zero to 40. Casting a spell reads a list of up to five integers.”


Rather than coding specific interactions for a combination like “Fire + Ice,” the system reads the integers (for example, 11, 12), removes them, and dynamically modifies a localized class object to apply the properties of “Water” (for example, integer 32). The final spell adds and modifies values based on those integers until it produces a final result class. This modular, mathematical approach keeps the game logic lightweight and saves SlugGlove from having to create thousands of individual assets.


Rhell combining Fire and Ice runes into Water


### The universal object system


To keep spells working consistently, SlugGlove used prefab variants. “Every single object in *Rhell* is a variant of one little gray cube,” they explain. “My thinking was, if any spell can work on this one cube, it could work on anything else in the game. I basically made the most complex cube I could.”


Because a barrel, a boulder, and an ocean of lava are technically variants of the same base prefab, SlugGlove only had to implement each spell interaction once. Modifying the base prefab updates hundreds of objects across the game, saving significant development time.


The player character, Rhell, is the one exception to this rule. Because she requires custom movement[controllers](https://docs.unity3d.com/Manual/class-AnimatorController.html) and unique[animation states](https://docs.unity3d.com/Manual/class-State.html) (like bouncing around while on fire), SlugGlove had to meticulously mirror the universal object code onto the player controller.


A few of the various custom animation states for the player character in *Rhell: Warped Worlds & Troubled Times*


## Saving time with randomized testing


On first look, accounting for 102 million spell combinations seems like a testing nightmare. SlugGlove was undeterred, and again found a simple, creative solution: brute-force randomization.


They started by adding a “Random” rune to the game, which randomly chooses among the integers that make up the rune system. To test possible interactions, SlugGlove would load into a room and spam the Random spell at objects until they encountered an error message.


“I would sit in a room, cast Random over and over, until I saw that little red symbol in Unity we all know so well,” they laugh. “There was one situation where, if you made an object Rewind through time, Duplicated it, and made it Sticky, and then tried to destroy the Sticky effect, it would throw a null reference error. Thanks to Random, I knew I needed to fix that.”


The Random rune in action


## The problem with portals


In-game, Rhell can send objects from one location to another via a Portal rune. This introduced another unique challenge to solve. SlugGlove noticed that when an object under the effect of the Grow rune (which enlarges objects) was sent through a portal, it would overlap with surrounding geometry, creating a clipping issue.


An under the effect of the Grow rune creating a clipping issue when sent through a portal


SlugGloveʼs first approach triggered teleportation to fail if an enlarged objectʼs Box Collider detected clipping upon emerging from a portal. However, this wasn’t ideal, as it contradicted their vision for the permissive, sandbox nature of *Rhell* ’s spell system.


“From a game design perspective, it felt terrible,” says SlugGlove. “I wanted to avoid introducing situations that would limit the player and what they could do. It was really unsatisfying to just say, ‘No.’”


Choosing to prioritize player enjoyment over a slightly more realistic physics interaction, SlugGlove wrote a custom collision-resolution script. If an object attempts to exit a portal and detects collision, the script pushes the portal a short distance from the collision point to let the object spawn unimpeded.


An object under the effect of the Grow rune emerging from a portal, post-fix


## Focusing on the process, not the outcome


In addition to sharpening their Unity skills during the development of *Rhell: Warped Worlds & Troubled Times* , SlugGlove also learned FL Studio to compose the game’s soundtrack. We asked how they prevent burnout while juggling so many tasks, all while working on a project of such dizzying scope.


“I just treat it like a new toy,” they say. “If you focus on the, ‘What if I fail?’ or ‘What if the thing I make isn’t good?’, you’re just going to get stressed out. I try to focus on the positive and have fun with it. Worst case, if I couldn’t learn this music program, I could commission a friend. There’s always a backup.”


This positivity shines through in SlugGlove’s words of encouragement for aspiring game devs who might be discouraged by challenges like scope creep and marketability before they ever even open a game engine:


“At the very least, make something you’re proud of, for yourself. Focus on, ‘Am I having a good time doing this?’ Because as long as you’re having a good time, it’s okay. Making video games is really silly and fun. It doesn’t always have to be a stressful thing.”


**[Rhell: Warped Worlds & Troubled Times](https://store.steampowered.com/app/3247030/Rhell_Warped_Worlds__Troubled_Times/) *is out now on Steam. Explore more Made with Unity games on our[Steam Curator page](https://store.steampowered.com/curator/45049063-Made-With-Unity-Official/) , and check out more stories from Unity developers on the[Unity Blog](https://unity.com/blog) and[Resource Hub](https://unity.com/resources) .***
