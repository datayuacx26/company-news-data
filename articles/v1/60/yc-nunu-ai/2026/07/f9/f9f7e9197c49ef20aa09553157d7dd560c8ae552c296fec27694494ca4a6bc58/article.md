---
schema_version: "1.0.0"
document_id: "f9f7e9197c49ef20aa09553157d7dd560c8ae552c296fec27694494ca4a6bc58"
company_key: "yc-nunu-ai"
company: "nunu.ai"
source_id: "yc-nunu-ai-news-import-9ab4e6dc8961"
canonical_url: "https://nunu.ai/blog/apk-arena"
published_at: "2026-07-10T12:41:00+00:00"
first_seen_at: "2026-07-22T22:59:24.636969+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:70f3e337b316d12123af9ea95dd10860c7e5def90ef1b1f755985e1a1efa6209"
---

# APK Arena: A Benchmark for Phone Use Agents

At[nunu.ai](http://nunu.ai/) we build a harness for AI agents to perform game and app testing on phones, which means we need precise device interactions, good long-term planning to execute test cases, and vision good enough to catch bugs on screen.


So we built our own benchmark: 50+ real-world problems we encountered, which we now run against SOTA models with our agent harness.


check out the[github repo](https://github.com/nunu-ai/apk-arena) ➡️


## What is APK Arena?


Popular Phone Use benchmarks like[Android World](https://github.com/google-research/android_world) are slowly saturated, annoying to set up and don’t include what is very important for our use case: games! We all love watching AI agents playing games, so inspired by the[WebGames](https://webgames.convergence.ai/) , we decided to make our own benchmark.


Each level comes from a real problem we hit while building our own agents, maybe a gesture that kept failing, a game our agent played badly, a task it couldn't complete reliably. Turning those into levels gives us fast, repeatable insight into how different models and different harness changes hold up against our actual use cases.


### Level Design and Methodology


APK Arena currently has 50+ levels spread across seven categories:


- 👆 **Primitives** - Basic touchscreen control and fine motor accuracy — tapping, swiping, complex gestures
- 👁️ **Vision** - Reading the screen: counting, matching, visual search
- 🧠 **Memory** - Detecting important information and recalling it across long tasks
- 🧩 **IQ** - Reasoning and rule induction, mostly puzzles
- ⏱️ **Tempospatial** - Temporal and spatial reasoning
- 🎮 **Games** - Multi-step games requiring strategy
- ✅ **Tasks** - Real workflows: using phone UI, following multi-step instructions


Each category maps back to a capability we actually need in production.


### How does scoring work?


Every level exposes a score between 0-100%, what the score means depends a bit on the level. We usually have these scoring paradigms:


- **Games:** are scored on in-game performance over a fixed time. The 50% baseline is usually our own human score from playing the game for the same amount of time. This makes a lot of sense as most people expect the agent to perform at the same speed and efficiency as a human if not better 🙂
- **Vision, Memory, IQ and interactions:** levels are scored on straightforward accuracy: did you count the correct number of objects? did you tap the right target? it’s very clear black and white scoring
- **Tasks** are scored programmatically against how much of the workflow was completed correctly, getting punished for making certain mistakes, the same idea as vision accuracy, just applied to a longer sequence of steps.


## How the Agent interacts


We run APK Arena with the nunu.ai harness: a simple agent loop wrapped around a code-execution sandbox. The sandbox exposes functions to grab screenshots and perform actions on the connected device. The agent freely takes screenshots, writes code to analyze them, and then performs actions.


python


```text
import   nunu    await   nunu  .  tap  (  x  =  806  ,   y  =  1461  )     # close popup     await   nunu  .  sleep  (  400  )    img   =     await   nunu  .  get_screenshot  (  display  =  True  )
```


There's no structured state, no accessibility IDs, and no text description of the screen at any point, the model reasons entirely from pixels and its own code output, with no level-specific guidance beyond what's visible.


Many games simply don't expose accessibility IDs, so we don't rely on them anywhere in the harness. As a bonus, this also means the agent is tested the way a human actually operates a phone, with no dev tools or hooks to fall back on.


## Level Examples


### 103: Count the Dots


Writing down a sequence of dice, sounds very simple but until the last generation of models, this was a surprisingly hard problem for no good reason.


They would mix up 4s and 5s, confuse 2s and 3s, and not see the out of distribution dice we snuck in.


GPT-5.5, Opus 4.8, and Fable 5 are the first models to actually handle this quite well, being able to read dices with rotation and even being able to detect for example the out of distribution dice with 7 pips.


### 403: Sequence Memory


One of our tempospatial levels works like the classic "watch the sequence, then repeat it" game, a 3x3 grid lights up tiles in order, and the agent has to reproduce that sequence back.


A few months ago, this level was impossible for us to do.


Nowadays with code execution models just ... solve it, by writing a frame buffer script that continuously pulls screenshots and detects the active tile using computer vision.


### 507: Sector Defense


Part of the games category, we have a tower-defense level: survive as many waves as possible in a fixed time.


It combines almost everything we care about at once: real-time strategy and economy management, planning ahead for camo waves that need a specific counter, and just plain accurate swiping, since placing a tower precisely next to a winding path turns out to be a lot harder than the visible grid makes it look.


It's very fascinating watching different models converge on completely different strategies. Some clearly struggle with placement accuracy, but some actually realize that they suck at placing towers, so they resort to randomly placing sniper towers. With their infinite range, it doesn't matter where you put them.


Stronger models make better use of the map, placing boomerang and AOE towers on efficient tiles and remembering to mix in a ninja tower to clear the invisible camo blobs.


## Using the benchmark


If you want to run the benchmark yourself, the app is available on our[github repo](https://github.com/nunu-ai/apk-arena) - we'd love to see more results.


## What's Next


The benchmark is very much a living thing for us. So it will hopefully keep evolving in the future and keep cool updates coming.


- Adding new failure modes and new games as we encounter them.
- Running the latest state-of-the-art models to see where they land.


[<< back to blog](https://nunu.ai/blog)
