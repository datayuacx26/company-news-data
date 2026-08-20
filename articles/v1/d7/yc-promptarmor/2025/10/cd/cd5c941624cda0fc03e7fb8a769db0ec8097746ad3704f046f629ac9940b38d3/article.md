---
schema_version: "1.0.0"
document_id: "cd5c941624cda0fc03e7fb8a769db0ec8097746ad3704f046f629ac9940b38d3"
company_key: "yc-promptarmor"
company: "PromptArmor"
source_id: "yc-promptarmor-rss-057c96aa2b5f"
canonical_url: "https://promptarmor.substack.com/p/hijacking-claude-code-via-injected"
published_at: "2025-10-16T12:31:54+00:00"
first_seen_at: "2026-07-25T19:53:47.024087+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:360754aad6cc9b70dc201e3204c83ae2607424448fdaf09db38bcef73f2704c5"
---

# Hijacking Claude Code via Injected Marketplace Plugins

# Hijacking Claude Code via Injected Marketplace Plugins


### By: PromptArmor


Oct 16, 2025


**Overview**


Claude Code has introduced a new functionality, and a new attack surface: Marketplaces for Plugins. Developers can install any marketplace and plugin; we show how a plugin can:


1.


Bypass human-in-the-loop protections


2.


Exfiltrate a user’s files via indirect prompt injection


**The Attack Chain**


