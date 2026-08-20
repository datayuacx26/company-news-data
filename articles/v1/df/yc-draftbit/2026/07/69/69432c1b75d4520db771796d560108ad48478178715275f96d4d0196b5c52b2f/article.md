---
schema_version: "1.0.0"
document_id: "69432c1b75d4520db771796d560108ad48478178715275f96d4d0196b5c52b2f"
company_key: "yc-draftbit"
company: "Draftbit"
source_id: "yc-draftbit-news-import-aa5bd69b40d1"
canonical_url: "https://draftbit.com/whats-new/gpt-5-6-grok-4-5-native-file-uploads-openrouter-claude-code/"
published_at: "2026-07-10T23:40:00+00:00"
first_seen_at: "2026-07-24T05:13:30.805139+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:236f85d529fc170ab1c08bb8549e451f965b294801e1a8b5239433762c8ca8ee"
---

# GPT-5.6, Grok 4.5, Organization-wide AI Providers, Free Plan Model Choice, Native File Uploads + more!

Hey builders! Quick update focused on giving you more choice in how you build and removing a few more obstacles from the workflow. We have added a new generation of OpenAI models, expanded what you can run through OpenRouter, added organization-wide AI provider connections, opened up more model choice on the Free plan, made visual component assembly more direct with the Add Component panel, brought native mobile project files into AI chat, and shipped another round of project, sandbox, Astro, export, and docs improvements.


> **TL;DR:** GPT-5.6 Sol, Terra, and Luna join Draftbit alongside Grok 4.5, and Claude Code can now be used with active OpenRouter-native models. Organizations can connect Claude and ChatGPT subscriptions plus OpenAI, Anthropic, and OpenRouter API keys once and share them across every app. Free-plan users on Draftbit Credits can choose any supported OpenAI Codex model instead of being locked to GPT-5.4. Plus the Add Component panel, native file uploads, code-editor downloads, and more reliable planning, projects, sandboxes, and docs.


## The GPT-5.6 Family Is Here


Draftbit now supports GPT-5.6 Sol, Terra, and Luna as Codex model options. All three models support context windows of up to 1.05 million tokens and outputs of up to 128,000 tokens, giving the agent substantially more room for large codebases, long conversations, and complex multi-step work.


These are the newest, most powerful and capable models. In a few weeks, if we like performance, we will likely start to phase out the gpt 5.4 models as these gain more usage.


## Grok 4.5 and More OpenRouter Flexibility


Grok 4.5 is now available through OpenRouter with either Draftbit Credits or your own OpenRouter key. It supports a 500,000-token context window and can be selected for Codex-based work. We've found Grok 4.5 surprisingly capable AND fast, so definitely check it out.


Claude Code can now be used in combination with OpenRouter keys. That currently includes Grok 4.5, DeepSeek V4 Pro, DeepSeek V4 Flash, and GLM 5.2. If you prefer Claude Code's workflow but want to use a model available through OpenRouter, you have more combinations to choose from without switching agents.


We also retired Qwen 3.7 Max from the active model list while preserving compatibility with existing threads that previously used it.


Behind the scenes, we fixed xAI compatibility and moved plan approval into Draftbit's own event flow. Planning with Grok now works even when xAI does not support MCP namespace tools, and plan approvals or revision requests survive queued prompts and thread reloads.


## Connect AI Providers Once for Your Organization


Organization admins can now connect OpenAI, Anthropic, and OpenRouter API keys, Claude Pro or Max subscriptions, and ChatGPT subscriptions from **Settings → AI Providers** . Every app in the organization, including newly created apps, can inherit those connections without signing in or adding the same key again.


App-level connections still take priority, so teams can share sensible organization defaults while individual apps use their own provider when needed. Draftbit now shows whether a key or subscription is coming from the app or the organization, and thread history records which credential scope funded the work.


Subscription credentials now live encrypted in Draftbit's database instead of being stored in sandbox Git history. Existing app logins migrate automatically the next time their sandbox starts, so teams get the safer storage model without needing to authenticate again.


