---
schema_version: "1.0.0"
document_id: "0fa5d8fdc938d8fdb34dbf5a2c89869c38dac8182958c5331b8cc2a51e9467ae"
company_key: "yc-circuitlab"
company: "CircuitLab"
source_id: "yc-circuitlab-rss-d25759254652"
canonical_url: "https://www.circuitlab.com/blog/2020/06/29/inverting-amplifier-how-to-build-and-simulate-op-amp/"
published_at: "2020-06-29T19:00:00+00:00"
first_seen_at: "2026-07-24T22:19:16.775475+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:ef7f4a3b8c38d6e15c9169ea3f586d1011f1fa3e7874f0260489fe98ce19b1f0"
---

# Inverting Amplifier: How to build and simulate op-amp circuit with a specific gain

## [Inverting Amplifier: How to build and simulate op-amp circuit with a specific gain](https://www.circuitlab.com/blog/2020/06/29/inverting-amplifier-how-to-build-and-simulate-op-amp/)


#### Jun 29 2020, 12:00 PM PDT ·[0 comments »](https://www.circuitlab.com/blog/2020/06/29/inverting-amplifier-how-to-build-and-simulate-op-amp/#comments)


In this tutorial video we build and simulate in an[inverting amplifier with a chosen gain using an op-amp and other passive elements](https://youtu.be/CbmKI9ncpJ8) . We go over a few key concepts of an op-amps, introduce negative feedback, and build out a circuit that allows you to choose a specific gain for your inverting amplifier.


### Video


### Circuit


### Transcript:


So I'm going to open up my CircuitLab editor here, and find the operational amplifier section, and I'm gonna drop this op amp onto my circuit. Op amps have lots of super cool properties, but I'm going to be using two big ones.


First, an op amp takes the difference of the two input voltages, and multiples that difference by a really really high number at the output voltage. This multiplication factor is the open loop gain of an op amp. We can see it here in the properties, it's the A_OL number, notice how all the device models have a different number, but the important thing is that number is a super high number, look em they're all in the hundreds of thousands or millions.


The other property is that no current (in reality very very little current) flows into or out of either of the input terminals. This will be important later.


The opamp models CircuitLab model the real devices based on the numbers in their datasheets. An "ideal" op amp which you may run into a problem set just has these properties maximally true, so the open loop gain is infinite and the input currents are actually 0.


Lets see these two properties in our simulation. \[Show open loop gain in CircuitLab editor\]


Ok so that's neat but not that useful because I want to be able to control this gain, just having an arbitrarily large number out here isn't helpful. So the first neat trick I'm going to use is negative feedback.


I'm gonna grab the output terminal and let's reason about what happens when I connect it directly to the minus input terminal. In this configuration, if something like a bit of noise in the universe was to make this voltage go up at the non inverting terminal, the output goes up, and the inverting input goes up, which makes the output go down. That output going down is fighting the original change.. And it balances out when there’s no difference between the two input terminals.


In this configuration, with negative feedback the op amp works to subtract any difference between the two input terminals. Meaning the inverting terminal will follow the non inverting terminal. We made a follower circuit!


So we are not there yet, let's introduce one more thing. I'm going to add a voltage source here, and two resistors. And I'm gonna call this one Rf, and this one Rin. Lets reason about this circuit. What is our output? We know in this configuration the difference between these two terminals is 0. The only way it can do that is by changing its output voltage and then measuring the two inputs again. We also know that no current can flow into or out of our input terminals. So I put a signal voltage here of 1, and if the op amp is forcing this to be 0, then our trusty friend V=IR says there is current flowing through this resistor, the other rule of the op amp says that current can't go into this terminal, so it has to go through this other resistor Rf. Again our trusty friend says if there is an IR there has to be voltage drop, and since it's the same I and R I think this voltage at out2 is going to be -1, and by golly it is.


So what if I make this Rf twice as large. Same input V, same Rin, so this current is the same, but now r is twice as much. Again V=IR so twice as much R and same I means twice as much Vout. Is it? Yes it is bloody exciting.


This means that just by changing the ratio of these resistors I can manipulate this circuit to have an arbitrary gain at its output!


You can find a link to this Circuit in the description of this video, you can open it up, change it and simulate it yourself to see what happens. If you found this helpful please like the video, and we will see you next time.


---


### Comments


No comments yet. Be the first!


#### Leave a Comment


Please[sign in](https://www.circuitlab.com/accounts/login/?next=/blog/2020/06/29/inverting-amplifier-how-to-build-and-simulate-op-amp/%23comment_form) or[create an account](https://www.circuitlab.com/accounts/register/) to comment.


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
