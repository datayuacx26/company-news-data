---
schema_version: "1.0.0"
document_id: "4a0e32ae6bf7300c2670094095a33104790c34a7e772b14cd66fc8c87ab2f8ae"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/ai-scientist-that-deserves-the-name"
published_at: "2026-04-23T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:d42d1aad74a41e64faa178db636f13d07b75ff40117a1c8e7dbbdc6039fa88e2"
---

# An AI Scientist that deserves the name

*I've spent more than a decade immersed in the biopharma industry, building tools for the scientists who drive it forward. Benchling now powers scientific progress for over 1,300 companies investing tens of billions in R&D.*


*The unglamorous work of digitizing the wet lab — structuring data, defining workflows, capturing decisions — has become the foundation for something bigger. We are building an AI Scientist with the ambition to get molecules to the clinic twice as fast.*


*This essay defines the AI Scientist, examines what makes it challenging to build, and shares an architecture for how it works.*


## The tinkering tax


*"It would have been a fantastic and vital discovery if I had been a good biologist. But I wasn't a good biologist.”*


Richard Feynman said this about his work in genetics. He was a legendary tinkerer whose approach to physics was to try things, break things, and learn by doing. He was curious enough about biology to spend a sabbatical year in a Caltech molecular biology lab, publishing findings that would later be cited by Francis Crick. But the wet lab blunted even his problem-solving skills. Experiments took weeks, samples were unreliable, and results were ambiguous. Tacit knowledge, like opening a test tube with one hand while pipetting, mattered as much as his brilliance.


He moved on and the story repeats. Biology is slow, expensive, and unforgiving. It charges a heavy tax on tinkering that many don’t want to pay.


Tinkering requires fast feedback loops. The faster you can test an idea, the more progress you make. The software industry made tinkering free. Twenty years ago, it took a team of engineers and millions of dollars to ship software. Today, a teenager can launch an app to millions of people. New AI coding agents will compress software development timelines another order of magnitude.


## Pharma is not digital. Wet labs matter.


The same energy behind coding agents is now pointed at scientific progress. The opportunity is enormous.


But Silicon Valley has no idea how medicines are discovered and developed. It’s been no surprise that while coding agents have taken over, scientific agents haven’t taken off. That’s because AI for science has a big wet lab problem.


Picasso said, “when art critics get together they talk about *form* and *structure* and *meaning* . When artists get together they talk about where you can buy cheap turpentine.” Today, scientific agents discuss science — summarize literature, generate hypotheses, produce charts — but they can’t do science. Scientists need cheap turpentine. Reagents already on the shelf, data automatically off the instrument, next experiment ready to go. It requires wading into the messy physical world.


That seam between the physical and digital worlds is where we are building an AI Scientist to operate.


## The AI Scientist is your infinite program team


The AI Scientist is an infinite program team: scientists, engineers, bioinformaticians, regulatory affairs, and program managers working around the clock. It wires together the digital and physical worlds of R&D. Predictive models, data infrastructure, and wet lab execution all feed a single compounding loop: design an experiment, run it, and learn.


The human scientist sets a goal. The AI Scientist generates a plan, selects predictive models, suggests experiment designs, routes high-throughput automation or guides bench work, captures results as structured data, and recommends the next step. At every step, it surfaces key decisions for approval.


The AI Scientist works across R&D. A discovery biologist optimizing antibody affinity, a preclinical scientist running animal studies, and a process development engineer increasing yields have the same feedback loop.


The human scientist is the primary investigator and program leader, setting direction and making tough calls. Great scientists do something no system can: they recognize when something unexpected is worth following. Judgment is their job.


## How we are building the AI Scientist


Every R&D organization already has the beginnings of the AI Scientist. What’s missing is the system that ties it all together. Predictive models that need GPU clusters. Instrument data locked in proprietary formats. Years of experimental history waiting to be reused.


We are doing the messy work of connecting these pieces into a system that reasons, acts, and improves with every experiment. This is the architecture of an AI Scientist operating at the seam between physical and digital.


#### *Every model, no assembly required*


Biology has no shortage of models. Large language models can generate hypotheses, create experimental plans, analyze incoming results, and draft study reports. Predictive models for everything from biophysical properties to ADMET to patient tumor response are proliferating. The AI Scientist harnesses these models, providing the right scientific context and shaping outputs to meet experimental and regulatory standards.


