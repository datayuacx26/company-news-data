---
schema_version: "1.0.0"
document_id: "afbebf21ddb3b8abcea2287135d6b8766a04635780c4383b1f16d2309d82a8d1"
company_key: "bentley-systems-incorporated-class-b-common-stock"
company: "Bentley Systems Incorporated"
source_id: "bentley-systems-incorporated-class-b-common-stock-news-import-dc7c98e657e9"
canonical_url: "https://blog.bentley.com/insights/ai-recreated-the-gherkin-london-tower/"
published_at: "2026-06-23T15:04:09+00:00"
first_seen_at: "2026-07-30T03:31:34.455664+00:00"
fetched_at: "2026-07-30T03:31:35.599702+00:00"
content_hash: "sha256:ddb382ef96e2f7ccd64070a294241c22143db25c3ec3fd492052c203dcb1b04a"
---

# Inside the AI Experiment That Recreated the Gherkin, One of London’s Most Iconic Towers

Before the London office tower had a name, it had a shape. Rising 41 stories, wrapped in a steel lattice, and clad in shaded glass that tapered to a point at the top, it looked like a futuristic Easter egg. But when the tower’s elongated curves caught the sun and shimmered in pale teal, Londoners saw something different: a gherkin. The affectionate name stuck.


The twisting form of the tower, officially named 30 St Mary Axe and designed by Foster + Partners, turned heads well before it was finished. It kept Londoners guessing how it would look and how it could possibly be built.


The design firm’s founder, Norman Foster, had long argued that architects must advocate for new ideas. He believed the building was genuinely radical—and when it opened in 2004, much of the architectural world agreed. It was one of the most innovative and energy-efficient towers in the world, and it looked like nothing else on London’s skyline.


One of the team members working on the Gherkin was Stuart Milne, now a product manager at Bentley Systems, the global infrastructure engineering software company. Taking Foster’s maxim to heart two decades later, Milne has also done something radical: He used an AI agent to recreate the Gherkin in just minutes, then proved the design would hold up. His experiment paired a probabilistic AI, the kind that imagines and guesses, with deterministic


AI techniques, which turn probability into reality.


The AI connected to Bentley software provided the creativity. Milne checked the design and supplied the proof.


Bentley's Stuart Milne has also done something radical: He used an AI agent to recreate the Gherkin in just minutes, then proved the design would hold up.


## A Building made of code


The Gherkin began the way many buildings did before the advent of computer modeling: architects worked out the early form with their hands in paper, colored pencil sketches, foam and cardboard models. But then the computers took over. “What few people outside the profession understood was how much math went into the tower’s curves,” says Milne, who handled the computer-assisted design.


Designing


the twisting glass skin to fit the building’s double helix-like skeleton and repeating, offsetting floor plates was a challenge, and the final solution relied on a repeating system that let computation do the heavy lifting.


