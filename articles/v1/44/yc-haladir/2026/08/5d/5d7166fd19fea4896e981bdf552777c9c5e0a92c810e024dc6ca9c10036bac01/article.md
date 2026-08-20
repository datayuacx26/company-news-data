---
schema_version: "1.0.0"
document_id: "5d7166fd19fea4896e981bdf552777c9c5e0a92c810e024dc6ca9c10036bac01"
company_key: "yc-haladir"
company: "Haladir"
source_id: "yc-haladir-news-import-6dc51fdf4310"
canonical_url: "https://haladir.com/blog/nomos-sandbox"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:45:46.871820+00:00"
fetched_at: "2026-08-13T02:45:48.948926+00:00"
content_hash: "sha256:cc8cf00d4370880fe9a45178ef74e512ed6bbd95a21566f79e1505bb5c7c560c"
---

# Introducing Nomos Sandbox: A Decision Layer You Can Watch Work

Today we are launching Nomos Sandbox, an interactive and public simulation of Nomos, our AI decision layer for logistics. It is open starting today: request access at[haladir.com/demo](https://www.haladir.com/demo) , log in, and spend a session inside a working warehouse or sortation operation, watching decisions get made, questioned, and approved.


In every conversation we've had with 3PL operators, we heard the same thing: the hardest part of adopting AI is not the technology, it’s change management. Nobody wants to buy software they have not seen work on problems that look like their own, especially when it will affect high-leverage decisions across their operation. Nomos Sandbox exists so that nobody has to imagine the software working, and months of internal convincing can happen in a single session.


## A Classic Example: Warehouse Fulfillment


A great example of where Nomos excels is wave planning and dynamic pick-pack optimization for 3PL fulfillment operations. In this classic instance, every order arrives with a deadline. Some orders are already committed to a specific carrier and departure, while others could make any of several trucks, so the planner decides when and how the work happens and, where there is a choice, which departure an order targets, so that every deadline is met. Picking waves form around cutoffs, waves break down into tours, tours must fit their equipment, and every tour needs a pick route, so changing any one piece moves all of the others.


To make the outbound truck departures, picking must finish early enough to leave time for packing and staging, and today that information still largely lives in a floor manager’s head. This is the shape of the problem throughout logistics: many interacting constraints, hard deadlines, and conditions that change by the hour.


## How Nomos Decides


We have[written before](https://www.haladir.com/blog/nomos) that generative AI is a bad decision-maker and a good formalizer, so Nomos splits the work across three layers. The AI formalizes: it translates field definitions, informal rules, and unwritten constraints from systems like the WMS and TMS into the constraint models and objective functions that define the problem. Forecasting models estimate predicted inputs to the solver, like arrival volumes, service times, and order flow, from the deployment's own history and through interactions with the WMS and TMS.


The decisions that dictate the operation are made by deterministic solvers, aided by AI planning. Given the orders, the resources on the floor, and the outbound schedule, the planning engine decides what work happens, in what grouping, and in what sequence, under the site's hard constraints. Predictions feed the solver and the AI explains its results, but neither ever computes a decision on its own. That solver layer is worth opening up, because it is built differently from what most people expect.


## Leveraging Deterministic Solvers & AI


Nomos’ perspective on optimal decision-making follows the tradition of Nobel Laureate in Economics (also considered one of the fathers of artificial intelligence), Herbert A. Simon, who famously said in his 1978 “Rational Decision-Making in Business Organizations,” “Model construction under these stringent conditions has taken two directions. The first is to retain optimization, but to simplify sufficiently so that the optimum (in the simplified world!) is computable. The second is to construct satisficing models that provide good enough decisions with reasonable costs of computation. By giving up optimization, a richer set of properties of the real world can be retained in the models. Stated otherwise, decision makers can satisfice either by finding optimum solutions for a simplified world, or by finding satisfactory solutions for a more realistic world. Neither approach, in general, dominates the other, and both have continued to co-exist in the world of management science.”


By preferring quicker, more feasible decisions based on a more realistic construction of the operation over slower, more optimal decisions in a more unrealistic construction of the operation, Nomos tends to make far more intelligent decisions than traditional, pure mathematical methods of the past. By combining heuristic-based approaches with traditional MILP optimizers as a “refiner,” Nomos provides more realistic and dynamic decisions that matter.


For each decision class Nomos works with, we construct a purpose-built solver with three layers. A deterministic constructive heuristic builds a complete, feasible plan in business-priority order, so there is always a usable answer. Local-search improvement then applies hill-climbing moves, accepted only when they strictly improve the objective, while exact dynamic-programming sub-solvers handle the parts of the problem where exact answers are computable, such as pick routes. Commercial optimization engines can be added as refiners, accepted only when strictly better than the baseline on identical inputs. Inputs, and problem construction, are handled by both simulation models and AI-as-a-formalizer methods.


## Bullet-Proof Decisions


A plan is only useful if the people running the floor can trust it, so every output is built to be checked. Costs are computed exactly and the same inputs always produce the same plan, which makes plans auditable and comparisons reliable. A plan that cannot meet its departures is refused with the reason stated, and in some specific deployments of Nomos, before any plan is offered for release, it is run through a discrete-event simulation of the operation, with simulated figures labeled as simulated and estimates labeled as estimates until real measurements replace them. Every plan carries its decision trace: the inputs read, the constraints applied, the alternatives rejected, and the simulation result behind the recommendation.


Nomos AI lives under the same evidence rules. It reads data only through tools that return what the console is already displaying, every tool call appears in your session as a visible step, and its explanations are generated from the decision trace rather than reconstructed after the fact. When it proposes an action, in some specific deployments of Nomos, it must simulate before it recommends, and nothing is applied without approval.


## What This Means for Operator’s Bottom Line


The decisions Nomos makes are the ones that determine margin: which orders to release and when, how to group work, and how to allocate labor against cutoffs. Today they depend on experienced planners working under time pressure, and decision quality leaves when those planners do. A decision layer makes that judgment consistent across shifts, simulated before release, and measured after.


Nomos reads from the WMS, TMS, and OMS and writes back through their existing interfaces, your operational data remains yours, and automation is controlled per decision class, starting in review mode and expanding only where you enable it. The same procedure extends beyond picking to sortation, staging, dock scheduling, labor placement, linehaul routing, shipping operations, and transportation planning, where the domain model is built per engagement and the procedure stays the same.


## Inside Nomos Sandbox


Nomos Sandbox packages all of this into scripted, synthetic operations: a warehouse day with a disruption written into it, arriving at the same minute for every visitor, which makes sessions repeatable and easy to compare across a team. Nomos Sandbox is complete with guided walkthroughs, free play interaction, and videos accompanying each demo. When the disruption hits, you watch the full sequence, from a re-plan computed against capacity, labor, and deadlines, to the result simulated before commitment, to the AI explaining what changed while you decide whether to approve it. You can also freely interact with the agent, ask it questions, and test out different aspects of the operation.


What Nomos Sandbox cannot show is your own real operation, and that is what Nomos, our enterprise AI decision platform for logistics, is for. Our POC engagements start with your historical data, require the model to reproduce your measured days before its recommendations count, and produce quantified results before anything touches a live system. Nomos Sandbox is available today: request access at[haladir.com/demo](https://www.haladir.com/demo) , and if what you see matches your problems,[book a call with us](https://www.haladir.com/meet) to transform your logistics operations.
