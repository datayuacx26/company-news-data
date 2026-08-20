---
schema_version: "1.0.0"
document_id: "65e7d9202dd5d2b3e6968431fcd913b0071946b92765f15a47252e99a5d674f3"
company_key: "kodiak-ai-inc-common-stock"
company: "Kodiak AI Inc."
source_id: "kodiak-ai-inc-common-stock-rss-dc4af1b0a9ba"
canonical_url: "https://kodiak.ai/news/pra-quantifying-safety"
published_at: "2026-06-03T15:00:00+00:00"
first_seen_at: "2026-07-20T23:18:01.334765+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:877df5ec9369143250c9e084f3b27a03187a56b67cdeb027ac270270bda0f8be"
---

# Quantifying Safety Knowns and Unknowns: How Kodiak Uses PRA and AI to Measure Risk

# Quantifying Safety Knowns and Unknowns: How Kodiak Uses PRA and AI to Measure Risk


[Kelly Smith](https://kodiak.ai/news?author=kelly-smith)


|


June 3, 2026


Rapid AI advances now enable engineers to develop autonomous driving technology faster than ever, but the true frontier of autonomous driving is the ability to couple those advances with demonstrable and rigorous safety.


Increasingly, depth and rigor is achieved not through the biggest budgets nor the largest fleets, but by distilling the most precise insights from real-world testing and simulation that assure autonomous systems can handle rare and unusual scenarios, the kind that may only occur once in a lifetime of driving.


Kodiak has met this challenge by adopting two tools, including one we created with the help of AI, that accelerate the pace, depth and precision of our safety engineering. They go beyond traditional approaches and deliver clear, compelling evidence of the Kodiak Driver’s safety.


The first tool is Kodiak’s **Probabilistic Risk Assessment** ( **PRA** ). The PRA is a methodology Kodiak uses to estimate the expected rate of collisions of varying severities for the Kodiak Driver and to identify the key scenarios, risk factors, and autonomy failure modes most responsible for dominating the risk profile. We then compare this output against human performance baselines which we established in partnership with leading centers of transportation research.


The second is **BreakPoint** , an AI validation tool internally developed. BreakPoint hunts with intelligence and efficiency for edge cases that could result in collisions or other undesirable behavior.


The deep analysis capability provided by BreakPoint helps inform our PRA models. From this information flow, we precisely understand the key areas of risk for the Kodiak Driver and focus our efforts accordingly.


Together, these tools form core elements of our safety case and power our capital-efficient approach for safely developing and deploying our AI-powered autonomous driver in a variety of real-world environments and applications.


Collectively, Kodiak’s PRA and BreakPoint tooling represent critical cornerstones efforts to scalably deploy safe driverless vehicles.


### **Probabilistic Risk Assessment: Bringing A Quantifiable Dimension To Safety**


Autonomous vehicle safety cannot be merely claimed. It must be proven. The PRA is a method pioneered in other safety-critical industries, like aerospace and nuclear energy, for measuring safety risk.


The Kodiak PRA melds Bayesian probability theory, systems engineering, reliability analysis, and statistical models into quantified results. It acts as an inference engine that allows us to calculate expected rates of collision for scenarios that occur so rarely that they often could not be captured in real-world testing alone. Critically, the PRA characterizes uncertainty associated with our risk assessment itself, allowing us to know, with mathematical rigor, where our evidence is strong and where it needs to grow. Hard numbers, not gut feelings.


In simple terms, the Kodiak PRA decomposes scenarios into three primary factors:


- **Scenario Exposure:** How often does our vehicle encounter this type of operating scenario?
- **Collision Likelihood:** Given that our vehicle encounters this operating scenario, how likely is it that a collision occurs?
- **Severity of Collisions:** How severe would the collision in this scenario be?


The PRA accounts for inevitable uncertainties and incorporates new information as Kodiak collects more data and observations. So as we collect more data, the PRA updates to reflect our increased knowledge.


### **Practically Managing AV Risk**


Functional safety tends to focus on “what happens when something breaks?” For autonomous vehicle safety, an even more challenging question to answer is “Is my system capable of safely handling real-world scenarios even when everything is working as intended?”


The PRA method represents an iterative, living process to addressing autonomous vehicle safety, not a one-off box-checking exercise. In that way, it is distinct from traditional functional safety processes and standards found in the automotive and trucking industries, where functional safety analyses are conducted once to validate safety and compliance, and then left static.


The most relevant standard for this class of behavioral safety is the Safety of the Intended Functionality (ISO 21448) standard, which addresses hazards caused by the system performing correctly but then encountering unexpected conditions.


SOTIF frames safety in terms of four quadrants defined by whether a scenario is known or unknown and whether it is safe or hazardous. Traditional testing and validation are effective at confirming performance in known, safe and hazardous scenarios. However, the critical work lies in shrinking the space of unknown hazardous scenarios, where the system may fail in ways that have not yet been identified.


That is precisely what PRA and BreakPoint are designed to do.


The PRA provides a structured, quantitative framework for reasoning about hazardous scenarios we've already identified, moving them from "known hazardous" to "known not hazardous." BreakPoint attacks the harder problem: it actively searches the unknown space, surfacing hazardous scenarios we haven't encountered yet and pulls them into the known row where they can be analyzed, quantified, and addressed. Together, they form a systematic engine for collapsing the unknown hazardous quadrant. This is the central objective SOTIF articulates, and Kodiak's PRA and BreakPoint provide the practical machinery to achieve it.


### **Risk Informed Prioritization**


With the PRA results, we can directly compare the Kodiak Driver against the benchmark rate of collisions from comparable human drivers in analogous operating environments. By decomposing driving into structured events ranging from the mundane to rare, we can estimate risk with bounded uncertainty and assure the Kodiak Driver achieves superior performance.


While the PRA is important for determining risk at the point of deployment decision, it is perhaps even more paramount in identifying risk factors throughout the development process.


During the engineering development phase of new features or expansion into new ODDs, the PRA directly tells us the key factors that we should focus on.


Our Bayesian methodology forces us, by design, to accumulate sufficient evidence to conclude that the residual risk is low enough. Critically, the absence of sufficient evidence counts against us in the PRA. This drives Kodiak to not only fix known risks **** but directly guides us toward expanding coverage in unknown, under-explored scenarios.


This approach assures our driverless technology delivers on the broad promise of bringing safety benefits to the road. But it is not enough on its own. In order to credibly and statistically demonstrate the rate of severe collisions is extremely low, a brute-force approach would require an infeasible amount of real-world or simulation tests.


That’s where BreakPoint comes in.


### **BreakPoint: Our In-House AI Tool That Advances Autonomous Safety Design**


Kodiak’s systems engineering and simulations teams built BreakPoint, our own proprietary AI tool that uses advanced techniques to aggressively try to compromise our system in tests we conduct in simulation.


A common engineering testing technique is to deliberately inject faults into a system and observe the system’s response, to ensure it can handle the fault as expected. BreakPoint takes that idea to the next level and applies it to our autonomy system.


BreakPoint deliberately injects realistic, time-varying errors onto the signals that flow through the autonomy system in order to ensure that the autonomy system still drives safely in both normal and extreme conditions. Better yet, BreakPoint actively guides its search adversarially, actively trying to drive our system into a “collision” situation, thus helping us discover autonomy failure modes. Then, BreakPoint tooling helps us estimate the risk associated with this failure mode, and this information flows directly into our PRA.


Critically, BreakPoint helps us tame the combinatorial explosion of possible scenario permutations while simultaneously considering the robustness of the Kodiak Driver to typical and atypical errors. BreakPoint is designed to discover rare failure modes before they manifest on a real highway.


Does it work?
Yes.


BreakPoint has already discovered previously unknown failure modes our autonomy system has not yet encountered in the real world. For example, we discovered a low probability failure mode scenario in simulation in which Kodiak's Perception system could mis-estimate the velocity of stalled vehicles in the roadway in our Industrial ODD. This allowed us to directly address this certain scenario and we estimate that this type of scenario that BreakPoint discovered in minutes would have required tens of thousands of miles of real-world driving to occur even once.


BreakPoint simulation-based analysis happens in minutes and can be run by any autonomy software developer at Kodiak, and it accelerates Kodiak’s long-tail exploration by orders of magnitude. Whereas classic simulation techniques allow engineers to answer questions like “Does my feature work as expected?”, BreakPoint enables individual engineers to answer much deeper questions, such as “Does my new feature introduce some critical new failure mode in an unexpected scenario?”


Adversarial simulation techniques for autonomous vehicles are an active area of research across industry and academia, and probabilistic approaches to safety quantification have deep roots in aerospace and nuclear engineering.


Kodiak's innovation is not in any single technique but in the closed-loop integration: BreakPoint discovers failure modes and estimates their likelihood, the PRA incorporates and contextualizes it, which in turn identifies where evidence is thin and directs engineering effort toward the highest-impact unknowns. This “discover, quantify, prioritize, fix, and re-assess” cycle runs continuously and compounds over time.


Each iteration therefore simultaneously shrinks the unknown-hazardous space and strengthens the safety case. The result is not just a safer system design, but a system where safety can be rigorously demonstrated and remaining gaps are explicitly understood at ever increasing speed and precision.


### **Safety engineering a new way forward**


The autonomous vehicle industry faces a fundamental challenge: how do you substantiate safety claims for a system that must handle scenarios so rare they may never appear in any realistic amount of real-world testing? Kodiak's answer is to stop waiting for those scenarios to find us and instead go find them first.


The PRA gives us the quantitative framework to measure and bound risk across the full spectrum of driving scenarios and to know, with mathematical rigor, where our evidence is strong and where it needs to grow. BreakPoint gives us the ability to actively hunt for the failure modes that matter most, surfacing vulnerabilities in minutes that might otherwise take tens of thousands of miles to encounter.


This is how Kodiak moves beyond benchmarks. Not by accumulating miles for their own sake, but by building the analytical machinery to understand and reduce risk faster, deeper, and more precisely than brute-force approaches allow. And it's how we safely and responsibly deploy driverless vehicles into the real world and then scale.


### Categories


- [Safety](https://kodiak.ai/news?category=safety)
- [Technology](https://kodiak.ai/news?category=technology)


## More like this


May 18, 2026[Kodiak AI Issues Statement on BUILD America 250 Act Bill Supporting Autonomous Trucks](https://kodiak.ai/news/build-america-250-act)


April 30, 2026[Bosch and Kodiak AI Execute Toward Scaled Production of Autonomous Trucking Hardware](https://kodiak.ai/news/bosch-kodiak-autonomous-trucking-hardware)
