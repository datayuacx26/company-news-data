---
schema_version: "1.0.0"
document_id: "9fae8ae721d424e6f62466a58be85b9645194b348b0af9b799b2fad5463fab97"
company_key: "ge-healthcare-technologies-inc-common-stock"
company: "GE HealthCare Technologies Inc."
source_id: "ge-healthcare-technologies-inc-common-stock-rss-f82d3b59c758"
canonical_url: "https://research.gehealthcare.com/in-the-device/the-real-time-hospital-concepts-for-resilient-edge-ai-for-neonatal-and-anesthesia-care-jb37792xx/"
published_at: "2026-04-24T19:41:58+00:00"
first_seen_at: "2026-07-20T03:33:19.174351+00:00"
fetched_at: "2026-07-28T20:52:06.558365+00:00"
content_hash: "sha256:3101656e365ad158091a31c96908f074fc5e13936d85794a69792c23a5ff6d85"
---

# The real-time hospital: concepts for resilient edge AI for neonatal and anesthesia care

Imagine a nurse taking over care of a newborn At the start of a shift in a neonatal intensive care unit. The nurse’s shift begins with a routine handoff and a careful check of positioning, lines, equipment connections, alarm limits, and emergency readiness. This helps establish a baseline in a setting where a lack of preparation can carry serious consequences. The work can become fast paced, and nurses are often responsible for more than one infant. They work to keep the environment as quiet as possible, among the cacophony of necessary equipment like monitors, ventilators and regular conversations. The nurse moves between tasks, providing hands-on care, adjusting breathing support, giving medications and fluids, charting as often as every 15 minutes, and responding to colleagues and parents while staying focused on each infant’s condition.


In the operating room, the setting changes, but similar problems can emerge during a normal procedure. An anesthesiologist synthesizes vital signs, delivery of therapy, checks equipment, documents patient treatment and responses, and coordinates with a team moving around a sterile field. The work is continuous, and the margin for delay is going to be small.


**What GE HealthCare and NXP are building together**


Recently, NXP Semiconductors and GE HealthCare announced a collaboration to push edge AI deeper into hospital acute care settings. NXP brings secure, high-performance edge processing designed for real-time workloads, including applications processors with integrated neural processing units and the eIQ AI Toolkit. GE HealthCare brings deep experience developing bedside and operating room technology that clinicians rely on every day.


We are exploring two concepts for on-device intelligence in high-acuity care: the first aiming to support neonatal care through intelligent, live monitoring, and the second aiming to enable hands-free, voice-guided support for anesthesia workflows in the OR. In both cases, AI runs on or near the device to enable low latency, resilience to network variability, and local handling of raw data, subject to final design and testing.


**Five workflow bottlenecks edge AI is designed to address**


This work is easiest to understand as an effort to address five bottlenecks that have limited what AI can do in acute care. The intent is to make on-device intelligence fast enough, resilient enough, and secure enough to support real clinical workflows.


*1.* *Time-to-recognition: turning continuous streams into usable signals*


In acute care, the difference between early and late alerts can be seconds. The challenge is converting high-frequency inputs into a small set of time-stamped, high-confidence events that arrive fast enough to support clinician action. That requires an end-to-end pipeline with a defined latency margin predictable behavior under load, and outputs that are easy to verify by clinicians.


In the NICU, we are exploring continuous, contactless video as an additional input. Frames would be processed on-device in real time, with local vision models generating candidate triggers using thresholds and confidence checks. Rather than exposing raw scores, the system would surface a short list of clear, reviewable events such as crying, posture suggesting repositioning, or a foreign object in the warmer. It also maintains a configurable short-term state to suppress repeats and avoid excess notifications during routine occlusions.


In anesthesia, we are exploring speech as the primary interaction with the device, which would combine with device status and physiologic data in the room. A low-latency intent pipeline would capture audio, converts it to text or intent, map the request to the correct data source, and return a concise response by voice or screen. Requests that could change device behavior are aimed to deliver features like safety gating with confirmation, limits on parameter changes, and logging for traceability.


*2.* *Network uncertainty: strict latency and degraded connectivity are normal conditions*


Cloud computing remains important for training, updates, and fleet-level analytics, but the clinical loop in the NICU and OR cannot rely on low-latency connectivity. Delayed alerts risk being ignored, and slow voice responses reduce usability. Continuous high-bandwidth streams, especially video, are also costly to transmit.


Edge execution can keep the AI decision step local. Inference runs on the device, avoiding network round trips and enabling faster response. It can also support data minimization by converting raw video or audio into discrete events or summaries instead of transmitting continuous streams.


Given real-world network variability, we are exploring a hybrid design: the device handles immediate decisions, while the cloud supports training, updates, and broader analysis.


*3.* *Interaction friction: reducing taps, screens, and context switching*


In the OR, small inefficiencies can compound quickly. Clinicians frequently move between displays, controls, and documentation while maintaining awareness of the patient. The anesthesia work is intended to focus on hands-free interaction so routine queries and selected tasks can be completed without breaking focus, subject to safety controls and testing.


A tiered model approach may help keep the interaction fast and predictable: a small model can handle routine requests locally, and a larger model is used only when the question truly needs it. The local model can route intent, answer structured questions about device status, and manage confirmations. Larger models, off device when needed, can be reserved for broader reasoning, richer summarization, or deeper cross-system context.


*4.* *Alert burden: selective prompting instead of more alarms*


In the NICU, alerts can be a potential source of caregiver interruptions. For monitoring to help, it has to be prioritized. That is why we are separating continuous perception from action: run continuously, but surface only the small set of actionable triggers, and do it in an intentionalway.


We are exploring using an overhead camera focused on a neonatal radiant warmer, with the video stream processed locally on an on-premises edge device. Core execution is intended to run on NXP’s i.MX 95 system-on-chip. For more demanding workloads, the design may use NXP’s Ara-2 discrete neural processing unit.


The intended design includes multiple convolutional neural network models running simultaneously on the same incoming frames for different tasks. One set can focus on detecting the infant and potential identify state of awake or sleep, while another looks for foreign objects in proximity to the infant. Triggers can prompt clinician review depending on risk and context.


To help ensure patient privacy is at the front and center of our design, we are exploring designs that keep processing on device, with no raw image transmission off the device and no persistent storage of images or video.


*5.* *Trust and validation: proving performance in the real workflow*


Demonstrating improved outcomes will depend on measurable performance in real conditions. Latency should be measured end to end. Reliability should be tested under degraded connectivity and realistic hospital conditions. False prompts and missed events must be quantified because they influence whether the system builds trust or gets ignored. Human factors work should verify that outputs fit into routines without increasing cognitive load or interrupting care at the wrong time. Security and privacy should be specified operationally, including what is retained, how access is controlled, how software is updated, and how failures are handled.


Because these concepts are intended to reduce a growing workload, evaluation should also include day-to-day operational metrics. For NICU monitoring, that could include insights like prompt volume per hour, the fraction of prompts acknowledged or acted on, suppression rates when prompts are repetitive, and performance under lighting changes, occlusions, and routine care activity. For anesthesia workflows, insights could include response time from spoken request to usable answer, task completion rate in a noisy room, and the rate of misrecognition or ambiguous intent that forces clarification with the device.


Taken together, this work reflects a shift toward AI that operates within the realities of clinical care, where timing, reliability, and clarity matter more than theoretical capability. By keeping intelligence close to the bedside and the operating room, the aim is to support clinicians without adding new layers of complexity or distraction. The measure of success will be simple: whether these systems fit into the rhythm of care and help clinicians act with greater confidence in the moments that matter most.
