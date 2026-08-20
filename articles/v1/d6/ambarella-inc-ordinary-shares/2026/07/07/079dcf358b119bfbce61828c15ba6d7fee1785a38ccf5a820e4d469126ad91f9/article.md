---
schema_version: "1.0.0"
document_id: "079dcf358b119bfbce61828c15ba6d7fee1785a38ccf5a820e4d469126ad91f9"
company_key: "ambarella-inc-ordinary-shares"
company: "Ambarella Inc."
source_id: "ambarella-inc-ordinary-shares-rss-4b0311b4df52"
canonical_url: "https://www.ambarella.com/blog/the-edge-cases-were-never-edge-cases/"
published_at: "2026-07-29T19:47:01+00:00"
first_seen_at: "2026-07-29T21:07:49.516797+00:00"
fetched_at: "2026-07-29T21:07:50.470795+00:00"
content_hash: "sha256:70034d40603dfce49f7df16a7d27cc23b814f37ffb384b223d7c27d4aa2d3c8a"
---

# The Edge Cases Were Never Edge Cases

On July 8, the National Highway Traffic Safety Administration sent a[letter](https://www.nhtsa.gov/sites/nhtsa.gov/files/2026-07/ADS-developers-letter-july-2026.pdf) to every autonomous vehicle developer in the country. The agency had documented a pattern: driverless vehicles entering active emergency scenes, blocking ambulances and fire crews, and failing to respond to flashing lights, flares, smoke, and traffic cones. Administrator Jonathan Morrison called the failures a functional insufficiency and gave developers until the end of the month to present solutions. One sentence in the letter deserves more attention than it has received. “Emergency scenes are not rare or extreme ‘edge cases.'”


The quotation marks around ‘edge cases’ are doing a lot of work. For years, the industry has used “edge case” to describe any situation a deployed system fails to handle: the fire hose across the road or the officer waving traffic through a red light, as examples. These are scenarios that existed nowhere in the training data. The term “edge case” implies these situations only exist at the margins of the operating environment, and that a system which handles the common cases has substantially solved the problem. Morrison has now stated plainly what operators of physical AI systems have understood for some time. Machines that operate in the physical world encounter the unfamiliar *continuously* . The category of unusual situations is what the physical world presents every day, and a system that cannot handle the category will be ineffective.


## **Detection was never the hard part**


The failures that the NHTSA describes are almost never failures of perception. A modern autonomous vehicle will almost certainly detect the fire truck. Its sensors will register the flashing lights, the flares, the cones, and the person standing in the roadway. Detection models for these object classes are mature, and they perform well within their power and latency budgets. However, what the documented incidents reveal is a gap between registering objects and understanding broader situations.


A fire truck parked at an angle across two lanes, lights active, with personnel moving around it, defines a safety perimeter with implicit contextual rules: yield to anyone directing traffic, expect people to step into the roadway without warning, and do not enter the defined space. A human driver reads this instantly and holds the entire situation in mind while acting. A perception pipeline simply outputs a list: truck, person, cone, light source. The list is accurate, but the meaning is absent. Every documented failure mode in the NHTSA letter follows this pattern. The system saw what was there but did not grasp what it meant.


This is the distinction between perception and reasoning. The first blog entry in this series, “[Everything Is Going to Be Driven by Algorithms](https://www.ambarella.com/blog/everything-is-going-to-be-driven-by-algorithms/) ,” described the shift from stateless inference pipelines to agentic systems that pursue goals and maintain context. The follow-on pieces examined how these systems are composed from specialized models coordinated by language-capable orchestrators, and why building them requires systems-engineering discipline. The NHTSA letter describes what it looks like when the industry deploys the perception layer at scale without the reasoning layer to match.


## **What changed, and why it arrived just in time**


Until recently, scene-level reasoning was not available on the device at all. Understanding that a fire truck at an angle means “perimeter” or “stay away” requires a model that can relate visual elements to situational context, and in the past, models with that capability were too large to run anywhere except a data center. The currently deployed generation of physical AI systems reflects that constraint. Perception runs locally because it must, but anything resembling judgment either runs remotely, runs nowhere, or is approximated by custom rules that enumerate the situations their authors anticipated.


Custom rules are precisely the wrong tool for a problem whose defining feature is novelty. A set of custom rules covers the situations someone thought to write down in advance. In the physical world, the space of possible situations is unbounded, and no finite set of rules can cover it.


Vision-language models change the equation by relating what they *see* to what they *know* , which means they can interpret a scene they have never encountered by reasoning over its elements (an emergency vehicle in an unusual position and active personnel, which means a perimeter that requires yielding). Quantized and optimized models with this capability now run within the power envelopes of embedded processors. The reasoning that was once confined to the data center can now operate where the sensing occurs, inside the loop that decides what the machine does next.


The location of the reasoning step is important because of time. A vehicle approaching an emergency scene at 40 miles per hour covers nearly 60 feet per second. A round trip to a cloud service for interpretation, assuming connectivity is even available, inserts delay into the one part of the loop that cannot tolerate it. Machines that act in the physical world run a continuous cycle of seeing, thinking, acting, and learning, and the cycle can only run as fast as its slowest element. When the thinking happens off-device, the machine is waiting on a network at exactly the moment it should be slowing down and pulling over. This illustrates the core argument that the full loop (see, think, act, learn) belongs on the machine itself.


## **Reasoning completes the perception layer**


None of the above is meant to diminish the importance of the perception layer. Purpose-trained detection models remain the right tool for high-frequency tasks: finding the vehicles, the people, the lane markings, the cones, at the proper frame rate, within a strict power budget. The reasoning model sits above these models, consuming their outputs alongside its own scene-level understanding, and determining what the situation requires. Each layer does what it does best. The specialist models deliver precision. The reasoning layer delivers judgment. The combination delivers behavior that holds up in situations neither layer could handle alone.


This division of labor is also what makes the approach viable on embedded silicon. A combined system distributes the work: compact detectors running continuously, a reasoning model engaged at the cadence needed to make appropriately quick judgements, and the whole arrangement fitting within the thermal and power limits of a machine that has to carry its own compute onboard.


**The lesson extends beyond vehicular autonomy**


Although Morrison was critiquing autonomous driving, the same problem exists in every domain where machines take action based on camera input. For example, a security system that detects a person after hours has successfully perceived a human being. However, the question of whether that person is an intruder, a returning employee, or a paramedic responding to a call on the property is a question of reasoning. Likewise, a drone inspecting a substation structure might perceive corrosion. However, the judgment of whether the corrosion pattern warrants an immediate alert is the difference between a sensor and a teammate.


In each case, the problem is that perception exists on the device, while leaving the “thinking” step to humans reviewing footage, or custom rules for known situations, or cloud services arriving after the moment has passed. The NHTSA letter documents the issues that pattern produces at scale in one of the most unforgiving deployment environments there is. Other industries have the opportunity to read the letter as a warning.


**Building for the situations nobody wrote down**


Ambarella has spent two decades building silicon for machines that operate in the physical world, with AI processors deployed today across automotive, security, and robotics at production scale. That history shapes our perspective: that the processor in a machine that acts among people has to run the perception stack and the reasoning layer together, on the device, within the machine’s power and thermal budget. Ambarella’s platform is built for that composition, spanning compact processors for intelligent endpoints through the N1 family for on-device reasoning, with a common software stack across the portfolio through the Cooper development platform.


The term “edge case” survived as a category in artificial intelligence because the systems of the last decade could not be asked to handle novelty, so novelty was defined as “out of scope.” Today, there must be a higher standard for every machine acting in the physical world. The systems that meet that standard will be the ones designed with an understanding of Morrison’s critique. The edge cases were never edge cases. They are the job.
