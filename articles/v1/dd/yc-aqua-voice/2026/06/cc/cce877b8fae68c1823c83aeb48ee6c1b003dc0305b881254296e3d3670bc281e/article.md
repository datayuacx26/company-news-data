---
schema_version: "1.0.0"
document_id: "cce877b8fae68c1823c83aeb48ee6c1b003dc0305b881254296e3d3670bc281e"
company_key: "yc-aqua-voice"
company: "Aqua Voice"
source_id: "yc-aqua-voice-news-import-16e95c4a8dc7"
canonical_url: "https://aquavoice.com/blog/voice-dictation-for-vs-code"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-21T07:23:55.387885+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:aa3c89340e1ce773fbef13dbaf4bd29b99b63305f0c207cbb21dc2edb609f445"
---

# Voice Dictation for VS Code (2026)

# Voice Dictation for VS Code (2026)


June 29, 2026


You can dictate in VS Code two ways in 2026: Microsoft's built-in **VS Code Speech** extension, which runs on-device and free and is best for prose and Copilot Chat, or a **system-wide dictation layer** like[Aqua](https://aquavoice.com/) that works across every app and is tuned to get code identifiers right. Which one depends on how much of your day is code.


The system-wide layer works in the editor, the integrated terminal, Copilot Chat, your browser and Slack with one hotkey, and it gets code-specific words right: function names, library names, file paths and CLI flags. If you mostly write comments and chat prompts, the built-in extension is plenty. If you live in identifiers and bounce between apps, you want the system-wide layer. That is the whole decision, and the rest of this page walks through both so you pick the right one.


## VS Code already has voice. Here is what it does well.


Unlike Cursor or Claude Code, VS Code ships an official voice option. The **VS Code Speech** extension (publisher` ms-vscode` ) landed in version 1.87 back in February 2024 and is now a mature, free download. It is worth understanding before you reach for anything else, because for some workflows it is genuinely the right tool.


What it gets right:


- **It runs locally.** Recordings are computed on your machine and are never sent to an online service. If offline-only or strict data handling is a hard requirement, this is a real advantage.
- **It is free** and maintained by Microsoft.
- **It powers Copilot Chat by voice.** You can say **"Hey Code"** to start a Copilot Chat session, and it understands chat grammar, so phrases like "at workspace" or "slash fix" map to the right` @workspace` and` /fix` commands.
- **Press-and-hold dictation.** Run *Voice: Start Dictation in Editor* (Ctrl+Alt+V on Windows/Linux, Opt+Cmd+V on Mac) and hold the key to dictate, release to stop.
- **26 languages** , each shipped as its own companion extension.


For writing a comment, a commit message, or a quick Copilot prompt, that is a solid, private, no-cost setup. Credit where it is due.


## Where the built-in extension runs out of road


The limits show up the moment you are doing real, fast, multi-surface agent work.


1. **It is a general speech model, not a code model.** VS Code Speech is built for natural language. Dictate a sentence full of identifiers and you get the same problem every general engine has:` useMemo` becomes "use memo,"` pnpm` becomes "P&M,"` git rebase --onto` becomes a phonetic guess. The further you get from prose, the more you fix by hand.
2. **It only works inside VS Code.** The dictation command targets the editor and the chat box. The instant you tab to your browser to read a doc, to Slack to answer a teammate, or to a standalone terminal, the hotkey does nothing. You are managing one voice tool for VS Code and typing everywhere else.
3. **No on-screen context.** It transcribes what it hears, not what you are looking at, so it cannot lean toward the library names already open in your buffer.


None of that makes the extension bad. It makes it a *prose-and-chat* tool. Coding is increasingly a prose-and-chat *and* identifiers *and* every-other-app activity, and that is the gap.


## What "best voice dictation for VS Code" actually means


A dictation tool earns that label for a developer on four things:


1. **Technical-vocabulary accuracy.** It writes` getServerSideProps` ,` tRPC` ,` Tailwind` ,` Supabase` and` npx` as written, not as prose or a mangle. Aqua runs its own proprietary model, **Avalon** , trained on human-computer-interaction speech (prompts, code, email) instead of audiobooks and podcasts. In a 9to5Mac side-by-side against built-in macOS Dictation on the same passage, Apple made 17 errors and Aqua made 1.
2. **Context awareness.** Aqua's Deep Context reads what is on screen, so when your editor is full of one library's identifiers, the transcription leans toward those terms instead of their English near-homophones.
3. **One layer, every surface.** The same hotkey should work in the editor, the integrated terminal, Copilot Chat, your browser and your chat app. A system-wide tool inserts text wherever the cursor is, so you learn one motion and use it everywhere.
4. **Speed.** Aqua inserts text in roughly 450ms after you release the key. When you are firing short instructions at an agent, that latency is the difference between flow and waiting.


On independently checkable footing: Avalon placed #1 among proprietary models on the OpenASR leaderboard at its October 2025 debut. Aqua's own AISpeak benchmark, which we label self-reported, puts Avalon at 97.3% on AI and coding jargon versus far lower scores for general-purpose engines. Aqua also covers **49 languages** , against the extension's 26.


## How to set up system-wide voice dictation for VS Code


