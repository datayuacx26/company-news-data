---
schema_version: "1.0.0"
document_id: "0cbd42a24616398fdf3d0a9095a575459fae3c56135a94a910d49da7fdfc43e9"
company_key: "pegasystems-inc-common-stock"
company: "Pegasystems Inc."
source_id: "pegasystems-inc-common-stock-rss-deafc2cd455e"
canonical_url: "https://community.pega.com/blog/you-let-ai-run-you-workflows-ask-yourself-two-questions"
published_at: "2026-06-26T12:00:00+00:00"
first_seen_at: "2026-07-25T01:11:26.395289+00:00"
fetched_at: "2026-07-28T20:48:11.217348+00:00"
content_hash: "sha256:7761487b4327ab5ae7fe8e912156216e1424a0dd4c923b7bcb83afc6a695585d"
---

# Before You Let AI Run You Workflows, Ask Yourself Two Questions

Financial services firms are excited about AI right now.


And the conversation always lands in the same place. Feed your operating procedures, policy documents, and compliance manuals to an AI system. Let it generate your workflows. Automatically.


Like magic.


Someone told me 25 years ago that what people like me do in software looks like magic to everyone else. Apparently, the magic show has upgraded.


Here's the thing — that excitement is racing ahead of a pretty important question. One that almost nobody is asking:


*Which of these workflows might a regulator want to examine?*


Firms are coming at this technology-first instead of governance-first. And the regulatory framework is catching up faster than most people realize.


What I keep seeing is that most teams treat AI like a light switch. On or off. In or out.


It's not. It's actually two spectrums. And most firms are only thinking about one of them.


How was the workflow *designed* ? And how does it *run* ?


Those are different questions. They carry different risks. And you can have strong governance on one and a complete blind spot on the other.


That's where the trouble starts.


But here's what I think most people miss — getting governance right isn't about slowing down. It's about knowing where you can move fast and where you can't. The firms that figure this out will adopt AI faster than the ones that either rush in blind or freeze up.


I've been thinking about how these pieces fit together. With a little thinking help from AI — the low-risk end of design time, if you're keeping score — I assembled what I'm calling the AI Workflow Autonomy Spectrum. Two spectrums in one framework. Curious whether it resonates.


## **Design Time: How Was This Workflow Built?**


Who — or what — decided this workflow should look the way it does?


**AI-Assisted Design**


Humans make all the real decisions. AI helps visualize, structure, and speed things up. A tool in human hands.


Example: a team uploads their client onboarding procedures into an AI tool. The tool maps out process steps, identifies handoffs, shows what the workflow could look like. Then the team debates it, changes it, signs off.


AI made them faster. Humans made the decisions.


Regulatory risk? Low. You can point to specific people who made specific choices and explain why. The thinking is clearly human.


This is where most firms *think* they are. It's also where most firms *should* start for anything regulatory.


**AI-Generated, Human-Governed Design**


Now AI is doing more of the heavy lifting. It proposes complete workflow structures — step sequences, decision logic, approval gates, data checks. Humans review and approve before anything goes live.


Wealth management example: a firm feeds its trust administration procedures to an AI system. The AI generates a proposed workflow for discretionary distributions — steps it inferred from the documents. A trust officer and compliance analyst review it and sign off.


Regulatory risk? Moderate to high. Depends entirely on how real that review is.


Here's the bottom line: can the reviewers explain *why* each step exists? Not just confirm it "looks right" — but actually articulate the reasoning behind each control, each gate, each check?


If the review is real and documented, this works. If it's rubber-stamping, it won't survive examination.


And here's a problem most people don't think about. The workflow might run the same way every time after approval. You can trace it. You can audit it. But its *design* wasn't repeatable. Run the AI again and you might get a different workflow with different steps.


Think about that for a second.


When a regulator asks, "why does your trust distribution process include these steps in this order?" — you need a better answer than "because the AI suggested them."


*You need a better answer than "because the AI suggested them."*


Build your review protocols now. Before you arrive here by default.


## **Run Time: How Does This Workflow Execute?**


This is the second dimension. And it's the one I don't see enough people thinking about.


You can have a well-designed workflow with strong governance around how it was built. But how it *runs* is a separate question.


**Deterministic Execution**


Same steps. Same order. Every time.


You can trace it, audit it, reproduce it. If a regulator asks "what happened?" you can show them. And you can prove it would happen the same way again.


This is where regulated workflows should live.


**AI-Orchestrated Execution**


This is where the hidden risk lives.


Individual workflows might be well-designed and fully deterministic. Each one runs the same way every time. Looks great in isolation.


But what if an AI layer sits *above* those workflows, making routing decisions? Which workflows to invoke, in what sequence, with what data?


The workflows underneath are governed. The orchestration layer deciding which ones to run and how to connect them? That might be a black box.


And here's the problem: you can log every routing decision the AI made. That tells you *what* it chose. It doesn't tell you *why* .


Think about what that means for a regulated process. The individual workflows are auditable. The decisions connecting them aren't. You've built governed rooms inside an ungoverned building.


*Governed workflows underneath an ungoverned orchestration layer. That's the blind spot.*


**Fully Autonomous Execution**


This is the far end. AI reads a client request, interprets the procedure, decides what steps to take, and executes them. All at runtime. Each run might follow a different path. No predefined workflow. No guaranteed consistency.


For anything involving supervisory, fiduciary, or recordkeeping obligations? I'll be direct: the risk ranges from high to indefensible. You can't guarantee consistent execution. You can't reconstruct the AI's reasoning. And you can't show a regulator that a human was actually in control.


