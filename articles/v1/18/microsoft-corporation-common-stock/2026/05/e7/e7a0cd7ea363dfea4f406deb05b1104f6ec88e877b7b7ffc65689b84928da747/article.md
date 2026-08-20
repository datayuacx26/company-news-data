---
schema_version: "1.0.0"
document_id: "e7a0cd7ea363dfea4f406deb05b1104f6ec88e877b7b7ffc65689b84928da747"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0d567709f64e"
canonical_url: "https://www.microsoft.com/en-us/power-platform/blog/power-pages/build-power-pages-sites-with-ai-through-agentic-coding-tools-now-generally-available/"
published_at: "2026-05-29T18:33:02+00:00"
first_seen_at: "2026-07-20T04:34:28.280378+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:2e7cf57dc2d0f62496872bfec4ef00ef0df8e0f7dbebc38431a5c35853164d73"
---

# Build Power Pages Sites with AI through Agentic Coding tools, now Generally Available

We are excited to announce the General Availability (GA) of the Power Pages plugin for[GitHub Copilot CLI](https://github.com/features/copilot/cli/) and[Claude Code](https://claude.ai/code) . Just few months back when we introduced the[preview](https://www.microsoft.com/en-us/power-platform/blog/power-pages/build-power-pages-sites-with-ai-using-agentic-coding-tools-preview/) , you could describe a site in natural language and the plugin generated the scaffolding, set up Dataverse entities, wired up the Web API, and deployed the website. We then[added server-side logic](https://www.microsoft.com/en-us/power-platform/blog/power-pages/build-your-server-side-logic-with-ai-new-power-pages-agentic-code-skills/) so your sites could call external services, manage secrets, and run secure business logic.


With GA, the plugin now covers the part of the journey that decides how your website project reaches your users, i.e., getting to production safely. You get Application Lifecycle Management (ALM) with the native Power Platform Pipelines, security hardening for your live site, and an expanded selection for authentication setup, all through the same conversational workflow you already know.


## What’s new at GA


The preview helped you build. GA helps you ship, govern, and protect what you build.


- **ALM and pipelines.** Promote your site from development to test to production with native Power Platform Pipelines, no external infrastructure required.
- **Site security.** Configure a web application firewall, tighten browser security headers, scan your live site for vulnerabilities, and audit table permissions before you go live.
- **Authentication and access.** The` /setup-auth` skill now configures more than one identity provider, generates registration and profile pages.


Everything from the preview is still included, generally available and production supported: site creation, deployment, activation, data modeling, Web API integration, server logic, cloud flows, and SEO.


Agentic authoring with Power Pages Plugin


## Move to production with ALM and pipelines


A site that lives only in a development environment isn’t finished. The new ALM skills package your site into a Dataverse solution and deploy it across environments using[pipelines in Power Platform](https://learn.microsoft.com/power-platform/alm/pipelines) , Microsoft’s native, in-product Continuous Integration and Continuous Delivery (CI/CD). There is no separate build server to stand up and no specialized ALM knowledge required to get started.


Describe what you want, and the plugin runs the right skills. Start with` /plan-alm` if you’re not sure where to begin: it’s the orchestrator and runs the other skills for you. If you already know what you need, run a direct skill on its own.


Skill Command What it does


Plan ALM (start here)` /plan-alm` Orchestrates the whole process: gathers your promotion strategy and target environments, generates a deployment plan for your review, then runs the other skills in the right order


Set up solution` /setup-solution` Creates a publisher and solution and adds your site components


Set up pipeline` /setup-pipeline` Configures a Power Platform pipeline from development to your target environments


Configure environment variables` /configure-env-variables` Makes site settings environment-specific so each stage gets the right values


Deploy pipeline` /deploy-pipeline` Validates the package and runs the deployment, polling until it completes


Export and import solution` /export-solution` ,` /import-solution` Packages and moves your solution between environments when you need manual control


Diagnose deployment` /diagnose-deployment` Surfaces upload and asynchronous errors, matches them to known failures, and offers fixes


Whichever path you take, the deployment plan is reviewable before anything runs, so you decide the promotion path and approvals up front. The plugin then handles the export, import, version handling, and environment wiring that used to be manual, repetitive work.


## Secure your site for production


Going to production for an external website means exposing your site to the open internet. The new security skills help you close common gaps without leaving your agent session, and each one explains what it found in plain language. Start with` /security-review` if you’re not sure where to begin, it’s the skill orchestrator which steps you through the others. If you want to target one area, run a direct skill on its own.


Skill Command What it does


Security review (start here)` /security-review` Orchestrates an end-to-end review across the live site, browser headers, firewall, authentication, and permissions, consolidates everything into one report, then runs the direct skills as needed


Manage firewall` /manage-firewall` Turns on the[web application firewall](https://learn.microsoft.com/power-pages/security/web-application-firewall) (powered by Azure Front Door) to block common attacks such as cross-site scripting, and adds custom IP, country, path, and rate-limit rules to protect a sign-in or sign-up page from brute-force attempts


Manage headers` /manage-headers` Reviews the security headers your site sends to browsers and fixes gaps in[Content Security Policy](https://learn.microsoft.com/power-pages/security/manage-content-security-policy) , Cross-Origin Resource Sharing (CORS), frame protection, and cookie behavior


Scan site` /scan-site` Runs a security scan against your published site and summarizes findings by severity so you know what to fix first


Audit permissions` /audit-permissions` Analyzes your table permissions against your code and Dataverse metadata, flagging overly permissive or missing rules


Whichever path you take, the plugin recommends a safe configuration, shows you the implications, and waits for your approval before making any change.


## Set up authentication and access


Authentication is one of the first things a real site needs, and small misconfigurations are common. The improved` /setup-auth` skill generates a clean sign-in and sign-out flow with anti-forgery token handling, and it now goes further:


- **More than one identity provider.** Beyond the default Microsoft Entra ID provider, configure additional identity providers, such as other OpenID Connect or OAuth 2.0 providers, so you can offer the sign-in options your users expect.
- **Registration and profile pages.** Give users a way to sign up and manage their own account details, not just sign in and out.


Paired with` /create-webroles` , you can stand up authenticated access, self-service registration, and role-aware UI in a single conversation.


## You stay in control


As in preview, the plugin proposes and you approve. The Data Model Architect, Web API Permissions Architect, and Server Logic Architect agents present their plans before making changes, and the new ALM and security skills follow the same pattern. Nothing is created, deployed, or hardened until you confirm.


## Get started


### Install or update the plugin and PAC CLI


If you’re new to the plugin, install it from the Microsoft marketplace and restart your agent. For the easiest setup, use[Quick Install (recommended)](https://learn.microsoft.com/power-pages/configure/create-code-site-using-claude-code#quick-install-recommended) ,


Already building with the preview? Turn on auto-update from the` /plugin` menu, or reinstall to pick up the GA skills. Then try the new flow end to end: run` /create-site` to build,` /security-review` to harden, and` /plan-alm` to promote your site to production.


**[Get started with the Power Pages plugin for GitHub Copilot CLI and Claude Code](https://learn.microsoft.com/en-us/power-pages/configure/create-code-site-using-claude-code) .**


## We are looking for your feedback


Your feedback helps us improve the developer experience on Power Pages. Share your thoughts and reach out on the[Power Pages Community Forum](https://powerusers.microsoft.com/t5/Power-Pages-Community/ct-p/PowerPagesCommunity) . You can also submit ideas through the[Power Pages Ideas portal](https://ideas.powerpages.microsoft.com/) .


## Resources


- [Get started with the Power Pages plugin for GitHub Copilot CLI and Claude Code](https://learn.microsoft.com/power-pages/configure/create-code-sites)
- [Pipelines in Power Platform](https://learn.microsoft.com/power-platform/alm/pipelines)
- [Use Power Platform pipelines with Power Pages](https://learn.microsoft.com/power-pages/configure/power-pages-pipelines)
- [Web Application Firewall for Power Pages](https://learn.microsoft.com/power-pages/security/web-application-firewall)
- [Manage your site’s Content Security Policy](https://learn.microsoft.com/power-pages/security/manage-content-security-policy)
- [GitHub Copilot CLI](https://github.com/features/copilot/cli/) and[Claude Code](https://claude.ai/code)
