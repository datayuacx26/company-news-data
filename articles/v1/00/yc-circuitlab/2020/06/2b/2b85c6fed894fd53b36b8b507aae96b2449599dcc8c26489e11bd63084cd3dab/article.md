---
schema_version: "1.0.0"
document_id: "2b85c6fed894fd53b36b8b507aae96b2449599dcc8c26489e11bd63084cd3dab"
company_key: "yc-circuitlab"
company: "CircuitLab"
source_id: "yc-circuitlab-rss-d25759254652"
canonical_url: "https://www.circuitlab.com/blog/2020/06/18/how-to-build-and-simulate-a-2x1-multiplexer-mux-from-nand/"
published_at: "2020-06-18T19:00:00+00:00"
first_seen_at: "2026-07-24T22:19:16.775475+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:17fac1f73d701acb719008487bf945dc98e36678994d9a5bfee2c370f6744ff5"
---

# How to build and simulate a 2x1 multiplexer (MUX) from NAND gates

## [How to build and simulate a 2x1 multiplexer (MUX) from NAND gates](https://www.circuitlab.com/blog/2020/06/18/how-to-build-and-simulate-a-2x1-multiplexer-mux-from-nand/)


#### Jun 18 2020, 12:00 PM PDT ·[1 comment »](https://www.circuitlab.com/blog/2020/06/18/how-to-build-and-simulate-a-2x1-multiplexer-mux-from-nand/#comments)


In this video tutorial we build and simulate a two input[2x1 digital mux using only NAND gates](https://www.youtube.com/watch?v=bs-IApefvPM) .


## Video


## Circuit


## Transcript


In this video we're going to build a two input multiplexer or two input digital mux made entirely out of NAND gates. So first what is a digital mux. A digital mux is a two input digital component that lets you select one of the two inputs based on the state of a third digital input.


CircuitLab has a digital mux component you can use, but that's not the point here, instead I'm going to be building my own entirely out of NAND gates.


So lets explore the NAND gate a little bit. Whenever I see a new component I like to simulate it just to see how it behaves. And the best way to do that is to just simulate it.


\[Set up simulation with single NAND gate\]


When the two inputs are digital low, the NAND gate makes the input high


Let's see what happens when I make one of these inputs a digital one. My` V(out)` is still high.


What happens when I make both inputs 1, now my` V(out)` is low. What I just went through is the truth table of a NAND gate.


\[show truth table in CircuitLab\]


That gives us a couple of very interesting properties. When one of the inputs is high, the output of a NAND gate is the opposite of the other input. I'm going to use that property later. Remember it.


The other property is that it's very easy to build an inverter using a NAND gate.


\[set up NOT gate configuration\]


So bear with me while I drop a whole bunch of gates into my circuit here.


\[wire up the two input mux\]


I'm going to name some nodes just to make it easier. This is my select node, and these two are going to be my input nodes. So what is going to happen in this configuration? What happens when I put a digital low at the select input. This puts a digital low at this NAND gate which means the output of this NAND gate is always high. Going back to the truth table, if any of the inputs are low the output is always high. So this` in1` doesn't matter. The other thing this does, is because the output is always high then this nand gate is in that passthrough inverting configuration.


So lets go back to over here and see this NAND gate is a straight inverter here, which means this other NAND gate is also in this inverting pass through configuration. So in2 gets inverted twice, and ends up at the output. And we can see if I switch this select node to be a 1, then the opposite happens:` in2` doesn't matter and` in1` gets inverted twice at the output! We made a digital mux!


---


### Comments


I'm glad that this blog has come back to life. Good work. Keep the posts coming!


by[Dan.Ka](https://www.circuitlab.com/user/Dan.Ka/)
October 04, 2020


#### Leave a Comment


Please[sign in](https://www.circuitlab.com/accounts/login/?next=/blog/2020/06/18/how-to-build-and-simulate-a-2x1-multiplexer-mux-from-nand/%23comment_form) or[create an account](https://www.circuitlab.com/accounts/register/) to comment.


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
