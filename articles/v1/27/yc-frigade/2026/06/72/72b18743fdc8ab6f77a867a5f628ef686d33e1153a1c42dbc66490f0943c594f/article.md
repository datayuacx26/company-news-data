---
schema_version: "1.0.0"
document_id: "72b18743fdc8ab6f77a867a5f628ef686d33e1153a1c42dbc66490f0943c594f"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/a-good-animation-disappears"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-24T09:12:25.171648+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:077cc3bcf5e7a383f5a570d3b5e58fab0178005bc549d1ae858f5e37ca0a5757"
---

# A good animation disappears

Open a product for the first time and the tour finds you. The screen dims, a card lands in the middle of it, an arrow bounces at a button you were not looking at, and a little dot says there are six more of these to go. Pendo, Appcues, and Userpilot all ship a version of it. The motion is meant to read as guidance. It reads as being walked somewhere you did not ask to go.


I have built plenty of that kind of guidance over the years, and almost none of it survived contact with real users. Eight years of building UI has beaten one lesson into me: motion that performs guidance gets in the way, and motion that points at the real thing gets out of it. The good version does one job and leaves.


That is the bar we hold every animation to at Frigade. It has to explain a change the user would otherwise miss, or point them at the one thing that matters right now. Anything that does neither is noise wearing a nice easing curve, and we delete it. What follows is the handful of principles that came out of getting this wrong first, starting with how we point and then how we animate.


## Point, don't trap


your product


Connect data


Invite your team


Frigade's ping pulses on the real element, scaling up and fading to zero over 2s on an ease-out curve. When a flow has more than one stop, the same beacon flies to the next element on an ease-out-cubic takeoff and a sine-eased landing, where a tour would just advance to the next modal.


The opposite of a spotlight is a ping. It is a dot with a ring that pulses outward and fades, sitting right on the element that matters: two seconds per pulse, scaling up and dropping to zero opacity on an ease-out curve, fading in over 200ms when it first appears. It does not dim the screen or block your click. You can act on it or ignore it, and either way you stay in control of the product.


A spotlight isolates one thing by darkening everything else. A ping does the reverse, brightening one thing and leaving the rest of the product exactly as it was. The hint that anchors it wraps the real button rather than a screenshot of one, and the ping rides 12px off the edge so the target underneath stays legible.


When guidance needs more than one stop, the ping flies. Instead of advancing a modal from step one of six, a single beacon travels to the next real element along a curved path, accelerating off the mark on an ease-out-cubic takeoff and settling in with a sine-eased landing and a small vibration as it arrives. The guidance moves through the live product, not over a dimmed picture of it.


That is also why it holds up. A long authored sequence is written against the product as it looked the day someone built it, and it drifts the moment the next release ships. A ping points at whatever is actually on the screen now. There is no sequence to drift.


The ping is the clearest case, but the same restraint runs through every animation we ship, including the ones inside the assistant.


## Animate a real process, never fake one


Each word fades up from a 4px lift with a 4px blur clearing to zero, 250ms on cubic-bezier(0.22, 1, 0.36, 1), a new word every 75ms. The cadence reads as the assistant composing in real time instead of pasting a finished block.


The reveal gets mistaken for decoration. Its job is to expose a real process, not perform a fake one. Every answer streams to the user token by token, the moment the model produces each one. We do not wait for the full completion to come back before we start painting it. That is most of why the assistant feels quick: the time to the first word is close to instant, where a product that buffers the whole response leaves you staring at a blank space until the last token lands. Plenty of assistants still work that way.


Streaming raw tokens is not a free win, though. They arrive in uneven bursts, words snap into place, and half-parsed markdown reflows as the rest catches up. The easy fix is to hold the answer until it is finished and drop it in clean, which throws away the speed we just bought. The reveal is how we keep both. Each word animates in the instant it arrives, so the stream reads as the assistant composing in real time instead of a cursor twitching through tokens. The speed stays real, and the experience stays calm.


A finished block of text dropped in all at once tells you nothing about where it came from. Watching it resolve word by word tells you the system is working, in order, in real time. The motion is information about something that is actually happening, not a loading state pretending to be one.


The honest part: the hardest bug in that component was not an animation bug at all. Words would stream in and sit there, permanently blurred, never resolving. I spent a while tuning durations and easings, certain the keyframes were wrong. The keyframes were fine. The markdown renderer was being handed a fresh` components` object on every render, so React saw a new component type every 75ms, tore the whole subtree down, and remounted it. Every remount restarted the animation from blur. The fix was one` useMemo` . The symptom looked like motion, the cause was reconciliation. fwiw, that is the pattern with most animation bugs I have chased: the curve is rarely the thing that is broken.


