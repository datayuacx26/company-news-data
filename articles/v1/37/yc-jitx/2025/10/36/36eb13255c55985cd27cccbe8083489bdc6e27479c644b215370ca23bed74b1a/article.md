---
schema_version: "1.0.0"
document_id: "36eb13255c55985cd27cccbe8083489bdc6e27479c644b215370ca23bed74b1a"
company_key: "yc-jitx"
company: "JITX"
source_id: "yc-jitx-news-import-ca0082c4ce50"
canonical_url: "https://blog.jitx.com/jitx-corporate-blog/whats-new-in-jitx-2025"
published_at: "2025-10-01T02:22:33+00:00"
first_seen_at: "2026-07-22T00:57:30.944798+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:26b6f16e577b0796dd5eb58eac0795428689f628fac01198af85eff878f78ecc"
---

# What's new in JITX 2025?

We started JITX with the idea that writing code is a better way to design electronics (than managing a complex database page by page with a vector graphics editing tool). This was a pretty obvious move (if a difficult one), because chips made this jump from manual schematics to code back in the 1980s and it would be death to go backwards.


What we didn't see coming was that AI was going to show up and write all the code. Here's AI writing code for a simple industrial I/O board -- including high-speed design constraints like impedance control, voltage isolation, as well as pin optimization on the controller.


The striking thing is that this demo doesn't use some canned, locked-down library. AI parsed component information from datasheets to build up the circuits and components it used in this design! This was complex stuff too, like the pin mux table of a microcontroller. The experience of using JITX with AI is being continuously surprised by what is now possible.


## Where chatbots fail EE, code succeeds


One thing we get out of the current race between the frontier AI labs (Google, OpenAI, Anthropic, xAI) is a really good autonomous software engineer. We now have a field of options, including Claude Code, Codex from OpenAI, and open-source coding agents like Cline. Today, software engineers are moving from controlling one AI to controlling a host of autonomous AI agents to further boost speed and productivity. This graph from METR, shows that the complexity of a software task that an AI can do is doubling every ~4 months.


METR: Measuring AI Ability to Complete Long Tasks \[2025\]


When interacting with AI as a chatbot, or having it click out things on your screen, the AI hallucinates too much for it to be useful for expert-level electrical engineering. But! If we can have AI write code -- good modern code (not SPICE, not Verilog), then we can get automation that is actually helpful for expert hardware engineers. AIs that can write code allowed the automation in the above video, and enabled a few new pathways like the simulation loop in a following section.


LLMs writing code is a powerful and weird automation loop, but once it's code it becomes, stable, precise, and inspectable. It's ready to be used for hardware design.


## Constraint-driven autorouting


One reason that we've been a bit behind on publishing is that we running head‑to‑head benchmarks with customer data, to show customers how much more productive JITX is (5-20x multiple on engineer hours, measured on complex 30+ layers designs, memory interfaces, RF boards, LRU backplanes). Obviously and unfortunately we can't share those designs, but we did learn a lot about what it takes to make hardware design go faster.


First, you have to enforce 100% quality -- zero known DRC/EMI/SI violations at tapeout. The circuits have to be right, and the board layouts must follow 100% of the rules. As one mystery customer said "If we fly this board, it's gotta work." Quickly designing a board with even one flaw in it is a waste of time.


Second, you have to deal with requirements changing all the time. Thermal simulation says you have to move something, the mechanical engineer wants to put a mounting hole in the memory interface, system integration swaps a MIL‑DTL‑38999 connector from male to female. Up to half of a design schedule gets eaten by requirements churn. To win that back you have to adapt on the fly.


This churn slows down hardware design flows because constraints and automation are underdeveloped in traditional EDA. Usually adding constraints is highly manual, and takes designer time. Then the reward mostly comes in the form of a correctness check: a designer draws out some copper, manually following rules, runs the checks, and then iterates to get a design that passes the check.


Often that tradeoff of effort isn't worth it, so what usually happens is that there's a manual layout review, and manual training, and manual work to draw traces that implement the complex and detailed tradecraft of PCB layout work. You can get to a quality result this way, but it does mean that every time there's a change, your beautiful manual work gets blown up, and you have to start over, redraw, recheck, re-review.


