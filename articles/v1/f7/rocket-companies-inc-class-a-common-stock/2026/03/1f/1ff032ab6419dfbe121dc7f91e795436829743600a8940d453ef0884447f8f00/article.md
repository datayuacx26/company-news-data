---
schema_version: "1.0.0"
document_id: "1ff032ab6419dfbe121dc7f91e795436829743600a8940d453ef0884447f8f00"
company_key: "rocket-companies-inc-class-a-common-stock"
company: "Rocket Companies Inc."
source_id: "rocket-companies-inc-class-a-common-stock-news-import-a92ea4897bd8"
canonical_url: "https://careers.rocket.com/blog/technology-and-product/ai-first-class-engineering-citizen"
published_at: "2026-03-16T00:00:00+00:00"
first_seen_at: "2026-07-24T00:08:45.498983+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:fafdfa88fe51133e90759ec4902c4890a03c40f4d37e8ca2fdd34a0230b1b31a"
---

# How we made AI a first-class engineering citizen — and sustained 3.5x productivity

# How we made AI a first-class engineering citizen — and sustained 3.5x productivity


Author:


Shravan Shah


Mar 16, 2026


•


10-minute read


Share:


Here is a question worth sitting with: If AI *wrote* 30% of your production codebase – and *touched* nearly every commit – would you trust it?


Not 30% of your prototypes. Not 30% of your internal scripts. Thirty percent of the code, serving millions of real requests every day, powering systems that shape people's financial lives. When your platform helps people navigate one of the most important financial decisions they will ever make, trust is not optional – it’s the foundation.


We did not start with a goal of proving AI could write production code at scale. We started with a responsibility: Build systems worthy of the people who depend on them. AI became part of how we deliver on that – but only because we held it to the same standard we hold ourselves.


This is the story of what that actually required and what it produced.


## The challenge we saw coming


When we began integrating AI into our daily engineering workflow, the early results were exciting. Code appeared faster. Prototypes came together in hours instead of days. But as we leaned in, we started noticing something that gave us pause.


AI-generated code had a structural pattern we had not anticipated: It was syntactically correct but architecturally inconsistent. It compiled. It passed basic checks. It did roughly what was asked. But it did not quite belong. Logging followed a different pattern than the service next door. Error handling diverged from our established approach. A utility function that already existed got rewritten – because the AI simply couldn’t see it, it was outside the context window.


We started calling this the context vacuum. AI coding tools operate inside a window, not inside your codebase. On any large production system, the vast majority of conventions, abstractions, and institutional patterns live somewhere the AI cannot see. So instead of reusing, it duplicates. Instead of extending the right abstraction, it invents a new one. The code isn’t wrong – it’s just not yours.


"Early on, I'd review a PR and everything would look fine on the surface," one of our senior engineers recalled. "Then I'd realize the AI had reimplemented a helper we already had – in a slightly different way. Multiply that across a hundred commits and you've got real drift."


What we saw emerging was a two-tier codebase: human code that followed our team's conventions and AI code that almost did. The "almost" was the problem. Code close enough to merge under time pressure, subtly inconsistent enough to become tomorrow's incident.


The compounding effects were real. Code churn accelerated. Duplication multiplied. Refactoring fell behind because we were too busy reviewing the flood of new output to address the structural drift it left behind. And the senior engineers absorbing all of this found that their ability to do original, high-leverage work was quietly eroding under the weight of review. The review fatigue, mounting churn, and deferred refactoring were not background noise – they were a forcing function, making clear that we could not scale AI contribution without scaling the guardrails alongside it.


We recognized a familiar tension: The code gets written faster, but the system gets harder to change. Those are not separate trends. They are the same trend seen from two different angles.


This was not an argument against AI. It was a signal that successful AI adoption demands more than most of us expected: a codebase structured enough that AI can navigate it, and enforcement strong enough that the quality bar applies to every contributor – regardless of who, or what, wrote the code.


We realized early that we could not afford a two-tier codebase – not for the systems our clients depend on. So, we built something different.


## Ready to apply for a position that utilizes your strengths?


