---
schema_version: "1.0.0"
document_id: "00eca08bab3baed7d96b2ccdef6d3f913a7be6f3f9d580a010e8894f794010c8"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/resend-cli-2"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:f5ab5dfa4fe88e145699eeed616be89d169018a46c2449f6e645a6aa4e4a996b"
---

# Resend CLI 2.0

The Resend CLI gives you **access to the full Resend API** without leaving your terminal.


Today, we're shipping **version 2.0** , including **four powerful new features** :


- Send emails using local React files
- Automations and events
- Webhook listening
- Skills for AI agents


## Get started


Install the CLI and log in to get started:


**cURL**


```text
curl   -  fsSL https  :  /  /  resend  .  com  /  install  .  sh   |   bash
```


**Node.js**


```text
npm install   -  g resend  -  cli
```


**Homebrew (macOS / Linux)**


```text
brew install resend  /  cli  /  resend
```


Or download the` .exe` for Windows directly from the[GitHub releases page](https://github.com/resend/resend-cli/releases/latest) .


## Send emails using local React files


Since version 1.0, you could use local` .html` files to send emails from your local machine. Now, you can use local React Email files (` .tsx` or` .jsx` ) to send emails as well.


## Automations and events


You can now manage[Automations](https://resend.com/docs/dashboard/automations/introduction) directly from the CLI. Create events, trigger automation runs, and inspect results, all in one place.


```text
resend automations list   resend event-schemas create
```


Build and debug multi-step workflows against your own data, without leaving your terminal.


## Webhook listening


Setting up a local webhook listener used to require a tunneling tool and manual configuration. Now it's a single command.


```text
resend webhooks listen
```


The CLI handles the tunnel and forwards incoming events to your local server. Every[webhook event type](https://resend.com/docs/dashboard/webhooks/introduction) is supported, making it straightforward to develop and test event-driven features locally.


## Skills for AI agents


The CLI now ships with Agent Skills. Skills give your agents an opinionated, well-defined interface for interacting with the Resend API. Skills help your agents follow best practices, avoid unnecessary API calls, and stay on task.


With the CLI, an agent can have its own inbox. It can sign up for accounts, receive confirmation emails, process attachments, and respond to users without your manual intervention.


## Other changes


We've also made a few other improvements to the CLI.


Now, you can programmatically[retrieve and inspect logs](https://resend.com/docs/api-reference/logs/list-logs) from the command line so you can debug and monitor your email events without leaving your terminal. The CLI access also means your agent can help debug and fix issues without your intervention.


We've also **improved the security** of the CLI's key storage. Now, when you create a new API key, it's stored in your system's keychain:


- macOS: Keychain
- Linux: Secret Storage
- Windows: Windows Credential Manager


Finally, we've deprecated the` team` command in favor of the new` auth` command.


Visit the[CLI docs](https://resend.com/docs/cli) to explore all 50+ commands across the full Resend API.
