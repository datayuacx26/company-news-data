---
schema_version: "1.0.0"
document_id: "bc747b6f101911151044314a7ac922f4419d09df9d257045515595dca38bb189"
company_key: "yc-hypercubic"
company: "Hypercubic"
source_id: "yc-hypercubic-news-import-1849a5c8a5f4"
canonical_url: "https://www.hypercubic.ai/insights/introducing-hopper-an-agentic-development-environment-for-the-mainframe"
published_at: "2026-05-12T23:30:00+00:00"
first_seen_at: "2026-07-21T23:26:19.477967+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:92a52a855b83f5f645d844df0749949f50b926309119ab948ac0c283609a8eb1"
---

# Introducing Hopper: An Agentic Development Environment for the Mainframe

Mainframes remain one of the most important computing environments in the world.


They process payments, run core banking systems, manage insurance claims, support government operations, power airline reservation systems, and hold decades of business logic that large institutions still depend on every day.


Yet the primary interface to many of these systems is still TN3270: a terminal protocol built around structured screens, function keys, ISPF panels, datasets, jobs, spool output, return codes, and operator-driven workflows. It is reliable, precise, and deeply embedded in how mainframe teams work.


But it was designed for expert humans, not AI agents.


Today, we are introducing Hopper, the first agentic development environment for the mainframe.


Hopper lets developers use AI agents to work directly inside mainframe environments across TN3270, ISPF, JCL, JES, CICS, VSAM, datasets, jobs, spool output, and return codes.


It is available now on Windows, Linux, and macOS.


