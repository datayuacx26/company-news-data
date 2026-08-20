---
schema_version: "1.0.0"
document_id: "1c51bcb8941eabd8136ea7417fd1f0587cbab46aa8d4c391204f3d53e406cc2c"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/what-the-world-cups-knockout-rounds-reveal-about-viewership/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:f98a77c5b8080491cb64f73f20f74ade7791c485d40ce5095d61ed6b11542147"
---

# No More Draws: What the World Cup's Knockout Rounds Reveal About Viewership

There are no draws in the World Cup knockout stages. Every match has a winner, which strips out one of the biggest confounds in the group stage data: matches that don't matter.


In our[last post](https://www.fastly.com/blog/world-cup-viewership-trends) , we looked at the group stage and found a handful of clear signals: halftime dips of roughly a quarter of peak bandwidth, a strong "close match" pile-in effect in the second half, and one Portuguese superstar who appears to move the needle more than anyone else on the pitch. But the group stage has a structural problem: some matches don't matter for advancement. Teams that have already clinched or been eliminated draw less attention, and that noise in the data makes it harder to isolate what's actually driving people to tune in.


The knockout rounds remove that externality. Every match is do-or-die. If two teams are level after 90 minutes, they play on 30 more minutes of extra time, and if it's still level after that, a penalty shootout. There is always a resolution, and it's always a must-watch. That makes the round of 16 and 32 - and those that follow - a great stress test for the patterns we spotted in the groups, plus it introduces something we didn't have before: a forced overtime period, on a schedule, in nearly a third of matches. We’re excited to watch and analyze every match, but for now, let's dive into the round of 16 and 32 viewership trends (based on traffic we saw across the global Fastly platform from June 28th through July 3rd).


## Halftime, under pressure


Given how much else changed between the group stage and the knockout rounds - draws eliminated and every match now carrying win-or-go-home stakes - we expected halftime behavior to shift too. It didn't, really.


Back in the group stage, we measured a 24.65% average bandwidth dip at halftime (standard deviation 4.22%), with Norway v. Senegal as the single biggest outlier at 34.13%. Carrying that same measurement through the round of 32, the average dip came in at 22.78% (standard deviation 4.16%), with France v. Sweden topping the list at 30.21%. That's a shift of about two percentage points - noise, essentially, not signal. The shape of the curve barely moved even as the stakes of every match went up dramatically.


Looking specifically at the round of 16, Paraguay v. France had the highest drop with 27.74%, Mexico v. England was at the low end with 20.74% (standard deviation 3.25%).


That's a useful reminder about what halftime actually represents in our data. It isn't really a measure of how invested people are in the match - it's a measure of how people use a scheduled 15-minute break, and that behavior looks a lot more like a human habit than a reaction to the scoreline. Whether it's a group-stage dead rubber or a knockout match that ends a country's tournament, a meaningful chunk of the audience is going to use the break the same way: step away, check a phone, change the channel for a minute. Stakes might change who's watching and how many of them there are, but they don't seem to change what people do the moment the referee blows the whistle for the break.


## Extra time and shootouts: a new kind of decision point


In our group stage post, we described halftime and the second hydration break as "decision points" - moments where exciting matches ramp up and lopsided ones fade. The knockout round hands us two entirely new decision points: extra time, and the shootout itself.


So far, four matches were decided on penalties - the highest shootout rate of any round so far this tournament. Germany fell to Paraguay 4-3 (1-1) in what was the[first penalty-shootout loss in Germany's World Cup history](https://www.nytimes.com/athletic/7400918/2026/06/29/germany-penalty-world-cup-breakdown/) . The Netherlands lost to Morocco 2-3 (1-1) after a back-and-forth shootout with saves on both sides. Egypt survived Australia 4-2 after 1-1 once the extra periods were done. Switzerland beat Colombia 4-3 (0-0).


Two matches - Argentina's classic against Cape Verde (3-2) and Belgium v. Senegal (3-2) were decided in extra time itself rather than going to kicks.


That gives us six matches to test a four-stage version of our original "close match" thesis: first-half peak → second-half peak → extra-time peak → shootout peak. If our theory from the group stage holds, we'd expect each additional stage to draw a bigger traffic spike than the last, as the "you need to see this" effect compounds.


**A few matches worth highlighting individually:**


-


**Argentina v. Cape Verde:** Argentina led 1-0 at half, Cape Verde equalized in the 59th minute, and the match went to extra time before Argentina finally won it. This is close to the "nail-biter" template from our first post (see: Spain v. Cabo Verde's 33% half-over-half jump) but stretched over 120 minutes instead of 90.


-


**Germany v. Paraguay:** a heavy favorite going out to a side that hadn't reached the round of 16 since 2010 - and doing so by missing in a shootout for the first time in the country's World Cup history.


-


**Portugal v. Croatia:** Portugal won this one in regulation, but it's worth a mention anyway - Ronaldo converted a 68th-minute penalty to level the match before Gonçalo Ramos won it in the fourth minute of stoppage time. No extra time, no shootout, but a genuinely late deciding goal in a match many expected to be tighter. Useful as a contrast case: does a dramatic finish in regulation spike traffic as much as one that spills into extra time?


## Does an upset move the needle more than a star?


Our group stage post found that Ronaldo's presence produced a bigger bandwidth spike than any other single factor we tested. The knockout round gives us a chance to test a competing hypothesis: does the possibility of an upset draw as much traffic as a marquee name?


Germany's exit and the near-elimination of Argentina by Cape Verde are the two best natural experiments here - teams with modest global fan bases pushing tournament heavyweights to the brink.


## A quick global gut-check


It's worth noting that most of the public reporting on this tournament's ratings has been U.S.-centric, for obvious reasons - this is the first World Cup played on U.S. soil since 1994, and domestic broadcasters have been setting records with almost every U.S. match (the U.S.'s round of 32 win over Bosnia and Herzegovina, for instance, was reported as the[most-watched English-language soccer telecast in U.S. history at the time](https://www.nytimes.com/athletic/7417436/2026/07/02/usmnt-tv-audience-world-cup-bosnia/) ).


Our data set isn't U.S.-only — it's global. Once you normalize for that regional pull - comparing each market's traffic relative to its own peak, rather than raw volume - the shape of the traffic itself holds remarkably steady no matter where the audience is watching from. A nail-biter draws the same kind of pile-in behavior whether the surge is concentrated in South America, Western Europe, or North America. Geography changes how many people show up. It doesn't seem to change how they watch.


## The trend holds - just bigger, and with more edges to test


The fundamentals from the group stage are holding up in the knockout round, and in most cases they're more pronounced. Close matches still draw the biggest traffic surges. Blowouts still fade. And now we have a genuinely new variable - extra time and shootouts - that looks like the most extreme version yet of the "you need to see this" effect.


We'll keep watching as the bracket narrows. Upcoming matches should only sharpen these patterns further, and we're curious whether the shootout effect holds at 5% or "explodes" the way close second halves did in the group stage.


*Fastly powers content delivery for some of the largest live events in the world. If you'd like your systems to be more performant, more secure, and ready for global scale,*[get in touch](https://www.fastly.com/contact-sales) *.*
