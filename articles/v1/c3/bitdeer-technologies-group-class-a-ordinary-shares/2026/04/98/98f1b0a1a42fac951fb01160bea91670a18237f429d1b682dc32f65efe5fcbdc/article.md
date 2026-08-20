---
schema_version: "1.0.0"
document_id: "98f1b0a1a42fac951fb01160bea91670a18237f429d1b682dc32f65efe5fcbdc"
company_key: "bitdeer-technologies-group-class-a-ordinary-shares"
company: "Bitdeer Technologies Group"
source_id: "bitdeer-technologies-group-class-a-ordinary-shares-rss-2127a671e698"
canonical_url: "https://www.bitdeer.ai/en/blog/getting-started-with-nemoclaw-on-bitdeer-ai-cloud/"
published_at: "2026-04-10T06:10:05+00:00"
first_seen_at: "2026-07-24T08:23:43.073205+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:7403b2a146bdfbd44698d1f8083e71fd6687e08292f6ed150b05534907e13118"
---

# Getting Started with NemoClaw on Bitdeer AI Cloud

Deploy secure, production-ready AI agents in minutes with NVIDIA NemoClaw on Bitdeer AI Cloud. In this guide, you’ll learn how to quickly set up a fully sandboxed environment and run powerful models like Moonshot AI’s Kimi K2.5 through Bitdeer AI’s high-performance inference endpoints.


## **What You'll Build**


By the end of this guide, you'll have:


- A secure, sandboxed AI agent running on Bitdeer AI Cloud
- Access to powerful models like Kimi K2.5 through Bitdeer AI's inference API
- Full network policy controls and monitoring capabilities


## **Why NemoClaw on Bitdeer AI?**


NemoClaw on Bitdeer AI Cloud brings together secure agent execution with scalable infrastructure and high-performance inference. While sandboxed execution with Landlock, seccomp, and network isolation is handled within the NemoClaw and OpenShell framework, Bitdeer AI Cloud can provide the underlying compute environment, enabling on-demand scaling of CPU and GPU resources with usage-based pricing. Our platform also delivers access to optimized inference endpoints for running leading models, along with network-level controls to help manage how agents interact with external services, ensuring both flexibility and operational control.


## **Prerequisites**


**NemoClaw System Requirements Minimum:** 4+ vCPU, 16 GB RAM, 40 GB of free disk space, and Ubuntu 22.04 LTS.


**One Bitdeer AI Cloud Requirements**


