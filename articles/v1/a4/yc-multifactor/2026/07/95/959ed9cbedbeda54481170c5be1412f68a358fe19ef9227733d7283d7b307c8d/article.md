---
schema_version: "1.0.0"
document_id: "959ed9cbedbeda54481170c5be1412f68a358fe19ef9227733d7283d7b307c8d"
company_key: "yc-multifactor"
company: "Multifactor"
source_id: "yc-multifactor-news-import-864f66d5ce56"
canonical_url: "https://multifactor.com/blog/becoming-a-design-engineer"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T21:20:01.229652+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:9789529c7fb48ab818df8497850cab36c04d5236ec0137950168e198eecd693f"
---

# I'm a Designer. Last Week I Shipped Dark Mode to Production.

# I'm a Designer. Last Week I Shipped Dark Mode to Production.


2026-07-27


[Company](https://multifactor.com/blog/tags/company)[Design](https://multifactor.com/blog/tags/design)[Agentic](https://multifactor.com/blog/tags/agentic)


[JB James Burns Design Engineer](https://multifactor.com/blog/authors/james)


No ticket. No handoff doc. No waiting for an engineer to find a free afternoon. I opened the design, described what I wanted, reviewed the diff, and merged it. The whole thing went out before lunch.


A year ago that sentence would have made no sense coming from me. I'm a designer — Figma is my native language, not Git. But the gap between “I designed it” and “it's live” has basically closed, and I want to walk through exactly how, because I don't think this is a James thing. I think it's where a lot of product design is heading.


Dark mode, live in the Multifactor app.


## The old loop


Here's how a UI change used to go.


I'd design it in Figma. Pixel-perfect, every state, every breakpoint. Then I'd write a handoff doc — spacing, tokens, hover states, edge cases — and pass it to a frontend or full-stack engineer. They'd build it when they had bandwidth, which was rarely *now* . It would come back about 90% right, with the last 10% being exactly the details that make design feel designed: the easing on a transition, the way a card reflows at 380px, the contrast on a disabled state. So I'd file feedback. They'd fix it. We'd review again.


The old handoff loop: the design intent and the code lived with two different people, and every round trip across that boundary cost days.


Nothing here was anyone's fault. It's just that the person who held the design intent and the person who could change the code were two different people, and every round trip between them cost days. For a product like Multifactor — where the surface is a password manager people live inside, and small UI details directly shape whether the thing feels trustworthy — those days add up fast.


## Joining Multifactor and the moment the loop collapsed


When I joined, I came in as a designer. The shift wasn't a job-title change; it was a tooling change. I started chaining together three things that, together, let me hold the design intent *and* change the code:


- **Figma MCP** — exposes the actual design context (variables, components, layout, tokens) from a Figma file to an AI coding agent. Instead of an engineer eyeballing my spec, the agent reads the design directly.
- **Claude Design** — where I iterate on the design itself in a canvas, conversationally, before anything touches the codebase.
- **Claude Code** — the agentic coding tool that takes the design context and the change I'm describing and produces an actual pull request against our repo.


The toolchain in one shot: Figma open, Claude Code pulling design context through the Figma MCP, and a pull request opening on the other side.


The unlock is the MCP connection. The design stops being a picture I describe secondhand and becomes structured context the agent can read — the same variables and components I'm working with in Figma. So “match the design” stops being a game of telephone.


## The receipts


Theory is cheap. Here's what's actually gone to production through this workflow.


### Dark mode


The same screen, light and dark.


Dark mode is the canonical “easy for users, miserable for the codebase” feature. It's not one change — it's every surface, every token, every state, audited for contrast and consistency. The old version of me would have specced all of it and handed off weeks of work.


Instead I worked through it surface by surface: iterate the theme in design, let Claude Code apply the token changes across components while reading the real design context, then review the diff like I'd review anyone's PR. The parts that needed human judgment — accessibility contrast on edge cases, a couple of components with custom states — I caught in review and corrected in the same loop.


The dark mode pull request — reviewed and merged like any other.


That's the receipt I care about most: dark mode went out as a single reviewed pull request that touched **55 files** — +880 / −339 — the kind of change that used to mean weeks of borrowed engineering time. One concrete number beats a paragraph of adjectives.


### Responsiveness fixes


The Multi agent chat, reflowing cleanly from a wide column down to mobile.


Responsiveness bugs are the other classic handoff tax — they're invisible until you're at the exact wrong viewport, and they're tedious to describe in a ticket. Now when I spot one, I fix it where I see it. Take the Multi agent chat: the message list and the composer had to hold up from a narrow desktop column all the way down to a phone. I reproduced each breakpoint, described how it should reflow, verified it, and shipped — no ticket, no waiting.


### And the steady stream of small stuff


A steady stream of small fixes — the kind that used to sit in a backlog forever.


The headline features get the attention, but honestly the bigger change is the long tail — the spacing nits, the misaligned icons, the hover states that were never quite right. The stuff that used to sit in a backlog forever because it was never worth interrupting an engineer for. Those now just get fixed.


## What this did to the engineers (it's not what you'd think)


The obvious-but-wrong read is “designers don't need engineers anymore.” That's not what happened, and I'd push back on anyone who frames it that way.


What actually happened is that our frontend and full-stack engineers stopped *building my UI work* and started *reviewing* it. Their involvement didn't disappear — it moved to the highest-leverage point in the process. They're the final gate: they catch the architectural thing I'd miss, the performance implication, the “this should be a shared component” call. That's a far better use of a senior engineer than translating my Figma file into JSX.


Review didn't go away — it moved. A real approval on one of my PRs, merged to main.


So the pace on UI/UX changes went up dramatically — not because we removed people, but because we removed the round trips. The design intent and the code change now live with the same person right up until the moment of review, and engineers spend their attention where it's worth the most.


## What “design engineer” actually means now


I used to think the line between design and engineering was about skills — that to cross it I'd need to become a “real” frontend developer. I don't think that anymore. The line was never really about skill; it was about *who could act on the design directly* . AI tooling moved that line, and it turns out the most valuable place to stand is right on top of it: close enough to the design to know what's right, close enough to the code to make it so.


I'm still a designer. I just don't hand the design to someone else to make it real anymore.


Still a designer — just no longer handing the design to someone else to make real.


— James


# We're redefining zero-trust — so you can protect your accounts with confidence.


Identity is your first and last line of defense, and the root cause of most application security breaches. Multifactor's provably secure zero-trust solutions cryptographically guarantee that only authorized users can access sensitive data, turning identity into your greatest asset in the fight against cyber threats. Learn more about our research, or reach out to explore working together.


[Get Started](https://multifactor.com/get-started)[Learn More](https://multifactor.com/company/sales)


## Related Posts


[We're Hiring](https://multifactor.com/blog/we-are-hiring)


2026-03-02


[Company](https://multifactor.com/blog/tags/company)[Research](https://multifactor.com/blog/tags/research)[Agentic](https://multifactor.com/blog/tags/agentic)


Multifactor is hiring. If you are a talented engineering leader or individual contributor who cares deeply about forging the future of authentication, authorization, and auditing for the agentic era, you should probably check out our open positions.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


[Multifactor Versus Password Managers: Securing Capabilities, Not Credentials](https://multifactor.com/blog/multifactor-versus-password-manager)


2026-01-31


Say goodbye to password managers and hello to Multifactor, the world’s first true Account Manager!


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


[Meet Multifactor: Putting Your Digital Life on Autopilot](https://multifactor.com/blog/meet-multifactor)


2025-10-31


A closer look at the platform designed to unify and secure your entire digital identity.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)
