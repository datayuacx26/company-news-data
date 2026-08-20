---
schema_version: "1.0.0"
document_id: "3986f4b5d9da22becb6eb43a9c6f7586ba6ea3e61cad505def232e17e0ea52fa"
company_key: "yc-jitx"
company: "JITX"
source_id: "yc-jitx-news-import-aad5bf5f2004"
canonical_url: "https://www.jitx.com/blog/why-we-started-jitx"
published_at: null
first_seen_at: "2026-07-24T05:01:33.070480+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:b376e58f2a680474d9a148b8826139990ec5f49c878540f7c7d5f1e095de1afc"
---

# Why we started JITX

## Automating the Last Bottleneck


Break open any electronic device and inspect its circuit board. Look at all the chips and wires sprawled across both sides of it. An electrical engineer manually analyzed thousands of parts to choose the chips on that board, manually placed each one, and manually drew wires to connect them. If two pins are connected when they shouldn't have been, the board doesn't work. If the board is too big, too wide, too hot, or too expensive, then almost every component has to be reconsidered, relocated, and each wire redrawn. These mistakes are far from uncommon: the board in your device has probably been revised several times, with each revision delaying production even more. The industry pace is so demanding that engineering firms distribute teams working in shifts 24/7, across the globe, just to make sure that all of the wires can be drawn on time. JITX has engineered a way to automate this last remaining bottleneck in the electronics industry: the circuit board design.


Circuit board design is a true multidisciplinary challenge, and the solution lies at the intersection of electrical engineering, mechanical engineering, manufacturing, physical simulation, artificial intelligence, optimization, compiler, and programming language design. These hard problems are our team’s home-turf - we came together at UC Berkeley to solve this exact problem.


## Our Approach


Our core technology is inspired by and modeled after the technology used for designing computer chips. Chips used to be designed in the same way that circuit boards are designed today: by engineers drawing shapes on a screen. As transistors shrank and chips became more complex, the task of drawing those shapes correctly became too complicated for human beings to handle. The introduction of hardware description languages (HDLs) in the 80s, such as Verilog and VHDL, revolutionized chip design.


JITX brings the same paradigm shift to circuit board design that Verilog and VHDL brought to chip design. Instead of manually drawing wires on a screen, engineers instead specify the intent of their circuit board using code that our algorithms translate into component choices, placements, and wiring.
We are able to:


- Design more sophisticated boards in a fraction of the time,
- Easily prototype and evaluate many variations of a design,
- Build a library of optimized and modular subcircuits, which are reused across designs, and
- Computationally optimize designs for space, cost, performance, and yield.


Chip designers have enjoyed three decades of these benefits. Our revolution in circuit board design has been long overdue.


## Better Circuit Boards, for Everyone


We started JITX to give everyone access to professional-quality boards, and today JITX runs as an electronics design contractor. You tell us what a circuit board needs to do - we use our tools to design the board, fab it, and get it back to you. We already do this faster than humans can, and our speed depends on the design. For product and proof-of-concept boards, JITX is on average three times faster and 25% cheaper than other contractors. Test fixtures and connector- based boards are almost fully automated, and we have already hit 24 hour turnaround times on designs.


If you need a custom circuit board designed -[start here.](https://www.jitx.com/#GettingStarted)


‍
