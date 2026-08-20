---
schema_version: "1.0.0"
document_id: "d03ebb7a5b11197062f409c9d026d078da07bae6fdcb5fd773cd36780dade0a4"
company_key: "yc-minusx"
company: "MinusX"
source_id: "yc-minusx-rss-04cd28ab33fb"
canonical_url: "https://minusx.ai/blog/plots01-nba-clutch"
published_at: "2024-10-29T00:00:00+00:00"
first_seen_at: "2026-07-25T14:25:41.183756+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:dbc3109adf012fe6adfcbdead0be4cdeedc41385c8ebef648ad8aecdbe49821d"
---

# Plots01: Who is the Clutch-est NBA player of all time? [2000-2024]

# Plots01: Who is the Clutch-est NBA player of all time? \[2000-2024\]


who's that pokemon?


[vivek](https://x.com/nuwandavek) /


2024-10-29


/


3 min read


/


Data Science


Since the 2022-2023 season, NBA awards["The Clutch Player of the Year"](https://en.wikipedia.org/wiki/NBA_Clutch_Player_of_the_Year) trophy. Named after Mr-Clutch himself, the Jerry West trophy is awarded to the player who "best comes through for his teammates in clutch moments" in the regular season. The[NBA defines](https://www.nba.com/news/stats-breakdown-coming-through-in-the-clutch) "clutch time" as:


- last 5 mins of the 4th quarter or overtime, when
- the score is within 5 points (basically 2 possessions)


Despite being a subjective award, voted on by the media, it is very evident from the plot below of all players in the respective seasons that it is basically awarded to the player with the most clutch points (and a decently high effective field goal %)


- eFG%=2_pointers_made+1.5×3_pointers_madetotal_shots_attempted\\text{eFG\\%} = \\frac{\\text{2\\_pointers\\_made} + 1.5 \\times \\text{3\\_pointers\\_made}}{\\text{total\\_shots\\_attempted}}


eFG%


=


total_shots_attempted


2_pointers_made


+


1.5


×


3_pointers_made


​


- Clutch points : Points scored in clutch minutes of a game tagged as a clutch game


Using this metric, we can now backfill The Clutch Player of the Year award for the last 20 years! **2016-2017 Russell Westbrook** has had the most clutch season of all, in the last 2 decades. **Lebron** would have probably won the award thrice - in 2007, 2008 and 2010. **Steph, KD and Westbrook** would have probably had 2 awards each.


Subscribe to Plots by MinusX - A Data Story Magazine


We hear a lot about how the game has changed, and how there is so much parity in the league. I wanted to see if this is reflected in the % of games in a season that are "Clutch Games". Surprisingly the % of clutch games is more or less the same at ~50%.


So to finally answer my original question, I aggregated the entire career clutch performance of players. To account for the fact that Lebron has played all of the last 2 decades, the total score is misleading. Avg. Clutch points is probably the best indicator (with a minimum of 100 career clutch games). **Kyrie Irving** is the clutch-est player with an average of 3.68 points in clutch at ~55% eFG%, with Lebron is not far behind. Lebron's dominance over the last 2 decades is truly baffling, and this just reinforces it!


Many thanks to[@shufinskiy's GH repo](https://github.com/shufinskiy/nba_data/) for the NBA` shotdetail` and` pbp` data.[Let me know](https://x.com/nuwandavek) what other NBA analysis or Plot you want to see! I'd love story requests! Of course all analysis was done using[MinusX on jupyter](https://minusx.ai/tools/jupyter) :)


Plots is a data story magazine by us, covering topics in sports, politics, science and society. Subscribe above to get the next one in your inbox!
