---
schema_version: "1.0.0"
document_id: "59a6d723a701b097c270a6f09012ac7301dedb7017fb97cf4e739e0feaea1fbb"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/create-ai-bot-for-whatsapp"
published_at: "2022-12-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:0285863e25a26e91129b58eadd3109ea68725ded0b2340de9382e08e88d256a3"
---

# Create AI Chat bot for WhatsApp

This guide shows how to integrate your **Quickchat AI Agent** with **WhatsApp** . The process is divided into **2 simple steps** :


- **Step 0** - Prerequisites
- **Step 1** - Connect WhatsApp


If you have problems with integration or need help, go toTroubleshooting & FAQs section or[contact us](https://quickchat.ai/contact) .


## Watch the Video


## Step 0 - Prerequisites


### A. Business Phone Number


Decide **which phone number** you’ll use for WhatsApp. You can[read more in WhatsApp documentation](https://developers.facebook.com/docs/whatsapp/cloud-api/phone-numbers) or follow the FAQs below:


**Which number can be used in WhatsApp Cloud API?**


- You need to use a **dedicated business phone number** that can receive **SMS or voice calls** for verification.
- The number must be valid, owned by you, and have a country and area code.


**Can I use a number that is already used in WhatsApp Messenger?**


- **No** . You can continue using this number for calls and texts, but **not in WhatsApp Messenger** .
- If the number is already used in WhatsApp Messenger,[delete it first](https://developers.facebook.com/docs/whatsapp/cloud-api/get-started/migrate-existing-whatsapp-number-to-a-business-account) . Don’t forget to[back up your WhatsApp chat history](https://faq.whatsapp.com/481135090640375) .
- Banned numbers must be unbanned before registering.


**Can I use a number that is already used by WhatsApp Business App?**


- **Yes** . If your number is currently used in the **WhatsApp Business App** , you may be able to onboard it to the Cloud API without deleting the app account.[Read more about Coexistence onboarding](https://developers.facebook.com/docs/whatsapp/embedded-signup/custom-flows/onboarding-business-app-users/) in Meta documentation.


**Can I use a new phone number?**


- **Yes** , if you prefer not to use your current number, get a **new number** (physical SIM or virtual number). You can buy a virtual phone number from services such as[Hushed](https://hushed.com/) .


### B. Business Information


Have these details ready (you’ll enter them during the Meta integration flow):


- Business name
- Business website
- Business category
- Business description


> Even if some fields seem optional, **fill them accurately** . Missing or inaccurate info can delay approval or cause account restrictions.


## Step 1 - Connect WhatsApp


Since our latest update, connecting takes a **single click** . There is no separate second step anymore: Meta’s own Embedded Signup wizard walks you through creating a new WhatsApp Business Account and number, or picking one you already have, and Quickchat AI finishes the rest.


Open the[Quickchat AI App](https://app.quickchat.ai/) → **External Apps** → **WhatsApp** → click the **Connect WhatsApp** button and follow the Meta pop-up:


1. Log in to your Meta (Facebook) account.
2. Review the permissions you’ll share with Quickchat AI (WhatsApp Business account access and Business portfolio access) and continue.
3. Provide accurate business details (Business Portfolio, Business Name, Business Website, Country, etc.).
4. Create or select your WhatsApp Business Account.
5. Choose a WhatsApp Business display name and category.
6. Add your **business phone number** and verify it via SMS/voice as prompted.
7. Click **Confirm** to give Quickchat AI the required access.
8. When Meta confirms the setup is complete, click **Finish** . The pop-up shows a Quickchat AI confirmation that your WhatsApp account has been connected and closes automatically.


Back in the Quickchat AI App, the WhatsApp panel shows **Connecting your WhatsApp account** and updates to **Connected** within about 30 seconds. If it doesn’t, click **Refresh Status** in the panel. Otherwise, please revisit **Prerequisites** above to make sure you correctly provided your business phone number and information.


**That’s it, you’re live!** 🎉 Customers can now message your WhatsApp number, and your AI Agent will respond automatically.


> As a final verification of your integration, send a text message to the WhatsApp number you just connected. You should get a reply from your AI Agent and the conversation should appear in the[Quickchat AI App](https://app.quickchat.ai/) Inbox. If you leave any of Meta’s setup steps unfinished, Quickchat AI also emails the account owner the remaining steps as a reminder.


## Troubleshooting & FAQs


**How WhatsApp Business API Pricing Works?**


- Replies to user-initiated ( **inbound** ) customer inquiries are treated by WhatsApp as **Service messages** . These messages enable businesses to manage incoming customer inquiries, whether that is via a human agent or a conversational AI-powered bot.
- When users message a business, this opens a **24-hour customer service window** during which businesses can respond with service messages, **at no charge** . This window **resets with each user message** .
- **Outside 24 hours** , you must use **approved Message Templates** (Marketing, Utility, Authentication, etc.) and pay per-message according to[WhatsApp Business Platform Message Rates](https://business.whatsapp.com/products/platform-pricing) .
- Conversations are **categorized and billed** by Meta. Ensure your WhatsApp Business Account has a **valid payment method** for business-initiated/template messages.


**The Connect WhatsApp button does not work. I don’t see the Meta pop-up.**


- If your browser blocks the pop-up, the app shows a message asking you to allow pop-ups. Disable **pop-up and tracker blockers** in your browser, refresh the website and try again. If it doesn’t help, try a different browser.


**Number verification failed.**


- Ensure the number can receive SMS/voice calls and isn’t blocked by an active registration path that Meta doesn’t allow to coexist. If the number is used in the **Business App** , try[onboarding via Coexistence](https://developers.facebook.com/docs/whatsapp/embedded-signup/custom-flows/onboarding-business-app-users/) .


**Meta says my setup isn’t finished, or the panel shows a warning.**


- Click **Reconnect WhatsApp** in the panel. This resolves most cases.
- If it doesn’t, your number may be connected to another provider (Meta allows only one partner per WhatsApp account at a time). Follow the on-screen instructions to add Quickchat AI as a partner in Meta Business settings, then click **Refresh Status** .


**I connected the wrong account.**


- Click **Reconnect** in the WhatsApp panel and repeat the flow using the correct Meta Business account and WhatsApp Account.


**Messages aren’t appearing in the Quickchat AI Inbox.**


- Confirm the pop-up showed the final confirmation that your WhatsApp account was connected.
- Double-check you linked the intended **WhatsApp Account** and the correct **AI Agent** is selected.
- If you’re sending messages **after 24 hours** without a user message, use **approved templates** and ensure a **payment method** is set in[WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager) .


**My AI’s WhatsApp replies are failing to send.**


- Quickchat AI shows a **“WhatsApp replies are failing”** alert on the WhatsApp panel and a notification on your dashboard. Common fixes: complete **display-name approval** in[WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager) ; if the connection **expired or was revoked** , click **Reconnect** ; if Meta **restricted your number** over quality or policy, check its status in WhatsApp Manager. The alert clears once a reply sends successfully again.


**Template or business-initiated messages fail.**


- Verify your **Message Templates** are **approved** and your **payment method** is active in[WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager) .
- New senders start with **messaging limits** that scale based on quality and verification. If you hit limits for business-initiated messaging, review[Meta’s Messaging Limits guidance](https://developers.facebook.com/docs/whatsapp/messaging-limits/) .
- Check **messaging limits** and **quality rating** in[WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager) if delivery is throttled.


**How to create my own WhatsApp link and “Chat on WhatsApp” button?**


- Follow[this WhatsApp tutorial](https://faq.whatsapp.com/5913398998672934) .


**How to create my own WhatsApp QR code?**


- Visit[WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager) → **Phone numbers** → select your business phone number → **Message links** → **Create Message Link** .
- Alternatively you can find it in your WhatsApp Business mobile app. Follow[this WhatsApp tutorial](https://faq.whatsapp.com/888878128766436) .


## Need Help?


Chat with Quickchat’s Support AI on WhatsApp:
📱 **+1 302-405-9992** :[Start a chat](https://wa.me/message/LHARULDFIUEZL1)


Or scan the QR code:
