---
schema_version: "1.0.0"
document_id: "d3083660133b32fa30c19fda4cd05e238efd9f0363a8748b0994fc044e39f1a3"
company_key: "yc-tractian"
company: "Tractian"
source_id: "yc-tractian-news-import-9393e6926c82"
canonical_url: "https://tractian.com/en/blog/7-things-to-prove-before-scaling-a-machine-monitoring-pilot"
published_at: null
first_seen_at: "2026-07-31T19:26:35.046657+00:00"
fetched_at: "2026-07-31T19:26:35.711553+00:00"
content_hash: "sha256:501f4d4621058e6e209e9d3e3392835f8dbc07b60904a9c4f8880b7723f2a18f"
---

# 7 Things to Prove Before Scaling a Machine Monitoring Pilot

## Key Points


- A machine monitoring pilot can easily prove detection works. However, detection itself rarely proves the things that decide whether a plant-wide rollout will be successful.
- Scaling stresses a program in specific ways due to more assets, higher alert volume, harder conditions, and far less expert attention per signal.
- The pilot worth trusting proves diagnostic clarity, prioritization under volume, decisions without a dedicated expert, adoption on the floor, data quality on hard assets, a closed loop to executed work, and a measured result against a baseline.
- Run the pilot on the assets that will make scaling hard, not the ones that make it look easy.


### **It seemed like enough proof**


The pilot went well. A handful of machines got sensors, the system flagged a real fault before it became a failure, and the save was concrete enough to put in a slide. It seemed like a sure thing, so the rollout was approved on the strength of it alone.


Then, across the plant, the program scaled, the rollout either stalled before it reached most assets or buried the team under alerts that no one had time to read.


