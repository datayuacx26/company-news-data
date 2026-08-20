---
schema_version: "1.0.0"
document_id: "72676ddbc7eda5e5a2933cee09cebe6ad482fc860f16d006a7ae7bccd3fd06e8"
company_key: "yc-upkeep"
company: "UpKeep"
source_id: "yc-upkeep-news-import-f157ad0fa20d"
canonical_url: "https://upkeep.com/blog/why-your-sensor-data-rarely-answers-the-question-you-actually-have/"
published_at: null
first_seen_at: "2026-08-05T02:28:53.243599+00:00"
fetched_at: "2026-08-05T03:48:27.623827+00:00"
content_hash: "sha256:b1c77c0bddc6cce53dd3d71ccba5b7898d4f753eaaf6bffd7e2d2b69c2a7981f"
---

# Why Your Sensor Data Rarely Answers the Question You Actually Have

# **Why Your Sensor Data Rarely Answers the Question You Actually Have**


The dashboard in the video counts asset runtime two ways at once, from motor current and from vibration, because somebody asked for the second one and had it working a few minutes later. Every plant has a version of that request. Somewhere on your floor is a machine where current is the wrong way to count hours, and somebody on your reliability team already knows which one it is.


Vibration sensors had been reporting all along and the database had been storing every reading. What stood in the way is that the request belongs to one plant, and a request that belongs to one plant is nobody's priority but yours.


Two conveyors on the same line show what the waiting costs. One moves product at rate, one turns empty because the upstream cell went down twenty minutes ago, and both draw current. A runtime dashboard reading amp draw logs the same hour against each of them, and every PM interval built on that number is wrong in the same way, with nothing to flag it.


Across plants running condition monitoring, that gap has surprisingly little to do with how many sensors are installed.


## **What happens when your dashboard can't answer a reasonable question**


The question leaves the system, either as an export somebody rebuilds by hand or as a question nobody asks twice, which leaves the team working off a number it already knows is rough.


Neither path is free, though they cost you differently. Build the math by hand and you now have a second version of the truth that no work order ever fires from, so the system of record and the real record slowly pull apart. Dropping the question does quieter damage, because nobody writes down a decision they made on a guess.


Survey data hints at how often the first path gets taken. Plant Engineering's annual maintenance study has repeatedly found in-house spreadsheets and schedules ranking alongside CMMS among the most-used tools for managing maintenance, with roughly half of plants reporting each (per Plant Engineering, 2018). Every one of those files marks a spot where the software stopped and somebody kept going.


## **Why standard reporting tools can’t answer plant-specific questions**


Most reporting tools only read, and that one limit explains most of the problem. They will show you whatever the sensors sent, but there is no way to feed them the extra numbers that turn a reading into something you can act on.


The missing pieces are usually simple:


-


**A cutoff that defines "running" for one specific machine.** Vibration above a set percentage means this one is working. The machine beside it needs a different percentage, set by whoever knows the equipment.


-


**A price.** Amp readings only become an energy cost after somebody supplies the rate per kilowatt hour, the voltage, and the power factor. None of that lives in a sensor.


-


**A rule that turns a signal into a job.** A dry contact opening or closing just means something switched. Whether that should pull work away from an asset depends on things the sensor has no way of knowing.


Look at what those three have in common. Every missing piece is something the plant already knows and no sensor can supply, which means buying more sensors does nothing to close the gap. Readings are raw material. Turning them into decisions takes what the people running the plant already have in their heads.


## **What the gap actually costs**


Over-maintenance costs show up first, mostly because they are the easiest to price. ARC Advisory Group has estimated that roughly 30% of preventive maintenance runs too often (per ARC Advisory Group's EAM market study), and when a PM interval runs off a runtime number that reads high, some of that extra work follows directly. A pump straining against a throttled valve racks up hours at the same rate as one moving full flow. The interval comes due, a tech opens a machine that was fine, and you pay in labor, in parts, and in the small chance that opening it up starts a new problem.


Energy is the cost nobody sees. Measuring it was never the hard part; splitting it up by machine is. Plants know their utility bill, and very few can say which asset on the floor is responsible for what share of it. Without that, you cannot build a replacement case for the compressor that quietly turned into the most expensive thing in the building.


Trust goes last and does the most damage. Once a reliability engineer proves that the system's runtime number is off by a wide margin, they stop using it for anything, and the whole program slides back toward calendar-based PMs. That slide undoes the exact thing the sensors were bought to fix.


## **Three requests that used to go nowhere**


**Runtime from two sensor types, side by side.** One dashboard showing runtime from current next to runtime from vibration on the same asset, with the "running" cutoff set separately for each sensor. Build time was about 15 minutes. With both numbers in one place, the comparison itself tells you which signal to trust on which kind of equipment.


**Cost per machine, per day.** Feed current readings a utility rate, a voltage, and a power factor and you get a running dollar figure per device. An internal test came out at $1.86 over 24 hours on a dehumidifier, which is a small number on a small machine. Run the same math on a chiller line or an air compressor and you have a capital request.


**Live equipment status that drives a decision.** Dry contact sensors on ground support equipment at airport gates, drawn as a red and green terminal map, so ground crews can send an inbound plane to a different gate instead of letting it burn fuel on its backup engine. A dry contact is about as simple as hardware gets. All the value came from putting what it knows somewhere an operator could act on it right away.


## **What changes when you can build it yourself**


The wait on a plant-specific question drops from a release cycle to an afternoon, and that changes what people bother to ask for.


UpKeep's Nova apps now read and write across all three core products, so one app can pull CMMS work orders, EHS safety events, and Edge sensor readings into a single view, along with data from connected third-party tools. Describe the calculation and you get the app. A team that wants runtime from vibration on its critical rotating equipment and from current everywhere else can set up both and keep them in the same system of record.


For your program, that means whether the software can do something stops depending on how many other customers asked for it first.


## **The list worth making**


Every maintenance team carries a list of questions it has stopped asking, and that list almost never gets written down anywhere. Nobody files a ticket for a report they assumed was impossible.


So ask your reliability engineers what they would measure if nothing was in the way. The answers are usually small, specific, and tied straight to money, which is about as good a description of useful work as an operations team is going to get.


---


**Learn more about Edge:**[https://upkeep.com/product/edge-iiot/](https://upkeep.com/product/edge-iiot/)
