---
schema_version: "1.0.0"
document_id: "d027d54139206af9fb4b701eb0f2e87d6f3ba31e737d77784a13c695d4fb7a26"
company_key: "yc-skillsync"
company: "Skillsync"
source_id: "yc-skillsync-news-import-bd29ecf3b7f3"
canonical_url: "https://app.skillsync.wiki/blog/b9d1f4a3"
published_at: null
first_seen_at: "2026-07-22T13:42:01.145674+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:bf253064d5744181fb544406940dfd38f84739b3571c53d06c19f94dae4a917e"
---

# How Zed hires engineers who think past the machine

In conversation with


[Mary Luna](https://www.linkedin.com/in/maryluna/)


Head of Recruiting, Zed


Mary is the Head of Recruiting at Zed, where she's helped nearly triple the engineering team in the past year. She's spent her career connecting people, first as an agency recruiter partnering with venture-funded companies to find engineers, then bringing that same hustle in-house as a talent partner. She joined Zed because the founders see hiring the same way she does: as community building, not a numbers game.


About Zed


Zed is a high-performance code editor built from first principles in Rust. Founded by Nathan Sobo and Max Brunsfeld, who built Atom at GitHub, alongside Antonio Scandurra, Zed is designed around real-time collaboration, speed, and craft. The team ships multiplayer-first developer tools, and recently announced Delta as part of their Series B.


~1M


Lines of Rust


100%


Remote


~3x


Team growth in 2025


This was a special conversation for me. If there's one company that has genuinely figured out how to hire great engineers through open source, it's Zed. They don't just talk about it, they've actually built their team this way. Less than half of their hires ever applied. The rest came through contributions, conferences, Hacker News, and real relationships built over time. I was fortunate enough to sit down with Mary and hear her break the whole thing down.


Zed doesn't hire like most companies. There are no all-day interview loops, no algorithm gauntlets, no weight placed on where you went to school. The interview process is three conversations, each between 20 and 45 minutes. The team pairs on almost everything. There are no code reviews.


01


## Thinking past the machine


Zed is a Rust shop, but the bar goes deeper than language proficiency. Mary describes the defining trait of Zed engineers as the ability to "think past the limitations of the machine."


"It's not just like 'oh, this doesn't work, it's impossible.' It's 'we're going to figure out how to push past that boundary of the machine.' That's the technical attribute people share here."


— Mary Luna


This makes sense when you consider what Zed is building. The editor is written from scratch, nearly a million lines of Rust, with performance as a first-order constraint. You're not pulling things off the shelf. Every decision has downstream effects, and the engineers who thrive here are the ones who reason about systems at the lowest level: people who've built programming languages, contributed to Rust itself, or led projects like rust-analyzer.


The team also has a clear rule for AI usage: don't have the agent build something you don't know how to write by hand.


"There's still a very high bar in terms of code and performance. The code the agent spits out has to be really performant. There's still a lot of rewriting agentic code because it just needs to be as performant as possible."


— Mary Luna


02


## Pair programming as culture


The most distinctive thing about Zed's engineering culture is how much they pair. Between 25% and 75% of work happens in pair programming sessions, and it's not the kind where someone shares their screen on Zoom while the other person watches.


Zed's collaboration feature was one of the first things the founders built. It's a CRDT-based system, like a Google Doc for code, where two engineers share a codebase with multiple cursors, editing simultaneously with built-in audio. The result: no code reviews, no PR backlog, and fast shipping.


"When you feel like you're both in a great place in terms of the code and figuring things out, you could ship it quickly and easily. There's no backlog of code reviews."


— Mary Luna


This style traces back to Pivotal Labs, where co-founders Nathan and Max worked over 15 years ago. Pivotal was known for 100% pairing: one screen, two keyboards. Zed evolved that model for the remote era.


Mary says pairing is probably the biggest culture shock for new hires. But it solves several problems at once: it distributes institutional knowledge across a nearly-million-line codebase, it replaces code reviews with real-time feedback, and it creates genuine social connection on a fully remote team.


"We don't have forced virtual happy hours or virtual events. The actual interactions, building this product together, that's the real thing."


— Mary Luna


The implication for hiring is significant: Zed engineers need to be social. Not extroverted, necessarily, but comfortable working closely with another person, thinking out loud, pushing back, and being open to discussion. Technical skill alone isn't enough.


03


## Open source as a hiring pathway


Zed has hired engineers directly from their open-source community. Contributors who caught the team's attention, sometimes by submitting PRs, sometimes by sparking interesting discussions, were invited to pair with team members. That pairing became a natural audition.


"We've gotten hires through our open-source community. People who have made contributions that have gotten our attention. Oftentimes our team pairs with contributors as well, so they get a sense of pairing with them."


— Mary Luna


Inside Zed, there's a dedicated group of "open-source engineers" focused on community-facing work: bugs, feature requests, contributor interactions. Some of these engineers entered without deep systems experience and grew into core team roles over time. It's a real pathway, not a token gesture.


Mary's advice for engineers who want to join a company like Zed through open source: have genuine intentions.


"People want to contribute just because they think that contributions can lead to a job. It's a very slight chance. You should have an interest in it. You should be getting something out of it. If you're just doing this because you want to get a job, that sets you up for disappointment."


— Mary Luna


Less than half of Zed's hires came through direct applications. The rest were a mix of open-source contributors, outbound sourcing, conference connections, Hacker News, and referrals.


04


## Three interviews, that's it


Zed's interview process is intentionally minimal: three interviews, each between 20 and 45 minutes. No all-day loops. No systems design rounds. No algorithm exercises.


**Interview 1 (20 min):** A conversation about hard problems you've solved, your impact, and how you've collaborated. The team is sussing out tenacity, the ability to push past constraints, and communication skills.


**Interview 2 (45 min):** A Rust coding session, pairing with team members. You can use any editor, including AI-assisted features like tab completions, but not autonomous agents. People at varying levels of Rust experience have passed.


**Interview 3:** A values interview. Alignment with Zed's culture matters as much as technical ability.


"I've seen interview processes that involve hours of engineering time. We don't have time for that."


— Mary Luna


The conversion rate on the coding interview is almost one in three. The strong pass rate comes down to pre-qualification: by the time someone enters the process, there's already strong signal that they could be a fit. The values interview carries equal weight. Passing the coding round doesn't guarantee an offer.


05


## The anti-sell


Zed is fully transparent about how they work, and it's reflected in the interview process so candidates can self-select out if it doesn't align. Mary calls it the "anti-sell."


"Hiring the wrong people is so much worse than not hiring. We want people to self-select out early on. These are really hard problems that we're solving. When people are really aligned, they're really aligned, and that's what's most important."


— Mary Luna


Zed's values are structured as deliberate tensions, pairs that push and pull against each other:


- **Autonomy and Ownership** and **Collaboration** - own your work, but build together
- **Intensity** and **Humility** - push hard, but know when you're wrong
- **Shipping** and **Craftsmanship** - move fast, but never ship slop


The sweet spot, as Mary describes it, is finding the balance point between each pair. You can't just ship for the sake of shipping. You can't get stuck in a cycle of perfectionism either.


"I don't think any amount of selling or convincing when somebody gets an offer is necessarily relevant. The process is more important. Give people all the information they need to make the decision that's right for them."


— Mary Luna


Summary


## Pedigree is overrated


Mary's sharpest take: pedigree isn't the first qualifier of a good resume at Zed. It's more of an afterthought.


> "Find the diamond in the rough. You can't just automatically look at school and pedigree of companies. Look at their projects. Look a little bit deeper. I don't think we put weight at all on pedigree."


— Mary Luna, Head of Recruiting at Zed


The team has hired new grads who are just as effective as seasoned engineers. They've found people through Hacker News threads and conference connections. They've onboarded contributors who started without systems experience and grew into core roles. What matters is the work, the thinking, and the fit, not the resume.


For engineering leaders trying to build their own hiring muscle, Mary's advice is simple: everyone on the team should be a recruiter. If you see something, say something. If an engineer posts something interesting, wrote a sharp contribution, or gave a great conference talk, reach out. Be genuine. Recruiting is community building, and the best connections are long-term, not transactional.


"Software engineering is a community. People want to hear from you. If you think their work is really cool, I think people want to hear that. Even if it's not recruiting right now, recruiting is always a long-term game."


— Mary Luna


Further reading


## Links


- [Hired Through GitHub, Part 1](https://zed.dev/blog/hired-through-github-part-1) - How Zed has hired engineers directly through open-source contributions.
- [Hired Through GitHub, Part 2](https://zed.dev/blog/hired-through-github-part-2) - More stories from the open-source hiring pathway.
- [Zed Is Our Office](https://zed.dev/blog/zed-is-our-office) - How the team uses Zed itself as their remote workplace, and how pairing fits into daily life.
- [The Full Spectrum of Collaboration](https://zed.dev/blog/full-spectrum-of-collaboration) - The three co-founders on why they built collaboration into the editor from day one.
- [The Case for Software Craftsmanship in the Era of Vibes](https://zed.dev/blog/software-craftsmanship-in-the-era-of-vibes) - Nathan Sobo on why quality should go up, not down, when code generation gets cheap.
- [How Our Community Shipped 66 Git Improvements in Under 2 Months](https://zed.dev/blog/lets-git-together) - A look at how Zed pairs with contributors through community sprints.