Pilots with insufficient or misattributed proofs are common enough that a McKinsey survey found[nearly 70 percent of industrial IoT initiatives were stuck between pilot and scale](https://www.mckinsey.com/capabilities/operations/our-insights/operations-blog/unlock-value-with-an-industrial-iot-technology-stack-that-scales) , with only about 15 percent reaching company-wide use within a year. But the technology usually is not the problem. The pilot is.


The issue is that a pilot is often built to succeed under favorable, fully attentive conditions. And the things that make it succeed, like close attention, easy assets, and a short window, are exactly the things scale takes away.


For a[reliability engineer](https://tractian.com/en/who-we-serve/reliability-engineer) weighing a rollout, that makes the pilot's real job clear. It is not to prove that machine monitoring can catch a fault. It is to prove the specific things scaling will test. This article lays out what those things are, and how to run a pilot that actually proves them.


## Why a Pilot Built to Succeed Can Prove the Wrong Things


***A pilot runs under the best possible conditions, which means it can pass without ever testing what a rollout will break.***


A pilot is designed to go well. It runs on the most accessible and best instrumented assets, the ones a team already understands. It gets vendor attention, a close eye from an engineer, and a short window where everyone is paying attention. Those conditions are reasonable for a first look. They are also **the opposite of the conditions the program will live in once it scales** .


That gap matters because[machine condition monitoring](https://tractian.com/en/glossary/machine-condition-monitoring) is not one thing. Condition monitoring is a set of techniques for checking the health of assets, their vibration, temperature, ultrasound, among other signals, taken on a schedule or a route. Yet, machine monitoring is the larger picture. You’re looking at continuous signals across many assets, interpreted automatically, so the health of the plant is known rather than sampled. A pilot can prove the techniques work on a few machines. Scaling asks whether the plant can be known continuously, at volume, without a specialist standing over every reading.


### **An example consideration for evaluating a planned pilot**


As the pilot is being planned, you see that the assets chosen for it are the plant's best assets. That means that if it proves the program, it does so on the machines least likely to expose your program’s limits. However, this is counter to the criterion any engineer on the floor would accept, which is that a pilot should prove that a rollout will de-risk operations.


So the useful question to evaluate the pilot would not be whether it found a previously undetectable fault on assets that are easier to work with. Your evaluation should ask which assets it will run on, and whether the hard ones were deliberately left out, as these could produce a significantly different result.


## What a Machine Monitoring Pilot Should Prove Before Scaling


***A pilot earns a rollout by proving it holds up under the pressures scaling adds, not by catching a single fault.***


Scaling stresses a monitoring program in very specific ways. These are things like more assets, more alerts, more variety, and less attention per signal. A pilot earns the decision to scale when it proves it can hold up under each of those stresses. And you can see that these are not just about detecting a problem on the best set of assets on a good day where everyone’s paying attention.


The following are the things worth proving before the rollout, framed as the checks a **reliability engineer** might bring to the review.


### **Diagnostic clarity, not just detection**


Detection tells you something changed. Its diagnosis tells you what changed, how serious it is, and what to do about it. The difference is small on one machine and enormous across hundreds.


A pilot that produces[anomaly detection](https://tractian.com/en/glossary/anomaly-detection) alerts still leaves a person to open each one, read the spectrum, and decide whether it matters. That person exists during a pilot. At scale, they do not, at least not in the numbers the alert volume would demand.


**The thing you’d want to prove for scale is that the system carries the interpretation, not just the flag.**


A useful yardstick is handheld parity, meaning the system reaches the conclusion a skilled analyst with a handheld instrument would have reached, with the same defensibility, so the reading holds up when someone asks why a machine was pulled. If the pilot still needed an expert to turn each alert into a decision, it proved detection, and detection is not the part scaling will struggle with.


### **Prioritization that survives alert volume**


On a dozen pilot assets, a team can chase every alert. But this is a small number and doesn’t represent a strategy. Across a plant, that same level of responsiveness (alert chasing) becomes noise, and the alerts that matter get lost among the ones that don’t.


This is where[criticality analysis](https://tractian.com/en/glossary/criticality-analysis) , the action of ranking assets by how much their failure would cost, becomes a filter the system needs to apply on its own.


A pilot rarely generates enough alerts to feel this. A program ready to scale has to sort by consequence, so the most critical assets surface first while the rest wait without being ignored. If the pilot never produced real volume, it never tested the sorting, and a flat alert list at scale trains a team to stop looking.


It’s worth checking the pilot record to see how many alerts came through in a week, and whether that number, multiplied by the real asset count, is something the team could actually act on.


### **Decisions that do not depend on a dedicated expert**


Many pilots succeed because someone skilled is watching, whether that be a reliability engineer who knows the assets, or a vendor analyst included for the trial. That is fine for a pilot, but it’s a problem for a rollout. If every good decision traced back to one expert's attention, the program proved a staffing model, not a monitoring capability. Is that staffing model what you’re looking to multiply across sites?


Labor scarcity is a real concern. Research projects that as many as[1.9 million skilled manufacturing jobs could go unfilled by 2033](https://themanufacturinginstitute.org/manufacturers-need-as-many-as-3-8-million-new-employees-by-2033/) . The deepest experience, the people who can read a spectrum through a quick manual review, is exactly what retires first. A program that leans on that expertise to function cannot scale faster than a plant can hire it. And that’s not even considering the additional cost of both the expert labor and the necessary corresponding human resources.


So the question is whether the pilot produced trusted decisions without a specialist in the loop for each one. If it did not, scaling means either hiring people who are not available or accepting decisions no one has time to stand behind.


### **Adoption and trust on the floor**


A pilot has believers. The people who set it up want it to work, and the crew treats it as a short experiment worth cooperating with. Scale removes both of these groups. The implemented system will land on technicians who did not choose it, who have watched tools come and go, and who will quietly route around anything they do not trust.


A monitoring program is only as good as the work it causes, and work generated from it only happens when the person holding the wrench trusts it.


The pattern related to scale to look for in a pilot is not the technology functioning. It is a skeptic changing their mind, the veteran technician who doubted the sensor until it called a failure they had missed, and who now checks it first. That conversion is what scales, because it spreads through trust rather than mandate. A pilot that won over the hardest audience on the floor proved something a clean dashboard cannot.


### **Data quality on the assets you did not choose**


Sensors on a pilot go where they work best, on accessible machines, at steady speed, with strong connectivity. Scaling does not have that luxury. The rollout reaches variable speed drives, machines that run intermittently, such as in hot, wet, or hazardous locations, and corners of the plant where wireless signal is an open question. Every one of those conditions can degrade a reading, and a degraded reading produces a confident wrong answer, which is worse than no answer.


So a pilot that ran only on easy assets proved the sensing on the population least likely to challenge it.[Industrial vibration analysis](https://tractian.com/en/glossary/industrial-vibration-analysis) on a well-mounted sensor at constant speed is close to a solved problem. The same analysis on a machine that changes speed constantly, or sits idle for hours, is not. The proof worth having is that the pilot deliberately included the awkward cases, so data quality was tested where scaling will actually strain it.


### **A closed loop to executed work**


The value of monitoring is realized only when an insight reaches the person who fixes things, inside the maintenance platform, and becomes a scheduled, completed job. Many pilots stop short of this. They prove the signal and leave the handoff to a spreadsheet or a verbal note, which works for ten alerts, but collapses in on itself for a thousand.


Before scaling, it is worth proving the whole path, from a detected problem to a diagnosis to a work order to a closed job, without a person retyping anything between steps. That loop is what turns monitoring into fewer failures, not more dashboards.


**Isn’t unplanned work the same as unplanned downtime?**


Let’s say you were called to fix a floorboard. Simple, right? But you get to the address only to find a multistory, 32-room building, not to mention all the between-room flooring and all the places that don’t count as ‘rooms.’ How would you feel about the person who called knowing you’ve got to go find that floorboard? You’d probably start working around them, trying to get residents to get in touch with you instead.


If the pilot ended at detection and left diagnosis or execution for later, the rollout will scale the detection and inherit the unplanned for work. Isn’t unplanned for work the same as unplanned downtime? You’ve just shoved the downtime to a different place.


**Machine Monitoring Pilot Closed Loop**


1


Signal detected


An anomaly appears on the asset


Where a detection-only pilot stops


The alert exists, but no work is diagnosed, ordered, or closed


2


Cause diagnosed


The failure mode and its urgency are identified


3


Work order created


The fix is prioritized and assigned to a crew


4


Job closed and verified


The repair is completed and checked against the baseline


The loop closes and the verified result resets the baseline for the next cycle


### **A baseline you can measure against**


A pilot that cannot say what operationally changed cannot justify a rollout. The most common weakness is not a technical one. Rather, it is the absence of a baseline, a clear picture of downtime,[reactive maintenance](https://tractian.com/en/glossary/reactive-maintenance) hours, and unplanned failures before the monitoring went in. Without that starting point, a dramatic catch is a story rather than evidence, and a scale decision built on a story tends to wobble the first time someone in finance asks what the program actually returned.


To get the proof you need to set the baseline at the start, measure the same things at the end, and attribute the difference honestly. This includes the saves that did not happen and the interventions that turned out to be unnecessary. A pilot with a measured result gives the[plant manager](https://tractian.com/en/who-we-serve/plant-manager) a number to defend. A pilot without one gives them a story that may prove to be fiction.


## Running the Pilot So it Proves the Right Things


***A pilot proves the right things when it is scoped and measured on purpose, not just that it ran and detection occurred.***


The seven items we’ve pointed out don’t require a longer pilot so much as a one that deliberately plans to prove ‘scale.’


Scope selection sets what the pilot can prove. Choosing only easy assets guarantees an easy result and a fragile rollout, so the scope should include some of the machines that will make scaling hard. A measured baseline exists so the outcome can be defended later, not just described. A run window long enough to generate real alert volume exists so prioritization and diagnosis are tested under load rather than assumed. And the evaluation itself should be set against the proof standard above, not against a single memorable catch. A downtime save from failure only shows the sensor can work. But the criteria show the program can scale.


**Run the pilot on the machines that will make scaling hard, not the ones that make it look easy.**


Run this way, a pilot answers the question the rollout will ask. Run as a demonstration, it answers a question no one at scale is asking, which is whether the technology can succeed under the best possible conditions.


### **Machine Monitoring Pilot Checklist**


What the pilot must prove The pressure it must survive at scale


Diagnostic clarity, not just detection


No analyst is available to interpret every alert


Prioritization that survives volume


A flat alert list turns real signals into noise


Decisions without a dedicated expert


Deep reliability expertise is scarce and retiring


Adoption and trust on the floor


The rollout reaches crews who did not choose it


Data quality on the hard assets


Variable speed, intermittent, and dead-zone machines


A closed loop to executed work


A detected fault must become a completed job


A measured result against a baseline


A demo cannot justify a plant-wide investment


## The Questions to Answer Before You Scale


***Before scaling, the pilot should give clear answers to a short list of questions the rollout will otherwise ask the expensive way.***


Before a rollout, the pilot should have clear answers to a short list of questions that the **reliability engineer** and the **plant manager** should agree on.


- Did the system produce decisions, or alerts that still needed an expert to interpret?
- Did it run on the hard assets, or only the convenient ones?
- Did it generate enough volume to prove it sorts by what matters?
- Did it change the mind of someone on the floor who did not want it?
- Did it move a real number against a real baseline?


Where the answer is yes, scaling extends something proven. Where the answer is no, scaling extends a question. Waiting for the rollout to answer that question will be very expensive.


## Tractian is Built to Scale


***Tractian is built so a pilot can test the scale standard from day one, across sensing, diagnosis, and executed work.***


Tractian believes that a monitoring program should produce decisions that hold up at scale rather than alerts that need a specialist. It works as a unified path from sensing to diagnosis to executed work.


Tractian sensors are made for the assets a pilot usually avoids. The[Smart Trac wireless sensor](https://tractian.com/en/solutions/condition-monitoring/ultrasonic-sensor) captures vibration and ultrasound together, along with temperature and magnetic field, so a reading holds up on variable-speed and[intermittent machines](https://www.youtube.com/watch?v=ejSj4LaVnWY&ref=ghost.tractian.com) and in the harsh locations where scaling tends to strain data quality.


On top of that sensing sits the layer that matters most for scale.[Insight and diagnosis](https://tractian.com/en/solutions/condition-monitoring/insights-and-diagnosis) that names the likely fault, its severity, and the recommended action, rather than leaving an anomaly for a person to interpret. Alerts are ranked by asset criticality, so the signals that matter surface first instead of arriving as a flat list. That is the difference between detection and[continuous condition monitoring](https://tractian.com/en/solutions/condition-monitoring) a plant can act on at volume.


The additional reach is what makes the loop whole. A diagnosis flows into any Tractian-enriched[CMMS](https://tractian.com/en/solutions/cmms) for[predictive analytics](https://tractian.com/en/solutions/apm/predictive-maintenance-software) and execution, either natively or through API, SQL, or open integrations into whichever maintenance platform a plant already runs, so a detected problem becomes a closed job. And when a unified workflow also reads the[electrical side](https://tractian.com/en/solutions/oee/electrical-monitoring) of an asset, mechanical and electrical signals can be correlated on one timeline, which is the strongest single reason a diagnosis arrives with confidence rather than ambiguity.


**Learn more about Tractian's machine monitoring and diagnosis** to see how high-quality, decision-grade IoT data transforms your program into AI-powered closed-loop workflows.


## FAQs about Machine Monitoring Pilots


### **How long should a machine monitoring pilot run?**


Long enough to generate real alert volume and to cover a full operating cycle of the assets involved, which usually means a few months rather than a few weeks. The aim is enough data to test prioritization and diagnosis under load.


### **How many assets should a pilot include?**


Enough to include some of the difficult ones. A pilot limited to easy, accessible machines proves the technology on the population least likely to expose its limits at scale.


### **What is the difference between a pilot that detects and one that is ready to scale?**


A detection pilot shows the system can flag a problem. A scale-ready pilot shows it can produce a trusted, prioritized decision without a specialist interpreting every alert.


### **Do you need a vibration analyst on staff to scale monitoring?**


Not if the system carries the diagnosis rather than only the signal. That is why diagnostic clarity is worth proving in the pilot, so decisions do not depend on expertise you would have to hire for every site.


### **How do you measure whether a pilot succeeded?**


Against a baseline set before it started, comparing downtime, unplanned failures, and reactive hours before and after, rather than against a single memorable catch.
