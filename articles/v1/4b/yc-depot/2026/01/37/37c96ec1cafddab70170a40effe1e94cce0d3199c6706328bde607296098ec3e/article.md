---
schema_version: "1.0.0"
document_id: "37c96ec1cafddab70170a40effe1e94cce0d3199c6706328bde607296098ec3e"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/3-fears-that-make-work-suck"
published_at: "2026-01-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:6ec9ebd21281ae27d004eb226b18bd7cf027c728cb0ceec06233b949fc9fbd2d"
---

# The 3 fears that make engineering suck

So there I was bringing prod to its knees… in my second week.


I was in my home office, properly caffeinated, trying to make a resource-hog of a query more performant. I’d been working on some ClickHouse query optimizations, and it became painfully clear that my “optimization” was doing the literal opposite of what it was supposed to do. Instead of reducing resource consumption, every CPU on the ClickHouse clusters we’d provisioned was pegged at 100%.


I instantly knew it was me. The timing was too perfect. I’d just deployed the thing, and now everything was on fire.


It was feature-flagged, so the fix was simple: reflag and redeploy. Easy. But that didn’t stop the panic from waking up some dear old fears of mine. What I like to call the 3 Horsemen of Making Work Suck: fear of looking dumb, fear of underperforming, and fear of wasting people’s time. All manifestations of imposter syndrome.


What’s funny is that these fears popped up despite the fact that I’ve worked with many of the engineers at Depot for years. It didn’t matter. Fears rarely care about logic.


Those fears turned me into a perfectionist editor. Typing, deleting, rewriting, deleting again. Imposter syndrome convinced me everyone else was superhuman while I’m an intellectual dumpster. The fear of underperforming made me assume I was doing a horrible job and everyone was just too nice to tell me.


These fears aren’t particularly useful. But they’re real, and they contract my ability to be effective and enjoy my work. They bleed into life after work too. It’s hard to enjoy dinner with my wife when the back of my mind is screaming, “wait til they find out how dumb you are,” like some cartoon villain twirling a mustache in my brain.


## Fear #1: Wasting people’s time


The fear of wasting people's time showed up right away. The moment I read Depot’s “how to ask questions” rubric, I thought: oh man, I hope I don’t ask a bad question. I was more self-conscious, not less.


The rubric was simple: be specific, provide context, be clear, be concise. But initially, I saw it as a test I might fail.


Then it clicked. The rubric wasn’t a trap. It wasn’t a list of ways to get yelled at. It was a cheat code for getting the answer I needed without five hours of async back-and-forth ping pong.


A lot of questions get asked like this: “Hey, can someone help me out?” This is not particularly useful. Maybe there is someone who can. We literally have no idea. What’s the actual question? Do you need a cold brew recipe for your at-home barista kit, or something technical? Either is fine, but no one knows if they can help without context. And now we’ve guaranteed async ping pong.


A specific ask with context lets someone answer in one go. High information and impact density.


Once I reframed this from “I hope I don’t ask a bad question” to “they’re literally telling me how to ask a good one,” a ton of pressure came off. Less second-guessing. Less typing and deleting. More forward motion.


Specific. Context. Clear. Concise. The rubric wasn’t there to punish violators. It was there to help me.


## Fear #2: Looking dumb


The thing about joining Depot (or really any company) is that I’m probably the dumbest person in the room. At least that’s what my inner critic says. And shoot, at Depot, that might even be true.


That said, this internal critic, however accurate, isn’t useful. Fear of looking dumb contracts my ability to be effective. If I’m afraid of appearing ignorant, I’m less curious. And without curiosity, I’m not nearly as useful as an engineer.


Fear of looking stupid kills curiosity. You nod along when you don’t actually get it. You avoid the areas where you’re weakest, which means you never get stronger in them. You become less useful, not more.


What punctured that illusion was watching other engineers ask real questions. Questions that revealed gaps in their knowledge. And no one treated them like they were dumb. The questions got answered. The work moved forward.


I realized the people I thought knew everything were also learning. The difference wasn’t that they knew everything. It was that they weren’t afraid to not know.


So I started asking questions. And shockingly, lightning did not smite me for my ignorance. I was encouraged, pushed back on when needed, and pushed forward. I got more shit done, faster. And maybe someone else noticed and felt less fear about looking dumb too.


## Fear #3: Underperforming


Whenever I start somewhere new, I assume I’m now the worst engineer on the team and doing a horrible job. At a remote company, that fear takes a special shape: what if I’m underperforming quietly? What if no one notices until it’s too late?


Remote work makes your work invisible by default. You’re not in a room where people can see you typing or overhear conversations. You’re just… there. Or not there. And if you’re quiet, you might as well not exist.


What helped at Depot was working in public. I was active in Discord, Linear, and GitHub with progress, blockers, and outcomes. When I was stuck, I said so. When I found something interesting, I shared it. When I shipped something, I posted a screenshot.


Communicating all the damn stuff I was doing helped. Not to look busy, but to make my work visible. Progress, lack of progress, and results were transparent. That visibility became its own form of accountability and support.


The fear of underperforming quietly disappeared because I wasn’t performing quietly.


## How I overcome it (kinda)


- Ask questions with context. Specific, clear, concise. The rubric is a cheat code, not a test.
- Say when I’m stuck. It speeds things up and shows others where to jump in.
- Work in public. Post in team channels, Linear, GitHub. Let people see the path, not just the final result.
- Treat visibility as clarity, not performance. I’m not proving I’m busy. I’m showing the state of the work so the team can move.
- Share the interesting things I find. Curiosity builds trust and shows I’m thinking about the system.


The annoying part is I don't really "overcome" these fears. I just get better at noticing them and moving anyway. Level 1 might be DMing a fellow engineer. Level 2 is asking the same question in a more public channel. As I get better at dealing with the fear, the game just hands me bigger boss battles. That's fine. The goal is not zero fear, it's keeping fear from driving.


## FAQ


What are the 3 fears that make engineering suck?


Fear of wasting people's time, fear of looking dumb, and fear of underperforming. They all stem from imposter syndrome and contract your ability to be effective and enjoy your work.


How do you ask questions without wasting people's time?


Be specific, provide context, be clear, and be concise. Instead of "Hey, can someone help me out?" say what you actually need. A specific ask with context lets someone answer in one go instead of five hours of async back-and-forth.


How do you know if you're actually underperforming or if it's just imposter syndrome?


Make your work visible so you're not guessing. Share progress, blockers, and outcomes in Discord, Linear, or GitHub. When people can see your work, you get real feedback instead of your brain making up worst-case scenarios.


What if working in public makes you more anxious about making mistakes?


It probably will at first. But working in public isn't about proving you're perfect. It's about showing the state of the work so the team can move. The fears don't go away. You just get better at noticing them and moving anyway.


## Related posts


- [We hire people who care about their work](https://depot.dev/blog/we-hire-people-who-care-about-their-work)
- [How support shapes the product at Depot](https://depot.dev/blog/how-support-shapes-the-product-at-depot)
- [What technical writers actually do at startups](https://depot.dev/blog/what-technical-writers-do-at-startups)


Andrew "Watts" Watkins


Engineering Manager at Depot
