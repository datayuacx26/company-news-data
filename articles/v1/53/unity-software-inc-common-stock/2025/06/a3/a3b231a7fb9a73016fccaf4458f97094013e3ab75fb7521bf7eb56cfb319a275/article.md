---
schema_version: "1.0.0"
document_id: "a3b231a7fb9a73016fccaf4458f97094013e3ab75fb7521bf7eb56cfb319a275"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/making-a-killing-psycasso-2d"
published_at: "2025-06-09T00:00:00+00:00"
first_seen_at: "2026-07-24T19:28:09.103822+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:dea843605162d87abe91c2a185f86bbf7f96e9a5f72012657afc0af0d1e0abdf"
---

# Making a killing: The playful 2D terror of Psycasso®

A serial killer is stalking the streets, and his murders are a work of art. That’s more or less the premise behind *[Psycasso](https://store.steampowered.com/app/3250080/Psycasso/) ®* , a tongue-in-cheek[2D pixel art game](https://unity.com/solutions/2d) from Omni Digital Technologies that’s debuting a demo at Steam Next Fest this week, with plans to head into Early Access later this year. Playing as the killer, you get a job and build a life by day, then hunt the streets by night to find and torture victims, paint masterpieces with their blood, then sell them to fund operations.


I sat down with lead developer[Benjamin Lavender](https://www.youtube.com/watch?v=T4d8_a_iaPc) and Omni, designer and producer, to talk about this playfully gory game that gives a classic retro style and a fresh (if gruesome) twist.


**Let’s start with a bit of background about the game.**


**Omni:** We wanted to make something that stands out. We know a lot of indie studios are releasing games and the market is ever growing, so we wanted to make something that’s not just fun to play, but catches people’s attention when others tell them about it. We’ve created an open-world pixel art game about an artist who spends his day getting a job, trying to fit into society. Then at nighttime, things take a more sinister turn and he goes around and makes artwork out of his victim's blood.


We didn’t want to make it creepy and gory. We kind of wanted it to be cutesy and fun, just to make it ironic. Making it was a big challenge. We basically had to create an entire city with functioning shops and NPCs who have their own lives, their own hobbies. It was a huge challenge.


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


**So what does the actual gameplay look like?**


**Omni:** There’s a day cycle and a night cycle that breaks up the gameplay. During the day, you can get a job, level up skills, buy properties and furniture upgrades. At nighttime, the lighting completely changes, the vibe completely changes, there’s police on the street and the flow of the game shifts. The idea is that you can kidnap NPCs using a whole bunch of different weapons – guns, throwable grenades, little traps and cool stuff that you can capture people with.


Once captured on the street, you can either harvest their blood and body parts there, or buy a specialist room to keep them in a cage and put them in various equipment like hanging chains or torture chairs. The player gets better rewards for harvesting blood and body parts this way.


On the flip side, there’s a whole other element to the game where the player is given missions each week from galleries around the city. They come up on your phone menu, and you can accept them and do either portrait or landscape paintings, with all of the painting being done using only shades of red. We've got some nice drip effects and splat sounds to make it feel like you’re painting with blood. Then you can give your creation a name, submit it to a gallery, then it goes into a fake auction, people will bid on the artwork and you get paid and large amount of in-game money so you can then buy upgrades for the home, upgrade painting tools like bigger paint brushes, more selection tools, stuff like that.


**Ben:** There’s definitely nothing like it. And that was the aim, is when you are telling people about it, they’re like, “Oh, okay. Right. We’re not going to forget about this.”


**Let’s dig into the 2D tools you used to create this world.**


**Ben:** It’s using the[2D Renderer](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@7.1/manual/2DRendererData_overview.html) . The[Happy Harvest](https://assetstore.unity.com/packages/essentials/tutorial-projects/happy-harvest-2d-sample-project-259218?srsltid=AfmBOopPd9PcUAgEHXPSRwP9nYlI3IVOx51Q_v0ZJMcZIDIpggSoBTQa)[2D sample project](https://unity.com/blog/games/happy-harvest-demo-latest-2d-techniques) that you guys made was kind of a big starting point, from a lighting perspective, and doing the[normal maps](https://docs.unity3d.com/6000.1/Documentation/Manual/StandardShaderMaterialParameterNormalMap.html) of the 2D and getting the lighting to look nice. Our night system is a very stripped-down, then added-on version of the thing that you guys made.


I was particularly interested by[its shadows](https://discussions.unity.com/t/2d-light-and-shadow-techniques-with-the-universal-render-pipeline/1641848) . The building’s shadows aren’t *actually* shadows – it’s a black light. We tried to recreate that with all of our buildings in the entire open world – so it does look beautiful for a 2D game, if I do say so myself.


Night scene in Psycasso


**Can you say a bit about how you’re using AI or procedural generation in NPCs?**


**Ben:** I don’t know how many actually made it into the demo to be fair, number-wise. Every single NPC has a unique identity, as in they all have a place of work that they go to on a regular schedule. They have hobbies, they have spots where they prefer to loiter, a park bench or whatever. So you can get to know everyone’s individual lifestyle.


So, the old man that lives in the same building as me might love to go to the casino at nighttime or go consistently on a Monday and a Friday, that kind of vibe.


It uses the A* Pathfinding Project, because we knew we wanted to have a lot of AIs. We’ve locked off most of the city for the demo, but the actual size of the city is huge. The police mechanics are currently turned off, but there’s 80% police mechanics in there as well. If you punch someone or hurt someone, that’s a crime, and if anyone sees it, they can go and report to the police and then things happen. That’s a feature that’s there but not demo-ready yet.


**How close would you say you are to a full release?**


**Omni:** We should be scheduled for October for early access. By that point we’ll have the stealth mechanics and the policing systems polished and in and get some of the other upcoming features buttoned up. We’re fairly close.


**Ben:** Lots of it’s already done, it’s just turned off for the demo. We don’t want to overwhelm people because there’s just so much for the player to do.


Psycasso


**Tell me a bit about the paint mechanics – how did you build that?**


**Ben** : It is custom. We built it ourselves completely from scratch. But I can't take responsibility for that one – someone else did the whole thing – that was their baby. It is really, really cool though.


**Omni:** It’s got a variety of masking tools, the ability to change opacity and spacing, you can undo, redo. It’s a really fantastic feature that gives people the opportunity to express themselves and make some great art.


**Ben:** And it's gamified, so it doesn’t feel like you’ve just opened up Paint in Windows.


**Omni:** Best of all is when you make a painting, it gets turned into an inventory item so you physically carry it around with you and can sell it or treasure it.


**What’s the most exciting part of *Psycasso* for you?**


**Omni:** Stunning graphics. I think graphically, it looks really pretty.


**Ben:** Visually, you could look at it and go, “Oh, that’s *Psycasso* .”


**Omni:** What we’ve done is taken a cozy retro-style game, and we’ve brought modern design, logic, and technology into it. So you're playing what feels like a nostalgic game, but you're getting the experience of a much newer project.


*Check out the[Psycasso demo on Steam](https://store.steampowered.com/app/3250080/Psycasso/) , and stay tuned for more NextFest coverage.*
