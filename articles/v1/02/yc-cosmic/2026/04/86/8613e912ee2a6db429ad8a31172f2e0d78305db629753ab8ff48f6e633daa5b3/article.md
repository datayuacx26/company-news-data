---
schema_version: "1.0.0"
document_id: "8613e912ee2a6db429ad8a31172f2e0d78305db629753ab8ff48f6e633daa5b3"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/ai-agents-reorg-project-scoped-teams-multi-bucket-conversations-and-a-chat-first-experience"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:2334e7ec5a2d81a56ff8986c1c954cb3ed75a0c08a15567e0c4f23e690fee604"
---

# AI Agents Reorg: Project-Scoped Teams, Multi-Bucket Conversations, and a Chat-First Experience

Cosmic AI Agents just got a major upgrade. Agents, Workflows, and Conversations now live at the Project level, not locked to a single Bucket. Pair that with a new chat-first agent experience and smarter limits across your workspace, and managing an AI team in Cosmic feels a lot more like managing a real one.


## Agents Belong to Projects, Not Buckets


Agents used to be scoped to a single Bucket, which made it awkward to run the same agent across staging and production, or to have a "Marketing Manager" that could read from one Bucket and publish to another.


Now every team agent and workflow lives at the Project level:


- **One Marketing Manager, every Bucket.** Hire an agent once and use it across any Bucket inside the Project. No more cloning agents into each Bucket.
- **Project-scoped routes.** Agents, workflows, conversations, and code repositories now live under URLs that always carry the right Project context. Old Bucket-scoped URLs continue to work and quietly redirect to the new home.


## Multi-Bucket Conversations


Inside a chat, agents can now switch the Bucket they are operating on, in the moment, without losing context.


- **Working bucket per conversation.** Each chat keeps track of which Bucket the agent is currently working in. Switch Buckets mid-conversation and the agent picks up the new context immediately.
- **Saved default stays put.** Switching the working bucket only affects the current conversation. The agent's saved default Bucket is left alone, so future runs and scheduled jobs keep using the bucket you configured.
- **Cleaner messaging.** When a Cosmic Agent or team agent switches Buckets, the chat now tells you exactly which Bucket the agent moved to and which one is still its saved default.


## A Chat-First Agent Page


Team agents now open into a dedicated chat experience instead of a generic settings screen.


- **New .** A focused, full-page chat layout for talking to a team agent, with conversations on the left, the chat in the middle, and quick header actions on top.
- **Your face in the chat.** Your avatar and display name show up next to your messages, so multi-user conversations are easy to follow.
- **Conversations rail with user details.** The conversations list now shows who started each conversation: a real Cosmic user when it came from the dashboard, or the channel attribution (Slack, Telegram, WhatsApp, web) when it came in from outside.
- **Quick reply pills.** Agents can now suggest follow-up actions as inline pill buttons. Click one to send it as your next message.
- **Friendlier token-limit errors.** When a conversation hits the AI token limit, you get a clear actionable message with a one-click link to your workspace billing page instead of a generic error.


## Per-Run Bucket on Every Execution


Every agent execution and workflow run now records the Bucket it operated on.


- **Bucket badge on every run.** Agent and workflow execution lists show the target Bucket inline so you can scan a long history and instantly see which run touched which Bucket.
- **Bucket details on the run page.** Open any execution to see the full target bucket info, in addition to the agent and inputs.


## Inline Editors in Agent Previews


When an agent proposes content for review, you can now edit it directly without leaving the preview in the same markdown and WYSIWYG experience as the Object pages in the dashboard.


- **Markdown editor inline.** Tweak markdown content in place before approving.
- **Rich text editor inline.** Edit HTML content with the same full rich text editor, right inside the preview modal.


No more raw HTML or Markdown content to sort through.


## Workspace Resource Limits, Reorganized


Workspace plans now expose three explicit limits instead of one combined "agents" pool:


- **Max Team Agents**
- **Max Automations**
- **Max Workflows**


The Workspace and Project usage screens now show these as separate thermometers, so it's clear which pool you are about to hit and what to upgrade for. Admins can configure each limit independently when creating or editing a Workspace.


Learn more about agent limits per plan on[the pricing page](https://www.cosmicjs.com/pricing#usage-table) .


## Slack, Telegram, and WhatsApp Integrations Move to the Project


Integration setup links inside the agent form (Slack, Telegram, WhatsApp) now use your active Project context. The "Connect" buttons take you to the right Project's integration settings every time, instead of falling back to the active Bucket. Repositories, deployments, and integrations all use a single project-scoped permission model under the hood.


## Project Buckets: Add a Bucket From the List


The Buckets table on a Project page now includes an **Add Bucket** card at the end of the list when you have permission to create Buckets. One click to add a new Bucket without leaving the table.


## Other Polish


A handful of small upgrades that add up:


- **Conversation creator metadata in the API.** The conversations list now returns a clean block: a real Cosmic user when initiated from the dashboard, or a channel-attributed identity for Slack, Telegram, WhatsApp, and web channels.
- **Better loading states.** Project team agents, project usage totals, and workspace usage totals all use structured loading placeholders instead of empty space while data is fetching.
- **Channel badge cleanup.** Channel badges in the agents list no longer wrap awkwardly on narrow screens.
- **Imgix URL repair in AI-generated code.** When Cosmic AI generates a Next.js or React app, any accidental double-host imgix URLs get auto-repaired before deploy, so generated apps don't break image rendering.
- **Bucket home redirect.** now redirects to so the Bucket home is always at one canonical URL.
- **Project home for empty states.** Brand new Workspaces with no Projects yet show a friendly empty state with a clear "Create a Project" call to action.
- **Repository detail layout fix.** The repository detail view now sizes correctly when switching between split view and chat view.


## Try It


These updates are live for every Cosmic Workspace.[Log in](https://app.cosmicjs.com/projects) , open any Project, head to the new **Agents** page, and click into a team agent to try the new chat-first experience.


New here?[Create your free account](https://app.cosmicjs.com/signup) and deploy your first AI-powered site, with help from your AI team, in minutes.
