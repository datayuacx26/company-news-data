---
schema_version: "1.0.0"
document_id: "87b7b3d1a8b2ced29c60b9bf81d915e7129c7ee73222409fe807da2c03694d51"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/hiro-event-leaderboards-update/"
published_at: "2026-03-20T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:d8832b257d6065b7f18587eccda5703af5ee8574460b78cbebfd837ddba15897"
---

# Race to the Finish: Score-Based Event Leaderboards Come to Hiro

Today we’re releasing a major update to[Hiro Event Leaderboards](https://heroiclabs.com/docs/hiro/concepts/event-leaderboards/) , the biggest change to Hiro’s competitive events system since its launch. Part of Hiro 1.32, this release introduces score-based events, repeatable competition within a single phase, and more ways to customize leaderboards, giving game designers the tools to build fast-paced, repeatable competitive loops that keep players coming back.


## Moving Beyond Timers


The first iteration of Event Leaderboards followed a straightforward path: players join a cohort, compete for the highest score over a fixed time window, and claim rewards when the timer expires. This works well for weekly tournaments and seasonal leagues, but it limits what kinds of experiences you can build.


We heard from our community that you wanted more dynamic event formats: race-to-the-finish challenges, multi-stage tournaments, and competition where players can win and then immediately jump into another round. This is now all possible. You can create events similar to those found in popular titles like:


- **Tycoon Racers** ( *Monopoly Go* ): Teams compete in a three-race championship against similarly skilled opponents, spending event currency to advance on a race track, with medal payouts tied to player rankings in each race.
- **Space Mission** ( *Royal Match* ): A three-stage event where players race to complete consecutive levels without failing; the first player to hit the streak threshold wins the stage and advances, with smaller competitor pools and bigger rewards at each higher stage.
- **Knight’s Race** ( *Homescapes* ): Players race to be the first to clear 15 levels, with top finishers moving into higher-stakes rounds against fresh opponents and increasingly valuable prize pools.


## Score-Based Events


The most exciting new feature with this update is **score-based event leaderboards** . Instead of competing for the highest score over a fixed window, players race to reach a target score. Once the target is hit, the player ends their cohort and can immediately (or after a cooldown) play again, perhaps in a more challenging and rewarding tier.


Enabling this mechanic is simple. In your configuration file, define a` score_target` and Hiro handles the rest. You can further fine-tune cohort completion behavior using these new fields:


- **` score_target_players`** : How many players must reach the target before the entire cohort ends. Set this equal to` cohort_size` for a cooperative feel where everyone can finish, or set it to` 1` for a winner-take-all race.
- **` score_time_limit_sec`** : A per-player time limit from when they join the cohort. When it expires, the player can no longer submit scores.


These fields can be combined with` max_num_score` (maximum score submissions) to create layered completion conditions. The system evaluates all configured conditions and whichever triggers first takes precedence, with the phase timer always acting as the final fallback.


### Terminology


- **Phase** : A single cycle of an Event Leaderboard, which is determined by its schedule. For example, for a weekly event that runs indefinitely and always starts on Monday and ends on Sunday, the phase would be roughly one week. When a phase ends, a new one begins and everything resets.
- **Cohort** : A group of players competing against each other on a shared leaderboard within a phase. Rather than pitting every player against every other player, the system assigns players to small cohorts based on their skill level.
- **Roll** : The act of joining a cohort. Previously, each player rolled once per phase. Starting with Hiro 1.32, players can roll multiple times within a single phase: completing one cohort, claiming rewards, and rolling into a fresh one with new opponents.


## Multi-Roll: Compete, Claim Rewards, Repeat


The second major addition is repeatable roll support via the new` max_rolls` field. Previously, players joined one cohort per phase. Now,` max_rolls` controls how many times a player can roll (join a cohort) within a single phase.


` max_rolls` Behavior


` 0` or` 1` One cohort per phase, identical to previous behavior


` N` One initial roll plus (N-1) re-rolls within the phase


The game loop becomes: roll into a cohort, compete, complete the cohort, claim rewards, and roll again. For added convenience, the` RollEventLeaderboard` API now accepts a` claimReward` parameter that claims the current reward and rolls into a new cohort in a single call.


## More Types of Competitive Play


Put these tools together and you can build the kind of fast-paced, repeatable competitive loops that players in some of the world’s most popular mobile games already love — events that reward skill, create urgency, and give players a reason to come back again and again.


For a full overview of all the new properties, along with examples of popular game designs and how to configure them, head over to the[Event Leaderboards documentation](https://heroiclabs.com/docs/hiro/concepts/event-leaderboards/) .


### Event Leaderboards for Teams


In addition, these latest updates to Event Leaderboards are also available for Teams! This means group vs group events can also enjoy all the possibilities that the multi-roll mechanic enables. See the[Team Event Leaderboards](https://heroiclabs.com/docs/hiro/concepts/teams/event-leaderboards/) guide for more details.


## Get Started with Event Leaderboards


Hiro 1.32 is available now. If you’ve been curious about how Hiro can take your game to the next level, our team is always happy to chat and walk you through a demo. We’re excited to see what kinds of competitive experiences you build with these new tools.
