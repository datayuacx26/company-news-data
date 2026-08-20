---
schema_version: "1.0.0"
document_id: "631eac9d3f7198e9ed2174164d12cde361c6241972918dee7899e681c570fada"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/why-prompt-injection-detection-fails"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:b5f10031b369cd4eaaa00b1684505b6aac771fdf63a800fb21cfe5babb065bc6"
---

# Why prompt injection detection fails, and what holds up

*Defenses against prompt injection in AI agents fall into two groups with very different track records. Defenses that inspect untrusted text, through prompting, adversarial training, or a detection model, report strong results against fixed test attacks but are broken above 90% once an attacker adapts to them. Defenses that constrain the agent's actions, deciding what it may do and where data of a given origin may flow before any untrusted content is read, hold up because they never depend on recognizing the attack. This post works through three papers, AgentDojo, The Attacker Moves Second, and CaMeL, that together make the case, and draws out what it means for anyone deploying agents in production.*


## Agents can be hijacked through the data they read


In June 2025, security researchers at Aim Security disclosed EchoLeak (CVE-2025-32711), a vulnerability in Microsoft 365 Copilot rated critical at CVSS 9.3 \[[1](https://www.catonetworks.com/blog/breaking-down-echoleak) ,[2](https://doi.org/10.48550/arXiv.2509.10540) ,[3](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-32711) \]. The attack needed no user interaction. The attacker sent the victim an email whose text was written to read as an ordinary message to a person, with no mention of AI or Copilot, so that Microsoft's cross-prompt injection classifier would not flag it. Later, when the user asked Copilot an unrelated question and its retrieval pulled that email into context, the hidden instruction executed and Copilot exfiltrated internal data to an attacker-controlled destination[\[1\]](https://www.catonetworks.com/blog/breaking-down-echoleak) .


Every action Copilot took in this attack was one it was allowed to take. It could read the user's email, it could read internal files, and it could embed a link or image that triggers an outbound request. The attacker added no new permissions and altered no code. Instead, they supplied an instruction, placed it in data the agent was always going to read, and the agent carried it out with the privileges it already held.


### No boundary between instructions and data


This is the mechanism behind *indirect prompt injection* , a term introduced by Greshake et al. in 2023[\[4\]](https://doi.org/10.1145/3605764.3623985) . Direct prompt injection is when the user typing to the model tries to override its instructions. Indirect prompt injection is more serious because the malicious instruction does not come from the user at all: it arrives inside third-party data that the agent processes while doing its job, such as a retrieved document or an incoming email. The agent has no reliable way to separate an instruction it should follow from text it should merely read, because to a language model both are simply tokens in the same context window[\[5\]](https://doi.org/10.48550/arXiv.2403.06833) . That missing boundary between instructions and data is the root of the problem, and it is why the defenses examined in this post are harder to get right than they first appear.


Definition: Trusted and untrusted input


In this post, trusted and untrusted refer to origin, not safety. Trusted input comes from the principal the agent acts for, meaning its user and its developer. Untrusted input is the third-party content the agent encounters while working, such as fetched web pages, received emails, and tool outputs.


### The lethal trifecta


EchoLeak was an instance of a general condition rather than a flaw specific to Copilot. The security researcher Simon Willison named this condition the lethal trifecta[\[6\]](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta) : an agent is exposed to serious injection risk when it combines three capabilities, access to private data, exposure to untrusted content, and the ability to communicate externally. Figure 1 shows the combination. Any one or two of these is manageable: an agent that reads untrusted web pages but holds no private data has little to leak, and an agent with private data that cannot communicate outward has nowhere to send it. The danger appears when all three are present at once, because an injected instruction in the untrusted content can then reach the private data and carry it out. The difficulty is that useful agents tend to end up with all three. An assistant that reads your email, drafts on your behalf, and sends messages has every one, and removing any of them often removes the reason to build the agent in the first place.


Figure 1: The lethal trifecta. An agent that combines access to private data, exposure to untrusted content, and the ability to communicate externally is exposed to data exfiltration through prompt injection. The term was coined by Simon Willison \[6\].


Microsoft patched EchoLeak by closing the specific chain it used and reported no exploitation in the wild. However, the vulnerability class it belongs to is a structural property of how current agents work rather than a defect in one product, and any system that lets a language model act on data it did not fully control is exposed to some form of it. The question this post takes up is which defenses against that class actually hold, and which only appear to.


## Measuring prompt injection with AgentDojo


To reason about defenses you first need a way to measure the attack, and that is what AgentDojo provides. Released by researchers at ETH Zurich in 2024, it is a benchmark of realistic agent tasks across four environments: a workspace with email, calendar, and cloud storage, a Slack workspace, a travel booking agent, and an e-banking agent[\[7\]](https://doi.org/10.48550/arXiv.2406.13352) . The agent is given an ordinary task, such as summarizing recent emails or paying a bill, and completes it by calling real tools against a stateful environment. Into the data those tools return, the benchmark plants an attacker's instruction, then measures what happens.


Two things are measured separately. Utility is whether the agent completed the user's task. Attack success is whether the attacker's injected goal was carried out, such as a security code sent to an outside address. Both are checked programmatically against the environment state after the run, not judged by another model. That choice matters, because a benchmark that used an LLM to grade success would itself be vulnerable to the injection it was measuring, since the instruction that hijacks the agent could hijack the judge. Checking the environment state directly avoids grading the attack with a component the attack can corrupt.


The baseline results show how exposed an undefended agent is. GPT-4o, the most capable model on the benchmark at the time, completed 69% of tasks with no attack present, and against it the benchmark's generic "important message" injection reached a targeted success rate of 47.7%[\[7\]](https://doi.org/10.48550/arXiv.2406.13352) . That injection is not sophisticated: a note in the tool output telling the agent an important task must be done first, addressed to the model and signed with the user's name, and roughly half the time that was enough. The paper also reports that more capable models tended to be easier to attack, because following an injected instruction competently is itself a capability. The better the agent is at doing what it is told, the better it is at doing what the attacker tells it.


The rate depends heavily on how much of the agent's input the attacker controls. In the Slack environment, where the agent reads web pages and messages and the attacker controls a large share of what comes back, attack success reached 92%[\[7\]](https://doi.org/10.48550/arXiv.2406.13352) . That describes the realistic case: any agent that reads from the open web, processes outside documents, or handles email operates in conditions closer to Slack than to a sealed sandbox. Even when the injection failed to achieve its goal, it frequently derailed the agent, with most models losing 10 to 25 points of utility under attack, so injection is a reliability problem as well as a confidentiality one.


AgentDojo ships with a set of defenses and is built to be extended with new ones. A benchmark is only as strong as the attacks it measures against, and the defenses evaluated here were measured against a fixed set of known injection payloads. Next, we will see why that qualification is so important.


## The attacker moves second


The defenses AgentDojo shipped with, and most defenses in the literature, are measured the same way: against a fixed set of injection payloads collected in advance. A defense that blocks those payloads reports a low attack success rate and looks robust. In 2025, a group of researchers from OpenAI, Anthropic, Google DeepMind, and ETH Zurich argued that this number means almost nothing, and demonstrated why[\[8\]](https://doi.org/10.48550/arXiv.2510.09023) .


Their argument comes from security's oldest assumption. A defense should be evaluated against an attacker who knows how the defense works and adapts to it, an idea that goes back to Kerckhoffs in the nineteenth century and that cryptography and the wider security field never abandoned[\[8\]](https://doi.org/10.48550/arXiv.2510.09023) . A real attacker does not throw a fixed list of known payloads at a system and give up when they fail. They look at the defense, understand its mechanism, and craft an input designed to defeat that specific mechanism, iterating until they succeed. The attacker moves second, and a defense measured only against attacks written before it existed has been tested under conditions no real attacker will reproduce.


To show what a defense's robustness looks like under this standard, the authors built adaptive attacks using four general techniques, gradient-based optimization, reinforcement learning, search, and human red-teaming, each following the same loop: propose a candidate input, observe how the defense responds, refine, and repeat[\[8\]](https://doi.org/10.48550/arXiv.2510.09023) . They then turned these attacks on twelve recently published defenses spanning every major category: prompting defenses, adversarially trained models, detector models, and secret-knowledge schemes. The reversal was consistent across all of them, as shown in Figure 2. The defenses had reported near-zero attack success rates in their original papers. Under adaptive attack, most were driven above 90%[\[8\]](https://doi.org/10.48550/arXiv.2510.09023) . Human red-teamers, given a description of each defense, succeeded on every scenario they were tested against.


Figure 2: Attack success rates for twelve published defenses, grouped by category. Green bars are the rates reported in each defense's original paper, measured against static attacks. Orange bars are the rates under adaptive attack. The rightmost bar shows human red-teaming, which succeeded on every scenario. Adapted from Nasr et al. (2025) \[8\].


The paper is careful about what this does and does not prove. An empirical evaluation can only show that a defense withstood the attacks that were tried against it, and a defense that survives one team's attacks may still fall to the next. The result does establish the opposite direction with confidence. A defense that reports a low attack success rate against a fixed set of payloads has shown almost nothing about its robustness, because that is not the condition under which it will be attacked. This reframes every number in the previous section and every defense in the next two. When a defense claims to reduce attack success from fifty percent to near zero, the question that matters is what happens when the attacker moves second, rather than whether it blocks the known payloads.


### Prompting and training defenses


The most immediate response to injection is to tell the model not to fall for it. Prompting defenses do exactly this. Spotlighting, from Hines et al. at Microsoft, marks the untrusted portions of the input with special delimiters and instructs the model to disregard any instructions inside them[\[9\]](https://doi.org/10.48550/arXiv.2403.14720) , and prompt sandwiching repeats the user's real task after the untrusted data so the model does not lose track of it[\[10\]](https://learnprompting.org/docs/prompt_hacking/defensive_measures/sandwich_defense) . Against static attacks these look reasonable, cutting attack success to the low tens of percent. Under adaptive attack both exceed 95%, as Figure 2 shows. The reason is structural: a prompting defense is an instruction, and the injected text is also an instruction, competing for the same model's attention in the same context window. Telling the model to prefer one over the other adds a hurdle that the attacker can phrase around rather than a boundary they cannot cross, and a payload framed as a legitimate part of the user's task steps past the instruction entirely.


The next response is to train the model to resist injection rather than merely ask it to. StruQ separates the user's instructions and the untrusted data into two channels and fine-tunes the model to follow only the first[\[11\]](https://doi.org/10.48550/arXiv.2402.06363) . Its successor MetaSecAlign, from an overlapping group at Berkeley and Meta, reformulates the same idea as preference optimization and improves on StruQ's robustness by a wide margin[\[12\]](https://doi.org/10.48550/arXiv.2507.02735) . However, the category inherits a limitation from a decade of adversarial machine learning: training against a fixed set of attacks teaches resistance to those attacks and ones like them, not to attacks drawn from outside that distribution[\[8\]](https://doi.org/10.48550/arXiv.2510.09023) . Because the space of possible injections is open-ended, an adaptive attacker simply operates outside the training distribution, and under adaptive attack these defenses also fall, MetaSecAlign to 96% and StruQ to 100%.


### Detection (Filter) models


If the model cannot be trusted to resist injection on its own, the next idea is to place a separate model in front of it whose only job is to catch injections before they reach the agent. This is the detection approach, and it is the one most widely deployed in practice. A detector is typically a smaller classifier, often a fine-tuned BERT model[\[13\]](http://arxiv.org/abs/1810.04805) , trained on examples of benign and malicious inputs and asked to flag anything that looks like an injection[\[8\]](https://doi.org/10.48550/arXiv.2510.09023) . When the AgentDojo team tested such a detector, it cut attack success to around 8%, the lowest of any defense they evaluated, so on the security axis alone detection appears to be the winner[\[7\]](https://doi.org/10.48550/arXiv.2406.13352) .


It fails on two axes at once, however. The first is utility, because the same detector had a false positive rate high enough to degrade the agent's performance on legitimate tasks, flagging harmless inputs as attacks often enough to interfere with ordinary work[\[7\]](https://doi.org/10.48550/arXiv.2406.13352) . This is the practical tax of detection: a classifier tuned to catch more injections also catches more of the benign inputs that resemble them, while a classifier tuned to interfere less catches fewer injections. No setting of the threshold escapes the tradeoff, only different points along it.


The second axis is the one The Attacker Moves Second exposes. A detector is itself a machine learning model, which makes it vulnerable to the same adaptive attacks as the model it protects. The authors tested four production and published detectors, Protect AI, PromptGuard, PIGuard, and Google's Model Armor, and drove attack success above 90% against most of them, with the most robust reaching only 71%[\[8\]](https://doi.org/10.48550/arXiv.2510.09023) . Feeding the detector's own confidence score back to the attacker, so that each attempt reveals how close it came to slipping through, turns bypassing the detector into a straightforward optimization problem.


More decisive than the numbers, however, is the shape of the payloads that beat the detector, because an injected instruction and a legitimate instruction are not different in form. One trigger that defeated a detector was simply a note claiming that, per company policy, calendar reschedules must be logged by deleting the corresponding tracking file, with a specific file ID given on the cloud drive[\[8\]](https://doi.org/10.48550/arXiv.2510.09023) . There is nothing in that sentence for a classifier to flag, no override language and no suspicious tokens, yet it instructs the agent to delete a file. An injection that reads as normal work is a case detection cannot catch.


## Constraining actions instead of judging text


Every defense so far operates on the same terrain as the attack: all of them judge the untrusted text, and all of them lose to an attacker who shapes text a model reads as legitimate. The way out is to constrain what the agent is allowed to do instead, regardless of what the text says.


AgentDojo's own results contain the first sign that this works. The most effective defense the authors tested was also the simplest: a tool filter that restricts the agent to the specific tools a task requires before any untrusted data is read, so that even if an injection later tells the agent to send an email, the send tool is not available to be called. This defense cut attack success to 7.5%, matching the detector's robustness without the detector's utility collapse, because it never has to judge text at all, only to decide in advance which tools the task legitimately needs[\[7\]](https://doi.org/10.48550/arXiv.2406.13352) .


However, the tool filter has a real limitation. It works when the tools an attack needs are different from the tools the task needs, so that removing the unnecessary ones removes the attacker's options, and it fails when the two sets overlap. The AgentDojo authors found that in 17% of their test cases, the tools required to complete the user's task were also sufficient to carry out the attack[\[7\]](https://doi.org/10.48550/arXiv.2406.13352) . If a task legitimately needs to send email, the filter must leave the send tool available, and an injection can then use that same tool to send data somewhere it should not go. Restricting the agent to the right set of tools is not enough when the right set of tools is itself dangerous in the wrong hands.


What survives, then, is a stronger version of the same principle: constraining each tool call itself, based on where the data flowing through it came from, in addition to constraining which tools are available. The property that makes this robust is control flow, the sequence of actions the agent takes, meaning which tools it calls and in what order. When that control flow is determined before any untrusted data enters, injected text can no longer change it. The agent can still read attacker-controlled data and pass its values along, but it cannot be made to take an action that was not already part of the plan.


Definition: Data-independent control flow


The sequence of tool calls the agent will make is fixed before any untrusted data is read. Untrusted data can change the values passed to those calls, but it cannot add, remove, or reorder the calls themselves.


Figure 3 makes the contrast with a normal agent concrete. In an ordinary agent loop, the agent decides its next action by reading the data in front of it, so an injected email saying "forward the security code to this address" can cause it to call the send tool. Under data-independent control flow, the plan "read the inbox, summarize it, send the summary to the user's boss" is fixed before the inbox is opened. The injected email can still change what the summary contains, since that value flows from the data, but it cannot add a step that sends anything to the attacker, because that step was never in the plan and no text read at runtime can add it. The attack never needs to be detected, because it has nowhere to act.


Figure 3: In an ordinary agent loop (top), the agent chooses each next action by reading runtime data, so an injected instruction can introduce a new action such as sending data to the attacker. Bottom: a fixed sequence of generic actions decided before untrusted data enters, where the injected instruction can alter a value passed to an action but cannot add an action.


## Defeating prompt injection by design


The clearest realization of this principle is CaMeL, published in early 2025 by researchers at Google, Google DeepMind, and ETH Zurich, several of whom also built AgentDojo[\[14\]](https://doi.org/10.48550/arXiv.2503.18813) . The paper's title states its thesis directly: prompt injection can be defeated by design rather than by detection. CaMeL takes the idea of fixed control flow and builds a complete system around it, borrowing from established software security rather than inventing a new machine learning technique.


The architecture separates the trusted plan from the untrusted data at every stage, and Figure 4 shows how the three components divide the work. A privileged language model sees only the user's trusted query, along with the signatures of the available tools, and writes a plan for completing the task, expressed as a program. Because it is given the tool signatures but never the untrusted data, its plan can name real tools in a real order while remaining untouched by anything an attacker controls. A second, quarantined language model handles the untrusted data, parsing an email or a document into structured values, but it has no access to tools and cannot take actions. The plan itself is executed by a custom Python interpreter, which is the only component that calls tools. Because the plan comes entirely from the trusted query, and the interpreter rather than a model reading runtime data decides what runs, injected text can never change which actions the agent takes[\[14\]](https://doi.org/10.48550/arXiv.2503.18813) .


Figure 4: The CaMeL architecture. The privileged LLM sees only the trusted query and the available tool signatures and writes a plan as code. The quarantined LLM reads untrusted data and returns structured values but cannot call tools. The interpreter runs the plan, is the only component that executes actions, and checks each call against policy. Adapted from Debenedetti et al. (2025) \[14\].


Fixed control flow alone does not stop everything, because an attacker can still corrupt the values that flow through the plan, and this is where CaMeL's second mechanism comes in. Every value carries metadata recording its provenance together with what may be done with it, a construct known in software security as a capability[\[14\]](https://doi.org/10.48550/arXiv.2503.18813) . The interpreter propagates these capabilities as values move through the program and checks them against security policies at each tool call. A policy can express, for example, that data derived from a private document may not be sent to a recipient not authorized to see it. When the plan reaches a tool call that would violate a policy, the interpreter blocks it, regardless of what any injected instruction asked for.


Definition: Provenance


The recorded origin of a value: which source it came from and what it was derived from. Enforcement decisions are made against provenance rather than against the content of the text, because an attacker controls what their text says but not where it came from.


This is information flow control, a discipline that traces back to Denning's work in the 1970s, applied to language model agents[\[15\]](https://doi.org/10.1145/360051.360056) . Framed this way, prompt injection becomes a data flow problem that classical security already has tools for, rather than a novel AI safety puzzle that better models will eventually solve. That framing is what lets CaMeL make a claim none of the earlier defenses could. On AgentDojo it completes 77% of tasks with provable security, meaning no policy-violating data flow is possible, against 84% for the same agent with no defense at all[\[14\]](https://doi.org/10.48550/arXiv.2503.18813) . The security is a property of the system around the model rather than of the model's ability to resist.


What provable security means here deserves precision, and the authors are careful about it. Given the policies, the interpreter guarantees that no data flow those policies forbid can occur. It does not mean the policies capture every possible harm, nor that the content of quarantined values is trustworthy. An injection can still corrupt the content of a value even when it cannot redirect control, and a poisoned document can supply a wrong answer as long as no policy blocks that flow. Writing correct policies is itself demanding, which is the historical difficulty of information flow control: it has long offered sound guarantees at the cost of the labor required to specify and maintain its labels and policies. Furthermore, the design's remaining surface, the content of parsed values and the correctness of the policies themselves, has not yet faced the kind of adaptive evaluation the earlier defenses failed. CaMeL closes the control flow channel with a strong guarantee and is open about the channels it does not close.


The guarantee also has a cost beyond policy authoring. CaMeL requires two separate language models, a generated plan, and a custom interpreter to run it, which is a significant departure from how agents are typically built today. This is the tradeoff the design makes: strong, provable control flow security in exchange for a rearchitected agent and the ongoing work of writing policies. Whether that same class of guarantee can be delivered with less machinery, and applied to agents built in the ordinary way, is the question the rest of this line of work, including our own, is taking up.


## What this means for deploying agents in production


The evidence across these papers supports one compact conclusion. Defenses that judge untrusted text, whether by prompting the model, training it, or placing a classifier in front of it, report strong numbers against the attacks used in their evaluations and fail once the attacker adapts to them. Defenses that constrain the agent's actions hold up, and they hold up for a structural reason: they do not depend on recognizing the attack at all. The one category the strongest published attack framework does not claim to break is the one where control flow is fixed before untrusted data is read, and the open problems that remain there are coverage, integration cost, and the work of writing policies rather than robustness.


None of this argues against defense in depth. Agent security will be layered, as security always is, and detectors and hardened prompts belong in those layers because they filter opportunistic attacks cheaply and raise the cost of everything else. A layer whose effectiveness depends on a model classifying adversarial text correctly cannot serve as the security boundary, because that is precisely the property an adaptive attacker optimizes against. The question to put to any defense is therefore what happens when the attacker knows the mechanism and adapts to it, and a reported attack success rate is a static-attack number unless the evaluation says otherwise. The boundary itself has to be a deterministic constraint that does not evaluate the text at all, a rule for which actions can run and for where data of a given provenance is allowed to flow. Provenance is the property to build on, because an attacker controls what their text says but not where it came from.


The engineering question this leaves open is the one CaMeL's tradeoff defines: whether provenance-based enforcement can be brought to agents built in the ordinary way, without a second model, a generated plan, and a custom interpreter, and without giving up the deterministic guarantee that makes the approach worth having. We are building Interbolt, an open source enforcement layer that takes exactly this point in the design space, and we are evaluating it on the same benchmark these papers use. The results will be the subject of a future post.


---


## References


\[1\] Ravia, Itay. "Breaking down 'EchoLeak', the First Zero-Click AI Vulnerability Enabling Data Exfiltration from Microsoft 365 Copilot." Aim Labs / Cato Networks, May 31, 2025.[https://www.catonetworks.com/blog/breaking-down-echoleak](https://www.catonetworks.com/blog/breaking-down-echoleak)


\[2\] Reddy, Pavan, and Aditya Sanjay Gujral. “EchoLeak: The First Real-World Zero-Click Prompt Injection Exploit in a Production LLM System.” arXiv:2509.10540. Preprint, arXiv, September 6, 2025.[https://doi.org/10.48550/arXiv.2509.10540](https://doi.org/10.48550/arXiv.2509.10540) .


\[3\] Microsoft. 2025. CVE-2025-32711, MSRC security update guide.[https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-32711](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-32711) .


\[4\] Greshake, Kai, Sahar Abdelnabi, Shailesh Mishra, Christoph Endres, Thorsten Holz, and Mario Fritz. “Not What You’ve Signed Up For: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection.” Proceedings of the 16th ACM Workshop on Artificial Intelligence and Security, November 30, 2023, 79–90.[https://doi.org/10.1145/3605764.3623985](https://doi.org/10.1145/3605764.3623985) .


\[5\] Zverev, Egor, Sahar Abdelnabi, Soroush Tabesh, Mario Fritz, and Christoph H. Lampert. “Can LLMs Separate Instructions From Data? And What Do We Even Mean By That?” arXiv:2403.06833. Preprint, arXiv, January 31, 2025.[https://doi.org/10.48550/arXiv.2403.06833](https://doi.org/10.48550/arXiv.2403.06833) .


\[6\] Willison, S. 2025. The lethal trifecta for AI agents: private data, untrusted content, and external communication.[https://simonwillison.net/2025/Jun/16/the-lethal-trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta) .


\[7\] Debenedetti, Edoardo, Jie Zhang, Mislav Balunović, Luca Beurer-Kellner, Marc Fischer, and Florian Tramèr. “AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents.” arXiv:2406.13352. Preprint, arXiv, November 24, 2024.[https://doi.org/10.48550/arXiv.2406.13352](https://doi.org/10.48550/arXiv.2406.13352) .


\[8\] Nasr, Milad, Nicholas Carlini, Chawin Sitawarin, et al. “The Attacker Moves Second: Stronger Adaptive Attacks Bypass Defenses Against Llm Jailbreaks and Prompt Injections.” arXiv:2510.09023. Preprint, arXiv, October 10, 2025.[https://doi.org/10.48550/arXiv.2510.09023](https://doi.org/10.48550/arXiv.2510.09023) .


\[9\] Hines, Keegan, Gary Lopez, Matthew Hall, Federico Zarfati, Yonatan Zunger, and Emre Kiciman. “Defending Against Indirect Prompt Injection Attacks With Spotlighting.” arXiv:2403.14720. Preprint, arXiv, March 20, 2024.[https://doi.org/10.48550/arXiv.2403.14720](https://doi.org/10.48550/arXiv.2403.14720) .


\[10\] Sander Schulhoff. The sandwich defense: Strengthening AI prompt security.[https://learnprompting.org/docs/prompt_hacking/defensive_measures/sandwich_defense](https://learnprompting.org/docs/prompt_hacking/defensive_measures/sandwich_defense) , October 2024.


\[11\] Chen, Sizhe, Julien Piet, Chawin Sitawarin, and David Wagner. “StruQ: Defending Against Prompt Injection with Structured Queries.” arXiv:2402.06363. Preprint, arXiv, September 25, 2024.[https://doi.org/10.48550/arXiv.2402.06363](https://doi.org/10.48550/arXiv.2402.06363) .


\[12\] Chen, Sizhe, Arman Zharmagambetov, David Wagner, and Chuan Guo. “Meta SecAlign: A Secure Foundation LLM Against Prompt Injection Attacks.” arXiv:2507.02735. Preprint, arXiv, February 6, 2026.[https://doi.org/10.48550/arXiv.2507.02735](https://doi.org/10.48550/arXiv.2507.02735) .


\[13\] Devlin, Jacob, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. “BERT: Pre-Training of Deep Bidirectional Transformers for Language Understanding.” arXiv:1810.04805 \[Cs\], May 24, 2019.[http://arxiv.org/abs/1810.04805](http://arxiv.org/abs/1810.04805) .


\[14\] Debenedetti, Edoardo, Ilia Shumailov, Tianqi Fan, et al. “Defeating Prompt Injections by Design.” arXiv:2503.18813. Preprint, arXiv, June 24, 2025.[https://doi.org/10.48550/arXiv.2503.18813](https://doi.org/10.48550/arXiv.2503.18813) .


\[15\] Denning, Dorothy E. “A Lattice Model of Secure Information Flow.” Communications of the ACM 19, no. 5 (1976): 236–43.[https://doi.org/10.1145/360051.360056](https://doi.org/10.1145/360051.360056) .
