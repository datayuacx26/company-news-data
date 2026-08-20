---
schema_version: "1.0.0"
document_id: "b43ed8860d4d3eecc8748d0fbf787c4e927b1a5b6d5ec60e32e9cc4463a6e90b"
company_key: "vtex-class-a-common-shares"
company: "VTEX"
source_id: "vtex-class-a-common-shares-rss-fffff04a1e70"
canonical_url: "https://vtextech.substack.com/p/best-practices-for-leadership-in"
published_at: "2024-11-26T12:30:07+00:00"
first_seen_at: "2026-07-20T23:19:02.969240+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:b8e51dd363cc6821106f32659af5705a724d37766a2ce27c68c5ae49a8dc1f86"
---

# Best Practices for Leadership in Critical Incidents

# Best Practices for Leadership in Critical Incidents


### Empowering people to lead Critical Incidents


[Ygor Santos](https://substack.com/@ygorsantos)


Nov 26, 2024


With over 12 years of experience in software engineering and large-scale distributed systems, I have had the opportunity to participate in various incidents in different roles—observing to learn, assisting a senior colleague, being responsible for a specific workstream, or leading the incident itself. Recently, I have been given more opportunities to lead critical and complex incidents where leadership has been a key factor in (1) bringing tranquility to the people involved in the mitigation process, (2) ensuring clear and precise communication to all stakeholders, and (3) making hard decisions.


*“Resolving an incident means mitigating the impact and/or restoring the service to its previous condition. Managing an incident means coordinating the efforts of responding teams efficiently and ensuring that communication flows between the responders and those interested in the incident’s progress.” ( [Google SRE - Incident Response chapter](https://sre.google/workbook/incident-response/) )*


There are different incidents that vary in terms of their level of diagnostic difficulty, mitigation complexity, and business risk. I want to define a


**critical incident** as a type of incident where the business impact is high, putting it at risk—whether due to an instability affecting all customers or the need to address a specific strategic client.


Due to the business risk, this type of incident is typically accompanied by the involvement of company leadership, such as C-level executives or directors, whether technical or not, who want to monitor the situation closely. Stakeholders are anxious and uncertain, and the engineers are under pressure to resolve the problem as quickly as possible.


In this post, I want to share experiences and best practices to help you in situations where you take on a leadership role in


**critical incidents** :


1.


Control communication with clarity and precision


2.


Open new workstreams


3.


Care for people


4.


Tips for making hard decisions


## Control communication with clarity and precision


Communication is the oil in the gears of an incident, enabling all parties to connect with each other with minimal friction. Generally, there are three groups we need to communicate with: (1) The engineering team working on the problem, (2) The customer support team, which is under pressure for responses, updates, and the elusive "root cause," and (3) The company’s leadership, such as C-level executives and directors, who are also under pressure, seeking answers and wanting to know how to influence and contribute to resolve the issue without delay.


Without clear and precise coordination and communication, stakeholders lack certainty, and instead of helping, they end up hindering each other. If a C-level executive is not addressed through proper communication, they may approach the engineering team directly with the intent of helping. However, this could be perceived as an additional pressure source, creating the opposite effect despite their good intentions. If the customer support team lacks answers, the client may feel that the team is lost and unsure of what to do, which contributes negatively to long-term customer retention and may also lead the client to escalate the issue to the company's C-level team.


Therefore, a healthy balance and a communication process that meets everyone's needs are essential. Below, I outline some tips:


-


*Establish an official communication channel* → In some companies, there is a defined incident response process. However, during a critical incident, we may face a scenario where stakeholders join the engineering meeting to follow the discussions more closely. It is important to emphasize to this group that any information discussed in the meeting is not official unless explicitly stated and that all information is subject to change as new evidence emerges. Whether through a document or a chat service, make it clear that they should wait for the official message that can be used for external communication.


-


*Establish a healthy update frequency →* Once we have established an official communication channel, we must align on an update frequency. The goal is to manage stakeholder anxiety regarding updates about the incident. If the frequency is too low, it may not allow enough time to receive responses from engineering about investigation progress and mitigation actions. On the other hand, if it is too high, stakeholders may not respect this limit and might join engineering meetings seeking unofficial preliminary information.


-


*Always anticipate whenever possible* → The agreed update frequency is a maximum interval, not a minimum. Therefore, if a relevant update contributes to keeping stakeholders informed about the incident, provide it whenever possible.


### Write good summaries (tl;dr)


This is one of the most challenging tasks for the person leading an incident. A good summary and update protect the engineering team, allowing them to stay focused with less external pressure. Conversely, poor communication can make leaders feel insecure and prompt them to step in to help. It is important to emphasize that having leadership close to the response is not inherently problematic.


> ```text
> tip: A good incident leader leverages leadership involvement to remove obstacles and gain additional support. However, this kind of contribution can be achieved through effective communication rather than stemming from their insecurity.
> ```


-


*Clarity and Precision* → Avoid vague and imprecise communications that raise more questions than they answer. Remember, stakeholders need to feel confident and reassured or to understand the severity of the problem as quickly as possible. Phrases like “We’ve identified the problem and are working on it” can be replaced with: “Based on our observability, we have identified issue X, which we believe to be a significant contributing factor to the incident. We will take action Y and assess the impact of this change in our monitoring. Other workstreams are underway in case this action does not have the expected effect.”


-


Why is this adjustment so significant?


-


Because if the next update does not provide a solution to the problem, confidence in the team will decrease. What ensures that the next "resolution" update will actually mitigate and solve the issue?


-


Because a new contributing factor may emerge when addressing this problem. If that happens, we will enter another round of solution-seeking.


-


Because the team is not waiting idly for one action but is proactively working, trying to anticipate new contributing factors that might be discovered until they are properly mitigated.


-


*The order of information matters* → Always start with the most important point for the stakeholders you are communicating with. Typically, they want to know: (1) Are we managing to mitigate the problem? and (2) Do we actually know what happened? A good incident leader will know how to organize information precisely for the incident audience. When dealing with a C-level audience, they often handle so many different issues that they do not have time to delve deeply into everything. Therefore, a well-structured summary can provide them with the necessary context without further involvement.


-


*A well-defined structure* → Standardization fosters proper understanding. Consistently responding in the same format can help educate stakeholders on processing the incident information.


## Open new workstreams


When dealing with an incident, whether critical or not, you are in a race against time to mitigate it. As the first hypotheses emerge, the group will naturally focus on mitigation and identifying contributing factors. As the incident leader, you need to stay ahead, considering the possibility that the approach the team is betting on to resolve and/or mitigate the issue might not work.


**What is a workstream?**


It is an initiative to validate a hypothesis or mitigate part or all of the problem itself. Its scope is well-defined, preventing the workgroup from becoming unfocused.


Define the main workstream and assign a leader to it. If possible, include an experienced technical person with context outside of the group to work with you, and ask the following questions:


-


Once mitigation action X is implemented, what problem might arise next?


-


And if this is not the problem, what would the new hypothesis be?


-


And if the system doesn’t recover, is it possible to implement any activity to operate without it (


*graceful degradation* )?


-


Is it possible to reduce the global outage to a percentage of customers?


In my experience, I have repeatedly observed the need for improvements in observability. Critical incidents where the engineering team cannot identify the cause often point to a blind spot in the architecture. It is common to have the following workstreams:


1.


Working on the main hypothesis for mitigating the incident.


2.


Identifying and addressing another hypothesis based on the questions above.


3.


Improvements in observability for existing components or blind spots.


> ```text
> Tip: If new hypotheses make sense, don't hesitate to open new workstreams.
> ```


## Care for people


Software engineering is not manual and repetitive work but a creative one. The more fatigued a person becomes, the lower their ability to make the best decisions and execute actions. In


**critical incidents** , you will see engineers proposing manual actions that may have never been carried out before. People need to be in a healthy condition to perform these actions.


How to take care of people during an incident?


1.


*Be mindful of the time elapsed during the incident* → Long hours during an incident can be very exhausting, especially when the people involved have already been on call during the day, for example. Consider taking breaks to breathe, have a snack, drink some coffee, or rotate team members.


2.


*Avoid extending into the night and early mornings as much as possible* → Be very cautious with actions taken during the night and early mornings. If the team has managed to mitigate, for instance, and wants to proceed to address other contributing factors, consider resting and continuing the next day. Remember, as the incident leader, you need to consider the scenario where the action might fail, leading to more work hours for a team that is already fatigued and sleep-deprived.


3.


*Perform checks with the people* → Ask the team if they are okay, and emphasize the importance of resting and eating. Share this decision with them, but if you notice that someone is already very tired, do not hesitate to make a decision. You are the incident leader; reduce the risks of the situation worsening. Some people even forget to eat due to the adrenaline of the incident.


Once, I was involved in a critical incident, and the problem had been partially mitigated. Testing needed to be performed during a low-traffic period, meaning during the night. We deployed and tested, and everything went well. However, I suggested that the team roll back to the partially mitigated version to ensure they could rest with peace of mind. Imagine if a side effect in the version arose during the night or early morning. It’s better to face that moment with a rested team. We aligned with the client, carried out this maneuver, and deployed safely during the day with a well-rested team.


> ```text
> Tip: People are the key agents in resolving incidents—take care of them!
> ```


## Tips for making hard decisions


This is the part that causes the most anxiety for people in the role of incident leadership. The problem is present, there’s pressure, many stakeholders are demanding answers, and the system in production is affected. The team, very concerned and aware of the severity, defines the action to be taken but feels uncertain. Therefore, they decide to share this decision-making process with you.


First of all, you are not a robot. Feeling fear, anxiety, and worry is normal. What cannot happen is letting fear paralyze you. How can you avoid that? Through self-awareness and experience. An exercise I practice to keep myself from being paralyzed is:


*"If everything goes wrong, let it be because I took action to try to solve the problem, not because I was paralyzed by fear!"*


Once you are ready to make decisions, consider:


1.


***The team has the answer.*** → In 99% of cases, the team has the answer; but the person is unsure and wants to validate their action while considering the impact. Instead of trying to make the decision for them and pursue the same context, focus on asking the right questions so they can arrive at the answer themselves.


2.


***Stress the action through the lens of risk*** → The questions should always be framed from the perspective of what side effects an action might cause, whether it could worsen the situation or create a new problem.


3.


***Involve people from other areas*** → There are cases where the right questions won't come from you but from another team. Consider involving people from strategic teams, such as the security or infrastructure teams. These individuals will be able to assess the situation more deeply than you.


4.


***Understand the problem and the action*** **→** Even if you are not the person who will ask the best questions or have the most context, it is essential to understand the problem. Ask the team to explain it in a way you can grasp, usually by asking clarifying questions or summarizing what you have understood so far. This will even help the team organize their thoughts about the action.


Do not shy away from the responsibility of understanding what is happening. Even if you don’t have the depth of knowledge of a specialist, grasping the situation is crucial so you can explain what is happening and its expected impact. Be mindful of the team’s time when explaining things to you. If you feel you need more context to understand the action and it’s taking too long, designate one person from the team for this task and allow the others to continue working on their respective workstreams.


> ```text
> Tip: You need to have a reasonable level of understanding to lead the actions without diving deep into the details.
> ```


## Conclusion


Taking on the role of leadership during incidents is highly challenging. As a leader, you need to think about what the team isn't considering, anticipate scenarios, act quickly in decision-making, and leverage the team itself to find the best solutions. You should also use techniques like


[command and control](https://en.wikipedia.org/wiki/Command_and_control) to keep the team focused on solving problems.


A common mistake is believing that the leader needs to have all the answers or be the one to pull a rabbit out of the hat, just like a magician. In reality, the magic lies in keeping the team focused, with the appropriate sense of urgency, asking the right questions to the group, and genuinely caring about everyone’s well-being. I hope the tips in this article can assist you in these moments.


## Acknowledgments


Thanks to the special contributors to this post,


[Fabiana Fonseca](https://open.substack.com/users/152054829-fabiana-fonseca?utm_source=mentions)


,


[Bruno Dias](https://open.substack.com/users/40396196-bruno-dias?utm_source=mentions)


and Sheila Gomes.
