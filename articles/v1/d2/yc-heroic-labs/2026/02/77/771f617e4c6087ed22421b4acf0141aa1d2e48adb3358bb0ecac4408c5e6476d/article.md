---
schema_version: "1.0.0"
document_id: "771f617e4c6087ed22421b4acf0141aa1d2e48adb3358bb0ecac4408c5e6476d"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/whatwapp-case-study-part-2/"
published_at: "2026-02-09T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T22:21:23.355424+00:00"
content_hash: "sha256:1e8d577a53a8c30cef7a4b2e59314ea2dfff813daf25a543b60b9dc59166af0c"
---

# Why Whatwapp Doesn't Need to Build Backends Anymore

In the fast-moving world of mobile gaming,[Whatwapp](https://whatwapp.com/) has carved out an enviable position. The Milan-based studio has become a powerhouse in European markets by committing to a singular goal: create something great, innovative, and fun… and do it fast!


> "Our goal is to entertain our players and bring them joy in every possible way. It's a combination of creativity and ownership. That's a key thing for us, loving what we do and having fun while doing it." - Antonis Tsalapakis, Senior Technical Product Manager, Platform Team


This ethos has shaped not just their games but their entire approach to technology. When faced with backend challenges, Whatwapp doesn’t just look for vendors, they seek partners who enable their creative independence.


## From Crisis to Creative Framework


When we[last spoke with Whatwapp](https://heroiclabs.com/blog/whatwapp-case-study/) , they had just completed a high-pressure migration from GameSparks to Nakama, moving three live titles with minimal player disruption. That was just the beginning.


Today, five Whatwapp games run on Nakama, and what started as an emergency backend replacement has evolved into a sophisticated shared framework across their entire portfolio. Rather than treating each game as an isolated project, Whatwapp transformed their migration lessons into reusable components that embody their “small team, big impact” philosophy.


> "We have built a common framework for all our games. Whenever we have a new game, we have a set of packages that everybody can integrate. Games share the same code, and whenever somebody finds a bug, the Developer can just make a PR in the common module, publish it, and everybody can update." - Ricardo Gonzalez, Technical Lead, Platform Team


This standardization delivers exactly what Whatwapp needs in today’s competitive market.


Since the initial migration of Burraco, Scopa, and Belote, Whatwapp has added Backgammon and Euchre to the platform. Their decision framework is simple: if a game needs multiplayer, social features, or remote capabilities, it runs on Nakama.


> "The ability to spin off a new game as fast as possible, test it, and scale without the hassle of rewriting the whole backend… that's what Nakama offers us." - Ricardo Gonzalez


BurracoScopaBeloteBackgammon


## Building Technical Expertise Together


The shared framework was just one part of Whatwapp’s evolution. As their understanding of Nakama deepened, the studio began leveraging its multi-language support to match their team’s evolving expertise. While they started with TypeScript to ease the migration, they later chose to implement certain systems in Go to take advantage of their growing familiarity with the language.


“We migrated the authoritative match loop from TypeScript to Go and had a substantial improvement,” Ricardo explains. This flexibility to choose the right language for each task shows how Whatwapp maximized Nakama’s capabilities. The partnership has also grown to include regular consultations with Heroic’s engineering team on architectural decisions.


> "I'm able to speak directly with the team. It makes us feel cared for." - Ricardo Gonzalez


This shared knowledge base has become as valuable as the shared code framework. “Everybody in the team has worked with Nakama so they can quickly spin off a new game.” says Antonis Tsalapakis.


## From Vendor to Ally


For a studio that values creativity and ownership, finding the right technology partnership has been transformative.


> "We see you as an ally in our operations." - Antonis Tsalapakis


For Whatwapp, that means stability in the product and dependability in the company.


What started as a necessary migration has become a foundation for Whatwapp’s continued growth. With their backend framework standardized across five games, the studio has reclaimed the speed and creative freedom that defines them. In an industry where velocity often determines who captures market opportunities, Whatwapp and Heroic Labs have built a partnership where technology accelerates creativity rather than constraining it.
