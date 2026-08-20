---
schema_version: "1.0.0"
document_id: "42a844f457936a60a6701b6cfe24725245c46b21bf498491f07daf95660f0b72"
company_key: "yc-saphira-ai"
company: "Saphira AI"
source_id: "yc-saphira-ai-news-import-f22693f6988e"
canonical_url: "https://www.saphira.ai/blog/designing-a-construction-robot-around-certification"
published_at: "2026-07-19T00:00:00+00:00"
first_seen_at: "2026-07-24T00:45:53.984427+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:b556bb2d1d974c66aec037952a001f1007757708dc3d3b3b942e38c9991fc265"
---

# Designing a Construction Robot Around Certification

**How Raise Robotics turned machinery safety standards into an engineering roadmap — and made safety a product asset instead of a retrofit.**


---


Building an autonomous construction robot means solving far more than autonomy. Raise Robotics' mobile manipulator — an autonomous base, a collaborative arm, interchangeable tooling, a lift system — has to operate on open jobsites, alongside people, with no fencing, across teleoperated, manual, and fully automatic modes. Every decision about sensing, braking, motor control, and software separation has certification consequences, and those consequences are scattered across thousands of pages of standards: ISO 12100, ISO 13849, ISO 10218, ISO 3691-4, ANSI/RIA R15.08, UL 3100, IEC 60204-1, IEC 61508, and more.


The question Raise faced wasn't "how do we write safety documentation?" It was "what exactly do we have to build — and buy — to certify this robot, and which of those decisions do we need to freeze now?"


## From kickoff to a working risk assessment in one day


The engagement kicked off in early July 2025. Within a day of the kickoff, Saphira had shared a preliminary risk assessment for the robot; within the first week, a structured compliance intake had captured the system's operating envelope — modes, speeds, power architecture, sensing, human interaction — and converted open certification questions into concrete engineering decisions: Is there a manual/autonomous mode selector? What stop category do the brakes achieve? If the LiDARs aren't safety-rated, what's the alternate detection architecture?


> “We had a preliminary risk assessment we could actually work from within a day of kickoff, and a structured picture of our whole operating envelope inside the first week.”
> — Rishabh Aggarwal, Raise Robotics


By the second week, the collaboration was already producing design guidance, not paperwork. When Raise raised the question of how to safely operate a drill end-effector near people, Saphira's response was a concrete architecture: guard the tool with a retractable or IR-based end-of-arm guard, interlock base and arm motion so only one moves at a time, inherit the certified cobot's power-and-force-limiting function, and rely on safety-rated *stop* rather than safe slow-down — deliberately avoiding the cost and complexity of a full safety-rated encoder and motor-control channel. Certifiable, without a safety bubble that would make the application impossible.


## A risk assessment built for engineers — and investors


Over the following months, Saphira built out a complete ISO 12100 / ISO 13849 risk assessment covering every mode of the robot's life: transport, teleoperation, manual mode, automatic mode, and maintenance.


The result: **a comprehensive hazard assessment spanning** motion, electrical, thermal, chemical, and stability risks — with only a small subset requiring the highest performance level (PL e), concentrated exactly where you'd expect for a mobile manipulator: automatic arm motion with tooling, high-force impacts, and thermal events. The engineering conclusion was clear and confidence-building: the robot presents no unmanageable risks, and its risk profile matches global best practice for construction and industrial mobile manipulators.


That assessment did double duty. It drove the safety architecture — and it went in front of investors:


> "I was hoping to get a risk assessment I could show investors — what I got was rigorous enough to prove our engineering maturity, not just tick a box."
> — Rishabh Aggarwal, Raise Robotics


For an early-stage robotics company, a rigorous, standards-based risk assessment isn't just compliance evidence. It's proof of engineering maturity.


## Freezing the right hardware first


Rather than treating the whole BOM at once, the work was sequenced around what was most expensive to get wrong. The architecture review deliberately focused first on the safety PLC and the safety sensing (particularly the LiDARs) — the costliest and hardest-to-integrate components in the system, and therefore the ones most critical to freeze early.


Along the way, Saphira functioned as an extension of Raise's engineering team on component selection:


- Vetting safety contactors, motor drivers, and IEC 61800-rated drives for use in the functional safety chain, including how to bring non-safety-rated components into a certifiable architecture via the safety PLC
- Auditing certification status of critical components — the cobot arm, LiDARs, battery system — and flagging where claimed certifications (e.g., UL 1740 electrical-only) didn't cover functional safety
- Defining a path to PL d human detection using non-safety-rated sensors, via OSSD outputs, monitoring boards, and safe communication (IEC 61784-3 black channel)
- Sourcing support for safety-relevant compute and sensing, with direct introductions to suppliers including NexCOBOT, RealSense, and Synapticon


## From risk assessment to a certifiable system specification


The risk assessment then became the engine for the Safety Requirements Specification. Requirements were derived systematically from every PL c-and-above hazard — a scoped set of roughly 85 requirements — and loaded into Saphira's platform, where the Safety Case Traceability Builder's ISO 13849 mode links each hazard to its requirements, safety functions, and verification activities.


Rishabh summarized the goal in a way that became the project's north star:


> "Saphira worked like an extension of our engineering team — every certification question turned into a concrete design decision instead of another pile of standards to read."
> — Rishabh Aggarwal, Raise Robotics


Alongside the SRS, Saphira delivered the formal ISO 13849 Safety Functions analysis — performance level calculations (MTTFd, DCavg, CCF) for each safety function — structured as the document a certification body like TÜV or Intertek will actually review, plus a UL 3100 checklist and critical-components list, and a concrete NRTL certification roadmap: a 17–20 week path from safety function validation through preliminary design review, assessment, testing, and final reporting.


When it came time to engage labs, Saphira brought its network: introductions and evaluations across TÜV SÜD, TÜV Rheinland, UL, Exida, CSA Group, and MET Labs (Eurofins) for specialized testing such as ATEX.


## The outcome: certification as a design input, not a post-mortem


Over twelve months, the engagement changed how Raise builds robots. Certification questions stopped being research projects and became design reviews. The conversation shifted from *"which standard should we read?"* to *"which requirement does this design decision satisfy?"* — with a standing weekly cadence that, tellingly, Raise increasingly didn't need because the major architectural questions had answers.


> “By the time we started scoping our next product, certification wasn’t a research project anymore — it was just part of the design review. We knew what to freeze and why.”
> — Rishabh Aggarwal, Raise Robotics


The clearest evidence came after the core engagement wrapped: as Raise began scoping its next product variant, its first move was to bring certification into the conversation at the concept stage — before the design was frozen.


That's the transformation: certification planning moved from the end of the development cycle to the beginning of it — for this robot, and for every robot Raise builds next.


---


## By the numbers


- **1 day** from kickoff to first working risk assessment draft
- **5 operating modes** analyzed: transport, teleop, manual, automatic, maintenance
- **~85 requirements** derived from PL c+ hazards into a certifiable system SRS
- **12+ standards** decomposed into engineering requirements (ISO 12100, ISO 13849-1/-2, ISO 10218, ISO 3691-4, ANSI/RIA R15.08, UL 3100, IEC 60204-1, IEC 61508, IEC 61784-3, IEC 61000 series, and more)
- **17–20 week** NRTL certification roadmap defined
- **6 certification labs** evaluated and engaged through Saphira's network
