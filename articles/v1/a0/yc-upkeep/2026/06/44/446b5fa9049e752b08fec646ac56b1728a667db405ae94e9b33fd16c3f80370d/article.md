---
schema_version: "1.0.0"
document_id: "446b5fa9049e752b08fec646ac56b1728a667db405ae94e9b33fd16c3f80370d"
company_key: "yc-upkeep"
company: "UpKeep"
source_id: "yc-upkeep-news-import-f157ad0fa20d"
canonical_url: "https://upkeep.com/blog/corrective-action-request-template/"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-26T03:52:48.498440+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:ca9abf09f8cec86be8824eddeaa283718ab188bc6bf68d60b1a877ddb4cb7865"
---

# Corrective Action Request Template: Fields, Process, and How to Use It

## **Key Takeaways**


-


A corrective action request template gives a failure event a documented starting point. It uses evidence rather than memory to pinpoint the root cause.


-


Generic CAR templates aren't built for maintenance operations. Standard forms don't have fields for asset ID, failure mode, work order reference, or PM history, which is where the root cause lives.


-


The verification of effectiveness (VoE) check is the field most commonly dropped, and its absence is expensive. A CAR that closes on paper without confirming the fix held hasn't closed.


-


With UpKeep, a safety event generates a CAPA record automatically, and the intake fields populate the asset, location, timestamp, and description.


