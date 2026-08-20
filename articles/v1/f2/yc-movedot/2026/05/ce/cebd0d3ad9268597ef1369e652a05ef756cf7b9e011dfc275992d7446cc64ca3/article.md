---
schema_version: "1.0.0"
document_id: "cebd0d3ad9268597ef1369e652a05ef756cf7b9e011dfc275992d7446cc64ca3"
company_key: "yc-movedot"
company: "MOVEdot"
source_id: "yc-movedot-news-import-b09513d212fd"
canonical_url: "https://movedot.ai/blog/closing-the-loop-between-simulation-driver-in-the-loop-and-real-measurements"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-25T16:12:42.390551+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:3d8609a440766d54379a7ad0c6e2cfd09d43b8137e3af7b6b4f456c7cfca467b"
---

# AI Agents closing the loop between simulation, driver-in-the-loop, and real measurements | MOVE. Blog

A real MOVEdot use case showing how AI agents connect simulation, driving simulators, and real measurement into one faster vehicle-development loop.


Surrogate trust loop


90%


less engineer time


From


1 week


to


Hours


Design iterations


80%


less engineer time


From


Weeks


to


Days


Senior-engineer hands-on time, manual workflow vs. agent-run. From this project: the surrogate trust loop that would cost an engineer a week ran in a couple of hours, and design iterations that used to take weeks run in days.


On most vehicle programs, the bottleneck is not the data. It is not the sensors or the simulations either. Teams have plenty of all three. The bottleneck is the engineer in the middle, because every piece of data has to pass through a person to become useful, and a person is a narrow channel.


The result is familiar. Most teams use a fraction of the data they collect. They don't run every simulation they want to run, or every validation they know they should. Not because the methodology is wrong, but because the methodology runs on human time, and human time runs out.


This is a walkthrough of a real project we ran to show what changes when AI agents take over the manual work of moving across a development program. To keep it concrete, we scoped it to handling and ride on an SUV, connecting simulation, a driving simulator, and real measurement into one loop. It's an illustration, but it scales the same way across the rest of a program.


## Where the time actually goes


Every engineer knows the V model. Targets cascade down into subsystem and component requirements on the left. Verification climbs back up the right, from software and hardware to the full vehicle. The methodology is good. That has never been the problem.


The problem is that every step is manual. Cascading targets down is manual. Running the verifications back up is manual. And connecting the two sides, matching what you designed against what you measured, is the most manual part of all.


Everyone agrees we should shift left, bringing validation and verification earlier. It's the right instinct. But shifting left says nothing about how to make it possible. If connecting the two sides was already hard, now imagine doing it for every iteration loop, earlier and more often. The manual cost grows fast.


Agents change the math here, not the methodology. The V stays. What changes is that the horizontal connections, the ones that used to cost an engineer days each, get handled by an agent that can move in any direction the project needs. Same V, run far more efficiently.


## The AI layer


This only works if the agent can reach everything. A useful agent is not a chatbot attached to one tool. It sits on a layer connected to every source the project touches: vehicle data, rig data, simulations and simulators, internal databases, test standards, internal reports, supplier data, video.


We made this layer native to the tools engineers already use. In this project it connects to VI-CarRealTime for simulation, to VI-DriveSim for the driver-in-the-loop simulator, and to HBK's systems for real measurement. Those are most of the pieces you need to close a development loop. With them in reach, the agent runs the iterations instead of waiting on a person to move files between systems.


## The project


The targets came from the market, the way they always do. Sportier than the outgoing car. A more premium feel. No compromise on ride. And sensible on cost, because cost per unit is real.


We picked four maneuvers: constant radius and step steer for handling, speed bump and rough road for ride. From those, the KPIs and criteria the team already works with. Before touching the new setup, the agent reviewed the benchmark fleet, subjective feedback, objective data, and simulation together, to help set where the targets should land.


## AI-powered model validation


Before any of this is worth doing, you have to trust the model. Every engineer will tell you the same thing: validating a vehicle model is slow, so it doesn't get done as often as it should.


We turned validation into something the agent does on its own, and called it auto-validation. It pulls data from previous vehicles, finds the relevant maneuvers, and crops them. It prepares the simulation files and reruns the maneuvers in VI-CarRealTime. It overlays the traces, compares the channels the analysis cares about, and flags where simulation and measurement disagree. It even proposes the model changes that would reduce the error.


