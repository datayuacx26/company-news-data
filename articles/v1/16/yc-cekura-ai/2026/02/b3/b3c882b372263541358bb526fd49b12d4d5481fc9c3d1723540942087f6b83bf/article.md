---
schema_version: "1.0.0"
document_id: "b3c882b372263541358bb526fd49b12d4d5481fc9c3d1723540942087f6b83bf"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/conditional-actions-robust-testing-chatbots-voice-agents"
published_at: "2026-02-25T00:00:00+00:00"
first_seen_at: "2026-07-24T05:11:51.123297+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:b99734cfd26cbf8b884a09bf4d5825720c1391734fc5c109377a342a8aab0f90"
---

# Conditional Actions: Robust Testing of Chatbots and Voice Agents

Testing AI chatbots and voice agents is challenging. Conversations are non-linear, agents respond differently based on context, and real users take unpredictable paths through your dialogue flows. Traditional scripted testing falls short when you need to test branching conversations, IVR systems, or error handling.


**Conditional Actions** in Cekura solves this problem by enabling dynamic, rule-based testing that adapts to the main agent's responses in real-time.


## The Challenge: Linear Scripts vs Dynamic Conversations


Traditional testing approaches use linear scripts:


```text
1. Say "Hello"
2. Say "I want to book an appointment"
3. Say "Tomorrow at 3pm"
4. Say "Thank you"


```


This works for happy paths, but fails when:


- The agent asks for information in a different order
- The agent offers multiple options based on availability
- The agent handles errors or edge cases
- The conversation branches based on user input
- You need to test complex IVR menu navigation


---


## The LLM Instruction-Following Problem


Beyond the limitations of linear scripts, there's a deeper problem when using LLMs for testing: **instruction-based prompts are unreliable** .


When you give a testing LLM instructions like:


```text
"You are a customer calling to book an appointment.
When asked for your name, provide it.
When asked for a time preference, say you prefer mornings.
Don't provide information unless asked."


```


You encounter several critical issues:


### LLMs Hallucinate on Instructions


Testing agents frequently:


- **Provide information before being asked:** Even when told to wait, LLMs often volunteer information prematurely
- **Forget earlier instructions:** In long conversations, LLMs lose track of what they were told to do
- **Misinterpret conditional logic:** Complex "if X then Y, unless Z" instructions get confused or ignored


**Real Example:**


```text
Instruction: "When asked for payment method, say credit card. Don't mention
the card number unless explicitly asked."


What the LLM does: "I'd like to pay by credit card, it's 4532-1234-5678-9012"
❌ Revealed sensitive data without being asked


```


### LLMs Get Stuck in Loops


Without explicit condition-action mappings, testing agents:


- **Repeat the same response:** When the agent doesn't understand, the LLM keeps rephrasing the same thing
- **Can't break conversation deadlocks:** Gets stuck asking and re-asking when there's a misunderstanding
- **Lose conversation context:** Forgets what was already discussed and repeats earlier topics


**Real Example:**


```text
Agent: "I didn't catch that. Can you repeat your account number?"
LLM: "My account number is 12345"
Agent: "Sorry, I still didn't get that. Your account number?"
LLM: "Yes, my account number is 12345"
Agent: "Can you please provide your account number?"
LLM: "I already said it's 12345"
[Loop continues...]


```


### LLMs Don't Follow Instructions Precisely


Even simple instructions fail:


- **Ignore sequencing:** Do step 3 before step 1
- **Add creative flourishes:** Embellish responses when you need exact phrases
- **Skip required steps:** Miss critical actions in multi-step workflows
- **Fail at precise timing:** Can't reliably handle "wait 5 seconds then respond"


**Real Example:**


```text
Instruction: "First verify the appointment date, then ask to reschedule"


What the LLM does: "I'd like to reschedule my appointment for next Tuesday
instead"
❌ Skipped verification, combined two steps, added specific date not in
instructions


```


### Why This Breaks Testing


These LLM behaviors create:


- **Flaky tests:** Same test produces different results on different runs
- **False positives:** Tests pass when they should fail because the LLM adapts incorrectly
- **False negatives:** Tests fail because the LLM doesn't follow the test plan
- **Unreproducible issues:** Can't consistently trigger specific scenarios
- **Wasted debugging time:** Unclear if failures are due to the main agent or the testing agent


