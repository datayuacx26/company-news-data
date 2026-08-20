---
schema_version: "1.0.0"
document_id: "62993c82e06322b634cdbe08866a801480fbaf3d27d9fdd1ae93f04f36cb7d96"
company_key: "yc-retell-ai"
company: "Retell AI"
source_id: "yc-retell-ai-news-import-48ab15cc20a2"
canonical_url: "https://www.retellai.com/blog/meet-retell-chat-agents-build-smarter-web-chats"
published_at: "2025-06-16T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:02.485527+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:d93ddb6e32d55a5554ea30d2f1fe0bff68ae60cdd0d58702ce6e24cf2f942bb5"
---

# Retell AI Chat Agent – Real-Time Web Chat Automation Widget | Retell AI

We’re excited to launch the **Retell Chat Agent** , now live for all users!


You can now bring real-time AI automation to your web chat with an embedded AI chat agent directly to your site using the new **Retell Chat Widget** , powered by the same high precision agent builder behind our voice agents.


## **What's New**


**Standalone Chat Agent** built for live web chat


**Embeddable Chat Widget** to integrate with any site using[public key.](https://docs.retellai.com/accounts/public-keys)


## **Why We Built Chat Agents**


AI voice agents have transformed how businesses handle phone conversations, but it’s not enough. In reality, communication is rarely limited to just calling, with customers often starting interactions by browsing a site, opening chat bubbles all with the expectation of immediate answers.


But most web chat systems are still stuck in the past. They’re rigid, decision-tree bots with limited memory, no real language understanding, and little flexibility when things go off-script.


We built the **Retell Chat Agent** to change that.


Now you can automate the smartest web chat experience for your site using the same dynamic logic, real-time memory, and LLM orchestration behind our voice agents, all through an embeddable widget, no extra tooling required.


## **How To Start**


Chat Agents can be created in two ways: **converting from existing voice agent** and **creating an entirely new agent**


### How To Build a New Chat Agent


Navigate to the Retell Agent Builder


1. Click “Create New Agent”


2. Select your **agent type**


Add nodes and build out prompting or conversation flow just like AI voice agents


Use variables like {{first_name}}, {{email}}, {{booking_time}}, and more to personalize messages based on user data or previous call context. Ensure all AI chat settings are set to your requirements. Test your conversation flow using **Manual Chat** or **AI Simulated Chat.**


See[docs](https://docs.retellai.com/build/create-chat-agent) for a deeper look into building and integrating Retell AI Chat Agent


### How To Create Chat Agent From Voice Agent


Loving the performance of your voice agents? Creating a chat agent with the exact logic, LLM orchestration, and conversation flow is as easy as one click.


1. Go into AI[Voice Agent](https://www.retellai.com/blog/connect-any-ai-voice-agent-to-mcp-with-retell-ai-mcp-node) Builder. Click the three dots (···) button. Click **Convert to Voice Agent** button


### How To Embed Retell AI Chat Widget On Site


The widget is embeddable via a single` <script>` tag and uses the Retell public key system, allowing direct API calls from the frontend with no backend proxy required. If you use WordPress, add this script via header.php or a plugin—just make sure your[WordPress hosting](https://ultahost.com/wordpress-hosting) supports real-time AI chat.


Ensure you have the following information to embed the chat widget: Retell Public Key, Chat Agent ID, Agent Versions.


#### Create Public Key


In Retell AI Dashboard click on **Keys** in the left menu. Click on top right **+ Add Key** and select **Public Key** . Input the name of the key and the site domain you will be embedding the widget on, then **Save** . See our[docs](https://docs.retellai.com/accounts/public-keys) to learn more about Retell public keys.


#### Embed Chat Widget


**Add the following script tag to your HTML** , in the` head` tag:


```text


```


See[docs](https://docs.retellai.com/deploy/chat-widget) for more information on Retell AI Chat widget functionality


## **Coming Next**


- **Bring your own Twilio for Chat Agent.**
- Chat History in History Tab .


[Start building your Retell AI Chat Agent free today!](https://dashboard.retellai.com/) Bring real-time AI conversations to your website in minutes.
