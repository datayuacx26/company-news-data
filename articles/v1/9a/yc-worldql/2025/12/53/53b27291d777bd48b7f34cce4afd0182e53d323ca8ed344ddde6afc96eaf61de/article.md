---
schema_version: "1.0.0"
document_id: "53b27291d777bd48b7f34cce4afd0182e53d323ca8ed344ddde6afc96eaf61de"
company_key: "yc-worldql"
company: "WorldQL"
source_id: "yc-worldql-news-import-5676989bd5ee"
canonical_url: "https://worldql.com/posts/2025-12-03-openmonsters/"
published_at: "2025-12-03T00:00:00+00:00"
first_seen_at: "2026-07-26T05:40:46.354698+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:1031b31201a39d41600bd1dd3338ee804c7666ad0cfbbeb7db26937d8ebd2961"
---

# Introducing OpenMonsters, an open-source multiplayer dungeon for AI evaluation and training

# Introducing OpenMonsters, an open-source multiplayer dungeon for AI evaluation and training


Dungeons designed to test and improve AI's intelligence!


Wednesday, December 3, 2025


Posted by Jackson Roberts


---


Pictured: LLMs ponder a simple keys+doors task


Games are great tools for training and evaluating AI. From[simple neural networks that play Mario](https://www.youtube.com/watch?v=qv6UVOQ0F44) to multi-modal LLMs that can[perform whole playthroughs of complex games](https://www.twitch.tv/claudeplayspokemon) .


However, these environments are difficult to create and often impossible to reproduce without pirating games, which limits their usefulness for training and benchmarking. That's why we're building OpenMonsters, an extensible open-source multiplayer game for AI training and evaluation!


## Features and Motivation


Our goal with this release is to showcase Dreamlab, our open-source multiplayer game engine which can be used to quickly create games for AI evaluation and reinforcement learning. OpenMonsters is an extensible foundation for building environments for RL/evaluation. It features:


- Working multiplayer with 100+ agents/players per server
- MCP interaction support
- Grid based world for creating puzzle dungeons
- Text-based and image-based vision support
- NPCs / Chat dialogue
- (Coming soon) Turn-based combat system


## Showcase


This initial release has a key+door puzzle, a task where you have to blow up walls with bombs, and a sokoban-style block pushing game.


### Lvl 1: Doors and Keys


In this challenge, you must collect keys that are match a certain door then reach the end of the room. Humans can complete this extremely easily.


The following system prompt is provided for this level:


> Escape the room by reaching the !
>
>
> When you observe, you will get a grid representing the level:
> @ represents your player
> W represents walls
> F represents floor
> E represents enemies (they can move)
>
>
> You can move in any direction but cannot move through walls.
>
>
> Other characters represent mystery things you will have to discover. Move around, use your tools, solve the puzzle!
>
>
> Think step-by-step about the level and the path you have to take.
>
>
> The world progresses one time tick every time you move.


The AI models have a surprisingly hard time with it, often randomly moving about the room. However, once they reach a key (which gives them the message stating it unlocks a door), the models quickly realize what they need to do:


However, with larger levels, the model would get confused even with very specific prompting. Even frontier models would get confused and be unable to identify the route to a door after picking up a key.


This level has indicated interesting gaps in models' ability to reason spatially.


### Lvl 2: Bombs!


This level is similar and the system prompt is modified to tell the model about bombs and how they are represented textually. Players must blow up the wall to progress.


Frontier models tolerated this task well and were able to complete many simple variations of this puzzle. One funny behavior is that they moved backwards after placing a bomb, which was not a requirement as bombs do not damage players. However, the models prior world knowledge caused them to make the judgement they should move back from the explosion!


### Lvl 3: Sokoban


We implemented a very traditional Sokoban room. Claude Sonnet 4.5 makes quick work of it:


While Sokoban puzzles are the hardest for humans, they are the easiest for AI. This is likely due to Sokoban puzzles being present in the training data.


### More coming soon!


We're working on turn-based combat mechanics + are interested in working with the community to come up with more challenges!


## 🦾 Benchmark Results + Surprising Dark Horses!


We scored some models on their ability to reach certain objectives across the keys+doors task.


There was some surprising behavior with models like gpt-oss-120b. The model would produce very long, nonsensical reasoning chains and then refuse to use its tools. As you can see in this video, it would not make any calls during the run and we would have to manually prompt it to use its tools. However, once it did, it shot right to the exit, producing a score on-par with Claude Opus 4.5!


The scoring methodology was as follows:


- Each key: **+25 points**
- Each door: **+25 points**
- Finish level: **+50 points**
- Per move: **−0.5 points**


**Total score formula:**


> Total score = (2 keys + 2 doors + finish) − (moves × 0.5)


Example for **70 moves** :


(2×25)+(2×25)+50−(70×0.5) = 115


- **Realistic good score:** 100–130
- **Human average score:** 137
- **Max moves per level:** 300, because once you pass 300 moves you’ll always receive 0 total score.


Here's a work-in-progress leaderboard of a few models, with more to be added very soon. This was the best of four attempts:


1. 👑 Gemini 3.0 Pro - 136. 2:32 wall clock time.
*Needed to be manually prompted to use its tools.*
2. 🥈 Opus 4.5 - 130 points. 1:11 wall clock time
3. 🥉Sonnet 4.5 - 126 points. 3:25 wall clock time
4. GPT 5.1 ("effort": "high") - 124 points. 4:15 wall clock time
*Needed to be manually prompted to use its tools after outputting a very long thinking trace.*
5. Haiku 4.5 - 95 points. 1:43 wall clock time


We evaluated several other models, but tool calling reliability prevented them from completing the benchmark. DeepSeek v3.2 in particular, despite having good strategy in its thinking traces, was not able to call the tools reliably even with prompting. We are going to develop an alternative calling convention inspired by[Code Mode](https://blog.cloudflare.com/code-mode/) to allow these models to interact with the environment more reliably.


# Get Involved!


The source code and instructions for running OpenMonsters is available at:[https://github.com/WorldQL/worldql](https://github.com/WorldQL/worldql)


- Want to help create new levels and environments for AI evaluation/training?
- Interested in fine tuning open-weights models to have better spatial reasoning and computer use capabilities?
- Are you an educator looking for a great hands-on way to test and improve AI in the classroom?
- Work at a lab and are looking for training data or RL environments?


If you answered "yes" to any of the above,[join our brand new Discord server](https://discord.gg/nPWVJzZFnP) or[send us an email](https://worldql.com/cdn-cgi/l/email-protection#d8bbb7b6acb9bbac98afb7aab4bca9b4f6bbb7b5) and we'll get back to you very quickly!
