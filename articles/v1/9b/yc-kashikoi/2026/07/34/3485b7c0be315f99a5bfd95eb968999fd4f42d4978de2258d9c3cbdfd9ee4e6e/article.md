---
schema_version: "1.0.0"
document_id: "3485b7c0be315f99a5bfd95eb968999fd4f42d4978de2258d9c3cbdfd9ee4e6e"
company_key: "yc-kashikoi"
company: "Kashikoi"
source_id: "yc-kashikoi-rss-651c469da509"
canonical_url: "https://blog.getkashikoi.com/2026/07/08/systematizing-context-capture/"
published_at: "2026-07-08T16:41:48+00:00"
first_seen_at: "2026-07-25T10:30:20.185118+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:e0607fe649b6f8f701dabd3ed0c07742da3cbf8b4592ec1117704b8cd22fe8d5"
---

# Systematizing Context Capture

Most people (a.k.a LI “thought leaders”, VCs, non-operators) who can talk about “Context” glibly have never had a painful encounter with the long tail or with the humans in whose heads that context actually lives.


To be more precise, humans who hold deep knowledge in their minds but are not incentivized, not rewarded, and not given the right scaffolding to translate that knowledge into a scalable system.


Kashikoi is down in the deep depths of a customer deployment, and I’m compelled to write up a retrospective. That nagging feeling of seeing history repeat itself (not rhyme, actually *repeat* ), so here’s that story.


The gap between what a person knows and what a system can use is the most underpriced problem in AI. I’ve spent a good part of my career standing inside it. It is also the problem we built Kashikoi to solve.


### The Three Levers


Something close to magic happens when context trapped in a human’s mind finally makes its way into a scalable system.


Three levers control whether it flows at all:


- **Incentive** — a reason for the person to talk.
- **Reward** — proof that talking changed something.
- **Scaffolding** — the structure that lets their knowledge flow without friction.


Unlock one and you get a trickle. Unlock all three and you get a flood. Most teams unlock none of them, and then wonder why their models are fluent everywhere and useful nowhere.


### One Small Model, Three Giants


Early in my career I worked on a system I’ll call Meridian. TL;DR: a single module ended up having an outsized impact on **two powerful governments and one trillion-dollar company.**


Deep learning reshaped computer vision and natural language processing on the back of one underappreciated resource: data. And who has massive amounts of data represented on the internet? The answer sounds great until you build ML that touches the real world every day. Then you run straight into its limit.


What about the under-represented *majority* ? The use cases that are routinely, and incorrectly, called a “minority”? The languages, the regions, the ways of speaking that the internet barely records? How do you make a model built for the represented *also* work for the under-represented?


That under-represented majority has another name. **The long tail.** It is notoriously hard to model, because making something built for the center also serve the edges takes a particular blend of expertise and creativity.


I found myself there early in my career. A senior engineer had tinkered with a classical language-modeling technique to chip away at the problem. I was handed the tinkering and one instruction: scale it, and see if it survives the only test that matters: global scale.


So we scaled it, for languages and regions deep in the long tail, weaving location and speech signals together in a privacy-conscious way to improve how well the system understood long-tail words.


Now here’s the fun part.


After their global release, a powerful, conservative government flagged these models and placed them **under export control.** *“Let’s stop the entire thing”* , the classic way regulation deals with what it doesn’t understand. The whole platform could operate across borders. These set of edge models could not. Their bits and bytes were not permitted to cross the optical fiber beyond the country’s physical border. Digital bits, treated like physical cargo.


At the time it felt absurd: a regulator treating a model’s weights like a crate of munitions at a customs checkpoint. It doesn’t feel absurd anymore.


#### Rhyme & Repeat


In June 2026, the U.S. Department of Commerce placed export controls on Anthropic’s most capable models, Claude Fable 5 and Mythos 5 under the Export Control Reform Act, the very machinery built for physical armaments, barring access by any foreign national, inside or outside U.S. borders. With no way to verify nationality at the API layer, Anthropic did the only thing compliance allowed: it shut the models down for *everyone* , worldwide, for roughly three weeks, until the order was withdrawn at the end of the month.


We read that headline differently than most people did, because we’d already lived a version of it years ago. When a government reaches for export control, it has decided your bits are powerful enough to be dangerous and its first instinct is the bluntest one: stop the whole thing.


Going back, in an internal report at the trillion-dollar company, someone measured how often the model was actually used.


> 35 times. Every second. Every day.


Let that sink in. The kind of “engagement” number that would give many product people an out-of-body experience. So much, by the way, for the long tail being *infrequent.*


Sometimes the proof that your work matters arrives in the form of a government file.


### Lat, Long, Load


The mechanism is simple enough to describe but the devil is/was in the details and eventually in its consequences.


The algorithm translated an opted-in person’s latitude and longitude into a **pixel on a map.** Based on the geographical cluster that the pixel fell into, we **interpolated, at runtime, a specialized edge speech-recognition model** that recalculated the probabilistic weights for exactly the entities that were more meaningful at that location.


