---
schema_version: "1.0.0"
document_id: "1dac60e49eed1873bd760fec7711f080c4fb5d0202adebbe5d148feea638d7d2"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-on-raspberry-pi"
published_at: "2026-05-18T12:55:19+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:b1d11cc2ffc4101107a340b8d9ca23f19200b62191c8dcf39e20a234301adf4a"
---

# OpenClaw on Raspberry Pi: Step-by-Step Setup Guide

## Step-by-Step Setup


1


#### Flash Raspberry Pi OS (64-bit — critical)


Download[Raspberry Pi Imager](https://www.raspberrypi.com/software/) and flash **Raspberry Pi OS Lite (64-bit)** — the headless version. No desktop needed; it uses less RAM.


In the Imager settings dialog, pre-configure before flashing:


- Hostname:` openclaw-pi` (or your choice)
- Enable SSH with a username and password
- Configure WiFi if not using Ethernet


Insert the card, boot the Pi, and wait 60 seconds.


2


#### Connect via SSH and update the system


```text
ssh   your-username@openclaw-pi
```


Once connected, update everything:


```text
sudo   apt   update   &&   sudo   apt   upgrade   -y
sudo   apt   install   -y   git   curl   build-essential
```


Set your timezone — this matters for scheduled tasks:


```text
sudo   timedatectl   set-timezone   America/Chicago
```


3


#### Install Node.js 24


OpenClaw requires Node.js 24 or later. Do not use the default` apt` version — it's outdated.


```text
curl   -fsSL   https://deb.nodesource.com/setup_24.x   |   sudo   -E   bash   -
sudo   apt   install   -y   nodejs
node   --version    # should show v24.x
```


4


#### Add swap (if you have 2 GB RAM or less)


On Pi 4B with 1–2 GB RAM, swap prevents out-of-memory crashes during peak usage. Skip this step on 4 GB+ models.


```text
sudo   fallocate   -l   2G   /swapfile
sudo   chmod   600   /swapfile
sudo   mkswap   /swapfile
sudo   swapon   /swapfile
echo   '/swapfile none swap sw 0 0'   |   sudo   tee   -a   /etc/fstab


# Reduce swappiness so the Pi uses swap only when necessary
echo   'vm.swappiness=10'   |   sudo   tee   -a   /etc/sysctl.conf
sudo   sysctl   -p
```


5


#### Install OpenClaw


The official installer handles everything — Node package installation, systemd service setup, and daemon configuration.


```text
curl   -fsSL   https://openclaw.ai/install.sh   |   bash
```


After installation completes, run onboarding:


```text
openclaw   onboard   --install-daemon
```


Follow the wizard. Use **API keys** rather than OAuth for headless devices — OAuth requires a browser. The wizard will prompt you to:


1. Enter your LLM API key (Anthropic or OpenAI)
2. Configure a communication channel (Telegram is easiest)
3. Set your agent's name and personality


6


#### Verify the service is running


```text
openclaw   status
systemctl   --user   status   openclaw-gateway.service
```


You should see the gateway service active. Check logs if something's wrong:


```text
journalctl   --user   -u   openclaw-gateway.service   -f
```


7


#### Configure your agent (SOUL.md and memory)


OpenClaw's personality and instructions live in markdown files at` ~/.openclaw/workspace/` :


- **SOUL.md** — your agent's identity, goals, and behavioral guidelines
- **HEARTBEAT.md** — scheduled tasks your agent runs automatically


Edit SOUL.md to give your agent context:


```text
nano   ~/.openclaw/workspace/SOUL.md
```


Add details about who you are, what you want the agent to focus on, and any standing instructions.


8


#### Enable auto-restart on boot (lingering)


By default, systemd user services don't start until you log in. Fix this so your agent runs even when you're not SSH'd in:


```text
sudo   loginctl   enable-linger   "$(  whoami  )"
```


Verify lingering is enabled:


```text
loginctl   show-user   "$(  whoami  )"   |   grep   Linger
# Should show: Linger=yes
```


Now your agent starts automatically on boot and restarts after crashes.


9


#### Connect via Telegram or Discord


Send your agent a message from your phone. If the onboarding went correctly, you'll get a response.


For a Discord setup — create a bot via the Discord developer portal, get your bot token, and add it during` openclaw configure` . The agent joins your server's channels and you chat from there.


For Telegram — create a bot via @BotFather, get the token, and the onboarding wizard handles the rest.


Use a USB SSD instead of a microSD card. SD cards wear out under constant read/write from an always-on agent. A $20 USB SSD dramatically improves performance and longevity. Check your Pi's[USB boot guide](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html) to enable booting from USB.


## Performance Tips for Raspberry Pi


**Free up GPU memory.** For headless Pi setups:


```text
echo   'gpu_mem=16'   |   sudo   tee   -a   /boot/config.txt
sudo   systemctl   disable   bluetooth
```


**Enable module compile cache.** This speeds up OpenClaw startup on lower-power Pi hosts:


```text
grep   -q   'NODE_COMPILE_CACHE'   ~/.bashrc   ||   cat   >>   ~/.bashrc   <<  'EOF'
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
EOF
source   ~/.bashrc
```


**Use cloud API models — not local LLMs.** This is critical. Even small Ollama models (1B–3B) are too slow on a Pi 4 for useful agent work. The Pi handles the gateway; let Claude or GPT do the inference. A good model config:


```text
{
"agents"  : {
"defaults"  : {
"model"  : {
"primary"  :   "anthropic/claude-sonnet-4-6"  ,
"fallbacks"  : [  "openai/gpt-5.4-mini"  ]
}
}
}
}
```


**Systemd drop-in for stable restarts.** Add a service override for better reliability:


```text
systemctl   --user   edit   openclaw-gateway.service
```


Add:


```text
[Service]
Environment  =  OPENCLAW_NO_RESPAWN  =1
Environment  =  NODE_COMPILE_CACHE  =/var/tmp/openclaw-compile-cache
Restart  =always
RestartSec  =2
TimeoutStartSec  =90
```


Then reload:` systemctl --user daemon-reload && systemctl --user restart openclaw-gateway.service`


## What the Pi Handles Well (and What It Doesn't)


**Pi handles well:**


- Running OpenClaw's gateway 24/7 — this is a Node.js process, not compute-heavy
- Telegram, Discord, WhatsApp channel connections
- Memory management and scheduled tasks (HEARTBEAT.md)
- Web browsing with headless Chromium
- Light local tooling and scripts


**Pi struggles with:**


- Local LLM inference (avoid entirely — use cloud APIs instead)
- Heavy vision model tasks with large images
- Multiple simultaneous heavy coding tasks (latency adds up)


If you want the Pi to handle local inference, a Pi 5 with the[AI HAT+](https://www.raspberrypi.com/products/ai-hat/) is worth exploring. But for pure OpenClaw gateway usage, standard Pi 4/5 with cloud models is the right setup.


## Common Issues and Fixes


Raspberry Pi OpenClaw vs Blink Claw — honest cost and effort comparison


Blink


**"exec format error" on a skill** Your Pi is aarch64 but the binary was compiled for x86. Check with` uname -m` (should show` aarch64` ). Look for a` linux-arm64` release on the tool's GitHub page.


**Service won't start**


```text
journalctl   --user   -u   openclaw-gateway.service   --no-pager   -n   100
openclaw   doctor   --non-interactive
```


Also verify lingering is enabled:` loginctl show-user "$(whoami)" | grep Linger`


**Out of memory**


```text
free   -h    # check swap is active
sudo   systemctl   disable   cups   bluetooth   avahi-daemon    # disable unused services
```


Make sure you're not running local LLMs. Use API models only.


**WiFi drops**


```text
sudo   iwconfig   wlan0   power   off
```


Ethernet is far more stable for an always-on agent.


**SD card corruption after a few weeks** Expected on cheap SD cards under constant write load. Move to a USB SSD. Back up your` ~/.openclaw/` directory regularly:


```text
openclaw   backup   create
```


## Accessing the Control UI from Your Computer


OpenClaw's web UI runs on the Pi but you access it through an SSH tunnel:


```text
# On the Pi, get the dashboard URL:
ssh   user@openclaw-pi   'openclaw dashboard --no-open'


# On your computer, open a tunnel:
ssh   -N   -L   18789:127.0.0.1:18789   user@openclaw-pi
```


Then open the printed URL in your local browser.


## Pi vs Blink Claw — Honest Comparison


Running OpenClaw on a Pi is a legitimate choice. It's cheap to operate, gives you full control, and teaches you a lot about how OpenClaw works. But it comes with real tradeoffs.


Raspberry Pi Blink Claw


Hardware cost $35–80 one-time $0


Setup time 1–2 hours 5 minutes


Uptime Depends on Pi health, SD card, power 24/7 guaranteed


Updates Manual (` openclaw update` ) Automatic


Operating cost ~$3/mo electricity + LLM API bills $22/mo all-in (LLM costs included)


Maintenance Your responsibility Handled


Remote access SSH tunnel or Tailscale required Built-in


Receiving a Telegram message from your always-on OpenClaw agent running on Raspberry Pi


Blink


The Pi wins on cost per month once it's running — ~$3 in electricity is hard to beat. But you're also handling SD card health, power outages, router changes, and manual updates. If your Pi goes offline while you're traveling, your agent goes offline too.


[Blink Claw](https://blink.new/claw) handles this automatically — OpenClaw runs on managed infrastructure, with LLM costs bundled at $22/mo via a 200+ model router. No Docker needed, no VPS setup, and your agent runs 24/7 — not just when your Pi is powered and connected.


If you'd rather skip the self-hosting —[Blink Claw runs OpenClaw for you at $22/mo with no setup](https://blink.new/claw) .


## Related Guides


- [OpenClaw Getting Started](https://blink.new/blog/openclaw-getting-started)
- [OpenClaw SOUL.md and HEARTBEAT.md Setup](https://blink.new/blog/openclaw-soul-heartbeat-setup)
- [How to Run OpenClaw Without Docker](https://blink.new/blog/how-to-run-openclaw-without-docker)


## FAQ


Technically yes, but it's slow. The Pi 3B+ has 1 GB RAM and a much weaker CPU than the Pi 4. You'll see frequent swapping and sluggish response times. If you have a Pi 3, it's worth the $35–55 upgrade to a Pi 4 with 4 GB for a dramatically better experience.


No. The official installer (` curl -fsSL https://openclaw.ai/install.sh | bash` ) installs OpenClaw natively via Node.js. Docker is not required and is not recommended on Pi — it adds overhead and complexity. The native install is simpler and faster.


On Pi 5 with 8 GB RAM, you can run very small models (1B–2B) via Ollama for lightweight tasks. On Pi 4 with 4 GB or less, skip Ollama entirely — even small models consume too much memory and will cause your agent to swap constantly. Use cloud API models (Claude, GPT) instead — the Pi handles the gateway, not the inference.


With` sudo loginctl enable-linger "$(whoami)"` configured, OpenClaw's systemd service starts automatically on boot. It typically takes 30–60 seconds to be fully online after a reboot. The only exception is extended power outages — if your Pi is offline, your agent is offline. A UPS (uninterruptible power supply) solves this if uptime is critical.


The Pi itself draws 3–7W depending on load. At average US electricity rates (~$0.16/kWh), a Pi 4 running 24/7 costs roughly $3–5/month. You also pay separately for LLM API usage (Anthropic, OpenAI, etc.) — typically $5–20/month depending on how heavily you use the agent. Total: roughly $8–25/month, plus the one-time hardware cost.


Yes, for anything you want to run 24/7. Laptops sleep, get closed, travel with you, and run other workloads that compete for resources. A dedicated Pi runs continuously, doesn't interrupt your daily machine, and costs almost nothing in electricity. The tradeoff is setup time and ongoing maintenance.