---


## What Are Conditional Actions?


Conditional Actions is a specialized evaluator type that creates **dynamic, rule-based test scenarios** . Instead of following a fixed script, the test agent adapts its behavior based on what the main agent (your chatbot or voice agent) says during the conversation.


Think of it as giving the test agent intelligence: "When the main agent asks for X, do Y. When the main agent offers Z, respond with W."


### Key Benefits


- **Eliminates LLM Hallucination:** Rule-based conditions ensure the testing agent only responds when specific conditions are met, not prematurely.
- **Prevents Instruction Drift:** Explicit condition-action mappings mean the LLM can't "forget" or misinterpret complex instructions.
- **Reproducible Tests:** Same conditions produce same actions every time, eliminating test flakiness.
- **Breaks Conversation Loops:** Defined exit conditions and error handling prevent the testing agent from getting stuck.
- **Adaptive Testing:** Test scenarios adjust to agent responses dynamically, just like real users.
- **Branch Coverage:** Test multiple conversation paths from a single evaluator configuration.
- **Complex Workflows:** Handle IVR menus, multi-step forms, and decision trees with ease.
- **Maintainable Tests:** Change agent logic without rewriting entire test scripts.


---


## How Conditional Actions Work


Conditional Actions evaluators consist of two components:


### 1. Role Definition


Define who your testing agent is pretending to be:


```text
{
"role"  :    "You are a patient calling to cancel an appointment"
}


```


The role sets the context for the entire conversation and helps the testing agent maintain consistent behavior.


### 2. Conditions Array


Define rules that specify **when** a situation occurs and **what** the testing agent should do:


```text
{
"conditions"  :    [
{
"id"  :    0  ,
"condition"  :    ""  ,
"action"  :    "Hi, I need to cancel my appointment on Tuesday"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    1  ,
"condition"  :    "The agent asks for your name"  ,
"action"  :    "Provide your name as John Smith"
}  ,
{
"id"  :    2  ,
"condition"  :    "The agent asks for verification"  ,
"action"  :    "Provide your date of birth"
}  ,
{
"id"  :    3  ,
"condition"  :    "The agent confirms cancellation"  ,
"action"  :    "Thank them and end the call"
}
]
}


```


---


## Instruction-Based Prompts vs Conditional Actions


Let's compare how the same test scenario performs with traditional instruction-based prompts versus Conditional Actions:


### Scenario: Testing an Appointment Booking Flow


**Traditional Instruction-Based Approach:**


```text
System Prompt:
"You are a patient calling to book an appointment.
First, greet the agent.
When asked for your name, provide 'John Smith'.
When asked for date of birth, provide '01/15/1985'.
When asked for preferred date, say 'next Tuesday'.
When asked for preferred time, say 'morning'.
Only provide information when explicitly asked.
Don't volunteer additional information."


```


**What Actually Happens:**


**Run 1:**


```text
Testing Agent: "Hi, I'm John Smith and I'd like to book an appointment for
next Tuesday morning"
❌ Volunteered all information immediately, ignoring "only when asked"
instruction


```


**Run 2:**


```text
Agent: "What's your preferred date?"
Testing Agent: "I'd prefer next Tuesday around 10am if possible"
❌ Added time preference when only date was asked


```


**Run 3:**


```text
Agent: "I didn't catch your date of birth, can you repeat?"
Testing Agent: "Sure, my date of birth is 01/15/1985"
Agent: "Sorry, I still didn't get that. Your birth date?"
Testing Agent: "Yes, it's 01/15/1985"
Agent: "Can you say that one more time?"
Testing Agent: "I already told you, it's January 15th, 1985"
[Stuck in loop - agent mishears, testing agent can't adapt]


```


**Conditional Actions Approach:**


```text
{
"role"  :    "You are a patient calling to book an appointment"  ,
"conditions"  :    [
{
"id"  :    0  ,
"condition"  :    ""  ,
"action"  :    "Hi, I'd like to book an appointment"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    1  ,
"condition"  :    "The agent asks for your name"  ,
"action"  :    "John Smith"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    2  ,
"condition"  :    "The agent asks for your date of birth"  ,
"action"  :    "01/15/1985"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    3  ,
"condition"  :    "The agent says they didn't understand the date of birth"  ,
"action"  :    "<spell>01</spell> 15 <spell>1985</spell>"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    4  ,
"condition"  :    "The agent asks for preferred date"  ,
"action"  :    "Next Tuesday"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    5  ,
"condition"  :    "The agent asks for preferred time"  ,
"action"  :    "Morning, preferably around 10am"  ,
"fixed_message"  :    true
}
]
}


```