[Apply](https://careers.rocket.com/careers)


## What "first-class citizen" actually means


When we say AI is a first-class engineering citizen on our team, we mean something specific: Every AI-generated change passes through the exact same gates as human-authored code. Same quality bar. Same accountability. That means AI integrated transparently, used ethically – not deployed as a shortcut that bypasses the obligations engineers carry.


Same type checking. TypeScript in strict mode across eight applications in the monorepo. Every type must resolve. Every interface must be satisfied.


Same architectural enforcement. We built seven custom ESLint plugins that enforce our patterns at the code level – covering data loading, error handling, federation compliance, structured logging, memory leak prevention, and schema governance. One engineer described these plugins as "an architect that never sleeps." They catch violations consistently and early – long before code reaches a pull request – whether a human or AI wrote it.


Same test requirements. Over 513 test files in production. No skipped tests. No suppression directives. If the tests do not pass, the code does not merge.


Same commit conventions. Conventional commit formatting enforced automatically. Same code review gates.


The key insight was making the standard mechanical rather than cultural. When the quality bar is automated – when it applies identically to every contributor – the two-tier problem becomes dramatically harder to introduce. That discipline benefits everyone, not just the AI.


## Interested in a job at Rocket®?


See all of our opportunities today.


[See Our Jobs](https://careers.rocket.com/careers)


## The infrastructure that made it possible


One insight reshaped how we thought about everything else: AI is only as good as the context it can read. A capable model working in a messy codebase produces messy code. A capable model working in a well-structured codebase produces code that fits.


We invested heavily in making our codebase legible – not just for humans, but for any contributor.


Strict layered architecture. Every feature follows a four-layer pattern: Resolver, Domain Service, Integration Service, Adapter. Every file has a predictable location. Every integration follows the same structure. Across 1.6 million lines of TypeScript, there is no guesswork about where new code belongs or how it should interact with existing systems – and that predictability is what makes AI contributions land correctly rather than drift.


Shared libraries with consistent interfaces. Twenty shared libraries covering everything from authentication to caching to structured logging. Each one exposes a clean public API. Each one follows the same conventions. When AI generates code that calls a shared library, the interface guides it toward the correct pattern.


Structured annotations throughout the codebase. We embedded over 6,400 anchor comments that mark complex logic, flag active migrations, document architectural decisions, and provide context right where it matters. These annotations create a persistent knowledge layer – a conversation between past and present contributors that any reader, human or AI, can follow.


"The annotations changed everything," one of our engineers noted. "The AI went from guessing at our patterns to reading them. The speed became trustworthy."


Federation boundaries that limit blast radius. Our 25+ services each evolve independently while the gateway maintains stability. A change in one domain cannot cascade into another. For AI-assisted development, this isolation is what makes rapid iteration safe rather than just fast: the blast radius is bounded by design, not by caution.


Observability baked in from the start. Distributed tracing and structured logging, enforced at the lint level. When development velocity exceeds what manual review can fully absorb, automated monitoring becomes a primary quality mechanism. The system surfaces problems. We do not have to hunt for them.


## The result


There are two numbers that define what happened, and they measure different things.


AI-written: 30% of the production codebase. Our codebase spans 1.6 million lines of TypeScript across 25+ integrated services – and approximately 486,000 of those lines were authored primarily by AI. Not suggested, not completed, but written: production code spanning tests, type definitions, and business logic that passed every gate and shipped. The result is a 3.5X productivity multiplier measured against our pre-AI baseline – story points shipped per sprint, tracked across several months of AI-assisted development and compared against an equivalent prior period at the same team size. Uptime, bug rate, and test pass rate showed no measurable degradation across that same period.


AI-assisted: 97.5% of all commits. Over our measurement period, a team of fewer than ten engineers sustained roughly 60 commits per day – totaling 1,265 AI-assisted commits – with AI meaningfully involved in nearly everyone: reviewing, suggesting, refactoring, generating. AI is not a tool that a few power users adopt. It is present in virtually everything we ship. That is not a sprint. That is a sustained operating tempo from a focused group of engineers, each one pairing with AI as part of their daily workflow.


Together, these two metrics paint a picture that neither tells alone. Nearly a third of the codebase is AI-authored. Virtually every commit is AI-touched. This moves beyond the typical model of AI-assisted development into something genuinely new.


None of this velocity came at the expense of reliability. The platform serves approximately 63 million requests every 24 hours at 99.999% availability. It did not just survive the increased development velocity. It thrived under it. In fintech, reliability is not a technical metric – it is a promise. The same architectural discipline that makes AI contributions trustworthy also makes the system resilient enough to keep that promise.


What used to take 2 to 3 weeks – standing up a new upstream data integration with proper error handling, logging, testing, and documentation – now takes 2 to 4 days. Faster integrations mean faster partner onboarding, faster approvals, and ultimately a better experience for the people navigating their path to homeownership.


"We used to choose between moving fast and keeping things clean," one engineer reflected. "We don't make that choice anymore."


## What we learned


### 1. Start with strong guardrails – then evolve them as fast as the AI does


We came in with a strong architectural foundation – but the guardrails did not stay static. Every time AI pushed the velocity higher, we discovered new edge cases the tooling needed to handle. Every improvement to the architecture made AI more effective. The standards and the AI scaled together, in a continuous loop, while the velocity kept compounding. The lesson is not "guardrails before AI" – it is "never let the guardrails fall behind."


### 2. Context is the real multiplier


A legible codebase matters more than a capable model. The investment in codebase clarity pays compounding returns – every annotation written once is read thousands of times.


### 3. Treat AI like a contributor, not a tool


The discipline of holding AI to an identical quality bar prevents the two-tier codebase problem – and forces you to build enforcement that benefits every contributor. When we stopped thinking of AI as a tool and started thinking of it as a team member, that is when the real gains began.


### 4. Architecture amplifies engineers – AI included


Seven custom plugins catch violations consistently and early. At scale, you cannot rely on human attention alone for structural consistency. Culture sets the direction. Tooling holds the line. And when the tooling is strong, it does not just constrain – it amplifies. Our engineers spend less time on mechanical review and more time on the creative, high-leverage work that drew them to engineering in the first place.


## What this means for engineering leaders


These are the principles that held up for us – and we think they generalize:


- AI adoption without architectural enforcement increases entropy. The faster AI generates code, the faster a codebase diverges – unless structure holds it together.
- Codebase legibility matters more than prompt engineering. Invest in making your codebase navigable – the returns compound.
- Guardrails must evolve as fast as the models. Static rules get outpaced. Build enforcement that can adapt alongside the tooling.
- Productivity gains are only durable when quality is mechanized. A velocity spike that degrades the codebase is not a gain. It is a loan with interest.


## How this connects to how we operate


None of this was a one-time effort or a special initiative. It was the product of engineering principles applied consistently under pressure – the same principles that define how Product Engineering at Rocket operates every day.


- When we held the quality bar for AI-generated code, that was Client Driven Quality.
- When engineers owned the guardrails alongside the velocity, that was Ownership and Accountability.
- When the architecture bounded blast radius and the observability layer surfaced problems before we had to hunt for them, that was Technical Excellence and Continuous Improvement.
- When security and correctness were architectural decisions, not post-hoc reviews, that was Secure-by-Design.
- When we shipped metrics instead of impressions, that was We Measure Impact, Not Hype. The story and the principles are the same story.


## Building toward the mission


Behind every metric in this post – every commit, every request, every line of code – is a purpose that matters. We help people navigate homeownership, one of the most significant financial decisions anyone makes. That is what Client Driven Quality means at its foundation: quality is the trust clients place in us; engineers own the production outcome – not just the delivery, and the bar does not flex under time pressure. The people on the other side of these systems are counting on us to hold it.


That connection is not abstract to us. It is what drives the determination to hold the quality bar when it would be easier to relax it. It is the same grit that defines Rocket and the city we call home – Detroit, where resilience is not a slogan but a way of building. When we cut the time to stand up a new integration from weeks to days, it is not just a velocity win. It is a capability that reaches the people who depend on this work sooner.


And we are not done. We are investing in deeper AI-codebase integration: richer contextual annotations, smarter agent workflows that span multiple services, and tighter feedback loops between AI-generated code and our observability systems. We are exploring how AI can move beyond writing code into contributing to architectural decisions, identifying patterns across the codebase, and surfacing improvements that humans might miss. The fundamentals will not change – strong architecture, rigorous enforcement, and a quality bar that applies to everyone. What will change is the scope of what a small, focused team can accomplish when that foundation is solid.


The architectural discipline – the custom lint rules, the annotation layer, the layered service structure – isn’t only what makes AI trustworthy as a contributor, it’s what lets us ship to a standard that people's financial lives deserve. Engineers who built strong foundations are not being replaced, they’re being amplified – doing more meaningful work at a higher standard for the people who need it most. That is what belief in your team looks like in practice.


The tools will evolve. The models will improve. What will not change is our responsibility – to build systems worthy of the people who trust us with their financial future. We did not set out to prove that AI could write production code at scale. We set out to do the best work we could for the people who depend on it. The velocity and the quality are the same bet, not a trade-off. And that is the part we would not change.


### Shravan Shah


Shravan Shah is an engineering leader at Rocket with over a decade in fintech, building and scaling everything from frontend experiences to distributed backend systems. His leadership centers on removing friction so engineers can experiment, take ownership, and adapt to an AI-driven way of building software. He earned his MS in Computer Science from Virginia Commonwealth University, mentors engineers, and travels to keep his perspective fresh. He believes the future belongs to compassionate trailblazers — leaders who pair AI capability with clarity of mission and genuine care for those they serve.


## Related Resources


[5-minute read Accelerated modernization: How AI collaboration helped Rocket transform RMO Discover how Rocket engineers used Anthropic’s Claude Code and agentic coding principles to complete a 3-year PHP-to-Angular migration in just 4 months &n... Read more](https://careers.rocket.com/blog/technology-and-product/accelerated-modernization-rocket-mortgage-origination)[3-minute read Speed of development using AI Discover how Rocket® leverages AI to transform every meeting into actionable insights, driving business value, productivity, and knowledge management. Read more](https://careers.rocket.com/blog/technology-and-product/speed-of-development-using-ai)[3-minute read What happens when product teams and AI build together? As AI moves from solo assistant to collaborative teammate, product teams are discovering new ways to build together. Learn how Rocket Flow connects human exp... Read more](https://careers.rocket.com/blog/technology-and-product/product-teams-and-ai-build-together)
