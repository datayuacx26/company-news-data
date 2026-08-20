---
schema_version: "1.0.0"
document_id: "b4e50c6258cdd6d271e3ccbb81935f94c60cfec3439e7cb90bfb19ec45c0dec7"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/setup-ai-analytics-teams-bot-open-source/"
published_at: "2026-03-12T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:c33114a5fe49360194ef0ff1d125634468aba90207a93844181209afe01566f5"
---

# How to Set Up an AI Analytics Teams Bot with an Open Source Framework

For teams that live in Microsoft 365, the right place for an **analytics agent** is often Microsoft Teams, not another standalone analytics app.


The practical goal is simple: let business users **chat with data** where they already collaborate, while the data team keeps control of context, evaluation, and warehouse access in one **open source** system.


With **nao** , the Microsoft Teams bot uses the same deployed agent as the browser chat interface. That means one agent, one **context engineering** layer, one **dbt** -aware setup, and one operational **data stack** behind both surfaces.


This guide follows the official nao Teams bot documentation and turns it into a setup flow data teams can actually run.


## What the Teams bot does


According to the official nao Teams docs, the bot:


- responds in Teams channels and group chats,
- answers direct messages,
- uses the same agent as the web chat interface,
- returns SQL queries, tables, and visualizations,
- keeps context inside threads,


That makes Teams a distribution channel, not a separate analytics product. Your reliability work stays centralized in the same agent.


## Step 1. Create an Azure Bot


The official docs start in the Azure portal.


Create a new` Azure Bot` resource and fill in:


- bot handle,
- subscription,
- resource group,
- pricing tier,
- app type,
- app ID creation mode,


The docs specifically recommend:


- ` F0` pricing tier for testing,
- ` Single Tenant` for enterprise use,
- ` Create new Microsoft App ID` ,


This gives you the Azure-side identity that Microsoft Teams will use to route bot messages to nao.


## Step 2. Open the Microsoft Teams integration in nao


Inside nao, go to your profile and open` Project > Microsoft Teams` .


Keep that page open while you finish the Azure configuration. You will paste the bot credentials there in the next steps.


This is the handoff point between Microsoft identity setup and the deployed nao agent.


## Step 3. Copy the app credentials into nao


From the Azure Bot resource:


1. Open` Configuration` ,
2. Copy the Microsoft App ID into nao,
3. Click` Manage Password` ,
4. In` Certificates & secrets` , create a new client secret,
5. Copy the secret value into nao as the App Password,
6. Copy the Directory (tenant) ID into nao as the Tenant ID,


Then click` Save` in nao.


The docs note one useful side effect here: nao automatically downloads an` app.zip` file after saving. You will use that package later when installing the bot in Teams.


## Step 4. Configure the messaging endpoint


Next, wire Azure back to your deployed nao instance.


In nao:


1. Copy the Messaging Endpoint URL,


In Azure:


1. Return to the Azure Bot` Configuration` page,
2. Paste the URL into the Messaging endpoint field,
3. Click` Apply` ,


This is the core connection. Teams messages route through Azure, and Azure forwards them to your nao deployment using that endpoint.


## Step 5. Enable the Microsoft Teams channel


In the Azure Bot resource:


1. Open` Channels` ,
2. Click` Microsoft Teams` ,
3. Accept the Terms of Service,
4. Click` Apply` ,


Until this step is done, the bot identity exists but is not enabled for Teams traffic.


## Step 6. Add the required Microsoft Graph permission


The official docs require one application permission:


` User.Read.All`


To add it:


1. Open` App registrations` ,
2. Select your bot app,
3. Go to` API permissions` ,
4. Add a permission for` Microsoft Graph` ,
5. Choose` Application permissions` ,
6. Add` User.Read.All` ,
7. Grant admin consent,


This is the step most likely to require coordination with an IT or Azure admin. It is also the step that tends to slow down enterprise rollouts, so it is worth planning early.


## Step 7. Install the bot in Microsoft Teams


Once the Azure and nao configuration is complete, go to Teams and upload the` app.zip` package downloaded by nao.


The official flow is:


1. Open` Apps` in the Teams sidebar,
2. Click` Manage your apps` ,
3. Choose` Upload an app` ,
4. Choose` Upload a custom app` ,
5. Select` app.zip` ,


After that, you can start talking to the bot in Teams.


## Step 8. Test the core analytics workflow


The docs show the same basic usage pattern as the Slack bot:


text


@nao What were our top 5 products by revenue last month?


The bot then:


1. processes the question with the deployed agent,
2. generates SQL from your context,
3. runs queries against your connected databases,
4. returns a formatted Teams response,
5. shares a browser link to the full conversation,


Follow-up questions should happen in the same thread so the agent keeps conversational context.


That is what makes this a real **chat with data** workflow and not just a search shortcut.


## Step 9. Roll out Teams only after the analytics layer is ready


The Teams setup is mostly infrastructure. The bigger question is whether the underlying agent is ready for real business traffic.


Before broad rollout, make sure your agent already has:


- clear warehouse access,
- trusted definitions,
- stable prompts and rules,
- useful **dbt** context,
- evaluation coverage,


If those pieces are weak, Teams will amplify the problems to more users faster.


That is why the right operating sequence is:


1. build the agent,
2. benchmark the agent,
3. deploy the Teams interface,


If you are still building the base system,[How to Build Your In-House Analytics Agent Fully with Open Source](https://getnao.io/blog/build-open-source-analytics-agent-text-to-sql) is the best starting point. If you need a more formal reliability loop, read[How to Evaluate an Analytics Agent: A Practical Guide with nao test](https://getnao.io/blog/analytics-agent-benchmark-context-stack) .


## When Teams is the right choice


Microsoft Teams is usually the right bot surface when:


- the company runs internal collaboration in Microsoft 365,
- distribution through Teams is easier than introducing a new interface,
- IT and security teams prefer Azure-native identity and permissions,
- the data team wants **agentic analytics** access embedded into existing enterprise workflows,


That makes Teams a strong fit for larger organizations standardizing around Microsoft infrastructure while still wanting an **open source** analytics framework underneath.


## Final takeaway


If you want to set up an **AI analytics Teams bot** with an **open source** framework, the cleanest approach is to use Teams as the interface and let nao handle the analytics engine, context, and evaluation layer behind it.


That gives you one agent across web and Teams, one place to improve reliability, and one controlled path to scale **chat with data** across the company.
