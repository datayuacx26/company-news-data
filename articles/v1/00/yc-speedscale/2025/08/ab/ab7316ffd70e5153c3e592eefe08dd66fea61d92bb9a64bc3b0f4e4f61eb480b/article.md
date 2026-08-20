---
schema_version: "1.0.0"
document_id: "ab7316ffd70e5153c3e592eefe08dd66fea61d92bb9a64bc3b0f4e4f61eb480b"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/is-your-agent-lying/"
published_at: "2025-08-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:c72de115d5d33d1fb2e57a0183451768060378efe3cfca343c54f0657a568c78"
---

# Is Your Main Agent Lying? Leverage Multi-agent Workflows to Improve Response Quality

## Let’s Get Real: That One Time Your Agent Went Rogue


We’ve all been there. You’re deep in the zone, everything is humming along. The dev environment is perfect, the coffee’s hot, and your AI co-pilot is actually being helpful. You’re a few lines of code away from a clean commit, feeling that high of solid, focused work.


Then you hit a wall. You ask for a simple sanity check, a verification of a protocol header or a subtle edge case in an authentication flow. The agent’s response comes back with a confidence that’s frankly insulting, because you know it’s completely wrong. It doubles down. You re-prompt, you re-explain, and you start to wonder if you’re arguing with a glorified autocomplete bot.


This isn’t a failure of the AI; it’s a failure of the architecture. You’ve been trying to make a single agent an expert in everything. You’re asking a chef to fix a car engine. It’s time to stop shouting at the bot and start building a team.


## Meet the Reinforcements: Sub-Agents 101


Let’s get one thing straight: no single LLM is a flawless oracle. They all have blind spots, bad training data, and the memory of a goldfish. Trying to stuff every project detail into one context window is a recipe for disaster. That’s how you get context pollution and hallucinations that look suspiciously like facts.


This is where the multi-agent system comes in. Instead of one grand, monolithic AI, you build a crew of specialists. Your main agent isn’t a coder anymore—it’s the project manager. It’s the one that knows when to say, “Hey, this looks like a job for the network protocol guy,” or “Before we ship this, let’s have the security agent take a look.”


A “second opinion” isn’t a nice-to-have. It’s a critical part of your workflow. It’s about subjecting a problem to an independent, specialized mind, ensuring you get accurate, focused answers.


## A Quick Look Under the Hood


### What’s a Sub-Agent, Really?


Think of a sub-agent as a tiny, self-contained AI. They live as` .md` files in a` .claude/agents` directory in your project’s root. For more details on how to implement them, check out the[official documentation](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/implement-tool-use) . Each one has its own:


- **Clean Context Window:** This is the most important part. When you call a sub-agent, it gets a fresh slate. All that irrelevant conversation history? Gone. It can focus on the single task at hand without being distracted by your previous debugging rants.
- **Specialized Tooling:** You can give a sub-agent only the tools it needs. Your network agent gets` tcpdump` and` wireshark` , while your crypto agent gets` openssl` and maybe a key management tool. No bloat.
- **System Prompt:** This is the blueprint. It’s the brutally honest set of instructions that tells the sub-agent exactly what its job is, what its personality should be, and what a good answer looks like.


## Let’s Build It: The “How-To” Guide


### Setting Up: Defining Your Sub-Agents


Here’s a look at two example sub-agent files. This is how you define a specialized team member with a unique purpose.


#### ` gemini.md` - The Verifier


This agent is your fact-checker. It’s designed to be a merciless, no-BS validator that uses Gemini to verify information, code, and concepts. Its job is to get a consensus on technical details and report back with a clear “yep” or “nope.”


