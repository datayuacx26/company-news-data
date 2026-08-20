---
schema_version: "1.0.0"
document_id: "06ff759f728652828a2f3608c7fcb1b19b25bd369a258664018ce5314f69c4ef"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-on-synology-nas"
published_at: "2026-05-10T01:13:29+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:07.653203+00:00"
content_hash: "sha256:1e9a746c0bcb4a864da0b0b92578cd4732928b058f1087898a668729643f6a83"
---

# OpenClaw on Synology NAS: Run Your AI Agent 24/7 From Your Home Server

## Step 2: Pull the OpenClaw image


1


#### Open a terminal session


In DSM, navigate to **Control Panel → Terminal & SNMP** and enable SSH. Then SSH into your NAS:


```text
ssh   admin@your-nas-ip
```


Or use the **Container Manager → Terminal** feature if you prefer staying in the browser.


2


#### Pull the OpenClaw Docker image


Run this command to pull the latest OpenClaw image:


```text
sudo   docker   pull   ghcr.io/openclaw/openclaw:latest
```


For ARM-based NAS units (older models), specify the platform explicitly:


```text
sudo   docker   pull   --platform   linux/arm/v7   ghcr.io/openclaw/openclaw:latest
```


The image is approximately 1.2GB. On a typical home internet connection, expect 5–10 minutes for the download.


3


#### Create the persistent storage volume directory


OpenClaw needs a persistent directory for its config, memory, and skills data. Create one on your NAS volume:


```text
mkdir   -p   /volume1/docker/openclaw/data
mkdir   -p   /volume1/docker/openclaw/logs
```


Replace` /volume1` with the actual volume where you want to store OpenClaw data. Check your volume name in **Storage Manager** if unsure.


## Step 3: Run OpenClaw with Docker


This is the core Docker run command for Synology. Copy it, fill in your values, then run it from the SSH terminal:


```text
sudo   docker   run   -d   \
--name   openclaw   \
--restart   unless-stopped   \
-p   18789:18789   \
-v   /volume1/docker/openclaw/data:/data   \
-v   /volume1/docker/openclaw/logs:/logs   \
-e   OPENCLAW_BASE_URL=https://your-nas-domain.synology.me   \
-e   NODE_ENV=production   \
ghcr.io/openclaw/openclaw:latest
```


**What each flag does:**


- ` --restart unless-stopped` — OpenClaw automatically restarts after DSM reboots or if the container crashes. This is what makes it truly 24/7.
- ` -p 18789:18789` — Maps the OpenClaw gateway port to the host. You'll configure Synology's reverse proxy to sit in front of this.
- ` -v /volume1/docker/openclaw/data:/data` — Mounts your persistent storage directory. Skills, memory, and config survive container restarts and image updates.
- ` OPENCLAW_BASE_URL` — Set this to the external URL where OpenClaw will be reachable (your DDNS hostname or custom domain).


Verify it started correctly:


```text
sudo   docker   logs   openclaw   --tail   50
```


You should see` OpenClaw gateway started on port 18789` in the logs within 15–30 seconds.


## Step 4: Configure Synology's reverse proxy with HTTPS


Running OpenClaw behind Synology's built-in reverse proxy gives you HTTPS for free using Let's Encrypt — no manual certificate management.


1


#### Enable DDNS or use your own domain


In DSM, go to **Control Panel → External Access → DDNS** . If you don't have a custom domain, Synology's free DDNS gives you a hostname like` yourname.synology.me` . Enable it and note the hostname.


2


#### Open the reverse proxy settings


Go to **Control Panel → Login Portal → Advanced → Reverse Proxy** . Click **Create** .


3


#### Configure the proxy rule


Fill in the form:


- **Description:** OpenClaw
- **Source protocol:** HTTPS
- **Source hostname:**` openclaw.yourname.synology.me` (or a subdomain of your custom domain)
- **Source port:** 443
- **Destination protocol:** HTTP
- **Destination hostname:** localhost
- **Destination port:** 18789


