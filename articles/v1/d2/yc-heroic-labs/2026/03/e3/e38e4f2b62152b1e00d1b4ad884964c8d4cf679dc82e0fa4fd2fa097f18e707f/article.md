---
schema_version: "1.0.0"
document_id: "e38e4f2b62152b1e00d1b4ad884964c8d4cf679dc82e0fa4fd2fa097f18e707f"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/announcing-mage-mayhem/"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:f708cc4aed44c01021241afc9beef4dd697c833c2854cde6c8b2660f3fc73774"
---

# Announcing Mage Mayhem: A Playable Hiro + Nakama Demo Game

We’re pleased to announce the release of Mage Mayhem, a fully playable arena brawler built with Unity 6, Nakama, and the Hiro GDK.


In Mage Mayhem, players fight hordes of goblins in a timed arena, score points for how many kills they get, and earn gold to purchase gear for their battlemage. Simple by design, Mage Mayhem puts Hiro’s metagame systems to work inside a complete gameplay loop: achievements, leaderboards, economy, and rewards, all while running on Heroic Cloud.


## What You’ll See in Action


Along the way, players complete quests and unlock achievements, compete on daily leaderboards for the fastest clear times, and build win streaks for increasing rewards. They can also spin a reward wheel powered by regenerating energy, forming a complete and replayable progression loop.


Here’s how each Hiro system shows up in the game:


- **Inventory** : Manages collectible and equipable gear (hats and swords) that players can equip to directly modify their combat stats.
- **Economy** : Handles gold, shop purchases, and reward payouts, securely validating transactions and updating the player’s wallet.
- **Achievements** : Tracks milestone based objectives like enemy kills and dungeon completions, rewarding players when thresholds are reached.
- **Leaderboards** : Ranks players by their best daily arena clear time, resetting each day to drive competition.
- **Streaks** : Rewards consecutive arena wins with increasing bonuses, resetting on a loss or at daily reset.
- **Energy** : Limits how often players can spin the reward wheel, regenerating over time to encourage repeat sessions.
- **Stats** : Records lifetime player metrics (such as wheels spun or performance data) to support progression and system updates.


Every system works together as it would in a production game, giving you a reference point you can build from rather than starting from scratch.


## Download the Sample Project


Mage Mayhem is hosted by us on Heroic Cloud, so after downloading the game you can run it immediately in Unity.


**[Download Mage Mayhem](https://github.com/heroiclabs/mage-mayhem) to explore the complete implementation and see Hiro in action.**


## Learn More


Mage Mayhem is also a learning resource. The repository includes the server side Hiro configuration files that contain all the metagame rules and behaviours.


For a deeper dive, **check out the Mage Mayhem[getting started guide](https://heroiclabs.com/docs/sample-projects/games/mage-mayhem/index.html)** .


Want to talk to our team about how Hiro can take your game to the next level? Connect with us and we’d be happy to set up a demo.


**Happy goblin hunting!**