Because Aqua is an OS-level layer, there is nothing to install inside VS Code and no extension to configure. Setup is at the system level and then it works in every VS Code surface and every other app.


1. **Install Aqua** for[macOS or Windows](https://aquavoice.com/) and sign in. (There is an iOS keyboard too, but VS Code is a desktop editor, so use the desktop app here.)
2. **Grant accessibility / input permission** when prompted so Aqua can insert text into other apps. On macOS this is System Settings > Privacy & Security > Accessibility.
3. **Pick your push-to-talk key.** The default is **Fn** (hold to record, release to insert). Hold-to-talk beats a toggle for coding because your bursts are short.
4. **Open VS Code and click into any input** that takes text: a file in the editor, the Copilot Chat box, or the integrated terminal.
5. **Hold Fn, speak, release.** The text appears at the cursor. In Copilot Chat, press Enter to send.


That is the whole loop. The same muscle memory works the moment you leave VS Code for a browser, Slack or Gmail.


### A real example in Copilot Chat


Click into the Copilot Chat box, hold Fn, and say:


> "In the auth middleware, wrap the Supabase client call in a try-catch, log the error with our logger util, and return a 401 instead of throwing."


Release. The full instruction lands in the box as written, including` Supabase` ,` try-catch` ,` 401` and` middleware` . You read it, fix nothing, and hit Enter.


### Dictating into the integrated terminal


The integrated terminal is the surface the built-in extension does not really serve. With a system-wide layer, click into it, hold Fn and say "git checkout dash b feature slash voice input." For the commands an agent suggests, speaking the intent and letting the model resolve the exact tokens is faster than reaching back to type a flag-heavy line.


## Dictating code accurately: a few habits


Voice is for **intent** , not for spelling out characters one by one. Describe at the level the agent already understands:


- Say **"the use-effect hook"** and let the model resolve casing. You are talking to an AI that knows` useEffect` ; you do not need to spell it.
- For symbols, speak them naturally: "open paren," "arrow function," "dot map." A coding-tuned model handles the common ones in context.
- Add anything you say constantly (your repo name, an internal library, a teammate's handle) to Aqua's **Custom Dictionary** (up to 800 entries on Pro) so it is locked in every time.
- Use **Custom Instructions** for standing style, for example "keep prose lowercase in commit messages" or "always spell out CLI flags."


You will still type the occasional exact token, and that is fine. The win is that most of what you send an agent is reasoning and instruction, which is exactly what voice is fastest at.


## VS Code Speech vs Aqua: which should you use?


A fair, honest split:


- **Use the built-in VS Code Speech extension if** you need offline / on-device transcription, you want a free tool, and your dictation is mostly prose and Copilot Chat inside VS Code.
- **Use a system-wide layer like Aqua if** you want higher accuracy on code identifiers, you want the same hotkey to work in the terminal, the browser, Slack and every other app, and you want 49 languages and on-screen context.


One honest caveat on Aqua: it is cloud-based, so it needs a network connection. On a plane with no Wi-Fi, an on-device tool wins. For everyday connected work, the accuracy and the system-wide reach are worth the trade.


On price, Aqua Pro is **$8/mo billed annually** , with a free Starter tier (1,000 words) to try it and a 70% student discount on a .edu email. See[how it stacks up against Wispr Flow](https://aquavoice.com/vs/wispr-flow) , and if you want to build voice into your own tooling, the[Avalon API](https://aquavoice.com/avalon-api) is an OpenAI-compatible endpoint at $0.39 per hour of audio. The full feature tour is in the[Aqua guide](https://aquavoice.com/guide) .


## The bottom line


VS Code gave you voice for prose and chat, on-device and free, and that is a fine place to start. The moment your day is identifiers, terminals and a dozen apps around the editor, you want a layer that gets the code words right and follows you everywhere with one hotkey. We have typed for 150 years. In an editor you increasingly talk to, it is time to speak.


## FAQ


**Does VS Code have built-in voice dictation?** Yes. Microsoft's free VS Code Speech extension (publisher` ms-vscode` ) adds on-device dictation and voice control of Copilot Chat, including the "Hey Code" wake word, in 26 languages. It runs locally and is best for prose and chat. For code-identifier accuracy and dictation that also works outside VS Code, developers add a system-wide tool like Aqua.


**What is the most accurate speech-to-text for coding in VS Code?** Accuracy on code-specific words is the deciding factor. Aqua runs its own Avalon model, trained on prompts and code rather than audiobooks, and placed #1 among proprietary models on the OpenASR leaderboard at its October 2025 debut. In a 9to5Mac test it made 1 error where macOS Dictation made 17.


**Can I dictate into the VS Code integrated terminal?** With the built-in extension, dictation targets the editor and chat. A system-wide layer like Aqua inserts text wherever the cursor is, including the integrated terminal, so you can speak commands and let the model resolve the exact flags and tokens.


**How much does Aqua cost for VS Code users?** Aqua Pro is $8/mo billed annually, with a free Starter tier (1,000 words) and a 70% student discount with a .edu email. There is also an Avalon API at $0.39 per hour of audio if you want to build voice into your own tools.
