---
schema_version: "1.0.0"
document_id: "dc174bb23a387762cc8d14c140a380833424a8830b55ae2819c8460bc59979e1"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/whatsapp-cloud-api/"
published_at: "2025-01-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:7062708b937e22c7684c4042b411977996453a1a5fb656f7c8644643cd9a0265"
---

# WhatsApp Cloud API: What It Is, How It Compares, and How to Get Started

Managing large-scale conversations can be a challenge—especially for growing businesses.


To meet this demand, Meta launched the WhatsApp Cloud API in May 2022. This API lets businesses quickly connect with customers, offering a secure and scalable way to chat — no matter where they are.


While the Cloud API is easy to set up with no hosting fees, it does limit customization and control.


That’s where Plivo’s[WhatsApp Business API](https://www.plivo.com/whatsapp/) comes in. Plivo offers more flexibility and control during setup, which can be beneficial for businesses in need of a more tailored solution.


In this guide, we’ll cover everything from setting up the Cloud API to real-world use cases, and also explore how Plivo’s API can offer more customization for businesses that need it.


## WhatsApp Cloud API 101


Before launching the WhatsApp Cloud API, Meta offered three versions of WhatsApp:


- The standard WhatsApp app for personal use.
- WhatsApp Business for small- to mid-sized businesses.
- WhatsApp Business API for companies that needed advanced features.


So, what makes WhatsApp Cloud API different from all of these versions?


Let’s learn more about it.


### What is WhatsApp Cloud API?


The WhatsApp Cloud API is a version of the WhatsApp Business Platform that runs on Meta’s cloud servers.


Unlike the older version, which required hosting on private servers or through a[business solutions provider (BSP)](https://www.plivo.com/blog/whatsapp-business-solution-providers/) , the Cloud API removed the need for businesses to manage their own servers.


#### Why did Meta create the Cloud API?


The older on-premises API had its drawbacks. Set up took time, updates were slow to reach businesses, and server maintenance was expensive.


#### How does WhatsApp Cloud API work?


Here’s how the WhatsApp Cloud API works:


- **Cloud-based architecture:** Hosted on Meta’s servers, it provides a reliable and scalable infrastructure for messaging.
- **Messaging capabilities:** Businesses can exchange text, images, videos, and documents programmatically using API endpoints.
- **Automation and bots:** It supports automated workflows, such as sending appointment reminders or responding to frequently asked questions.
- **Integration-friendly:** The API connects seamlessly with other tools, streamlining customer communication processes.
- **Multi-agent support:** Teams can manage conversations collectively, ensuring faster response times and better customer service.


## What’s the difference between the WhatsApp on-premises API and Whatsapp Cloud API?


Understanding the differences between the WhatsApp on-premises API and the WhatsApp Cloud API is key to choosing the right solution for your business.


Let’s explore how they compare.


**Feature** **WhatsApp on-premises API** **WhatsApp Cloud API**


**Access** Requires registration with a BSP Free access via Meta


**Hosting** Businesses or BSPs need to host the API on their own servers Hosted by Meta, eliminating server management


**Costs** BSPs cover server setup and maintenance costs, along with per-message charges Businesses only pay per message or conversation


**API protocol** REST API Graph API


**Maintenance** BSPs manage software updates and new feature integrations Meta handles updates and releases new features


**Server location** Based on the business’s infrastructure Located in North America, managed by Meta


**Uptime** Dependent on the BSP’s infrastructure reliability Meta aims for 99.9% uptime


**Sticker pack management** Supported for managing custom sticker packs Not supported


### WhatsApp business Cloud API features


The WhatsApp Cloud API comes with features that allow businesses to easily connect WhatsApp with their existing tools and workflows.


Here are its main offerings:


#### Seamless integration with Meta


Businesses can connect their systems to WhatsApp through Meta’s cloud platform. This ensures smooth and dependable communication with customers.


#### Versatile messaging options


The WhatsApp Cloud API supports text, media such as images, videos, audio, GIFs, and files, as well as contact cards and location sharing. It also facilitates interactive messages like list messages, reply buttons, and single or multi-product messages.


Additionally, businesses can use message templates for both text and media, incorporating interactive options for customer engagement.


#### Data access across platforms


The Graph API allows businesses to access and send data easily across different Meta platforms, including WhatsApp, Facebook, and Instagram.


It uses a single point of connection, so businesses don’t need to connect separately to each platform. This makes it simpler for businesses to manage data, whether it's about users, posts, or events, all from one place.


#### Global accessibility with no setup fees


The WhatsApp Cloud API is hosted in North America, but it allows businesses worldwide to connect with customers. This gives them access to WhatsApp’s large user base.


Meta offers the Cloud API for free, so businesses don't have to pay setup or access costs. However, businesses will still pay for the messages they send.


#### Easy-to-use API documentation


Meta offers straightforward API documentation, making it easier for developers to connect their systems to WhatsApp. This helps businesses build customized solutions and drive innovation.


#### Scalable and secure communication


The WhatsApp Cloud API enables businesses to scale their communications as they grow. It meets Meta’s strict security and privacy standards, ensuring both business and customer conversations remain secure.


## How to get WhatsApp Cloud API


Setting up the WhatsApp Cloud API can vary depending on the method you choose. Here’s a quick comparison between using a BSP like Plivo and going through Meta directly.


**Feature** **Embedded signup through a BSP** **Meta for Developers**


**Method** Register through a BSP platform Create an app on Meta's platform


**Developer resources** No resources provided Access to comprehensive developer tools


**Steps** Connect to Facebook Business Manager, set up WABA, and verify phone number Set up development tools, send test message, configure webhook, set up WABA, and verify phone number


**Onboarding time** Less than 5 minutes Several hours for completion


### Using an embedded signup


Signing up through a BSP enables you to complete the process in just a few steps. The signup flow is simple and stays entirely within the site.


Here’s how to do it with Plivo:


#### Step 1: Create a WABA in the Plivo console


To set up your WABA in Plivo, register a phone number that can receive an OTP via text or call. You'll use this number to message customers on WhatsApp.


You can either rent a number from Plivo or use your own. If you’re moving from another provider, you can transfer your number to Plivo.


Be sure to check[Meta's guidelines](https://developers.facebook.com/docs/whatsapp/cloud-api/phone-numbers) and Plivo’s requirements for registration and migration.


#### Step 2: Customize your WhatsApp profile


Pick a display name for your WhatsApp account and follow the setup guidelines. Then, grant Plivo the necessary permissions to act as your WhatsApp solution provider.


#### Step 3: Verify successful onboarding


To confirm your setup is successful, check your Meta Business Account. First, choose your Meta account, then go to Business Settings and click on WhatsApp Accounts.


In the Partners tab, Plivo should appear as your partner.


Then, go to the Settings tab, where you'll find 'PLIVO INC' listed as a payment option.


Once you open "WhatsApp Manager," your number should show as connected.


#### Step 4: Register WhatsApp templates


Businesses must use[WhatsApp messages templates](https://www.plivo.com/blog/whatsapp-message-templates/) approved by Meta. You can create new templates in WhatsApp Manager.


Meta segments templates into utility, marketing, or authentication categories, based on the message content. The category impacts both the conversation type and the cost.


In the[Plivo console](https://developers.facebook.com/docs/whatsapp/embedded-signup/#the-new-embedded-signup-flow) , you can find template information. Navigate to Messaging > WhatsApp Business Account > Templates to sync and view details from Meta.


#### Step 5: Manage your business phone number


Go to WhatsApp > WhatsApp Business Account to manage business phone numbers in Plivo.


Select your account and click on 'Configurations.' To add a phone number, use the ‘+Add Phone Number’ button found under account details.


After adding, you can also unlink numbers if needed.


This section also shows the connection status (whether it’s connected, pending, or disconnected), the name status (which reflects Meta’s verification), and the quality rating, based on how recipients respond to your messages.


### Using Meta


Using the Cloud API through Meta requires development skills, such as making API calls to send and receive messages and setting up a webhook endpoint to handle message notifications.


You’ll also need to switch between Meta for Developers, your webhook server (like Glitch), and the WhatsApp application to test everything.


To learn more about these steps, refer to[Meta’s documentation](https://developers.facebook.com/docs/whatsapp/cloud-api/get-started/) .


***Note:*** *Meta prohibits independent software vendors from developing the WhatsApp Cloud API for clients. Non-compliance with*[WhatsApp's Commerce Policy](https://business.whatsapp.com/policy#policies_for_whatsapp_commerce_features) *may lead to suspension or removal of API access.*


## WhatsApp Cloud API pricing


Businesses can access the WhatsApp Cloud API for free, but they pay based on their conversations with users.


Conversations on the WhatsApp Cloud API fall into two categories: user-initiated and business-initiated. A **user-initiated conversation** occurs when a user sends a message to a business, and the business responds within 24 hours.


On the other hand, a **business-initiated conversation** happens when the business messages a user after the 24-hour window following the user's initial message.


WhatsApp offers the first 1,000 conversations each month for free. Once businesses exceed this limit, they must add a credit card to their account to continue using the service.


Even if a WABA has multiple phone numbers, the 1,000 free conversation limit still applies, and it resets monthly. In addition, businesses won’t incur charges when users contact them through the business’s call to action (CTA) button.


## Pros of using the Whatsapp Cloud API


The WhatsApp Cloud API offers several advantages for businesses looking to integrate WhatsApp messaging into their customer service and communication workflows.


These advantages include:


### Faster approval process


Previously, businesses had to go through BSPs and wait for approval before gaining access to the WhatsApp Business API. With the new approach, businesses can now directly access the API, removing the middleman.


### Instant updates


Direct access to the WhatsApp Business API ensures that businesses immediately receive updates, including important security fixes.


### Reduced costs


Traditionally, BSPs charged businesses for API access.


Now, with the WhatsApp Cloud API, Meta has eliminated setup charges, meaning businesses of all sizes can use the API at no cost.


## Cons of using the Whatsapp Cloud API


While the WhatsApp Cloud API offers many benefits, there are also some potential drawbacks to consider:


### Customization and control


The WhatsApp Cloud API is a ready-to-use solution managed by Meta. It makes setup and maintenance easy, but businesses can’t change its main features or underlying infrastructure. However, it allows for flexible integrations and workflow setups within its standard limits. For companies with complex needs or very specific requirements, this lack of control might be a drawback compared to on-premises options.


### Support and service level agreement (SLAs)


Meta’s Cloud API support is general, whereas the on-premises API often includes specific SLAs and disaster recovery managed by BSPs. Businesses with high-stakes communication needs may find it challenging to operate without dedicated support. Downtimes or delays in issue resolution can impact critical service levels.


Plivo offers strong support with clear SLAs and disaster recovery measures. This dedicated service helps businesses maintain consistent, reliable communication when it matters most.


### Feature parity and updates


The Cloud API may lack certain advanced features or update flexibility found in the on-premises API. Businesses using the Cloud API can face restrictions in accessing new tools or controlling update timing, limiting how they manage feature rollouts.


Plivo gives businesses more options, with multiple engagement channels like SMS, WhatsApp, and voice. Its additional features, such as marketing automation, provide flexibility beyond what the Cloud API offers.


For instance, Plivo allows businesses to provide customer support via a no-code AI-powered[WhatsApp chatbot](https://www.plivo.com/cx/service/channels/whatsapp/) . This bot can handle routine customer inquiries, such as order tracking or store hours, without needing an agent. It reduces the workload on human agents and improves response speed.


Plivo also offers data-led routing, which uses customer data to direct queries to the right agents.


For example, if a returning customer has a technical issue, the system can route their inquiry to a dedicated technical support agent. This ensures the customer gets the right help faster and more efficiently.


### Latency and performance


Meta’s North American data hosting can cause delays for international users, as requests must travel across continents. For global businesses, this lag can impact response times, making interactions feel slow and lowering user satisfaction.


Plivo’s network, with connectivity in over 220 countries and territories, routes requests closer to users to reduce latency. This setup helps businesses offer faster, more responsive service worldwide.


## Automate your WhatsApp communication with Plivo


The WhatsApp Cloud API is fully managed by Meta, making it easier to set up and scale, but it gives you less control over infrastructure.


If you want something quick and easy, go for the Cloud API. If you need more customization and control, the Business API is the way to go.


Plivo offers a reliable and easy-to-use API platform that consolidates all your communication channels under one bill, contract, and point of contact. With competitive pricing and customer support ranked 99/100 for satisfaction on G2, you can count on us for top-tier service.


Plivo’s WhatsApp Business API makes managing customer interactions at scale effortless. Rich messaging and advanced automation tools not only improve customer satisfaction but also make your communication processes more efficient.


Ready to see how Plivo's WhatsApp Business API works for your business?[Book a demo](https://console.plivo.com/accounts/request-trial/?_gl=1*r33b69*_gcl_au*NTU5MjMwMTYyLjE3MzIxNjUwOTI.*_ga*MTk5NTc5NjUzMy4xNzMyMTY1MDkz*_ga_YLW0ZWN2T7*MTczMjE3MDgzOC4yLjEuMTczMjE3MzIzMi4yNC4wLjA.) today!