## More Model Choice on the Free Plan


Free-plan users building with Draftbit Credits are no longer locked to GPT-5.4. Codex remains the available agent for free Draftbit-credit threads, but the model picker now lets you choose any active OpenAI model that Codex supports on credits, including the new GPT-5.6 options.


GPT-5.4 remains the safe fallback if an old or unsupported selection is encountered. Claude Code and OpenRouter models remain unavailable on free Draftbit-credit threads, while existing bring-your-own-key and connected subscription behavior is unchanged.


## Add Components Visually


We have been steadily improving the **Add Component** panel, but it has never had a proper introduction. Select an element in the Component Tree, open the **+** menu, and choose **Add Component** . From there, click a component to add it beneath the selected element or drag it onto the exact parent you want—without asking the agent or writing the import and JSX by hand.


The **Available** tab lets you search a curated catalog of ready-to-use components, grouped by source. **Existing** surfaces reusable components already in your app and gives you a shortcut to edit their source. The panel uses the selected parent to keep incompatible choices out of the way, while still letting you reveal them when you want to see every option.


Draftbit handles the repetitive setup when you add a component, including imports, compatible package versions, useful starter values, and any required configuration. It is a faster, more precise way to assemble and extend an interface visually while keeping the generated code clean and editable.


## Bring Native Mobile Files Into AI Chat


You can now attach native iOS and Android project files directly to AI chat. Supported files include Swift, Objective-C, Kotlin, Java, Gradle, Xcode project and configuration files, storyboards, CocoaPods files, Ruby and Fastlane files, native C and C++ modules, and related lockfiles.


Extensionless files such as` Podfile` ,` Gemfile` ,` Fastfile` , and` Appfile` are supported as well. Uploaded filenames keep their capitalization, and nested` ios` or` android` directories used by plugins and custom native modules can be tracked without being hidden by top-level generated-folder rules.


This makes it much easier to ask the agent for help with native configuration, custom modules, build settings, plugins, and publishing issues without first converting or renaming the files.


## Projects Keep Getting Sharper


Project app cards now show a clear app-type icon for mobile apps, web apps, and Astro sites. Their “Updated” timestamps also reflect real work such as agent runs, saves, commits, visual edits, and publishes instead of only app settings changes.


Apps are ordered using that recent activity, making it easier to jump back into the work you touched most recently. We also fixed the Builder app switcher so it shows the correct project name when an app belongs to a different organization than the one most recently selected.


Builder browser tabs now include the project name alongside the app name, making it easier to identify the right tab when you have several projects or apps open at once.


## + More


#### Download files from the code editor


The code editor's file menu now includes a **Download file** action in both the three-dot menu and right-click context menu. Files are downloaded without text conversion, so images and other binary assets stay intact.


#### More reliable Astro previews


Astro previews no longer flip into a failed state because of a transient full-reload error, and static fallback builds no longer create unnecessary watcher churn. Unsupported Design, Styles, and Properties controls are now clearly disabled for Astro apps while visual editing is still in beta, so the available code, agent, configuration, and history workflows are easier to understand.


#### GitHub exports are working again


We fixed an authentication-helper issue that could prevent an app from being exported to GitHub. Export now uses the same safe credential handling as our newer sandbox workflow.


#### Faster, steadier sandbox startup


Repository cloning and sandbox server startup now allow more time for normal initialization instead of prematurely restarting work that was still making progress. This reduces avoidable startup delays and timeout failures, especially for larger projects.


#### Cleaner preview consoles


We removed a source-map error produced by analytics canvas replay that could make a working preview look broken. DOM and iframe session replay remain available, while the noisy canvas worker is no longer started.


#### Faster docs on mobile


Draftbit Docs now serve responsive image sizes across the site, use smaller source screenshots, and defer nonessential support scripts until interaction or idle time. The iOS and Android publishing guides also host their screenshots directly with the docs, keeping those guides reliable and allowing the images to use the same optimization pipeline as the rest of the site.


---


That's a wrap for this round. Thanks for building with us, and keep the feedback coming. Happy building!
