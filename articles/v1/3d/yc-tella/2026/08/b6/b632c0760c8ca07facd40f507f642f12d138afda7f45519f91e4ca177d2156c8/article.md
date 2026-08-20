---
schema_version: "1.0.0"
document_id: "b632c0760c8ca07facd40f507f642f12d138afda7f45519f91e4ca177d2156c8"
company_key: "yc-tella"
company: "Tella"
source_id: "yc-tella-news-import-adc6187f1c82"
canonical_url: "https://www.tella.com/blog/best-claude-code-skills"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-03T06:23:02.866682+00:00"
fetched_at: "2026-08-05T03:48:28.288743+00:00"
content_hash: "sha256:ca3ed5bb20e5adc10f3a404e53ce70f3baef2f875402d5e011b0e96e52acb7ce"
---

# The 12 Best Claude Code Skills in 2026 (With Install Commands)

Claude Code skills are folders of instructions — a` SKILL.md` file, sometimes with scripts attached — that teach Claude a repeatable workflow. Install one and Claude picks it up automatically or as a slash command:` /cut-video` ,` /clipify` , and so on. They turned Claude Code from a coding tool into a general-purpose agent that can edit video, build slide decks, and publish content.


There are now thousands of skills on GitHub, and most are thin wrappers or abandoned experiments. This list is the twelve we'd actually install, based on what they do, how well they're documented, and how actively they're maintained.


One honest disclosure up front: we build video skills at Tella — six of the twelve below come from our team or community, and they're marked **(Tella)** . We use the other six too; this list leans video because that's the work we do every day. Most are open source. Anthropic's` docx` ,` pdf` ,` pptx` , and` xlsx` skills are source-available under Anthropic's terms instead, which restrict copying, derivative works, and redistribution.


## Quick picks


Skill Best for By


