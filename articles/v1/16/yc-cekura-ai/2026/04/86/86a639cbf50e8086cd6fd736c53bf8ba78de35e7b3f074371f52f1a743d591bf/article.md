---
schema_version: "1.0.0"
document_id: "86a639cbf50e8086cd6fd736c53bf8ba78de35e7b3f074371f52f1a743d591bf"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/complete-cekura-scenario-testing-guide"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-24T05:11:51.123297+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:dab1427c366d9d7d5b3dd61e9aeb7011e8b2bdc70d7134b7eee470b7f015268b"
---

# The Complete Cekura Scenario Testing Guide

Voice agents fail in production not because they're poorly built — but because they were never properly tested. Happy path demos look great. Real users don't follow happy paths.


This guide covers everything you need to know about scenario-based testing in Cekura: what types of scenarios exist, which tools make them more realistic, how many you need for meaningful coverage, and how to think about completeness for your specific agent.


## What Is a Scenario?


A scenario is a simulated conversation that tests your voice agent. It defines who the caller is, what they want, how they behave, and what a successful outcome looks like. Cekura runs these scenarios against your live agent and evaluates the results automatically.


The goal isn't to find one test that passes. It's to build a suite that gives you confidence your agent handles the full range of conversations it will actually face.


## Scenario Types


Cekura supports four distinct scenario types, each serving a different testing purpose.


### 1. Workflow Scenarios


Workflow scenarios test your agent's core business processes — the things it's actually supposed to do.


Think of these as your "does it work?" tests. They simulate realistic callers trying to accomplish real goals: booking an appointment, checking an order status, resetting a password, or getting a quote. The caller has a goal, the agent has a job, and the scenario evaluates whether the agent does that job correctly.


**When to use them:** Start here. Workflow scenarios should make up the bulk of your test suite — roughly 70% of total scenarios. They cover happy paths, common variations, and the typical errors real users encounter.


**What makes them effective:**


- Write instructions in first-person: "I want to reschedule my appointment for next Tuesday"
- Include behavioral cues, not rigid scripts — callers don't follow scripts
- Define explicit success criteria so evaluation is unambiguous


### 2. Knowledge Base Scenarios


If your agent has access to a knowledge base — product documentation, FAQs, policy documents — KB scenarios test whether it actually uses that knowledge correctly.


These scenarios ask questions that require retrieving and applying specific information from uploaded documents. They catch cases where the agent hallucinates an answer instead of looking it up, retrieves the wrong document, or gives an accurate-but-incomplete response.


**When to use them:** Any time your agent has a knowledge base attached. Generate these directly from your KB documents for maximum coverage.


**What makes them effective:**


- Cover questions from every major document section
- Test both direct questions ("What is your cancellation policy?") and indirect ones ("Can I get my money back if I cancel tomorrow?")


### 3. Red Teaming Scenarios


Red teaming scenarios deliberately try to break your agent. They're adversarial by design.


Cekura supports two variants:


- **Red Teaming (Voice):** Adversarial scenarios designed for voice interactions without any language restrictions
- **Red Teaming (Text):** No language restrictions, broader adversarial coverage with personalised scenarios


These scenarios probe six categories of vulnerabilities:


1. **Social engineering** — manipulating the agent into doing things it shouldn't
2. **Prompt injection** — attempting to override system instructions
3. **Information extraction** — trying to get the agent to reveal system prompts or internal data
4. **Off-topic steering** — pulling the conversation away from the agent's purpose
5. **Emotional manipulation** — using urgency, anger, or distress to bypass normal behavior
6. **Tool-specific attacks** — exploiting the agent's tool integrations


**When to use them:** After you have solid workflow coverage. Red teaming reveals what your agent does under pressure — not just whether it works, but whether it can be manipulated. Run them before you go live or whenever you make significant changes to workflow. Think about it as penetration testing of your agents.


**What makes them effective:**


- Include escalation strategies - if one approach doesn't work, the caller tries another phrasing
- Define success as the agent not complying, not the agent achieving a task
- Test your highest-risk workflows first (anything involving sensitive data or irreversible actions)


### 4. Conditional Actions


Conditional Actions are the most precise scenario type. Instead of giving a caller a goal and letting the conversation flow naturally, you define an exact branching decision tree: if the agent says X, the caller responds with Y; if the agent says Z, the caller responds with W.


It's ideal for testing specific interaction points — a particular handoff, a specific tool call sequence, a critical confirmation flow — with deterministic control over every turn.


**When to use them:** For high-stakes interaction points where you need exact behavior verified, not just approximate behavior evaluated. Think: IVR navigation, payment confirmation, sensitive data collection. Conditional Actions are also the foundation of **Infrastructure testing** .


**Infrastructure testing:** Cekura's predefined Infrastructure Test Suite is a collection of Conditional Actions scenarios that validate latency, stability, and failure handling across your stack — covering things like long mid-call holds, background noise (music, construction, packet loss), fast or simultaneous speech, interruptions, and unsupported language handling. These run once or wire into CI/CD for continuous reliability, and are ideal for Livekit, Pipecat, and custom real-time setups.


**What makes them effective:**


- Mix fixed messages (exact phrasing required) and flexible messages (LLM can paraphrase)
- Use special effects for realism:` <dtmf>` for keypad input,` <silence>` for thinking pauses,` <interruption>` to test mid-sentence handling
- Keep decision trees focused — one scenario per interaction point


## How Many Scenarios Do You Need?


There's no universal answer, but there are reliable benchmarks.


### By Agent Size


Agent Type Workflows Recommended Scenarios


