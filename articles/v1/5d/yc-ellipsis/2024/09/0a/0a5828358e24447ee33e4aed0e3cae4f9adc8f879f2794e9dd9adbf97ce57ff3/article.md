---
schema_version: "1.0.0"
document_id: "0a5828358e24447ee33e4aed0e3cae4f9adc8f879f2794e9dd9adbf97ce57ff3"
company_key: "yc-ellipsis"
company: "Ellipsis"
source_id: "yc-ellipsis-news-import-97656c4ed8a9"
canonical_url: "https://www.ellipsis.dev/blog/how-to-move-your-github-app-to-gitlab"
published_at: "2024-09-01T00:00:00+00:00"
first_seen_at: "2026-07-21T17:55:16.673666+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:c509500170df5dd35b61dbfa5b4a199aa3d17567ced194afc56a9152aef94d7e"
---

# How to Move Your GitHub App to GitLab

Ellipsis launched as a GitHub App and recently expanded to support GitLab. The two platforms handle third-party integrations quite differently, and we're sharing our migration experience to help other developers bring their applications to GitLab.


## Background concepts


### Architectural differences


GitHub consolidates authentication, webhooks, and UI into its "Application" concept. GitLab treats these as separate components, which offers greater granularity. The GitLab Applications API enables resource access on behalf of a user, but bot account actions require a different approach.


### A note on nesting


GitHub's structure is relatively flat: Accounts and Repositories. GitLab Projects support arbitrary nesting through Groups, allowing paths like` top_group/sub_group/another_sub_group/project_name` instead of a simple` owner/repo` . If your application logic assumes two-level paths, revisit those assumptions first.


### SaaS vs. self-managed


Unlike GitHub's predominantly SaaS model, GitLab offers both SaaS and self-managed options. Your application must track a host per installation — from the standard` gitlab.com` to custom domains like` https://gitlab.example.com` .


## Authentication with Access Tokens


End users manually create Access Tokens in two varieties: Group Access Tokens and Project Access Tokens, which differ mainly in scope. Avoid Personal Access Tokens — they impersonate individual users rather than functioning as bots.


Customers configure tokens through the GitLab UI with:


- **Name.** The display name shown on actions and comments.
- **Role.** The required permission level — for a code review tool, the Developer role.
- **Scopes.** Typically` api` ,` read_repository` , and` write_repository` .
- **Expiry date.** Maximum one year; GitLab Ultimate admins may impose stricter limits, so plan for token rotation.


Group Access Tokens grant privileges across subgroups and subprojects, which suits large organizations with nested structures while still allowing selective installation in a subset of the org.


### The bot user


GitLab automatically associates access tokens with bot users. The display name is configurable, but usernames and avatars are auto-generated (e.g.` group_123_bot_4ffca233d8298ea1` ). Display names enable autocomplete during mentions — a marked improvement over GitHub, which doesn't allow autocomplete for bots or Apps.


## Setting up webhooks


Webhooks are configured separately from Access Tokens, through the webhook UI at the group or project level where the token was created:


- **URL.** The endpoint receiving POST requests from GitLab. Consider an event gateway (we use one) for retries and reliability.
- **Events.** Select the triggers you need — push events, issue events, merge request events, and comments are the common set.
- **Secret token.** Generate a unique, cryptographically strong string per customer to prevent event spoofing.
- **SSL verification.** Leave it enabled.


## OAuth and logins


Direct Access Token usage gives you immediate functionality without any OAuth setup — verify with a simple GET request using a library like` python-gitlab` . That said, implementing "Login with GitLab" is convenient for users managing settings and billing, and follows the standard OAuth2 pattern you already know from GitHub. Libraries like NextAuth.js ship a GitLab provider.


## Putting it all together


A simple webpage with clear instructions and a form collecting (1) group/project URLs, (2) access tokens, and (3) webhook secrets (auto-generated or user-entered) is all the onboarding you need. You can see our approach at[app.ellipsis.dev/gitlab](https://app.ellipsis.dev/gitlab) .
