---
schema_version: "1.0.0"
document_id: "a66659c09968444b9186857e899bd86b1cbc9313cc444fa57a6f2b9f330a2b66"
company_key: "shopify-inc-class-a-subordinate-voting-shares"
company: "Shopify Inc."
source_id: "shopify-inc-class-a-subordinate-voting-shares-news-import-b162817d780b"
canonical_url: "https://www.shopify.com/blog/claude-code-vs-code-extension"
published_at: "2026-08-12T20:31:05+00:00"
first_seen_at: "2026-08-13T01:33:17.639375+00:00"
fetched_at: "2026-08-13T01:33:19.540665+00:00"
content_hash: "sha256:90f9d7698a902d5cc86e0be28ba8fe87a1c200b1c76774d4790bce5f0247ba92"
---

# How To Use the Claude Code VS Code Extension

The Claude Code VS Code extension combines two powerful coding tools: Claude Code, which uses AI to write, edit, and test code based on plain language prompts, and Visual Studio Code (VS Code), Microsoft’s free code editor used by more than 50 million developers across the globe.


The extension brings both tools into a single environment, embedding Claude Code directly into VS Code as a sidebar panel. Developers, including those building Shopify themes and apps, can give instructions in plain language, review proposed code changes before they’re applied, and manage multiple conversations across different parts of a project—all without leaving the editor they’re already working in.


