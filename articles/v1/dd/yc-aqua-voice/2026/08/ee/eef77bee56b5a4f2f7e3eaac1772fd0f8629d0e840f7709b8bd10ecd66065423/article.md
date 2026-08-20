---
schema_version: "1.0.0"
document_id: "eef77bee56b5a4f2f7e3eaac1772fd0f8629d0e840f7709b8bd10ecd66065423"
company_key: "yc-aqua-voice"
company: "Aqua Voice"
source_id: "yc-aqua-voice-news-import-16e95c4a8dc7"
canonical_url: "https://aquavoice.com/blog/voice-dictation-for-jetbrains"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T06:33:48.753053+00:00"
fetched_at: "2026-08-05T06:33:50.832294+00:00"
content_hash: "sha256:658593cd4a63f81b6ec0596d36761fb30464022a669ef4d61d998d1697633c6d"
---

# Voice Dictation for JetBrains (2026)

JetBrains IDEs have no built-in voice dictation. IntelliJ IDEA, PyCharm, WebStorm and the rest ship no microphone in the editor, the terminal, the commit dialog, or the AI Assistant chat. The marketplace fills the gap with plugins, but they wrap general speech models and each one is scoped to a single IDE. The working answer across the whole JetBrains suite is a system-wide layer like[Aqua](https://aquavoice.com/) : one push-to-talk key that inserts text at your cursor, tuned to get code right.


That is the short version. Below: what JetBrains actually offers today, why a plugin per IDE is the wrong shape for a suite this wide, and the setup that covers every JetBrains window plus the terminal, AI Assistant and Junie.


## Does JetBrains have built-in dictation? No


There is no first-party speech-to-text anywhere in the JetBrains IDEs. Not in the editor, not in Search Everywhere, not in the integrated terminal, and not in the AI Assistant panel or Junie, JetBrains' coding agent. You can confirm this by the shape of the marketplace itself: the reason a dozen third-party dictation plugins exist is that nothing native covers the need.


On macOS you can fall back to the system Dictation key, but it behaves poorly inside a JVM editor. It fights the IDE's own completion popups, drops out on longer thoughts, and knows nothing about code, so every identifier comes out as plain English words. On Windows there is even less to fall back on. That is the gap the plugins and system-wide apps are competing to fill.


## The plugin route, and why it does not fit a suite


Search the JetBrains Marketplace and you will find several dictation plugins: Whisper Speech to Text, SpeakIDE, a Speech-To-Text plugin built on AWS Transcribe, an AI Dictation plugin, and others. They work, and if you live in exactly one IDE they are a reasonable option. Two structural problems keep them from being the real answer.


First, most of them are wrappers around general-purpose speech models. The Whisper-based plugins say so plainly in their own listings. A general model is trained on everyday speech, so it treats your code as English. "camelCase" becomes "camel case,"` useEffect` becomes "use effect,"` psycopg2` becomes "psycho PG two," and a library like` kotlinx.coroutines` turns into a small paragraph. You end up correcting the tokens that matter most, which is exactly where dictation was supposed to save time.


Second, and specific to JetBrains: this is not one IDE, it is a suite. IntelliJ IDEA, PyCharm, WebStorm, GoLand, CLion, Rider, RubyMine, PhpStorm, DataGrip and RustRover are separate products, and many developers use two or three of them plus Android Studio. A plugin has to be installed, configured, updated and relearned in each one, and it only works while that IDE window is focused. The moment you switch to the terminal outside the IDE, a browser tab, a Linear ticket or Slack, the plugin is gone. A dictation tool that stops at the edge of one editor is the wrong shape for how JetBrains work actually spreads across apps.


## The system-wide answer


A system-wide dictation layer inverts the model. Instead of a plugin bound to one IDE,[Aqua](https://aquavoice.com/) runs at the OS level on Mac and Windows and types wherever your cursor is. Hold the Fn key, speak, release, and the text lands in the active field. That field can be the IntelliJ editor, the PyCharm run configuration, the WebStorm search bar, the integrated terminal tool window, the Git commit dialog, a DataGrip SQL console, the AI Assistant chat, a Junie prompt, and then your browser and Slack, all with one setup and one muscle-memory key.


Because it sits below the IDE rather than inside it, there is nothing to keep compatible with each IDE version or EAP build, and it behaves identically across every JetBrains product you open. One install covers the whole suite.


### Accuracy is the part that matters for code


Aqua is built on[Avalon](https://aquavoice.com/avalon-api) , Aqua's own speech model, and it is tuned for how developers actually talk. It keeps casing conventions, punctuation and symbols intact, and it recognizes the framework and library names that a general model mangles. Across the JetBrains stack that means Kotlin and Java identifiers in IntelliJ, the Python and data-science vocabulary in PyCharm (` numpy` ,` pandas` ,` pytest` ,` asyncio` ), JS and TypeScript in WebStorm, Go idioms in GoLand, C# and .NET types in Rider, and SQL in DataGrip, all transcribed the way you would write them.


On independent measurement, Avalon debuted as the #1 proprietary model on the OpenASR leaderboard in October 2025 (it currently sits at #6 overall as newer models have landed). On Aqua's own AISpeak benchmark it reaches a self-reported 97.3% accuracy, and in a 9to5Mac head-to-head on the same passage Apple Dictation made 17 errors to Aqua's 1. The point for JetBrains users is narrow and practical: the words you dictate into code are the ones a general model gets wrong, and a code-tuned model is the difference between dictation that saves time and dictation you spend the afternoon fixing.


## Why voice is worth more in JetBrains now


For years an IDE was mostly a place you typed exact syntax, where dictation had a natural ceiling. That has changed. In 2026.1 JetBrains shipped Junie as a full coding agent that plans and executes multi-step tasks from a prompt, alongside the AI Assistant chat that now sits in every IDE. Both are driven by prose. You describe what you want, review a plan, refine it, and steer.


Describing intent is exactly what voice is fast at and typing is slow at. Speaking a paragraph of context to Junie, or a precise instruction to AI Assistant, is two or three times faster than typing it, and it keeps your hands off the keyboard between edits. As more of the JetBrains workflow becomes agent steering rather than character-by-character coding, the input bottleneck moves from the editor to the prompt, and that is where a fast, accurate dictation layer pays off most.


## How to set up voice dictation in JetBrains


The setup is IDE-agnostic on purpose, because Aqua is not a JetBrains plugin.


1. Install[Aqua](https://aquavoice.com/) on your Mac or Windows machine and grant it microphone and accessibility permissions when prompted. This is a one-time OS-level setup, not a per-IDE one.
2. Open any JetBrains IDE. There is nothing to install inside it and no plugin to keep updated.
3. Put your cursor where you want text: the editor, the terminal tool window, the commit message box, Search Everywhere, or the AI Assistant / Junie input.
4. Hold the Fn key, speak, and release. Your words appear at the cursor, punctuated, with code identifiers intact.
5. Add project-specific terms (your service names, internal libraries, unusual APIs) to Aqua's custom dictionary so they transcribe correctly every time.


The same key works in your browser, your notes app and Slack, so you are not switching tools when you leave the IDE. If you want a deeper walkthrough of dictation habits and editing by voice, the[Aqua guide](https://aquavoice.com/guide) covers it.


## How it compares to the other options


If you are weighing a JetBrains plugin against a system-wide app, the trade is straightforward. A single-IDE plugin is fine if you never leave that one IDE and do not mind correcting code tokens. A system-wide, code-tuned layer wins the moment you use more than one JetBrains product, spend time in the terminal or browser, or lean on AI Assistant and Junie.


Aqua is often compared with other dictation tools on those same axes. Our breakdowns of[Aqua vs Wispr Flow](https://aquavoice.com/vs/wispr-flow) and[Aqua vs Superwhisper](https://aquavoice.com/vs/superwhisper) go through accuracy, reach and pricing in detail if you are choosing between apps rather than plugins.


Aqua is free for your first 1,000 words, with no card required, and Pro is $8/mo billed annually for unlimited dictation. That is a low bar to test whether dictating into IntelliJ or PyCharm actually fits your day.


## FAQ


**Does IntelliJ IDEA have built-in voice dictation?** No. IntelliJ IDEA, and every other JetBrains IDE, ships no first-party speech-to-text in the editor, terminal, commit dialog or AI Assistant. You need either a marketplace plugin or a system-wide dictation app.


**Are the JetBrains dictation plugins any good?** They work, but most are wrappers around general-purpose speech models, so they struggle with code identifiers, library names and casing, and each plugin only works inside a single IDE while it is focused.


**Can I dictate into the JetBrains AI Assistant and Junie?** Yes, with a system-wide layer like Aqua. Because it types at the OS cursor, it works in the AI Assistant chat and the Junie prompt just as it does in the editor, which is where voice speeds up agent-driven work the most.


**Does Aqua work across the whole JetBrains suite?** Yes. One OS-level install covers IntelliJ IDEA, PyCharm, WebStorm, GoLand, CLion, Rider and the rest, plus the terminal, browser and every other app, with no per-IDE plugin to maintain.
