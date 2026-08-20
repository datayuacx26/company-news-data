---
schema_version: "1.0.0"
document_id: "08454a63e87e2b34253c8abe5093727dcf00ce9f0800e367f8835ea48f5d0cc4"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-on-mac"
published_at: "2026-05-07T12:16:17+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:16.827560+00:00"
content_hash: "sha256:8722e986222268e87172f0e05583cc77cd8b1e3f7166c17d681ee57a47bcc2e2"
---

# How to Run OpenClaw on Mac (M1, M2, M3): Complete Setup Guide

## Step 2: Run Onboarding and Install the Daemon


```text
openclaw   onboard   --install-daemon
```


This does three things: runs the setup wizard, configures your first AI provider, and installs a launchd service so the OpenClaw gateway starts automatically at login.


The launchd service is labeled` ai.openclaw.gateway` . It restarts the gateway if it crashes. Without` --install-daemon` , your agent stops working when you close the terminal.


To skip the wizard and configure manually:


```text
openclaw   onboard   --no-onboard   --install-daemon
```


## Step 3: Verify the Install


```text
openclaw   doctor
```


Run this after every install or major update. It validates your gateway service, checks file permissions, inspects the launchd config, and warns if something else is already on port 18789.


If it finds issues:


```text
openclaw   doctor   --fix
```


This auto-repairs what it can: broken permissions, missing directories, stale config keys, macOS LaunchAgent mismatches.


## Step 4: Apple Silicon vs Intel — What Actually Matters


Check which binary you're running:


```text
file   $(  which   openclaw  )
```


You want` arm64` . If you see` x86_64` , you're on Rosetta — it works but is slower for anything touching local model inference. Reinstall via Homebrew to get the native build.


Apple Silicon memory allocation for OpenClaw — gateway uses 200-500MB, model is the expensive part


Blink


On Apple Silicon, the gateway itself uses 200–500MB. The real memory cost is the AI model you connect to it. Here's what fits:


Mac RAM What fits comfortably


8GB OpenClaw + 3B model (tight — close browsers first)


16GB OpenClaw + 7B–14B Q4 model


24GB OpenClaw + 14B Q6 or 32B Q4


32–48GB OpenClaw + 32B Q8


64GB+ OpenClaw + 70B Q4 or any 122B MoE


On Intel Mac: OpenClaw runs fine. Local model inference through Ollama is slower — no Metal GPU acceleration for LLM inference on Intel chips. For always-on setups, an Apple Silicon Mac Mini is a much better host.


## Step 5: Connect to a Model


OpenClaw is a gateway — it routes messages and manages tools, but doesn't run a model itself. The most common Mac setup is OpenClaw + Ollama.


**Install Ollama if you don't have it:**


```text
brew   install   ollama
```


**Start Ollama:**


```text
ollama   serve
```


**Pull a model (16GB Mac example):**


```text
ollama   pull   qwen2.5-coder:7b
```


**Tell OpenClaw where to find Ollama:**


```text
launchctl   setenv   OLLAMA_API_KEY   "ollama-local"
```


Don't use` export OLLAMA_API_KEY=...` in your shell profile. The OpenClaw gateway runs as a launchd service, not a shell process. Shell env vars are ignored. Always use` launchctl setenv` for persistent env vars that the gateway needs.


After setting env vars, restart the gateway:


```text
openclaw   gateway   restart
```


Verify Ollama is visible:


```text
curl   http://localhost:11434/api/tags
```


If that returns your model list, OpenClaw can find Ollama.


**Using a cloud API instead?** Skip Ollama. Run` openclaw onboard` and enter your OpenAI or Anthropic API key when prompted. The key stores in` ~/.openclaw/openclaw.json` .


## Step 6: Connect Telegram (or Discord)


The real payoff of OpenClaw on Mac is chatting with your agent from your phone while your Mac runs the brain.


**Set up a Telegram bot:**


1. Message` @BotFather` on Telegram
2. Send` /newbot` , follow the prompts, copy the bot token
3. In your OpenClaw config (` ~/.openclaw/openclaw.json` ), add:


```text
{
"channels"  : {
"telegram"  : {
"token"  :   "YOUR_BOT_TOKEN"
}
}
}
```


1. Restart the gateway:` openclaw gateway restart`
2. Message your bot on Telegram — it should respond.


Telegram uses long-polling by default, so you don't need to open any inbound ports. Your bot polls Telegram's servers. No Cloudflare tunnel, no port forwarding.


For Discord, install the Discord plugin:


```text
openclaw   plugins   install   @openclaw/discord
openclaw   doctor   --fix
```


