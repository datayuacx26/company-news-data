---
schema_version: "1.0.0"
document_id: "42d7b56bc2d04d878f1c58d59d1050329b31fab2e073f8125aab0bb857d2883c"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/company/ai-toolkit"
published_at: "2026-05-22T17:48:34+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:12:57.660175+00:00"
content_hash: "sha256:9c08a306bbd8fd90b921ee76099982c32652d06a7f915d7398d82e7477a10091"
---

# Meet the RevenueCat AI Toolkit

For a growing number of app teams, setup now starts with an agent. The agent sees the codebase, knows the platform, can edit files, can run commands, and can help connect RevenueCat with the app.


Meet the RevenueCat AI Toolkit. It brings RevenueCat into your AI coding assistant, starting with a Claude Code plugin that can help configure RevenueCat, integrate the Purchases SDK, inspect project health, pull RevenueCat Charts data, and troubleshoot in-app purchase issues from the tools where you already write code.


The AI Toolkit gives your agent access to both the RevenueCat context and the skills it needs to understand it.


## Meet the RevenueCat AI Toolkit


The RevenueCat AI Toolkit is distributed as a plugin you install from your AI coding assistant’s marketplace. It is available for Claude Code, and it also works with OpenAI Codex, Gemini CLI, and Visual Studio Code. Cursor support is coming soon; for now, Cursor users can connect the RevenueCat MCP server directly.


The plugin gives your agent two things. First, it includes RevenueCat MCP server configuration, which lets your agent access RevenueCat after you authenticate with OAuth. Second, it includes RevenueCat-specific skills, which guide the agent through setup, SDK integration, paywalls, purchases, testing, analytics, and troubleshooting.


Think of it as access plus knowledge. MCP gives your agent a way into RevenueCat. Skills tell it which steps to take next.


Feature


What it gives your agent


Why it matters


Plugin


RevenueCat inside Claude Code, OpenAI Codex, Gemini CLI, and Visual Studio Code agent plugins beta.


You can work on monetization where your app code already lives.


MCP server configuration


A connection to RevenueCat through OAuth.


Your agent can read and update RevenueCat configuration with your account permissions.


Skills


RevenueCat-aware playbooks for common workflows.


Your agent follows the right order of operations instead of guessing from generic docs.


## What you and your agent can do


**Subscription infrastructure from the editor.** Instead of coordinating every setup step by hand, you can ask your agent to create or select a project, add apps, create products, define entitlements, build offerings, attach packages, and return the public SDK keys your app needs.


This matters because order matters in RevenueCat. Products need to map to entitlements. Offerings need packages. Packages need products. Apps need the right public SDK keys. The AI Toolkit helps your agent follow that order.


**SDK setup with RevenueCat context.** Your agent can detect whether your app is native iOS, native Android, React Native, Flutter, or Kotlin Multiplatform. Then it can install the right Purchases SDK, configure Purchases once at app launch, enable debug logs during setup, and verify the expected SDK output.


That saves more than tab switching. It reduces the classic mismatch between product configuration and app code: the wrong key, the wrong app identifier, or a configure call in the wrong place.


**Paywalls and purchases where the code lives.** If you use RevenueCat Paywalls, your agent can add the right RevenueCatUI package and present the dashboard-configured paywall from your app. If you need custom UI, it can guide the purchase flow around` getOfferings()` ,` purchase(package)` , restore purchases, and entitlement checks.


**Analytics and troubleshooting inline.** Your agent can inspect project status, query RevenueCat Charts, and help diagnose common setup issues like empty offerings, missing products, inactive entitlements, or sandbox purchase failures.


## How to install it


Start with Claude Code. From inside Claude Code, run:


```text
/plugin
```


Choose **Marketplace** , select **+ Add Marketplace** , enter:


```text
RevenueCat/ai-toolkit
```


Then install the **RevenueCat** plugin.


You can also install it from the command line:


```text
claude   plugins   marketplace   add   RevenueCat/ai-toolkit
claude   plugins   install   RevenueCat
```


For OpenAI Codex CLI, add the marketplace first:


```text
codex   plugin   marketplace   add   RevenueCat/ai-toolkit
```


Start Codex, run` /plugins` , search for **RevenueCat** , and install the plugin.


For the OpenAI Codex desktop app, run the same marketplace command in your terminal:


```text
codex   plugin   marketplace   add   RevenueCat/ai-toolkit
```


Then open **Plugins** in the Codex app, select **RevenueCat** from the plugin source dropdown, and click the plus button next to the plugin.


For Gemini CLI, install the extension directly from GitHub:


```text
gemini   extensions   install   <  https://github.com/RevenueCat/ai-toolki  t  >
```


For Visual Studio Code, use the agent plugins marketplace beta. Add this repository as a plugin marketplace, then install the RevenueCat plugin.


Cursor support is coming soon. Until then, configure the RevenueCat MCP server directly in Cursor using the setup instructions in the RevenueCat docs.


For other AI coding environments, you can install the skills with:


```text
npx   skills   add   RevenueCat/ai-toolkit
```


That fallback installs the skills, not the MCP server. If you want live RevenueCat project access in an unsupported environment, configure the MCP server manually.


## What you can ask it


Start with the job you would otherwise split across your editor, RevenueCat setup, store consoles, SDK docs, and logs.


