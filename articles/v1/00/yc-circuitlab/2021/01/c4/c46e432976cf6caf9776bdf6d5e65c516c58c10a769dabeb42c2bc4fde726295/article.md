---
schema_version: "1.0.0"
document_id: "c46e432976cf6caf9776bdf6d5e65c516c58c10a769dabeb42c2bc4fde726295"
company_key: "yc-circuitlab"
company: "CircuitLab"
source_id: "yc-circuitlab-rss-d25759254652"
canonical_url: "https://www.circuitlab.com/blog/2021/01/04/extended-numerical-precision-for-parameter-sweeps/"
published_at: "2021-01-04T21:00:00+00:00"
first_seen_at: "2026-07-24T22:19:16.775475+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:a6e2c67355f7a0f0b6f7feb00dfa3b18611792f5b6330a7f2fdc9d60bc38030b"
---

# Extended Numerical Precision for Parameter Sweeps

## [Extended Numerical Precision for Parameter Sweeps](https://www.circuitlab.com/blog/2021/01/04/extended-numerical-precision-for-parameter-sweeps/)


#### Jan 04 2021, 1:00 PM PST ·[0 comments »](https://www.circuitlab.com/blog/2021/01/04/extended-numerical-precision-for-parameter-sweeps/#comments)


CircuitLab has always made it easy to simulate the same circuit with variations of one or more parameters. For example, we can quickly compare the frequency response (Bode plot) of a RC low-pass filter with different capacitors by setting up the simulation with a` Decade` sweep over parameter` C1.C` from` 1n` to` 1u` :


With` 1` point per decade, this instructs the circuit simulator to build the circuit with four values for capacitor $C_1$


, specifically $1 \\ \\text{nF}, 10 \\ \\text{nF}, 100 \\ \\text{nF}, 1 \\ \\mu\\text{F}$


. The four responses are compared by plotting them in different colors on a single Bode plot. With old CircuitLab software prior to today, the result looked like this:


While it works, you’ll notice in the plot legend that there are rounding errors in the values, such as` when C1.C is 1.0000000000000042e-8` , when it should be precisely` 1e-8` . That happens because the computer represents values as 64-bit floating point, leading to roughly 16 decimal digits of precision.


Instead of simply rounding or truncating the display, we’ve fixed this by applying our extended numerical precision to parameter sweeps as well. See our earlier article[Double-Double, Please! When 64-Bit Floating Point Isn’t Enough](https://www.circuitlab.com/blog/2013/07/22/double-double-please-when-64-bit-floating-point-isnt-enough/) for more details about extended precision numerical routines.


As of today, this issue has been fixed, and the capacitor values are calculated precisely:


We hope this makes CircuitLab a more pleasant tool to use for investigating circuit behavior.


Try the example simulation for yourself:


---


### Comments


No comments yet. Be the first!


#### Leave a Comment


Please[sign in](https://www.circuitlab.com/accounts/login/?next=/blog/2021/01/04/extended-numerical-precision-for-parameter-sweeps/%23comment_form) or[create an account](https://www.circuitlab.com/accounts/register/) to comment.


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