See the[full OpenClaw getting started guide](https://blink.new/blog/openclaw-getting-started) for detailed channel configuration.


## macOS-Specific Gotchas


These are the things that cost time if you don't know them.


**Env vars must use` launchctl setenv`**


Set environment variables for the gateway through launchd, not your shell:


```text
launchctl   setenv   OPENAI_API_KEY   "sk-..."
openclaw   gateway   restart
```


Setting vars in` .zshrc` or` .zprofile` does nothing for the gateway process.


**Spotlight indexes model files**


When Ollama downloads a 4+ GB model, Spotlight tries to index it. On an 8GB Mac this can push you into swap. Exclude it:


*System Settings → Siri & Spotlight → Spotlight Privacy → add` ~/.ollama/models/`*


Do the same for` ~/.openclaw/` to avoid indexing session transcripts.


**Time Machine backs up model files**


Exclude` ~/.ollama/models/` from Time Machine backups. A 4.5GB GGUF file is just a downloaded artifact — no need to back it up.


**Always-on Mac Mini setup**


If you want your agent running 24/7 on a Mac Mini (not dependent on your laptop being open):


```text
# Disable sleep
sudo   pmset   -a   sleep   0   displaysleep   0   disksleep   0


# Auto-restart after power failure
sudo   pmset   -a   autorestart   1
```


A Mac Mini M4 Pro at $600–800 costs about 10–15W idle. That's roughly $15–20/year in electricity for a personal AI server that runs 24/7.


**After updates, run doctor**


After` brew upgrade openclaw-cli` or` openclaw update` , the launchd service config can become stale:


```text
openclaw   doctor   --fix
```


This rewrites the launchd plist if it's pointing at the wrong binary path.


## The Honest Tradeoff: Self-Hosted vs Blink Claw


Running OpenClaw on your own Mac works well — especially on a Mac Mini as a dedicated AI server. The experience is genuinely good once it's configured.


The catch: your agent runs only when your Mac is on. A closed laptop means no agent. A power outage means no agent. A macOS update that breaks launchd means no agent until you debug it.


[Blink Claw](https://blink.new/claw) runs OpenClaw in a managed container across 30+ data center regions. Your agent runs 24/7, not just when your laptop is open. There's no Docker to configure, no VPS to maintain, no security patches to track. LLM costs are included via a 200+ model router. Starts at $22/mo all-in.


If you want a personal always-on agent without the ops work, that's the path. See the[Blink Claw vs clawctl comparison](https://blink.new/blog/blink-claw-vs-clawctl) for a full breakdown.


If you're happy maintaining your own Mac setup — the steps above are everything you need.


## Frequently Asked Questions


By default, the gateway binds to` 127.0.0.1` only. To allow connections from other devices on your network, update the` server.listenAddress` in` ~/.openclaw/openclaw.json` to` 0.0.0.0` . Then add an exception in macOS Firewall (System Settings → Network → Firewall) for the OpenClaw process. You don't need this for Telegram or Discord — those use outbound long-polling, not inbound connections.


Run` openclaw doctor --fix` . macOS updates sometimes invalidate launchd plist configs, especially after major OS version jumps. The doctor command detects stale service configs and rewrites them. If that doesn't fix it, try` openclaw onboard --install-daemon` to reinstall the daemon from scratch.


Yes, but it's tight. The gateway itself uses 200–500MB. Add macOS overhead (2–3GB) and a 3B local model (~2.5GB), and you're near the limit. Close browsers and Slack before running large tasks. Alternatively, point OpenClaw at a cloud API (OpenAI, Anthropic) instead of a local Ollama model — cloud inference has no local memory cost.


The built-in auto-updater (` update.auto` ) is off by default. If you installed via Homebrew and don't want automatic updates during` brew upgrade` , pin the package:` brew pin openclaw-cli` . You can then update manually with` brew upgrade openclaw-cli` when you're ready.


Yes. Use` $KEYCHAIN:secret-name` in your config, and store the key with:` security add-generic-password -a openclaw -s "anthropic-api-key" -w "your-key"` . The config loader resolves these at startup via` security find-generic-password` . This is more secure than plaintext in` ~/.openclaw/openclaw.json` , especially on machines with iCloud Keychain enabled.


Check whether you're on the native ARM64 binary:` file $(which openclaw)` . If it says` x86_64` , you're running through Rosetta — reinstall via Homebrew. Also check` ollama ps` to confirm the model is loaded on GPU (Metal), not CPU. If the model is too large for your unified memory, it will partially offload to CPU, reducing token speed significantly.


For more setup context, see the[OpenClaw soul and heartbeat setup guide](https://blink.new/blog/openclaw-soul-heartbeat-setup) and the[getting started guide](https://blink.new/blog/openclaw-getting-started) .
