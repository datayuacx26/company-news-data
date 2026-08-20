---
schema_version: "1.0.0"
document_id: "6a11805fe7f893c728c87d1f7ad6fd311cda702fcf6bd2d25bd29c8a1f41fc0c"
company_key: "yc-notte"
company: "Notte"
source_id: "yc-notte-news-import-7238aba58520"
canonical_url: "https://www.notte.cc/blog/browser-harnesses-compared"
published_at: "2026-04-25T15:45:00+00:00"
first_seen_at: "2026-07-22T06:23:42.732156+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:9455b73c5f9721e615cc85b0c29d064ce37f9c3e4e57a34dc1f9116b75ac92f9"
---

# Browser harnesses compared: notte-cli, agent-browser, browser-harness, playwright-mcp

A new category of tooling showed up between "agent framework" and "cloud browser": the browser harness. These tools wrap Chrome DevTools Protocol behind a CLI or MCP surface that an LLM can actually drive, without a Python/TS framework wrapped around it.


They look alike at first. All of them give an agent a way to click, type, snapshot, and navigate. The differences show up in everything bundled around the harness: who runs the browser, how credentials work, what you can see when something breaks, what the production path looks like.


## What a browser harness actually is


It sits between the agent loop and the browser. An agent framework (Browser Use, Stagehand) decides *what to do* : click this, scroll there, extract that. A cloud browser (Browserbase, Kernel) decides *where Chromium runs* : your laptop, their fleet. The harness is the contract in the middle: a stable, agent-readable interface to a real browser.


In practice that usually means:


1. A CLI the agent can call as tool calls or shell commands.
2. A snapshot/ref system so the agent can refer to elements without selectors that rot.
3. A transport to a real Chromium: local CDP, remote CDP, or a managed cloud session.


Every tool below checks those boxes. What separates them is everything *outside* the harness: credentials, identities, observability, deployment.


## agent-browser (Vercel Labs)


Native Rust CLI plus daemon, talking CDP directly over IPC. No Node.js in the hot path, no Playwright wrapper.


The core design choice is refs. Snapshot the page, get an accessibility tree where every interactive element is tagged` @e1` ,` @e2` , etc. Click and fill take refs directly, which side-steps selector rot.


script.sh


```text
agent-browser     open     example.com
agent-browser     snapshot     -i                          # accessibility tree with refs
agent-browser     click     @e2                            # click by ref
agent-browser     fill     @e3     "test@example.com"
```


The surface is wide: semantic finding, network interception with HAR recording, React DevTools introspection, Web Vitals, tracing, batch execution, and a` chat` REPL backed by Vercel's AI Gateway.


The` -p` flag swaps the browser provider, so the same commands run against local Chrome, hosted providers, or an iOS Simulator. It also ships an opt-in security stack: auth vault, domain allowlist, action policy gates.


agent-browser stays focused on the harness. Cloud browsers, credentials beyond the local vault, schedules, and replays live somewhere else.


## browser-use/browser-harness


A small Python harness with a direct WebSocket to Chrome. The README frames it bluntly: *"One websocket to Chrome, nothing between. The agent writes what's missing during execution."*


When the agent hits something the harness lacks, it writes a local helper and keeps going. Recurring patterns get distilled into reusable domain skills. The agent generates them; community contributors submit the useful ones as PRs.


Installation is a setup prompt you paste into Claude Code. The harness installs itself, attaches to Chrome, and gets out of the way. You can swap local Chrome for Browser Use Cloud to inherit proxies, captcha solving, and stealth.


The trade-off is stability. For research, a self-modifying harness is the point. For a production endpoint that has to behave the same way at 3am, the lack of a fixed contract matters.


## playwright-mcp (Microsoft)


Playwright exposed as an MCP server. TypeScript, maintained by the Playwright team. Run it over stdio (via` npx` ) or SSE for headless environments.


Accessibility tree first, following Playwright's philosophy. Vision mode exists as an opt-in when you need pixel coordinates. Profile management mirrors Playwright's own model: persistent profile, isolated sessions, or browser-extension mode where you connect to an existing tab with logged-in state.


Client coverage is the broadest: VS Code, Cursor, Claude Desktop, Codex, Copilot, Gemini CLI, Kiro, Windsurf, and more. If your stack speaks MCP, this drops in.


The scope is MCP-only. No CLI for direct invocation, no cloud browser abstraction, no vault, no scheduler. You get Playwright over MCP.


## notte-cli


The CLI front end to a managed cloud browser platform. Go, MIT, installable via Homebrew,` go install` , or source.


script.sh


```text
notte     sessions     start     --browser-type     chrome     --solve-captchas
notte     page     goto     "https://example.com"
notte     page     observe
notte     page     scrape     --instructions     "Extract the top items with titles"
notte     sessions     stop
```


These commands look like the others, but they run on managed infrastructure.` notte sessions start` provisions a cloud session with proxies, stealth, and captcha solving as toggles. Same code locally and in production. The deployment path is` notte functions create` .


The CLI also exposes platform primitives directly: vaults (credentials the LLM never sees), profiles (persistent browser state across sessions), functions (turn any script into a scheduled HTTP endpoint), and agents (managed AI loops with replays).


Every session gets an MP4 replay, network logs, and an action timeline. The CLI installs as a skill in Claude Code, Cursor, and Windsurf via` npx skills add nottelabs/notte-cli` .


## How to pick


If you need... Start with... Why


An MCP-native Playwright surface playwright-mcp Maintained by the Playwright team, broadest client support.


Portability across browser providers agent-browser The provider flag is the core design point.


A self-improving research harness browser-harness The agent extending its own tooling is the point.


CLI, cloud browser, credentials, identities, schedules, and replays in one product notte-cli You're buying the platform; the CLI is the surface.


Most teams feel the platform gap when they try to ship a harness-based prototype to production. Vault rotation, persona signups, the cron job, the replay system. Those things sit outside the harness, but they decide whether the agent actually runs reliably next month.


## What's specifically different about notte-cli


The local-to-remote step is invisible. The same` notte page goto / click / fill / scrape` commands you run locally during dev run unchanged against a managed cloud session in production. Deployment is` notte functions create` .


Credentials never enter the LLM context. The other tools either inherit credentials from the user's Chrome profile (fine for local dev, bad for shared production) or store them locally and trust the agent to read them safely. Notte's vault model is different: the LLM emits a placeholder, the platform substitutes the secret at the browser action layer, and the credential never exists in any prompt or response.


agent-browser's portability is real. playwright-mcp's MCP-nativeness is real. browser-harness's self-modification is real. These tools solve different shapes of the same problem.


If you're picking a harness for production agent work and want one product to own the full path from local CLI to scheduled, observable, credential-safe runs in the cloud, start at[console.notte.cc](https://console.notte.cc/) or[book a demo](https://cal.com/team/notte/demo) .
