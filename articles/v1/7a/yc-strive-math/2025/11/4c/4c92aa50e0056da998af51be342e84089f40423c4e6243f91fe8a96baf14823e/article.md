---
schema_version: "1.0.0"
document_id: "4c92aa50e0056da998af51be342e84089f40423c4e6243f91fe8a96baf14823e"
company_key: "yc-strive-math"
company: "Strive Math"
source_id: "yc-strive-math-news-import-52f0d1e528c9"
canonical_url: "https://www.strivemath.com/blog/anna-accessible-speed-typing-game"
published_at: "2025-11-21T00:00:00+00:00"
first_seen_at: "2026-07-26T02:26:17.501056+00:00"
fetched_at: "2026-07-28T22:25:10.100738+00:00"
content_hash: "sha256:77d3a665f916955850dd2ce280277f77befc95fe47b77561a0be841968c605da"
---

# Anna Built a Speed Typing Game With Accessibility Features For Her Friend

One of the clearest signs that a student has genuinely learned something is when they use it for someone else. Anna is 11, and she's a student in Strive's[Coding](https://strivemath.com/) programme. She built a Speed Typing Challenge — a fully playable game — and the reason she built it the way she did is the part worth knowing about.


Her friend has dyslexia. And Anna decided, on her own, that her game should work for her.


## What Anna Built


The Speed Typing Challenge is a polished browser game. Players type words or phrases against the clock, competing for speed and accuracy. That alone takes real coding skill — tracking keystrokes, scoring in real time, managing game states between rounds.


But Anna went further. She added:


- **Open Dyslexic font** — a typeface specifically designed to be more readable for people with dyslexia, with weighted letter bases that help with visual orientation
- **Audio mode** — words are read aloud rather than displayed, so players can engage without relying on visual text processing
- **Speech input** — players can respond verbally rather than by typing, removing the keyboard barrier entirely


Each of these features required Anna to think carefully about how her game would feel for someone with different needs, and then work out how to implement that technically. She did this at age 11, without a template to copy, guided by her own clear intention.


## Why This Kind of Project Matters


In Strive's[Coding](https://strivemath.com/) course, students build games, visualisations, and interactive tools from the first few weeks. The technical foundations — Python, loops, functions, logic — are learned through projects students care about, not through drills or fill-in-the-blank exercises.


Anna's project is a good illustration of where that approach leads. She didn't build an accessible game because accessibility was a lesson objective. She built it because she thought about who would play it and what they would need. The technical choices followed from the human reasoning.


That's coding in the real world: starting from a problem, thinking through what someone actually needs, and working out how to deliver it. It's also what professional software developers do every day.


## What the Technical Implementation Involves


Adding Open Dyslexic requires loading an external typeface and wiring up a settings toggle that swaps the active font class on the page. Audio mode requires a text-to-speech implementation — either using the browser's built-in speech synthesis API or an external library — and logic to suppress the visual display when that mode is active. Speech input uses the browser's speech recognition API, which involves event listeners, parsing spoken input, and matching it against the expected answer.


None of these are trivial. Anna worked through each of them with her Strive teacher, asking questions and debugging as she went. The game works. Her friend can play it.


## What Comes Next


Students who complete Strive's Coding foundations progress to[Strive AI Coding](https://www.strivemath.com/courses/ai-coding) , where they build production-grade software and work alongside AI tools the way professional developers do. The move to AI Coding happens once a student has a solid enough grasp of how code works to direct AI effectively — to catch errors, understand what generated code is actually doing, and build things that function reliably.


Anna is on that path. And she's already building things worth showing.
