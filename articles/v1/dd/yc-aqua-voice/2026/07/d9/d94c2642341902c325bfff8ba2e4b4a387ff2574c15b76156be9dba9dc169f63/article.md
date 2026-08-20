---
schema_version: "1.0.0"
document_id: "d94c2642341902c325bfff8ba2e4b4a387ff2574c15b76156be9dba9dc169f63"
company_key: "yc-aqua-voice"
company: "Aqua Voice"
source_id: "yc-aqua-voice-news-import-16e95c4a8dc7"
canonical_url: "https://aquavoice.com/blog/voice-dictation-for-zed"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-21T07:23:55.387885+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:98ef380816243310223b4b20a9320540f3f6512c9e20c45c7d795e4ac868e357"
---

# Voice Dictation for Zed (2026)

Zed does not ship built-in voice dictation. There is no microphone in the Agent Panel, the Inline Assistant, or the editor, and the mic you may have seen in Zed is for collaboration voice chat with teammates, not speech-to-text. Dictation has been an open request on Zed's own tracker since August 2024. So the working answer for Zed is a **system-wide dictation layer** like[Aqua](https://aquavoice.com/) : one push-to-talk key that inserts text at your cursor in every Zed surface and every other app, tuned to get code identifiers right.


That is the short version. Below: what Zed actually offers today, why an editor built around parallel agents makes voice worth more than it is in a conventional IDE, and the setup that covers all of Zed.


## Does Zed have built-in dictation? No, and here is the evidence


This is worth stating plainly because the last few editors we covered muddied the water. VS Code ships an official speech extension. Windsurf shipped Voice Mode for Cascade. Warp ships native voice input in the terminal. Zed ships none of these.


What exists instead is a request queue. Zed's issue tracker has carried[Dictation for Assistant (inline and chat-based)](https://github.com/zed-industries/zed/issues/16410) since **August 2024** , asking for a microphone in the inline assistant so you can dictate to the agent and approve the text before it goes to the model. It sits in Zed Community Requests marked Planned, which means acknowledged, not shipped. A separate 2026 request asks for microphone input in the **Agent Panel** for external ACP agents, pointing out that the Agent Client Protocol already defines audio content blocks and an` audio` prompt capability, so the plumbing exists upstream even though Zed does not surface it.


Planned is not shipped. As of this writing there is nothing to enable in Zed's settings.


### The mic in Zed is not a dictation mic


Here is the near-miss that sends people in circles. Zed has real, well-built **collaboration** features: channels, screen sharing, and voice chat, with a microphone control in the top right of the window that you can mute and unmute during a call. That mic carries your voice to another human on the call. It does not transcribe anything, and nothing it hears ever becomes text in your buffer.


If you clicked it expecting dictation, that is why nothing appeared at your cursor. They are unrelated features that happen to share an icon.


## Why Zed in particular rewards voice


Zed's pitch is speed: a Rust-built, GPU-accelerated editor that stays responsive at 120fps. Then it layers on the agentic surfaces, and that is where the real bottleneck moved.


Zed now runs **Parallel Agents** , multiple agents working at once in the same window with a Threads Sidebar to monitor them, plus **external agents over ACP** , the open protocol Zed co-developed with JetBrains and announced in January 2026. Add the **Agent Panel** , the **Inline Assistant** , and **Text Threads** , and the shape of the work changes: you are not typing code most of the day, you are writing prose that directs several agents at once.


Think about what that costs at a keyboard. Every agent you run in parallel needs its own instruction, its own follow-up, its own correction. The editor is fast, the models are fast, and the slowest component is you typing a paragraph of intent into a panel. Speaking runs several times faster than typing, which is precisely the constraint an agent-first editor puts pressure on. Zed optimized microseconds of input latency and then asked you to hand-type your prompts.


The catch is that those prompts are dense with the words general speech models handle worst:` AgentServerSettings` ,` zeta` ,` cargo nextest run` ,` crates/agent_ui/src` , a crate name, a teammate's handle. A model trained on audiobooks and podcasts hears "zeta" as the Greek letter and "cargo" as freight. In a document you would skim past it. In an agent prompt, one mangled identifier sends the agent editing the wrong symbol, and you spend the time you saved cleaning up. That is the same reason we wrote up[speech-to-text for Cursor](https://aquavoice.com/blog/best-speech-to-text-for-cursor) and[voice dictation for Windsurf](https://aquavoice.com/blog/voice-dictation-for-windsurf) : agent surfaces reward voice and punish an engine that cannot spell your codebase.


## What voice dictation for Zed actually has to do


Four requirements, given Zed has nothing native to build on:


1. **Code-identifier accuracy.** It writes` Zeta` ,` cargo nextest run` ,` crates/agent_ui/src` and` AgentServerSettings` as written, not as a phonetic guess. Aqua runs its own proprietary model, **Avalon** , trained on human-computer-interaction speech (prompts, code, commands, email) rather than audiobooks. In a 9to5Mac side-by-side against built-in macOS Dictation on the same passage, Apple made 17 errors and Aqua made 1.
2. **Every Zed surface, plus everything outside Zed.** Since there is no per-panel integration to rely on, the layer has to be system-wide: the Agent Panel, the Inline Assistant, a Text Thread, the editor buffer, the integrated terminal, the commit box, and then your browser, Slack and the GitHub PR description without changing tools.
3. **Context awareness.** Aqua's Deep Context reads what is on screen, so when your editor is full of one project's names, transcription leans toward those terms rather than their English near-homophones.
4. **Speed.** Aqua inserts text about 450ms after you release the key. In an editor that markets its frame budget, a dictation tool that makes you wait defeats the point.


