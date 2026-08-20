---
schema_version: "1.0.0"
document_id: "97abb4eb74b5805f84f3d900781178d78912f8c72193aeeca26c02bb3a0ec385"
company_key: "yc-aqua-voice"
company: "Aqua Voice"
source_id: "yc-aqua-voice-news-import-16e95c4a8dc7"
canonical_url: "https://aquavoice.com/blog/voice-dictation-for-codex-cli"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T21:20:39.425283+00:00"
fetched_at: "2026-08-11T21:20:41.544143+00:00"
content_hash: "sha256:0b80556c84ce8d6860dd9e93515ad79215d1bde1ffe49c391263e01e3b1de093"
---

# Voice Dictation for Codex CLI (2026)

OpenAI's Codex CLI is text-first: its official documentation describes a terminal interface with no built-in dictation. If you want to talk to Codex instead of typing, the reliable answer is a system-wide voice layer like[Aqua](https://aquavoice.com/) : one push-to-talk key that inserts text wherever your cursor is, tuned to get code, commands and file paths right, so it works in Codex and everywhere else you type.


That is the short version. Below: what Codex CLI actually offers today, why "voice for Codex" gets confused with OpenAI's realtime voice work, and the setup that lets you dictate a prompt to Codex and a` git rebase` in the same terminal without switching tools.


## Does Codex CLI have built-in voice dictation?


Not in any documented, dependable form. OpenAI's own Codex CLI documentation describes a text-based terminal interface and does not document a voice input, dictation, or microphone feature. Inside the openai/codex repository, the request for native voice-to-text during an active session was closed as a duplicate of an older tracking issue, which tells you two things: developers clearly want it, and it is not a settled, shipped part of the CLI you can lean on.


You will find plenty of third-party discussion and experimental setups, and OpenAI has been building realtime voice into its broader stack, but that is a different thing from dictating text into your terminal (more on that below). For day-to-day work, the practical state today is: Codex CLI expects typed input, and if you want to speak, you bring your own dictation.


The macOS system Dictation key is the usual fallback, and it is a poor one in a terminal. It fights the shell, cuts off longer thoughts, and knows nothing about code, so` pnpm` ,` --force` , and` ~/.codex/config.toml` come out as English words you then have to fix. On Windows there is even less to fall back on. That gap is what third-party dictation tools and system-wide apps are competing to fill.


## "Voice for Codex" vs dictating into Codex


This is where searches get muddy, so it is worth separating cleanly.


One thread is OpenAI's realtime, speech-to-speech voice: a conversational mode where you talk and a model talks back over a live audio connection. That is voice as a conversation layer, and it is evolving fast across OpenAI's products. It is not the same as inserting your words as editable text at the cursor in your Codex CLI prompt, your editor, and your other tools.


The other thread, the one most people actually mean by "voice dictation for Codex," is exactly that: hold a key, speak, and have accurate text land where you are typing, whether that is the Codex composer, a shell command next to it, or a commit message. This post is about the second thread. A realtime voice conversation does not help when you need to dictate a precise multi-line instruction, paste it, edit one word, and then run a build. Dictation does.


## The system-wide answer


