---
schema_version: "1.0.0"
document_id: "eae82e4ce7d7aa43ed513a2de6fbfd0a6b571fdc2a1566ec81c6a7a60bb940c3"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/ai-chat"
published_at: "2025-06-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:dd70f5a0971e99dd507e40e0182d28380d302eb481d6e04e0d9e1f4de1d82fbc"
---

# Windmill AI Chat: Context-aware assistance across the entire platform

AI is becoming a natural part of how we build and automate. With the right context and interface, it can significantly speed up tasks without disrupting your workflow. At[Windmill](https://www.windmill.dev/) , we've embraced this shift by introducing an AI chat assistant that's deeply integrated across the entire platform.


The assistant is now available on every screen: in the[script editor](https://www.windmill.dev/docs/script_editor) ,[flow editor](https://www.windmill.dev/docs/flows/flow_editor) ,[app editor](https://www.windmill.dev/docs/apps/app_editor) , and even whilenavigating . It's always nearby when you need it, yet unobtrusive when you don't.


> All of the modes are evidenced in the video above.


## A mode for every task​


To make the assistant more effective, we've introduced three specialized modes:Navigation ,Script , andFlow . These modes help ensure more relevant responses and better user experiences, tailored to the task at hand.


### Navigation mode​


Navigation mode is ideal for discovering Windmill's features and understanding how the platform works. It's especially helpful for new users, providing quick guidance and links to the[docs](https://www.windmill.dev/docs/intro) . For more advanced users, it's still useful for uncovering less obvious functionalities without needing to search manually.


### Script mode​


In the[script editor](https://www.windmill.dev/docs/script_editor) , the assistant shines by working with rich context. You can provide specific context - like referencing a database schema, selecting a few lines of code, or comparing with the latest deployed version - all easily addable using the` @` symbol or through a context dropdown.


Once provided, this context enables a range of quick actions. For example, if you test a step and it fails, you'll be offered options to fix the error or improve the code right from the chat. These actions are tuned to the situation, helping you move faster with fewer steps.


You can also choose to apply or discard specific parts of a suggestion. This granular apply/reject is built directly into the Monaco editor and makes AI suggestions easier to manage - not a flashy feature, just a necessary one.


### Flow mode​


[Flow](https://www.windmill.dev/docs/flows/flow_editor) mode turns natural language into fully structured workflows. You can describe your intent in plain English, and the assistant will build a flow step by step, connecting scripts, filling in parameters, and even handling branches and loops.


It pulls from your workspace and[Windmill Hub](https://hub.windmill.dev/) , reusing existing components or generating new ones as needed. Based on the context, it sets inputs, chooses iterator expressions, and configures predicates to match the data structure of your flow.


## Supported models and interface details​


Windmill AI is compatible with a[range of language models](https://www.windmill.dev/docs/core_concepts/ai_generation) . While we mostly test and optimize with OpenAI and Claude - and currently see the best results with Claude - the assistant is designed to work well with other configured models too.


You can easily switch between your available models from within the chat interface. For quick access, use` Cmd + L` to open the assistant at any time, from anywhere in the app. The assistant adapts to the screen you're on and the actions you're taking, aiming to assist without interrupting.


## Looking ahead​


We see AI becoming an increasingly central part of the Windmill experience. We're focused on making it as helpful, flexible, and responsive as possible.


In the near future, expect a more intelligent AI chat, tighter integrations across the platform, and a redesigned autocomplete experience that speeds up scripting.


In addition to the[MCP server we already provide for executing scripts](https://www.windmill.dev/docs/core_concepts/mcp) we're also exploring the possibility of exposing a similar capability for creating, editing, deploying, and configuring scripts, flows, triggers, and other Windmill resources - accessible from local tools like Claude or Cursor.


The AI assistant is now live and available across[Windmill cloud](https://app.windmill.dev/) , and on all[self-hosted](https://www.windmill.dev/docs/advanced/self_host) instances after version[v1.495.0](https://github.com/windmill-labs/windmill/releases/tag/v1.495.0)


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