- Active[Bitdeer AI Cloud](https://www.bitdeer.ai/en/instance/server?ref=bitdeer.ai) account
- API key from Bitdeer AI Cloud (for inference)


**Bitdeer AI Instance:** Select **g4a.xlarge (** 4 vCPU, 16 GB RAM **)**


**Important:** The sandbox image is approximately 2.4 GB compressed. For instances with 16 GB RAM, you'll have sufficient memory for smooth operation.


## **Software Dependencies**


- Node.js 20 or later
- npm 10 or later
- Docker (installed and running)
- OpenShell CLI (installed by NemoClaw)


### **1.1 Create Virtual Machine**


1. Log in to your[Bitdeer AI Cloud](https://www.bitdeer.ai/en?ref=bitdeer.ai) account


2. Navigate to **Virtual Machine** services


3. Click **Create Instance** and select:


**Instance Type:** **g4a.xlarge** (4 vCPU, 16 GB RAM)


**Storage:** 40 GB SSD minimum


**OS:** Ubuntu 22.04 LTS


**Region:** Choose nearest to your location


4. Complete the order and wait for instance provisioning


5. Once running, note the public IP address


### **1.2 Access Your Instance**


Connect via SSH:


```text
ssh ubuntu@<your-instance-ip>


```


## **Step 2: Install Docker**


NemoClaw requires Docker as the container runtime:


```text
# Update package index
sudo apt-get update


# Install Docker (service starts automatically)
sudo apt-get install -y docker.io


# Add user to docker group
sudo usermod -aG docker $USER


```


**Important:** Log out and log back in for group changes to take effect:


```text
exit
ssh ubuntu@<your-instance-ip>


```


Verify Docker installation:


```text
docker --version
docker run hello-world


```


## **Step 3: Get Bitdeer AI API Key**


Before installing NemoClaw, obtain your Bitdeer AI API key:


1. Log in to Bitdeer AI Cloud


2. Navigate to[Models](https://www.bitdeer.ai/en/model/explore?ref=bitdeer.ai) → **API Keys**


3. Click **Generate API Key**


4. Copy the generated key and save it securely


**⚠ Security Warning:** Never commit API keys to version control or share them publicly.


## **Step 4: Install and Configure NemoClaw**


Download and run the NemoClaw installer, which includes an interactive wizard for configuring your Bitdeer AI inference endpoint:


```text
curl -fsSL https://www.nvidia.com/nemoclaw.sh | bash


```


**During the Installation Wizard**


When prompted, configure Bitdeer AI as your inference provider:


```text
[2/7] Configuring inference (NIM)
──────────────────────────────────────────────────


Inference options:
1) NVIDIA Endpoints (recommended)
2) OpenAI
3) Other OpenAI-compatible endpoint  ← Select this
4) Anthropic
5) Other Anthropic-compatible endpoint
6) Google Gemini


Choose [1]: 3


OpenAI-compatible base URL: https://api-inference.bitdeer.ai/v1
Other OpenAI-compatible endpoint API key: <paste-your-bitdeer-api-key>


Other OpenAI-compatible endpoint model []: nvidia/NVIDIA-Nemotron-3-Super-120B-A12B
# or: moonshotai/Kimi-K2.5


```


The wizard will:


- Install Node.js 20+ if not present
- Install npm packages
- Set up the NemoClaw CLI
- Configure Bitdeer AI as your inference provider
- Save your API key securely to ~/.nemoclaw/credentials.json
- Create your first sandbox


**Post-Installation Summary**


After installation completes, you should see:


```text
Sandbox      my-bitdeer-agent (Landlock + seccomp + netns)
Model        nvidia/NVIDIA-Nemotron-3-Super-120B-A12B (Bitdeer AI Endpoint)
Run:         nemoclaw my-bitdeer-agent connect
Status:      nemoclaw my-bitdeer-agent status
Logs:        nemoclaw my-bitdeer-agent logs --follow


[INFO]  === Installation complete ===


```


If nemoclaw is not found after installation, run:


```text
source ~/.bashrc
# or for zsh users:
source ~/.zshrc


```


Verify installation:


```text
nemoclaw --version


```


## **Step 5: Connect and Chat with Your Agent**


**5.1 Connect to the Sandbox**


```text
nemoclaw my-bitdeer-agent connect


```


You'll enter the sandbox shell:


```text
sandbox@my-bitdeer-agent:~$


```


**5.2 Interactive TUI Mode**


Launch the OpenClaw TUI for interactive chat:


```text
openclaw tui


```


Send test messages and verify responses.


**5.3 CLI Mode**


For programmatic access or long outputs, use the CLI:


```text
openclaw agent --agent main --local -m "Hello, how can you help me today?" --session-id test


```


To exit the sandbox shell:


```text
exit


```


## **What's Next?**


Now that you have your NemoClaw agent running on Bitdeer AI Cloud, here are some ways to extend it:


- **Connect to Telegram**


Enable mobile access to your agent:


```text
nemoclaw my-bitdeer-agent telegram-setup


```


- **Try Different Models**


Experiment with other available models through Bitdeer AI Model Studio:


```text
openshell inference set --provider openai --model zai-org/GLM-5


```


- **Monitor Activity**


Launch the OpenShell TUI to watch your agent in action:


```text
openshell tui


```


- **Customize Security**


Define custom network policies to control what your agent can access.


## Conclusion


With NemoClaw running on Bitdeer AI Cloud, deploying secure, production-ready AI agents becomes both practical and scalable. By combining NemoClaw’s sandboxed execution and policy framework with Bitdeer AI’s flexible infrastructure and optimized inference endpoints, you can move from setup to real-world usage in just a few steps. Whether you are experimenting with AI agents or building toward production, Bitdeer AI Cloud offers a reliable environment to run, scale, and manage your workloads with confidence.


### **Resources**


**Documentation:**


- [NemoClaw Docs](https://docs.nvidia.com/nemoclaw/latest/?ref=bitdeer.ai)
- [OpenShell Docs](https://docs.nvidia.com/openshell/latest/?ref=bitdeer.ai)
- [Bitdeer AI Cloud](https://www.bitdeer.ai/en/instance/server?ref=bitdeer.ai)


**Community & Support:**


- [NemoClaw GitHub](https://github.com/NVIDIA/NemoClaw?ref=bitdeer.ai)
- [NemoClaw Discord](https://www.bitdeer.ai/en/blog/getting-started-with-nemoclaw-on-bitdeer-ai-cloud/NemoClaw%20Discord)
- [Bitdeer AI Support](https://www.bitdeer.ai/en/contact?ref=bitdeer.ai)
