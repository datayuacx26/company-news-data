---
schema_version: "1.0.0"
document_id: "3e9df4ca686641178f6e008260e56c634f31c87af2e6d3790c0228e1a986937e"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/connect-gmail-to-your-agent-without-google-cloud"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-24T07:49:23.882568+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:adc83991c5555186893204338b15fc3cdfda7dce6048da5f4924242c088e67cc"
---

# Connect Gmail to your agent without a Google Cloud project

Giving a personal agent access to your Gmail sounds like it should be one click, and it usually is not. The common path, a command line tool like[gog](https://operator.io/blog/debugging-gog-openclaw-google-cli) that talks to the Google APIs, asks you to build a Google Cloud project, enable each API by hand, create an OAuth client, download a` client_secret.json` , and click through a consent screen. Then it stores a refresh token in a file or your keyring, and on a personal account that token can expire after seven days unless you publish the app. None of it is hard exactly, but it is an afternoon of Google Cloud Console for the privilege of letting your agent read your inbox. There is a path that skips almost all of it.


## Why connecting Google is hard


The difficulty is built into how Google works: it treats every app touching your mail as its own registered application. That registration, the Cloud project plus OAuth client, has to exist somewhere and be owned by someone. When you wire it up yourself, you are the one who owns it, which means you also own enabling the right APIs, keeping the OAuth app published so tokens do not expire, and storing the refresh token safely on whatever box your agent runs on.


If the moving parts here are fuzzy, ByteByteGo's short explainer walks through the authorization code grant and the difference between an access token and a refresh token, the exchange a broker like Composio or Pipedream runs and then keeps alive for you.


Google's[OAuth 2.0 documentation](https://developers.google.com/identity/protocols/oauth2) spells out the testing mode trap explicitly. A Cloud project with an external user type and a publishing status of Testing issues refresh tokens that expire in seven days unless the only scopes requested are basic profile and email. Google's[app audience guide](https://support.google.com/cloud/answer/15549945) confirms the same rule: authorizations by a test user expire seven days from consent, including any refresh token issued for offline access.


Moving to production requires publishing the app and, if you request sensitive Gmail scopes, completing[Google's verification process](https://support.google.com/cloud/answer/13461325) . The deep version of those problems, including the` accessNotConfigured` errors and the keyring that a background service cannot read, is its own[debugging guide](https://operator.io/blog/debugging-gog-openclaw-google-cli) . The short version is that you become a small OAuth operations team for one inbox.


## The managed OAuth path


The alternative is to let a connection broker own the registered application instead of you.[Composio](https://composio.dev/) and[Pipedream Connect](https://pipedream.com/docs/connect) both do this: they hold an approved OAuth client for Gmail, Calendar, Drive, Slack, Notion, and the other apps you connect through them, run the sign in popup, store the resulting tokens securely, and refresh them on their own. When your agent needs Gmail, it asks the broker, which hands back a live token. You never create a Cloud project, never download a client secret, and never get the Monday morning failure from an expired testing token.


The two are similar in shape and differ mostly in catalog and billing. Composio publishes its integrations as a[Model Context Protocol](https://modelcontextprotocol.io/) endpoint and leans toward AI agent use, with a usable free tier. Its[Gmail toolkit docs](https://docs.composio.dev/toolkits/gmail) list the actions available, confirm OAuth2 as the auth type, and note that Composio's managed app is available out of the box so you can connect without your own Google credentials.


Pipedream Connect exposes a comparable registry and a[managed auth quickstart](https://pipedream.com/docs/connect/managed-auth/quickstart) flow built for embedding in your own product. For the everyday case of running actions from an agent, both default to the broker's own OAuth client, which is why you can connect Gmail without registering anything with Google yourself.


Both brokers also support per user OAuth, which matters when more than one person will connect their own Gmail. Composio's[per user OAuth guide](https://composio.dev/content/per-user-oauth-for-ai-agents) explains why shared credentials fall apart at scale and how user scoped tokens stay isolated. Each user completes Google's consent screen once, the broker stores the refresh token linked to that user ID, and the agent never sees the raw token. If you are running something serious and want control over the exact scopes or your own consent screen branding, both also let you[bring your own OAuth client](https://docs.composio.dev/toolkits/gmail) , which is the route to take for production even though it reintroduces a Cloud project on your side.


## How it looks in Operator


[Operator.io](https://operator.io/) is managed OpenClaw, and it leans on this path on purpose so connecting Google is not a Cloud Console errand. The flow is short:


1. Open the integrations in your dashboard and pick Composio or Pipedream.
2. Sign in to your own broker account in the popup and approve.
3. The first time you ask the agent to do something in Gmail, approve Google's normal sign in screen, and the agent has it.


There is no config file to edit and nothing to restart. The full Composio walkthrough is in[automating anything with Composio and OpenClaw](https://operator.io/blog/automate-anything-with-composio) , and the same pattern covers Gmail, Calendar, and Drive.


Because the connection sits with your own broker account, the tokens, the linked apps, and the usage are all yours, not pooled into a shared key. To cut the agent off from an app, you disconnect it in the broker's dashboard or revoke the app from your[Google account permissions](https://myaccount.google.com/permissions) .


## The tradeoff


With the managed path, the broker holds the OAuth connection to your Google account, so you are trusting Composio or Pipedream with that relationship the way you already trust them with the other apps you connect. With a local tool like gog, the tokens live on your own machine and you own every scope, at the cost of running the Cloud project and the token plumbing yourself. If keeping Google access entirely on hardware you control is the point, use gog and the[debugging guide](https://operator.io/blog/debugging-gog-openclaw-google-cli) will get you there. If you mostly want your hosted agent reading your inbox without a Cloud Console detour, the managed path is the faster and more durable answer.


Two details make that trust concrete. The first is scope. Connecting Gmail through a broker grants the read, send, and modify access the Gmail actions use, and you can see what was granted and narrow it in the broker's connection settings, or bring your own OAuth client when you want to pin the scopes yourself. Granting read only first and adding send once you trust it is the same easing in you would do with gog.


The second is what the brokering does and does not cover. Because the token stays in the broker's vault and never enters the model's prompt, a prompt injection cannot read the credential out of the conversation, which is a real gain over a refresh token sitting in a config file. That does not stop a bad instruction from using the tools the agent already holds, so having it show a draft before it sends, and reading what it plans to archive or delete, still earns its keep.


For Calendar, Drive, and Sheets beyond Gmail, the[Google Workspace credentials guide](https://operator.io/blog/connect-google-workspace-to-your-agent) covers the gws route when you want one credentials file that covers the whole suite. The managed broker path and the gws path can coexist: Gmail through Composio for the inbox, gws for Calendar and Drive, each scoped to what you actually need.


## A first task


However you connect it, start with something read only so the output is easy to check before you let it act:


```text
Read me my unread email from the last day and tell me which three need a reply.


```


Once you trust how it reads the account, let it draft and ask to see the result first:


```text
Draft a reply to the last email from Priya saying Thursday at 2 works, but show it to me before sending.


```


The setup is the part that used to take an afternoon, and the managed OAuth path is what turns it back into the one click it always should have been. You can[try it on Operator free](https://operator.io/pricing) , or wire up gog yourself if owning the tokens matters more than skipping the setup.
