---
schema_version: "1.0.0"
document_id: "1d6e4f4ca38f78ed27fe3c39fc51b8978e9a9bc55daa09dc580f82ef830aa32e"
company_key: "innodata-inc-common-stock"
company: "Innodata Inc."
source_id: "innodata-inc-common-stock-rss-09b9ff75e5ec"
canonical_url: "https://innodata.com/physical-ai-starts-with-data-not-models/"
published_at: "2026-03-20T17:13:01+00:00"
first_seen_at: "2026-07-20T23:17:35.625386+00:00"
fetched_at: "2026-07-28T21:56:54.694470+00:00"
content_hash: "sha256:adbd3372650f687f5aef2fe0fc293b77ce0ed82d39dc500c4628afe2a4f7ba5a"
---

# Physical AI Starts With Data, Not Models

# Physical AI Starts With Data, Not Models


#### Frank Tanner, VP of Computer Vision and Robotics
Simranjeet Bal, Director of Solutions
Jefferson Barlew, VP of Delivery


#### March 20, 2026


Last week, an 18-month-old child opened a back door for the first time. After being told, “let’s go outside,” he walked to the door, reached up to the handle, pulled it down, and swung the door open. No one had explicitly taught him how to operate that specific handle.


This small moment illustrates a pattern that is just as relevant to Physical AI as it is to human development. At the same time that the child was developing the motor coordination required to grasp and pull the handle, he was also building a structured understanding of the world: doors enable transitions between inside and outside; handles are manipulable components; certain actions cause predictable state changes.


Physical intelligence is not only about motor control. It is about learning categories, properties, processes, and relationships. In short, it is about building ontologies and leveraging them to understand both known and novel objects and situations.


For decades, robotics research focused primarily on the mechanical and control aspects of interaction. That focus was necessary. If a system cannot reliably grasp a handle, it does not matter whether it understands what a door represents. Today, advances in perception, representation learning, and emerging world models have shifted the bottleneck. The challenge is no longer only executing actions. The challenge is understanding which actions are appropriate in a given context.


This shift places ontology and data at the center of Physical AI.


## Designing Data Before Behavior


What makes a Physical AI system behave intelligently? The industry’s revealed answer, judging by investment, research output, and conference agendas, is hardware and model scale. Experience shipping real systems across insurance, infrastructure, agriculture, and many other domains suggests something less glamorous and more fundamental. First design the data. Then behavior follows.


For years, designing data first meant teaching systems to detect damage, defects, and anomalies in claims photos, inspection imagery, and agritech datasets. The move into Physical AI is a natural extension of that work. The same question remains central. What has the model actually experienced, and how is that experience structured?


Traditional computer vision has been shaped by large public datasets such as ImageNet, which drove progress in static object recognition. Vision language models extended this paradigm to web scale image text corpora such as LAION-5B and Microsoft COCO, learning shared embeddings between pictures and captions.


These datasets are powerful, but they largely describe the world from a third person perspective. They focus on what is in a scene, not how the scene and its available actions evolve as interactions unfold. As a simple example illustrates in the next section, the same object configuration can represent entirely different task states depending on context.


Physical AI pushes directly against this limitation. Achieving long-horizon tasks in the physical world requires reasoning over context, state, and the consequences of action.


## From Objects to States


Consider a pile of clothing:


- In a laundry basket in a utility room, it implies starting or continuing a wash cycle.
- On a bathroom floor, it may signal clutter that requires cleaning.
- Folded on a bed, it suggests a nearly completed task.
- Left in a hallway, it blocks movement and requires clearing.


To a generic classifier, all four scenes reduce to “clothes on a surface.” To a task-aware system, they represent distinct states with different priorities and next actions. The difference lies not in pixels alone, but in the ontology linking object, location, state, and human intent.


*Figure 1 – C ontextual Variation of Identical Object Classes*


Once you have that kind of task‑centric data, the job becomes assigning ontology states that match what is really happening. The same logic applies to dishes on a counter, tools on a workbench, or components in a warehouse bin. A dish is not simply “a plate.” It may be:


- a dirty plate by the sink,
- a clean plate on a drying rack,
- or a plate stored safely in a cabinet.


Each state connects to a different action. A dish is not just “a plate,” it is “a dirty plate by the sink,” “a clean plate on the rack,” or “a plate safely stored in the cabinet,” and each state connects to different affordances – the actions the scene makes possible and the ones that are actually likely. Most of the time “plate on the counter” implies “put in the dishwasher” or “put away,” but in a very different context – say, a burglar in the house – that same object might suddenly afford “use as a weapon.” Physical AI systems need to learn both: the long‑tail of what’s possible and the narrower band of what humans will usually do next in a given situation.


