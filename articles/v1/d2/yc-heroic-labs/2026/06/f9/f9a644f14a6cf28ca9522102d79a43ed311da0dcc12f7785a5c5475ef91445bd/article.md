---
schema_version: "1.0.0"
document_id: "f9a644f14a6cf28ca9522102d79a43ed311da0dcc12f7785a5c5475ef91445bd"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/well-played-games-case-study/"
published_at: "2026-06-07T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:6787f415b3e694704b188190f3f306903ffff24c2a907eeee1905de19fa6bc11"
---

# Playing the Long Game: Lessons from Building Clans of London

[Well Played Games](https://wellplayed.games/) has been making live service titles based on beloved IPs since 2017. *Warhammer Combat Cards* is in its eighth year, built on top of established lore. Their newest project,[Clans of London](https://www.clansoflondon.com/) , takes a different approach: where most games ask players to simply inhabit a world, this one asks them to shape it.


Set in the *Vampire: The Masquerade* universe, the game revolves around time-limited events where player choice matters. Last Christmas, players decided the fate of Lili Valentine, a mortal caught between rival vampire clans. A week later, she returned to the game transformed and considerably more[dangerous](https://vtm.paradoxwikis.com/CoL_Card:Lili_Valentine) .


For players, that’s the kind of magic that keeps them hooked. For the studio behind it, it’s a commitment that runs all the way down to their choice of backend and LiveOps.


When Well Played Games’ co-founder and producer[Adam Wells](https://www.linkedin.com/in/ajwells/) was asked what he’d do differently if he could go back to those early years after the studio’s founding, he doesn’t hesitate: don’t build a custom backend.


This is the story of how he came to that resolution, how it shaped *Clans of London* , and in turn, how it shaped the studio.


## A mortal mistake


> “We built it because we could. But the biggest thing we underestimated is maintaining it.” — Adam Wells, Co-founder, Well Played Games


For a young studio with the engineering talent to pull it off, building everything themselves made sense when they first set out to make games. The backend, the LiveOps tooling, the server infrastructure: all of it was custom-built and owned in-house. At the time, that felt like control.


What it actually meant was that every system they built, they also had to feed: updating it, debugging it, scaling it, and defending it from entropy while simultaneously trying to ship new features and keep players engaged. Every new feature had to be weighed against the cost of owning it forever.


“I sort of regret that to this day,” Adam says. “Because now we’re constantly talking about how to get our other games over onto Heroic.”


## The backend they didn’t have to build


The world of Clans of London, where players shape the story through time-limited Chronicles


Before *Clans of London* even had a name, Well Played Games knew they wanted an off-the-shelf backend this time. Adam and his team learned about[Nakama](https://heroiclabs.com/nakama/) , Heroic Labs’ open-source game backend, and it immediately checked all their boxes.


Part of it was the built-in features every multiplayer game needs:[groups](https://heroiclabs.com/docs/nakama/concepts/groups/) ,[leaderboards](https://heroiclabs.com/docs/nakama/concepts/leaderboards/) ,[matchmaking](https://heroiclabs.com/docs/nakama/concepts/multiplayer/matchmaker/) ,[tournaments](https://heroiclabs.com/docs/nakama/concepts/tournaments/) , things they’d have otherwise had to build and maintain themselves. Part of it was that their existing card game logic, the technology they’d carried across every title, translated neatly into Nakama’s architecture. And part of it was how fast the team moved once they started; the server runtime was easy to learn and clicked with their engineers immediately.


Choosing a pre-built backend also meant choosing to partner with Heroic Labs. Early in the project, the team visited us in-person for a day of learning, sharing stories, and aligning on what to build. “Alignment of the stars,” Adam calls it. The prototypes that came out of that period, like tournaments and clans, were already working in staging from the get-go.


> “We were confident we could bring those social systems into the live game and they’d work seamlessly.” — Adam Wells


## LiveOps, handled


LiveOps is the difference between a game that lasts and one that doesn’t. It’s what keeps a game feeling alive between updates and turns an OK retention curve into a great one. Spending engineering cycles just to keep it running costs more than most studios budget for. Well Played Games knew that. For *Clans of London* , they found their answer in[Satori](https://heroiclabs.com/satori/) , Heroic Labs’ LiveOps platform.


> “As free-to-play developers, LiveOps is massive. It’s so important to what we do. Satori just really enables us to satisfy that strategy.” — Adam Wells


Earlier this year, the team identified that high-end iOS devices were defaulting to 60fps and overheating. Within hours, their game configuration had been[remote updated](https://heroiclabs.com/docs/satori/concepts/remote-configuration/) from Satori to default those devices to 30fps, all without needing to go through any app store approvals or make any deployments.


“A few years ago, that would have required a new submission to Apple’s App Store and a lengthy wait,” Adam says.


Satori also handles the shape of the player’s journey. Well Played Games uses[audience segmentation](https://heroiclabs.com/docs/satori/concepts/segmentation/) to serve different offers and events depending on where a player is in their lifecycle: new player, returning player, engaged non-spender, or spender. They use[feature flag](https://heroiclabs.com/docs/satori/concepts/remote-configuration/understand-feature-flags/) to test[live events](https://heroiclabs.com/docs/satori/concepts/live-events/) internally before general release, letting the team play the live game in a completely different configuration from what players see.


“The main thing we’re trying to do is give a more bespoke experience to each individual player,” Adam says. “Based on whether they’re a spender, a non-spender, highly engaged, returning, all of those things.”


[Learn more about how Satori solves the entire LiveOps lifecycle](https://heroiclabs.com/docs/satori/concepts/introduction/) , from player targeting to experimentation to behavior analysis.


*Clans of London* features eye-catching card art with real-life models and photography, a luxury they can afford since they don't need to worry about infrastructure headaches


## The team that came with the software


Running a live game surfaces problems that no amount of documentation can prepare you for. What Well Played Games found, beyond the software, was a team willing to work through those problems alongside them: on calls, in their codebase, and under load on their staging servers.


For example, in a server-authoritative mobile game, the size of data sent back and forth between client and server has a direct impact on how the game feels: response times, battery drain, and server load under scale. During a review, Heroic’s engineers flagged that some of the game’s client-server exchanges were reaching 70kb. The target is around one. When the team audited their own code against that benchmark, the impact it had on the player experience became noticeable.


> “We often found ourselves on a call with very senior people helping us debug our problems.” — Adam Wells


*Clans of London* is known for its player-driven storytelling, immersive atmosphere, and fast-paced battles


## Well played, all round


Working with Well Played Games has been one of those partnerships that reminds you why this work is worth doing. From the early workshop in London to the late calls debugging server memory under load, the relationship has been defined by the same quality that defines *Clans of London* itself: a genuine willingness to listen, and to act on what you hear.


We’re proud of what the Heroic tech stack has made possible for the studio. But what we’re proudest of is the team they’ve become in the process: sharper, faster, and more confident in what they’re building. Watching a studio grow into their infrastructure is one of the better things you get to witness in this industry.


*Clans of London*[version 1.2.0](https://www.clansoflondon.com/post/v1-2-0-release-notes) drops June 11th: a new season, a new Chronicle, and a round of improvements built on everything we figured out together. We’ll be there for the next one too.
