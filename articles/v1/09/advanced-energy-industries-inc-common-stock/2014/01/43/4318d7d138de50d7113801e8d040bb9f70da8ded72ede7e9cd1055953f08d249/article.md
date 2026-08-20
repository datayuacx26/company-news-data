---
schema_version: "1.0.0"
document_id: "4318d7d138de50d7113801e8d040bb9f70da8ded72ede7e9cd1055953f08d249"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/5-easy-steps-to-5x-full-oxide-rate/"
published_at: "2014-01-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T22:27:47.856975+00:00"
content_hash: "sha256:41a6501ea25f081e07520152e421c1fc9019d9093ba5debd95d69a4fa8e8e61c"
---

# 5 Easy Steps to 5X Full Oxide Rate

Process engineers understand the benefits of achieving higher deposition rates while using less incoming power. We’ve also experienced the spiral into metal mode or full oxide mode simply by looking at the chamber the wrong way.


To achieve higher deposition rates by running “high on the transition curve,” we typically manipulate power supply voltage while in power control mode. In the case of doped silicon dioxide, we would then adjust the percent O2 in the process accordingly to keep the voltage (usually around 650 V for SiO2) where we want to run.


But this creates a pretty darned steep slope that we will slip and slide upon without costly aids such as a very fast MFC (40 msec from zero to rail) or a special PID. This also adds to the probability of a 2 A.M. call, as the full control loop must be stable and repeatable day after day.


*Hysteresis curve*


Here’s another way. The Advanced Energy® Crystal® AC power supply has a pretty cool built-in algorithm to run high on the curve without special MFCs or PIDs. It’s easy:


1. Set the supply in voltage control mode.
2. Give it a meaningful set point while in a full Ar atmosphere (such as 650 V).
3. Turn on the process.
4. You will notice that the power is quite low. Add O2 until you reach the power that you desire.
5. Run product.


“Wow,” you say. “How does this happen?”


Well, the power fluctuates a few watts up and down to keep the voltage constant. As the O2 content increases, so too does the power to consume it. When the O2 content decreases, the power also decreases to keep the proper O2 percentage in the plasma.


“OK, Doug,” you ask, “what happens if there’s an arc?”


Of course we want the Crystal power supply to react to the arc exactly as it is famous for, so there will be no spits or bands on the substrate. The difference here is that it will revert to process power by first increasing the power to consume the excess O2 that popped into the process during the arc. This makes for a very stable and repeatable regime.


This is particularly good for web coating, as the substrate and therefore the load is constant, and really all we need to deal with is arcing. Glass coating can be accomplished in the same way if the gap between lights is small. Pieces parts can also be done if the gaps are small.


I have experienced 4X to 5X full oxide rate with little effort. Even 8X to 9X is achievable with tweaks to gas delivery set up, process pressure (aka: Ar) control, and substrate (load) refinements.


The result will be more square feet of material into the box without 2 A.M. calls for help.


It is worth a try. Good luck. Be safe.
