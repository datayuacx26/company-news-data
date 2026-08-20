---
schema_version: "1.0.0"
document_id: "2a5618f76d794ad02a62590396748e51970e32a59883e6b54172f4e0f211bab4"
company_key: "hagerty-inc-class-a-common-stock"
company: "Hagerty Inc."
source_id: "hagerty-inc-class-a-common-stock-rss-699fc274d6c4"
canonical_url: "https://www.hagerty.com/media/opinion/the-hack-mechanic/diagnosing-an-odd-problem-with-the-lotus/"
published_at: "2026-07-27T13:00:00+00:00"
first_seen_at: "2026-07-27T14:14:22.491446+00:00"
fetched_at: "2026-08-12T11:56:44.014133+00:00"
content_hash: "sha256:9f5af981efaccee2d2799be42d8f215e4352c450ad01b3eae7e1a080006342e0"
---

# Diagnosing an Odd Problem with the Lotus

During the past few weeks, I’ve made several references to an odd problem with the ’69 Lotus Elan +2. The car would start and run fine, but when I began cruising on the highway at about 65mph or above, it would run out of power—there was very little response to feeding more gas, and the car would gradually lose speed. It wasn’t the kind of power loss you get from a dying fuel pump or a clogged fuel filter where the car stumbles and feels like it’s running out of gas and then actually *does* run out of gas in terms of fuel being in the tank but not reaching the engine. This was less dramatic than that, more like a subtle stumble and a soft bogging-down in power. Still, whenever it happened, since something was clearly wrong, I’d get into the right lane and take the first exit, and once I was off the highway and driving more slowly on a secondary road, the problem was gone.