On independently checkable footing: Avalon placed **#1 among proprietary models** on the OpenASR leaderboard at its October 2025 debut. Aqua's own AISpeak benchmark, which we label self-reported, puts Avalon at 97.3% on AI and coding jargon. Lead with the checkable proof and treat the in-house number as directional. Aqua covers **49 languages** and runs on Mac, Windows and iPhone.


## How to set up voice dictation in Zed


Because Aqua is an OS-level layer, there is nothing to install inside Zed, no extension to hunt for, and no setting to flip in` settings.json` . You set it up once at the system level and it works in every Zed surface.


1. **Install Aqua** for[macOS or Windows](https://aquavoice.com/) and sign in.
2. **Grant accessibility / input permission** when prompted so Aqua can insert text into other apps. On macOS that is System Settings > Privacy & Security > Accessibility.
3. **Pick your push-to-talk key.** The default is **Fn** (hold to record, release to insert). Hold-to-talk fits an editor, where your bursts are short: a prompt here, a rename there.
4. **Click into any Zed surface:** the Agent Panel, the Inline Assistant, a Text Thread, a code buffer, or the integrated terminal.
5. **Hold Fn, speak, release.** The text lands at the cursor. Read it, then send or run.


The same habit keeps working the moment you leave Zed for a browser tab or Slack.


### A real example in the Agent Panel


Open the Agent Panel, hold Fn, and say:


> "In` crates/agent_ui` , pull the thread-title logic out of the panel component into its own module, and add a test covering the empty-thread case."


Release, and the whole instruction lands in the panel with` crates/agent_ui` intact, then you hit send. No reaching back to repair a mangled path. Running three agents in parallel, that is three prompts spoken in the time it took to type one.


## Dictating code accurately: a few habits


Voice is for **intent** , not for spelling out every character:


- **Speak identifiers naturally.** Say "agent server settings" or "the zeta model" and let a code-tuned model resolve the casing. For an oddly-cased symbol you use constantly, add it to your dictionary once instead of fighting it every time.
- **Let the agent handle boilerplate.** Describe the goal ("extract the thread-title logic into its own module") rather than narrating syntax. The agent writes the code; you supply the direction.
- **Add your constants to the Custom Dictionary.** Crate names, internal libraries, service names, model names, a teammate's handle (up to 800 entries on Pro) so they land correctly every time.
- **Use Custom Instructions** for standing preferences, for example "keep commit messages in conventional-commits format."


You will still type the occasional exact token, and that is fine. Most of what you send an agent is reasoning and instruction, which is what voice is fastest at.


## What about Talon, or waiting for native support?


Two honest alternatives:


- **Talon and Cursorless** are the serious voice-coding stack, built for people who cannot use a keyboard, and they are genuinely powerful. They are also a command grammar you learn, not dictation you just start using, and Cursorless targets editors Zed is not among today. If you want hands-free structural editing and will invest the training time, that path is real. If you want to talk in sentences and have text appear, it is more machinery than you need.
- **Waiting for Zed to ship it** is reasonable if you only ever dictate into the Agent Panel. Given the request has been open since 2024 and the ACP audio capability is defined upstream, native support may well arrive. It would still be one editor's box, and it would not follow you into your browser, Slack, or the PR description.


One honest caveat on Aqua: it is cloud-based, so it needs a network connection. On a fully offline machine, a local tool wins. For everyday connected work, the code accuracy and the everywhere-reach are worth the trade.


On price, Aqua Pro is **$8/mo billed annually** , with a free Starter tier (1,000 words) to try it and a 70% student discount on a .edu email. To build voice into your own tooling, the[Avalon API](https://aquavoice.com/avalon-api) is an OpenAI-compatible endpoint at $0.39 per hour of audio. See[how Aqua compares to Wispr Flow](https://aquavoice.com/vs/wispr-flow) , and the full feature tour is in the[Aqua guide](https://aquavoice.com/guide) .


## The bottom line


Zed built the fastest editor most developers have used, then put parallel agents inside it, and left the input method at the keyboard. That gap is the whole opportunity. When your job is directing several agents in prose, typing is the slow part, and a general dictation engine that cannot spell your crate names just moves the cost from typing to proofreading. A system-wide, code-tuned layer covers every Zed surface today, no extension required, and keeps working in every app after it. We have typed for 150 years. In an editor you increasingly talk to, it is time to speak.


## FAQ


**Does Zed have built-in voice dictation?** No. Zed ships no speech-to-text in the Agent Panel, the Inline Assistant, or the editor. Dictation has been an open request on Zed's issue tracker since August 2024, carried as Planned in Zed Community Requests, and a separate 2026 request asks for microphone input for external ACP agents. The microphone you see in Zed is for collaboration voice chat with teammates, not dictation.


**How do I dictate code in Zed?** Use a system-wide dictation layer, since there is no Zed extension for it. Install Aqua on macOS or Windows, grant accessibility permission, then hold the Fn key, speak, and release. Text is inserted at your cursor in the Agent Panel, the Inline Assistant, a code buffer, or the integrated terminal, and in every app outside Zed.


**What is the most accurate speech-to-text for Zed?** Accuracy on code identifiers, crate names and file paths is what decides it. Aqua runs its own Avalon model, trained on prompts and code rather than audiobooks, and placed #1 among proprietary models on the OpenASR leaderboard at its October 2025 debut. In a 9to5Mac test it made 1 error where macOS Dictation made 17.


**How much does Aqua cost for Zed users?** Aqua Pro is $8/mo billed annually, with a free Starter tier (1,000 words) and a 70% student discount with a .edu email. There is also an Avalon API at $0.39 per hour of audio for building voice into your own developer tooling.