Click **Save** .


4


#### Request a Let's Encrypt certificate


Go to **Control Panel → Security → Certificate** . Click **Add → Add a new certificate → Get a certificate from Let's Encrypt** . Enter the subdomain you configured above. DSM handles renewal automatically.


5


#### Assign the certificate to your proxy rule


In the **Certificate** tab, click **Configure** . Assign the new certificate to the OpenClaw reverse proxy service. Click **OK** .


After these steps, OpenClaw is reachable at` https://openclaw.yourname.synology.me` with a valid certificate. No Nginx config files, no Certbot cron jobs.


## Step 5: Verify everything is working


From any browser, navigate to` https://openclaw.yourname.synology.me` . You should see the OpenClaw web UI load over HTTPS.


Check that the agent is running and the Telegram or Discord integration works if you've configured one. One of the best things about running OpenClaw on a NAS is that you can message it from Telegram at 2am and it's still there — not just when your laptop is on.


Power usage: OpenClaw adds approximately 10W to your NAS's power draw. At average US electricity rates, that's roughly $1.20/month added to your electricity bill. For a 24/7 AI agent, that's hard to beat.


## When to use Blink Claw instead


Running OpenClaw on your Synology NAS is a great option. It's also genuinely maintenance-intensive.


You'll need to:


- Track OpenClaw releases and pull new images manually (or write a cron job)
- Monitor your NAS for disk health, DSM updates, and RAM pressure
- Handle SSL certificate renewal when Let's Encrypt fails
- Restart the container when DSM updates require a reboot
- Maintain your home internet connection's uptime (power outages, ISP issues)


If any of those feel like chores you'd rather skip,[Blink Claw](https://blink.new/claw) handles all of it. No Docker needed, no VPS setup — your agent runs in 30+ data center regions, security patches are applied automatically, and LLM costs are included in the $22/mo all-in price.


The NAS setup is satisfying if you enjoy the tinkering. Blink Claw is better if you want the agent running reliably while you focus on what the agent actually does.


**Specifically avoid the NAS route if:**


- Your NAS is a DS220j or older J-series (512MB RAM, non-upgradeable)
- You travel frequently and your home power or internet isn't reliable
- You want your agent to stay online during power outages (NAS needs a UPS for this)
- You prefer not to expose your home IP address to the internet


## Frequently Asked Questions


Container Manager (and Docker) requires a 64-bit Intel or AMD processor. The Plus and xs+ series (DS923+, DS1522+, DS1823xs+, DS923+, DS720+, DS920+) all qualify. The J-series (DS220j, DS420j, DS723j) and Play series use ARM processors and either don't support Docker at all or have limited compatibility. Check the[Synology compatibility list](https://www.synology.com/en-us/dsm/packages/ContainerManager) for your specific model.


The hardware and electricity are your only costs. OpenClaw itself is open source and free. Your NAS adding 10W for OpenClaw comes to about $1.20/month in electricity at average US rates. You'll also need an API key for your LLM provider (Anthropic, OpenAI, etc.) — that's billed separately based on usage.[Blink Claw](https://blink.new/claw) bundles LLM costs into the $22/mo all-in price if you'd rather not manage API keys separately.


Yes, if you used` --restart unless-stopped` in the Docker run command. That flag tells Docker to restart the container automatically after the Docker daemon itself restarts — which includes DSM reboots. The container won't restart if you explicitly stopped it with` docker stop` .


Yes. Run additional containers on different ports (18790, 18791, etc.) with separate data directories. Each instance needs its own API keys and its own reverse proxy rule. RAM is the limiting factor — each instance uses 400–700MB under load, so a 4GB NAS can typically support 2–3 concurrent instances.


Self-hosting gives you full control and zero recurring subscription cost beyond electricity and API keys.[Blink Claw](https://blink.new/claw) ($22/mo all-in) gives you managed hosting in 30+ global data centers, automatic security patches, LLM costs included via 200+ model router, and no Docker setup. The right choice depends on whether you value control or convenience more.
