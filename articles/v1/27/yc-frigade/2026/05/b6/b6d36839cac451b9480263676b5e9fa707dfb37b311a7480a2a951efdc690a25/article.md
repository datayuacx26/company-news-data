---
schema_version: "1.0.0"
document_id: "b6d36839cac451b9480263676b5e9fa707dfb37b311a7480a2a951efdc690a25"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/why-i-took-a-physics-class-as-a-software-engineer"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-21T20:57:52.164816+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:6a4ad3dcdcfde5d5951cbd5d96c4e1935b2ab56eefc3d411cfffc62b0ea40ed5"
---

# Why I took a physics class as a software engineer

When I was in school, we had the option to take either physics or biology as our science requirement. I was getting a computer science degree and figured the closest I'd get to real physics was making games. The second I started working in engineering, I promptly forgot most of it.


Yeah, I was wrong to think that.


The Frigade assistant floats in one of eight anchor points around your product's viewport. Customers can drag it to any edge. When I picked up the drag system, it used a` setTimeout` to distinguish clicks from drags and CSS transitions to snap to rest. It worked. It also felt like nothing.


I figured I'd spend an afternoon tightening it up. That afternoon turned into a week, and the week turned into me re-learning spring-damper dynamics, numerical integration stability, and rubber-band kinematics from first principles. Not from a textbook. From bugs.


Eight anchor points. Drag the assistant and watch it spring to the nearest one.


## What the drag was actually missing


Three things were broken, and each one pulled me a step deeper into physics I didn't know.


1.


The 150ms timer. Click versus drag was decided by time, not distance. Every interaction started with a dead zone where the assistant sat still while the system decided what you were doing. Replacing it with a 5-pixel distance threshold was simple. It was the last simple thing.


2.


Viewport quartiles. The landing anchor was chosen by which quarter of the screen your pointer was in. A fast flick to the left would land right if you released on the right half of the screen. Fixing it meant learning velocity estimation: a rolling sample buffer of the last 100ms of pointer motion, projected forward by 150ms to predict where the flick is heading. Suddenly I'm computing vector magnitudes and projecting coordinates. This is physics.


3.


CSS transitions for the snap.` transition: transform 300ms ease-out` can't accept initial velocity. Every release killed the momentum and restarted from zero. Fixing it meant building a spring.


Without projection, this release snaps to the right anchor. With 150ms of velocity projection, it snaps left — where the flick was heading.


That's where I hit my limit. Not even joking, I signed up for a real online class on spring physics. I have a nice little book to show for it.


## The spring has opinions


A spring-damper system has three constants, and each one changes how the drag feels. They're the three knobs of the simulation. I spent a few days tuning these before the drag started to feel right.


**Stiffness** (k = 700) controls how aggressively the assistant pulls toward the target anchor. Too low and the assistant floats past. Too high and it jerks.


k = 100 (too soft)


k = 700 (ours)


Low stiffness crawls to the target. Our value snaps.


**Damping** (c = 52) controls how quickly the oscillation dies. This is the one that taught me the most. I initially set it to 2 and the assistant bounced like a trampoline for a full second before settling.


Our first pass landed at k=400 with c=50, a damping ratio of 1.25 — overdamped, no bounce at all. It worked. Then Eric dragged it and said it felt like floating on ice. The assistant coasted toward the anchor asymptotically instead of committing. The analogy he gave me: imagine each anchor is a hole in the surface, and the assistant is a ball. The ball should slot into the hole, not drift toward it.


That feedback changed two constants, not one. We bumped stiffness from 400 to 700 so the spring pulls harder toward the target, then adjusted damping to 52. At k=700, critical damping is about 53, which puts us at a ratio of ~0.98 — just barely underdamped. The assistant accelerates into the anchor instead of floating toward it. One clean settle, no rattle. Ball drops into the hole.


Note how long it takes for the animation to replay. That's actually how long it takes the element to settle.


c = 2 (trampoline)


c = 52 (ours)


The thin line is the target anchor. At c=2, the dot oscillates for over a second. At c=52, one smooth settle.


**Mass** scales with the assistant's visible area: a compact pill weighs about 1.4 units, the input bar about 2.5, and a fully expanded card stack caps at 3. Heavier assistants coast longer on a flick, which is what you'd expect from something that looks bigger.


m = 1.4 (compact pill)


m = 3 (expanded cards)


Same flick, same spring. The heavier assistant takes longer to slow down.