The AI Scientist should remain model-agnostic. No model is perfect and state of the art changes frequently. Scientists are busy making medicines. They don’t have time to track which protein stability model leads the field this month or which LLM can best transcribe paper batch records. Seamless access to all models allows the AI Scientist to pick the best tool for a given task, using multiple models in parallel to improve accuracy. Through[partnerships like Lilly TuneLab](https://www.benchling.com/news/benchling-and-lilly-tunelab-partner-to-democratize-access-to-ai-models) , we are hosting and distributing predictive models that any scientist can run directly in their workflow, a level of access that was once limited to the largest companies.


#### *More wet lab execution, less overhead*


The AI Scientist accelerates both human and robotic wet lab execution. Biology is not software. Cells need to grow. But much of what slows down experiments is not biology. It is overhead that can be compressed.


Humans running experiments are here to stay. Pipetting is not the problem: re-creating an optimized protocol, synthesizing a molecule that’s in storage, forgetting to request material lots, reading instrument manuals, writing a Python script to parse strange data formats, and painstakingly creating charts are the problem. The AI Scientist can do all of this.


Lab automation is making rapid progress. Robotic workcells used to be prohibitively expensive, difficult to set up, and even harder to change. New offerings support greater flexibility and open connectivity at a variety of price points. The AI Scientist tells the robots what experiments to run and what data it needs back. We are building standardized integrations with partners like[HighRes](https://www.benchling.com/blog/benchling-and-highres-lab-automation) to streamline this, solving a decades-old infrastructure problem. Sometimes, these robots will sit at CROs and in autonomous labs like Twist Bioscience, Ginkgo Bioworks, and Adaptyv, which can programmatically receive orders for molecule synthesis and assay data generation.


#### *Structured data, memory that lasts*


The AI Scientist needs access to structured data that captures the complexity of science, from molecules to experimental protocols to assay results. This must also include process data: the workflows, handoffs, and decisions that encode how an organization works. Together, these are a living record of R&D and a starting point for the AI Scientist. We have built this at Benchling, and it is the basis of a scientific intelligence that compounds with each additional experiment.


The biopharma industry wastes institutional knowledge at an industrial scale. Pharma acquires a biotech, the team moves on, and years later a scientist unknowingly repeats the same study. Experimental data and know-how are too heterogeneous to easily share between companies. Every R&D organization is a nation-state with its own language, legislation, and currency. It turns out LLMs are excellent diplomats.


Structured data connects an organization, so a process development campaign helps research hand off the next molecule pre-optimized and bioanalytical data from patient samples informs early discovery decisions. We are building an[ecosystem of MCP connectors](https://www.benchling.com/ai/connectors) so data flows seamlessly in and out of Benchling to other repositories and specialized tools.


#### *Expert interfaces, proactive intelligence*


The scientific data and workflows in Benchling enable the AI Scientist to act proactively. It can request material lots before an experiment stalls, process and analyze instrument data as it comes off the machine, and compare results to prior studies in search of anomalies. No prompt required.


Human scientists will have multiple ways to engage. Chat is one of them, but work like designing molecules, tracing sample lineage, and verifying study data is visual, precise, and contextual. These tasks need purpose-built interfaces. The AI Scientist assembles the right interface for the task at hand from the building blocks already in Benchling today.


Most science happens away from the computer. The AI Scientist already captures and parses data from instruments. As voice and video models improve, ambient data capture will extend across the entire experimental lifecycle, turning manual execution into codified knowledge.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Free the scientists, let them tinker


Big tech and AI labs are hiring scientists, building wet labs, and training proprietary models. Some are vertically integrated down to the hardware. They have the capital and compute to outgun everyone. China has ambitions to become the global leader in science and has already overtaken Europe in molecules produced.


The largest pharma companies still have generational moats: capital, patent portfolios, clinical infrastructure, regulatory expertise, manufacturing scale, and global distribution. Boston biotech, CROs, and academic labs are not going anywhere just yet.


But the world is shifting. The newest generation of AI-native biotechs have focused on closing the loop between computation and experimentation from day one.


In the AI era, the traditional biopharma industry is the underdog. The question is whether the industry will wait to find out what it means.


It doesn’t have to. The biopharma industry holds many of the necessary pieces. They are just disconnected. The opportunity is to connect it all: the models, wet lab, data, and interfaces, into a shared scientific infrastructure that runs the AI Scientist.


If the biopharma industry acts, it can define the AI era. If it waits, it will inherit it. The stakes are high. It takes over $2B and 10 years to make a medicine.[We desperately need more medicines, faster](https://www.benchling.com/blog/rebuild-biotech-for-the-ai-era) . The biopharma industry doesn’t need to wait for permission. We can put the AI Scientist in the hands of every scientist who dreams of tinkering.


*If you’re interested in early access to the AI Scientist, head to*[benchling.com/ai](http://benchling.com/ai) *or sign up for our*[waitlist](https://www.benchling.com/ai-scientist-waitlist) *.*
