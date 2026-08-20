---
schema_version: "1.0.0"
document_id: "145b4037576982cce8dda364514cd34e462986471f40d977bb7901b9b47ad3de"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/workspace-skills"
published_at: "2026-05-21T02:28:36+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:bf6157cca95a08e8736947212e824f278584ca1406b017a186529f6fb5bb0710"
---

# Teach Blink the Way You Build with Workspace Skills

## Three ways to add one


Blink builder pinning SKILL.md scrolls on a whiteboard with each scroll connected to a different app project


Blink


**Browse the Blink shelf.** Open **Settings → Skills** . The "Made by Blink" section has a curated set — UI design principles, SaaS scaffolds, SEO article writing, debugging playbooks, deploy recipes. Flip toggles for the ones you want on across your workspace.


**Ask Blink to draft one.** Type` /skill-creator` in any chat and describe what you want taught:


```text
/skill-creator make a skill that teaches my agent to write idiomatic
React Server Components for Next.js 16, including when to use "use client"
and when to compose with Suspense.


```


Blink writes the full skill, validates the frontmatter, and renders a card right in the chat. One click — **Add to workspace** — and it's saved. The skill appears in your slash picker within ~100 ms.


**Bring your own.** Click **Add** in Settings → Skills:


- Write it inline in the multi-file editor (WYSIWYG markdown, file tree, syntax highlighting).
- Drop a` .zip` of any existing skill folder.
- Paste a public GitHub repo URL. The importer finds every` SKILL.md` in the repo and creates a separate skill for each — perfect for skill-collection repos like[anthropics/skills](https://github.com/anthropics/skills) where one repo holds dozens.


## Using a skill


Two flows, pick whichever fits the moment.


**One-off — pin a skill to one message.** Type` /` in the chat input. A fuzzy-matching picker opens with every skill in your workspace plus the Made-by-Blink shelf. Pick one and it applies for that next message only:


```text
/seo-article-writing write an article about
"best ai coding assistants 2026"


```


**Always-on — keep a skill loaded forever.** Toggle a skill on in Settings → Skills. From that moment, the agent has it loaded for every chat in this workspace. You don't type` /skill-name` . It just applies.


Mix both. Most builders end up with a handful always-on (their design system, their language preferences) and a long tail on-demand (deploy playbook, SEO workflow, billing pattern).


## Workspace-scoped means team-scoped


Skills live in your workspace, not in a single project, and not on your personal account. Add one and every teammate on every project in that workspace inherits it on their next message.


This is the part that compounds. The first skill saves you the next 30 chats. By the time your library has 10 skills, you're not really prompting anymore — you're directing.


## Where to find it


**[Settings → Skills](https://blink.new/settings?tab=skills)** — under the Customize section, next to Connectors, Domains, and AI Calling. Open any chat to use` /skill-name` .


Full walkthrough, format spec, and FAQ:


[Workspace Skills docs Three ways to add a skill, how to use them, format spec, edit + share.](https://blink.new/docs/build/skills)[Prompting Guide Write prompts that get exactly what you want.](https://blink.new/docs/build/prompting)[Best Practices Expert techniques for complex builds.](https://blink.new/docs/build/best-practices)[Changelog Everything that shipped on Blink this month.](https://blink.new/docs/changelog)