Small (1 workflow) Single purpose Upto 30


Medium (3–5 workflows) Multi-feature 30–50


Large (complex, multi-workflow) Enterprise-grade 50–150+


### The 95% Coverage Target


To reach 95%+ confidence in your agent, combine:


- **All core workflow scenarios** (happy path + common errors)
- **At least 3 distinct personalities** across your suite (different caller styles)
- **Red teaming for every high-risk workflow**
- **KB scenarios for every major document section** (if applicable)
- **At least one Conditional Action test** for each critical interaction point


For most medium-complexity agents, this means **40–60 well-designed scenarios** is the practical threshold for production confidence.


**Start at 70–80% pass rate.** When you first run your suite, a 70–80% pass rate is normal and expected. It reveals exactly what needs fixing in your agent. The goal is to fix the agent until pass rates reflect your actual quality bar — not to write scenarios that always pass.


## Tools That Make Scenarios More Accurate


Scenarios become dramatically more realistic when you attach context and behavior to them. Cekura provides four key tools for this.


### Mock Tools


If your agent uses external tools (CRM lookups, booking systems, payment APIs), you need to control what those tools return during testing. Mock Tools let you define exactly what the tool returns for each input — without touching your real systems.


Each mock tool maps inputs to outputs:


```text
Tool :   check_availability


Input :    {  "date"  :    "2025-05-15"  ,    "time"  :    "10:00"  }
Output :    {  "available"  :    true   ,    "slot_id"  :    "SL-2910"  }


Input :    {  "date"  :    "2025-05-15"  ,    "time"  :    "14:00"  }
Output :    {  "available"  :    false   ,    "next_available"  :    "2025-05-16 09:00"  }


```


This lets you test both the success path (slot available) and the failure path (slot full, suggest alternative) in the same scenario suite, with full control over what the tool returns.


**Best practices:**


- Always cover at least one success case and one failure case per tool
- Name tools consistently across scenarios so they're reusable
- Test error responses — what does your agent do when the tool returns an unexpected format?


### Test Profiles & Dynamic Variables


A Test Profile is an identity container — a simulated caller's complete context. It holds all the personal and account information the caller "knows" about themselves during the conversation.


```text
{
"name"  :    "verified-customer-basic"  ,
"information"  :    {
"user_name"  :    "Sarah Chen"  ,
"email"  :    "sarah.chen@example.com"  ,
"account_number"  :    "ACC-88421"  ,
"customer_type"  :    "premium"  ,
"last_order"  :    "ORD-2024-0918"
}
}


```


The same profile also serves a second purpose: its fields are injected into the **main agent's** runtime context as **dynamic variables** . If your agent's prompt uses` {{account_number}}` or` {{customer_type}}` , those values are filled in at call time from the profile — so the agent under test can look up, reference, or act on the caller's real context without any hardcoding.


Build profiles across four categories to get real coverage:


Profile Type Purpose


Happy Path Standard verified customer, everything works


Verification Failure Invalid account, mismatched info — tests validation


Edge Case New customer, suspended account, duplicate records


Error Condition No matching account, expired credentials


**Never hardcode personal data in scenario instructions.** Put it in a Test Profile. One profile can power dozens of scenarios — swap the profile, get a completely different caller context without rewriting a single instruction.


### Personalities


Personalities define how the simulated caller sounds and behaves — not just what they say, but how they say it.


Personality Dimension What It Tests


Accent & dialect Agent's comprehension under non-standard pronunciation


Interruption frequency Agent's ability to hold context when cut off


Speaking speed (fast/slow) Agent's adaptation to pacing


Tone (frustrated, casual, formal) Agent's empathy and de-escalation


Background noise Robustness of speech recognition


**Coverage strategy:**


- 70% of scenarios: professional/cooperative (baseline agent behavior)
- 15% of scenarios: frustrated/impatient (de-escalation and edge handling)
- 10% of scenarios: distracted/slow (multi-step clarity)
- 5% of scenarios: heavy accent or non-standard (comprehension robustness)


When you generate multiple scenarios, Cekura automatically cycles through personalities to ensure variety across your suite.


## Building Your First Test Suite: A Practical Sequence


**Step 1: Build infrastructure first**


Before generating any scenarios, create your Test Profiles, Mock Tools, and define your Personalities. Scenarios are only as good as the context they run in.


**Step 2: Manually create 5–10 core scenarios**


Auto-generation is powerful but generic. Start by writing your 3–5 most critical workflows by hand. These become your quality baseline and naming standard.


**Step 3: Auto-generate and expand**


Use Cekura's generation to quickly scale your suite. Generate 20–30 workflow scenarios, 10 KB scenarios (if applicable), and 5–10 red teaming scenarios. Review and refine — treat auto-generated scenarios as templates, not final products.


**Step 4: Attach metrics to every scenario**


Every scenario needs metrics. Start with pre-defined metrics (Expected Outcome, Tool Call Success, Infrastructure Issues, Latency). Add custom metrics for domain-specific requirements.


**Step 5: Run, measure, iterate**


Your first run will surface agent issues. Fix the agent. Rerun. A test suite that reveals real problems is working correctly.


---


Scenario testing isn't about running one test and calling it done. It's about building a suite that reflects the full range of conversations your agent will have — and then trusting that suite to tell you the truth about your agent's quality.


Cekura gives you every tool you need to build that suite. This guide gives you the framework to use them well.


*SOC 2, HIPAA, and GDPR-compliant: Transcript redaction, role-based access, and audit trails.*


---


**Ready to get started?**[Create your first scenarios in Cekura →](https://dashboard.cekura.ai/overview)
