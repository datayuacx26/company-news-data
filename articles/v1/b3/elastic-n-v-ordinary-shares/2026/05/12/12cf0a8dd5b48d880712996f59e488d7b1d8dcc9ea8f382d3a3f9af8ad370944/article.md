---
schema_version: "1.0.0"
document_id: "12cf0a8dd5b48d880712996f59e488d7b1d8dcc9ea8f382d3a3f9af8ad370944"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/entity-analytics-agent-builder"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:c743608349c32f0f311896f2a57a859970545b6c1078d24a622f71cb57cce9d3"
---

# Elastic Conversational Entity Analytics: threat hunting in a single conversation

4 May 2026 •


[Erik Huang](https://www.elastic.co/security-labs/author/erik-huang) •


[Paulo da Silva Junior](https://www.elastic.co/security-labs/author/paulo-da-silva-junior)


# Elastic Conversational Entity Analytics: threat hunting in a single conversation


Conversational Entity Analytics delivers Entity Analytics features as rich inline attachments and Canvas previews into Agent Builder, so you don’t have to leave the conversation.


4 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


Entity Analytics is a core security analytics capability that extends Elastic Security from event-centric to entity-centric investigation.


By focusing on critical entities, such as users, hosts, and services, it builds a complete profile of each entity’s attributes, lifecycle, behaviors, relationships, and risk score over time. This security context equips threat hunters to stop chasing isolated alerts and instead uncover the full narrative of a potential compromise. In this blog, we walk through Conversational Entity Analytics, the Agent Builder AI agent skill that delivers entity risk scores, profiles, dashboards in-line, and more, so the hunt stays in one place.


##


Why Entity Analytics matters for threat hunters


Threat hunting in most SIEMs is a tab-juggling exercise. The hunter sees a risk score in one place, opens the host detail page in another, navigates to the dashboard for context, jumps to alerts to read the evidence, and then back to a notes app to write down what they found. Every pivot loses context. Every navigation costs minutes. And the hunts that matter most the subtle, cross-source ones) are the hardest to phrase as a query in the first place.


Conversational Entity Analytics collapses that loop. The hunter can start with a question in the Agent Builder chat or ask a question after clicking on an entity in the Kibana UI, and the answer is delivered into the conversation as rich inline attachments and Canvas previews. The hunt becomes interactive with an AI agent acting as a defender and guiding each step of the way.


##


What Conversational Entity Analytics is


Conversational Entity Analytics is the Entity Analytics AI Agent Skill in Elastic Agent Builder. It turns natural-language questions about users, hosts, and services into the same structured outputs the Entity Analytics Kibana UI produces ranked entity lists, full entity profiles, resolution groups, and the Entity Analytics Dashboard), rendered directly inside the conversation.


Two rendering modes do the heavy lifting: **Rich inline attachments** land the answer in chat as a live, structured artifact. **Canvas previews** open the corresponding Entity Analytics surface in a panel next to the conversation. The hunter never leaves the thread, and the underlying source of truth is always Entity Analytics in Kibana.


Two rendering modes, one conversation:


-


**Rich inline attachments:** Structured cards that appear in line with the skill's reply, such as ranked entity tables, entity profile cards with risk-score breakdowns, and dashboard cards. Every attachment carries an "Attachment added" marker so the hunter knows it will persist with the thread.


-


**Canvas previews:** A Preview action on any attachment opens the full Entity Analytics Kibana UI surface in a Canvas pane beside the chat.


##


1. Start the hunt in chat. Or in the Kibana UI. Or in both.


Entity Analytics provides an out-of-the-box experience on what the riskiest entities are in your environment through our pre-generated AI-Hunting Leads and entities list by risk score. However, if a hunter has a specific question in mind and wants to ask it directly, the hunter can open the Elastic Agent Builder and ask:


**Prompt:** What are the top 5 riskiest hosts in my environment?


The agent loads the entity-analytics skill, which is visible in the reasoning trace as: "Now that the entity-analytics skill is loaded, I'll search for the top 10 riskiest hosts in the environment." Same Entity Store. Same risk score contract. Same answer the Kibana UI would return, delivered as a conversation.


##


2. The conversation follows the hunter into the UI


When asked about a specific user, host, or service, the conversation opens a user interface within the chat and includes links to directly open the Kibana UI for entity flyouts.


The hunters get to the same page they would have reached by navigating manually, and with Conversational Entity Analytics, they can interact through the conversation.


##


The power to threat hunt in any way


Every Entity Analytics AI Skill in the Chat-First Experience has a corresponding Kibana Entity Analytics UI surface it can hand off to, preview, or sit alongside. The hunter chooses the path: some hunts are best opened in chat, and others are best opened in the UI. Hunters can interact freely between both.


**What this means for the hunter:** Start with a question, a hypothesis, a dashboard, or a raw log. Move between chat and the Kibana UI at any point. The Entity Store, Risk Score contract, Unified Entity Resolution, AI Hunting Leads, Watchlists, and the Entity Analytics Dashboard are the same underneath — reached through whichever surface fits the moment.


In practice, Hunters spend less time navigating and more time analyzing. They get to the right entity in seconds, see the full risk-score breakdown and threat narrative inline, without losing the evidence on screen. The hunt accelerates, and the surface of what’s interactive expands.


[Entity Analytics AI Skills](https://www.elastic.co/docs/solutions/security/ai/agent-builder/skills-use-cases#entity-risk-investigation) offer a conversational experience. Together with the Kibana UI, they give every hunter the power to hunt in any way.


#### Jump to section


- [Why Entity Analytics matters for threat hunters](https://www.elastic.co/security-labs/entity-analytics-agent-builder#why-entity-analytics-matters-for-threat-hunters)
- [What Conversational Entity Analytics is](https://www.elastic.co/security-labs/entity-analytics-agent-builder#what-conversational-entity-analytics-is)
- [1. Start the hunt in chat. Or in the Kibana UI. Or in both.](https://www.elastic.co/security-labs/entity-analytics-agent-builder#1-start-the-hunt-in-chat-or-in-the-kibana-ui-or-in-both)
- [2. The conversation follows the hunter into the UI](https://www.elastic.co/security-labs/entity-analytics-agent-builder#2-the-conversation-follows-the-hunter-into-the-ui)
- [The power to threat hunt in any way](https://www.elastic.co/security-labs/entity-analytics-agent-builder#the-power-to-threat-hunt-in-any-way)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Elastic%20Conversational%20Entity%20Analytics:%20threat%20hunting%20in%20a%20single%20conversation&url=https://www.elastic.co/security-labs/entity-analytics-agent-builder)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/entity-analytics-agent-builder)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/entity-analytics-agent-builder&title=Elastic%20Conversational%20Entity%20Analytics:%20threat%20hunting%20in%20a%20single%20conversation)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/entity-analytics-agent-builder&title=Elastic%20Conversational%20Entity%20Analytics:%20threat%20hunting%20in%20a%20single%20conversation)