To fix that we did three things:
1. Developed a code-based constraint system both for signal integrity (impedance control, structures, skew, loss), as well as any rule you'd need to express on a board (e.g. this trace to a pour of this net, on this layer).
2. Automated adding those code-based design constraints
3. Developed a router that is constraint-driven: given a set of rules, directly make a layout that meets the rules (rather than just report a check failed)


With that we were able to accelerate adding constraints to a design so much that we were able to add ~50% more constraints than the original design. It ended up being constrained more like a package design (where there are real rules, instead of rules of thumb). So more correctness was enforced automatically, and designs can adapt on the fly to huge perturbations in requirements.


(there's a lot to show here -- we'll be making some more comprehensive demos with our own designs)


## Optimization loop with SI/ANSYS


Along with writing code for designs, we found that LLM-based AI could be used to close the loop on higher level optimizations. We presented some of our early work at[Ansys Simulation World](https://www.ansys.com/events/sw-california-2025?utm_campaign=events&utm_medium=paid-search&utm_source=bing&utm_content=events_sim-world-2025_event-virtual_register_brand-search_NULL_a_en_amer%7C1169882302583710%7C73117989839270%7C&campaignid=701Pf00000QaI5uIAF&utm_term=simworld+2025) to kick off the Signal and Power Integrity track. AIs can write JITX code, which creates a design for an FPGA and memory combo, and set up the SI constraints for the autorouter. We took that design into Ansys for simulation of the memory strobe, and one of the AI models we tested was able to correctly identify that the termination had been left off. The real power here is that AI can close the loop through requirements, design, simulation, and design update by using code.


This was very early work. Next we're going to be making some highly integrated automation for SI engineers doing structure optimization (launches, via structures, ballouts). If you spend a long time setting up HFSS simulations,[please reach out to get early access](https://www.jitx.com/contact) .


## RF Autorouting


We had user feedback that RF layouts, and RF design closure were getting extremely slow and expensive. The via-fence tools in CAD tools don't always work well for curved RF traces, and will drop vias from the fence in corners. Too slow, and too many bad boards from missing vias causing a lack of expected isolation. So we added interactive fence generation to the autorouter, along with support for arbitrary pad stacks, multi-layer structures (like skip-layer CBCPW), and complex via arrangements to our physical design tool.


All of that RF routing automation, combined with the ability to swap pins in the router makes for a very powerful design tool for complex RF systems like phased arrays.


## Package routing density


Our autorouter is topological (operates on continuous geometry rather than grid‑aligned Manhattan traces), and makes route shapes that reminds old hands of the days of manually taping out boards. It looks very different than the ortholinear style of 45° and 90° bends, which raises questions sometimes.


Frankly put, we built a topological autorouter because it has the highest performance on density, signal integrity, and RF. People use JITX to do better on their hardest designs, and we haven't had cause to make the autorouter look 'normal' because that would strictly reduce its performance.


This came up with a package routing problem between two chiplet Advanced Interface Bus interfaces. The traditional package routing tool previously used by the customer could fit six traces between these vias. Seven traces could fit with a whole lot of work (because the obstacles aren't exactly at 45 degrees, and if you use curves in that tool, your database balloons in size so quickly that it becomes unusable).


But the JITX topological autorouter could easily flow 8 traces between these obstacles. That's a benefit of 33% extra density when controlling for layout hours (and engineer morale).


## JITX is in Python now


As part of AI writing all the code, and in response to a whole lot of customer feedback, we've moved our front-end language from a domain-specific language implemented in Stanza to the much more common Python. We went with a pure-Python strategy for the language implementation, because:


1. We wanted to take advantage of all the Python tooling, libraries, and ecosystem.
2. Modern AIs are really good at writing Python


And my goodness it works really well. Any advantage of a custom DSL is completely blown out of the water by the support of modern toolchains, and having AI write all your code and fix all your bugs for you.


We're going to be launching the Python version of JITX in Q4 2025,[sign up now](https://www.jitx.com/contact) for early access .