When a piece of equipment fails and there’s no standard intake document to capture it, the investigation starts from memory. Someone fills out what they recall or can piece together from[work order](https://upkeep.com/blog/work-order-process/) notes and a conversation in the break room. The root cause analysis runs on incomplete evidence, the corrective action addresses whatever the team can identify, and six months later, the same asset fails the same way again.


That's the operational cost of an undocumented nonconformance. Generic corrective action request templates aren’t enough here. They were built for ISO 9001 quality management systems, so they capture incident descriptions and sign-offs, not asset IDs, failure modes, or work order references. The maintenance-specific version below is built to feed a CAPA process, not just close out a record.


## **What Is a Corrective Action Request?**


A corrective action request (CAR) is a document that **formally opens an investigation into a nonconformance, defect, or equipment failure.** It captures the evidence at the point of discovery, including asset information, failure description, and initial containment. That then kicks off the process of finding and eliminating the root cause.


The CAR is the intake document.[CAPA (corrective and preventive action)](https://upkeep.com/blog/corrective-and-preventive-action/) is the broader system the CAR feeds into. Opening a CAR without a CAPA system behind it is like writing up a defect and then stuffing it in a drawer to be forgotten. The form is only as useful as the process that acts on it.


A SCAR, or supplier corrective action request, follows the same structural logic as an internal CAR but routes to a vendor rather than an internal team. It’s used when the nonconformance traces back to a supplier part or process.


That distinction is important for[regulatory compliance](https://upkeep.com/es/blog/cmms-compliance/) . For maintenance teams in regulated environments, such as[ISO 9001-certified operations](https://www.iso-9001-checklist.co.uk/10.2-corrective-action.htm) , medical device manufacturers subject to[FDA QMSR](https://www.fda.gov/medical-devices/postmarket-requirements-devices/quality-management-system-regulation-qmsr) /[21 CFR Part 820](https://www.ecfr.gov/current/title-21/chapter-I/subchapter-H/part-820) , or food facilities subject to preventive controls or similar frameworks, **a CAR is a documented log of identified, investigated, and resolved issues** . Regulators and auditors don't just want to see that you fixed the problem; they want to see exactly how you got there.


## **What a Standard CAR Template Gets Wrong for Maintenance Teams**


A regular CAR form typically captures the incident description, corrective action taken, and sign-off. In a quality management context, that’s usually sufficient. But for a maintenance team investigating an equipment failure, it's not enough.


Here’s what’s missing:


-


**Asset ID:** The CAR isn’t tied to the machine’s history.


-


**Criticality Classification:** There’s no way to calibrate how fast the investigation needs to move.


-


**Failure Mode:** Two investigators looking at the same breakdown might document it completely differently. One may write “motor failure” while the other may jot it down as “overheating.” Neither will clearly point to a corrective action.


-


**Work Order Reference Field:** The CAR is disconnected from the repair history sitting in your[CMMS](https://upkeep.com/blog/what-is-cmms/) (computerized maintenance management system).


-


**PM History:** An investigation could start without knowing the asset’s been on a skipped maintenance cycle for six months.


-


**Verification of Effectiveness:** This is usually either absent or optional, so the CAR can close without anyone confirming the fix worked.


The investigation is only as good as the evidence it starts with. If the form doesn't collect the right fields at intake, your team will have to reconstruct that data later, under time pressure, from memory.


## **Corrective Action Request Template: Fields for Maintenance Teams**


Below is a maintenance-specific CAR template to outline what a solid form looks like. Each field has a specific job in the investigation, so you shouldn’t skip any.


**Field**


**Purpose/What to Capture**


CAR Number


Unique identifier linking the CAR to the asset record and any work orders opened as part of the investigation.


Date Issued/Due Date


Opens the accountability window. Due dates scale with severity; critical failures have tighter timelines than low-priority deviations.


Asset ID and Criticality


Maintenance-specific. Criticality classification determines the depth of investigation and the required response timeline. Without it, every failure looks the same.


Failure Mode


How the asset failed, not just what broke. Bearing failure, lubrication failure, and contamination failure each point to different corrective actions. The mode is where the investigation starts.


Work Order Reference


Links the CAR to the triggering work order and the asset's full maintenance history. Root cause evidence lives here.


Immediate Containment Action


What stopped the problem from spreading before the investigation started? Containment isn't the corrective action. Document it separately so reviewers don't mistake it for root cause resolution.


Root Cause Analysis


Method used (e.g., 5 Whys, fishbone) and findings. Must reach the system-level condition, not the visible failure or the proximate trigger.


Corrective Action Plan


Actions, owners, and deadlines. Each action must address the root cause along with restoring function.


Verification of Effectiveness


How and when effectiveness will be confirmed. Define the criterion before the CAR closes. For equipment failures, two PM cycles without recurrence is a practical standard.


Closure


Status, closed-by, date. Completed only after the VoE check confirms the fix held.


[Downloadable Template](https://docs.google.com/document/d/1mQT8ke7e0B4n59CIwtlZpQbHQDegWit8SPVzXkxajo0/edit?usp=sharing)


VoE is one of the easiest fields to skip when teams manage CARs manually. Without it though, a CAR closes when the action plan is documented, rather than when the problem stops recurring. A program can show high closure and recurrence rates at the same time. Define the effectiveness criterion before the CAR closes, while the failure is still fresh.


## **How to Complete a Corrective Action Request**


The form structure only works if it's filled in correctly. Following these steps gives the investigation the best chance of reaching the actual root cause.


1.


**Document the nonconformance with enough specificity to support investigation.** “Equipment malfunction” doesn't support a root cause analysis. “[HVAC](https://upkeep.com/learning/hvac-preventive-maintenance/) unit in Building C ran continuously for 72 hours without cycling off, triggering a thermal fault on 14 May” does. The level of detail in the description directly shapes the quality of the investigation.


2.


**Assign ownership before the investigation starts.** A CAR that opens without a named owner automatically has an accountability gap. Someone needs to be responsible for moving it forward from the starting line.


3.


**Run containment first, then root cause analysis.** Isolate the failure and prevent it from spreading, then begin the investigation. These are separate actions, so they belong in separate fields. Combining them obscures whether the containment actually held and makes the record harder to review.


4.


**Match the root cause method to the complexity of the failure.** The 5 Whys approach handles most equipment failures. It's fast, structured, and forces the team past the visible symptom. Fishbone analysis is warranted when multiple contributing factors are at play across people, processes, equipment, and the environment.


5.


**Define the verification criterion before writing the corrective action plan.** If the team can't say how effectiveness will be confirmed before the plan is finalized, the plan isn't finished.


6.


**Set a follow-up date and assign it to a specific owner.** Without enforcement, the check-in gets dropped, and the CAR closes as “incomplete.” This is the most common failure point in CAR programs that aren't tracked in a digital system.


## **Corrective Action Request Examples**


To illustrate the importance of a thorough CAR form, we’ve outlined a few industry-specific examples of a well-crafted template in action.


### **Example 1: HVAC Failure in a Commercial Facility**


A rooftop HVAC unit runs continuously without cycling off, and one section of the office has been warm for three days.[ASHRAE Standard 180-2018](https://www.ashrae.org/File%20Library/Technical%20Resources/Bookstore/previews_2016639_pre.pdf) provides minimum inspection and maintenance requirements for commercial HVAC inspection and maintenance. If the facility uses this standard as its maintenance benchmark, the CAR should show whether the inspection schedule contributed to the failure.


A CAR that records a failure mode as “HVAC malfunction” and closes with a unit replacement leaves the schedule unexamined. A CAR that records failure mode as “thermostat sensor failure causing continuous operation” traces back through 5 Whys to a PM plan that covers filter changes and coil cleaning but never included sensor calibration. The corrective action revises the PM schedule. The unit stays.


### **Example 2: Recurring Seal Failure on a Production Line**


At a food processing facility, a pump seal in a sanitation-critical zone fails for the third time in four months on the same unit. Under[21 CFR §117.150](https://www.ecfr.gov/current/title-21/chapter-I/subchapter-B/part-117/subpart-C/section-117.150) , the corrective action must be documented and should show how the problem was identified, corrected, and made less likely to recur.


A useful internal benchmark is two PM cycles without recurrence, although the right window should depend on asset criticality and failure frequency.


The first two CARs documented failure mode as “seal wear” and closed with the seals being replaced. The third pulls in the work order history, runs fishbone analysis, and finds a partially blocked downstream valve creating backpressure that accelerates seal wear. Replacing the seal a fourth time would have closed the CAR and missed the root cause entirely.


## **KPIs for Managing Your CAR Process**


Track these five metrics to see if your CAR program is actually eliminating failures or just closing records.


**KPI**


**What It Measures**


**Warning Signal**


CAR Cycle Time


Average days from CAR opened to CAR closed.


Extended timelines point to bottlenecks in root cause investigation or action plan approval.


Recurrence Rate


% of closed CARs where the same failure mode reappears within a defined window.


High recurrence means investigations are stopping at the symptom level.


VoE Completion Rate


% of closed CARs with a documented verification of effectiveness on file.


Anything below 100% requires structural control in the closure workflow.


Open CAR Backlog Age


Number of open CARs past their target closure date.


Aging CARs signal a program that can't close actions within its own timelines.


PM Compliance Rate


% of scheduled PMs completed on time (leading indicator).


PM gaps precede CAR volume increases by weeks to months. The corrective action queue absorbs the downstream work.


PM compliance rate is a leading indicator here. When PM completion drops, the downstream effect is more equipment failures, and more equipment failures mean more CARs. The corrective action queue is often where deferred maintenance shows up, weeks or months after the PM gaps that caused it.


## **How to Manage Corrective Action Requests in UpKeep**


For most teams, the CAR and the asset record live in different places. The form is a piece of paper or a generic digital template, and the maintenance history sits in the CMMS. Investigation work has to pull from both systems at the exact moment the team is also handling containment. The PM history review is what’s usually dropped, even though it's where most root causes are visible.


[UpKeep Safety’s CAPA workflow](https://help.onupkeep.com/en/articles/11647616-creating-capas-in-upkeep-safety) helps teams document corrective and preventive actions directly inside a structured safety process. A CAPA can be linked to an[incident report](https://upkeep.com/blog/safety-incident-report-template/) , assigned to an owner, tied to a location or asset, prioritized by urgency, and supported with RCA details, attachments, tags, and due dates.


The RCA section supports methods such as 5 Whys, fishbone diagrams, and fault tree analysis, while the action section separates immediate corrective steps from longer-term preventive measures. The result is a clearer record of what happened, who owns the response, and what needs to change before the issue is considered resolved.


## **A High-Functioning CAR Program Is Non-Negotiable**


Without a structured intake process, maintenance failure investigations inherit whatever evidence happens to survive the problem, whether that’s a partial work order note, a recollection, or a description captured after the fact. Root cause analysis then tends to stop at whatever visible failure is in front of the team. The corrective action addresses that symptom, and the asset fails again in the same way a few months later.


A maintenance-specific template combined with a digital system that enforces closure breaks the cycle. The CAR opens with the asset record already attached. Root cause analysis runs on full maintenance history. The VoE check is enforced before the record can close, which means a closed CAR actually represents a resolved failure rather than a documented one.


To see how a proper CAPA workflow can smooth out the creases in your processes,[start a free trial or request a demo](https://upkeep.com/free-trial-signup/?referring_element=navbar) with UpKeep today.


## **FAQ**


### **What's the difference between a corrective action request and a corrective action plan?**


A CAR is the intake document. It opens the investigation and captures the evidence at the point of discovery. A corrective action plan is the output of that investigation: the specific actions, owners, and deadlines that address the root cause. You can't write a good corrective action plan without a well-documented CAR to base it on.


### **What's a SCAR, and when does it apply?**


A supplier corrective action request (SCAR) follows the same structure as an internal CAR but routes to a vendor rather than an internal team. It’s used when the nonconformance traces back to a supplied part, material, or service. The aim is to get the supplier to investigate and correct the root cause on their end, not just replace the defective item.


### **How many fields does a CAR template need?**


It needs enough to support a root cause investigation, and no more. For maintenance teams, that means at minimum CAR number, asset ID and criticality, failure mode, work order reference, containment action, root cause analysis, corrective action plan, verification of effectiveness, and closure status. Generic templates that skip asset ID and failure mode will produce investigations with missing evidence.


### **What happens if you close a CAR without a verification check?**


The CAR closes on paper but not in practice. If the fix didn't hold, there's no mechanism to catch it. The recurrence shows up as a new failure event, another work order, possibly another CAR. Over time, a program that closes CARs without VoE checks will show a high recurrence rate on the same failure modes and an inflated closure count that doesn't reflect actual problem resolution.


### **How does a CMMS support the corrective action request process?**


A CMMS connects the CAR to the asset's maintenance history, which is where much of the root cause evidence lives. When a CAR opens inside the CMMS workflow, the investigating team can pull PM compliance history, prior work orders, and repair records without switching systems. Work orders can open directly from the CAR, and the investigation timeline is documented alongside the asset record.


### **Does a corrective action request template need to follow ISO 9001?**


No. ISO 9001:2015 requires organizations to take action on nonconformances and retain documented evidence of the investigation and corrective action taken, but it doesn't prescribe a specific template format. The requirement is that the documentation be sufficient to demonstrate the process was followed. For maintenance teams, a template built around maintenance-specific fields (asset ID, failure mode, work order reference) will satisfy the documented evidence requirement more reliably than a generic form that doesn't capture asset context.
