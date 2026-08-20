---
schema_version: "1.0.0"
document_id: "f26f5efbcf6f8d79797ab42781769a4b1e1c5925ca34f1f9812bf20c563d6912"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-on-windows"
published_at: "2026-06-13T12:44:52+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:672695e435da03439195905b611dd7ab3b4a5683a16515ff0dd10e2cc6d1f9f5"
---

# OpenClaw on Windows: Complete Setup Guide (Without the WSL Headaches)

## Path 2: Native Windows CLI via PowerShell


If you prefer terminal-first setup or need the Gateway without the GUI, install OpenClaw directly from PowerShell:


```text
iwr   -  useb https:  //  openclaw.ai  /  install.ps1   |   iex
```


Verify the install:


```text
openclaw   --  version
openclaw doctor
openclaw gateway status   --  json
```


Install the Gateway as a Windows managed service (auto-starts with Windows):


```text
openclaw gateway install
openclaw gateway status   --  json
```


For CLI use without a managed service:


```text
openclaw onboard   --  non  -  interactive   --  skip-health
openclaw gateway run
```


This path uses Windows Scheduled Tasks for managed startup when available, falling back to a per-user Startup-folder entry if task creation is denied. Full CLI control, no GUI dependency.


## Path 3: WSL2 Gateway (Most Linux-Compatible)


WSL2 is the most Linux-compatible runtime for OpenClaw on Windows. Use this path for advanced configurations, when you need full Linux tooling, or for headless server operation without any GUI layer.


**Step 1: Enable WSL2**


```text
wsl   --  install
```


Or pick a specific distro:


```text
wsl   --  list   --  online
wsl   --  install   -  d Ubuntu  -  24.04
```


**Step 2: Enable systemd inside WSL**


Open a WSL terminal and run:


```text
sudo   tee   /etc/wsl.conf   >  /dev/null   <<  'EOF'
[boot]
systemd=true
EOF
```


Restart WSL from PowerShell:


```text
wsl   --  shutdown
```


**Step 3: Install OpenClaw inside WSL**


```text
curl   -fsSL   https://openclaw.ai/install.sh   |   bash
openclaw   gateway   status
```


**Step 4: Auto-start the Gateway before Windows login**


Inside WSL:


```text
sudo   loginctl   enable-linger   "$(  whoami  )"
openclaw   gateway   install
```


In PowerShell as Administrator (to start WSL at system boot, before login):


```text
schtasks   /  create   /  tn   "WSL Boot"   /  tr   "wsl.exe -d Ubuntu --exec /bin/true"   /  sc onstart   /  ru SYSTEM
```


Replace` Ubuntu` with your distro name from` wsl --list --verbose` .


Troubleshooting OpenClaw WSL2 setup on Windows — reading error logs and applying fixes


Blink


## Common Windows Setup Errors and Fixes


**Tray icon doesn't appear after install**


Open Task Manager and search for` OpenClaw.Tray.WinUI.exe` . If it's running, the icon is hidden in the collapsed tray area — expand it and pin OpenClaw. If it's not running, launch OpenClaw Companion from the Start menu.


**Local setup fails during the Windows Hub wizard**


Open the setup log:


```text
notepad   "  $  env:  LOCALAPPDATA  \OpenClawTray\Logs\Setup\easy-setup-latest.txt"
```


Most failures trace to: disabled WSL in Windows Features, blocked virtualization in BIOS, or a stale previous WSL state. The log shows the exact failed step.


**"Pairing is required" message after connecting**


The Gateway needs to approve the device. Run from PowerShell or a WSL terminal:


```text
openclaw devices list
openclaw devices approve   <  request-id  >
```


**WSL IP changes after every Windows restart**


WSL gets a new internal IP after each restart, which breaks any manual port forwarding rules. Windows Hub refreshes this automatically. For manual CLI setups, write a short script that re-reads` $(wsl -d Ubuntu -- hostname -I)` and updates your` netsh` portproxy rule on login.


**Git or GitHub connectivity failures inside WSL**


Some corporate networks block HTTPS to GitHub. Try a different network, a VPN, or set a Git HTTP proxy. For token-based auth in the current WSL session:


```text
export   GH_TOKEN  =  "<your-token>"
gh   auth   status
```


## Path 4: Skip Windows Entirely — Blink Claw


If you're not a developer, don't have time for WSL debugging, or need OpenClaw running 24/7 without managing a machine, Blink Claw is the simplest path.


Blink Claw is managed OpenClaw hosting from **$22/month** . You get:


- **No Docker required** — no WSL, no PowerShell, no BIOS virtualization settings
- **24/7 uptime** — your agent runs when your laptop is off or sleeping
- **200+ models included** — GPT-4o, Claude Opus, Gemini, and more, all on one plan
- **Telegram, Discord, and Slack access** — chat with your agent from any platform you already use
- **Auto-patching** — Blink handles every OpenClaw update automatically, no maintenance window needed


For solo users, Blink Claw often costs less than the time spent debugging a WSL2 setup. For teams and businesses, it eliminates the "who manages the server" problem entirely. There's no machine to maintain, no port forwarding to configure, and no WSL IP to chase.


For more on getting started, see the[OpenClaw getting started guide](https://blink.new/blog/openclaw-getting-started) and[how to run OpenClaw without Docker](https://blink.new/blog/how-to-run-openclaw-without-docker) .


## FAQ


No. The older documentation implied Docker was required, but OpenClaw now ships Windows Hub (a native app) and a PowerShell CLI installer. Neither requires Docker Desktop. WSL2 is still used internally by Windows Hub for the Gateway runtime, but Windows Hub provisions and manages it automatically.


WSL2 requires Windows 10 version 2004 (Build 19041) or later, or Windows 11. Home, Pro, and Enterprise editions all support it. If you're on an older Windows 10 build, run Windows Update first.


Yes. Windows Hub is a full GUI app. After the setup wizard, you interact with OpenClaw through the system tray, chat window, and Command Center — no terminal required for day-to-day use.


OpenClaw is designed for a single user per instance. For team access, Blink Claw is the better option — it runs in the cloud and multiple team members can connect via Telegram, Discord, or Slack without sharing a machine or credentials.


Windows Hub is a native GUI app with tray status, a chat window, Command Center diagnostics, and automatic WSL management. The PowerShell CLI install gives you the Gateway and CLI tools without any GUI. Use Windows Hub for a full desktop experience; use the CLI install for headless or server setups.
