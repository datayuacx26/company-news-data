---
schema_version: "1.0.0"
document_id: "2ecad122cd527c8a4c4c45257c9996453c67bb35939e437e7a3424a95339ee4e"
company_key: "yc-multifactor"
company: "Multifactor"
source_id: "yc-multifactor-news-import-864f66d5ce56"
canonical_url: "https://multifactor.com/blog/how-we-built-multifactors-founding-engineering-team"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-07-28T23:50:04.870202+00:00"
fetched_at: "2026-07-28T23:50:05.940094+00:00"
content_hash: "sha256:f3e8f89d7f75342426b1c0dd057f4a86447f815c2d27a482a8f7cefaa654464b"
---

# How we built Multifactor's founding engineering team in six weeks

# How we built Multifactor's founding engineering team in six weeks


2026-07-13


[Company](https://multifactor.com/blog/tags/company)[Team](https://multifactor.com/blog/tags/team)[Hiring](https://multifactor.com/blog/tags/hiring)


[CS Chintan Shah Interim VP of Engineering](https://multifactor.com/blog/authors/chintan)


The first thing I did when Vivek Nair asked me to help hire Multifactor's first engineers was write a rubric.


That may sound counterintuitive. Most early-stage hiring starts with a req and a gut feel — you post the role, you read résumés, and tell yourself you'll know the right person when you see them. But when you're a founder, "I'll know it when I see it" is not a direction you can hand off to someone else — it lives in one person's head, and a bar that lives in one person's head gets applied slowly, inconsistently, and only by the founder.


Some context. Multifactor (YC F25) is rebuilding the identity layer of the internet for the AI era — cryptographic infrastructure that lets people and their AI agents share access to online accounts without ever exposing credentials. Turn any account into a shareable, revocable, cryptographically guaranteed permission link. The company had just closed a $15M seed led by Nexus Venture Partners, with Y Combinator, Taurus Ventures, and Pioneer Fund in the round. Abhishek Sharma at Nexus, who led the deal, introduced me to Vivek. The science is unusually real — peer-reviewed at USENIX Security, presented at DEF CON, a decade of patents underneath it — which set an unusually high bar for who could build on top of it. We needed engineers with genuine security depth *and* genuine AI-agent depth, at startup speed. That combination is rare, and a lazy résumé filter would have thrown away most of the people who actually had it.


Two beliefs shaped everything that followed: that a recruiting process should be designed backward from what the company is building, and that for a founding team, technical depth and cultural fit matter in equal measure.


Hiring your first engineers is an inexact science. Survey ten founders who've been through it and you'll get ten different answers on their approach — and timing and context matter too, since what worked a year ago might not work today. What follows are the principles that worked for us, in case they're useful to other early-stage teams doing the same thing.


## First, I used the product


Before I wrote a word of the rubric, I did the thing I'd tell any technical advisor to do first: I used the product. Vivek's team had built a working proof-of-concept during YC, and I spent real time using it — not just listening to the pitch. That turned out to be the most useful step in the whole process.


Understanding the product at that level is what made every downstream decision easier to get right. It told me what the team actually needed to be — which pillars mattered, and how much — so the rubric described *this* product's problem instead of a generic senior engineer. It shaped the interview itself — the live coding briefs were slices of our real surface rather than abstract puzzles. It told me who to look for and where to find them. And it meant I could explain Multifactor to candidates myself — the vision, why it matters, why now — and get them genuinely excited without routing every conversation through the founder. On an early team, the founder's time is the scarcest resource there is; having someone else carry the product context made the whole process faster and let Vivek spend his hours where only he could.


You can't hire well for a product you don't understand. So I made understanding it step zero — and then I wrote the bar down.


## Step one: decide what "great" actually means


The easy part was writing the Founding Engineer and Engineering Lead role definitions. The hard part: turning "great for Multifactor" into an explicit, six-pillar rubric. Security, cryptography, and identity systems. AI, ML, and agent systems. Systems level thinking. Practical engineering choices. Academic and research signal. Startup, zero-to-one execution.


Hiring is not a one-size-fits-all process. These pillars were designed around Multifactor's product and the challenges we foresaw getting it to market: you cannot build a zero-trust identity product for AI agents without someone who has actually built security systems, and you cannot build for the agentic web without someone who has actually built with LLMs and agents. Each pillar had explicit tiers — what a 7 looked like versus a 17 versus a 22 — with named companies, named teams, and named signals. The point wasn't false precision. It was to force the fuzzy judgment out of my head and onto paper, where it could be applied the same way to the hundredth candidate as to the first.


And the first version wasn't the one we used. A hiring bar is a product, and like any product it took iterations: we tuned pillar weights, rewrote tiers that were too vague to apply consistently, and recalibrated against real candidates as they came in. It didn't fully settle until the end of the second week. The founders who expect to write a perfect scorecard on day one are usually the ones who abandon it by day three — the point is to start rough and sharpen it against reality.


This is, quietly, the same discipline the product itself demands. In security, an idea that lives only in the architect's intuition is an idea that gets implemented inconsistently. So is a hiring bar.


## Step two: prime the pipeline — and give it three weeks


Sourcing was deliberately wide. YC's Work at a Startup gave us volume. Warm referrals — from investors, from university networks, from the team — gave us signal, and we tracked every candidate back to the exact person who referred them, because provenance matters a lot when you're calibrating trust. LinkedIn and direct inbound filled in the rest.


Founders can sometimes give up on the funnel too soon. They want it humming on day one, but sourcing is a spring you load before it releases — for the first couple of weeks it looks like nothing is happening, just quiet on the other end. By the end of week two we saw the first real movement: candidates writing back, reserving slots for intro calls. It took a full three weeks before the pipeline was genuinely humming. That lag is normal, and the founders who panic through it — lowering the bar to manufacture the feeling of progress — are trading away their single most important early decision.


## Step three: review the flood without drowning


A written bar has a second payoff: you can partially automate against it. As résumés came in, we structured them and scored each candidate against the rubric — and we did it with two independent frontier models rather than one. Where both models agreed, we had a fast, high-confidence read. Where they disagreed, the disagreement itself was the signal to put a human on it.


That's the same fail-closed instinct we hold in the product: automate the easy, confident cases; route the ambiguous ones to a person; never let the machine make a silent, irreversible call it isn't sure about. It let a small, part-time recruiting effort keep pace with a large inbound without ever letting a strong candidate fall through on autopilot — and it decided who was worth a live conversation in the first place.


## Step four: how to interview engineers when everyone codes with AI


Coding interviews have taken on a different meaning in the age of AI coding tools.


Every serious candidate now has Cursor, Claude, and Copilot. Which means the classic screen — can you produce a correct algorithm on demand — now tests the one thing the machine already does well. If a model can ace your interview, you are no longer measuring the candidate. You're measuring their tools. Making the question harder doesn't fix it — it just moves the problem: ratchet up the difficulty and the model clears that bar too, and all you've done is raise everyone's score by the same amount.


So we did the opposite of what the automated tools do. No take-home, no timed leetcode, no proctored puzzle — a live, 60-minute paired-programming session with me. Candidates got a short prep guide beforehand so they knew exactly what to expect, and the brief itself at the top of the call. The problems were slices of our real product surface — build an end-to-end-encrypted file-sharing flow where the server can never read the contents; build an internal tool that surfaces potential PII across messy, multi-source data — spanning backend and frontend, in whatever stack they liked.


AI tools weren't just allowed; they were encouraged. But before writing a line, each candidate walked me through their plan — which tool for which part, and how they'd stay in control of what it generated — and then we built together while I asked about every meaningful piece of code. That's the whole test: not whether the AI can write it, but whether the human can *own* it — steer it, catch its mistakes, defend the trade-offs, and cut the right scope when the clock is short.


Two things mattered more than I expected. First, the human on the other end. Candidates are increasingly alienated by this new breed of LLM-powered interview bots — one-way recordings, automated scorers, nobody actually watching. A real person pairing with them is both more respectful and more revealing: you see how they think out loud, how they collaborate, how they react when something breaks. Second, we let AI do the part humans are bad at — every session was captured by an AI notetaker (Fathom) so I could give the candidate my full attention instead of scribbling notes. AI assisting the evaluation; a human making the call. The same pattern we used everywhere.


I was much more interested in the choices they made than the actual code that was ultimately written. Did they reach for a vetted crypto library or try to roll their own? Where did they put the trust boundary? When they had to cut scope — and everyone cuts scope in sixty minutes — did they cut the right things, and say so out loud? Could they explain a tradeoff to a non-expert and defend a protocol choice to one? Those are the signals that predict whether someone can turn a research paper into correct-by-construction production code. None of them come out of a prompt.


## Step five: fly the finalists to San Francisco


The remote session proved someone could build. The on-site was for everything you can only judge in person — so we flew the finalists to the San Francisco office for a day built around three things.


The first was **systems thinking** : an open-ended whiteboard design problem — an AI system in a regulated, customer-facing domain — worked through out loud, together. We weren't after the "right" architecture, but how they scoped it before drawing, how they reasoned about trade-offs and failure modes, and whether they went deep where it mattered instead of surveying everything.


The second was **the perspective of the person on the other end** . In a regulated, customer-facing domain, the tell was whether they raised privacy, compliance, explainability, and human review *unprompted* — designing for the user, not just the diagram. A live coding module, scoped from their own design, then showed us they could build a real slice of what they'd just proposed.


The third was **judgment about their own experience** . Senior engineers carry strong priors; the best ones know when not to apply them. We looked for people who drew on hard-won experience but picked the right tool for *this* problem — who didn't reach for an LLM by default just because their last company did.


The rubric had six pillars on paper. Cultural fit was the seventh — the one you can't score in a spreadsheet, and we weighted it just as heavily. A founding hire has to fit the people, not only the problem, so candidates spent real time with more than Vivek — the founding designer and Leslie, our chief of staff. I'd worked with Leslie on a set of culture questions, and instead of marching through a checklist she wove them into real conversation. Candidates left with a genuine feel for what working at Multifactor is like — the people, not just the technical challenge. Culture fit, judged in both directions: an offer went out only when someone cleared the bar *and* the team wanted to build alongside them.


## Treat every candidate like you'll meet them again


One thing we treated as non-negotiable: every candidate should leave the process better than they came in — offer or not. Not out of politeness. The engineers you most want are already being courted by other companies and weighing several options at once; for them, the interview *is* part of your pitch. How your process feels is the clearest preview a candidate gets of what working with you will actually be like.


So we made it personal and kept it a real conversation, not a gauntlet — and the whole team leaned in, not just the interviewers of record. Candidates met the actual people they'd build with and got an honest read on the place instead of a recruiter's script. That effort is what made the process feel alive rather than transactional, and the strongest candidates noticed. When someone has four other offers, that feeling is often the deciding factor.


The payoff also outlasts any single decision. People remember how they were treated long after they've forgotten the coding brief. Candidates we passed on have sent us referrals; some are people we'd want to work with down the road; all of them describe the experience to other engineers. In a small world where the best people all know each other, a great process compounds — treating candidates well is the highest-return recruiting investment there is.


## The result: a founding team in six weeks


Once the spring released, it released fast. Referrals compounded, the paired sessions and the on-site rounds did the filtering résumés couldn't, and the offers went out. We went from no engineers to a founding team in six weeks — and, more importantly, to a *team* , not a set of individually impressive résumés. The pillars were a deliberate hedge against monoculture: security-native, AI-native, and startup-speed, balanced across people who make each other better.


The bar had one more effect I didn't expect. It was so specific — deep security *and* deep AI-agent experience — that some of the strongest people who cleared it weren't looking for a job at all. They were founders already building a piece of this problem. That's how we met[Dave Blumenfeld](https://multifactor.com/blog/authors/dave) . He'd started Keypo, attacking the secure-enclave side of exactly what we do — using trusted execution environments so an agent can *use* a credential without ever seeing it, the natural complement to Multifactor's key-derivation work. Dave was a fit in three ways at once: he cleared the engineering bar, he'd been building the other half of our technology, and he had the zero-to-one instinct of someone who'd done it before. The moment the overlap was obvious, a hire turned into something bigger, and[Keypo joined Multifactor](https://multifactor.com/blog/keypo-acquisition) . The search didn't just find us engineers; it surfaced a fit deep enough to combine two companies.


That team is now heads-down shipping V1, and I'm in it with them as interim VP of Engineering.


## One seat left


The founding engineering team is complete. The one remaining founding seat is the hardest one: a **Founding Chief Scientist** — a founding executive who will own Multifactor's genuinely unsolved problems and carry the answers all the way into production. Fine-grained delegated authorization for AI agents. Cryptographic delegation at scale. Provable, natural-language auditing of what an agent actually did with your account. None of these has a known answer yet. If that's the kind of problem you've spent your life running toward, we should talk.


The smarter agents get, the more of your life they'll touch — and the more it matters who's building the layer that stands between them and everything you own. That's the bar this team was hired against.


— Chintan


# We're redefining zero-trust — so you can protect your accounts with confidence.


Identity is your first and last line of defense, and the root cause of most application security breaches. Multifactor's provably secure zero-trust solutions cryptographically guarantee that only authorized users can access sensitive data, turning identity into your greatest asset in the fight against cyber threats. Learn more about our research, or reach out to explore working together.


[Get Started](https://multifactor.com/get-started)[Learn More](https://multifactor.com/company/sales)


## Related Posts


[Meet Multifactor's Board of Advisors](https://multifactor.com/blog/advisory-board-2025)


2025-10-28


[Company](https://multifactor.com/blog/tags/company)[Team](https://multifactor.com/blog/tags/team)


Meet the world-class team of investors and advisors supporting Multifactor's mission to redefine zero-trust for the agentic web.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


[Multifactor Receives Zcash Community Grant for Development of MFKDF2](https://multifactor.com/blog/zcash-grant-2025)


2025-08-21


[Company](https://multifactor.com/blog/tags/company)[Public Benefit](https://multifactor.com/blog/tags/public-benefit)[Funding](https://multifactor.com/blog/tags/funding)


We are are grateful to the Zcash Community for their support of MFKDF2, a secure, user-friendly, next-generation multi-factor key derivation function.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


[Multifactor Wins Hertz Foundation's Harold Newman and David Galas Entrepreneurial Initiative](https://multifactor.com/blog/hertz-grant-2025)


2025-07-25


We are honored to receive this recognition, and are grateful to the Hertz Foundation for their belief in our mission to reinvent zero-trust for the modern web.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)
