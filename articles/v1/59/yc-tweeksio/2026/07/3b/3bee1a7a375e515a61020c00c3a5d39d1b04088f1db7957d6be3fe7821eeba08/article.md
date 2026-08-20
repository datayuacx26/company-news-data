---
schema_version: "1.0.0"
document_id: "3bee1a7a375e515a61020c00c3a5d39d1b04088f1db7957d6be3fe7821eeba08"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-rss-bbf1d949eada"
canonical_url: "https://www.tweeks.io/blog/export-gemini-chats-to-markdown"
published_at: "2026-07-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.378405+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:120677c5947a05c1a9a12a827006c42b08b5c4a45812bfce53c1e4023901a696"
---

# Export Gemini Chats to Markdown or Claude

Gemini can send a single response to Google Docs, but there's no way to export a whole Gemini chat to Markdown. Claude and ChatGPT are no better: their export options cover your entire account history, delivered by email as an archive. If you want one specific chat as clean Markdown for your notes, a doc, or another assistant, the expected workflow is select-all, copy, and fix the formatting by hand.


We built a tweek for this one.[Chat Exporter](https://www.tweeks.io/t/ab46eaf4412c4575a692d791) adds two small buttons to Gemini, Claude, and ChatGPT: one copies the open conversation to your clipboard as Markdown, and one downloads it as a` .md` file. Below are the four directions, step by step: Gemini to Markdown, Gemini to Claude, Claude to Markdown, and ChatGPT to Markdown.


## Why Not the Official Exports?


Each app has an export path, but none of them produce a single conversation in a usable format. As of writing:


- Gemini's "Export to Docs" covers one response at a time, and[Google Takeout](https://takeout.google.com/) gives you your whole Gemini history as an archive.
- ChatGPT's export (Settings, then Data controls) sends your entire account history by email as a zip.
- Claude's export works the same way: all conversations, one archive, delivered by email.


These are data-portability features, built for leaving a platform, and none of them help with "I want this one chat in my notes app five seconds from now". The tweek exists for that second case.


## Put the Exports to Work


The obvious use is notes. The filenames sort themselves by platform, title, and date, and the role headings mean a pasted chat lands in Obsidian or Notion already outlined. If you research with AI and keep losing the good answers to a sidebar of untitled chats, downloading the keepers as you go is the fix.


Exports also make good second opinions. Copy a Gemini conversation, paste it into Claude, and ask:


> Here's a plan I worked out with Gemini. Poke holes in it.


In our experience a model is far more willing to disagree with another model's transcript than to reverse itself mid-conversation, which makes this round trip more useful than asking Gemini "are you sure?" for the tenth time.


And if you use AI for code, the download button pairs well with git. Commit the exported chat next to the change it produced, and the reasoning behind a strange-looking function survives long after everyone forgot the conversation happened.


## Why Markdown and Not a Doc or PDF?


Because Markdown is the one format that is service agnostic. The chat apps generate their answers in it and render it to HTML, which is why code fences and tables can survive the trip back out. Obsidian and Bear store it natively, Notion imports it, and it diffs cleanly in git, so an exported chat can live next to the code it produced. When you paste it into another assistant, the model reads the structure directly instead of reconstructing it from flattened text.


A PDF freezes the conversation in a way that is not easily searchable or machine readable, and a Google Doc moves it into yet another account, but Markdown is just text, and plain text is the format most likely to still open in twenty years.


## What the Export Looks Like


Every export has the same shape: a title, a timestamp, then the conversation as alternating sections.


markdown


```text
# Gemini Conversation: Trip Planning
**Exported:**   2026-07-05 14:12:03


---


<!-- message:user -->
## User


What's the best time of year to hike the Dolomites?


<!-- message:assistant -->
## Gemini


Late June through September is the usual window...
```


The HTML comments above each heading mark the role, so if you ever want to re-parse these files with a script, who said what is machine readable.


## What Survives the Conversion


Select-and-copy from a chat window loses most formatting on the way to your notes. The exporter walks the page's HTML instead, so nearly everything the model produced keeps its structure:


In the chat In the export


Code blocks Fenced blocks, with the language tag when the site shows one


Tables Markdown pipe tables


Nested lists Indented Markdown lists


Bold, italics, strikethrough The usual asterisks and tildes


Links` \[text\](url)`


Images Image links, though the URLs can expire


Math notation Plain text, and not always pretty


Gemini's source citations Dropped


Claude's artifacts and thinking Not exported


The first six rows cover almost everything in a typical chat, so most exports come out ready to use as-is. The last three are edge cases: rendered math comes through as text without the LaTeX behind it, citation chips are buttons rather than content so the converter skips them, and artifacts live outside the message stream entirely. For the occasional chat that leans on one of those, check the export before you delete the original.


Your own messages are the simplest case: they come through exactly as you typed them, under their` ## User` headings.


## Setup


You do this once, and it takes about a minute:


1. Install the[Tweeks extension](https://www.tweeks.io/get-extension) for Chrome or Firefox.
2. Open the[Chat Exporter](https://www.tweeks.io/t/ab46eaf4412c4575a692d791) share page, review the source if you want, and click install.
3. Open Gemini, Claude, or ChatGPT. Two icon buttons appear in the bottom-right corner: copy and download.


The same two buttons do the same two things on all three sites, so everything below is a variation on one move.


### Export a Gemini Chat to Markdown


1. Open the conversation on gemini.google.com.
2. If the chat is long, scroll up to the beginning once. Gemini drops older messages from the page until you scroll back, and the exporter can only read what's actually there.
3. Wait for any response that's still generating to finish. The exporter checks for this and will ask you to wait rather than cut a message in half.
4. Click the download button for a file, or the copy button for the clipboard.


The download is named after the chat, for example` Gemini-Trip-Planning-2026-07-05.md` . Your prompts come through as plain text under` ## User` headings, and Gemini's responses are converted to Markdown under` ## Gemini` , with code blocks, tables, and nested lists intact. One caveat: images in responses export as Markdown image links, and those URLs may stop working once the session is gone.


Gemini has a growing set of tweeks beyond this one;[browse gemini.google.com](https://www.tweeks.io/sites/gemini.google.com) .


### Export a Gemini Chat to Claude


This is the same export pointed somewhere else. There's no import button on either side, but Markdown pasted into a chat box works fine as one:


1. In the Gemini conversation, click the copy button.
2. Open a new chat on claude.ai.
3. Paste, and add one line of framing at the top, something like: "This is a conversation I had with Gemini. Read it and continue from where it ends."


The export labels every turn with` ## User` and` ## Gemini` headings, so Claude can follow who said what without any cleanup. For very long conversations, trim the export down to the parts you still need before pasting; you are spending context window either way, and the early back-and-forth is usually dead weight.


For a chat that's too big to paste whole, there's a cleaner move. Ask Gemini for a handoff first, something like "summarize this conversation for another assistant: the goal, the decisions we made, the current state, and the open questions", then export that response instead of the whole transcript. You lose the play-by-play, but the next model gets exactly what it needs at a fraction of the tokens.


The same trick works in any direction, including out of Claude and into Gemini or ChatGPT. Whatever you paste into is the assistant that inherits the chat.


Claude users have their own shelf in the library too:[see what people run on claude.ai](https://www.tweeks.io/sites/claude.ai) .


### Export a Claude Chat to Markdown


Same two buttons, now on claude.ai:


1. Open the conversation.
2. Scroll to the top if it's long, and let any streaming response finish.
3. Copy or download.


Claude responses convert cleanly, including fenced code blocks with their language labels. Two things stay behind, though: artifacts (the code and documents Claude opens in a side panel) and extended thinking blocks aren't part of the message stream, so they don't come along. Copy artifacts out separately if you need them.


### Export a ChatGPT Chat to Markdown


The exporter runs on both chatgpt.com and chat.openai.com:


1. Open the conversation.
2. Scroll up once if it's long, and wait for streaming to finish.
3. Copy or download.


Code blocks keep their language tags, so a chat full of Python lands in your notes with proper syntax fences instead of the flattened text you get from select-and-copy. ChatGPT is also one of the most modified sites in the Tweeks library;[browse chatgpt.com](https://www.tweeks.io/sites/chatgpt.com) .


## How It Works


The exporter reads the conversation out of the open page and converts the HTML to Markdown with its own converter, which handles headings, bold and italics, inline code, fenced code blocks with language detection, links, images, nested lists, blockquotes, and tables. Everything runs locally in your browser. The only permission it asks for is clipboard write, it makes no network requests, and the full source is on the share page if you want to check that yourself.


The honest limitations: it exports the conversation you have open, not your whole history; it can only see messages the page has loaded, which is why long chats need one scroll to the top; and since it reads each site's page structure, a redesign on Gemini, Claude, or ChatGPT can break extraction until the tweek is updated.


It's also a tweek, so the behavior is yours to change. Open it in Tweeks and describe what you want: front matter with the chat URL, a different filename pattern, plain text instead of Markdown. Like the[Universal AI Slop Finder](https://www.tweeks.io/blog/what-is-ai-slop) we covered before, edits are one-sentence requests, and the[Tweeks vs. Tampermonkey](https://www.tweeks.io/blog/tweeks-vs-tampermonkey) post explains that workflow if it's new to you.


Your conversations are worth keeping in a format you control, and Markdown will outlive all three of these apps.


## Sources


- [Chat Exporter tweek](https://www.tweeks.io/t/ab46eaf4412c4575a692d791)
- [Google Takeout](https://takeout.google.com/)
- [OpenAI Help Center](https://help.openai.com/)
- [Claude Help Center](https://support.claude.com/)