[Download Hopper →](https://www.hypercubic.ai/hopper)


## **The bottleneck in mainframe development**


Modern software development has become increasingly agentic.


AI systems can read code, edit files, run tests, explain failures, navigate repositories, and operate across local and remote environments. In modern software, agents are beginning to work inside the same environments developers use every day.


Mainframe development has not had the same interface shift.


A typical workflow still requires developers to move manually across terminal screens, ISPF panels, datasets, members, jobs, JES output, return codes, and system-specific conventions. Much of the context required to complete a task is distributed across the environment..


Dataset naming conventions are implicit. Compile procedures are shop-specific. The meaning of a return code depends on the job. The relevant spool output may live several panels away. The person who knows which CICS region matters may be retiring soon.


This creates two bottlenecks.


- Speed: Even routine work requires a high-friction loop of navigation, inspection, job submission, failure analysis, and correction.
- Knowledge: The mainframe contains the information needed to understand and change the system, but that information is trapped behind interfaces that require years of operational familiarity.


For modernization, this becomes a fundamental constraint. Before a system can be transformed, it has to be understood. Before it can be understood, agents need access to the real operating context: the code, jobs, datasets, outputs, transactions, errors, and conventions that define how the system actually behaves.


A chatbot next to a terminal is not enough.


The agent needs to operate inside the mainframe environment.


## Introducing Hopper


Hopper is an agentic development environment for the mainframe.


It combines a TN3270 terminal, mainframe-aware context panels, and an intelligent AI agent that can operate across the core surfaces of z/OS development.


The agent can navigate ISPF, inspect datasets, write JCL, submit jobs, parse JES spool output, analyze return codes, query VSAM, interact with CICS, and explain failures. It works through the same terminal interface that mainframe developers use today, while also receiving structured context from jobs, datasets, and members.


Hopper is not trying to hide the mainframe behind a generic abstraction. It is not a chatbot pasted onto a terminal. It is built around the objects and workflows mainframe teams use every day.


The design principle is simple: preserve the fidelity of the mainframe environment, but make it accessible to AI agents. The user can drive the terminal manually. The agent can take action when asked. Sensitive operations require approval and the terminal remains visible at all times.


Hopper gives AI agents a way to work with the mainframe without pretending the mainframe is a modern code repository.


We named it Hopper after Rear Admiral Grace Hopper, whose work helped make computing more accessible to humans. Hopper carries that same spirit into the mainframe era: a new interface for the systems that still run the world.


## **What Hopper can do today**


### An agent that understands mainframe workflows


Hopper's agent is mainframe-native. On first connection it probes the LPAR and learns the dataset HLQs you own, the JES configuration, the compile procs your shop maintains, and the CICS region you build into. Every action runs against your shop's conventions.


The agent ships with over 50 mainframe-specific tools. It drives ISPF panels by name, writes JCL aligned to column 72, parses JES spool, queries VSAM as SQL, runs CICS transactions, and triages abends. The user stays in the chat. Hopper navigates the terminal.


### Browse datasets without leaving the terminal


Hopper's datasets panel replaces the ISPF 3.4 → 3.1 → Browse round-trip with one always-visible view. You filter by dataset name, drill into a PDS, and read a member's source without leaving the panel. *Send to chat* hands the dataset or member to the agent as an` @` -tag, so the agent reads tagged content without you pasting source into the prompt. From there it can edit the file or submit a job that uses it.


### Track jobs and diagnose spool output


You filter the jobs panel by status, owner, or class. A JOBID's status updates from INPUT to ACTIVE to OUTPUT in place, with no swap to SDSF. When a job fails, *Send to chat* hands the JOBID to the agent as an` @` -tag. Instead of cycling through JESMSGLG, JESYSMSG, and IBM message lookups, the agent reads the spool, identifies the abend, and resubmits with corrected JCL.


### Drive the terminal yourself


The TN3270 terminal is unchanged. PF, PA, and attention keys behave as in any TN3270 emulator. Hidden fields stay hidden. Light-pen events register, AID values pass through. You can drive the terminal manually at any time; the agent waits when you take the keyboard. For developers who know their way around ISPF, the agent is an add-on to the terminal you already trust.


## The engineering challenge behind Hopper


Building an agentic IDE for the mainframe is different from building an AI coding assistant for modern software.


Modern code agents usually operate over files, repositories, terminals, package managers, test runners, and cloud APIs.


Mainframe agents have to operate across fixed-width source, column-sensitive JCL, EBCDIC data, partitioned datasets, ISPF panels, JES queues, spool output, VSAM files, CICS transactions, return codes, compile procedures, and shop-specific conventions.


They also have to operate safely in environments where mistakes can affect critical business processes.


Hopper gives AI agents an operating layer for that world. It turns the mainframe from a human-only terminal environment into an agent-accessible development environment, while preserving the control and fidelity that mainframe teams require.


That is the shift: not code explanation, not documentation, not chat, but safe, real-time operation inside a mainframe


## Origin of Hopper


When we started Hypercubic, we were focused on understanding legacy systems. That work quickly led us deeper into the mainframe environment itself.


To build useful modernization systems, we needed to interact with real mainframes. We needed to inspect COBOL programs, submit JCL, read JES output, work with datasets, understand VSAM files, test changes, and observe how systems behaved in practice.


Existing tools were useful, but they made one thing clear: the interface layer was a major bottleneck.


The system contains rich operational knowledge, but AI agents had no native way to access or act on it.


So we built Hopper.


The first version was a CLI called HyperFrame. It connected to the mainframe over FTP, moved code and jobs between a local machine and the LPAR, and made it possible to run COBOL programs through JCL from a modern development workflow.


Then we added TN3270 protocol support.


That changed the shape of the tool. HyperFrame was no longer just a convenient bridge over FTP. It became a way for AI systems to operate through the same screen-based interface mainframe developers and operators use every day.


For the first time, we could interact with the mainframe through natural language while preserving the fidelity of the underlying TN3270 environment.


That became Hopper.


## **What Hopper unlocks**


Hopper gives mainframe teams an AI-native development environment today.


Over time, it becomes the foundation for a much broader set of workflows.


Once AI agents can operate inside the mainframe environment, maintenance and modernization changes.


Agents can inspect how programs are compiled and run. They can trace jobs and outputs. They can gather evidence across datasets, spool files, transactions, and return codes. They can connect code to runtime behavior.


They can help developers understand not just what a program says, but how it behaves inside the system.


That creates the foundation for AI-assisted mainframe development, faster job debugging, automated documentation, safer code changes, test generation, migration planning, traffic replay, modernization verification, data movement, and end-to-end transformation workflows across COBOL, JCL, VSAM, CICS, and related systems.


At Hypercubic, we are building AI-native infrastructure for the full lifecycle of legacy modernization: understanding, operating, transforming and testing.


Hopper is one part of that platform.


HyperDocs explains the system. HyperTwin captures expert knowledge. Hopper lets you operate inside the environment. HyperLoop transforms and verifies change.


Together, they form a comprehensive best-in-class mainframe modernization platform.


## **Download Hopper**


Hopper is available today on Windows, Linux, and macOS.


If you work with mainframes, try it on a real workflow. Connect to your LPAR, inspect a dataset, submit a job, debug a failure, and see what an agentic mainframe environment feels like.


[Download Hopper →](https://www.hypercubic.ai/hopper)


[Get in touch →](https://www.hypercubic.ai/contact)
