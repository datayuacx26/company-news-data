---
schema_version: "1.0.0"
document_id: "06b083ebcd5fb7e9a8167c431aa4c23e8f63aa8ebafb5fc97d75a8ced32293fd"
company_key: "bentley-systems-incorporated-class-b-common-stock"
company: "Bentley Systems Incorporated"
source_id: "bentley-systems-incorporated-class-b-common-stock-news-import-dc7c98e657e9"
canonical_url: "https://blog.bentley.com/insights/the-2000-year-old-computer-ai/"
published_at: "2026-07-21T18:56:43+00:00"
first_seen_at: "2026-07-24T19:29:36.034394+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:36757c3be41baebe71436e6fc6b0b57b85c6e148eff5fa277c09ba7832f748a5"
---

# An AI Rebuilt a 2,000-Year-Old Computer. The Real Prize Is Your Next Bridge.

Louis-Martin Losier watched as a transparent box full of delicate gears turned, and in turn recreated the motions of the cosmos. Mercury rushed around its orbit, Saturn barely crawled, and the Sun circled the Earth, like clockwork. He was running a digital simulation of the Antikythera mechanism, a miracle of ancient Greek engineering. Only a third of the original survives, and piecing together how it worked has taken scholars generations of detective work.


“It blew my mind,” says Losier, an archaeologist by training and now a senior product manager for Bentley Systems’ MicroStation, the software behind much of the world’s infrastructure. For once he wasn’t marveling at the mechanism itself, but at how easy it had been to recreate it. “Without even touching my design software, just using natural language, I was able to communicate my goal and watch this 2,000-year-old masterpiece reconstruct itself.”


With an AI agent connected to MicroStation – in this case, Anthropic’s Claude – Losier simply supplied the goal, the research, and the occasional correction. No need to manually navigate MicroStation’s branching menus, no writing code. The agent built the Antikythera model under Losier’s supervision. Even a year ago, a reconstruction this detailed might have taken weeks. Losier did it in under a day’s worth of actual work, snatched an hour at a time across a few weeks.


For an archaeologist, that ease represents the ability to test an idea the moment you have it: how a missing gear might sit, or whether a stray fragment belongs to a larger system. For a working engineer already adept at operating the software, the gain is time. Building a 3D model by hand of, say, a bridge takes hours, even when you know exactly what you want. If an agent takes on more of that load, the engineer’s effort goes into judgment, not grunt work.


Making that possible is Bentley’s new, experimental MCP (Model Context Protocol) server for MicroStation. On its own, an AI agent can reason and write but cannot reach into a program such as MicroStation and operate it. The MCP server connects the two, giving the agent a menu of things the software can do – draw a gear, place a part, set a dimension – and carrying out those actions inside the program at the agent’s direction. The agent supplies the intent; the MCP gives it hands.


