---
schema_version: "1.0.0"
document_id: "a5002a86352b1a5f11996beaba12c097c2aeef93ff8d2821bb97c0407f2f2977"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/-gas-town-x-webbs-dok"
published_at: "2026-02-05T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T20:54:31.900628+00:00"
content_hash: "sha256:52f0c5a4f896e3e3083df0139f81535a80cf483ddae2a9c0b5c4650d7dd53114"
---

# Gas Town x Webb's DOK

## TL;DR


I built a framework that measures two dimensions of AI collaboration: **how sophisticated your tools are** (Gas Town stages) and **how deeply you're thinking** (DOK levels). The intersection reveals where your next growth opportunity lives.


---


## The Problem


Many AI practitioners face a hidden inefficiency: a mismatch between tool sophistication and task cognitive complexity.


Anti-Pattern Impact


Using powerful autonomous agents for simple "what is X?" queries Unrealized potential


Asking deep strategic questions through basic chatbot interfaces Bottlenecked thinking


No visibility into personal AI usage patterns Stagnant growth


No framework for intentional growth in AI collaboration skills Missed opportunities


I noticed this pattern in my own work: I'd use powerful autonomous agents to answer questions I could have Googled. I was driving a Ferrari to the mailbox.


The tools were sophisticated. The questions weren't.


Without measurement, there's no improvement. I needed a mirror to see my AI collaboration patterns clearly.


---


## Two Frameworks, One Insight


I combined two existing frameworks to create a two-dimensional model for understanding AI collaboration maturity:


### Horizontal Axis: Gas Town Stages


Steve Yegge's["Welcome to Gas Town"](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) (January 2026) describes an 8-stage progression of AI tool adoption:


Stage Name Description


8 Full Gas Town Complete AI-native development ecosystem


7 Agentic Workflows Automated pipelines with agent coordination


6 Multi-Agent Orchestrating multiple specialized agents


5 CLI Single Agent, YOLO Terminal-based autonomous agent (e.g., Goose)


4 Chat IDE Integrated chat in development environment


3 Copilot Using AI code completion, inline suggestions


2 Curious Experimenting with basic chatbots occasionally


1 Observer Watching and evaluating AI tools, not yet actively using


Most engineers I talk to are somewhere between Stage 3 and Stage 5. The tools are available. The question is: what are we doing with them?


### Vertical Axis: Depth of Knowledge (DOK)


Norman Webb's[Depth of Knowledge framework](https://static.pdesas.org/content/documents/m1-slide_19_dok_wheel_slide.pdf) (1997) measures cognitive complexity across four levels:


Level Name Prompt Indicators


4 Extended Thinking "Research and synthesize...", "Create a framework...", "Investigate over time..."


3 Strategic Thinking "Design...", "Analyze...", "What if...", "Develop a strategy..."


2 Application "How would you...", "Compare...", "Explain why..."


1 Recall "What is...", "List...", "Define..."


DOK 1 is looking up syntax. DOK 4 is creating new knowledge. Most day-to-day engineering work lives at DOK 2.


---


## The Integration Matrix


When you plot Gas Town stages against DOK levels, six distinct zones emerge:


```text
1                   DOK 1        DOK 2         DOK 3          DOK 4
2                 (Recall)   (Application) (Strategic)   (Extended)
3                ┌──────────┬──────────────┬────────────┬────────────┐
4  Stage 6-8     │   Over-  │    Over-     │ Underutil- │  Frontier  │
5  (Multi/       │  powered │   powered    │   izing    │            │
6   Agentic)     │          │              │            │            │
7                ├──────────┼──────────────┼────────────┼────────────┤
8  Stage 5       │   Over-  │  Underutil-  │  Expected  │  Growing   │
9  (CLI YOLO)    │  powered │    izing     │            │            │
10                ├──────────┼──────────────┼────────────┼────────────┤
11  Stage 3-4     │   Over-  │   Expected   │  Growing   │  Frontier  │
12  (Copilot/     │  powered │              │            │            │
13   Chat IDE)    │          │              │            │            │
14                ├──────────┼──────────────┼────────────┼────────────┤
15  Stage 1-2     │ Expected │   Growing    │  Thinking  │  Thinking  │
16  (Observer/    │          │              │   Ahead    │   Ahead    │
17   Curious)     │          │              │            │            │
18                └──────────┴──────────────┴────────────┴────────────┘
```


**The insight:** Your tools and your thinking should grow together.


### Zone Definitions


Zone Description Action


**Frontier** Pushing boundaries of both tool and cognition Celebrate & Document


**Thinking Ahead** High cognitive work with basic tools Upgrade tools


**Growing** Stretching into higher complexity, positive trajectory Encourage


**Expected** Appropriate match of tool sophistication to task complexity Maintain


**Underutilizing** Sophisticated tools for simpler tasks Increase DOK


**Overpowered** Tools exceed task needs—opportunity to level up your questions Realign


Every zone is valid. The framework isn't about judgment—it's about awareness and intentional growth.


---


## What This Looks Like in Practice


I built a skill that analyzes AI collaboration sessions and returns a snapshot:


