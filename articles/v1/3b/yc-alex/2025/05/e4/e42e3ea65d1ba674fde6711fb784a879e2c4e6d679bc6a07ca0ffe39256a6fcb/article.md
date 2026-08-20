---
schema_version: "1.0.0"
document_id: "e42e3ea65d1ba674fde6711fb784a879e2c4e6d679bc6a07ca0ffe39256a6fcb"
company_key: "yc-alex"
company: "Alex"
source_id: "yc-alex-news-import-06f85188604b"
canonical_url: "https://www.alexcodes.app/blog/alex-3-1-release"
published_at: "2025-05-15T00:00:00+00:00"
first_seen_at: "2026-07-21T05:43:46.556958+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:1fe4afd0f107d2690001355a1e475fc654039a697b5fc65a74100735ebd5d587"
---

# Introducing Alex 3.1 🚀

Here’s what’s new:


- **📲 Simulator Testing + Vision**
- **💻 Reading Console Logs**
- **🛠️ More reliable compilation**
- **📦 Multi-Folder Indexing**
- **📁 Drag and drop folders**
- **∞ Infinite Chat + Context Bar**
- **🔁 Retry Button**
- **🗂️ Indexing XcodeProj files**
- **✨ Summarize Button**
- **🔧 Gemini code parsing fixes**


It’s another massive release. Let’s get into it.


Your browser does not support the video tag.


---


# 📲 Simulator Testing + Vision


Alex can now:


- Run your app
- Click around
- Enter text
- Scroll


And see every step of the way.


This is pretty shocking. It only works on simulator right now, but MacOS app support coming later!


---


# 💻 Reading Console Logs


Alex can also read Xcode’s console logs, which helps it debug issues.


# 📦 Multi-Folder Indexing


You can now add external folders to the same project.


Examples: You could add multiple SPM packages, or add your server/android code in the same context. Enable it in Settings -> Indexed Files -> Additional Folders


# 📁 Drag and drop folders


You can now drop a whole folder into the context, and Alex will read the files in that folder.


# ∞ Infinite Chat + Context Bar


You can now chat much longer with our infinite chat logic.


You can also see what % of the context is filled, and how much (estimate) a message will cost to send via API key.


- Other improvements:


### 🛠️ More reliable compilation


With our new building technique, you won’t have the issues with infinite loading & stale build results anymore. It’s much more robust.


### 🔁 Retry Button


(For when Gemini 2.5 Pro is stuck.)


### 🗂️ Indexing XcodeProj files


Previously, you couldn’t chat with your .pbxproj file. Now you can!(We still don’t recommend making changes to it via code, but it’s helpful for understanding your project).


### ✨ Summarize Button


Now you can one-off summarize and start a new chat. It will also add the summary to memory.


(See the button in our new Context Bar.)


### 🔧 Gemini code parsing fixes


We made some improvements to fix broken code parsing.


If you have any questions or feedback, please reach out to[\[email protected\]](https://www.alexcodes.app/cdn-cgi/l/email-protection#a7d3c2c6cae7c6cbc2dfc4c8c3c2d489c6d7d7) ! We'd be happy to help.


Also, join our[Discord](https://discord.gg/T5zxfReEnd) ! (You can chat with us directly.)
