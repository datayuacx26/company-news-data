---
schema_version: "1.0.0"
document_id: "2e6edfefc243d091dfa113e77f0f7b46468f49e149a1b50bf2625a22fc77d468"
company_key: "yc-subimage"
company: "SubImage"
source_id: "yc-subimage-news-import-96b948f674dd"
canonical_url: "https://www.subimage.io/blog/veriff-case-study/"
published_at: "2026-01-20T00:00:00+00:00"
first_seen_at: "2026-07-26T02:26:23.667341+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:cbc8fd3d37eeb66ced7e483282a2750cc1d1e8b2128792b11ab01d1498039cc7"
---

# How Veriff Evaluated React2Shell Risk Across 600 Repos in Minutes With SubImage

## **Executive Summary**


After evaluating several established vendors,[Veriff](https://veriff.com/) 's security team adopted[SubImage](https://subimage.io/) —a platform that maps identity, infrastructure, and vulnerabilities into a single graph, so teams can see which findings sit on real attack paths.


This enabled a shift in approach:


- **From raw counts to attack paths:** Instead of treating all findings equally, the team saves time by prioritizing what's actually exploitable.
- **From patching containers to fixing base images:** Rather than chase vulnerabilities across thousands of containers, they can now patch at the base image and fix in one place.
- **From infrequent patches to daily updates:** With patching centralized at the base image, frequent updates become easy, removing the need to create tickets in many cases.


When the critical[React2shell](https://www.subimage.io/blog/react2shell/) npm vulnerability hit in December 2025, Bauman's team verified that they had a clean bill of health in **minutes, not days** .


## **The Challenge**


**Viljar Bauman** has been in cybersecurity for two decades. He's worked in telecoms, banking, and spent formative years in Estonia's Ministry of Defence. For the past seven years, he's served as Deputy CISO at Veriff, the identity verification company that helps businesses prevent fraud by verifying real people against real documents.


At Veriff, security is central to the product. The company processes sensitive identity data at scale, operating a sprawling cloud-native infrastructure on AWS with EC2 instances, Kubernetes clusters, tens of thousands of containers, and hundreds of repositories full of dependencies.


"Our stack is like other modern startups," Bauman says. "But what makes it special is the nature of our service. To make sure fraudsters can't pass our technology, we have to collect significant amounts of data and rely heavily on machine learning. That means a lot of infrastructure to protect."


His team ran a proof-of-concept with traditional vulnerability management tools on their staging environment.


"At one point, I saw the number: **seven million vulnerabilities** ."


He pauses. "First I was like, *what is going on here?* But then I realized, yeah, it makes sense. Even one Linux machine can have thousands of packages. Multiply that by thousands of containers and the number goes up fast."


This raised a key question: ***Who is going to fix all of this?***


## **The Problem with Vulnerability Counts**


The vulnerability management process itself isn't broken: scan, find, ticket, patch, repeat. It produces clean metrics and satisfies auditors. But Bauman saw a problem with taking the output at face value.


"Too much effort goes into patching stuff that may not actually be relevant, or that you could mitigate other ways. Your security team, your IT team, maybe even engineering, they're all putting resources into patching. But does it actually make attackers go away? Not necessarily."


"Let's say you have a high-severity vulnerability in a container. But if the path to that container is so hard that an attacker would never realistically get there, does it make business sense for them to try? Meanwhile, you might have something else that's wide open."


Bauman kept coming back to the same realization: **a vulnerability on its own doesn't tell you much. What matters is whether it's part of an attack path** : a chain of access, permissions, and misconfigurations that an attacker could actually exploit to get somewhere valuable. Tie a vulnerability to an attack path and suddenly you know whether it matters or not.


"You have to think like the attacker thinks," he says. "They're drawing out different approaches: looking for the open window, the unlocked door. If you start thinking that way, you can start fixing the things that actually matter."


## **Mapping the Real Attack Surface**


But thinking like an attacker required something Bauman didn't have: visibility into how everything connected. He had all the infrastructure components, but no clear view of how they connected.


"It was like Lego pieces scattered around a kid's room floor. I had all the parts, but I couldn't see the picture."


He needed a way to map relationships: which IAM roles attach to which instances, what permissions flow where, and how an attacker might chain them together.


**With SubImage, that picture came into focus** .


"What I loved is that you're showing the connections: what an attacker would actually want to get their hands on. You start at a simple EC2 machine, you can see what IAM roles are attached, what group the role belongs to, and so on."


**Bauman had also rethought how patching should work, and the visibility made it actionable** . "Just make sure you're always using the latest image," he says. "If you deploy containers at least daily, you don't have to bother engineers with tickets; the vulnerability count goes down on its own."


The graph-based approach also improved communication. Security findings are often difficult to explain to non-technical stakeholders, but the visual representation made these relationships clearer.


"You can show it to your top manager who doesn't know anything about security. It's like playing Monopoly: you go from one square to another, and you can show them: **this is how the attacker would get to our precious data** ."


For Bauman, what made the difference wasn't just the tooling; it was the philosophy behind it.


"The mindset working with the SubImage team is excellent. You're thinking like attackers, that makes a lot of difference."


## **React2Shell: A Clean Bill of Health in Minutes**


In December 2025, a malicious npm package triggered what the security community called "[React2shell](https://www.subimage.io/blog/react2shell/) ." For security teams, the immediate question was simple and urgent: *Are we affected?*


For Bauman, with 600 repositories and thousands of Node.js dependencies across Veriff's codebase, that question used to be hard to answer.


"With the tools we were using before, it was very difficult to figure out if we were vulnerable or not. It could take so much time."


With SubImage, he queried across 600-700 packages and had a clean bill of health confirmed instantly.


" **You almost just talk to it like ChatGPT** —tell me if I have this, and you get the answer. **React2shell was a critical issue. You really want to know as fast as possible so you can fix it or mitigate it. And we could.** "


---


For Bauman, the value is cumulative: the more systems connected, the more complete the map, the better the decisions.


"Don't just pick another security tool because it's popular. Think about what you actually want to fix. Think like the attacker. And then find the tools that fit *your* context, not someone else's."


---


*Viljar Bauman is Deputy CISO at Veriff. This conversation has been edited for length and clarity.*
