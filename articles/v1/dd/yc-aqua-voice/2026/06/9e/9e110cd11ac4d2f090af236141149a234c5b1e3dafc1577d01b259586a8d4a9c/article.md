---
schema_version: "1.0.0"
document_id: "9e110cd11ac4d2f090af236141149a234c5b1e3dafc1577d01b259586a8d4a9c"
company_key: "yc-aqua-voice"
company: "Aqua Voice"
source_id: "yc-aqua-voice-news-import-16e95c4a8dc7"
canonical_url: "https://aquavoice.com/blog/voice-dictation-claude-code"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-21T07:23:55.387885+00:00"
fetched_at: "2026-07-28T21:23:21.398471+00:00"
content_hash: "sha256:aed0be3814bc2741423a3b654eaa7a344d1a2b2a28a109502bfbbebca5d48cbb"
---

# Voice dictation for Claude Code: how to talk to your terminal in 2026

**Yes, you can dictate to Claude Code two ways.** Claude Code has a built-in` /voice` command (push-to-talk, right in the CLI), and you can layer a system-wide dictation app like Aqua on top so the same key works in Claude Code *and* Cursor, your terminal, the browser, and Slack. Built-in voice is the fastest way to start; a system-wide layer is what you want once your day spans more than one tool.


Here's exactly how each works, and when to reach for which.


## Option 1: Claude Code's built-in` /voice`


Anthropic shipped native voice dictation to Claude Code on March 3, 2026. It's the zero-setup path:


1. Update Claude Code (voice needs **v2.1.69+** ; tap mode needs **v2.1.116+** ). Check with` claude --version` .
2. Run **` /voice`** in the CLI. The first time, macOS prompts for microphone access for your terminal.
3. **Hold mode** (default): hold **Space** , talk, release to finalize. Your words stream in dimmed, then commit at the cursor. **Tap mode** (` /voice tap` ): tap Space to start, tap again to send.


It's tuned for code: terms like` regex` ,` OAuth` ,` JSON` , and` localhost` are recognized, and your **project name and git branch are added as recognition hints automatically** . It also works in Claude Code's VS Code extension and in agent view. For dictating a prompt straight into the agent you're already in, it's genuinely good, and it doesn't consume Claude messages or tokens.


### Where the built-in stops


Three boundaries matter once you actually live in voice:


- **It only works inside Claude Code.** The CLI and the VS Code extension: that's the perimeter. Switch to Cursor's composer, a plain terminal tab, a GitHub PR description, Gmail, Slack, or a different agent like Codex, and the microphone is gone. You're re-learning where voice does and doesn't exist, tab by tab.
- **It needs a Claude.ai-account login.** Built-in voice streams audio to Anthropic's transcription service and is **only available when you authenticate with a Claude.ai account** , not when Claude Code is configured with an Anthropic API key directly, or via Amazon Bedrock, Google Vertex AI, or Microsoft Foundry, and **not when your org has HIPAA mode on** . Plenty of professional setups run exactly those paths.
- **~20 dictation languages.** Great if you speak one of them; a real limit if you don't. (Aqua supports 49 languages.)


None of that makes the built-in bad. It makes it *scoped* . For a single-tool session, it's all you need. For an agent-era workflow that crosses apps all day, you want one voice everywhere.


## Option 2: one system-wide voice across every tool


This is the gap Aqua fills. Aqua is **system-wide push-to-talk dictation** : hold the **Fn** key, talk into whatever has your cursor, release, and the text lands there: in Claude Code, Cursor, the terminal, a PR, Gmail, Notion, or Slack. One key, every app, including the ones you'll add next quarter. Text appears in roughly **450ms** after you release, and it works the same no matter how Claude Code is authenticated, because Aqua sits at the OS layer, not inside the agent.


What you get on top of "it works everywhere":