A system-wide dictation layer sits below the terminal instead of inside any one tool.[Aqua](https://aquavoice.com/) runs at the OS level on Mac and Windows and types wherever your cursor is. Hold the Fn key, speak, release, and the text appears in the active field. That field can be the Codex CLI prompt, the shell command you run right after, the` git` commit box, a` ~/.codex` config file open in your editor, a Linear ticket, or Slack, all with one setup and one muscle-memory key.


Because Codex CLI lives in the terminal, this matters more than for a graphical IDE. Your day is not only agent prompts. It is prompts interleaved with commands, paths, flags and package names in the same window, plus the browser tab and the notes app next to it. A dictation tool bound to one app would cover a fraction of that. One that types at the OS cursor covers all of it, and it keeps working no matter which Codex version you are on, because there is nothing to keep compatible.


### Accuracy is the part that matters in a terminal


Aqua is built on[Avalon](https://aquavoice.com/avalon-api) , Aqua's own speech model, and it is tuned for how developers actually talk. In a Codex CLI session you are dictating two things at once, and a general speech model is bad at both. The natural-language half is your intent: "refactor the auth middleware to read the token from the Authorization header and add a test." The syntax half is everything a shell needs exact:` npm run build` ,` git rebase -i HEAD~3` ,` --dry-run` ,` pip install ruff` ,` apps/marketing/src` .


A general model treats all of it as English. It writes "dash dash force" instead of` --force` , "use effect" instead of` useEffect` , "num pie" instead of` numpy` , and it drops the casing and punctuation that make a command runnable. Avalon keeps identifiers, flags, paths and casing intact, and it recognizes the library and tool names a general model mangles. On independent measurement, Avalon debuted as the #1 proprietary model on the OpenASR leaderboard in October 2025 (it currently sits at #6 overall as newer models have landed). On Aqua's own AISpeak benchmark it reaches a self-reported 97.3% accuracy, and in a 9to5Mac head-to-head on the same passage Apple Dictation made 17 errors to Aqua's 1. In a terminal, where one wrong token means a failed command, that difference is the whole game.


## Why voice is worth more with Codex now


Codex CLI is an agent, not an autocomplete. You describe a task in prose, it plans and edits across files, you review and steer. The center of gravity has moved from typing exact syntax to expressing intent, and intent is exactly what voice is fast at and typing is slow at.


Speaking a paragraph of context to Codex is two or three times faster than typing it, and it keeps your hands free between edits. The more of your work becomes steering an agent rather than writing every character yourself, the more the bottleneck is the prompt, and the more a fast, accurate dictation layer pays off. If you also live in other terminals or work across Claude Code and Cursor, the same layer covers them too. Our guide to[voice to text in the terminal](https://aquavoice.com/blog/voice-to-text-terminal) goes deeper on dictating CLI syntax generally.


## How to set up voice dictation for Codex CLI


The setup is tool-agnostic on purpose, because Aqua is not a Codex plugin.


1. Install[Aqua](https://aquavoice.com/) on your Mac or Windows machine and grant it microphone and accessibility permissions when prompted. This is a one-time OS-level setup.
2. Open your terminal and start Codex CLI as usual. There is nothing to install inside it and no config to keep in sync.
3. Put your cursor where you want text: the Codex prompt, a shell command, the commit message box, or a config file in your editor.
4. Hold the Fn key, speak, and release. Your words appear at the cursor, punctuated, with commands and identifiers intact.
5. Add project-specific terms (your service names, internal libraries, unusual CLI tools) to Aqua's custom dictionary so they transcribe correctly every time.


The same key works in your browser, your notes app and Slack, so you are not switching tools when you leave the terminal. The[Aqua guide](https://aquavoice.com/guide) covers dictation habits and editing by voice in more depth.


## How it compares to the other options


If you are weighing a Codex-specific voice tool against a system-wide app, the trade is straightforward. A single-tool or terminal-only utility is fine if you never leave that one window and do not mind correcting code tokens. A system-wide, code-tuned layer wins the moment you interleave prompts with shell commands, use more than one AI coding tool, or spend time in the browser and everywhere else.


Aqua is often compared with other dictation tools on those same axes. Our breakdowns of[Aqua vs Wispr Flow](https://aquavoice.com/vs/wispr-flow) and[Aqua vs Superwhisper](https://aquavoice.com/vs/superwhisper) go through accuracy, reach and pricing in detail.


Aqua is free for your first 1,000 words, with no card required, and Pro is $8/mo billed annually for unlimited dictation. That is a low bar to test whether talking to Codex actually fits your day.


## FAQ


**Does OpenAI Codex CLI have built-in voice dictation?** Not in a documented, dependable form. OpenAI's Codex CLI documentation describes a text-based terminal interface with no voice input feature, and the repo's request for native in-session voice-to-text was closed as a duplicate. To dictate into Codex reliably today, you use a system-wide dictation app.


**Is dictating into Codex the same as OpenAI's realtime voice?** No. OpenAI's realtime voice is a live speech-to-speech conversation layer. Dictation inserts your spoken words as editable text at your cursor, in the Codex prompt and everywhere else you type. This guide is about dictation.


**Will voice transcription get my terminal commands right?** A general speech model usually will not: it turns flags, paths and package names into English. Aqua runs on Avalon, a code-tuned model that keeps` --force` ,` git rebase -i` , casing and identifiers intact, which is what makes dictated commands actually runnable.


**Does Aqua work outside Codex CLI too?** Yes. One OS-level install types at your cursor in any app on Mac or Windows: other terminals, your editor, Claude Code, Cursor, the browser and Slack, with no per-tool setup.
