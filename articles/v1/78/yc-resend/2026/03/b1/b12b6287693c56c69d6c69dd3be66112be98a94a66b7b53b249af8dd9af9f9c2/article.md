---
schema_version: "1.0.0"
document_id: "b12b6287693c56c69d6c69dd3be66112be98a94a66b7b53b249af8dd9af9f9c2"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/cli"
published_at: "2026-03-13T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:c4a4fe31d9c1c6cfc9d26de0abde927f09c6f356934e7e2b1208add4b31853b5"
---

# Official CLI for Resend

We believe that the best tools meet you where you already are.


For many of us, that's the terminal.


That's why we're introducing the[Resend CLI](https://github.com/resend/resend-cli) .


It's an open source project with more than **53 commands across 13 resources** .


## Built for humans & agents


We created the CLI to work well for both humans and AI agents.


**For humans:** interactive mode with suggestions, readable tables and spinners, and natural language for things like scheduling ("tomorrow at 9am").


**For agents:** authenticate directly with an API key, get machine-readable JSON output by piping, and send emails with idempotency keys so agents can safely retry.


## Install


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


**PowerShell (Windows)**


```text
irm https  :  /  /  resend  .  com  /  install  .  ps1   |   iex
```


Or download the` .exe` directly from the[GitHub releases page](https://github.com/resend/resend-cli/releases/latest) .


## Authenticate


The` login` command opens your browser, walks you through creating an API key, and stores your credentials locally.


```text
$ resend login
```


For agents, you can pass a key directly:


```text
$ resend login   --key   re_xxxxxxxxx
```


## Send emails from the terminal


The fastest way to send a test email:


```text
$ resend emails send   \        --from     "Acme <hello@resend.dev>"     \        --to     "user@gmail.com"     \        --subject     "Hello from the CLI"     \        --html     "<h1>It works!</h1>"
```


In interactive mode, the CLI suggests your verified domains and common sender prefixes like` hello@` and` team@` , so you can fire off an email with just a few keystrokes.


Already have a local HTML file? The CLI can send it directly:


```text
$ resend emails send   \      --html-file   "welcome.html"
```


## Manage your entire account


Every resource you can manage in the[Resend API](https://resend.com/docs/api-reference/introduction) is available in the CLI.


That's 53 commands (subcommands) across 13 resources, including:


- **Domains** : create, verify, configure, enable open/click tracking
- **API keys** : generate scoped tokens per domain for tighter security
- **Contacts** : create, update, manage subscriptions and segments
- **Broadcasts** : draft, schedule, and send bulk emails to contacts
- **Webhooks** : register endpoints and subscribe to any of 17 event types


For example, creating a domain-scoped API key for your CI pipeline:


```text
$ resend api-keys create   \        --name     "ci-pipeline"     \        --permission     "sending_access"     \      --domain-id   "8b8ceabd-7b45-4244-82d9-8082821dcb5b"
✔ API key created
Name:    ci-pipeline     ID:      acf3445b-d0a0-4c0a-8138-0e9fa1c66190     Token:   re_xxxxxxxxx
⚠  Store this token now — it cannot be retrieved again.
```


## Schedule broadcasts with natural language


One of our favorite features: you can schedule broadcasts using plain English:


```text
$ resend broadcasts send   "f47ac10b-58cc-4372-a567-0e02b2c3d479"   \       --  scheduled  -  at   "tomorrow morning"
```


The CLI parses natural language dates like` "in 1 hour"` or` "next Monday at 3pm"` alongside standard ISO 8601 timestamps.


## Switch between teams and accounts


If you work across multiple Resend teams or accounts, the CLI handles that too.


Switch between profiles without logging in and out:


```text
$ resend auth   switch    │   ◆  Switch to which profile  ?    │  ○   default    │  ●   dracula     (  active  )    └
```


You can also use the` --profile` flag on any command to run it with a specific profile.


```text
$ resend domains list   --profile   dracula
```


## Integrate with your CI/CD pipeline


The CLI has two output modes.


In an interactive terminal, you get styled tables, spinners, and guided prompts.


```text
$ resend domains list
✔ Domains fetched   ┌──────────────────┬──────────┬───────────┬──────────────────────────────────────┐   │ Name             │ Status   │ Region    │   ID                                     │   ├──────────────────┼──────────┼───────────┼──────────────────────────────────────┤   │ draculatheme  .  com │ verified │ sa  -  east  -  1   │ 8b8ceabd  -  7b45  -  4244  -  82d9  -  8082821dcb5b │   └──────────────────┴──────────┴───────────┴──────────────────────────────────────┘
```


When piped or running in CI, it switches to structured JSON.


```text
$ resend domains list   --  json    {        "object"  :     "list"  ,        "has_more"  :     false  ,        "data"  :     [          {            "id"  :     "8b8ceabd-7b45-4244-82d9-8082821dcb5b"  ,            "name"  :     "draculatheme.com"  ,            "status"  :     "verified"  ,            "created_at"  :     "2026-01-11 16:55:08.253795+00"  ,            "region"  :     "sa-east-1"  ,            "capabilities"  :     {              "sending"  :     "enabled"  ,              "receiving"  :     "enabled"            }          }        ]     }
```


## What's next


This is just the beginning.


We're excited to keep improving the CLI and would love to hear your feedback.


Check out the source code, open an issue, or suggest a feature in the[GitHub repository](https://github.com/resend/resend-cli) .
