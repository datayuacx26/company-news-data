---
schema_version: "1.0.0"
document_id: "ac5ddc3d3bd9d6612f7985db9284d867ffe106bfc8e615e733dc910fd7619170"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/describe-the-world-not-the-widget"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-24T09:12:25.171648+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:c56f4f77c9b048155a0b2e4d5f00c7c6f9f12cb5641e96911dc4ce9814221df7"
---

# Design prompting: describe the world, not the widget

Animations are hard to describe. Most of us don't have the vocab (keyframes, easing curves, durations) to spell out what we want, so we end up typing things like "make it pulse on hover, kind of a blue glow, soft, not too aggressive" and the model gives back something fine. Not what we wanted. We try again, hedge a few words, get something else fine. The loop is real.


The approach we've been getting good results from doesn't require learning that vocabulary. You describe a world for the animation to live in and let the model derive the timing and feel from there.


## The story behind this post


A few weeks ago I was working on the highlight inside our assistant. The product is built around a small blue ping that travels across the screen and lands on the element it's pointing the user toward. Customers told us the landing didn't register clearly enough. The ping reached the target, but the moment of arrival was too subtle to feel.


The next version added an animated treatment on the border of the target element, triggered on landing. My first few drafts described the new treatment as a list of properties: an outline that grows, a glow that fades, a color, a duration. What came back looked fine and felt flat. I switched to describing the motion as a physical thing, something with weight and travel time and a place it starts and a place it ends. The version that came back from that clicked on the first try.


The move that mattered was switching from enumerating properties to describing a physical thing. The loop ended on the next try.


## What metaphors actually do for an LLM


There's a mechanical reason this works. A metaphor ships with a coherent world attached, and a property list doesn't. When you tell a model "make it pulse softly, blue glow," you've given it a list of properties to satisfy and nothing to connect them. The model picks defaults for everything else and the result feels stitched together.


Try the other way. Imagine a stone trough that runs along the top of a castle wall in a siege, the kind defenders pour glowing liquid into. The liquid travels along the trough until it spills out the far end. The glow is the only thing alive on the wall. Everything else holds still.


A castle channel is slow because the liquid is heavy. A castle channel glows because the liquid is hot. A castle channel ends when the trough does. None of those are details the model had to invent.


There's a second thing the metaphor buys you: density. Describe an animation literally and you are stuck with a bad trade. Either you enumerate every property, which means knowing it exists and what it is called (the exact filter, the easing curve, the color stops), or you keep it short and let the model guess the rest. A metaphor refuses the trade. "Molten metal" is two words carrying temperature, color, weight, flow, and the way the glow dies when the source cuts off. That is the densest way to hand the model a world, and it never makes you name the parts.


Lakoff and Johnson made this argument forty years ago about humans, in *Metaphors We Live By* . We organize abstract reasoning in the shape of physical experience. The argument works on language models for the same reason it works on us: most of what we know is stored as worlds, not as specifications.


## Try it: a button


Here's a plain button. No animation, no hover state.


We asked two separate Claude instances to animate it on hover. No shared context, same starting button.


**Prompt A** is how most of us actually describe animations:


> On hover, do a little ring of gradient around the outside of the button. The ring should start from where the cursor enters and flow both directions until it meets on the opposite side.


**Prompt B** swaps the description for a world:


> On hover, do a little ring of gradient around the outside of the button. Imagine the button border is a channel, and molten liquid is flowing through the channel and filling the perimeter. The liquid first starts from where the mouse enters the button, and flows both directions until it meets.


What came back from A (hover over the button):


What came back from B (hover over the button):


A spins. The gradient is a rotating loop of purple, pink, and amber, with a small gap that travels around the perimeter forever. The cursor entry sets where the gap starts. It looks fine, but it's not what I asked for. The prompt said "flow both directions until it meets on the opposite side." A didn't do that. It defaulted to a fancy-border pattern and never built a stopping point.


B did what I asked. The molten gradient starts at the cursor, flows both ways around the rim, meets on the far side, and holds. When the cursor leaves, it drains in reverse. It's also orange and red, with a soft heat glow around the rim. None of that color or glow was in the prompt. The metaphor said "molten liquid," and the model knew what molten looks like.


The literal prompt gave the model a list of behaviors to satisfy. It satisfied "ring of gradient" and "cursor entry," then reached for the nearest familiar pattern for everything else. The metaphor gave it a world that already had rules: liquid fills a channel and stops, liquid drains when the source goes away, molten metal glows orange. The model rendered the world, and the behaviors fell out of it.


## The principle


The fastest way to get a coherent artifact out of a language model is to hand it a coherent world to draw it from. A spec gives the model a list of constraints. A metaphor gives it a generator. The list will produce something that satisfies each constraint locally and lands dead on the page. The generator will produce something that holds together because every detail came from the same source.


This shows up most clearly in animation, because animation is the part of the interface where every micro-decision (timing, easing, color temperature, rest) compounds into either coherence or noise. But it generalizes. Anywhere the work depends on a hundred small choices fitting together, telling the model the world the work lives in is cheaper than telling it the choices.


Next time you fire up your favorite LLM to animate something, write a sentence about a castle wall instead of a list of keyframe instructions. Hover the result. You'll see what I mean.