There is no magic in the process we show: the AI will not design the vehicle for you. We have developed vehicles, and we are pragmatic about it. The expert, you, defines the methodology. The agent then runs it, at a scale no person has time for.


## Hundreds of simulations, then a surrogate


Once the model is trusted, the goal was to find the setup that best meets the targets across all four maneuvers at once. That is a multi-objective optimization, and the natural tool is a genetic algorithm. But a genetic algorithm needs to evaluate thousands of candidate setups, and a full VI-CarRealTime run for each one would take days.


So the first step was a feasibility check. Running the full count directly was not practical. The better route was to map the parameters that matter most, then replace the slow solver with a fast stand-in. A sensitivity analysis narrowed the inputs to the twenty parameters that drive these KPIs at this stage of the project.


On those twenty, the agent built a surrogate model: same inputs in, same KPIs out, but thousands of times faster. Trained on VI-CarRealTime data, it keeps the physics-based accuracy of the solver with the speed of a lookup. That is what makes thousands of optimization iterations possible at all.


We don't impose a fixed surrogate strategy. The agent builds the one the problem needs, the way an engineer would.


## How the surrogate gets trusted


The surrogate is only useful if it is accurate, and accuracy is something the agent checks for itself rather than assuming. Here is the loop it runs.


It draws a batch of simulations from a design of experiments and runs them in VI-CarRealTime. It fits a surrogate on the runs it has so far. Then it validates that surrogate against fresh points it has never seen, checked against VI-CarRealTime as the source of truth. If the error is still above two percent on the parameters that matter, it draws more samples where the surrogate is least certain and repeats. It stops only when the surrogate is accurate enough to trust.


This is the idea in one figure: the agent closing a loop on its own, checking its own work against a trusted reference, and iterating until the result is good enough. Doing this by hand would cost an engineer a week. Here it took a couple of hours, and the VI-CarRealTime runs went to VI-DataDrive Cloud in parallel, so the total time dropped further.


## Picking from the Pareto front


With a validated surrogate standing in for the solver, the genetic algorithm ran. Every point on the resulting front is an optimal setup, each with a different trade-off between handling, ride, and cost per vehicle.


The agent helped narrow it to three. A value option: low cost increase, giving up a little handling and ride. A balanced option: a moderate price increase with strong handling and ride. And a performance option: the highest cost, with the best scores on paper.


The pattern was the one you would expect. Spend more per unit and the setup fits the targets better. But simulations can only tell you so much. Between three good setups, which one would a driver actually prefer?


## Into the simulator


So the agent connected to the driving simulator. It writes vehicle models straight into the VI-DriveSim database, the driver drives them, and the results come back automatically. We have integrated this into several labs already, and the agent pulls in everything around the session: the data, the video, the radio transcriptions, the driver ratings, all the context it needs.


The driver ran the three setups back to back across a range of maneuvers. That left a lot to make sense of: forty VI-CarRealTime files, more than five hours of driving-loop data, and two hundred driver ratings and notes to correlate. By hand, that is a nightmare. With the agent, it became a data-driven decision.


## Performance wasn't worth the extra cost


The balanced setup showed clear improvement across everything the project cared about, at a moderate price increase. The performance setup cost more again, but the driver could not tell it apart from the balanced one by feel. Four hundred dollars per vehicle for a difference no one in the seat could detect.


That decision, balanced over performance, was one call. A real program makes hundreds, sometimes thousands of them, each one connecting simulation to a driver's hands and back to measurement. Do that manually and you understand exactly why a vehicle takes years to develop.


## The loop, closed


This project showed that the loop between simulation, driver-in-the-loop, and reality can run in one shared place, with an agent moving between them instead of an engineer carrying files across by hand. Cascade a design down, verify it back up, correlate sim against rig against measurement, and feed the result into the next iteration.


Do that fast enough and the design iterations that used to take weeks take days. This is the future of engineering, and it is already here.


---


If you develop vehicles and recognize the bottleneck this started with, we would like to talk. We have dozens of these demos across different areas. Get in touch:founders@movedot.com , or[www.movedot.ai](https://www.movedot.ai/) .
