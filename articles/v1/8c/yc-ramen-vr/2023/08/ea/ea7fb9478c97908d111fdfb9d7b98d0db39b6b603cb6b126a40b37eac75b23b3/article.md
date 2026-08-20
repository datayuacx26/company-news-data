---
schema_version: "1.0.0"
document_id: "ea7fb9478c97908d111fdfb9d7b98d0db39b6b603cb6b126a40b37eac75b23b3"
company_key: "yc-ramen-vr"
company: "Ramen VR"
source_id: "yc-ramen-vr-atom-8d2a75475800"
canonical_url: "https://zenithmmo.com/blogs/news/the-citadel-update"
published_at: "2023-08-25T21:09:09+00:00"
first_seen_at: "2026-07-25T20:28:01.905287+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:099c80f21a238f502fb588cf4c61583ece02b19ae50eea2c116ab811a504e981"
---

# The Citadel Update

# Resolved Issues


- Fixed public event HUD timers sticking around for too long
- Fixed player abilities being unequipped randomly
- Potential fix on some sub-accounts on shared quest headsets being unable to play the game due to an unpack error
- Fixed a bug with Cyber Ninjas' Assassin Strike ability that allowed unintended advantages. This has been resolved as part of ongoing efforts to ensure a balanced gameplay experience. In addition, we are making progress in prototyping the[new skill tree system](https://zenithmmo.com/blogs/news/zenith-godstone-revamp) and will share more once we have an update.


# Hotfix 1


- Fixed Sae Jiko being invisible for some players
- Fixed rewards not dropping for events
- Fixed Sae Jiko getting staggered while doing the pillar jump
- Fixed Citadel arena pillars staying up when the event is failed
- Improved shard stability (decreased memory usage and better performance on the client side)
- Tuned Sae's health and scaling difficulty. She was previously able to get 320% health (!) in very large groups
- EM Essotech Weapons incorrectly dropped Legendary modifiers for a very small set of users. This will be corrected on next login where the modifier will be rerolled with the appropriate one


# Update 1


- Added respawn points all across Skyland to fix some players respawning in Citadel Arena
- Fixed Kojika staying sad after petting them
- Removed collision on hookshot anchors in the Citadel Arena
- Fixed collision on geo in the Citadel Arena
- Removed scaling from Sae Jiko and Arboruht based on player count. Sae Jiko WB now has slightly higher base HP/Damage, and Arbhorout doesn't scale beyond the previous base HP/Damage
*Developer Note (9/6): Since the launch of 1.3.3, the team has been closely monitoring the sentiment regarding the scaling of the Sae Jiko world boss. Specifically, we've noted the wedge that's been driven between some early game and end game players.*


*Our goal for this boss has always been to bring the community together to tackle a challenging foe, and the current scaling implementation has clearly been shown to hamper that experience. As a first step to better align our vision for the fight with the actual event, we're removing scaling based on player count and have opted to increase Sae's base health to compensate. To ensure parity across our two World Boss events, the same scaling system has been disabled for Arboruht. We appreciate the passionate feedback, and look forward to more as you playtest the new changes on prerelease*


- Increased cooldown on Sae Jiko's healing and jumping on pillars ability
- Increased Sae Jiko's Lingering AoE visuals to match damage area
- Increased radius for player participation in Sae Jiko WB event
- Increased Sae Jiko's event preamble to 11:50 (from 8:30) and decreased cooldown to 10s after event ends (from 2 mins)
- New event timer for Sae Jiko world boss displayed in the hub in the Skyland area
- Improvements to memory allocations so players encounter less "Freeing Memory" instances
- Performance improvements across all raid instances
- Fixed the double quest title, added in progress text for pet/weapon quest "Claim Kojika Pet" and "Essotech Weapon"
- Adjusted the Essotech Weapon turn in quest
- Fixed EM & CN Kojika pet drop tables
- Fixed Kojika pet claim quest HUD displaying wrong cost


*Developer Note (9/11): Hi all!*


*As some of you have likely noticed, two issues regarding the new Kojika pet were discovered after 1.3.3 launch.*


*The first was that Cyber Ninja and Essence Mage characters were experiencing the wrong drop rate, making the pet impossible for those classes to receive. This issue was fixed on 9/8, and all classes may now receive the pet.*


*The second issue was that due to a typo, the Kojika pet claim quest displayed the incorrect number of Marks needed for completion. The quest text displayed a cost of 1000 Marks of Devotion, when the intended cost (and the number actually needed to complete the quest) is actually 2000 Marks. This typo has been fixed in 1.3.3 Update 1, with the cost display updating to show the 2000 Mark cost that was always present under the hood.*


*This 2000 Mark cost was already intended to be significantly lower than the 3000-4000 Mark cost range that better aligns with the drop rate of the pet, which is why the team has decided to fix the typo and maintain the 2000 Mark cost rather than lowering the price.*


*Unrelated to the pet is a third issue that players may have noticed, that being the Essotech Weapon claim quest cost being too great. This has also been remedied in 1.3.3 Update 1, with the cost of 1000 Marks of Devotion being reduced to the intended 185 Marks of Devotion cost.*


*We thank you for your continued interest in the Sae Jiko boss fight and related content, and apologize for the inconvenience this has caused to our players.*


*As recompense for 2 out of 3 classes not properly receiving drops for the pet, and the quest HUD text displaying the incorrect cost for the pet, the team has decided to run a special event this weekend.*


*For this weekend only (Sept. 15th-18th), the drop rate for the Kojika pet will be increased by 1.5x its current drop rate!*


# Hotfix 2


- Fixed an infinite number of mini map markers building up over time, causing severe performance drops