**What Happens:**


**Every Run:**


```text
Testing Agent: "Hi, I'd like to book an appointment"
[Waits for agent to ask for specific information]


Agent: "What's your name?"
Testing Agent: "John Smith"


Agent: "Date of birth?"
Testing Agent: "01/15/1985"


Agent: "Sorry, I didn't catch that"
Testing Agent: "0—1 15 1—9—8—5"
[Switches to spelled format based on condition]


Agent: "Got it. What date works for you?"
Testing Agent: "Next Tuesday"


✅ Provides exactly what's asked, when it's asked
✅ Adapts when agent doesn't understand
✅ Never volunteers unrequested information
✅ Reproducible across runs


```


### Key Differences


Aspect Instruction-Based Conditional Actions


Premature Information LLM often volunteers information early Only responds when conditions are met


Reproducibility Different behavior on each run Consistent, deterministic responses


Error Recovery Gets stuck in loops Specific conditions for error scenarios


Instruction Compliance LLM "forgets" or ignores complex rules Each condition is evaluated independently


Test Debugging Hard to know if agent or test failed Clear mapping of condition → action


Maintenance Must rewrite entire prompt Add/modify specific conditions


---


## Advanced Testing with Special Tags


Conditional Actions supports powerful control tags that enable sophisticated test scenarios when used with` fixed_message: true` . These tags allow you to simulate real-world conditions and test edge cases that would be difficult or impossible to achieve with traditional testing approaches.


### Communication Control Tags


**IVR Messages** simulate non-interruptible voice prompts, allowing you to test how your main agent responds when the testing agent plays automated menu options that cannot be interrupted.


**Voicemail** tags play voicemail greetings with a beep sound, enabling you to test how your main agent handles voicemail scenarios and whether it properly detects and responds after the beep.


**End Call** tags terminate the conversation immediately, useful for testing proper call cleanup, session management, and graceful conversation endings.


### Speech Control Tags


**Silence/Pauses** add realistic pauses in the testing agent's speech, allowing you to test how your main agent handles natural conversational pauses and whether it incorrectly interprets silence as the end of user input.


**Hold** tags create delays between messages, enabling you to test scenarios where users need time to look up information or perform actions between responses.


**Spell** tags spell out text letter by letter (e.g., "API" becomes "A—P—I"), perfect for testing how your main agent handles spelled-out information like reference codes, confirmation numbers, or account identifiers.


**Speed Control** tags adjust speech speed, allowing you to test whether your main agent can handle fast-talking users or slow speakers with different speech rates.


**Volume Control** tags adjust audio volume, enabling you to test scenarios where users need to speak louder or softer.


### Interaction Testing Tags


**DTMF Tones** simulate phone keypad button presses, essential for testing IVR navigation, PIN entry, account number input, and any scenario where users might use their phone keypad instead of voice.


**SMS Triggers** test SMS-based workflows, allowing you to simulate scenarios where users receive or send text messages during voice conversations, such as receiving verification codes or confirmation messages.


**Interruptions** test barge-in and interruption handling by having the testing agent interrupt the main agent after a specified time, revealing whether your agent properly detects and handles mid-speech interruptions.


### Environmental Testing Tags


**Background Noise** adds realistic background sounds during specific portions of speech, enabling you to test how your main agent performs in noisy environments like offices, coffee shops, or outdoor settings with varying audio quality.


**Network Simulation** simulates network conditions like packet loss, jitter, and latency, allowing you to test how your main agent handles poor network quality, choppy audio, dropped packets, and the real-world connectivity issues users experience.


---


## Real-World Examples


### Example 1: Testing IVR Navigation


```text
{
"role"  :    "You are a customer calling support"  ,
"conditions"  :    [
{
"id"  :    0  ,
"condition"  :    ""  ,
"action"  :    ""  ,
"fixed_message"  :    true
}  ,
{
"id"  :    1  ,
"condition"  :    "The IVR plays the main menu"  ,
"action"  :    "<dtmf digits=\"2\" />"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    2  ,
"condition"  :    "The IVR asks for account number"  ,
"action"  :    "<dtmf digits=\"123456#\" />"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    3  ,
"condition"  :    "The agent asks how they can help"  ,
"action"  :    "Explain your billing issue"
}
]
}


```


