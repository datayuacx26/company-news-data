---
schema_version: "1.0.0"
document_id: "c02adeab4c484b795a51aa431cfbe2ad0625f94eb3cd9523c80fb0bf4b97ce09"
company_key: "yc-circuitlab"
company: "CircuitLab"
source_id: "yc-circuitlab-rss-d25759254652"
canonical_url: "https://www.circuitlab.com/blog/2020/08/13/voltage-and-current-step-sources-in-circuitlab/"
published_at: "2020-08-13T16:30:00+00:00"
first_seen_at: "2026-07-24T22:19:16.775475+00:00"
fetched_at: "2026-07-28T22:26:40.232877+00:00"
content_hash: "sha256:06948d970ddfbbc7a6c9339d043c79c8f95d9b6308831bc11431af4156fc1639"
---

# Voltage and Current Step Sources in CircuitLab

## [Voltage and Current Step Sources in CircuitLab](https://www.circuitlab.com/blog/2020/08/13/voltage-and-current-step-sources-in-circuitlab/)


#### Aug 13 2020, 9:30 AM PDT ·[0 comments »](https://www.circuitlab.com/blog/2020/08/13/voltage-and-current-step-sources-in-circuitlab/#comments)


We’ve just made it much easier to simulate a step response in CircuitLab. Finding a circuit’s unit step response in the time domain is one of the most common operations for anyone learning about or designing analog filters and amplifiers.


It’s always been possible to simulate step responses in CircuitLab using a combination of a voltage source and a[switch](https://ultimateelectronicsbook.com/switches/) , but we’ve now bundled this behavior into a signal component: the **Voltage Step Source** and the corresponding **Current Step Source** .


Here’s a simple example comparing voltage step responses between a series RC circuit and a parallel RL circuit:


Click to open and simulate the circuit above. Can you predict the shape of V(RC) and V(RL) before you run the simulation?


Here’s a more advanced example showing how an op-amp non-inverting amplifier can exhibit stability problems, including ringing and overshoot, even with small amounts of parasitic (undesired) capacitance at the feedback node:


Click to open and simulate the circuit above. How long does it take for the output to settle down after the input step? Is there a capacitance level beyond which this amplifier is basically useless?


**Note that the step only happens in a transient simulation.** A DC simulation (or DC Sweep) always happens *before* the step. This lets the simulator find the *old, pre-step* steady-state operating point first, before it models the transient change to a[new post-step steady-state](https://ultimateelectronicsbook.com/steady-state-transient/) .


These new step source components are both available in the “Voltage Signal Sources” and “Current Signal Sources” sections of the CircuitLab toolbox, right between the configurable function generator source and the CSV input source:


After inserting a step source, you can double-click it to configure its amplitude and provide a delay. Conveniently, by default, a step source provides a unit step at t=0.


---


### Comments


No comments yet. Be the first!


#### Leave a Comment


Please[sign in](https://www.circuitlab.com/accounts/login/?next=/blog/2020/08/13/voltage-and-current-step-sources-in-circuitlab/%23comment_form) or[create an account](https://www.circuitlab.com/accounts/register/) to comment.


### About CircuitLab


CircuitLab is an in-browser schematic capture and circuit simulation software tool to help you rapidly design and analyze analog and digital electronics systems.


- [CircuitLab Home](https://www.circuitlab.com/)
- [Example Circuits](https://www.circuitlab.com/user/CircuitLab/)
- [Blog](https://www.circuitlab.com/blog/)
- [Forums](https://www.circuitlab.com/forums/)
- [About Us](https://www.circuitlab.com/about/)
- [Frequently Asked Questions](https://www.circuitlab.com/docs/faq/)
- [Documentation](https://www.circuitlab.com/docs/)
- [Electronics Q&A](https://www.circuitlab.com/questions/)
- [Electronics Textbook](https://ultimateelectronicsbook.com/)


New @ CircuitLab


[salvimkelvot234 answered: "CircuitLab models of crystals"](https://www.circuitlab.com/questions/5b2b4psb/circuitlab-models-of-crystals/#answer_uuhyehg3)


[alopulid322 answered: "Bandwidth of RLC circuit"](https://www.circuitlab.com/questions/a7rb52j9/bandwidth-of-rlc-circuit/#answer_psh8u6q9)


[dorianmc saved circuit: "555 Charge Detector"](https://www.circuitlab.com/circuit/k2y67g7qtz76/555-charge-detector/)


[hairforce1 saved circuit: "Battery Supply"](https://www.circuitlab.com/circuit/f689yfbmmtfa/battery-supply/)


[kelvin.stott asked: "Simplest Red-Green LED control..."](https://www.circuitlab.com/questions/upwxg2y6/simplest-red-green-led-control-circuit/)


[alopulid322 commented on question: "Clock Chime Circuit"](https://www.circuitlab.com/comments/cr/53/z7ptf5au/#c10744)


[CASparks saved circuit: "BootstrapBoost"](https://www.circuitlab.com/circuit/y3gbg3449brw/bootstrapboost/)


[tejpani saved circuit: "VFD Schematic"](https://www.circuitlab.com/circuit/utruwrht37ne/vfd-schematic/)


[RDHam commented on question: "CircuitLab models of crystals"](https://www.circuitlab.com/comments/cr/53/5b2b4psb/#c10741)


[oftdiscreet commented on question: "CircuitLab models of crystals"](https://www.circuitlab.com/comments/cr/53/5b2b4psb/#c10740)


[FricktalerAusblick answered: "Please review my circuit for proper..."](https://www.circuitlab.com/questions/9pcm7cd7/please-review-my-circuit-for-proper-operation-thx/#answer_89mz392d)


[schrifts answered: "Clock Chime Circuit"](https://www.circuitlab.com/questions/z7ptf5au/clock-chime-circuit/#answer_jw4h2f55)


[RDHam asked: "CircuitLab models of crystals"](https://www.circuitlab.com/questions/5b2b4psb/circuitlab-models-of-crystals/)


[Cadmium started discussion: "Group rotate"](https://www.circuitlab.com/forums/feature-requests/topic/nxb3v45j/group-rotate/#comment_10736)


[G3YTZ started discussion: "SR Flip Flop suggestion"](https://www.circuitlab.com/forums/support/topic/wc93ugsw/sr-flip-flop-suggestion/#comment_10733)


[JosGr asked: "Bandwidth of RLC circuit"](https://www.circuitlab.com/questions/a7rb52j9/bandwidth-of-rlc-circuit/)


[Marryjoseph commented on an answer to: "Clock Chime Circuit"](https://www.circuitlab.com/comments/cr/54/56wg48sc/#c10732)


[blazstyn asked: "Complete novice looking for help..."](https://www.circuitlab.com/questions/zc45v7w5/complete-novice-looking-for-help-with-bypassing-touch/)


[Dan.Ka replied to discussion: "Yet another "Unable to get..."](https://www.circuitlab.com/forums/support/topic/g48py5yz/yet-another-unable-to-get-solution-for-dc-solver/#comment_10720)


[ramprao started discussion: "arctan2 functionality"](https://www.circuitlab.com/forums/feature-requests/topic/ausz2mdq/arctan2-functionality/#comment_10714)


[CircuitLab Blog: "Live DC Simulation!"](https://www.circuitlab.com/blog/2021/01/27/live-dc-simulation/)


[CircuitLab Blog: "Digital Adders and Wider Muxes"](https://www.circuitlab.com/blog/2021/01/14/digital-adders-and-wider-muxes/)


---
