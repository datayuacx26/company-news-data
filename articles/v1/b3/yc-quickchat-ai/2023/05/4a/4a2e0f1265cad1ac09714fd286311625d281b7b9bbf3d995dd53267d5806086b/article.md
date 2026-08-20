---
schema_version: "1.0.0"
document_id: "4a2e0f1265cad1ac09714fd286311625d281b7b9bbf3d995dd53267d5806086b"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/create-ai-chatbot-for-zendesk"
published_at: "2023-05-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:c35f20d258f89e9305df396aa22b0bbaf7b180ec50f0e9f4d9d899f05d4041cf"
---

# Create AI Chat Bot for Zendesk

### What is Quickchat AI?


[Quickchat AI](https://quickchat.ai/) , a platform designed for building fully customized and multilingual AI Agents powered by state-of-art models, now brings its impressive capabilities to **Zendesk Messaging** . With Quickchat AI, you can develop a ChatGPT- or GPT-4-based AI Agent tailored to your[company branding](https://quickchat.ai/post/exploring-personalities-creativity/) and designed with the exact[knowledge](https://quickchat.ai/post/chatbot-knowledge-base-guide/) you need to support customers’ inquiries seamlessly.


Integrating the Quickchat AI Agent with Zendesk Messaging is a powerful customer service tool that enables businesses to interact with their customers by providing instant support, and build customer relationships, providing **automated** responses to customer queries and streamlining your customer support processes.


## How do I start?


#### Set up your Quickchat AI:


1. Go to[https://app.quickchat.ai](https://app.quickchat.ai/) , create an account and start with the Free plan
2. Build your Knowledge Base (what do you want your AI to know?)
3. Test it using the Preview window


#### In your Zendesk Account:


1. (Required) Your Zendesk account must be on the[Professional plan](https://www.zendesk.com/pricing/) or higher.
2. (Required) Your Zendesk account must be using **Zendesk Messaging** ,[read more](https://support.zendesk.com/hc/en-us/articles/4408846454682-About-messaging)
3. Navigate to the **Admin Center** and in the **Apps and integrations** section, go to the **API > Conversations API**
4. **Create API key** button and insert name: quickchat-ai-api-key
5. *App ID* , *Key ID* and *Secret key* should appear. Copy them to a safe place.


‍ *Zendesk: creating Conversations API key* ‍


#### Back to the Quickchat[dashboard](https://app.quickchat.ai/) :


1. Navigate to the **Integrations** tab and click on **Zendesk**
2. In the pop-up window, paste *App ID* , *Key ID* and *Secret key* from the previous step and click on the **Authenticate with Zendesk** button.
3. You will see the *“Your Quickchat account is successfully connected to your Zendesk account”* message and more settings will appear.


*‍Enabling the integration‍*


## How do I get the Quickchat AI Agent to respond in Zendesk?


There are two possible ways of using Quickchat AI with Zendesk


### 1. Make Quickchat always respond to all newly created conversations


#### In the Quickchat[dashboard](https://app.quickchat.ai/) :


1. Navigate to the **Integrations** tab and click on **Zendesk** .
2. In the pop-up window, switch **Respond to all new Zendesk conversations** to **ON** .
3. Switch **Activate Integration** to **ON** .


The integration is now ready! 🎉 Your Quickchat AI Agent will now be the default integration and respond to **ALL** incoming messages in your Zendesk account.


### 2. (Advanced) Make Quickchat Assistant respond only in particular cases rather than always


More advanced users might want to transfer the conversation over to Quickchat AI only under certain conditions. Here is how to achieve that using the Zendesk Answer Bot:


#### In your Quickchat[dashboard](https://app.quickchat.ai/) :


1. Navigate to the **Integrations** tab and click on **Zendesk** .
2. In the pop-up window, switch the **Respond to all new Zendesk conversations** to **OFF** .
3. Copy your generated **Quickchat Tag** (should look something like: quickchatai-bot-asd3f23pk1). It will be used in the next step.
4. Switch the **Activate Integration** to **ON** .


#### In your Zendesk Account:


1. Navigate to the **Admin Center** and in the **Channels** section go to the **Bots and automation > Bots** .
2. Click on the **Create bot** button and give your bot any name you like.
3. Create a new answer or start editing an existing one.
4. In the Flow Builder add a **Transfer to agent** step.
5. Enter bot message that will be diplayed right before the conversation is transferred to Quickchat AI.
6. Add your personal Quickchat Tag (from step 3. earlier) into the Tags section.
7. Save your changes and publish the bot.


Now, when the Zendesk Answer Bot reaches the **Transfer to agent** step with your **Quickchat Tag** tag, the conversation will be transferred to Quickchat AI.


‍


## (Advanced) How do I use the Automated Human Handoff feature?


Quickchat AI is a powerful tool that can help you automate your customer support. However, sometimes it is necessary to transfer the conversation to a Human Agent for further help. Quickchat AI can help you with that too!


1. In your Quickchat[dashboard](https://app.quickchat.ai/) :
2. Activate Human handoff in the **Subscription** tab.
3. Navigate to the **Integrations** tab and click on **Zendesk** .
4. In the pop-up window, switch the **Automated Human handoff** to **ON** .
5. Now, when Quickchat AI **automatically detects** that the conversation should be handed off to a Human Agent, it will add the following tag to the conversation: quickchatai-handoff and stop responding.
6. You are now able to create a[Zendesk trigger](https://support.zendesk.com/hc/en-us/articles/4408886797466-Creating-triggers-for-automatic-ticket-updates-and-notifications) that will be triggered when the quickchatai-handoff tag is added to the conversation. You can use this trigger to[send a notification to your agents or to automatically assign the conversation to a specific agent](https://support.zendesk.com/hc/en-us/articles/4408886797466-Creating-triggers-for-automatic-ticket-updates-and-notifications) .


‍


## Additional settings


Quickchat AI’s diverse range of additional features is designed to offer a genuinely[customized](https://quickchat.ai/post/exploring-personalities-creativity/) and efficient user experience, perfectly catering to your company’s distinctive needs.


Benefit from **adjustable reply length** settings to provide concise or more detailed responses, based on your preferred communication style. Try out the **AI creativity levels** to strike the perfect balance between innovative ideas and practical suggestions, aligning with your company’s tastes and aspirations.


These powerful customization options, paired with Quickchat AI’s core functions, result in a finely-tuned Conversational AI experience that elevates your customer interactions and sets your brand apart from the competition.


‍


## Even more features


If your business requires more custom features, you can contact us atcontact@quickchat.ai to provide you with a custom solution tailored to your needs.


‍


## Congratulations! 🎉


Your Zendesk integration is now set up, enriching your business with a powerful AI Agent geared to enhance your customer service and boost customer satisfaction.


‍
