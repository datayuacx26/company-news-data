---
schema_version: "1.0.0"
document_id: "0c9b668e15d49581955fe99c3ab1808f3264d3ad6ef770adf1486925d815aa56"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-a-slack-bot"
published_at: "2026-05-10T01:08:26+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:07.653203+00:00"
content_hash: "sha256:b1e2113284b7be49770447130b634cb0d792067b22a186abd64dab3c9798cbf9"
---

# How to Build a Slack Bot With AI (No AWS Lambda Required)

## Building a Slack Bot With Blink


[Blink](https://blink.new/) gives you the HTTP endpoint, database, and credential storage in a single platform. No Vercel config. No Supabase setup. No Lambda.


Here's the complete flow:


1


#### Create your Slack app


Go to[api.slack.com/apps](https://api.slack.com/apps) and click "Create New App." Choose "From scratch," name your bot, and select your workspace.


In the app settings, navigate to **OAuth & Permissions** and add these bot token scopes:


- ` chat:write` — to post messages
- ` commands` — for slash commands
- ` channels:history` — to read channel messages (if needed)
- ` users:read` — to look up user info


Install the app to your workspace and copy the **Bot User OAuth Token** (starts with` xoxb-` ).


Under **Basic Information** , copy your **Signing Secret** — you'll use this to verify that events are actually from Slack.


2


#### Open Blink and describe your bot


Go to[blink.new](https://blink.new/) and start a new project. Describe what you want:


> "Build a Slack bot backend. It needs a POST /slack/events endpoint that verifies Slack's signing secret, handles slash commands, and responds with messages. Use the SLACK_SIGNING_SECRET and SLACK_BOT_TOKEN environment variables. Store conversation history in the database."


Blink generates the backend — the HTTP endpoint, the request verification logic, the database schema for storing state, and the Slack API client. The database is a Postgres instance included automatically. No Supabase account needed.


3


#### Add your Slack credentials to Blink


In your Blink project's environment settings, add two variables:


- ` SLACK_SIGNING_SECRET` — from your Slack app's Basic Information page
- ` SLACK_BOT_TOKEN` — the` xoxb-` token from OAuth & Permissions


These are stored securely as environment variables. The bot code reads them at runtime. You never commit secrets to your repo.


4


#### Register your Blink endpoint with Slack


Deploy your Blink project — it gets a public HTTPS URL automatically (e.g.,` https://yourproject.blink.app` ).


Back in your Slack app settings:


- **Event Subscriptions** → enable and set the Request URL to` https://yourproject.blink.app/slack/events`
- **Slash Commands** → create a` /yourbotname` command pointing to` https://yourproject.blink.app/slack/slash`


Slack will send a verification challenge to your endpoint. Blink's generated code handles this automatically — if you describe it in step 2, the bot responds to the challenge correctly.


5


#### Invite the bot and test


In Slack, type` /invite @yourbotname` in any channel. Then use your slash command or trigger the events you configured.


To add AI responses, tell Blink:


> "When the bot receives a message mentioning it, call the OpenAI API with the message and conversation history, then post the response back to Slack."


Blink adds the AI integration. The conversation history is stored in the database automatically — no Redis, no session management to configure.


## Four Bots Worth Building


Once the foundation is running, here are four extensions you can add with a single Blink prompt:


**1. Daily standup reminder**


> "Add a cron job that runs at 9am on weekdays, sends a DM to each team member asking for their standup update, then collects responses and posts a summary to #standup."


Blink adds the scheduler and the message routing. The database stores which team members to remind and their timezone.


**2. Ticket creation from Slack**


> "When someone types /ticket followed by a description, create a record in the database with status 'open,' generate a ticket ID, and post a confirmation with the ID back to the channel."


No external ticketing service needed. The tickets live in your Blink database. Build an admin panel in the same project to manage them.


**3. Leave request approver**


> "When someone uses /leave \[dates\] \[reason\], post an approval request to #hr-approvals with Approve and Deny buttons. When a manager clicks a button, update the status in the database and notify the requester."


Slack's[Block Kit](https://api.slack.com/block-kit) handles the interactive buttons. Blink handles the database state and the followup messages.


**4. Channel summarizer**


> "Add a /summarize command that reads the last 50 messages from the current channel, calls the AI to generate a bullet-point summary, and posts it as an ephemeral message visible only to the person who ran the command."


The` channels:history` scope reads the messages. The AI summarizes them. The response is invisible to the rest of the channel — no noise.


## What Makes This Different From the Lambda Approach


AWS Lambda approach Blink approach


Setup time 4-8 hours Under 1 hour


Services to configure 7+ (Lambda, API Gateway, IAM, RDS, Secrets Manager, VPC, CloudFormation) 1


Database Configure RDS or DynamoDB separately Included


Secrets management AWS Secrets Manager Blink env vars


Deploy CloudFormation or Terraform Automatic


Custom domain API Gateway + Route 53 Included


Cold start latency 100-800ms on Lambda cold start Persistent server, no cold starts


Monthly cost (starter) $0-10 for Lambda itself + $25 RDS minimum From $0 (Blink free tier)


Lambda's operational cost is low. The setup cost is high. For a team that already runs their infrastructure on AWS and has DevOps engineers who know the stack, Lambda is a reasonable choice.


For a team that wants a working Slack bot by end of day — without a DevOps engineer —[Blink](https://blink.new/) removes the friction.


## Frequently Asked Questions


No. You describe what you want in plain English and[Blink](https://blink.new/) generates the code. You'll need to follow the Slack app setup steps in the API console (creating an app, copying credentials, registering the event URL) — that's a form-filling process, not a coding one. The bot logic, database schema, and API integration are all generated by Blink.


Slack requires your endpoint to respond within 3 seconds or it retries the event. For quick operations (database lookups, simple responses), returning a` 200 OK` immediately is fine. For AI-powered responses that take longer, the pattern is: acknowledge the event with` 200 OK` immediately, then send the AI response asynchronously using` chat.postMessage` . Blink's generated code handles this pattern when you describe it — ask for "async response with immediate acknowledgment."


Slack retries events if your endpoint doesn't respond quickly enough or returns an error. Add a` X-Slack-Retry-Num` check to your endpoint: if this header is present and greater than 0, return` 200 OK` immediately to stop retries. Store processed event IDs in your database to prevent duplicate processing.[Blink](https://blink.new/) includes the database for deduplication automatically.


Yes. Tell[Blink](https://blink.new/) : "When the bot is mentioned or receives a DM, call Claude/GPT-4 with the message and the last 10 messages as conversation history, then post the response." Blink adds the AI API call and the conversation history storage. No separate AI API management needed — store your API key as a Blink environment variable.


In your Slack app settings, go to Slash Commands and create a command (e.g.,` /mybot` ). Point the Request URL to your Blink endpoint — e.g.,` https://yourapp.blink.app/slack/commands` . In Blink, tell the AI: "Handle POST requests to /slack/commands — parse the command name and text, route to the right function, and respond." Each command becomes a route in your Blink backend.


Yes — this is where[Blink](https://blink.new/) shines over a pure Lambda approach. Tell Blink: "Add an admin panel where I can see all the tickets the bot has created, approve leave requests, and configure which users receive standup reminders." Blink generates the frontend admin UI connected to the same database the bot uses. No separate app needed.