## Animate the first time, not the tenth


First visit


Resolved


87%


Helpful


94%


Avg rating


4.6


Every visit after


Resolved


87%


Helpful


94%


Avg rating


4.6


Same page, same numbers. On a first visit the metrics count up and stagger in. Every visit after, they are just there, because by then the motion is only a half-second tax on getting to your data.


Animation earns its keep the first time someone sees a screen, and gets in the way every time after. So we tie it to the first visit and drop it on the rest.


The dashboard home page has a three-state load. Empty, then a skeleton, then the real numbers blurring and counting up into place on your first visit. The count-up is not decoration; it pulls your eye to the four metrics that matter and tells you they just landed.


The second time you hit that page, none of it animates. Return visits render instantly: no fade, no count-up, no stagger delay, gated on a module-level flag that flips 100ms after the first render. The motion that oriented you on Monday would only be a half-second tax on getting to your data by Friday.


## Most of the work is deciding what not to animate


your product


The scan overlay we built, gated behind a flag, and deleted. An 1100ms line on a cubic-bezier(0.7, 0, 0.3, 1) curve, sweeping the screen the first time the assistant thought. It looked considered and told you nothing the thinking state was not already saying.


The deletions taught us more than the launches. We built a full-screen scan overlay for the assistant once, a scanning line that swept the viewport over 1100ms the first time it started thinking. It looked great in Storybook. We gated it behind a` scanimation` flag, lived with it, and deleted it a few days later. It was answering a question nobody asked. The thinking state already had a legible signal, and the overlay just performed effort on top of it.


Same story, bigger. I once audited every animation in the dashboard, 36 Framer Motion sites plus a pile of CSS, and built a tiered system of shared transition presets to make them consistent: micro, base, exit, drawer. I migrated about twenty components onto it. Then we killed the branch with zero commits. The consistency was real, but the payoff at our stage was not, and a half-finished animation system is worse than two inconsistent ones. It was a P2 dressed up as a P0.


I am generous to the other read. Motion genuinely helps comprehension, and design research backs that up. That is exactly why restraint matters: the same mechanism that carries information one moment becomes noise the next, the instant it repeats what you already know. Deciding which case you are in is the actual work.


## A missing animation beats a janky one


Before: no exit


After: slide out


Left is what we shipped while the exit was broken, the drawer just disappears. Right is the slide-out we landed later. Enter 0.2s, exit 0.174s, both on cubic-bezier(0.16, 1, 0.3, 1).


Given the choice between no animation and a bad one, we ship none. For a while the query history drawer animated open cleanly, then just vanished when you closed it. No exit animation. Not for lack of trying. Radix tears down the dialog's DOM the instant` open` flips to false, before Framer Motion gets to run the exit, and the two race every time. I tried three ways around it: splitting` AnimatePresence` , decoupling the mounted state from the open state with` onExitComplete` , and dropping` AnimatePresence` for animate-prop switching. None of them beat Radix's cleanup.


So for months the drawer shipped with no exit at all. A missing exit goes unnoticed, but a janky one is exactly the thing people remember, so the missing one was the right call until we had a real one. We have a real one now: the drawer slides out on its own variants, enter at 0.2s and exit at 0.174s on` cubic-bezier(0.16, 1, 0.3, 1)` , the overlay fading with it. The point was never that the drawer would stay broken. It was that nothing beats a bad version of the thing, right up until you have a good one.


## What we are working on now


A tour performs guidance. A ping points at the thing and trusts you to take it from there. Perform versus point is the whole argument, and it runs through every animation in this post.


Every animation is a small sentence the interface says about itself. The good ones are short, true, and said once. We get the most out of the product not by adding more of them, but by being ruthless about which ones stay.


Three things on the motion side this month, ordered by how much they are occupying us:


1. Taking that drawer exit fix everywhere it is still missing. Plenty of dialogs and popovers still hard-cut on close because Radix and Framer Motion fight over who owns the unmount. The query drawer is the template now.
2. The shared transition system, take two, but only the presets we can prove we reuse. No speculative tiers this time.
3. ` prefers-reduced-motion` everywhere. The streaming reveal and the dashboard honor it; the ping does not yet, and a beacon that pulses forever is exactly the thing that should go still when someone asks their OS for less motion.


Most of this is the small stuff that does not have to be there, and also the stuff that makes the product feel like someone is paying attention. Try the assistant and watch the text come in. That one we kept on purpose.
