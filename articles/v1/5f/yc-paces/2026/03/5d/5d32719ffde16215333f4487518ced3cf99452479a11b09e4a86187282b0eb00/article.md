---
schema_version: "1.0.0"
document_id: "5d32719ffde16215333f4487518ced3cf99452479a11b09e4a86187282b0eb00"
company_key: "yc-paces"
company: "Paces"
source_id: "yc-paces-news-import-cb0c795e26d8"
canonical_url: "https://www.paces.com/post/from-weeks-to-days-what-vertical-ai-looks-like-inside-energy-diligence"
published_at: "2026-03-24T00:00:00+00:00"
first_seen_at: "2026-07-25T18:30:06.754267+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:1a829e15333dcdbfa1bb6091505ed6affa2778e6cd02f260ef4cadbfb6362887"
---

# From weeks to days: What vertical AI looks like inside energy diligence

The core problem we're trying to solve at Paces is fairly straightforward: reduce the failure rate of energy infrastructure projects. The majority of projects in a developer’s early-stage pipeline ultimately fail to come to fruition, meaning developers, consultants, regulatory authorities, utilities, and others involved in the development process spend most of their time and capital on projects that will never be built.


This fundamental inefficiency keeps our entire company up at night. It's also what makes energy development a perfect application for vertical AI: systems built deep into a specific industry rather than broad, general-purpose tools. The work of project diligence is highly structured yet requires genuine expertise to execute well. It demands synthesis across heterogeneous data sources: geospatial layers, regulatory documents, utility records, and local ordinances. And the stakes of getting it wrong (months of wasted effort, millions in sunk capital) are high enough that pure automation without human oversight isn't acceptable. This is exactly the kind of problem where vertical AI shines: domain-specific workflows, specialized data pipelines, and humans in the loop where judgment actually matters.


The first is capitalizing on AI breakthroughs emerging from frontier labs. Large language models (LLMs) and multi-agent workflows have gotten genuinely more useful for the types of tasks that used to require armies of researchers and months of manual work. Software engineers are undeniably becoming more efficient at writing code, and these advances are still in the very early innings of diffusing throughout the rest of the white-collar economy.


Second, we're pairing that with an aggressive data democratization strategy. We're collecting all publicly available data—messy, unstructured, and scattered across government agencies and utilities—and consolidating it on a unified platform. We're also leveraging data vendors to ingest higher-quality, commoditized datasets where necessary and working with the utilities and regulatory agencies themselves to peek behind some of those walled gardens. Classic extract, transform, load (ETL) allows us to structure everything into a coherent, queryable form.


But the real differentiator is the team. We've assembled best-in-class AI engineers from places like Meta, Nvidia, and Amazon who are genuinely obsessed with cutting-edge agentic capabilities, and paired them with industry domain experts like power engineers, permitting specialists, and environmental scientists who are frustrated with the status quo, whom we call “solution engineers.”


In practice, our product teams aren’t split between these two types of engineers. We just have “engineers,” and their talents fall along a spectrum from deep technical competency in software and AI to deep knowledge of energy development. Our AI engineers learn how to conduct permitting diligence or a power flow study, and our solutions engineers learn how to vibe code and iterate upon scalable systems. What this has really done is formally inject domain knowledge into the classic “tech stack” that matured during the 2010s; not only do we have specialization in frontend, backend, data, and dev ops amongst engineering teams, but we now have workflow expertise as well.


## **How we build: thinking in risk swim lanes**


We organize our workflow expertise around what we call "risk swim lanes": the high-level categories of risks most likely to kill your project. Each swim lane, such as interconnection, permitting, environmental review, site control, etc., has its own set of milestones and diligence steps that must be completed to reach Notice to Proceed (NTP). The implications for our product teams are that each swimlane has a unique data ecosystem, optimal workflow, and specialized knowledge required to identify risks.


