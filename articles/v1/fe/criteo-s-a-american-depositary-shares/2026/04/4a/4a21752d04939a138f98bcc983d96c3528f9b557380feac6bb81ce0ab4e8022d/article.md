---
schema_version: "1.0.0"
document_id: "4a21752d04939a138f98bcc983d96c3528f9b557380feac6bb81ce0ab4e8022d"
company_key: "criteo-s-a-american-depositary-shares"
company: "Criteo S.A."
source_id: "criteo-s-a-american-depositary-shares-rss-02db2411825d"
canonical_url: "https://medium.com/criteo-engineering/the-grind-behind-the-epiphany-a-short-story-of-a-research-project-45e94a21a489"
published_at: "2026-04-30T06:46:01+00:00"
first_seen_at: "2026-07-20T23:17:33.645392+00:00"
fetched_at: "2026-08-20T03:05:58.983824+00:00"
content_hash: "sha256:a9de135af2a8adba13848920ed9cf8804aa15d6c6caf483ac665e649a591d25c"
---

# The Grind Behind the Epiphany: A Short Story of a Research Project

Author: Alain Rakotomamonjy


> From ideation to outcome, this is the story of a privacy-preserving research project. It tells how research can generate innovations but also joy and despair.


### **Early 2024: The “Hammer” Phase**