Pulling that off takes three things at once: smart model training, smart engineering to actually fit a model onto the CPU of a phone and run inference there. And for any of this to be worth the trouble, *meaningful context to train on.* The right data.


(Some more technical aspects/benchmarking are detailed in the[ICASSP paper](https://ieeexplore.ieee.org/document/8462550) ).


### Fourteen Timezones


The differentiating data came from a small group of extraordinary linguistic experts, equipped with a tool we built for them.


They could run millions of entities through the simulation tool (leveraging TTS+ASR internals) and have the system surface back to them the ones ASR most often confused: the cases where the entity ranker’s probabilities weren’t differentiated enough to be trusted.


My team gave them the ability to pass any vendor’s or external database’s list of entities through the system and get back a *subset* . The subset of entities for which, if they supplied abbreviations, colloquial names or authentic pronunciations, the model would measurably improve.


The best use of a domain expert’s knowledge is directing their expertise at exactly the cases where the model is failing. That is incentive, reward, and scaffolding in a single interface. Incentive: the work obviously mattered. Reward: they watched the model improve. Scaffolding: the system did the searching.


### Why Markets Miss This


Edwin Chen bootstrapped Surge AI to over a billion dollars in revenue with around a hundred people and no venture funding while a far better-known, far better-capitalized rival burned through money chasing the same market. His edge came from years of frustration: at every company he’d worked at, getting *quality* data was a disaster. At Twitter, his team had to label fifty thousand businesses and hired a vendor; the data came back as junk — restaurants labeled as coffee shops, coffee shops labeled as hospitals.


We have lived that exact experience.


Here’s what almost no one says out loud: the echo chambers inside the biggest companies are *impervious* to junk data. It doesn’t touch them. And because promotion incentives reward shipping the shiny thing, not fixing the unglamorous foundation, none of it matters to anyone with the power to change it. To fix the long tail you need someone who has made it their **survival** problem, someone who can say, and mean it, *“that’s the hill I’m going to die on.”*


The same blindness shows up today in how we evaluate models.


### The Principle


Simulation went a long way toward proving the ROI of all that quiet work : the experts, the dialects, the entities no benchmark would ever have caught.


And the principle generalizes cleanly into the world of LLMs and context capture today:


> **Brute-forcing context through prompt engineering will not work.**


You still need good *listening* systems plugged to humans who are incentivized to talk. And then the algorithms and engineering that can digest all of it and bring the right piece of context to a human at the exact moment it’s useful. Incentive. Reward. Scaffolding. The three pillars don’t change just because the model got bigger.


Kashikoi brings that systematization ethos to evaluation: simulation-for-eval. Instead of optimizing toward the applause of a leaderboard, we build the verification systems that catch where a model actually fails the long tail. So teams don’t ship models that benchmark beautifully and break in the field, and so the broader system doesn’t sleepwalk into a junk-data fate at AGI scale. Same ethos, new frontier.


### ELI5


Over the years that the team worked on this system, it has been clarifying to explain it to multiple people from their point of view. Besides such thought experiments are superbly fun: “What would it take to ELI5?” etc. Here are some of those thought experiments:


- **For a Product Manager:** It’s last-mile delivery for a speech model.
- **For a Physicist:** Models already bend to time (sequences). We taught one to bend to space, a word’s probability shifting with the coordinate you’re standing on.
- **From a Computer Vision lens:** It maps a user’s lat/long to a pixel on a world map, reads the regional cluster that pixel lands in, and serves a model specialized for that region, the long tail reframed as a spatial problem.
- **For a CEO:** It makes our speech recognition actually work for the under-served majority: the billions whose languages and places the internet barely represents and it does it privately, on the device.
- **For a CTO:** It’s a runtime architecture: from a device’s coordinates it interpolates and loads a region-specialized edge model, with no voice data ever leaving the phone.
- **ELI5:** Your phone knows what town you’re in, so it learns the funny names of the shops near you, like a friend who grew up on your street, and it figures it all out by itself, without whispering your voice to anyone.


### References


*Public coverage of the region-specific language modeling work referenced here:*


- *[machinelearning.apple.com/research/regionally-specific-language-models](https://machinelearning.apple.com/research/regionally-specific-language-models)*
- *[TechCrunch](https://techcrunch.com/2018/08/09/siri-is-now-trained-to-recognize-your-local-weirdly-named-small-businesses/)*
- *[Geographic Language Models for Automatic Speech Recognition](https://ieeexplore.ieee.org/document/8462550)*


*On the data-quality parallel, see[Edwin Chen / Surge AI](https://www.linkedin.com/posts/henrythe9th_i-just-met-a-savage-ai-founder-who-bootstrapped-activity-7346588358086709248-vXxe) .*


### Like this:


Like


Loading…