Regulators see this coming. FINRA's 2026 Annual Regulatory Oversight Report added an entire section on agentic AI for the first time. The message to the industry: if your AI is doing a supervisor's job, you still own the supervision (FINRA Rules 3110 and 3120, for the compliance folks reading this). Snell & Wilmer's analysis called it a "supervisory reckoning for autonomous AI." Their key point? Once AI can *do things* — not just write things — your supervisory and recordkeeping obligations change.


The SEC has already fined firms for overclaiming what their AI does. But nobody's drawn the bright line yet — and the first firm that gets it wrong becomes the case study everyone else learns from.


Do you want to be that firm?


There's an operational cost too. Every time an AI agent reasons through a workflow at runtime, it burns AI processing. Every. Single. Time. And every run introduces variability. Your compliance team has to explain it. Your audit team has to reconstruct it. Your legal team has to defend it.


A deterministic workflow runs the same way at a fixed cost. Fully autonomous execution pays a premium for every run — and you're paying that premium for unpredictability.


*You're paying a premium for unpredictability.*


Now — not every workflow in a financial services firm is regulatory.


For purely operational stuff with no regulatory touchpoint? Go get the efficiency gains. Don't over-engineer governance where it's not needed.


The problem isn't autonomous execution. It's applying it to the wrong workflows.


## **Where Design Time and Run Time Intersect**


Here's why both dimensions matter. You can have strong governance on one spectrum and a dangerous blind spot on the other.


Well-designed workflows running deterministically? Lowest risk. You can explain why the workflow looks the way it does *and* prove it runs the same way every time. That's where regulated workflows should live.


AI-designed workflows running deterministically? Design risk exists, but the runtime is auditable. This works if the design review was real. If it was rubber-stamping, the exposure catches up eventually.


But here's the trap I keep seeing: strong design governance paired with an AI-orchestrated runtime.


Each individual workflow looks fine. The orchestration layer sitting above them — deciding which workflows to invoke and how to connect them — is ungoverned. I call this "downstream entanglement."


## **The Window Is Open**


The regulatory framework for AI in financial services is still being written. But the expectations are getting clearer by the month. Firms that build their compliance architecture now won't have to slam the brakes later when a regulator asks questions they haven't thought about.


Because there are really two compliance questions: *Am I complying?* And just as important — *if I get audited, how do I prove it?*


The two-spectrum framework helps with the first one. It shows you where your governance risks live — design time, run time, or both. But the second question is about evidence. Can you show a regulator a blueprint? Can you reconstruct not just what happened, but *why* your process is designed to handle it that way?


If your workflows are well-designed and deterministic, you can whip that blueprint out. If you're running AI-orchestrated routing or autonomous execution in regulated spaces, that conversation is going to be much harder.


*The technology decision comes after the governance decision. Not before.*


What I've learned from watching this space is that the role of the agent is not to run the process — but to follow the process. It's a lot easier to move forward when you've already figured out where the guardrails go.


This isn't about saying no to AI. It's about saying yes in the right places with the right safeguards. In a follow-up piece, I'll share four questions I use to classify where a workflow sits on each spectrum — and where downstream entanglement tends to hide.


I'd welcome hearing how your firm is thinking about this in the meantime.


Here's what that looks like.


A trust officer uses an AI-orchestrated process to research a client's tax situation. The individual research workflows are well-designed. But the AI layer decided which ones to invoke, in what sequence, with what data. That research informs a discretionary distribution decision.


Six months later a beneficiary challenges the distribution. The firm has to reconstruct how the research was done. They can show the individual workflows. They can't explain why those specific workflows were chosen, in that sequence, for that client.


*The workflows weren't the problem. The routing decisions were.*


Think about how many processes in your firm involve AI deciding which steps to connect. That's the exposure most firms haven't mapped.


### **Further Reading**


FINRA, "2026 Annual Regulatory Oversight Report," December 2025. Retrieved March 12, 2026.


[https://www.finra.org/sites/default/files/2025-12/2026-annual-regulatory-oversight-report.pdf](https://www.finra.org/sites/default/files/2025-12/2026-annual-regulatory-oversight-report.pdf)


Snell & Wilmer, "FINRA's 2026 Oversight Report Signals a Supervisory Reckoning for Autonomous AI," December 18, 2025. Retrieved March 12, 2026.


[https://www.jdsupra.com/legalnews/finra-s-2026-oversight-report-signals-a-4923301/](https://www.jdsupra.com/legalnews/finra-s-2026-oversight-report-signals-a-4923301/)


SEC, "SEC Charges Two Investment Advisers with Making False and Misleading Statements About Their Use of Artificial Intelligence," March 18, 2024. Retrieved March 12, 2026.


[https://www.sec.gov/newsroom/press-releases/2024-36](https://www.sec.gov/newsroom/press-releases/2024-36)


CPO Magazine, "2026 AI Legal Forecast: From Innovation to Compliance," January 15, 2026. Retrieved March 12, 2026.


[https://www.cpomagazine.com/data-protection/2026-ai-legal-forecast-from-innovation-to-compliance/](https://www.cpomagazine.com/data-protection/2026-ai-legal-forecast-from-innovation-to-compliance/)


*These are my own observations from working in enterprise software for financial services. Practitioner perspective, not legal advice. Talk to your compliance and legal teams for guidance specific to your firm.*


*This framework benefited from conversations with[Fritz von Bulow](https://www.linkedin.com/in/fritz-von-bulow/)* *, who challenged me to separate design-time and run-time risk into distinct dimensions. Better frameworks come from better questions.*