```text
1  ╔══════════════════════════════════════════════════════════════════╗
2  ║                    SESSION ANALYSIS                              ║
3  ╚══════════════════════════════════════════════════════════════════╝
4
5  GAS TOWN STAGE: 5 (CLI Single Agent, YOLO)
6
7  DOK DISTRIBUTION
8  ────────────────────────────────────────────────────────────────────
9  DOK 1 (Recall):      ████░░░░░░░░░░░░░░░░  17%
10  DOK 2 (Application): ████████████░░░░░░░░  52%
11  DOK 3 (Strategic):   ██████░░░░░░░░░░░░░░  26%
12  DOK 4 (Extended):    █░░░░░░░░░░░░░░░░░░░   5%
13
14  QUADRANT: Underutilizing
15  ────────────────────────────────────────────────────────────────────
16  You're using powerful autonomous tools—there's an opportunity to
17  match your questions to that power.
18
19  GROWTH NUDGES
20  ────────────────────────────────────────────────────────────────────
21  1. Shift 2-3 DOK 2 prompts to DOK 3 by adding "analyze trade-offs"
22  2. Before simple queries, ask: "Can I make this more strategic?"
23  3. Try one DOK 4 extended investigation this week
```


The goal isn't to maximize DOK level on every interaction. Sometimes you need to look up syntax. The goal is **awareness** —knowing when there's an opportunity to go deeper.


> 📘 **Want to try this yourself?** Try the skill using` /rp-why init` to establish your baseline,` /rp-why current` to analyze your current session, or` /rp-why compare` to compare your session to your baseline.


---


## The Growth Formula


```text
Growth = (↑ DOK Level) + (↑ Gas Town Stage) + (Intentional Practice)


```


Moving from "Underutilizing" to "Growing" doesn't require new tools. It requires better questions:


Instead of... Try...


"How do I implement X?" "What are the trade-offs between approaches A, B, and C for implementing X?"


"Fix this bug" "Analyze why this bug occurred and what systemic changes would prevent similar issues"


"Write tests for this" "Design a testing strategy that balances coverage, maintainability, and execution time"


The tools are the same. The cognitive complexity is different.


---


## Target User Profiles


The framework maps cleanly to different practitioner profiles:


Profile Typical Stage DOK Distribution Characteristics


Traditional 1-2 DOK 1: 60%, DOK 2: 30%, DOK 3: 10% Minimal AI use


Adopter 3-4 DOK 1: 40%, DOK 2: 40%, DOK 3: 15%, DOK 4: 5% Growing comfort


Practitioner 5 DOK 1: 25%, DOK 2: 45%, DOK 3: 25%, DOK 4: 5% Autonomous agents


Advanced 5-6 DOK 1: 15%, DOK 2: 35%, DOK 3: 35%, DOK 4: 15% Strategic use


Frontier 7-8 DOK 1: 10%, DOK 2: 25%, DOK 3: 40%, DOK 4: 25% Agentic workflows


The path from Practitioner to Advanced isn't just "use more tools." It's "do more DOK 3-4 work"—design systems, analyze trade-offs, create frameworks, drive technical direction.


AI tools can accelerate that path. Or they can let you coast at DOK 2 indefinitely. The framework helps you see which one is happening.


---


## Actionable Takeaways


**If you're in the Learning Zone (low stage, low DOK):**


- This is a natural starting point—focus on learning the tools
- Try one new AI capability each session
- Don't worry about DOK yet—get comfortable first


**If you're Overpowered (high stage, low DOK):**


- Your tools exceed your task needs—opportunity to level up your questions
- Before each prompt, ask: "Can I make this more strategic?"
- Batch simple queries; save the agent for complex work


**If you're Thinking Ahead (low stage, high DOK):**


- Your thinking exceeds your tools—time to upgrade!
- Explore CLI agents or IDE integration
- Your DOK is strong; let better tools amplify it


**If you're in the Frontier (high stage, high DOK):**


- You're pushing boundaries—document what you learn
- Share patterns with others; teach what works
- Explore the edges: what's not yet possible?


---


## Try It Yourself


Next time you're working with an AI tool, pause and ask:


1. **What Gas Town stage am I operating at?** (Basic chat? Autonomous agent? Multi-agent?)
2. **What DOK level is this prompt?** (Recall? Application? Strategic? Extended?)
3. **Is there a mismatch?**


If you're using Stage 5 tools for DOK 1 questions, there's untapped potential. Small, consistent nudges compound into significant growth. The framework just helps you see where you are—and where you could go next.


> Try the[skill](https://github.com/block/agent-skills/tree/main/rp-why) using` /rp-why init` to establish your baseline,` /rp-why current` to analyze your current session, or` /rp-why compare` to compare your session to your baseline.


---


## Attribution


- **Gas Town Stages** : Steve Yegge,["Welcome to Gas Town"](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) (January 2026)
- **Depth of Knowledge** : Norman Webb (1997). *Criteria for alignment of expectations and assessments in mathematics and science education.* Council of Chief State School Officers.
- **Integration** : The Gas Town × DOK integration is original to this work, combining these frameworks to create a two-dimensional model for reflective practice in AI collaboration.
