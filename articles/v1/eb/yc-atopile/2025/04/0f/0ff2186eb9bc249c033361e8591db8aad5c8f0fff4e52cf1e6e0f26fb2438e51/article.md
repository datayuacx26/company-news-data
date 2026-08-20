---
schema_version: "1.0.0"
document_id: "0ff2186eb9bc249c033361e8591db8aad5c8f0fff4e52cf1e6e0f26fb2438e51"
company_key: "yc-atopile"
company: "atopile"
source_id: "yc-atopile-rss-31511e11ba21"
canonical_url: "https://blog.atopile.io/p/how-to-win-at-hardware-testing"
published_at: "2025-04-03T18:40:19+00:00"
first_seen_at: "2026-07-24T17:27:13.472252+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:5f95740f24a5fad234643fb696ed798453fd4ba5327a95eb3b8498e7865466c1"
---

# How to win at hardware; testing

# How to win at hardware; testing


[Ioannis P](https://substack.com/@iopapa)


Apr 03, 2025


If you’ve ever worked developing any hardware, chances are you’ve seen testing that resembles a better structured version of “I ran it and it seemed to work”.


Unlike software, there’s no


` pytest` ,


` jest` or


` #\[cfg(test)\]` to take-over once pace and complexity make “I ran it” testing the major burden. More often than not there’s just blood, sweat, an oscilloscope and a PDF.


Heck, this is why you hear “we can’t change anything, it’s certified” from aerospace ✈️ . It’s not as much that you can’t and more that you’d need to re-test (read, re-assess) everything along with it and boy is certification one heck of a test suite.


## So, where’s hardware testing lacking?


-


It’s too


**expensive** to be ubiquitous. 🤑 Manufacturing lines justify the investment in reasonably thorough automated testing, and these setups frequently run into the $100s of thousands of dollars to build.


-


It’s often


**too fragile** to be orchestrated. ⛓️‍💥 For CI to be useful, it needs to be reliable. Typical HiL setups (hardware-in-the-loop) often look like a circuit board and NUC connected via some loose cables on Joe’s desk. If Joe accidentally unplugs something or, god forbid, he stands up too fast and shakes the desk results get iffy.


-


Its


**setup is too difficult** to be the norm. 😫 As mentioned, software has capable frameworks and a strong culture of testing. When you mention hardware testing, most people’s minds go to either a lab bench. The lab is manual, and requires serious tactile real-world skill to operate properly.


## Let’s think about HW testing in a similar way to SW 🧠


Okay, so how do we think about testing (in general) then?


Well, is it:


-


one-shot vs. automate runs


-


orchestrate vs. setup ad-hoc


-


log results vs. read them out


-


have shared setups to log into vs. dedicated per-user or per-test setups


-


capture statistical vs. pass-fail results


-


spot-check values vs. sweep for coverage


-


test all-up vs. mock out parts of the system


I won’t illustrate all these points, but for one example; one-shot testing is the “I ran it” and for automated testing, think CI blocking you merging your PR. In many of these cases there’s a place for both, but a SWE will most likely be opinionated as to which they trust in production.


As your project progresses, your test requirements resolve and confidence in the design increases. Testing expectations


**should** increase alongside it, Here’s one mental model we use to discuss testing:


-


**Phase** of the project lifecycle. Early prototyping, engineering, new-product-introduction and manufacturing ramp - in rough sequence


-


**Fidelity** of the test is a measure of how closely the test matches the real world. One of the biggest factors here is how much of the system is connected together and how much you're emulating or mocking


-


In Lab bench (or “Bench”) testing you typically just power up a little bit of the circuit, perhaps you stick a power supply on some rail, and you might check the output of a regulator is at a reasonable voltage. You’re only testing a tiny part of the circuit under highly synthetic conditions, so we’d think of it as low fidelity and you start this typically at early phases


-


In integration testing you’re bringing sub-systems together, so they’re each partially interacting with a true counterpart


-


In functional testing you’re typically testing on complete and final hardware, measuring off a few points on it to examine its behavior.


## How to win.


-


Make custom test equipment


-


Don’t share between jobs


-


Automate


Creating custom test equipment is, counter intuitively, usually


**significantly cheaper** than purchasing an off-the-shelf product - frequently by


**an order of magnitude** even as a one-off. The reason for this is because off-the-shelf equipment is typically made extremely generic to sell to a wide audience, meaning that it’s vastly over-spec’d and feature rich than you need. In larger volumes this trends to two orders of magnitude lower cost.


One thing here is to avoid falling into the same trap that the off-the-shelf stuff does; don’t share. Design something someone loves, don’t try fill the needs of the whole company. So instead of designing universal tools, here’s a split that’s worked well for us:


-


Lab and Bring-up equipment are owned by someone, and frequently run one-shot (multimeter, oscilloscope)


-


CI/HiL, Integration and Rel. equipment are multi-user and larger (think server rack)


-


Functional testers have to live in the factory and cycle fast (bed-of-nails, pogo jigs)


Finally,


**automate** . Automation shouldn’t need a plug, but honestly still needs to be said. Make a good scripting interface, perhaps above all else. Make CI include end-to-end tests on hardware. For shaking down a new design, throw a board in the same thermal chamber and abuse it over a wide temperature range all culminated in an automatically generated report after it runs overnight. Automation also takes the drudgery out, and keeps the best engineers doing their most productive work.


## A plug for atopile


If you or someone you know develop in-house electronics, hit us up 👇


[Make me a world-class test setup](https://form.typeform.com/to/DLMYKqCu)


We’ll design, bring-up and send you world-class custom test equipment for an order of magnitude lower cost than off-the-shelf general equipment. They come with drivers, a framework and orchestration system to make hardware testing as easy as


` pytest` (because it’s based atop it 😊).


Sounds magic, but we can manage this because at atopile we make tools to design circuit boards blazing fast. By using code, rather than schematics, atopile has a far higher fidelity snapshot of your design requirements. Alongside our compiler, we’ve built a suite of circuit modules, matching drivers and a framework.


Check out our test equipment repo here:


[https://github.com/atopile/hil](https://github.com/atopile/hil)


Get started with the atopile compiler yourself here:


[https://docs.atopile.io/latest/](https://docs.atopile.io/latest/)


Thanks for reading atopile Blog! Subscribe for free to receive new posts and support my work.