MicroStation is a deeply capable industry solution, used to design bridges, buildings and many other structures, and to create digital twins of real infrastructure. That depth normally comes with a learning curve. An agent that takes plain-language instructions dramatically lowers the barrier, on problems ancient or modern. For now, the MCP server is available only to Bentley’s trusted testers. Bentley also built an MCP server for its STAAD structural analysis software – it’s[available on GitHub](https://github.com/BentleySystems/openstaad-mcp) – and more are on the way.


Losier also used the MicroStation MCP server to[recreate the Quebec Bridge](https://blog.bentley.com/insights/quebec-bridge-rises-ai-recreates-engineering-wonder/) , known among engineers as the 8th wonder of the world. His colleague Stuart Milne used the technology to[redesign the Gherkin](https://blog.bentley.com/insights/ai-recreated-the-gherkin-london-tower/) , an iconic skyscraper in the City of London.


## A Machine That Shouldn't Exist


The Antikythera mechanism was a wooden box no larger than a thick book, faced with bronze dials and packed with interlocking bronze gears, powered by a hand crank. Turn the handle and it modeled the heavens: the Sun and Moon moving through the zodiac, eclipses predicted years ahead, the calendar of the Greek games, and, in the most authoritative reconstruction, the motions of the five planets known to the ancient Greeks. The Moon pointer was thought to quicken and slow to match the real Moon’s uneven pace across the sky.


Left: Front view of the Antikythera mechanism. Right: An exploded model of the gearing, and the calendar dials on the back of the device (credit: Tony Freeth/UCL)


A fragment of the Antikythera mechanism.


## How to Recreate an engineering marvel


Losier wanted to know whether an AI agent could construct something this intricate with just simple instructions. The mechanism was an ideal test. It was also a personal passion: Reconstructing the past from scraps of evidence had been the work of his early career.


Before the agent could build the mechanism, it had to learn it. Losier gathered the research, chiefly[the authoritative and detailed papers of Tony Freeth of University College London](https://www.ucl.ac.uk/news/2021/mar/experts-recreate-mechanical-cosmos-worlds-first-computer) , and those of[Alexander Jones of New York University](https://archive.nyu.edu/handle/2451/48010) , along with images and diagrams. He then set Claude to digest it all. The model contains no new archaeology: Every part of it derives from that published research, the product of decades of painstaking scholarship.


From that reading, the model distilled the gear trains and tooth counts, the size of the case, and the Greek inscriptions into a detailed written brief, a plain-text markdown document setting out what to build, and how.


At Losier’s instruction, the agent kept careful track of what the surviving fragments actually show and what scholars have had to infer. Everything that followed would rest on that document, so Losier made it with the most capable model he had. “When you’re building your plan, your context, it’s important to use the best models available, so I created that ‘blueprint’ with Claude Opus 4.8,” he says.


For the build itself, he handed the plan to a simpler, faster model, Claude Sonnet. His prompt began: “You are an engineer supporting the progressive reconstruction of the Antikythera mechanism …” From there, the agent wrote its own Python scripts and ran them inside MicroStation’s built-in editor, work Losier would once have done by hand. He told it to build in small stages rather than generate the whole model at once, and he checked each stage himself before it went on.


The point was to stop errors compounding. An agent that does everything in one pass can make an early mistake that can corrupt all that follows. Taken step by step, each error shows up while it is still small and easy to fix. The mechanism came together part by part: gears aligned first, then the case, the faces, the hand-crank, and finally the dials and inscriptions, each step checked before the next was added.


The model in motion, showing the planets move across the Greek skies.


The last step brought it to life. At Losier’s direction, the agent wrote an animation script in Python that set the whole gear train turning, along with a control panel to vary the speed – a 21st century hand crank. Each heavenly body moved at its own rate. The Sun, Moon, and planets moving across ancient Greek skies once more.


## Getting a handle on hallucination


Losier is the first to call his project an “interpretive reconstruction” of the mechanism, built on incomplete evidence. It makes no claim to settle how the mechanism worked, and where the evidence runs out, the agent’s choices are informed guesses that specialists may well dispute. Which ties in to a more fundamental worry. Generative AI systems are prone to hallucinate, inventing details that were never there. Losier’s process was built to give AI as little room as possible to make stuff up.


The markdown plan was the first guardrail. Rather than let the model attempt a complex reconstruction in one shot, the planning document provides the steps that an agent has to check its progress against.


Losier also wouldn’t let the agent run unattended, and made sure each step was completed satisfactorily before the next could begin. That paid off when the model forgot the small matter of the hand crank, Losier says: “I asked Claude, ‘Did you miss a step, because the crown gear and the hand crank are missing,’ and it replied, ‘Oh yeah, good observation. I will add it.'”


The crank was a harmless thing to miss. On a real bridge or high-speed train line, the same slip would not be, and the safety process has to be airtight. “You want to make sure your infrastructure doesn’t fall down,” says Losier. In the real world, the


[engineer must stay firmly in the loop](https://blog.bentley.com/insights/would-you-cross-that-bridge-ai-and-the-trust-problem-in-infrastructure/) , and separate tools, such as Bentley’s STAAD for structural analysis, would check the drafting agent’s work against load limits and safety tolerances. Losier’s Antikythera bears no loads and holds nothing up, so that check doesn’t apply. His recreation is visually persuasive, but persuasive is not validated. No complete original survives, and judging the model’s fidelity remains work for the scholars who know the fragments best.


## Life begins at 40


Shortly after Losier finished his Antikythera project late this spring, new papers appeared proposing fresh interpretations of parts of the mechanism. Once upon a time, changing one part of a complex, interlocking design would have meant reworking everything downstream by hand. Giving agents access to MicroStation solves the problem. Rather than start over, Losier simply plans to give the paper to Claude with a simple instruction: “Alter this design so that it matches what has been found.”


On infrastructure projects, requirements never stop evolving. A client asks for another design option. A clearance changes. A new constraint appears halfway through a project. Pushing those changes though a complex design can consume days of engineering effort. With an agent at their side, engineers can describe a change in plain language, then quickly review and validate the result.


This is also what excites Bentley. Make a sophisticated tool such as MicroStation this easy to use, and more people can build with it. Losier’s reconstruction was a passion project with a purpose. But the real prize is for the working engineer who, even without years of training in the software or knowing how to code, will be able to


[turn an idea into a design and test five versions of it in an afternoon](https://blog.bentley.com/insights/from-code-to-command-how-ai-is-rewiring-the-way-engineers-design-infrastructure/) . And when an experiment costs a few hours instead of weeks, engineers can pursue the ideas they would once have abandoned.


[MicroStation is now 40 years old](https://blog.bentley.com/insights/built-to-last-how-microstation-digitized-infrastructure-and-fueled-bentleys-40-year-rise/) . For four decades, it has helped engineers design and document infrastructure around the world. For most of that time, engineers have had to learn how to speak its language through commands, menus, and workflows. Projects like Losier’s hint at a different future: one where engineers simply describe what they want to achieve, and AI helps translate that intent into a design. Where Losier wanted to go was 2,000 years ago, to the Antikythera mechanism. “Whether the complex problems come from ancient history or modern engineering, we can use AI to help solve them,” he says. “That’s what matters.”


## FAQ:


**


**


How was AI used to model the 2,000-year-old Antikythera mechanism?


An AI agent connected to Bentley Systems’ MicroStation software was directed using natural language to recreate the ancient astronomical device. By interpreting plain-English instructions and research materials, the AI built the complex 3D gear model and simulation in less than a day of total work without requiring manual software navigation or coding.


**


**


How can engineers control MicroStation using plain language?


Engineers can connect an AI assistant directly to MicroStation software. The AI translates typed design intent into software operations, allowing users to generate and refine complex 3D models simply by describing what they want to build rather than navigating complex menu options or writing scripts.


**


**


Why is human oversight still necessary when AI generates engineering models?


While AI agents quickly automate drafting tasks and complex geometric builds, human oversight ensures accuracy and engineering discipline. Qualified engineers are still needed to provide context, set clear guardrails, review generated work, and catch errors to ensure the final design meets real-world requirements.
