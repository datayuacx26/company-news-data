---
schema_version: "1.0.0"
document_id: "bc50393b3a1d3e4cb0c137b417dbe8814796453ee7673260543913d3c755a5f8"
company_key: "yc-openphone"
company: "Quo (fka OpenPhone)"
source_id: "yc-openphone-news-import-b4955105a2fb"
canonical_url: "https://www.quo.com/blog/personalize-ecommerce-customer-experience/"
published_at: "2026-03-13T17:20:36+00:00"
first_seen_at: "2026-07-23T17:34:43.505198+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:c8e5e46a4b4cbb546b4f2c30cc41752b87709fca3679e6fe85bb1db6662b2160"
---

# How Zazzy Box saves 160 hours per month & delivers quotes faster

Running a jewelry business means juggling a lot of moving parts. Design approvals, supplier coordination, customer follow-ups, invoicing. For most independent jewelers, those moving parts live across notebooks, personal phones, text threads, and memory. It works… until the business starts growing and you become the bottleneck.


This is the reality[Zazzy Box](https://www.quo.com/customers/zazzy-box) founder Asit Shah dealt with when he entered his family’s jewelry business. He thought there had to be a better way. He migrated Zazzy Box’s operations to the digital world, including his business phone.


After Asit switched to Quo, everything changed. Now, they’re able to serve customers at least 2x faster than the industry average. In this article, Asit shares the specific ways his team uses Quo to save time and provide a personalized customer experience at scale.


##


**1. Coach your team with AI-powered conversation analysis**


Before they switched to Quo, Asit was the point of contact between his customers and suppliers for every project. After moving to Quo, he was able to share his business number with suppliers so they can talk to customers directly. That way, they’re faster and can deliver more accurate communication.


In this new communication model, Asit realized their suppliers needed customer service training. They had to manage customer requests without derailing timelines or inflating costs. Scope creep is a common issue in jewelry designs, and it can affect Zazzy Box’s bottom line.


Asit used Quo and Claude to help his suppliers with the right communication strategies. First, he[connected Quo and Claude](https://support.quo.com/core-concepts/integrations/mcp) .


Next, he asked Claude for sentiment analysis, tone analysis, and script generation on call transcripts.


“When Claude read through our conversations, it gave us really quick feedback on how to spot sentiment differences and differences between customer requests and what we’re able to do,” says Asit.


The insights are used to create internal training materials, like Notion documents, snippets, and Loom videos. These materials give staff tools for handling common questions and difficult conversations.


“As a small business, it’s expensive to have our team analyze conversations and come up with suggestions to train our suppliers. This has been a game changer for us,” says Asit.


### **How to connect Quo with Claude to analyze your conversations**


1. Connect Quo and ChatGPT:


- Open ChatGPT and go to **Settings** and **Apps & Connectors** .
- Turn on **Developer mode** under **Advanced settings** .
- Click the **Create** button at the top of the **Apps & Connectors** page.
- Name it Quo.
- Paste this URL:[https://mcp.quo.com/sse](https://mcp.quo.com/sse) .
- **Set Authorization** as **OAuth** .
- Complete the setup and **paste your API key** .


2. Prompt ChatGPT to analyze sentiment, identify common customer questions, and generate response scripts. Here are a few example prompts:


*“Look up all our call transcripts with customers for the past quarter. Based on these transcripts, write 5 response snippets our team can copy and paste when a customer asks for additional design revisions beyond what’s included. The tone should be friendly, firm, and make the customer feel like we’re making an exception — while clearly communicating our standard process.”*


*“Compare the tone and sentiment of our conversations with first-time customers vs. repeat customers. Are there noticeable differences in how we communicate with each group? Where are we strongest, and where do we lose people?”*


*“Based on these transcripts, write a script for when a customer compares our pricing to a cheaper competitor. The response should acknowledge the comparison without being defensive, highlight what’s different about our process, and redirect to value rather than price.”*


3. Store the best scripts in a shared knowledge base (Notion, Google Docs, etc.) for your team to reference.


**Bonus:** You can also ask ChatGPT to create training documentation to onboard new team members:


*“Based on these transcripts, create a training document for new hires. For the 10 most common customer scenarios, provide 2-3 example responses that sound natural. Include brief context on why the customer is asking and what a great response accomplishes.”*


##


**2. Filter spam calls and vet new customers with Sona**


As a fast-growing business in the jewelry industry, Zazzy Box receives a high volume of spam calls to its main business number. It wastes their team’s time and takes them away from focusing on genuine customers who are looking to place an order with them.


Asit uses two Quo features to combat spam calls: **phone menus** and **Sona** . Phone menus act as Asit’s first line of defense against spam calls. They’re a good option to stop robocallers because they can’t get past keypad options. Asit uses Sona as a second line of defense to vet new customers. New callers are directed to a dedicated menu option that connects them with Sona, where they can share their interest in Zazzy Box’s services.


“Sona has been a great buffer to help us understand a new customer’s requirements before we get on a call with them,” says Asit.


### **How to set up a phone menu and Sona to filter spam calls**


1. Go to **Settings** and **Phone numbers** . Select the number to configure.
2. Click **Edit call flow** under the **Call flow** section.
3. In the call flow builder, drag a **Phone menu** call step to your call flow.
4. Edit your phone menu options:


- **1 – For existing customers.** Route these calls to your team.
- **2 – For new customers.** Route these calls to Sona.


5. Set up Sona with a custom script to handle new customers.


- Ask new callers for their name, phone number, and reason for their call.
- If they’re interested in your products and services, instruct Sona to ask them which products they’re interested in and their intended timeline.


##


**3. Use contact notes and custom properties to personalize every interaction**


Managing customer relationships as a small team can be challenging. Multiple team members interact with the same customer, making it hard to keep track of customer preferences, behaviors, and purchase histories. The problem compounds when you have many customers with similar names, as Zazzy Box does.


Asit addressed this problem by using Quo as his CRM. The Zazzy Box team stores customer information using custom properties and contact notes in Quo instead of a separate platform. They include behavioral information about customers in their contact notes so any team member can quickly get up to speed before the next interaction. They also plan to add spend profile information to custom properties so staff can quickly see a customer’s lifetime value with the business.


### **How to set up custom properties and contact notes in Quo**


1. Open a contact in Quo.
2. Add relevant information to **Custom properties** :


- **Multi-select tags:** Categorize contacts with labels like “Customer,” “Prospect,” or “VIP.”
- **Text fields:** Add custom-named text properties for any information.
- **Date fields:** Track follow-up dates, contract renewals, or important milestones.
- **URL fields:** Store website links, social media profiles, or portfolio URLs.
- **Address fields:** Record business addresses, shipping locations, or meeting venues.
- **Phone and email:** Add additional contact methods beyond the primary number.


3. Use **Contact notes** to document behavioral insights and preferences that’ll help your team tailor their approach.


*An example Zazzy Box contact profile*


##


**Start scaling your customer relationships with Quo today**


With the ability to coordinate his team more effectively, Asit can turn around quotes to his customers faster than the industry average. Zazzy Box delivers comprehensive quotes to new customers in less than 24 hours. Other jewelers take 48 hours to a week and offer generic quotes to customers. His team also **saves 160 hours per month** .


If Asit wasn’t using Quo, he would need a dedicated receptionist to manage customer communications, analyze call transcripts, and manage project communication.


Go even further with Quo’s collaboration features — check out our[team collaboration support guide](https://support.quo.com/core-concepts/team-collaboration/guides/collaborate-better-in-quo) .