Doing that math by hand would have taken years, Milne says. Instead, the team—which included Milne, Francis Aish, whose father Robert was the director of research at Bentley, and the late Hugh Whitehead—used[Bentley’s MicroStation software](https://blog.bentley.com/insights/built-to-last-how-microstation-digitized-infrastructure-and-fueled-bentleys-40-year-rise/) to build a parametric model of the building. This means that they encoded the design as a set of dependent variables, rather than as static geometry. Next, they generated an intricate script that defined each panel, one after another, until they had the entire surface.


The approach let them turn architectural sketches and models into something a contractor could actually construct. From their point of view, the Gherkin was a story about creativity meeting computation long before anyone said the words “generative AI.”


## Same Engineer, New Tool


After he moved to Australia, Milne never lost sight of the Gherkin, and his name appears in a monograph about the building titled


*30 St Mary Axe: A Tower For London* . But when he returned to its design this June, he did not start with a pencil or even a desktop workstation. He started with an AI agent in a data center.


Speaking from Sydney, Milne said he wanted to see “whether a modern AI system could take on a problem that once required a team of computation specialists.” He wanted to know if AI could handle the computational process and find the points where a human needs to step in.


The answer turned out to be a useful lesson in what AI in engineering and architecture can and cannot do.


## Teaching the Agent the Building


First, Milne had to make sure the agent understood what it was doing. Searching online, he gathered background on the Gherkin and loaded it into NotebookLM, a Google tool that lets AI study a set of public documents, such as websites and YouTube videos, and answer questions about them. That gave the agent a working knowledge of the building’s geometry, its history, and the logic behind its shape.


“Many people had tried to recreate the building in the past,” Milne said, including with tools like[MicroStation](https://blog.bentley.com/insights/built-to-last-how-microstation-digitized-infrastructure-and-fueled-bentleys-40-year-rise/) , Generative Components, Rhino, Revit, and Dynamo. “There was plenty of information I could find, but it


required a lot of research and constant back-and-forth referrals to the data. AI pulls this messy information, structures the data, then organizes in such a way that new AI capabilities in MicroStation can rebuild it.”


Only then did the design work begin.


## Plugging the AI into Real Software


This is where the project got exciting for Milne. He used a Claude AI agent, which is an autonomous AI system, to analyze the information. He connected it to MicroStation through a tool called an MCP (


Model Context Protocol)


server.


In plain terms, that MCP connection is the plumbing of the AI revolution we live in.


MCP servers let the AI agents reach directly into professional design software and other sources, almost any software, really, and command it in plain English. They are built around an open standard that connects probabilistic large language models (LLMs) with engineering software. Engineers and architects describe what they want in plain English, the AI carries it out inside the application, and[the human stays in the loop to review and validate every outcome](https://blog.bentley.com/insights/would-you-cross-that-bridge-ai-and-the-trust-problem-in-infrastructure/) . “We did it by


using deterministic techniques we deploy in the process, like refining the code, to get what we need,” Milne said.


Bentley engineers have built MCP servers for MicroStation, the company’s[STAAD](https://blog.bentley.com/insights/from-code-to-command-how-ai-is-rewiring-the-way-engineers-design-infrastructure/) structural analysis software, PLS Power Line Systems, and other solutions. Many more are on the way.


With the MCP link in place, the AI agent generated the tower’s form inside the same tested and reliable engineering tool used on the real Gherkin more than two decades earlier. The AI was creating a design that could be verified by engineering software and trusted by architects and engineers.


## From a good idea to a real answer


The agent’s first output was a proposal shaped by everything it learned from the material Milne loaded into NotebookLM. But AI systems like Claude work by predicting what should come next, which makes them good at imagining a form but does not always make them right. Engineers, architects, and the rest of us who rely on their work,[require safety and unrelenting reliability.](https://blog.bentley.com/insights/would-you-cross-that-bridge-ai-and-the-trust-problem-in-infrastructure/)


To verify the AI design, Milne used two features already available to any MicroStation user around the world: Bentley Copilot and a Python Assistant. The Copilot allowed him to turn the AI design into a 3D model of the Gherkin. Next, he used the Python Assistant to create reusable scripts that defined and checked the model’s geometry. He then repeatedly ran those scripts to ensure the results held up and behaved the way the engineering demanded.


Those two steps dramatically changed the nature of the experiment. The model stopped being an AI-generated shape and became a physics-based, deterministic design. Deterministic means the same inputs always produce the same result, every time, with no guesswork allowed. “The scripts could be rerun and verified,” Milne said. “A human engineer stayed in the loop the whole way, deciding when an answer was good to keep. The division of labor was clear: The AI generated the design, the engineering software tested it, and the human verified it.”


## Why a redesigned skyscraper matters


The Gherkin already exists, and this project was not about replacing it. Rather, it was about testing a new approach. What Milne demonstrated is a way to use AI on work where a wrong answer has real consequences. It also showed that AI agents could help architects power through their workload and handle the time-consuming parts of a project while humans use their judgment and check the work.


“Nobody wants to sign their name on a design if they’ve not been able to verify what is generated is not going to put people in danger,”


Julien Moutte, Bentley’s chief technology officer,


[recently told AI Magazine](https://aimagazine.com/interviews/the-ai-interview-julien-moutte-bentley-systems) .


“I really see the role of civil engineers becoming an orchestrator, where you create a team of AI agents doing specific parts of the workflow and you remain the one taking the important decisions.”


The experiment also contributes to the debate about the evolution of engineering and architecture.


“While this experiment took the outcome of years of design expertise, and recreated it in minutes, how do architects and engineers capture their own expertise and apply that to design in the new AI future,” Milne asks. “Historical design expertise is recorded in drawings, documents, sketches, physical models, 3D models. It lives in the experience of great designers. How can they leverage AI to increase their expertise?”


There is also a neat symmetry to all of this. The Gherkin design was completed with scripts and parametric math, written by people who believed a building could be designed with code. The young mathematician behind its facade, Francis Aish, learned the craft in part from his father, who was building the very tools that made it possible.


More than 20 years later, Milne, one of the engineers from that original team, picked the building up again and handed it to an AI. The AI agent is just the latest chapter in a story the Gherkin has been telling all along.


## FAQ:


**


**


How did Stuart Milne use AI to recreate the Gherkin design decades later?


Milne first gathered historical and geometric data on the Gherkin and fed it into Google’s NotebookLM so the AI could learn the building’s logic. He then used a Claude AI agent connected to Bentley’s MicroStation software via a[Model Context Protocol (MCP) server](https://blog.bentley.com/insights/would-you-cross-that-bridge-ai-and-the-trust-problem-in-infrastructure/) , enabling the AI to generate the tower’s form in minutes using plain-English commands.


**


**


Is an AI-generated building design actually safe and reliable?


A purely probabilistic AI design is based on guesswork and prediction, which isn’t enough for structural safety. To ensure reliability, Milne used deterministic tools—such as a Python Assistant—to turn the AI’s proposal into a physics-based 3D model, running repeatable scripts to rigorously verify the geometry, with a human engineer in the loop at all times.


**


**


What does this experiment suggest about the future role of civil engineers?


As Julien Moutte, Bentley’s CTO, explained, the role of civil engineers is evolving into that of an “orchestrator.” Instead of doing all the manual heavy lifting, engineers will manage teams of AI agents to handle time-consuming tasks, while humans remain responsible for making critical decisions, verifying the work, and ensuring public safety.
