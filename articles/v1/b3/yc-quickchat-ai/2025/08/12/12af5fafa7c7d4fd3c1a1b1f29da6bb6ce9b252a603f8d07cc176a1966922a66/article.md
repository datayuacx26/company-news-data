---
schema_version: "1.0.0"
document_id: "12af5fafa7c7d4fd3c1a1b1f29da6bb6ce9b252a603f8d07cc176a1966922a66"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/slack-notification-ai-action"
published_at: "2025-08-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:3a24ea3343e2ab15c7aa6e270ecfd6eceff4888d91dd07888fe24dbbe49a6429"
---

# Send Slack notifications with AI Actions

Quickchat AI’s Custom Actions allow you to extend your AI agent’s capabilities by connecting it to external tools and services via API.


This means your AI can do more than just answer questions – it can perform custom tasks, collect data, and even notify your team.


In this tutorial, we’ll walk you through setting up a “Human Handoff Notification” Action that pings your team on Slack whenever a user requests human assistance from your Quickchat AI agent.


Let’s get started!


## Step 1: Navigate to AI Actions in Quickchat AI


1. Log in to your Quickchat AI dashboard.
2. On the left-hand sidebar, click on **“Actions & MCPs”** .


- You’ll see a section for “Quickchat AI Actions” (like “Human Handoff” and “Smart Data Gathering”) and “Custom Actions” below.


## Step 2: Add a New API Action


1. In the “Custom Actions” section, click the **”+” button** to add a new Action.
2. A small menu will appear. Select **“API Action”** .


- This will open a new “Add Action” wizard with four steps.


## Step 3: Define AI Action Details


This is where you give your Action a clear name and explain its purpose to the AI.


1. **Name:** Type a clear, descriptive name that helps the AI understand when to use this action.


- For this example, type:` Slack - Human Handoff Notification`


2. **Description:** Provide a detailed explanation of what this action does, when the AI should use it, and what data it retrieves or triggers. This is crucial for the AI’s understanding.


- For this example, type:` Use this after the Human Handoff is triggered to notify the team on Slack.`


3. Click **“Continue”** .


## Step 4: Configure API Connection (Slack Webhook)


Now, we need to tell Quickchat AI where to send the information.


We’ll use Slack’s Incoming Webhooks for this.


1. **Action Type:** From the dropdown menu, select **“POST”** .
2. **Get your Slack Webhook URL:**


- Open a new tab and go to` api.slack.com` .
- Click **“Create an App”** .
- Select **“From scratch”** .
- **App Name:** Enter a name for your Slack app (e.g.,` Quickchat AI Human Handoff` ).
- **Pick a workspace to develop your app in:** Select your desired Slack workspace from the dropdown.
- Click **“Create App”** .
- On the left sidebar, under “Features,” click **“Incoming Webhooks”** .
- Toggle **“Activate Incoming Webhooks”** to **“On”** .
- Scroll down to the “Webhook URLs for Your Workspace” section and click **“Add New Webhook to Workspace”** .
- **Where should Quickchat AI Human Handoff post?:** Search for and select the Slack channel where you want these notifications to appear (e.g.,` #handoff-notifier-test` ).
- Click **“Allow”** .
- Once authorized, you’ll see a new Webhook URL listed. Click **“Copy”** next to it (2:45).


3. **Return to Quickchat AI:** Paste the copied Slack Webhook URL into the **“Action endpoint URL”** field.
4. **Add Headers:** Under “Headers,” click on the **“Key-Values”** tab.


- Click **“Add Key”** and type:` content-type` .
- Click **“Add Value”** and type:` application/json` .


5. Click **“Continue”** .


## Step 5: Define AI Parameters


This step defines what information the AI will send in the message.


1. Click **“Add parameter”** .
2. **Name:** Type` text` .


- This is the standard parameter name for the message content when sending to Slack webhooks.


3. **Type:** Ensure it’s set to **“Text”** (default).
4. **Default:** You can leave this empty or add a default value if desired.
5. **Location:** Ensure it’s set to **“Body”** (default).
6. **Description:** Explain what information should be included in the` text` parameter. This helps the AI formulate the correct message.


- Type:` Content of the message sent to Slack when a Human Handoff is triggered. Start the message with \[HANDOFF REQUEST\], include a brief summary of the issue, and add the app link https://app.quickchat.ai/ at the end for easy access` .


7. Click **“Continue”** .


## Step 6: Test Your Action


It’s always a good idea to test your API connection to ensure everything is set up correctly.


1. In the “Test response” section, you’ll see your` text` parameter.
2. In the “Value” field next to` text` , type a test message.
3. Click **“Test request”** .
4. You should see **“200 Ok”** appear under the JSON response, indicating a successful request.
5. **Verify in Slack:** Switch to your Slack workspace and check the channel you selected. You should see your test message posted by the new Slack app you created.


## Step 7: Finalize the Action


1. Once your test is successful and you’ve confirmed the message in Slack, click **“Done”** (4:13).


Your “Slack - Human Handoff Notification” custom action is now active!


Your Quickchat AI agent will use this action to notify your team on Slack whenever a human handoff is required, providing the necessary context for your agents to take over.


---


Feel free to experiment with different custom actions and integrations to further enhance your Quickchat AI agent’s capabilities! If you encounter any issues, remember that Quickchat AI support is always there to help.


## Related guides


- [Connect your AI Agent to Google Sheets](https://quickchat.ai/post/connect-ai-agent-to-google-sheets) : log leads, unanswered questions, and demo requests to a spreadsheet
- [Connect your AI Agent to HubSpot: create contacts, deals, and tickets](https://quickchat.ai/post/connect-ai-agent-to-hubspot)
- [Connect an AI Agent to Jira tickets](https://quickchat.ai/post/search-jira-tickets-in-ai-conversation)
- [Connect Cal.com to Your AI Agent in 5 Minutes](https://quickchat.ai/post/connect-calcom-to-your-ai-agent)
- [Build an AI Telegram bot to manage your group](https://quickchat.ai/post/connect-ai-agent-to-telegram-bot-api) : announce, pin, and moderate with AI Actions
