---
schema_version: "1.0.0"
document_id: "8f7b19a9c61298f3301f4a9b90bbf06f6aebaa4c09772a4487e9f26b7381f629"
company_key: "yc-movedot"
company: "MOVEdot"
source_id: "yc-movedot-news-import-b09513d212fd"
canonical_url: "https://movedot.ai/blog/running-unattended-simulations"
published_at: "2026-07-12T00:00:00+00:00"
first_seen_at: "2026-07-25T16:12:42.390551+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:22c7c92a6a7fb4d2d6b5965595426a8beb7f95f5fbfa1c90ef28d7503f76b00b"
---

# AI agents: what it takes to run simulations unattended

Over 5,000 multibody simulations overnight, no human in the loop, and a surrogate model at the end.


Simulation campaign


90%


less engineer time


From


3–5 weeks


to


1–2 days


Surrogate development


85%


less engineer time


From


2–4 weeks


to


2–3 days


Senior-engineer hands-on time: defining the study, handling failed and silently-wrong runs, post-processing, model fitting, and validation. Solver runtime is separate — the 6,400-run campaign itself finished in 9.7 hours overnight. Estimate based on comparable published CAE workflows.


In most engineering teams, simulation answers take days, and running the solver is the smallest part of it. Someone has to define the cases, set up the runs, watch the solver, debug what fails, and process the results. A single simulation tells you what one configuration does. The questions that fill a development program are about many: what happens if this hardpoint moves, what if these bushings get softer, which parameters actually matter.


Each of those questions can become a full study, with thousands of runs and one engineer in the middle of it.


That is the bottleneck AI agents remove. The agent organizes the whole study and runs it. It plans the cases, executes the simulations, catches and fixes the failures, and processes the results into something the team can use. Nobody sets up runs. Nobody watches the solver.


We did exactly that: over 5,000 multibody simulations overnight on a validated suspension model, a surrogate model fitted and checked on top of the results, and an interactive app at the end.


## The starting point: a validated model


Simulation data is only useful if the model matches the real system. Ours is a five-link rear suspension, validated against real K&C measurements. The agent ran the simulations, compared the curves against the measured ones, and found that the bushing stiffnesses explained most of the gap. A consistent scaling correction across bushings of the same type brought the model on top of the measured behavior.


Doing this by hand takes a long time: run, compare, adjust, repeat, for every curve. The agent automated the whole loop and delivered a validation report at the end.


The original model, the adjusted model, and the K&C measurement. After the agent corrected the bushing stiffnesses, the model sits on top of the measured curves.


Everything below builds on this model. Without this step, the 5,000-plus simulations that follow would just be 5,000-plus wrong answers.


## Overnight: over 5,000 simulations, zero manual interventions


The design space had 38 parameters: 30 hardpoint coordinates, each moving up to +/-15 mm, plus seven bushing-stiffness parameters and the spring rate. The agent planned the runs, executed them in batches, restarted where needed, and kept every result traceable to the exact inputs that produced it. The same workflow can run locally or across an HPC cluster, with the agent handling the queue, failures, and post-processing.


Some runs fail to converge. Worse, some converge to a wrong equilibrium while the solver reports success. The agent checks every result against physical plausibility, catches both cases, works out which parameter combinations break the solver, and continues.


When the campaign finished, the agent post-processed everything into one organized results set: kinematic and compliance curves plus the peak loads on every pickup point, for every configuration.


The campaign over one night. Failed runs are detected, diagnosed, and re-solved without anyone watching.


This also changes when simulation work happens. The agent runs while the computers would sit idle, at night and on weekends. From the first day of a new program, it can already be generating the data that program will need.


## Building the surrogate: from model fitting to a full study


Fitting a surrogate model was never the bottleneck. The reason most teams do not have one is everything around the fit: running enough simulations to produce the data, cleaning the results and removing the runs that failed, and knowing which modeling technique works at which sample size.


The agent ran that study too. It researched candidate techniques, fitted each one at different sample counts, and quantified the errors. The best technique at a few hundred samples is not the best at five thousand, and knowing that curve tells you how many simulations you actually need for the accuracy you want.


Prediction error per modeling technique as the number of training simulations grows.


Whenever working with surrogate models, it is very important to understand the impact in areas where the prediction is worse. Below we see that all of the critical cases are still acceptable.


The six worst predictions out of the entire design space, overlaid on the simulated curves. Everything else is better.


## What you can now answer in minutes


Once the surrogate exists, questions that used to be a simulation study become a lookup.


You can test any configuration in seconds instead of queueing a run. You can run an optimization over thousands of candidate designs in minutes instead of weeks. And you can run sensitivity studies to understand which parameters dominate the behavior and which ones you can stop worrying about.


One example from this campaign: several pickup-point loads are far more sensitive to vertical hardpoint position than to lateral, and the same study shows how the bushing stiffnesses, alone and in combination, shift those loads. That is the kind of answer that used to be a dedicated project and now takes minutes.


Sensitivity of pickup-point loads to hardpoint coordinates and bushing stiffnesses.


## Sharing it as an app


A surrogate model sitting in a script helps one engineer. So instead of a static report, the agent packages it as an interactive app, shared inside the platform.


A colleague opens it, moves a hardpoint, changes a bushing stiffness, and sees the suspension curves and the pickup-point loads respond instantly. No solver license, no queue, no request to another team. The people who produce simulation answers and the people who consume them are looking at the same live object, and the app can connect the surrogate with the underlying data and, when needed, with the simulations themselves.


The app built from the campaign. Move a hardpoint, see kinematics, compliance, and loads update.


## The next study starts from this one


The agent documented the whole process while doing the work: the model adjustments, the campaign setup, the configurations that failed and why, the modeling study, and the decisions along the way. The fitted models and the app stay in use. When the next suspension study starts, it starts from all of this instead of from zero, and the teams that used the app are already connected to the same source.


The same pattern applies to the simulation tools engineering teams already use: Adams Car, VI-CarRealTime, CarSim, and others. The agent operates the software. The methodology, the checks, and the results stay yours.


The change is in how simulation gets used. When an agent runs the studies on its own, handles the failures, and packages the results into models and apps that colleagues use directly, simulations stop competing for one engineer's time. The questions that never got asked, because nobody had a week to spend on them, get answered.


## Run this on your own models


None of this is specific to suspensions. Whatever simulation tools your team runs, an agent can operate them the same way. If you want to see AI agents operating your simulation tools on your own models, get in touch:info@movedot.com .
