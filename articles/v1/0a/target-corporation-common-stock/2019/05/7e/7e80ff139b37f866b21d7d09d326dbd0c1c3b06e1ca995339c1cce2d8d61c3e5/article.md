---
schema_version: "1.0.0"
document_id: "7e80ff139b37f866b21d7d09d326dbd0c1c3b06e1ca995339c1cce2d8d61c3e5"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/chaos-engineering-at-target-part-2"
published_at: "2019-05-09T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:3381ae4a52d179f25df62c1a47d46a556e4404f5e75ef236d35d425e64194d9b"
---

# Ɔhaos Ǝnginǝǝring @ Target - Part 2

What is a Game Day?


A Game Day is an event hosted to conduct chaos experiments against components of your system to validate or invalidate a hypothesis about a system’s resiliency.


Game days provide teams with the opportunity to execute potentially highly disruptive experiments in a safer, more controlled way. Teams can then exercise their recovery procedures and validate their resolution protocols.


A game day can be as simple as verifying alerting works for an expected fault of a new application feature or as complex as a full regional failover to simulate a cloud provider outage. Additionally, they can also be used to prove an application’s behavior or validate a major incident remediation action plan. Game days play an important role in teaching teams about their systems, enabling teams to learn more about how they respond and how to recover in the event of an outage.


What is a Chaos Experiment?


A Chaos Experiment is an experiment to intentionally break or disrupt an application based on a hypothesis of how you expect that application to fail.


How to Prepare a Chaos Experiment


Preparing chaos experiments for a game day is just as important as the game day itself. With the right preparation ahead of time teams learn more from their experiments and game days.


- Identify the application to undergo experimentation.


- Agree on the details of the experiment including:


1. Attack Type - the chaos experiment to be run (CPU, Memory, Network Latency, etc.)


2. Duration of the Attack – the length of time to run the experiment


3. Scope of the Attack - one instance or an entire cluster of instances


4. Environment where the attack will occur – local, test, or production


5. Load (simulated, synthetic, or actual user data) intended to exercise the application during the experiment


6. Observability - the telemetry setup (alerting, metrics, and reporting) for the application that informs teams which are participating in the experiment that something is happening


7. Hypothesis - what impacts, behaviors, and failures you expect will occur during and resulting from the experiment


8. Results - the actual impacts, behaviors, and failures from the experiment


9. Action Items – when the experiment concludes, the team identifies work that needs to be done. For example, to retry the experiment with a different set of constraints, to add better telemetry, or to fix a bug in the application


How to Prepare for a Game Day


Once you've defined the chaos experiments, the next step is to identify the teams/stakeholders that need to participate in the game day to perform the experiments. Having the right teams and Subject Matter Experts (SMEs) participating in the game day not only helps facilitate better execution of the experiments, but it also ensures unexpected disruptions are contained. Additionally, when an experiment goes sideways (and when you first start experimenting, they do) having the ability to recover quickly is critical to continue to experiment while maintaining the trust of your partner teams.


Logistics matter. Whether your team is all at the same campus or located around the world, coordinating the time and place with the right methods of communication is critical to the success of any game day. Often managing the human to human interactions of a game day is much more of a challenge than the technical execution of the experiments.


Communication is critical when conducting experiments. Not only should the teams experimenting be involved, but so should the users/stakeholders – knowing what systems are being affected while experimenting is critical to letting other parts of the business see how the experiments work.


Observability is required, not only for the ability to monitor and measure the experiments but also for the response to and recovery from an experiment.


Recovery plans. Not every experiment requires one, but if the experiment targets production or may impact customer-facing applications, a recovery plan should exist before any experiments are executed.


Transparency leads to trust. Over-communicating with engineering and business teams about your experiments builds their trust in that your experimentation results in better systems. It can also lead to future partnerships with these teams asking you to experiment actively with a system on their behalf.


After every experiment, a live retro helps everyone improve. Discuss what went well, what could have gone better, what needs to be added, and what might need to be taken out before the next experiment. For example, maybe your customers did not know about the outage, so better communications need to be a priority. What if there was no application impact during an experiment, but you expected there to be? Perhaps the recovery playbook for an experiment was missing information so the on-call engineers could not restore the steady state of the application before the experiment.


The first few game days can be difficult because we don’t typically think about how to break applications in our designs. That is until the team gets comfortable with the un-comfortability of breaking their stuff. If you can embrace the chaos and get a few experiments completed, you will start to find a way forward. It’s not how teams traditionally think about their systems or how they are trained to develop or support them. However, it is exactly this mindset shift that enables more resilient applications to emerge.


A Game Day at Target


In a recent game day, we partnered with one of Target’s platform teams to run experiments against proxy servers hosted on one of our cloud providers supporting Target.com.


In this experiment, the team knew what component to break and how it should break, but it did not break as they expected. The proxy servers auto-scaled to handle the latency.


During the next set of experiments 500m/sec and 1000m/sec, the proxy servers appeared to handle the increased latency. In reality, unexpected performance behavior started to impact the synthetic load generator and metrics logging from the servers at those latency levels impacting the experiments themselves.


During these experiments, the team used existing playbooks detailing how to restore the proxy servers to their expected steady state. The team followed these playbooks' instructions to restore the services after each experiment.


The teams participating in this game day were very familiar with chaos experiments, and yet still produced numerous learnings around application behavior and findings for what things need to be fixed before their next game day.


Future Game Days at Target


As teams at Target continue to adopt chaos engineering, they require a more natural path to running chaos experiments and participating in game days. Not only will the chaos tools need to adapt to become more usable and available where teams need them; the barrier to using these tools to run experiments must eventually disappear.


This methodology increases collaboration between teams with dependent applications or similar technology stacks from which the operationalization of established chaos experiments will begin to emerge. Teams may then be able to replay their own experiments or introduce new experiments from other teams in the CI pipeline.


Experimentation informs future design. As teams at Target embrace chaos engineering, their discoveries will transform how they view their applications, the platforms which they run on, and how to design the resilient applications of Target’s future.


## RELATED POSTS


### Ɔhaos Ǝnginǝǝring @ Target - Part 1


By Brian Lee, Jason Doffing, and Sean Peters, February 5, 2019


Chaos engineering is the discipline of experimenting on a distributed system in order to build confidence in the system’s capability to withstand turbulent conditions in production.