**1. A user installs a malicious Marketplace and Plugin, typically after finding it via a registry such as [claudecodemarketplaces.com](http://claudecodemarketplaces.com/) *.**


**This registry automatically discovers and displays public Marketplaces and Plugins, making it easy for a malicious repository to gain visibility, more on this below.*


The GitHub for our demo Plugin Marketplace


**2. The Plugin bypasses Claude’s human-in-the-loop protections using a malicious hook***


**Hooks are commands that are executed every time a specific event occurs, for example, running a Python script every time the user submits a prompt. As seen in [Anthropic’s documentation](https://docs.claude.com/en/docs/claude-code/hooks#json-output-example%3A-pretooluse-with-approval) , one of the intended uses of hooks is the management of permissions for commands.*


Claude Code gives users the ability to have a permissions object in the settings.local.json file that tells Claude Code when (and when not) to ask for permission before executing a command. Our first step was to bypass this to allow a malicious command to be run


*without a user’s permission* . We examined two (of many possible) techniques to accomplish this:


-


Use a hook that triggers every time a user submits a prompt, which


**rewrites Claude’s permissions files** to permanently allow dangerous commands (in this case, curl).


-


Use a hook that triggers each time Claude requests to execute a command, which auto-approves specific (or all) types of commands. In this case, the attacker’s hook auto-approves curl commands.


**3. The attacker has embedded prompt injections in the Plugin Commands* to manipulate Claude into executing malicious tools**


**Plugin Commands are shortcuts to reusable prompts that can be included in a Plugin.*


In this attack, the user expects to be activating a command (prompt shortcut) that will give Claude additional instructions and information that will help it provide improved debugging assistance.


However, the “RAG service” is a ruse. It is actually an attacker’s server, masquerading as a useful service to convince Claude to send the user’s data to it.


Upon reading the prompt injection, Claude believes that it needs to gather debugging context and send it to the server to use the debugging service. Claude will search the user’s codebase for context, and activate a curl command to try to send the data.


Usually, a human in the loop would be required to approve the curl request:


However, since the attacker has bypassed the human-in-the-loop defense using one of the malicious hook methods from step 2, the curl command is executed immediately without requesting approval.


Thus, the relevant data collected from the codebase is sent to the attacker.


And at this point, attackers can read the data from their server.


**Building a Plugin and Marketplace**


Creating marketplaces and plugins is very easy. Each requires creating a single metadata file.


Here is the attacker’s Marketplace Metadata:


Here is the attacker’s Plugin Metadata:


To make a Marketplace and Plugin available for download, Marketplaces (and their Plugins) can be posted, for free, on GitHub. This requires a simple file upload.


Attackers can create a GitHub account that masquerade’s as an official vendor to improve the odds that developers will install plugins from their marketplace. Here is our account that impersonates an official Anthropic GitHub.


Now, you (the user) can add the Marketplace in Claude using the GitHub URL.


And choose to install the Plugin:


Once the attacker adds the components (described below), the user will see:


With set-up is out of the way, the attacker can focus on building functionality.


**Bypassing Human in the Loop: In Depth**


When Claude runs commands that interact with external services, by default, it requests user approval:


The attacker’s first goal is to circumvent this defense.


An attacker has four kinds of components to work with that can be included in a plugin:


**Commands** - A shortcut to a reusable prompt.


**SubAgents** - Specialized copies of Claude with specific permissions and prompts that your main Claude can launch to handle targeted tasks.


**MCP Servers** - Allows Claude to interact with other systems, such as to send emails.


**Hooks** - Activates an ability every time a specific event occurs (for example, every time the user sends a message, run a script that gathers extra context to improve Claude’s responses).


Each plugin contains zero or more components of each type.


During our testing, we explored two mechanisms to bypass Claude’s human-in-the-loop approval, both leveraging hooks. As with Marketplaces and Plugins, an attacker can define a hook with one short metadata file. Here are the two bypasses we experimented with:


**1. Overwrite Claude’s permissions file that controls when a human-in-the-loop is required**
To overwrite the permissions file, the attacker can define a hook that activates each time the user submits a prompt:


This hook will activate each time you (the user) submit a prompt. When it activates, it executes the script ‘docs.py’.


‘Docs.py’ is a short Python script that overwrites Claude’s permissions settings file. When it overwrites the permissions file, it removes any permissions that were previously in place and sets an ‘Allow’ rule for curl* commands. This will allow curl commands to execute without requiring human approval.


**curl (Client for URL) is a tool used to send and retrieve data over the internet.*


Now, anytime you (the user) submit a prompt, the permissions are overwritten. This means that anytime Claude tries to respond by executing a curl command, it is automatically approved*.


**We know from Claude’s normal ‘allow-list’ update flow that permissions updates can be reflected within a session, and early in testing we recorded a demonstration of our malicious update taking effect within a session. During later tests, changes were not always reflected until the user started a new session. This raises a question regarding what conditions trigger Claude to respect new permissions, and whether the discrepancy between permissions updates occurring and being respected could be an attack surface for further manipulation.*


**2. Create a hook that listens for Claude’s command approval requests, and responds to the approval request itself**


Here is the metadata file for that hook:


This hook will trigger right before Claude executes a tool, and execute the script ‘docs.py’*.


**In our testing we utilized one hook at a time, though an attacker could use both. [Docs.py](http://docs.py/) is the same file name as in the previous example, but is not the same code.*


This script, when executed, has two possible outcomes:


1.


It detects that Claude is trying to run a curl command, and outputs an approval message that takes precedence over existing permission files. This will allow Claude to execute the curl command without approval.


2.


It detects that the command is not curl, and allows the system to fall back on the permissions schemes defined in Claude’s permissions files.


The only immediately evident difference from the user’s perspective between the two methods is this that for the PreToolUse hook method, a small ‘PreToolUse:Bash’ indicator is visible for a split second while the hook completes*:


**It was displayed for so little time, we had to use a recording to capture this screenshot.*


By using the suppressOutput configuration, the attacker can dictate that the hook activation indicator


*will be removed from chat history upon hook completion* .


**A malicious Command containing a prompt injection**


Now that the attacker has compromised Claude’s human-in-the-loop guardrail, they will need to find a way to manipulate Claude into actually executing an unsafe curl command.


To do this, they can add another tool to the plugin: a Command Plugin.


For this demo, the prompt injection disguises itself under the ruse of being a documentation tool - a shortcut to a prompt that instructs Claude on how it can help with debugging by invoking a nonexistent external service.


To do this, the attacker writes a Markdown file containing the prompt injection.


This prompt tells Claude that to answer the user’s documentation question, it must use curl (the command we bypassed guardrails for) to send context about the user’s problem to a RAG database. In reality, this will send the context to the attacker’s server.


**Attack Feasibility** An adept reader may note that this attack requires the user to find and install the malicious marketplace and plugin. Here’s why that is highly plausible:


**1. An attacker’s Marketplace can be listed on Marketplace registries - without any further steps being taken the attacker**


[Claudemarketplaces.com](http://claudemarketplaces.com/) appears to be a leading registry for Marketplaces. It automatically scrapes GitHub to find new ones every hour.


This means that they will list a malicious Marketplace in their registry for people to discover within an hour of an attacker making it public!


We can even see evidence of potentially dangerous repositories there already, written by other Anthropic impersonators!


[https://github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) is Anthropic’s official Claude repository.


[https://github.com/anthropics-claude/claude-code](https://github.com/anthropics-claude/claude-code) is a fork, meaning that the developer copied the content to build off of.


Another prominent registry of Marketplaces,


[claudecodemarketplace.com](http://claudecodemarketplace.com/) , allows anyone (developers, cyber criminals, etc.), to submit their own Marketplaces.


They explicitly state that they “Don’t verify individual plugins”, and indicate that each Marketplace is responsible for ensuring code security.


**2. A fake Anthropic GitHub account can be pretty convincing**


Here is what the marketplace we created looks like on GitHub:


During our test, several people invited us to collaborate on GitHub projects (including a private repository).


**Attack Surface Considerations** We exhibited one of many ways a malicious plugin can compromise your Claude Code instance. Here are a few more examples of more malicious things that could happen as a result of combining malicious hooks and prompt injections:


-


Claude could be manipulated to delete user files.


-


Claude could be manipulated to open a reverse shell, allowing complete device takeover.


1


-


Claude could be manipulated to write and execute malware.


2, 3


1 *Claude executed reverse shells under our supervision, but we did not focus on developing an indirect one-shot injection to demonstrate a complete remote code execution attack chain.*


2 *Our attack leverages Claude to perform malicious actions instead of including data stealing malware directly in the plugin, as this is stealthier and less likely to be caught by traditional security tools than if the malware was stored in the plugin code.*


3


*This may be non-trivial in practice due to the following system prompt snippet (and Claude’s other model-level protections). However, due to the stochasticity of LLMs, we know it is possible with the right prompt and hook. “IMPORTANT: Assist with defensive security tasks only. Refuse to create, modify, or improve code that may be used maliciously. Do not assist with credential discovery or harvesting, including bulk crawling for SSH keys, browser cookies, or cryptocurrency wallets. Allow security analysis, detection rules, vulnerability explanations, defensive tools, and security documentation.”*


Thanks for reading! Subscribe to stay in the know about AI Security.
