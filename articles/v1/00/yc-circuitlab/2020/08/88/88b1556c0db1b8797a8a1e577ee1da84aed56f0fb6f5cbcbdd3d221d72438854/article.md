---
schema_version: "1.0.0"
document_id: "88b1556c0db1b8797a8a1e577ee1da84aed56f0fb6f5cbcbdd3d221d72438854"
company_key: "yc-circuitlab"
company: "CircuitLab"
source_id: "yc-circuitlab-rss-d25759254652"
canonical_url: "https://www.circuitlab.com/blog/2020/08/10/ideal-diodes-in-circuitlab/"
published_at: "2020-08-10T16:30:00+00:00"
first_seen_at: "2026-07-24T22:19:16.775475+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:cc6240345751e5d36d5d319804f5810cee0481c97ea15a00fb9d94d06f8f80e6"
---

# Ideal Diodes in CircuitLab

## [Ideal Diodes in CircuitLab](https://www.circuitlab.com/blog/2020/08/10/ideal-diodes-in-circuitlab/)


#### Aug 10 2020, 9:30 AM PDT ·[0 comments »](https://www.circuitlab.com/blog/2020/08/10/ideal-diodes-in-circuitlab/#comments)


We’re introducing a new component to the CircuitLab toolbox: the **ideal diode** .


We’ve had semiconductor PN junction diodes since we’ve launched, which show the exponential current-voltage relationship and accurately model real-world diodes.


In contrast, the ideal diode is more like a simulated on-off switch: the I-V curve would be piecewise linear. It acts like it’s open-circuit when reverse biased, and short-circuit when forward biased. You can simply drag the ideal diode from the toolbox into your circuit, and optionally double-click to configure its parameters.


Here’s an example using four ideal diodes to build a full-wave rectifier:


Click to open and simulate the circuit above. Observe how the 4 diodes turn on and off at different times in the AC cycle.


Here’s an simulation comparing the regular PN Junction Diode with the ideal diode:


Click to open and simulate the circuit above.


Note that D2 (a PN junction diode) gives us a gentle knee as it transitions from off to on, as real diodes do. D2’s curve is smooth in the calculus sense: its derivative is continuous.


In contrast, ideal diodes D1 and D3 show an abrupt (piecewise-linear) knee when they change from off to on. These are not smooth in the calculus sense: their derivatives are discontinuous.


### Use the PN junction diode when:


- Intending to accurately simulate real-world devices
- Simulation runtime is not a constraint


### Use the ideal diode when:


- Learning about signal rectification (where PN junction complexity is unnecessary)
- Simulating signal clamping behaviors (where hard clamping is acceptable)
- Simulating switching power supplies: buck, boost converters, etc. (where simulation performance is a constraint)
- Other cases where much faster-running simulations are preferred to accurate diode modeling


Both diode models, as well as Zener Diodes, photodiodes, and LEDs, are nows available in the CircuitLab toolbox:


---


### Comments


No comments yet. Be the first!


#### Leave a Comment


Please[sign in](https://www.circuitlab.com/accounts/login/?next=/blog/2020/08/10/ideal-diodes-in-circuitlab/%23comment_form) or[create an account](https://www.circuitlab.com/accounts/register/) to comment.


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
