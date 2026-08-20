---
schema_version: "1.0.0"
document_id: "51dd0bd95723f9168e8339248642b4cbad137ca527e7513c7f04ee2523dffb81"
company_key: "emerson-electric-company-common-stock"
company: "Emerson Electric Company"
source_id: "emerson-electric-company-common-stock-rss-55c4887b1417"
canonical_url: "https://www.emersonautomationexperts.com/2026/services-consulting-training/dead-time-compensation-what-every-control-engineer-should-know/"
published_at: "2026-06-25T18:00:10+00:00"
first_seen_at: "2026-07-25T01:09:56.979097+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:4b362758caf06da61445e7e2eeec6a5c916bfe506fc7fbc0e012d84f5bb3522c"
---

# Dead Time Compensation: What Every Control Engineer Should Know Before Turning It On

Process automation hall-of-fame member[Greg McMillan](https://www.linkedin.com/in/greg-mcmillan-5b256514/) walks through the practical realities of dead time compensation in proportional-integral-derivative (PID) control loops in this recorded webinar **,[PID Deadtime Compensation](https://www.youtube.com/watch?v=uFdGmWJTfxU)** . Greg methodically dismantles several long-held assumptions and shows engineers exactly when and how dead time compensation delivers results.


### Why It Matters


Dead time is the single most limiting factor in any control loop. It is the total elapsed time from the start of a change until it completes a trip around the entire loop and returns to the point of origin. Until a controller detects a disturbance and its corrective action reaches the same process location, nothing improves. Every engineer who tunes a loop, bumps into this wall. Dead time compensators promise a way around it, but Greg’s demos reveal that the benefit is not automatic and that the compensator’s behavior defies several conventional tuning instincts.


### Key Takeaways


- **Dead time is not eliminated.** A compensator does not remove dead time from the loop. The fundamental limit on disturbance rejection remains set by the total loop dead time.
- **Adding a compensator alone makes things slower, not faster.** Without retuning, performance degrades. The compensator only pays off when the controller gain is increased, in some cases by 250%.
- **The biggest gains come where you least expect them.** Loops where the process lag is larger than the process delay benefit more from dead time compensation than loops dominated by pure dead time.
- **Underestimating dead time degrades gracefully.** Performance slides back toward what you would get without compensation. Overestimating dead time creates a distinctive jagged response that worsens with larger errors.
- **For dead time dominant loops, both gain and reset time must change.** Failing to reduce reset time results in severe undershoot.


### Most of Your Dead Time Is Under Your Control


Greg makes a point that should reshape how engineers prioritize loop improvement. The total loop dead time comes from two broad categories: process dead time (piping, ducts, reactors, conveyors, extruders, and other transport delays set by mechanical design) and automation dead time (digital device update and execution times, analyzer sample and cycle times, signal filtering, transmitter damping, and valve response). For a majority of loops, particularly analyzer, flow, pressure, level, and speed loops, the automation-related delays are the dominant contributor. That means the automation engineer, not the process designer, holds the lever for the largest share of the total delay.


### The Smith Predictor vs. the External Reset Approach


Greg traces dead time compensation back to the Smith predictor, introduced in 1957. The Smith predictor creates an internal model of the process, with and without dead time, effectively presenting the controller with a simulated process variable with no delay. It works in theory, but it requires the user to set the process gain, process time constant, and process dead time correctly. The PID sees a modeled process variable rather than the real one, which means operations staff need a special faceplate or a separate tracking controller just to monitor the actual measurement.


The external reset dead time compensator takes a simpler path. A single dead time block is inserted into the PID’s external reset feedback path. The user only needs to set one parameter: the estimated dead time. The PID continues to see the actual process variable; no special faceplate is required, and Greg’s testing shows this approach is less sensitive to model errors than the Smith predictor. It does require the positive-feedback implementation of integral action (as used in[DeltaV](https://www.emerson.com/en/automation-systems/distributed-control-systems-dcs/deltav-distributed-control-system) ) and the dynamic reset limit option to be enabled.


### What the Demos Showed


In the first demo, Greg configures a loop where the process time constant (ten seconds) is much larger than the process delay (roughly three to four seconds total). With conventional PID tuning, the rise time to the setpoint is about 23.5 seconds. Turning on dead time compensation without retuning nearly doubles that rise time to over 40 seconds. Only after increasing the controller gain by 250% does the compensator deliver: rise time drops to about ten seconds with negligible overshoot. Attempting to also decrease the reset time for this type of loop does not help and actually worsens the response.


In the second demo, the process is reconfigured so the pure delay (nine seconds) dominates the process lag (one second each for primary and secondary). Here, increasing the gain alone is not enough. The reset time must also be substantially decreased. Greg shows that cutting reset time from five seconds down to half a second progressively smooths out undershoot and produces a tighter response. However, the dead time dominant loop is far more sensitive to errors in the dead time estimate. An overestimate of less than ten percent is enough to trigger a visible jagged oscillation, whereas the lag-dominant loop tolerated overestimates of 100% or more before significant problems appeared.


### Practical Guidance for Dead Time Compensation


Greg’s closing summary reinforces a clear hierarchy: for loops where the process lag exceeds the process delay, increase the PID gain and leave the reset time alone. For loops dominated by process delay, increase the gain and decrease the reset time, with larger decreases as the delay-to-lag ratio grows. In all cases, round the dead time estimate upward rather than downward, because an underestimate simply degrades performance back toward conventional PID behavior, while an overestimate introduces the harder-to-manage jagged response.


For more insights from Greg on process control, generously shared from decades of hands-on experience,[explore his other posts here on the blog](https://www.emersonautomationexperts.com/author/greg-mcmillan/) .


**Note** : The website showing the demonstration is no longer available.