There were four odd components to the problem. The first was it initially only manifested itself when I switched on[the electrically driven air conditioning](https://www.hagerty.com/media/opinion/the-hack-mechanic/the-conclusion-of-the-electric-a-c-story/) . This causality seemed as reliable as gravity—I’d hit the highway, wind it up to 65, turn the A/C on, and experience the bog-down. Now, I know what you’re thinking—a 57-year-old British car with a little 1558-cc engine and retrofitted air conditioning, of *course* it bogs down when you switch the air on. But this wasn’t like that. The A/C doesn’t have a magnetic clutch like a standard belt-driven compressor where you can practically feel it kick the engine in the family jewels when it switches on. The electrically-driven compressor is much gentler than that. Plus, it never happened unless I was running on the highway at speed. But the fact that it only happened with the A/C on made me think that it had to be due to the 80-ish amp draw of the electric compressor and the fans causing a voltage drop to the fuel pump, the ignition, or both. I took the car for several decently long highway drives without switching on the air conditioning, and didn’t have the problem appear.


The second bit of weirdness was that this then changed—the problem began rearing its ugly, strange Lotus-inspired head even with the A/C off, but still only at speeds above about 65mph.


The third component provided a big clue: I had installed an air-fuel meter connected to an ultra-wideband oxygen sensor over the winter to help diagnose a backfiring issue, and I noticed that, 100% of the time, when this bogging-down problem appeared, the meter swung hard lean. So clearly either the engine wasn’t getting enough fuel, or it was getting too much air—e.g., a vacuum leak. I’ve used air-fuel meters to dial in mixture on cars, but this was the first time it became a troubleshooting aid.


The air-fuel meter pegging hard lean was telling me something crucial.


Rob Siegel


The fourth el-weirdo thing was that the problem wasn’t tied to high engine rpm—I never had it occur when winding the car out or holding it at a constant 5000 rpm on a local road. It was *only* at fourth gear highway speeds of about 65mph (about 3800 rpm).


While thinking about how to diagnose and fix the problem, two things occurred to me. The first was that if you had a vintage car doing this and took it to even a reputable vintage car specialist, I think it’s unlikely they’d actually take the time to run it on the highway and experience the problem. Instead, they’d probably listen to your description of the symptoms, decide that it’s a fuel-delivery issue, replace the fuel pump and filter, return the car to you, and say “Let us know if it happens again.”


The second is that even though you as the owner are the person best-equipped to troubleshoot an odd problem like this, exactly *how* are you going to do it? On the one hand, you want to catch it in the act, but on the other, you don’t want to be *so* in the act that the car dies and leaves you stranded. Plus, living in suburban Boston means that needing to run at a sustained 65 mph and waiting for something to stumble requires driving on the highway. From a safety standpoint, you really don’t want to be troubleshooting things in the breakdown lane of an interstate during high traffic. So I tested mostly on weekend mornings. Both I-90 and I-95 are two miles from my house. I-95 has exits that are spaced closer, allowing me to bail at the first sign of trouble, but I-90 gives more running room for the problem to manifest itself. I alternated between them.


In my gut, I felt like this was a fuel delivery issue even though the car didn’t sputter and die. The fuel filter is a small metal cylinder that screws directly into the aftermarket electric fuel pump in the trunk. I unscrewed it and drained it over a paper towel. A little sediment came out, but not much. When it was dry, I blew through it. It didn’t feel restricted. I replaced it anyway and drove the car. No change.


This wasn’t the cause of my woes.


Rob Siegel


I connected my fuel-pressure gauge to the output of the electric fuel pump and found that it delivered about 4psi, which, if anything, was a little high. I decided that I needed to watch the fuel pressure delivered to the carbs in real time while I was driving, which was possible only because I still had the hood off the car from[adding the additional cooling fan](https://www.hagerty.com/media/opinion/the-hack-mechanic/extra-cooling-for-the-lotus-elan-2/) last month. It took a few iterations of ordering gauges and fittings from Amazon to get something that I could cajole into drive-time visibility, but I jury-rigged it into functionality.


90-degree 1/4-inch barbed to 1/8-inch NPT adapters to the rescue.


Rob Siegel


With the key cracked to ignition, there was 3 psi of fuel pressure in the engine compartment, which dropped to about 2.5 with the engine idling, which is what the Strombergs reportedly want to see. While driving at speed on the highway, this dropped further, sometimes even as low as 1psi. This seemed very low to me, but I saw no correlation between it and the bog-down / lean reading on the air-fuel gauge. Still, I bought a proper Huco fuel pump from a European source to have on hand if I needed to swap it in.


This worked just well enough to be useful, but didn’t help me find the problem.


Rob Siegel


So, electrical issues next. I ran a wire from the hot side of the ignition coil into the cabin, and another one from the hot side of the electric fuel pump, and looked at both of these on my multimeter while driving. As with the fuel pressure gauge, there was no voltage drop on the coil or the fuel pump that accompanied the onset of low power and lean running.


I use this pair of banana plugs and a wire with quick-connect spades at the end when I need to run long wires to the multimeter.


Rob Siegel


Okay, so that leaves a vacuum leak, right? This is a primitive car. Whatever emission controls were on it in 1969 are gone. The only vacuum lines are to the brake booster and the pop-up headlights. I disconnected and capped them and drove the car. No difference.


The disconnected and capped-off vacuum line to the pop-up headlights.


Rob Siegel


To be thorough, I used the[smoke tester](https://www.hagerty.com/media/opinion/the-hack-mechanic/diy-discussion-smoke-em-if-you-got-em/) I employ to find small vacuum leaks on my contemporary BMWs (nearly always due to splits in tiny rubber/cloth hoses). I duct-taped over the carburetor intakes and zip-tied a rubber glove over the exhaust, and didn’t find any smoke trail indicating a vacuum leak.


The duct-taped-over Strombergs on this little British car reminded me of the cover of The Sex Pistols’ “God Save The Queen *.* “


Rob Siegel


I was stumped. I sought input from my friend and fellow writer Sam Smith. He said a few things that were helpful and interesting. The first was “Sounds like the problem is engine load.” I hadn’t thought about that. There is an SAE International standard definition of load, but heuristically, it’s the amount of work the engine is performing, sort of “torque demand.” From a common-sense standpoint, if you’re driving on a level road, the load is much less than maintaining the same speed while heading up a steep hill. The load is higher when the throttle is open further, but to actually calculate load, the ECU in modern cars uses engine RPM, the Throttle Position Sensor (TPS), the Mass Air Flow (MAF) sensor, the Air Intake Temperature (IAT) sensor, and the Engine Coolant Temperature (ECT) sensor. It’s not like you can throw a single sensor on and measure load in the same way that I just measured voltage and fuel pressure. Besides, the problem in the Lotus seemed to be binary—it was either experiencing the bog-down-lean-running or it wasn’t. If the engine was actually leaning out as load increased, I’d expect the air-flow meter to show a gradual change. But that was just a guess.


Then Sam said, “You know, those Stromberg 175CD carbs are basically a pile of hot early emission-compliant garbage. You should ditch them and install a pair of Mikunis. You know you want to.” And I did (want to). The Strombergs in my Lotus Europa seem well-mated to its bone-stock engine, but the engine in this car has a pair of sprint-specification cams like the European Elan +2 S130 installed in it, and the Euro car never had the Strombergs—it had Webers. And it’s non-trivial to convert the car to Webers because the intake manifolds are a cast-in part of the head. Suddenly I had the image of a Mikuni conversion solving my running problem *and* uncorking power, throttle response, and drivability. I read up on the conversion on the Elan forum and found John Parker at[Vintage Performance Developments](https://www.v-performance.com/) . He was extremely responsive via email, and quoted me $1500 all-in for a conversion kit including the dual Mikunis, air filters, adapter collars, a carb jet tuning kit, the choke bracket system, and shipping.


Hmmmmn.


Now, I’m a practical guy, and as I say over and over, none of the cars ever gets everything it needs. I decided that I’d pony up for Mikunis only if there was some issue that I could trace to the Strombergs that there was no easy way to fix. Sam mumbled something about me not being any fun.


But it did get me thinking that maybe the root of the problem was not a general fuel-delivery-pump-and-filter issue, but was instead carburetion. The folder that came with the Elan +2 contained a receipt for a professional rebuild of the Strombergs when the car had its at-home restoration in 2013, so it’s not as if someone had simply poured fresh gas into the tank of a long-dead British car and hoped for the best. Still, if something was happening to *one* of the Strombergs at a certain load, that would explain a lot.


Float level! That’s it, right? Maybe the level in the float bowl of one of the carbs is adjusted too low, and under high load, is causing that bowl to run dry. That’d show up as a gradual loss of power and a lean condition, and it would recover when it refills, right? Booya! I’ve got a bead on you, sucker!


Great theory. How do you prove it?


On the Webers that are usually on the vintage BMWs I’m used to, you’d wait for the symptom to exhibit itself, pull over, yank the top off the carburetor, and look into the float bowl. It’s easy, really, as long as you don’t mind doing it in the breakdown lane with death whipping past you at 80 mph. However, on the Strombergs, the float bowls are underneath and thus not nearly as easy to get to. A direct measurement and adjustment of the float level can only be achieved by pulling both carbs off and turning them upside down, and I was loath to disconnect them from the linkage, as that would likely require resynchronization, which I’m currently not equipped to do.


Still, I could check the fuel level in the bowls, right? I could run the electric fuel pump to fill the float bowls up, shut it off, drop them, and look at the amount of fuel in each, right? And if absolutely necessary, I *could* do it by the side of the road, right?


So I tried. And it doesn’t work, because the float bowls have this dubious design where there’s a big brass plug with a rubber O-ring around it right in the middle, and when you drop the bowl, all the fuel leaks out where the plug had been.


The float bowl showing the gaping hole left by the plug when you drop it (and, yes, a little bit of rubber and sediment).


Rob Siegel


It made me realize that, on both Lotuses, I’ve treated the Strombergs as black boxes, and don’t really know how they work. I have a basic understanding of normal jetted carburetors with throttle plates, but these things with their needle valves, pistons, rubber diaphragms, and oil-damped dashpots are like a deep-sea creature pulled up from the Marianas Trench that’s mistaken for an alien life form.


So I did a little reading. As I said, I was hesitant to pull both carbs off unless I had to, but if there were things I could check with them installed, I was all in. I checked the oil in the dampers and made sure that the pistons slid up and down. Then I undid the tops and checked for holes in the rubber diaphragms. I found none, but the diaphragm on the rear carb appeared slightly misshapen.


This didn’t look quite right.


Rob Siegel


I carefully lifted it up and noticed that a little rectangular protrusion on the bottom edge of the diaphragm wasn’t seated into the mating notch on the rim where it’s supposed to go. I wondered if *that* could be the cause of a vacuum leak above a certain load. As I thought about it, I realized I might have been responsible for it—I have the tool that you use to raise or lower the needle jet, and had used it at some point to richen up the mixture to get the car to run closer to the center of the air-fuel meter, and have a vague recollection of the diaphragm rotating around when I adjusted it.


Whatever the cause, I carefully aligned the diaphragm with the notch and reassembled the carb. I took the car onto the highway, raised the speed to 65, turned on the A/C, and waited.


No bog-down. No air-fuel meter swinging hard lean.


I drove the car for about 20 miles, varying speed between 65 and 70 (old tires and bent wheels function as a speed limiter). The problem never recurred.


So I think I fixed it. And that means that I can *finally* get back to what was supposed to be the main issue, which was going the final mile with the air conditioning and seeing if it actually gets the cabin cold enough to be useful, even though the evaporator that’s mounted in the passenger-side footwell isn’t directly blowing cold air at me.


But part of me thinks “Damn, I guess that’s a no on the Mikunis.”


Of course, my birthday *is* coming up, and I *have* been known to buy myself presents.


***


Rob’s newly expanded book, *The Best of The Hack Mechanic: 40 years of hacks, kluges, and assorted automotive mayhem* , is available on Amazon[here](https://www.amazon.com/dp/0998950750) . His other seven books are available[here](https://www.amazon.com/Rob-Siegel/e/B00E6EXQ6Q) , or you can order personally inscribed copies from Rob’s website, www.robsiegel.com.


The post[Diagnosing an Odd Problem with the Lotus](https://www.hagerty.com/media/opinion/the-hack-mechanic/diagnosing-an-odd-problem-with-the-lotus/) appeared first on[Hagerty Media](https://www.hagerty.com/media) .