The thing nobody tells you until you tune it yourself: the damping ratio (` ζ` , the one with the fancy symbol) has to stay constant as mass changes. Without scaling damping by` sqrt(mass)` , expanding the cards made every flick bouncier. Physically correct, maybe. Felt wrong. The math is` ζ = c / (2 * sqrt(k * m))` . At our values (k=700, c=52, m=1), that's about 0.98. If you want ζ constant when m changes, c has to grow with sqrt(m). I learned this by watching the assistant bounce wrong for twenty minutes and then googling "damping ratio derivation."


The spring runs in a` requestAnimationFrame` loop. Each frame:


```text
vx = (vx - k * dx * dt / m) / (1 + c * dt / m)
x  += vx * dt


```


Framer Motion or React Spring have spring primitives that could do this. But their APIs are built around declarative targets, not pointer-driven simulations where the initial velocity comes from a rolling sample buffer and the mass is derived from a live` getBoundingClientRect` . I would have spent more time fighting the abstraction than writing the equation.


## Two bugs that nearly shipped


The spring worked. Then two bugs nearly made it to production, and each one taught me more physics than the class did.


**The flicker.** After the spring settled, the assistant appeared at its pre-drag position for one frame before snapping to the final anchor. I spent an embarrassing amount of time adding` console.log` s before I found it: the settle function wrote the new position to React state, but React's flush landed one frame late. For that single frame, the DOM had old CSS positioning and the new transform at the same time.


Fix: write the CSS properties imperatively in the same tick as the transform. Don't let React own the DOM write during an animation. The physics loop is the source of truth, and the write has to live there.


**The fling.** This is the one that sent me to Wikipedia for an hour. Fast flicks past the viewport edge launched the assistant clean off the screen. It would vanish, then pop into the correct anchor with no animation.


Two things compounded. The spring was seeded with raw pointer velocity, but starting from the rubber-banded assistant position. Past the rubber-band cap, the pointer keeps moving fast while the assistant asymptotes at about 15px past the clamp. So the spring got full pointer energy from a near-resting position, which is like kicking a ball that's already against a wall.


The integrator made it worse. I was using explicit[Euler integration](https://en.wikipedia.org/wiki/Euler_method) , which is only conditionally stable. There's a threshold: when damping exceeds a certain ratio relative to the timestep and mass, the simulation doesn't just lose accuracy. It grows. Every frame adds energy instead of removing it. Frame hitches pushed` dt` close to the stability boundary, and the overdamped config tipped it over.


explicit Euler


implicit Euler (ours)


Same spring (k=700, c=52), same 55ms timestep. The only difference is where the damping term lives in the equation.


I figured out the correlation myself during a tuning session: increasing damping past critical was causing the blowup. The fix was switching the velocity update from explicit to implicit damping. The derivation is just solving for` v_{n+1}` on both sides of the equation, but it makes the integrator unconditionally stable for any positive damping value. I also capped release velocity at 4 px/ms and added a safety net that force-settles if any value goes non-finite or exceeds three times the viewport.


A game developer would know this from day one. I learned it from an assistant that was flying off the screen.


## Pulling it apart


Once the bugs were fixed, I pulled all the physics into a pure module:` draggable-physics.ts` . No React, no DOM, no window. Settings as arguments. 26 Jest tests cover rubber-band asymptotes, mass scaling, spring convergence, unconditional stability under large timesteps, and a regression test that verifies the fling's excursion bound holds under capped velocity.


For what it's worth, the extraction made the integrator bug obvious in hindsight. When the spring math lived inside a 400-line React component tangled with pointer handlers and resize listeners, the stability issue read as a visual glitch. Once it was a pure function with a test that could pass` dt = 0.1` , the conditional stability was impossible to miss.


The textbook had all of this. I just couldn't make any of it stick until I was watching it go wrong on a screen and had to chase the bug back to the math.


## What we're working on now


1.


Expanded-state drag. Dragging with cards open still feels off. We tried collapsing to compact on drag start and immediately reverted it. Watching the content vanish mid-drag was worse than the problem it solved. Next attempt: dim the cards and animate into position on drop only.


2.


Keyboard repositioning. The drag system is pointer-only right now. Arrow-key navigation between anchors is the obvious gap.


3.


Velocity estimator accuracy. The current estimator averages across the full 100ms sample window. If y'all decelerate before lifting, the average under-samples the actual release velocity. Narrowing to the last 30ms should tighten the projection on slow releases.
