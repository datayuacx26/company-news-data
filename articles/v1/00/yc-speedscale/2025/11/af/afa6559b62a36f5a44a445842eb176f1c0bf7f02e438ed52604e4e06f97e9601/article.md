---
schema_version: "1.0.0"
document_id: "afa6559b62a36f5a44a445842eb176f1c0bf7f02e438ed52604e4e06f97e9601"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/kubecon-retrospective-platform-engineering-needs-to-do-more-testing/"
published_at: "2025-11-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:25:08.967255+00:00"
content_hash: "sha256:15cdf6e922a8b25da433ed173a426aac3f4ef4af8c69a93c9504f1fde5cf1c29"
---

# KubeCon Retrospective: Platform Engineering Needs to Do More Testing

Every year, KubeCon offers a candid look at where the cloud-native community stands — the tools gaining traction, the pain points teams share, and the big gaps still holding organizations back. After a week of deep conversations, session hopping, and talking to dozens of platform teams, one theme became impossible to ignore:


**Platform engineering still isn’t doing enough testing.**


And even more surprising: **many teams don’t think testing is their responsibility.**


Despite massive investments in IDPs, golden paths, GitOps, automation, and Kubernetes itself, the foundational work of *validating* the platform is lagging far behind.


Here’s the retrospective.


---


## **1. Platforms Are More Complex Than Ever — But Testing Hasn’t Evolved**


Platforms now span:


- multi-cluster Kubernetes
- service mesh
- secrets and identity systems
- CI/CD pipelines
- golden paths
- observability stacks
- traffic routing layers


But when I talked to teams at KubeCon, a common admission was:


> “We’ll know something’s wrong once developers report it.”


Even worse, many platform engineers said bluntly:


> “Testing isn’t really my job.”


Yet platform issues impact **every** team that deploys software — which makes testing one of the most important responsibilities on the platform side.


---


## **2. Platform Teams Are Shipping Faster Than They Can Validate**


Kubernetes versions move fast.


Service meshes evolve weekly.


Cluster APIs change.


Policies shift.


Templates drift.


Platform teams push changes constantly, but few have automated testing to verify them.


The common rationale?


> “If something breaks, we’ll fix it quickly.”


That leads directly to fire drills, regressions, and developer frustration.


---


## **3. Golden Paths Aren’t Golden If No One Tests Them**


Golden paths were everywhere at KubeCon.


Teams want consistent workflows and easy developer experiences.


But the truth came out in conversations:


- many golden paths drift
- they’re not load tested
- they break after Kubernetes upgrades
- they rely on fragile assumptions
- they’re often only tested manually


When asked why, the response was consistent:


> “Developers test their apps — we assume the platform will be fine.”


Golden paths require **continuous platform testing** , not hope and goodwill.


---


## **4. Realistic Traffic Still Isn’t Part of the Testing Strategy**


One of the biggest gaps everywhere:


**No one is testing against real traffic patterns.**


Teams test:


- YAML linting
- static analysis
- integration boundaries


But they *don’t* test:


- ingress behavior during spikes
- autoscaling under real load
- service mesh routing under stress
- canaries with true request patterns
- cross-service dependencies
- realistic error scenarios


When I asked if teams replay production traffic, nearly all said:


> “That’s not something we do — that’s more for app teams.”


But this is **platform behavior** , and only platform teams can validate it reliably.


---


## **5. Burnout Is High — and Lack of Testing Is a Core Reason**


A quiet theme across KubeCon was exhaustion.


Platform teams are overwhelmed because they’re constantly:


- debugging broken pipelines
- firefighting regressions
- rushing cluster patches
- dealing with unplanned outages
- supporting frantic deployments


Every platform engineer I spoke to who invested in testing said the same thing:


> “We stopped firefighting. Everything became predictable.”


Automated testing reduces stress, improves reliability, and gives platform teams room to breathe.


---


## **6. The Bonus: Building a Digital Twin With Service Mocking**


One of the most exciting ideas that clicked with teams at KubeCon was the concept of **creating a digital twin of your production environment using service mocking.**


With tools like Proxymock and Speedscale, you can:


- clone production-like behavior
- mock external services
- test against realistic dependencies
- simulate failure modes
- validate integrations
- reproduce issues consistently
- scale tests without touching real systems


This gives platform teams something they’ve never had before:


**A safe, production-like environment for testing platform changes.**


### **And here’s the most important insight:**


### **⭐**


### **You don’t need to be a developer to test your platform.**


You can simply take a **snapshot of real production traffic** and **replay it** against a mock or digital twin.


No code changes.


No special setup from application teams.


No deep service knowledge required.


Just:


1. Capture real traffic
2. Replay it against your platform
3. Validate how your platform behaves under authentic conditions


This is a superpower for platform engineering.


---


## **7. The Teams Winning Today Are Testing Everything**


The most forward-thinking platform teams at KubeCon all embraced:


- automated cluster upgrade testing
- traffic replay
- digital twin environments
- service mocking
- golden path validation
- policy evaluation tests
- scaling + load simulations
- SLO-driven quality gates


They treat their platform as a product — and test it like one.


---


# **Final Reflection**


KubeCon made something very clear:


**Platform testing is the responsibility no one wants — but the responsibility every successful team embraces.**


The teams who shrugged and said *“Not my problem”* were the ones dealing with outages and developer complaints.


The teams who invested in testing — especially using **traffic replay** , **service mocking** , and **digital twins** — were the ones shipping reliably, confidently, and calmly.


The future of platform engineering isn’t just paved roads.


**It’s paved, tested, simulated, replayed, and trusted roads — validated without needing to write a single line of application code.**