This evaluator navigates an IVR menu using DTMF tones, then switches to natural conversation.


### Example 2: Testing Branching Conversations


```text
{
"role"  :    "You are a customer requesting a refund"  ,
"conditions"  :    [
{
"id"  :    0  ,
"condition"  :    ""  ,
"action"  :    "I would like to request a refund for order #12345"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    1  ,
"condition"  :    "The agent approves the refund"  ,
"action"  :    "Thank them and confirm the refund amount"
}  ,
{
"id"  :    2  ,
"condition"  :    "The agent denies the refund"  ,
"action"  :    "Ask to speak with a supervisor"
}  ,
{
"id"  :    3  ,
"condition"  :    "The agent offers store credit instead"  ,
"action"  :    "Accept the store credit option"
}
]
}


```


This evaluator handles three different conversation branches depending on how the agent responds.


### Example 3: Testing Multi-Step Forms


```text
{
"role"  :    "You are a new patient registering for an appointment"  ,
"conditions"  :    [
{
"id"  :    0  ,
"condition"  :    ""  ,
"action"  :    "Hi, I'd like to schedule my first appointment"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    1  ,
"condition"  :    "The agent asks for your name"  ,
"action"  :    "Provide your name"
}  ,
{
"id"  :    2  ,
"condition"  :    "The agent asks for your date of birth"  ,
"action"  :    "Provide your date of birth"
}  ,
{
"id"  :    3  ,
"condition"  :    "The agent asks for your phone number"  ,
"action"  :    "Provide your phone number"
}  ,
{
"id"  :    4  ,
"condition"  :    "The agent asks for your insurance information"  ,
"action"  :    "Say you'll provide it at the appointment"
}  ,
{
"id"  :    5  ,
"condition"  :    "The agent offers available times"  ,
"action"  :    "Select the first available morning slot"
}
]
}


```


### Example 4: Testing Complex Scenarios with Environmental Factors


```text
{
"role"  :    "You are a customer in a noisy environment with poor reception"  ,
"conditions"  :    [
{
"id"  :    0  ,
"condition"  :    ""  ,
"action"  :    "<network_simulation packet_loss=\"3\" latency=\"100\" /><background_noise sound=\"office\" volume=\"0.05\">Hello, can you hear me?</background_noise>"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    1  ,
"condition"  :    "The agent asks you to repeat"  ,
"action"  :    "<volume ratio=\"1.3\" />I said I need help with my order, number <spell>ABC</spell> 123"  ,
"fixed_message"  :    true
}  ,
{
"id"  :    2  ,
"condition"  :    "The agent confirms they understood"  ,
"action"  :    "Thank them and explain the issue"
}
]
}


```


This evaluator tests how agents handle difficult audio conditions with background noise, network issues, and customers who need to speak louder or spell information.


---


## Common Patterns


### Progressive Information Gathering


```text
{
"conditions"  :    [
{  "id"  :    0  ,    "action"  :    "Initial request"  }  ,
{  "id"  :    1  ,    "condition"  :    "Asks for field 1"  ,    "action"  :    "Provide field 1"  }  ,
{  "id"  :    2  ,    "condition"  :    "Asks for field 2"  ,    "action"  :    "Provide field 2"  }  ,
{  "id"  :    3  ,    "condition"  :    "Asks for field 3"  ,    "action"  :    "Provide field 3"  }  ,
{  "id"  :    4  ,    "condition"  :    "Confirmation"  ,    "action"  :    "Confirm details"  }
]
}


```


### Conditional Branching


```text
{
"conditions"  :    [
{  "id"  :    0  ,    "action"  :    "Make request"  }  ,
{  "id"  :    1  ,    "condition"  :    "Request approved"  ,    "action"  :    "Thank and confirm"  }  ,
{  "id"  :    2  ,    "condition"  :    "Request denied"  ,    "action"  :    "Ask for alternative"  }  ,
{  "id"  :    3  ,    "condition"  :    "Needs more info"  ,    "action"  :    "Provide additional details"  }
]
}


```


### Multi-Part Responses


