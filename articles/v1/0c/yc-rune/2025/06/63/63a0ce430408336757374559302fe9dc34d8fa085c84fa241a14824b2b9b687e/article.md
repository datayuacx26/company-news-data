---
schema_version: "1.0.0"
document_id: "63a0ce430408336757374559302fe9dc34d8fa085c84fa241a14824b2b9b687e"
company_key: "yc-rune"
company: "Rune"
source_id: "yc-rune-news-import-1f6fc4a2bb8f"
canonical_url: "https://developers.rune.ai/blog/release-notes-may-1st-2025"
published_at: "2025-06-11T00:00:00+00:00"
first_seen_at: "2026-07-23T23:32:13.637334+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:3908f0da66c7c218a1c2753bec664d094082e66e9b99f6f91f1b3c0834af7d99"
---

# New Updates! May 1st-15th 2025

## 🛠️ App Improvements​


- 🎉 Kicked off the Rune MCP project—our new AI tool for instantly spinning up Rune games with VSCode, going from an empty folder to a running game in seconds! 🤖🚀
- 📦 Pulled in starter templates from the Rune repo to bootstrap game creation.
- Built an interactive version of the "create Rune game" tool with a polished prompt flow for easier game scaffolding.
- Updated the README with local testing instructions, VS Code setup, and a new inspect script for smoother debugging 📖
- 🔇 Removed stray console logs to keep output clean and useful for tooling.
- Added installDependenciesForProject tool to automate setup during game creation.
- 🌐 Added tools for dev server launch and auto-open support directly in the MCP for instant feedback.
- Implemented graceful shutdown handling so the MCP server exits cleanly when interrupted.
- 🧪 Began refining prompt UX and parameter descriptions to make the AI more intuitive.
- Added tools to detect and restart the dev server, plus a check project errors tool to lint and fix issues automatically 🔍
- Introduced the explain-rune-project tool to make the AI easily understand generated projects 🗂️
- Updated network URL reporting to handle edge cases where some dev server URLs may fail to connect 🌐
- 🧠 Introduced multiplayer sync fundamentals like predict-rollback netcode, rate limits, and stateless logic architecture—backed by working examples.
- Improved prompts for the AI to generate better Rune-compatible code by documenting required logic structure and multiplayer flow.
- 📱 Added mobile-specific prompts for screen size handling and orientation locking.


## 🪲 App Bug Fixes​


- Restored group membership checks before navigating to group chat on minimize to prevent unintended access 👥🔒


Subscribe to our newsletter for more game dev blog posts


We'll share your email with Substack ⓘ Substack's embed form isn't very pretty, so we made our own. But we need to let you know we'll subscribe you on your behalf. Thanks in advance!