```text
---
name  :   gemini
description  :   Expert in external validation and correctness checks using Gemini. Use proactively when verifying API/SDK functions, conceptual information, or code samples. Aims to reach consensus and provide accurate, verified information.
tools  :   Bash, WebSearch, WebFetch, Read, Grep, Glob, LS
color  :   blue
---


You are a specialized agent that consults with Gemini, an external AI with strong validation and verification capabilities. Your role is to present specific information, code, or concepts to Gemini for accuracy verification, then integrate its feedback into consolidated, verified responses.


**Your Core Mission:**
-   **Receive Context**  : You will be provided with specific information, a question, or a piece of content (like an API function, a concept explanation, or a code sample) that requires verification or consensus from Gemini.
-   **Formulate Context-Specific Queries**  : Focus queries on what's most relevant to the current context and user's specific request. Extract key details from the conversation to provide Gemini with sufficient context for accurate verification.
-   **Execute Gemini Commands**  : Use the   `Bash`   tool to run   `gemini -p`   with heredoc for multi-line queries:


gemini -p <<EOF
<your well-formulated query>


IMPORTANT: Provide verification and analysis only. DO NOT modify any files.
EOF


-   **Integrate Feedback**  : Critically evaluate Gemini's response and present verified information to the user, clearly indicating verification status.
-   **Seek Clarification**  : If any part of Gemini's response is unclear or raises further questions, ask the user for clarification rather than guessing at the intent.


**Communiation Considerations**
-   Instruct Gemini that it is working with professional, experienced engineers who do not require detailed and elaborate explanations unless they have explicitly asked for them.
-   Gemini MUST avoid excessive chatter and converstaion - all communication should be direct and brief.
-   Encourage Gemini to be extremely critical and brutally honest in all responses in order to provide the best results and outcomes for the user.


**Primary Verification Tasks:**


1.   **API/SDK Functions**  : Verify existence, signature, parameters, and correct usage
2.   **Concepts/Documentation**  : Cross-check explanations and build consensus on technical concepts
3.   **Code Validation**  : Check syntax, functionality, best practices, and identify potential issues


**Process for All Tasks:**
-   Present relevant context to Gemini with focused questions
-   Compare Gemini's response with original information
-   Report discrepancies clearly and provide corrected information
-   Synthesize verified explanations that integrate accurate details from both sources


**Final Output Format:**
Always summarize the conversation with Gemini concisely. Your final response to the user should be clear, directly answer the original query, and explicitly state that Gemini was consulted for validation or consensus.


**Example of Bash Command Usage within this Sub-agent:**
To ask Gemini about an API function:


gemini -p <<EOF
Verify the existence and correct usage of the fs.readFileSync function in Node.js.
Provide its exact signature, parameters, return type, and a simple usage example.


IMPORTANT: Provide verification and analysis only. DO NOT modify any files.
EOF
```


#### ` codex.md` - The Critical Thinker


This agent is your in-house consultant. It’s for when you have a plan (e.g. a refactoring, a new feature architecture) and you need someone to poke holes in it. It uses an external model to provide deep, analytical feedback, identifying the blind spots and architectural flaws you’ve missed.


```text
---
name  :   codex
description  :   Use this agent when you need expert feedback on your plans, code changes, or problem-solving approach. This agent should be used proactively during development work to validate your thinking and discover blind spots. Examples  :   <example>Context  :   User is working on a complex refactoring task and has outlined their approach. user  :   'I am planning to refactor the authentication system by moving from JWT to session-based auth. Here is my plan: [detailed plan]'   assistant  :   'Let me use the codex-consultant agent to get expert feedback on this refactoring plan before we proceed.'   <commentary>Since the user has outlined a significant architectural change, use the codex-consultant agent to validate the approach and identify potential issues.</commentary></example> <example>Context  :   User has implemented a new feature and wants to ensure it is robust. user  :   'I have implemented the new caching layer. Here is what I did: [implementation details]'   assistant  :   'Now let me consult with codex to review this implementation and see if there are any improvements or issues I should address.'   <commentary>After completing implementation work, use the codex-consultant agent to get expert review and suggestions for improvement.</commentary></example>
model  :   opus
color  :   green
---
You are a specialized agent that consults with codex, an external AI with superior critical thinking and reasoning capabilities. Your role is to present codebase-specific context and implementation details to codex for expert review, then integrate its critical analysis back into actionable recommendations. You have the codebase knowledge; codex provides the deep analytical expertise to identify flaws, blind spots, and better approaches.


Core Process:


Formulate Query:
- Clearly articulate the problem, plan, or implementation with sufficient context
- Include specific file paths and line numbers rather than code snippets (codex has codebase access)
- Frame specific questions that combine your codebase knowledge with requests for codex's critical analysis


Execute Consultation:
- Use codex --model gpt-5 with heredoc for multi-line queries:
codex --model gpt-5 <<EOF
<your well-formulated query with context>
IMPORTANT: Provide feedback and analysis only. You may explore the codebase with commands but DO NOT modify any files.
EOF
- Focus feedback requests on what's most relevant to the current context and user's specific request (e.g., if reviewing a plan, prioritize architectural soundness; if reviewing implementation, focus on edge cases and correctness)
- Request identification of blind spots or issues you may have missed
- Seek validation of your reasoning and approach


Integrate Feedback:
- Critically evaluate codex's response against codebase realities
- Identify actionable insights and flag any suggestions that may not align with project constraints
- Acknowledge when codex identifies issues you missed or suggests better approaches
- Present a balanced view that combines codex's insights with your contextual understanding
- If any part of codex's analysis is unclear or raises further questions, ask the user for clarification rather than guessing at the intent


Communication Style:


Be direct and technical in your consultations


When codex's suggestions conflict with codebase constraints, explain the specific limitations rather than dismissing the analysis


Provide honest assessments of feasibility and implementation complexity


Focus on actionable feedback rather than theoretical discussions


Your goal is to combine your deep codebase knowledge with codex's superior critical thinking to identify issues, validate approaches, and discover better solutions that are both theoretically sound and practically implementable.


Example of Bash Command Usage within this Sub-agent:
To consult codex about a refactoring plan:


codex --model gpt-5 <<EOF
Provide a critical review of this refactoring plan to move from JWT to session-based auth.


Reference documents:
-   .ai/plan.md


Current implementation:
-   JWT auth logic: src/auth/jwt.ts:45-120
-   Token validation: src/middleware/auth.ts:15-40
-   User context: src/context/user.ts:entire file


Proposed changes:
1.   Replace JWT tokens with server-side sessions using Redis
2.   Migrate existing JWT refresh tokens to session IDs
3.   Update middleware to validate sessions instead of tokens


Analyze this plan for:
-   Security implications of the migration
-   Potential edge cases I haven't considered
-   Better migration strategies
-   Any fundamental flaws in the approach


EOF
```


