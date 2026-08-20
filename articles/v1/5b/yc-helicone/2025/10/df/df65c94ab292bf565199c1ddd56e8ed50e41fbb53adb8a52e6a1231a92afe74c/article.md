---
schema_version: "1.0.0"
document_id: "df65c94ab292bf565199c1ddd56e8ed50e41fbb53adb8a52e6a1231a92afe74c"
company_key: "yc-helicone"
company: "Helicone"
source_id: "yc-helicone-news-import-836d1e549638"
canonical_url: "https://www.helicone.ai/blog/n8n-helicone-node"
published_at: "2025-10-24T00:00:00+00:00"
first_seen_at: "2026-07-21T22:44:44.297475+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:d2079882927a9e3e15a14271871dba4963dc351abb798f836c5f3abc368e692a"
---

# How to Use Helicone in Your n8n Workflows

# How to Use Helicone in Your n8n Workflows


October 24, 2025


· 2 minute read


Juliette Chevalier


· October 24, 2025


# How to Use Helicone in Your n8n Workflows


Want to monitor your LLM calls in n8n, while taking advantage of Helicone's AI Gateway? Here's how to set up Helicone's community node in under 5 minutes.


## Table of Contents


- What You're Building
- Prerequisites
- Step 1: Self-Host n8n with Docker
- Step 2: Install the Helicone Community Node
- Step 3: Configure Helicone Credentials
- Step 4: Build Your First Workflow
- Step 5: Verify in Helicone Dashboard
- Optional: Add Extra Observability Features
- What's Next?
- Troubleshooting


## What You're Building


By the end of this tutorial, you'll have:


- A self-hosted n8n instance running locally
- The Helicone n8n node installed
- A working workflow that logs all LLM interactions to your Helicone dashboard


## Prerequisites


- Docker installed on your machine
- A Helicone API key ([get one here](https://helicone.ai/api-keys) )


## Step 1: Self-Host n8n with Docker


The fastest way to get n8n running is with Docker. This gives you a public URL to work with webhooks and integrations.


Make sure you have Docker installed and running on your machine.


```text
docker --version


```


If you don't have Docker installed, you can install it[here](https://docs.docker.com/get-docker/) .


Then, create a new directory for your n8n instance and run the following command to start the container:


```text
mkdir   self-hosted-n8n
cd   self-hosted-n8n
docker volume create n8n_data


docker run -it -- rm   \
--name n8n \
-p 5678:5678 \
-e GENERIC_TIMEZONE= "America/New_York"   \
-e TZ= "America/New_York"   \
-e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS= true   \
-e N8N_RUNNERS_ENABLED= true   \
-v n8n_data:/home/node/.n8n \
docker.n8n.io/n8nio/n8n


```


Open` http://localhost:5678` and sign up for your new account.


## Step 2: Install the Helicone Community Node


From the n8n interface:


1. Click the **user menu** (bottom left corner)
2. Select **Settings**
3. Go to **Community Nodes** (last on the left sidebar)
4. Click **Install a community node**
5. Enter the package name:` n8n-nodes-helicone` and click on the checkbox below it
6. Click **Install**


Wait ~30 seconds for the installation to complete. The node will appear automatically in your nodes panel.


## Step 3: Configure Helicone Credentials


Before using the node, add your Helicone API key:


1. Click back on **Settings** and select the **Credentials** tab


1. Click **Add Credential**
2. Search for "Helicone" and select **Helicone LLM Observability**


1. Get your API key from[your Helicone dashboard](https://helicone.ai/api-keys) and enter it in the **API Key** field
2. Click **Save**


## Step 4: Build Your First Workflow


Now let's create a simple workflow that asks an LLM a question and logs it to Helicone.


1. Click **Create Workflow**
2. Select the "+" icon and search for "Helicone" in the nodes panel (you should see the **Helicone Chat Model** node) and click "Add to workflow"


1. On the bottom left Chat Input panel, ask a question: "What is the capital of Panama?"
2. See the response appear in the output panel with your input variables populated on the right side panel


## Step 5: Verify in Helicone Dashboard


Open your[Helicone dashboard](https://helicone.ai/dashboard) and head over to the "Requests" tab. You'll see:


- The request logged with model` gpt-4o-mini`
- Token usage and costs
- Response time metrics
- The full prompt and completion


**Congratulations!** You have now created your first workflow that logs to Helicone. Make sure to set it as "Active" in your dashboard.


## Optional: Add Extra Observability Features


The Helicone n8n node already comes with observability included, but you can add extra features to your workflows to get more insights on your[Helicone dashboard](https://helicone.ai/dashboard) . For example:


### Custom Properties


Tag requests for filtering and analysis:


```text
{
"environment"  :    "production"  ,
"user_id"  :    "user_123"  ,
"feature"  :    "chat"
}


```


### Session Tracking


Group related requests together:


- **Session ID** :` chat_session_456`
- **Session Name** :` Customer Support Chat`
- **Session Path** :` support/chat`


### Response Caching


Enable caching to reduce costs and improve latency:


- **Enable Caching** :` true`
- **Cache TTL** :` 3600` (1 hour in seconds)


## What's Next?


Now that you have Helicone monitoring set up in n8n:


1. **LLM workflows** - Chain multiple LLM calls and see the full trace
2. **Monitor costs** - Track spending across different models and use cases
3. **Debug faster** - Inspect failed requests with full context
4. **Optimize performance** - Identify slow requests and cache common patterns


## Troubleshooting


**Model not found?** Check[helicone.ai/models](https://helicone.ai/models) for the exact model name. Different providers use different naming conventions.


**Need help?** Join our[Discord community](https://discord.gg/S7AvKzUx) where engineers share tips and troubleshoot together.


---


**Want to dive deeper?** Check out the[Helicone n8n node on GitHub](https://github.com/juliettech13/n8n-nodes-helicone) or explore the[full Helicone documentation](https://docs.helicone.ai/) .
