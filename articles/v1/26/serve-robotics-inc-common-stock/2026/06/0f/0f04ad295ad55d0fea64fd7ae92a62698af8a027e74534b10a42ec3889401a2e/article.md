---
schema_version: "1.0.0"
document_id: "0f04ad295ad55d0fea64fd7ae92a62698af8a027e74534b10a42ec3889401a2e"
company_key: "serve-robotics-inc-common-stock"
company: "Serve Robotics Inc."
source_id: "serve-robotics-inc-common-stock-news-import-b3bd6cb23a99"
canonical_url: "https://www.serverobotics.com/blog/serve-robot-safety-standards/index.html"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:11:37.118741+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:cb39aff70a08a7ca33c07b106cb0f767ca9826524057acb22cf21c81bd2dec31"
---

# How Serve Robotics Builds Safety Into Sidewalk Delivery Robots

Back


# Safety at Serve, from Principle to Practice


S


a


f


e


t


y


a


t


S


e


r


v


e


,


f


r


o


m


P


r


i


n


c


i


p


l


e


t


o


P


r


a


c


t


i


c


e


Article


June 29, 2026


Safety at Serve, from Principle to Practice


Safety at Serve, from Principle to Practice


Article


## How our commitment to a core value drives every decision we make


**By[Jake Streeter](https://www.linkedin.com/in/jake-streeter-a37b9217b/) , Head of Safety, Serve Robotics**


As an early leader in the delivery robotics space, Serve is continuously refining how we operate in busy, ever-changing sidewalk environments alongside pedestrians, cyclists, animals, and more.


In addition to building capable technology, safe delivery in real-world settings requires thoughtful operations and oversight. That’s why we’ve developed a rigorous, systematic approach to safety that extends from the robots’ design to the culture of the organization building them.


Safety anchors everything we do at Serve. This article explores, in detail, how that commitment is implemented: across our engineering practices, autonomy stack, operational infrastructure, incident response systems, and the company as a whole.


## The Science Behind the Standard


"Safety" is an easy word to say but a difficult concept to measure. If harm is not defined in precise, testable terms, it becomes challenging to engineer against it. To address this issue, we decided to draw on principles from injury epidemiology, a trusted framework that originated in the medical and transportation safety fields, to better define what constitutes safe operating conditions.


Injury epidemiology studies the causes and mechanisms of injury—specifically, how energy is transferred between an external source and the human body. This framework was developed in part by Dr. William Haddon Jr. at the U.S. Department of Transportation, and became foundational to modern vehicle safety systems such as seatbelts and airbags.


Injury epidemiology provides a natural framework for systematically evaluating whether our robots can cause harm. At its core, it focuses on three types of energy transfer:


-


***Kinetic energy* (collision):** We engineer to minimize collision risk at every level, including the robot's weight, maximum speed, deceleration capabilities, and proximity rules built into its operating policy.


-


***Thermal energy* (heat):** We design and validate the system to ensure that external surfaces do not reach temperatures that could cause burns under foreseeable conditions, including in hot weather and during extended operation.


-


***Electrical and chemical energy* (shock):** We incorporate multiple layers of protection within the battery and electrical to prevent hazardous energy transfers through the robot's exterior.


Nearly every design specification at Serve, from deceleration rates and sensor detection thresholds to surface temperature limits and battery safety standards, traces back to our injury epidemiology framework. Injury epidemiology serves as the bridge between the principle of “do no harm” and the concrete engineering decisions that make the principle real.


## Robots Designed for Sharing the Sidewalk


Sidewalks exist for pedestrians who walk at varying speeds, make unpredictable movements, travel in groups, push strollers, use wheelchairs, and walk dogs. Any robot moving through that environment must be designed accordingly. We build our robots around human behavior, continuously refining how they move and respond based on real world experience.


We designed our robot’s appearance to reflect its purpose: operating in a friendly and considerate way. Its expressive eyes help signal intent in shared spaces, similar to how people rely on eye contact when passing one another. For visually impaired individuals, the system aligns with ADA guidance, including height specifications informed by established accessibility standards.


Early on, we observed that pedestrians infer the robot’s direction by watching its wheels pivot, much like they read a person’s intent by watching their head turn. The robot's dual-axle steering system makes those directional cues clear and noticeable.


We also calibrated the robot's deceleration to align with the kinematic profiles of other sidewalk users. Pedestrians, bicycles, and even vehicles all decelerate at roughly 0.5g, and our robot is designed to match that profile. As a result, when our robot needs to stop, it does so with a familiar level of urgency.


When encountering signals like stop signs, people need time to respond: typically less than a second when focused, and up to two seconds when distracted. Our system is designed to significantly exceed human perception-reaction time, providing an additional safety margin in dynamic conditions.


Finally, to perceive its environment, the robot utilizes a wide array of sensors, including LiDAR, cameras, GPS, and inertial measurement unit (IMU). These complementary systems continuously work with one another to provide overlapping and independent coverage. The robot is also equipped with bright headlights and LED signaling to help communicate its presence and intent to the people around it. In certain markets, additional features such as physical safety flags or audio signaling may also be used.


## A ‘Risk-Aware’ Representation of the World


We built our technology around a core principle: safety comes first. Our autonomy stack reflects that priority at every layer.


Safety begins with our Operational Design Domain (ODD). Our routing system incorporates risk directly into deployment decisions, operating only in environments where the system has been designed and validated to perform safely. As robot capabilities improve, we expand the ODD through a continually learning routing analytics layer that informs our mission guidance system. This ensures robots are deployed and navigate with a safety-first approach. Our onboard autonomy system combines end-to-end learning with rule-based safety constraints. Learned models power perception, while a risk-based representation informs navigation behaviors and enables predictable maneuvers. A physics-grounded, rule-based guardrail system ensures the robot consistently operates within defined safety limits.


To support perception, we integrate LiDAR, cameras, and additional sensors to interpret the environment across a wide range of conditions. This multi-sensor approach provides redundancy and improves system reliability in real-world operation.


Our system also combines real-time perception with mapping where available. These maps can capture contextual features such as construction zones and enclosed spaces, and are used alongside live sensor data and learned models to inform how the system behaves and makes decisions.


By fusing environmental context with real-time situational awareness, the robot maintains a continuously updated understanding of its surroundings, while actively managing safety risk.


## The Relationship Between Speed and Safety


Lanes, signage, and traffic laws provide clear structure to guide driver behavior on roadways. Sidewalks, by contrast, are far less structured. As a result, defining how a robot should operate on them required us to develop our own framework.


Our approach is probabilistic and risk-based. Rather than simply asking, "what is directly ahead?”, the robot evaluates the risk distribution of its environment and determines the safest path forward.


We designed a perception-to-behavior framework centered on the relationship between risk and speed. By decoupling these two elements, we were able to systematically relax navigation constraints and encode a key human intuition: move efficiently when conditions are safe, and slow down when uncertainty or risk increases. This allows the robot to operate quickly in open areas while appropriately reducing speed near blind corners or in high pedestrian traffic areas.


## Calibrating to Each Community


Every city Serve operates in has its own sidewalk conditions, pedestrian density, behavior patterns, and physical infrastructure. Atlanta sidewalks are not Los Angeles sidewalks. Chicago presents challenges such as snow and ice, while Miami experiences frequent afternoon thunderstorms.


Before launching in a new location, we study the operational environment, conduct mapping where applicable, and stage deployments to build confidence incrementally. We work directly with city departments, such as transportation agencies and policy teams, to align on standards and reporting protocols before a single robot hits the sidewalk. Ongoing check-ins, operational reporting, and direct communication help ensure our approach remains aligned with each city’s needs.


Safety extends beyond the robot itself to the systems supporting it. Trained supervisors are available during operations to step in when needed, providing an additional layer of oversight in complex or unexpected situations.


## The Architecture of Accountability


No autonomous system operating in the real world will achieve a perfect safety record. The key question is not whether incidents occur, but whether an organization can detect them, respond effectively, understand their root causes, and reduce the likelihood of recurrence.


Before a robot is deployed in a city, our team works to proactively manage risk through a structured Safety Risk Management (SRM) process. We identify hazards in advance, test and analyze the robot’s behavior in controlled conditions, assess associated safety risks, and define acceptable thresholds for risk acceptance. Risk is then continuously monitored and reassessed over time.


When an event occurs, it is managed through our Emergency Response Plan (ERP) based on its severity and context. In lower-severity situations, such as when the robot identifies that it is in an untraversable or safety-constrained state, the system flags the condition and a remote supervisor provides corrective guidance without escalation.


In higher-severity cases, such as when the robot detects an impact, the system immediately escalates the event with supporting context and reasoning. This is first reviewed by a remote supervisor and, if necessary, escalated to the incident response team. The team then assesses the situation and determines whether intervention, including physical retrieval is required.


From there, the incident is triaged by a cross-functional team to ensure the safety risk is addressed across all relevant stakeholders without introducing additional risk. Before resolving the case and re-deploying the robot, the team conducts a root cause analysis, implements any necessary mitigations, and re-evaluates risk acceptance thresholds.


We also monitor incidents across the broader industry, including those involving other robotics companies. When such events provide meaningful lessons, we work to understand and incorporate those learnings into our own safety program.


## A Federated Approach to Safety at Serve


When people talk about robot safety, they often focus on the robot itself: its sensors, software, and hardware. But at Serve, we take a broader view: a safe product cannot exist without a safe company to build it.


We operate under a Safety Management System (SMS), the gold standard for safety in the transportation industry. Our SMS consists of four components: Safety Risk Management, Safety Assurance, Safety Policy, and Safety Promotion.


Each of these components is integrated across the organization in order to both support our culture and enable safer products and services. For example, all our employees follow a set of companywide safety policies and emergency response protocols. We also maintain dedicated processes for reporting and escalating safety concerns so they can be reviewed and addressed through the appropriate channels.


Safety performance is also regularly reviewed through internal processes and cross-functional oversight, ensuring that lessons learned are incorporated into both our technology and operations. This closed-loop approach allows us to continuously identify risks, implement improvements, and strengthen our safety culture over time.


Across the organization, we take a federated approach to safety. Our SMS establishes centralized safety standards, while empowering individual stakeholder groups—across engineering, policy, and operations—to manage their domain-specific safety responsibilities.


## Beyond the Last Mile


Safety drives everything we do at Serve, from our products and services to our organizational systems. It’s the foundational requirement for building our business, and the only path to delivering meaningful, positive impact.


But there's a broader reason we prioritize safety as our guiding principle. As part of an emerging industry, we have the opportunity to help define the safety standard for last-mile delivery robotics. Serve aims to demonstrate not only what robots can do, but how they can operate as safe, reliable participants in their communities.


*This article was originally published on[LinkedIn](https://www.linkedin.com/pulse/safety-serve-from-principle-practice-serverobotics-zddkc/) .*