Two research leads who pioneered Criteo’s early privacy initiatives, as part of the Criteo multi-year research program, introduced me to a challenge born from the[Privacy Sandbox](https://privacysandbox.google.com/) era: Learning from Label Proportions (LLP). The data was no longer granular; we were looking at “bags” of samples where only the aggregate label distributions were known.


I let the problem sit. Research often begins with a subtle, subconscious “incubation” phase. This is where the mental load of the unknown begins to take its toll. In my case, but I know I am not alone, the problem starts to consume your brain **.** It follows you home, sitting at the dinner table and echoing through your sets at the gym. It even finds its way into your dreams, playing out in abstract logic. Eventually, this constant mental churn led to a breakthrough. I convinced myself this was a Domain Adaptation (one of my favorite research topics) problem in disguise that could be addressed using my favorite mathematical tool.


As the saying goes, when you have a hammer, everything looks like a nail, and my favorite mathematical tool was the only hammer I liked to use.


> Research Lesson #1: The Tool Trap.


> We often default to our favorite methods because they feel safe. The difficulty lies in staying objective enough to realize when your favorite tool is actually a distraction.


### **October 2024: The “False Peak”**


I began generating “synthetic” samples to start working on the problem. On toy datasets, the results were staggering. **Mood: Indestructible!**


I onboarded two other researchers on the problem, with whom I already had successful research stories, and moved to real-world benchmarks and stress-tested the model on problems considered hard by Google fellows. It outperformed the state-of-the-art (SOTA) in nearly every category. We felt like world champions, and an[ICML 2025](https://icml.cc/Conferences/2025) submission was a no-brainer… or so we thought.


### **January — March 2025: The Reality Check**


We submitted the paper, complete with a new algorithm and what we believed was foundational theory. Then came the “silence”, two months of forced patience to let our brains muse on other problems while the work sat in the hands of reviewers.


Then came the day of the reviews. We were overconfident, eager to see just how enthusiastic the reviewers would be. You know the feeling: refreshing the webpage every minute, heart rate climbing with every click.


The reviews showed up. They weren’t glowing; they were lukewarm.


The crushing blow came from a single comment: a reviewer pointed to a 2019 paper we had completely overlooked. At first glance, it felt like the classic “reviewer-wants-to-be-cited” situation, a common hurdle in the peer-review process. Yet I rushed to re-implement the paper, as performance is everything, praying it was a false alarm or a niche case. It wasn’t. Their performance was better than ours. The “novelty” and performance we felt so proud of were suddenly under threat.


**Mood: Rock bottom.**


> Research Lesson #2: The Hidden Literature.


> Research is difficult because you aren’t just racing against the present;
> you’re racing against every paper ever written. Missing one citation can invalidate months of work.


### **April 2025: The Valley of Despair**


The rejection was official. It stung, but it was fair: the competition’s performance was simply superior. In a single review cycle, we went from feeling like world champions to local league players.


To make matters worse, the ground shifted beneath us:[Google announced it was pivoting away from the Privacy Sandbox](https://privacysandbox.google.com/blog/privacy-sandbox-update) . The emotional toll was too high and our motivation evaporated. We lacked the energy to pivot and the time to reflect, so we did what many researchers do when the light fades: we put the project in a drawer.


> Research Lesson #3: The Psychological Toll.


> This is the hardest part of research: the “Dead End” phase. It requires immense emotional resilience to watch your work be rejected and your project’s relevance questioned simultaneously.


Sometimes, “putting it on pause” isn’t giving up; it’s necessary maintenance for your brain.


> Research Lesson #4: Strategic Patience.


> Sometimes, the best thing you can do for a project is to stop working on it. Burnout kills creativity; distance creates perspective.


### **November — December 2025: The Transcendent Simplicity**


Dust settled. The relentless pressure to “publish or perish” for the ICML cycle had faded, and another successful privacy project gave us the breathing room and the energy to open the drawer again.


Then, it happened. Out of the quiet churn of dozens of trials and errors, and the solitary musings of a researcher staring at mountains of experiments, came a spark. A new way to refine the method, one that actually simplified the entire architecture.


The results were undeniable: we had achieved state-of-the-art performance. Again.


> Research Lesson #5: Order from Chaos.


> The difficulty of research is that you must navigate through extreme complexity to find simplicity. You have to be willing to be wrong, to be rejected, and to wait, just to find that one elegant truth that was hiding in the noise all along.


In December, as we sat down to write the paper, the true “Epiphany” arrived. The new algorithm fit perfectly into a beautiful mathematical framework, one that made the entire approach feel flawless. There were no longer any “black box” components or parts justified by sloppy rationales. The pieces didn’t just fit; they belonged together. It was no longer a collection of ideas; it was a cohesive truth.


As it turns out, performance isn’t everything. Mathematical beauty counts.


> Research Lesson #6: Writing is Reasoning.


> The “write-up” isn’t just a report; it is the final, most rigorous stage of discovery. Moving from a mental sketch to a formal paper acts as a cognitive filter, exposing “hand-wavy” logic and forcing messy architectures into elegant frameworks. You don’t write to record an epiphany, you write to invite one.


### **January 2026: The Seoul Cycle**


The dust from the previous year’s rejection has been recycled into fuel. We’ve just hit “Submit” for[ICML 2026](https://icml.cc/) in Seoul. The paper is unrecognizable from the 2025 version; the new algorithm is more efficient, the math is rigorous, and we’ve proactively addressed that 2019 competitor in the related work.
**Current mood status: Cautiously Zen** .


But the difficulty of research doesn’t end at the submission button. Now begins the “Black Box” phase. Between now and the April 30 notification date, the work is out of our hands. We will likely face a tough rebuttal period in late March, where we’ll have one week to defend our work against “Reviewer 2”, who might still argue that the problem is irrelevant because the Privacy Sandbox was dropped.


> Research Lesson #7: The Zen of Detachment.


> You have to care enough about the work to spend a year perfecting it, but be detached enough to realize that a conference decision isn’t about you
> personally. Research is a marathon where the finish line (the conference in Seoul) is fixed, but the path is paved with “Weak Rejects” that you have to flip into “Accepts” through sheer persistence.


### **So What May Happen Next?**


- **The Best Case** : The epiphany holds up. Reviewers recognize the elegance of the mathematical framework, and by July 2026, we will be presenting our work at the COEX Center in Seoul, contributing a definitive solution to the LLP problem.
- **The Reality Check** : We might get “lukewarm” scores again. If so, the next step isn’t to give up as we did in April 2025, but to use the rebuttal phase to educate the reviewers on why this simplified framework is actually a fundamental shift in the field.
- **The Pivot** : Regardless of the ICML outcome, the “epiphany” has given the work a life of its own. Even if the Privacy Sandbox is gone, the math behind learning from aggregated data is universal, it’s the future of medical data, financial privacy, and beyond.


### **The Final Word: The Pulse of Discovery**


In the end, this is the heartbeat of research. It is a cycle that demands you find the courage to begin confident and joyful, the resilience to endure the despair of being “scooped” or rejected, and the clarity to see that a failed submission is often just the “incubation” for a greater idea. Of course, we must also accept the truth that, sometimes, the drawer stays closed.


Whether the notification on April 30th is an “Accept” or a “Reject,” the outcome has already been won. We traded the overconfidence of the “world champion” for the quiet certainty of the architect of a novel framework. We found the beauty in the math, and that is a victory that no reviewer can take away. Furthermore, the algorithm we built will serve as a keystone when the industry shifts again, placing Criteo in a formidable position for whatever comes next in privacy.


**Research is a rollercoaster, but it’s a ride worth taking.**


#### **The Math Behind the Epiphany**


The core challenge of **Learning from Label Proportions** is that the labels are hidden inside “bags”. Our breakthrough came from a simple intuition: if we introduced a small set of samples, anchors, with known labels, they could act as a compass for guiding the model to identify categories within the rest of the data, only based on proportions.


We framed this as a **Domain Adaptation** problem. We treated the anchor samples with known labels as a “source” and the unlabeled bags as the “target”. The goal was to align the features of the unlabeled data within those bags with the known behavior of our anchors.


To make this alignment happen, we used **Optimal Transport** . Think of it as a mathematically disciplined way of moving points in space. Instead of letting the model guess, we used an iterative process to “move” samples toward their most likely counterparts. This was the “hammer” I loved to use, and it provided a crucial guarantee: the math ensured that the majority of our samples were being pulled toward the correct regions of the feature space, rather than drifting aimlessly. It turned a guessing game into a precise geometric migration.


The final, “beautiful” framework emerged as a **single, unified optimization problem** . It doesn’t just move the data; it jointly learns the model parameters while migrating the anchor samples toward their final goal in one seamless mathematical motion.


Check the paper 👉[Particle Flow for Learning from Label Proportion.](https://hal.science/hal-05063059/)


---


[The Grind Behind the Epiphany: A Short Story of a Research Project](https://medium.com/criteo-engineering/the-grind-behind-the-epiphany-a-short-story-of-a-research-project-45e94a21a489) was originally published in[Criteo Tech Blog](https://medium.com/criteo-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