*Figure 2 – Object States and Context-Dependent Actions in a Task-Aware Ontology*


Without explicit state modeling, large foundation models often collapse contextual differences. A clean plate on a rack and a dirty plate in a sink may both be represented primarily as “plate,” even though they imply opposite actions. In real systems, this can produce redundant behaviors, such as washing already clean items, or missed tasks, such as failing to clear clutter that blocks movement.


Physical AI systems become useful when their training data makes these distinctions explicit. Each state is not just a label — it implies a different set of available actions and likely next steps. The most basic version of this is differentiating between:


- needs attention now,
- in progress,
- already done.


## Ontologies as Structured World Models


These examples illustrate a deeper point. The difference between a dirty plate and a clean plate is not just visual. It is structural. It reflects a set of relationships between objects, locations, states, and actions. At present, to make those distinctions reliable and scalable, they must be encoded explicitly. That structure is what an ontology provides.


An ontology is more than a label set. It is a structured representation of entities, attributes, relationships, and permissible state transitions. A well-designed ontology is comprehensive enough at the top level to accommodate situations it has not yet encountered. It defines what distinctions matter for a specific task and how those distinctions influence action.


In prior industrial use cases, ontologies encoded business and safety significance: whether a visible issue required repair, claim adjustment, or monitoring. In Physical AI, the same principle extends to contextual tasks. Concepts such as “laundry,” “wet floor,” “full hamper,” and “clean towel” are not isolated nouns; they are nodes in a graph of states, constraints, and action implications.


That graph forms the backbone of a world model: an internal representation of “what is happening here” and “what should happen next.”


Web-scale pretraining teaches models to recognize patterns. It does not teach them which states matter for completing a task. Without training data that clearly links context to consequence, models struggle to determine what action is appropriate. Ontology-guided datasets make those links explicit.


## Expert-in-the-Loop Ontology Evolution


Ontology updates must be grounded in domain expertise. In household contexts, that expertise may come from operations teams, professional cleaners, or robotics researchers familiar with task sequencing. In industrial settings, it may come from line supervisors or maintenance engineers. This is another data design decision. Training on how the best practitioners work, not just how any practitioner works, is the difference between a system that completes tasks and one that completes them well.


The role of the data engineering team is to translate domain knowledge into precise schema modifications and propagate those updates through labeling, validation, and quality control pipelines at scale. Ontology evolution becomes a disciplined feedback loop between domain reality and structured representation.


Over time, the combination of task-centric sequences and evolving ontologies produces a richer internal representation than static benchmarks alone can provide. The system does not merely recognize “clothes.” It distinguishes between clutter, pending laundry, and intentional placement. It does not merely see “a plate.” It recognizes state, location, and consequence.


## Data as the Engineering Core of Physical AI


Physical AI does not fail because models cannot see. It fails because they cannot tell what matters. The goal is not simply to collect more data. It is to design data that makes state and consequence unmistakable.


That transition from object recognition to contextual state modeling is what transforms perception into behavior.


Collecting more data is not enough. Training larger models is not enough. This is worth stating carefully, because a similar claim about language was tested by large language models and found to be largely wrong. Scale alone, applied to unstructured text, produced capabilities that few predicted.


Physical AI may follow a different path. Language models produce tokens; the cost of a collapsed distinction is a worse sentence. Physical AI systems produce actions; the cost is a missed hazard, a redundant task, or a failed manipulation. That difference in stakes makes structured representations of state, context, and consequence harder to bypass through scale alone. Whether purely unstructured approaches will eventually close that gap remains an open question, but the systems working today in production are the ones where data design encodes those distinctions explicitly.


Physical AI will not advance simply because models get larger. It will advance when systems can tell the difference between “something is here” and “something needs to happen.”


That distinction lives in the ontology.


When data explicitly encodes state, location, and consequence, perception becomes decision and decision becomes action. The next wave of Physical AI will be shaped by teams that treat ontology and data design as core engineering disciplines rather than downstream annotation tasks.


[Connect with an Expert](https://innodata.com/contact-us)


### Bring Intelligence to Your Enterprise Processes with Generative AI.


Innodata provides high-quality data solutions for developing industry-leading generative AI models, including diverse golden datasets, fine-tuning data, human preference optimization, red teaming, model safety, and evaluation.


[Talk to an Expert](https://innodata.com/contact-us/)


Follow Us


[Linkedin-in](https://www.linkedin.com/company/innodata-inc-)


[Facebook-f](https://www.facebook.com/Innodata.Inc/)


[X-twitter](https://twitter.com/innodata)