- **A model trained on how engineers talk.** Aqua runs on **Avalon** , its own speech-to-text model trained on human-computer-interaction speech (prompts, code, email) rather than audiobooks. On the **OpenASR leaderboard** it ranked **#1 among proprietary models** at its October 2025 debut (and is #6 overall), an independently checkable placement. When[9to5Mac put it head-to-head with macOS dictation](https://9to5mac.com/2025/08/15/aqua-voice-shows-just-how-good-mac-dictation-could-be-if-apple-just-tried/) , Apple's made **17 errors** to Aqua's **1** . (On Aqua's own AISpeak coding-jargon benchmark, a self-reported benchmark, Avalon scores 97.3% vs 65.1% for Whisper Large v3.)
- **A custom dictionary for *your* repo.** Add the invented words every codebase has (service names, internal acronyms, that one library only your team uses), up to **800 entries** on Pro, so they come out right every time. The built-in's automatic project/branch hints are nice; a dictionary you control is the next level.
- **Deep Context.** Aqua reads what's on your active screen to disambiguate a variable, a CLI flag, or a teammate's handle, and adjusts tone to the destination (terse in a commit, complete sentences in Slack).
- **~49 languages** and Mac, Windows, and iOS, at **$8/mo billed annually** .


The honest summary: use built-in` /voice` for quick prompts inside Claude Code, and add Aqua when you want the same accuracy and the same muscle memory across your whole stack.


## How to set up Aqua for Claude Code


1. **Install Aqua** on macOS or Windows from[aquavoice.com](https://aquavoice.com/) and grant microphone + accessibility permissions.
2. **Open your terminal** (or the VS Code integrated terminal) where Claude Code runs. Aqua works in any text field, including the CLI prompt, with no Claude.ai login and no provider restrictions.
3. **Hold Fn, dictate your prompt, release.** Your transcript inserts at the cursor in Claude Code's input. Press Enter to send.
4. **Load your stack into the Custom Dictionary:** framework names, services, CLI tools, the libraries you say twenty times a day. See the[setup guide](https://aquavoice.com/guide) .
5. **Optional:** set Custom Instructions like "keep code identifiers in camelCase" so spoken names format the way you write them.


Building voice into your own agent instead? Avalon is available as an[OpenAI-compatible API](https://aquavoice.com/avalon-api) .


## Tips for dictating prompts to a coding agent


- **Be verbose.** A fast input means you can give the agent the full picture: the file, the constraint, the edge case, the thing you already tried. More signal, better output, and now it costs seconds instead of minutes.
- **Name the casing when it matters.** "camelCase user ID" produces` userId` ; "snake_case max retries" produces` max_retries` .
- **Dictate file paths and flags naturally.** Avalon's training includes them, and Deep Context can often see the path you mean on screen.
- **Mix typing and talking.** Type the half that's faster to type; hold Fn and speak the half that's faster to say.


We've typed for 150 years. In an era where you spend the day describing software to an agent that writes it, the keyboard is the bottleneck, and your voice is the fix. **[Try Aqua free](https://aquavoice.com/)** (1,000 words free, then $8/mo billed annually on Mac, Windows, and iOS).


---


## FAQ


**Does Claude Code have built-in voice dictation?** Yes. Run` /voice` in Claude Code (v2.1.69+) to dictate prompts with push-to-talk: hold Space to record, release to insert. It's tuned for coding vocabulary and adds your project and git-branch names as recognition hints. It works in the CLI and the VS Code extension, and requires a Claude.ai-account login.


**Do I still need a separate dictation app if Claude Code has` /voice` ?** Only if your workflow leaves Claude Code. Built-in voice works inside Claude Code (CLI + VS Code extension). If your day also spans Cursor, the terminal, GitHub, Gmail, and Slack, or you run Claude Code via an API key, Bedrock, Vertex, or Foundry, where built-in voice isn't available, a system-wide tool like Aqua gives you one push-to-talk with the same accuracy everywhere.


**Is voice dictation accurate enough for coding?** It is when the model is built for it. Aqua's Avalon model is trained on code and prompt speech and was the #1 proprietary model on the public OpenASR leaderboard at its October 2025 debut, and a custom dictionary handles your repo's specific terms. Claude Code's built-in voice is also coding-tuned and uses your project and branch as hints.


**How fast is dictation compared with typing?** Most people speak around 150 words a minute and type around 40. Aqua inserts text roughly 450ms after you release the key, so the speed you gain isn't eaten by lag.


---


*Last updated: June 16, 2026. Built-in` /voice` behavior per Claude Code's official documentation (verified June 2026). Benchmarks: OpenASR placement is independently verified; AISpeak is Aqua's own coding-jargon benchmark. Aqua does not make claims about competitors' underlying models.*