```text
Set   up   RevenueCat   for   my   fitness   app.   I’m   building   for   iOS   and   Android.   I   want   monthly   and   annual   subscriptions.
```


```text
Integrate   RevenueCat   in   this   React   Native   app   and   configure   the   SDK   with   the   correct   public   API   key.
```


```text
What’s   the   status   of   my   RevenueCat   project?
```


```text
What   was   my   revenue   last   month,   and   how   does   churn   look   over   the   last   90   days?
```


```text
My   offerings   are   empty   on   iOS   sandbox.   Diagnose   the   RevenueCat   setup   and   tell   me   what   to   fix.
```


## Frequently Asked Questions


**What can an agent do through the AI Toolkit?**


Your agent can use RevenueCat MCP access and RevenueCat skills to help configure projects, apps, products, entitlements, offerings, packages, and public SDK keys. It can also inspect project health, query chart data, and diagnose common purchase setup problems.


**Can an agent change live RevenueCat configuration?**


Yes, if your authenticated RevenueCat account has permission to make that change. Treat agent access like any other privileged product access. Review changes to projects, products, entitlements, offerings, packages, and webhooks before accepting them.


**How does authentication work?**


The plugin uses OAuth. Your agent gets access based on your RevenueCat account permissions and can work with the projects your account can access.


**Should you use this directly in production?**


Use the same judgment you would use with dashboard or API changes. For setup, testing, and debugging, start with sandbox data, test stores, or non-production projects when the change could affect live users. Confirm purchases in RevenueCat, not only on the device.


**Does this replace the RevenueCat dashboard?**


No. The dashboard remains the place to see, review, and manage RevenueCat visually. The AI Toolkit gives teams another way to work with RevenueCat: the coding agent where more app work now starts.


## Skills included in the AI Toolkit


The AI Toolkit includes thirteen RevenueCat skills. Each one gives your agent a focused playbook for a common developer workflow.


Skill


Use it when you want to…


What your agent does


integrate-revenuecat


Add RevenueCat to an app for the first time.


Sets up the RevenueCat side through MCP, retrieves the public SDK key, detects your app platform, installs the Purchases SDK, configures it once at launch, and verifies the SDK logs.


create-revenuecat-project


Bootstrap a RevenueCat project from scratch.


Creates or selects the project, creates platform apps, adds products, entitlements, offerings, and packages, attaches everything in the right order, and returns public API keys.


revenuecat-paywall


Show RevenueCat Paywalls in your app.


Installs the right RevenueCatUI package, presents the dashboard-configured paywall, handles callbacks, and verifies that your configured template renders instead of a fallback layout.


revenuecat-purchase-flow


Build a custom purchase and restore flow.


Fetches offerings, selects a package, calls purchase, handles cancellations and errors, exposes restore purchases, and treats entitlements as the source of truth.


revenuecat-entitlements-gate


Gate paid features behind subscription access.


Checks customerInfo.entitlements.active, listens for entitlement changes, avoids product-ID-based gating, and verifies that access updates without restarting the app.


revenuecat-identify-user


Connect RevenueCat identity to your auth system.


Wires logIn and logOut, uses stable opaque app user IDs, avoids PII, handles anonymous users, and preserves purchases when users identify later.


revenuecat-testing-setup


Test purchases without charging real money.


Picks the right testing channel, configures RevenueCat Test Store or store sandboxes, separates sandbox and production views, and verifies purchases in RevenueCat.


revenuecat-troubleshoot


Diagnose empty offerings, missing products, inactive entitlements, or failed sandbox purchases.


Reads SDK debug logs, inspects RevenueCat configuration through MCP, checks products, entitlements, offerings, packages, app IDs, and webhooks, then proposes concrete fixes.


revenuecat-status


Get a quick project health check.


Summarizes apps, products, entitlements, offerings, packages, and webhooks, then flags orphaned products, empty offerings, or apps without products.


revenuecat-charts


Ask questions about RevenueCat Charts and analytics.


Queries chart schemas and chart data, reasons about acquisition, conversion, retention, and reactivation, and builds dashboard links for the charts it references.


revenuecat-customer-center


Add self-service subscription management.


Installs and presents RevenueCat Customer Center, connects it to identified users with purchases, and verifies restore, cancel, refund, support, and dismiss callbacks.


revenuecat-migrate


Move from raw StoreKit or Google Play Billing to RevenueCat, or upgrade SDK major versions.


Chooses the migration path, uses observer mode when adopting RevenueCat alongside existing purchase code, preserves user continuity, and verifies sandbox and existing-subscriber behavior.


revenuecat


Handle a RevenueCat task without a more specific skill.


Uses the MCP server and RevenueCat docs as the fallback path.


The skills stay narrow on purpose. Paywall work differs from entitlement gating. Testing differs from migration. Project creation differs from SDK configuration. Smaller playbooks make your agent easier to steer and easier to review.


## Get started


Install the RevenueCat AI Toolkit in Claude Code:


```text
claude   plugins   marketplace   add   RevenueCat/ai-toolkit
claude   plugins   install   RevenueCat
```


Then ask your agent to set up RevenueCat, integrate the SDK, inspect your project, or debug the purchase flow that’s blocking your launch.


[The RevenueCat AI Toolkit is available on GitHub](https://github.com/RevenueCat/ai-toolkit) at` RevenueCat/ai-toolkit` .
