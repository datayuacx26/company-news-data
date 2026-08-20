---
schema_version: "1.0.0"
document_id: "1dd1409a58e4625912e9e70c175380d7e42abf068e6c6eecb2fcfe7d3837896c"
company_key: "bitdeer-technologies-group-class-a-ordinary-shares"
company: "Bitdeer Technologies Group"
source_id: "bitdeer-technologies-group-class-a-ordinary-shares-rss-2127a671e698"
canonical_url: "https://www.bitdeer.ai/en/blog/installing-and-configuring-openclaw-on-bitdeer-ai-cloud/"
published_at: "2026-02-03T10:30:59+00:00"
first_seen_at: "2026-07-24T08:23:43.073205+00:00"
fetched_at: "2026-07-28T22:22:53.892181+00:00"
content_hash: "sha256:b7b5b5418dd2d7d303ca7a5509b231c98c3fda6039ac28b6fd290b12d1ac8c0e"
---

# Installing and Configuring OpenClaw on Bitdeer AI Cloud

OpenClaw is a powerful AI agent framework that enables you to build intelligent assistants with multiple capabilities, including web search, code execution, and integration with various AI models. With Bitdeer AI Cloud, OpenClaw can be deployed online with flexible resource allocation and usage-based pricing, making it easier to run AI agents without long-term hardware constraints.


In this guide, we'll walk through the complete process of deploying OpenClaw on Bitdeer AI Cloud and configuring it to work with Telegram for an interactive AI assistant experience.


## **Prerequisites**


Before getting started, ensure you have:


- A Bitdeer AI Cloud account
- Basic knowledge of Linux command line
- A Telegram account for bot integration


## **Step 1: Setting Up Bitdeer AI Cloud Instance**


### **Ordering a Virtual Machine**


First, navigate to the Bitdeer AI Cloud platform and place an order for a virtual machine instance.


Select an appropriate instance configuration based on your needs. For OpenClaw, we recommend:


- **CPU** : At least 2 cores
- **RAM** : Minimum 4GB
- **Storage** : At least 20GB SSD
- **OS** : Ubuntu 20.04 or later


Once your order is confirmed, wait for the instance to be provisioned.


### **Accessing Your Instance**


After the instance is ready, you'll see it in your dashboard with a "Running" status.


Connect to your instance using SSH:


```text
ssh username@your-instance-ip


```


## **Step 2: Installing OpenClaw**


### **Quick Installation**


OpenClaw provides a simple one-line installation script that handles all dependencies automatically:


```text
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard


```


This command will:


- Install all required system dependencies
- Set up Node.js environment
- Install OpenClaw


Once the installation is completed, verify it:


```text
openclaw --version


```


**💡 Tip:** If you encounter openclaw: *command not found* , you may need to source your *.bashrc* file for the current session or update your PATH:


> ```text
> source ~/.bashrc
> # Or manually add to PATH if necessary
> export PATH="$HOME/.npm-global/bin:$PATH""
>
>
> ```


## **Step 3: Configuring OpenClaw**


### **Initial Setup**


Run the OpenClaw setup wizard to initialize your environment:


```text
openclaw setup


```


This will create the basic configuration structure for OpenClaw.


### **Configuring AI Models**


After the setup completes, edit the OpenClaw configuration file to add your AI model settings:


```text
vim ~/.openclaw/openclaw.json


```


Update the configuration with your preferred models:


```text
{
"agents": {
"defaults": {
"model": {
"primary": "bitdeerai/moonshotai/Kimi-K2.5"
},
"workspace": "/home/ubuntu/.openclaw/workspace"
}
},
"models": {
"mode": "merge",
"providers": {
"bitdeerai": {
"baseUrl": "https://api-inference.bitdeer.ai/v1",
"apiKey": "your-api-key-here",
"api": "openai-completions",
"models": [
{
"id": "moonshotai/Kimi-K2.5",
"name": "Kimi-K2.5"
}
]
}
}
}
}


```


You can find more details in model card here:


### **Getting Bitdeer AI Cloud API Key**


To obtain your API key from Bitdeer AI Cloud:


1. Log in to your[Bitdeer AI Cloud](https://www.bitdeer.ai/en/instance/server?ref=bitdeer.ai) account
2. Navigate to the[API Keys](https://www.bitdeer.ai/en/model/apikeys?ref=bitdeer.ai) section in Models
3. Click "Generate API Key"
4. Copy the generated API key and save it securely


**⚠️ Important:** Keep your API keys secure and never commit them to version control.


### **Verify Model Configuration**


After configuring your models, verify that they are properly set up by listing available models:


```text
openclaw models list


```


This command will display configured models and their availability.


### **Onboarding Process**


Complete the onboarding process to finalize your OpenClaw service:


```text
openclaw onboard --flow quickstart


```


### **Setting Up Telegram Channel**


During the onboarding process, select Telegram as your communication channel and provide your bot token.


After completing the onboarding process, the OpenClaw service will automatically start and run in the background.


### **Accessing OpenClaw Control Panel (Optional)**


If you want to access the OpenClaw control panel via web interface, you can set up an SSH tunnel and access it using the gateway token.


First, find your gateway token in the configuration file:


```text
cat ~/.openclaw/openclaw.json


```


Look for the gateway configuration section:


```text
{
"gateway": {
"mode": "local",
"auth": {
"mode": "token",
"token": "your-gateway-token"
},
"port": 18789
}
}


```


Create an SSH tunnel from your local machine to access the OpenClaw control panel:


```text
# Run this on your local machine
ssh -L 18789:localhost:18789 username@your-instance-ip


```


Then open your browser and navigate to:


```text
http://localhost:18789?token=your-gateway-token


```


Use the gateway token from your configuration file to authenticate.


## **Step 4: Pairing with Telegram**


### **Connect Your Telegram Chat**


After the OpenClaw service is running with Telegram integration enabled, you need to pair your Telegram chat:


1. Open Telegram and find your bot
2. Send /start command to initiate the conversation
3. The bot will respond with a pairing code
4. Enter the pairing code in your OpenClaw terminal:


```text
openclaw pairing approve telegram <pairing-code>


```


### **Interactive Chat**


Once paired, you can start chatting with your AI assistant:


The bot can:


- Answer questions using the configured AI model
- Perform web searches
- Execute code snippets (with proper sandboxing)
- Handle file uploads and processing
- Maintain conversation context


### **Using Telegram Commands**


OpenClaw provides several built-in commands:


Common commands include:


- /start - Initialize the bot
- /help - Show available commands
- /status - Check bot status


## **Conclusion**


You now have a fully functional OpenClaw AI assistant running on[Bitdeer AI Cloud](https://www.bitdeer.ai/en/instance/server?ref=bitdeer.ai) with Telegram integration. This setup provides you with a powerful, customizable AI agent that can assist with various tasks while maintaining security and performance.


The combination of Bitdeer AI Cloud's reliable infrastructure and OpenClaw's flexible framework creates an ideal environment for building and deploying intelligent assistants. As you become more familiar with the platform, you can explore advanced features like:


- Advanced agent skills
- Integration with other channels


Happy building!


* **Security Note:** Apply appropriate access controls for sensitive actions.


## **Additional Resources**


- [Bitdeer AI Cloud Documentation](https://bitdeer.ai/en/docs/center?ref=bitdeer.ai)
- [OpenClaw GitHub Repository](https://github.com/openclaw/openclaw?ref=bitdeer.ai)