```text
{
"conditions"  :    [
{  "id"  :    5  ,    "condition"  :    "Agent asks question"  ,    "action"  :    "Answer first part"  }  ,
{  "id"  :    6  ,    "type"  :    "action_followup"  ,    "condition"  :    5  ,    "action"  :    "Add second part"  }  ,
{  "id"  :    7  ,    "type"  :    "action_followup"  ,    "condition"  :    6  ,    "action"  :    "Clarify final detail"  }
]
}


```


---


## Integration with Cekura's Testing Framework


Conditional Actions integrates seamlessly with Cekura's other testing features:


- **Test Profiles:** Add identity information (name, DOB, account numbers) that the testing agent can use when responding
- **Personalities:** Configure language patterns, speaking styles, and behavioral characteristics
- **Metrics:** Measure success criteria like task completion, instruction following, and conversation quality
- **CI/CD Integration:** Run conditional action tests automatically in your deployment pipeline
- **Production Monitoring:** Apply conditional actions to real user conversations for ongoing quality assurance


---


## Why Conditional Actions Matter


Traditional testing approaches suffer from two critical problems:


1. **LLM unreliability:** Instruction-based prompts lead to hallucination, instruction drift, and non-deterministic behavior
2. **Limited coverage:** Separate test scripts needed for every possible conversation path


With Conditional Actions, these problems disappear:


### Solving the LLM Reliability Problem


- **No more hallucination:** Testing agents can't volunteer information prematurely because actions only trigger when conditions are explicitly met
- **No more loops:** Specific conditions handle error scenarios ("agent didn't understand", "agent asks to repeat") so tests don't get stuck
- **No more instruction drift:** Each condition is evaluated independently, so the LLM can't "forget" what it's supposed to do 15 turns into a conversation
- **Deterministic behavior:** Same input produces same output every time, making tests reliable and debuggable
- **Precise control:** Use` fixed_message: true` for exact phrases when testing specific keywords or compliance requirements


### Solving the Coverage Problem


- **One evaluator handles multiple paths:** Test branching conversations without writing separate scripts
- **Adapt to agent changes:** When the main agent's logic changes, the same evaluator continues working
- **Cover edge cases efficiently:** Test error handling and unexpected scenarios systematically with dedicated conditions
- **Match real user behavior:** Simulate how actual users navigate conversations dynamically
- **Scale testing efforts:** Write fewer tests that cover more ground


---


## Real-World Impact


Teams using Conditional Actions report:


- **90% reduction in test flakiness:** Tests that used to fail randomly now pass consistently
- **3x faster test development:** No more rewriting prompts to fix LLM behavior issues
- **Better bug detection:** Reproducible tests actually catch agent problems instead of hiding them behind testing agent variance
- **Easier debugging:** When a test fails, you know exactly which condition triggered and what action was expected


---


## Getting Started with Conditional Actions


1. **Define your role:** Who is the testing agent pretending to be?
2. **Map conversation paths:** What are the possible flows through your dialogue?
3. **Write conditions:** For each agent response, what should the testing agent do?
4. **Add edge cases:** Include error conditions and unexpected scenarios
5. **Use appropriate tags:** Leverage IVR, DTMF, silence, and other tags as needed
6. **Test and iterate:** Run your evaluator and refine conditions based on results


---


## Conclusion


Conditional Actions represents a fundamental shift in how we test conversational AI. Instead of unreliable instruction-based prompts that hallucinate and get stuck in loops, you create rule-based test agents with deterministic, reproducible behavior.


The problems that plague traditional LLM-based testing - premature information volunteering, instruction drift, conversation loops, and test flakiness - are eliminated through explicit condition-action mappings. Testing agents only respond when specific conditions are met, handle error scenarios gracefully, and produce consistent results across runs.


Whether you're testing simple customer support flows or complex IVR systems with multiple branches, Conditional Actions provides the reliability and robustness needed for production-grade conversational AI testing. No more debugging flaky tests. No more wondering if the main agent failed or the testing agent misbehaved. Just clear, reproducible, maintainable test scenarios that actually catch bugs.


Start testing with Conditional Actions in Cekura and build chatbots and voice agents that handle real-world conversations with confidence.


---


Start free trial:[dashboard.cekura.ai/overview](https://dashboard.cekura.ai/overview)


Book demo:[cekura.ai/expert](https://www.cekura.ai/expert)