The early version of Paces was predominantly a siting search engine: we ingested large geospatial and textual datasets, structured them, and pinpointed the best real estate options based on project criteria and specific development strategies. That's still foundational to what we do. But the story of where we're going is really about the next step in this process: desktop diligence. This is where the problem demands that we go beyond land identification and start inserting deep human expertise into project diligence, which requires thorough risk identification and analysis, development strategy recommendations,[report presentation](https://www.paces.com/products/reports) , and application submissions.


The thesis is straightforward: reducing the marginal cost of project diligence enables us to initiate it earlier in the development process and to parallelize diligence across risk swimlanes, rather than moving sequentially through different specialist teams or consultants. This means developers using Paces can identify the biggest project risks earlier, enabling them to eliminate the projects most likely to fail and shape development strategy around the unique characteristics of remaining projects to target the largest areas of uncertainty before sinking real time and capital.


## **The division of labor between human and machine**


So how do we automate project diligence?


There are three specialized roles to be played in our automated diligence workflows:


**Deterministic software and classic ETL** : Handle geospatial data on the built environment, environmental layers, government boundaries, grid infrastructure locations, and more. All of this needs to run on a deterministic platform where we can calculate distances and buildable areas in a highly performant way. No AI magic here, just solid data and backend engineering.


**Large language models and multi-agent systems:** This is what I call "light reasoning at scale." Taking open-ended yet repetitive instructions and turning them into structured, text-rich documents, and making them available in a much more human-digestible format. AI models are excellent at turning messy ordinances and regulatory documents into actionable insights.


**Human specialists:** They're involved at specific stages, overseeing the work that requires actual domain knowledge accumulated through experience. Real human-level reasoning, not pattern matching.


Our goal as we build products is to liberate our domain specialists from drudgery and keep them focused on the most cerebral aspects of the job. Given the volume of sites they diligence every day, we want them to tweak the language of executive summaries, test the rationale of the agents when reviewing their work, and perform other intellectually stimulating tasks rather than data collection, formatting, etc.


We are a team of AI optimists and[envision a positive future between human and machine](https://www.paces.com/post/the-one-person-billion-dollar-power-development-company) , and the ideal we’re striving towards internally reflects this philosophy at large.


## **The local zoning and permitting workflow**


One of the most difficult aspects of site assessment is analyzing local zoning codes and ordinances. Every project must ultimately complete a permitting process with an Authority Having Jurisdiction (AHJ), which could be one or more counties or municipalities, that determines whether a proposed project can be built and under what conditions. This requires hours and hours of tedious online research to find, analyze, interpret, and summarize large PDF documents and geospatial data.


We’ve dramatically cut the active time required for a human researcher to do this work by building what we believe is a new paradigm for vertical AI: robust data pipelines interspersed with Human-Agentic Loops (HALs, working title). HALs, which we are building and have built for many similar workflows across our platform, take an initial research question and produce verified conclusions by implementing the division of labor articulated in the previous section. Here’s how it works:


**Step 1: Site identification —** When we get a site request from a client, it goes onto our platform, and we immediately identify the county, nearby municipalities, and township boundaries.


**Step 2: Collection agents go to work —** A data collection multi-agent system scrapes zoning ordinances, local laws, data center-specific or renewables-specific ordinances, subdivision laws, annexation rules, and all official documents, plus zoning maps and GIS maps for the relevant AHJs.


**Step 3: Human review —** A specialist reviews the initial report and the data. They're essentially conversing with the agent: "Did you look for this? Why couldn't you find this?" The agent explains what was publicly available and what wasn't. Then the specialist takes a pre-written email from the agent to send to the local authority, confirming our information is correct and everything's up to date.


**Step 4: Analysis agents synthesize —** After reviewing the data inputs, the specialist then kicks everything to an analysis and report-writing multi-agent system, which queries and structures all those documents, reasons through the text, and produces a draft report.


**Step 5: Human sign-off —** The specialist converses with the multi-agent system to tweak the narrative, retest reasoning, and ensure we're reaching appropriate conclusions. Then one more button press, and that section integrates with the rest of the report in the proper format.


## **The multi-agent architecture**


To go even deeper into how the multi-agent system works, our analysis system has four primary steps:


**Data preparation and cleaning:** We use optical character recognition to make sure all PDFs are interpretable by subsequent models. Everything cleaned, ready to go, fully ingestible.


**Indexing and extraction:** A team of agents reviews and asks questions based on what our permitting specialists have defined in the Standard Operating Procedures. They create a semantic map of every section of all the documents (i.e., "I can use this to answer this type of question.")


**Research and collection:** We loop through all the questions that need to be answered for a given[report](https://www.paces.com/post/the-future-of-due-diligence-ai-accelerated-modular-reports) . The system pulls from that map to synthesize information and put it in the right context window.


**Synthesis:** Another team of agents looks over every chunk, makes sure the narrative is coherent, and goes through different rounds of self-review to tweak language and ensure logical consistency.


## **On model selection**


Something we think about a lot is which models perform best for which tasks. We're keeping up with all the most popular benchmarks for each model, and our systems are highly configurable so we can swap out models as the landscape evolves.


However, what we've realized from working closely with these models is that raw benchmarks are not as important as understanding how they present different trade-offs depending on how they're trained. I can't get into specifics, but for example, our AI team has found that certain models are excellent for background research tasks, where you can give them complex instructions and they work autonomously on long time horizons whereas others are much better for that interactive synthesis step when the model is conversing with itself to refine language and then explaining its reasoning to our human specialists.


## **Where this is going**


The key insight here is that as we iterate, we bring in additional expertise to refine the interpretation. The interpretations of our HALs have been trained on our human specialists, who have reviewed hundreds of these local jurisdictions, as well as third-party consultants for independent verification and review of methodology.


But the real power is in the feedback loop. Every report we produce makes the system smarter. Every edge case our specialists catch trains better judgment. The marginal cost keeps dropping, which means we can bring this analysis earlier and earlier into the development process.


This is ultimately why vertical AI works for energy development, while general-purpose AI tools fall short. A horizontal copilot can summarize a document, but it doesn't know which documents matter, what questions to ask, or how to structure findings for a development team making real capital allocation decisions. Building vertically means our data pipelines, agent architectures, and human checkpoints are all designed around how this industry actually operates. The domain expertise is embedded in the system from the ground up rather than being bolted on. And as the frontier models improve, we're positioned to capture those gains immediately, because we've already solved the harder problem: understanding the work itself. If you have any questions or thoughts about this, reach out to me on[LinkedIn](https://www.linkedin.com/in/kyle-baranko-8623baa7/) . If you're ready to put this in action,book time with our team .


‍