[Superpowers](https://github.com/obra/superpowers) Structured dev workflow obra


[Anthropic document skills](https://github.com/anthropics/skills) Word, PDF, PowerPoint, Excel Anthropic


[skill-creator](https://github.com/anthropics/skills) Writing your own skills Anthropic


[frontend-design](https://github.com/anthropics/skills) Polished web UI Anthropic


[Cut Video](https://www.tella.com/skills/cut-video) Removing silences and ums Tella


[Clipify](https://www.tella.com/skills/clipify) Social clips from long video Tella


[Add Zooms](https://www.tella.com/skills/add-zooms) Punch-zooms on talking heads Tella


[B-Roll Finder](https://www.tella.com/skills/b-roll-finder) Sourcing accurate b-roll Tella


[video-shotcraft](https://github.com/Vincentwei1021/video-shotcraft) Cinematic product videos Vincent Wei


[remotion-video-skill](https://github.com/wshuyi/remotion-video-skill) Programmatic video in React wshuyi


[Slack Assets Bot](https://www.tella.com/skills/slack-assets-bot) Building an asset library Tella


[Tella to YouTube](https://www.tella.com/skills/tella-to-youtube) Recording → published video Tom Granot


## General-purpose skills


### 1. Superpowers


[Superpowers](https://github.com/obra/superpowers) is the most-starred skills project on GitHub (~265k stars at the time of writing) — less a single skill than a software development methodology expressed as skills: brainstorming, planning, test-driven implementation, and debugging workflows that Claude follows step by step. If you write code with Claude Code daily, this is the one to study even if you don't adopt all of it.


### 2. Anthropic's document skills


Anthropic's[official skills repository](https://github.com/anthropics/skills) (~166k stars) contains the reference skills, including the document family:` docx` ,` pdf` ,` pptx` , and` xlsx` . They let Claude create and edit real Word documents, PDFs, PowerPoint decks, and Excel spreadsheets — the same skills that power document handling in the Claude apps. If your work produces documents, install these first. One licensing note: unlike the rest of this list, the document skills are source-available rather than open source — Anthropic shares them as a reference, but the license doesn't allow copying, derivatives, or redistribution.


### 3. skill-creator


Also in Anthropic's official repo:[skill-creator](https://github.com/anthropics/skills) , a skill that helps you write skills. It scaffolds the folder, drafts the` SKILL.md` , and applies Anthropic's own guidance on what makes instructions reliable. The fastest way to turn a workflow you repeat every week into something Claude can run.


### 4. frontend-design


Anthropic's[frontend-design](https://github.com/anthropics/skills) skill pushes Claude toward distinctive, polished web interfaces instead of generic-looking output. Useful on any project where the UI matters and you don't have a design system to lean on.


## Video editing skills


This is our lane. Tella's growth team ships video every week, and these are the skills we built (and use) to do it — plus two excellent community skills for programmatic video.


### 5. Cut Video (Tella)


[Cut Video](https://www.tella.com/skills/cut-video) tightens long recordings: it removes silences, ums, fillers, and dead air while keeping laughs and deliberate pauses. Cuts are driven by forced alignment (Montreal Forced Aligner) rather than raw Whisper timestamps, so word boundaries land within ~10–20ms and nothing gets clipped mid-word. Runs entirely on your machine.


### 6. Clipify (Tella)


[Clipify](https://www.tella.com/skills/clipify) turns long videos into social-ready clips: it scans the transcript for clip-worthy moments, reframes 16:9 → 9:16 with pans that follow the speaker, and burns opus-style word-by-word captions. About 20 seconds of work for a 20-second clip on Apple Silicon, no cloud APIs.


### 7. Add Zooms (Tella)


[Add Zooms](https://www.tella.com/skills/add-zooms) gives flat talking-head clips their energy: hard-cut punch-zooms on emphasized words, ratchet zooms for listicles, and slow Ken Burns pushes on key lines. Transcript-driven and non-destructive — you hand it a small JSON map of moments and it renders with ffmpeg.


### 8. B-Roll Finder (Tella)


[B-Roll Finder](https://www.tella.com/skills/b-roll-finder) sources accurate, on-brand b-roll for talking-head video and places each cutaway on the exact word. It classifies each beat of your transcript, routes it to the right kind of footage, and returns vetted candidates for you to pick from — no keyword-matched stock slop.


### 9. video-shotcraft


[video-shotcraft](https://github.com/Vincentwei1021/video-shotcraft) (~3.3k stars) generates cinematic product videos with Remotion from a library of over a hundred shot recipes. If you want launch-video polish without opening an editor — and you're comfortable with video as code — this is the strongest Remotion skill we've seen.


### 10. remotion-video-skill


[remotion-video-skill](https://github.com/wshuyi/remotion-video-skill) is the simpler entry point to programmatic video: it teaches Claude to build Remotion compositions — videos written in React — from a plain-language brief. Good for animated explainers and data-driven video.


## Asset and publishing skills


### 11. Slack Assets Bot (Tella)


[Slack Assets Bot](https://www.tella.com/skills/slack-assets-bot) is the supply line for agentic editing: drop files, YouTube links, or image URLs into a Slack DM and they land organized and searchably named on your machine. Later you tell your editing agent "use the kanye meme" and it finds the file.


### 12. Tella to YouTube


[Tella to YouTube](https://www.tella.com/skills/tella-to-youtube) , by Tom Granot, closes the loop from recording to published video: it grabs your latest[Tella](https://www.tella.com/) recording and transcript, writes a title and description, and uploads the 4K master to YouTube with resumable uploads.


## How to install a Claude Code skill


There isn't one universal install command. Use the method documented by each project:


**Superpowers** is a Claude Code plugin:


```text
/plugin install superpowers@claude-plugins-official


```


**Anthropic's document skills, skill-creator, and frontend-design** come from Anthropic's plugin marketplace. The first plugin installs` docx` ,` pdf` ,` pptx` , and` xlsx` ; the second includes` skill-creator` and` frontend-design` :


```text
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills


```


**Cut Video and Clipify** install directly into Claude Code's skills directory:


```text
git clone https://github.com/louisedesadeleer/cut-video.git ~/.claude/skills/cut-video
git clone https://github.com/louisedesadeleer/clipify.git ~/.claude/skills/clipify


```


**Add Zooms** uses its installer to create an updatable symlink:


```text
git clone https://github.com/louisedesadeleer/add-zooms.git
cd add-zooms
./install.sh


```


**B-Roll Finder** needs its repository registered in` CLAUDE.md` (or its` SKILL.md` copied into your skills directory), plus the command-line tools it drives. On macOS:


```text
git clone https://github.com/louisedesadeleer/b-roll-finder.git
brew install yt-dlp ffmpeg imagemagick
uv tool install mlx-whisper


```


**video-shotcraft** supports the skills CLI:


```text
npx skills add Vincentwei1021/video-shotcraft


```


**remotion-video-skill** is copied into the skills directory. Edge TTS is its free quick-start option; it also requires Node.js, Python, and ffmpeg:


```text
git clone https://github.com/wshuyi/remotion-video-skill.git ~/.claude/skills/remotion-video
pip install edge-tts


```


**Slack Assets Bot** requires a Slack app created from the repository's` manifest.yml` , then its setup wizard installs dependencies and configures the bot:


```text
git clone https://github.com/louisedesadeleer/slack-assets-bot.git
cd slack-assets-bot
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python setup.py


```


**Tella to YouTube** is a plugin. On first use, its bundled setup wizard configures your Tella and YouTube credentials:


```text
/plugin marketplace add TomGranot/tella-to-youtube
/plugin install tella-to-youtube@tella-skills


```


After installing a direct skill, restart Claude Code so it becomes available as a slash command or gets picked up automatically when relevant. Project-specific skills can live in` .claude/skills/` inside a repo so your whole team gets them.


## Where to find more skills


- [Tella's Claude Code skills directory](https://www.tella.com/skills) — our open-source video skills, with demos, install commands, and usage guides for each.
- [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) — the largest curated index of skills across every category.
- [Anthropic's skills repo](https://github.com/anthropics/skills) — the official reference collection, plus the spec for building your own.


## FAQ


**What are Claude Code skills?** A skill is a folder containing a` SKILL.md` file with instructions — and optionally scripts or assets — that teaches Claude a specific, repeatable workflow. Claude loads the instructions when the skill is invoked or when it decides the skill is relevant.


**Are Claude Code skills free?** Most projects in this list are open source and free to install. Anthropic's` docx` ,` pdf` ,` pptx` , and` xlsx` skills are source-available under separate terms rather than an open-source license. You need your own Claude Code subscription to run them, and some workflows also require paid services or other dependencies.


**What's the difference between a skill and an MCP server?** Skills are instructions: they teach Claude how to do a workflow using the tools it already has. MCP servers are connections: they give Claude new tools, like access to an external API. The two compose — a skill can orchestrate tools that an MCP server provides.


**Do these skills work with other coding agents?** Many do. A` SKILL.md` is plain markdown, so agents like Codex or Cursor can follow the same instructions —[Tella to YouTube](https://www.tella.com/skills/tella-to-youtube) is explicitly built to be agent-agnostic. Skills that rely on Claude Code plugin infrastructure are less portable.
