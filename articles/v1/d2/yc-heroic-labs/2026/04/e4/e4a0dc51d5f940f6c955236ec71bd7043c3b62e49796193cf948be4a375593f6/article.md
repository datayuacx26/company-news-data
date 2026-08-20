---
schema_version: "1.0.0"
document_id: "e4a0dc51d5f940f6c955236ec71bd7043c3b62e49796193cf948be4a375593f6"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/making-of-pocketbound/"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:8054b645d8cf6a992da2358996fbc81777298e43b4895064921352d263b715cf"
---

# The Making of Pocketbound: A Brackeys Game Jam Story

Given one week to build a game, most developers wouldn’t even think to add multiplayer to their title. There simply isn’t enough time or bandwidth for it. Or is there?


> “This was such a treat. I would have never guessed this was made in a week. Honestly impressed with the amount of polish and the number of features. A level editor?? Are you guys human?” —[Jerrylikesgreen, Itch.io](https://itch.io/jam/brackeys-15/rate/4304774)


Jerrylikesgreen is talking about *Pocketbound* : A charming, hand-crafted puzzle game that sends you chasing after your cat through a dungeon that you can rearrange, reshape, and share with your friends. A love letter to classic 2D pixel games, it uses Nakama and Heroic Cloud to power the social features that players couldn’t stop gushing over.


Players create shape-shifting dungeons in Pocketbound and compete against each other for the best time.


Heroic Labs sponsored[Brackeys Game Jam 2026.1](https://itch.io/jam/brackeys-15) and a few members of our team thought it would be fun to challenge themselves by participating in it, both to indulge in their love of game development and to showcase what our tech can do. I sat down with two of *Pocketbound* ’s developers, Andrés Méndez and Enrique Perez, to hear how it all came together: the creative decisions, finding fun, and the moment they realized they had a meme-worthy game in their hands.


---


### Meet the Team


**[Andrés Méndez](https://www.linkedin.com/in/andras-felipe-mendez-03872657/)** is a self-taught software engineer with 13 years in the games industry and a stint at Unity. He works as a Hiro onboarding engineer at Heroic Labs, building working prototypes from studios’ game design documents.


**[Enrique Perez](https://www.linkedin.com/in/enrique-alejandro-p%C3%A9rez-2818b368/)** spent 13 years in game development—four as a developer and nine as a game designer—across genres ranging from party games to VR. He’s now on the Developer Relations team at Heroic Labs.


**[Fábio Sousa](https://www.linkedin.com/in/dannyisyog/)** is a software engineer and game developer who has been building games and participating in jams since 2020, with a background in game networking and Unity and Godot development. He’s now a Developer Relations Engineer at Heroic Labs.


---


## How did the idea for *Pocketbound* come about?


**Enrique:** The jam theme was “Strange Places.” I’m a big fan of fantasy RPGs and dungeon crawlers, and Andrés is fond of the classic Zelda games, so we started thinking about a very strange dungeon, a place with shifting walls that can change at any moment. That led us to the idea of a mage running through this dungeon who has the power to control the rooms around them. *The Legend of Zelda: Link’s Awakening* , which has a dungeon builder mode, was also a huge inspiration. We thought, what if we center the entire game around creating and rearranging the dungeon?


That’s when the social element clicked. One of the daunting questions early on was: “How do we create enough puzzles to fill a whole game?” And the answer was: we don’t have to. We build a few levels to teach players the design language, and then the community creates the rest.


The in-game level creator: build a dungeon, share it with friends.


**Andrés:** Another keyword that comes to mind is replayability. Features like leaderboards and user-generated content mean your game can live beyond what you initially ship. Players keep coming back. They want to see what their friends made, they want to share what they made, they want to compare scores. It gives the game a life beyond what we as developers could have come up with on our own.


## You knew early on that you wanted to use Nakama to build your game. But what does it mean to make a game “using Nakama”?


**Andrés:** One common misconception is that Nakama is just a game backend. It is, but that narrows your perspective. It’s more like a social network engine with the facade of a game backend. It has so many social features: friends, chat, leaderboards, all the interactions between players. It’s incredibly taxing if you build them yourself from scratch.


**Enrique:** It’s not a mystery that games are better with friends. Playing is an activity that gets enhanced when it involves other people, and that social connection makes games more meaningful. In game jams, there’s usually so little time for backend work that a lot of developers are afraid of it. Leveraging Nakama as that social network engine behind the game was key for us getting to where we got.


## One week, full-time jobs, remote team, and your own lives to take care of… how did you pull it off?


**Andrés:** Bold of you to assume I have a life. That week was tough, I’m not going to lie.


**Enrique:** We were very careful choosing what kind of game to make because we knew we didn’t have much time. Even though a week sounds reasonable, we were still working our 9 to 5 every day. *Pocketbound* was originally going to be a lot bigger, but we kept cutting until we reached a scope we were confident about.


By Friday we still had a lot to do, so we pulled an all-nighter: energy drinks, snacks, the works. We do not endorse this! But real-time communication was crucial. We were in different cities, but being on a call together the whole time, being able to point at things in Figma, that kept us aligned and accountable.


**Andrés:** Nakama was actually the least time-consuming part of the development. I’d say 90% of the time was spent on the game’s UI and UX interactions.


The level selector: browse and play dungeons created by the community.


## What were the biggest challenges you faced?


**Andrés:** UI. And the need to sleep.


**Enrique:** Scope creep. We kept having ideas… imagine a room made of ice where you slide through it! We loved these ideas, but we had to keep pulling back the reins.


**Andrés:** And one thing that caught me completely by surprise: Godot’s web export was giving us a lot of trouble. Depending on the browser, the game would scale differently or behave strangely when opened in a new tab. We only discovered this right around submission time, so we had very little time to deal with it. Lesson learned: always test on your primary platform from day one.


## How did you feel when you first saw people’s reactions to the game?


Player reactions on Itch.io after the jam submission.


**Andrés:** First, relief. Yes, I can sleep now. And then this very warm feeling, seeing friends and players creating levels and challenging each other. Seeing the buzz around a game we made, it’s hard to describe. Our expectations were low. I made the game I wanted to make, I didn’t have expectations about whether other people would get it or like it. Seeing that level of engagement was a positive surprise.


**Enrique:** For me it was very validating. By that point I’d been staring at the game for a week straight; I was seeing green. I could spot every little thing that could have been better. But despite all the corners we cut, people were still having fun. That felt like we’d achieved our goal: making people have a good time.


We also saw people who didn’t know each other before start having conversations around the game. Someone started speedrunning it. We even got memes. That’s how you know you reached the fun.


**Andrés:** It also validated the choice of using Nakama. Of course we were going to use our own tools, but it paid off in seeing real player engagement firsthand. I’m still smiling ear to ear.


## What’s next for *Pocketbound* ?


**Andrés:** I want a second version with Hiro to showcase how much further the game can evolve with very little effort. I also want to export to mobile and test there. Plus a lot of bug fixes. And a couple of new mechanics that I don’t want to reveal right now—they’re a surprise!


**Enrique:** Same as Andrés. I want to update the original campaign levels, think about the level design more thoroughly now that we have the benefit of time. I’d love to see this game on mobile. I was able to get it running on a Steam Deck, and seeing it on a handheld was really exciting. I think this game could do well on those kinds of platforms.


And I want to give a big shout-out to my partner,[Cam Gonzalez](https://camgonzalezart.carrd.co/) , who helped with the animations and cover art. She did a fantastic job. The animation of the mage trying to catch the cat was a big favourite among players, especially when you can pet the cat in the credits!


Cam Gonzalez illustrated the game's cover art and helped with animations.


## Any advice for future jammers who want to add social features to their games?


**Enrique:** Test your game as early as you can. That’s common game jam advice, but with a social game you need to go further. Make sure you can create multiple accounts and test the game between them simultaneously. Check how it behaves when multiple players are interacting at once.


Also, think about the player’s flow through the UI. We had a moment where we realized we didn’t have separate login and create-account screens; both were the same. We had to go back and build that. And people don’t love creating usernames and passwords; it adds friction. Plan for that from the start, and once you do get them past the login screen, get them into the game as quickly as possible.


**Andrés:** And always test on your primary platform from day one. I knew that, and I still fell for it.


---


We’re incredibly proud of what Andrés, Enrique, and Fabio pulled off. What started as a fun challenge turned into something that brought strangers together, sparked conversations, and produced some top-notch memes. While the reception was wonderful, what meant the most to us was seeing the passion and craft our team gave to something purely for the love of games.


It’s this kind of creativity that made sponsoring Brackeys Game Jam such a pleasure. Suffice to say, our team can’t wait to jam again when the opportunity arises. If you’re running a game jam, a community event, or something else that brings developers together, we’d love to hear about it.