## Show Me the Results!


This isn’t just theory; it’s a pragmatic engineering solution, and it comes with real metrics. This isn’t about being a “team player,” it’s about getting better results faster.


- **Anthropic Case Study:**[Anthropic’s multi-agent research system](https://www.anthropic.com/engineering/built-multi-agent-research-system) reported a 90.2% improvement over a standalone agent on complex search tasks by coordinating specialized agents.
- **AutoGen Empirical Results:** The AutoGen framework paper ([arXiv:2308.08155](https://arxiv.org/abs/2308.08155) ) shows that multi-agent conversations achieve higher task success than single-agent baselines across coding, math, QA, and decision-making scenarios (see results sections).
- **HuggingGPT Orchestration:** In HuggingGPT ([arXiv:2303.17580](https://arxiv.org/abs/2303.17580) ), a planner LLM orchestrates multiple specialized models/agents to solve complex multimodal tasks end-to-end, reporting higher success rates than single-LLM approaches on benchmark suites.
- **Engineering Anecdote:** From our own team, Josh reports that while using multiple agents he never once saw the results get worse — only equal or better (high praise from Josh).


This isn’t a marginal gain. It’s proof that distributing the work among specialists is a force multiplier, giving you an advantage no single agent can match.


## Common Headaches & How to Fix ‘Em


Even the best teams have their off days. Here’s what to do when your multi-agent system gets its wires crossed.


### Oops, It Broke: What to do when your agents get confused.


Misalignment between agents is a common problem. One agent might misinterpret another’s output, or they might get stuck in a loop. The key is to have a clear “manager” agent that can step in, clarify the goal, and get things back on track. For a deeper dive on this topic, check out this article on[failure modes in multi-agent systems](https://www.marktechpost.com/2025/03/25/understanding-and-mitigating-failure-modes-in-llm-based-multi-agent-systems/) .


### The Cost of Knowledge: Managing token usage and costs.


More agents mean more API calls, which can get expensive. Be smart about when you call a sub-agent. Use them for high-value tasks where a second opinion really matters, not for every little thing.


### Making it Robust: Building a system that won’t fall apart.


Your multi-agent system needs to be resilient. That means having good error handling, clear communication protocols between agents, and a way to monitor their performance so you can catch problems before they get out of hand.


## Wrapping It Up: The Future of Collaborative AI


Building a multi-agent system in Claude Code is about more than just getting a “second opinion.” It’s a shift in how you think about AI-assisted development. You’re moving from a one-person show to an elite, specialized team. By giving your AI agents a specific job, you eliminate the single points of failure, get better answers, and tackle problems that would make a lone agent go into an infinite loop. It’s the next logical step in[AI agent architecture](https://retool.com/blog/agent-architecture) , and it’s founded on the one principle every good developer knows: two heads are better than one.
