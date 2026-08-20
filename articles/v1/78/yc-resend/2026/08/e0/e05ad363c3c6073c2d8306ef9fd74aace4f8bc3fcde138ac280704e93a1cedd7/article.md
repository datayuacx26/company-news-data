---
schema_version: "1.0.0"
document_id: "e05ad363c3c6073c2d8306ef9fd74aace4f8bc3fcde138ac280704e93a1cedd7"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/email-compatibility-checker"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T04:04:51.492455+00:00"
fetched_at: "2026-08-13T04:04:53.512457+00:00"
content_hash: "sha256:be59b51fc7aca3de78a8ced946c820144991abc2ced89010670cbe724524b781"
---

# Email Compatibility Checker

In a world of mostly-compatible web standards, developers often assume that email clients support many modern features as well.


Today, we're introducing an **email compatibility checker** in the code editor for Templates and Broadcasts to help you catch major compatibility issues beforehand so you can send with confidence.


As you edit HTML in the editor, your emails are scanned for patterns that clients strip or render inconsistently.


## In edit mode


When you upload an HTML email template using the editor (via the CLI, MCP, API, etc.), the checker runs automatically.


The editor marks flagged code with a wavy underline colored by severity and a warning badge shows a full overview of the issues found.


## In HTML blocks


You can also use the compatibility checker in HTML code blocks within the visual editor.


## What it catches


Client support data comes from[caniemail](https://www.caniemail.com/) and is refreshed daily so it reflects the latest email client support. It catches major known issues like:


- **Content that gets removed** :` <script>` tags, event handler attributes like` onclick` ,` <iframe>` , linked stylesheets,` <svg>` , forms, and more.
- **Layout that breaks** :` display: flex` and` grid` in Outlook for Windows,` position: fixed` , viewport units, and CSS math functions like` clamp()` .
- **Colors that get dropped** : CSS variables without a fallback, and modern functions like` oklch()` and` color-mix()` .
- **Markup left over from a web page** : framework artifacts like` data-reactroot` or Next.js/Nuxt root IDs, a sign the HTML wasn't written for email in the first place.


## Test in your own email client


As always, you can send yourself a test Broadcast or Template to see how your email looks in your own email client.


We're happy to bring this new compatibility checker to code blocks within the Resend editors, giving you more confidence when sending. If you have any questions, please reach out to us and we'll be happy to help.
