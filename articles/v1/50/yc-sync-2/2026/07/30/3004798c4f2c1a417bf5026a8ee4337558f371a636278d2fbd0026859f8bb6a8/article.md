---
schema_version: "1.0.0"
document_id: "3004798c4f2c1a417bf5026a8ee4337558f371a636278d2fbd0026859f8bb6a8"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/sync-labs-in-chatgpt/"
published_at: "2026-07-15T16:00:00+00:00"
first_seen_at: "2026-07-22T15:25:00.668099+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:d4e675a9daad1b3a778b6486388ed4f4e9f59e79a151b669924282addbebf0d1"
---

# sync. labs is now in ChatGPT

sync. labs is now a plugin in ChatGPT. Upload a video, tell chat what you want it to say, and get back a lip synced result without ever leaving the conversation. Dubbing, dialogue replacement, voice cloning, text-to-speech, and animating still images all work the same way: describe the output, attach the files, and the plugin handles the rest.


You can[connect it from ChatGPT’s plugin directory](https://go.sync.so/kdWbIjQ) right now.


For two years the way into our models has been the sync. labs app, the API, or an editor plugin. All of those assume you know what a generation is, what a segment is, which model to pick. ChatGPT removes that layer. You already know how to ask for what you want. Now asking is the interface.


## What the sync. labs plugin in ChatGPT can do


The sync. labs plugin in ChatGPT runs our full creation stack from a single conversation: video-to-video lip sync, dubbing, voice cloning, text-to-speech, and image animation. These are real prompts that work today:


- “Dub this video into English.” Attach a video, get it back speaking a new language with the mouth matching every word.
- “Change this line to say X instead.” Replace dialogue in a finished video, the classic[ADR](https://sync.so/blog/understanding-automatic-dialogue-replacement-adr-in-content-creation/) problem, from chat.
- “Clone the voice from this audio, then have it say this script in that video.” Compound requests work. The plugin chains voice cloning, speech generation, and lip sync in one go.
- “Make this image talk.” Attach a photo and audio (or just text) and get back a speaking video.


Under the hood these run on the same models as everything else we ship, including[sync-3](https://sync.so/sync-3) , our most advanced lip sync model. There is no separate “ChatGPT version” of the product. Your generations, voices, and projects live in your sync. labs account, so anything you start in chat is right there when you open the app or the API.


The core of it is still what we care about most: take real footage of a real person and change what they say, naturally enough that nobody watching thinks to check.


## Any agent can do this, not just ChatGPT


In ChatGPT, sync. labs lives in the plugin directory, where OpenAI now lists workflow capabilities for ChatGPT and Codex. Connecting the plugin connects our approved ChatGPT app underneath, the integration that actually talks to sync. labs. You connect once, sign in, and ChatGPT knows how to use it. No servers, no configuration, nothing to install.


For everyone else building with agents, the same capabilities are available through the sync. labs MCP server. Point Claude, Cursor, or an agent you built yourself at it and they can upload assets, clone voices, generate speech, run lip sync, and fetch results, the same stack the ChatGPT app uses. The[MCP server docs](https://sync.so/docs/plugins-and-extensions/mcp-server) cover setup for each client.


Getting there took more than wrapping the API. We rebuilt the public API around how agents actually work: direct asset uploads, standalone text-to-speech, a voices endpoint for cloning and management, a projects API, and requests that can wait synchronously for a result instead of forcing a polling loop. Around 30 pull requests and three rounds of app review later, the whole pipeline is something a model can drive on its own. ChatGPT is the first place most people will feel that, but it works anywhere an agent runs.


## How to connect it


1. Open the[sync. labs listing in ChatGPT’s plugin directory](https://go.sync.so/kdWbIjQ) , or search “lip sync” in the directory.
2. Select Connect and sign in with your sync. labs account (or[create one](https://sync.so/) , it takes a minute).
3. Attach a video, image, or audio file and describe what you want.


That’s it. The[ChatGPT plugin docs](https://sync.so/docs/plugins-and-extensions/chatgpt-plugin) walk through every flow in detail, but the best way to learn what it can do is to ask for something and see it come back. We’ve been doing exactly that all week, and it still hasn’t stopped being fun.