For developers familiar with the[Shopify CLI](https://help.shopify.com/en/partners/help-support/faq/partner-program) , the result is faster theme customization, more capable app development, and the ability to tackle complex multi-file changes that otherwise would require significant time or outside help. Here’s what the Claude Code VS Code extension is and how[Shopify Partners](https://help.shopify.com/en/partners/build-integrate/making-apps) and developers can take advantage of it.


## What is Claude Code?


[Claude Code](https://claude.com/product/claude-code) is an AI coding tool by Anthropic that handles coding tasks on your behalf. You describe what you want in plain language (“Add a discount for customers who buy more than 10 items”), and it writes, edits, and tests the code for you.


## What is Claude Code VS Code extension?


The[Claude Code VS Code extension](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code) brings Claude Code’s capabilities directly into the VS Code editor, so you can access it without switching to a separate terminal or application. It works the same way as Claude Code—you describe what you want, it writes and edits code on your behalf—but instead of operating in a standalone environment, it sits inside the VS Code interface, removing the need to switch contexts while keeping the same delegate-and-review workflow Claude Code is known for.


Any changes appear as[inline diffs](https://dev.to/shrsv/understanding-diff-formats-a-developers-guide-to-making-sense-of-changes-414o) —highlighting which lines of code have been changed—so you can see exactly what Claude did and accept or reject individual edits before they’re applied.


## How to set up Claude Code in VS Code


1. Meet the prerequisites
2. Install Claude Code
3. Install the Claude extension in VS Code
4. Authenticate
5. Open your project
6. Start using Claude Code


Here’s a step-by-step guide for setting up Claude Code in VS Code.


### 1. Meet the prerequisites


Before you can start using the VS Code extension, you need to satisfy a handful of prerequisites. Make sure you have:


-


**A paid Claude subscription.** Claude Code is not available on the free plan. Claude Pro ($20 per month) includes access.


-


**VS Code 1.98.0 or later.** VS Code is free and available for Windows, Mac, and Linux. If you don’t have it yet, visit[code.visualstudio.com](https://code.visualstudio.com/) and download the installer for your operating system. On Windows, run the installer file, and VS Code will be added to your system automatically. On Mac, download the zip file, extract it, and drag VS Code to your Applications folder.


-


**Node.js 20 or later.** Node.js is a free, open-source software environment that allows JavaScript—a programming language originally designed to run inside web browsers—to run directly on your computer instead. Anthropic’s native Claude Code installer includes Node.js. If you choose to install Claude Code a different way and don’t already have Node.js, you can download it from[nodejs.org](http://nodejs.org/) .


### 2. Install Claude Code


There are two ways to install Claude Code.


Native installer. A native installer downloads a ready-to-run version of the program built specifically for your operating system, with everything it needs bundled inside, so nothing else has to be present on your machine first. This is the method Anthropic recommends: it requires no additional dependencies, installs in under a minute, and updates automatically in the background. Copy and paste one of the following commands into a terminal (your PC, Mac, or Linux command-line interface):


-


**Mac or Linux.** *curl -fsSL https://claude.ai/install.sh | bash*


-


**Windows PowerShell.** *irm https://claude.ai/install.ps1 | iex*


-


**Windows Command Prompt.** *curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd*


**Node package manager (npm).** Npm is the standard tool bundled with Node.js for downloading and installing software packages, meaning this method depends on Node.js already being installed on your system. If you’re currently working in a Node.js environment and prefer to install via npm, open your terminal and run: *npm install -g @anthropic-ai/claude-code* .


### 3. Install the Claude extension in VS Code


Open the *Extensions* view with Cmd+Shift+X (Mac) or Ctrl+Shift+X (Windows/Linux), search “Claude Code,” and click *Install* . Make sure you’re installing the official extension published by Anthropic: Check that the publisher is listed as “Anthropic” with a verified checkmark next to the name. Community-built alternatives may not work with newer versions of Claude or handle your Anthropic credentials correctly.


Once installed, a small lightning bolt icon should appear in your VS Code sidebar. If it doesn’t, try the following:


-


*Run Developer: Reload Window* from the Command Palette (Cmd+Shift+P on Mac, Ctrl+Shift+P on Windows and Linux).


-


Make sure you have a file open—the icon only appears when a file is active.


-


If you’re still having trouble, open a terminal and type *claude doctor* to get a plain-language report about what’s working and what isn’t.


### 4. Authenticate


The first time you open the extension you’ll see an authorization screen. Choose the option that matches your account, such as Claude.ai Subscription for a personal membership or Anthropic Console for pay-as-you-go API billing. Click the matching button, choose *Open* when prompted to continue in the browser, then *Authorize* to confirm.


### 5. Open your project


Open your Shopify project folder in VS Code. The extension bundles the CLI inside it, so the full Claude Code agent is available directly in the editor without switching to a separate terminal.


### 6. Start using Claude Code


Click the Claude Code icon in your sidebar to open the panel. From there, you can give instructions in plain English, review inline diffs before any changes are applied, @-mention specific files when you want Claude to examine a particular part of the project, and open multiple conversations in separate tabs.


Build an online store with AI


Create a website in minutes with the AI store builder. Describe your brand or products to generate a free custom theme that fits your idea.


[Try it now](https://www.shopify.com/tools/ai-store-builder)


## Best practices for using Claude Code CLI and VS Code together


- Use VS Code to review changes, CLI to run them
- Use @-mention files to keep Claude focused
- Use Plan Mode before executing large tasks
- Consult the doctor after updates
- Open your Shopify project as a workspace folder


Getting the most out of Claude Code comes down to staying in control of what it changes and giving it enough context to work effectively. These best practices help with both.


### Use VS Code to review changes, CLI to run them


Let Claude Code execute tasks in the terminal while keeping the VS Code extension open alongside it. Changes appear as inline diffs in the editor as they’re made, making it easy to review, accept, or reject edits without switching contexts.


### Use @-mention files to keep Claude focused


When you know which file needs changing, @-mention it directly rather than letting Claude Code search the codebase. This method is faster and reduces the risk of unintended edits to files you didn’t mean to touch.


### Use Plan Mode before executing large tasks


For any task touching multiple files, switch to Plan Mode. This makes Claude Code describe what it intends to do before making a single change to the actual code—especially important on a live Shopify store where a mistake could affect real customers.


### Consult the doctor after updates


Claude Code updates automatically in the background, and occasionally an update can affect your configuration. Running *claude doctor* periodically can catch problems before they interrupt your work.


### Open your Shopify project as a workspace folder


Claude Code understands your project much better when VS Code has the full project folder open rather than individual files. That way, it can see how your product templates, scripts, and stylesheets relate to each other, instead of treating each file as a standalone document.


## Claude Code VS Code extension FAQ


### Is Claude Code built on VS Code?


No. Claude Code is a separate product built by Anthropic that works independently of a code editor like VS Code. It integrates with VS Code through an extension, but it isn’t built on VS Code the way[Cursor](https://www.shopify.com/blog/claude-code-vs-cursor) is.


### What does Claude Code VS extension do?


The Claude Code VS Code extension brings Claude Code directly into your VS Code editor as a sidebar panel, so you can use it without switching to a separate terminal. It adds a graphical interface for giving instructions, reviewing inline diffs, and managing conversations—all without leaving the editor you’re already working in.


### Is Claude Code better in terminal or VS Code extension?


Neither approach is inherently better or worse; the choice depends on your preferred working style. The terminal gives developers more direct control and pairs naturally with command-line workflows like[Shopify CLI](https://help.shopify.com/en/partners/help-support/faq/partner-program) , while the VS Code extension is easier to use and better suited to reviewing changes visually.
