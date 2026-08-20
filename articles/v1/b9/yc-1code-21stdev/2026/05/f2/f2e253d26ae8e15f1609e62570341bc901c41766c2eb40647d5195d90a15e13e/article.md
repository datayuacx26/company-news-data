---
schema_version: "1.0.0"
document_id: "f2e253d26ae8e15f1609e62570341bc901c41766c2eb40647d5195d90a15e13e"
company_key: "yc-1code-21stdev"
company: "1code (21st.dev)"
source_id: "yc-1code-21stdev-news-import-0b053d108858"
canonical_url: "https://21st.dev/blog/component-libraries"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-24T04:15:31.771016+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:c37291cfd905fa90e7aa1c16ab38c52afe5f891d82ba5c8fc049752c083cba99"
---

# Introducing Component Libraries: Publish from Your Terminal, Used by Your Agent

## The problem: agents invent UI every time


Most teams already have the components an agent needs. A Figma button the design team blessed six months ago. A marketing block that's been re-used in every campaign. A modal pattern someone wrestled accessibility into for two days. The agent doesn't see any of it. So it invents from scratch, every time. The result is a codebase that looks like five different people wrote it, because, in a way, they did. Five separate generation runs by the same model.


The shadcn registry model fixed this for the public ecosystem: components live as code, installable on demand. Libraries on 21st brings the same model to your team's private components.


## What we shipped


A Library is a scoped collection of components your agent installs from on demand. Two pieces:


1. **Publish your components** into a library scoped to you or your team.
2. **Tell your agent** to install from that library when it needs UI.


No new MCP server. No new model. No SDK to install. Just one line of rules pointing at a shell command your agent already knows how to run.


## How it works


### 1. Publish


You publish a component two ways: open Studio and upload through the UI, or run the CLI from your terminal (handy in CI, scripts, or when an agent itself is doing the publishing).


$


```text
# One-time auth (caches your key to ~/.21st/credentials)
npx @21st-dev/cli login


# Publish from your repo. Defaults to personal scope, public visibility.
npx @21st-dev/cli publish ./src/button.tsx \
--description "Primary button"


# Private and scoped to a team
npx @21st-dev/cli publish ./src/button.tsx \
--private --to acme
```


The CLI supports` --public` ,` --unlisted` ,` --private` , and` --to <registry-slug>` so a CI job can publish a whole library deterministically. No clicking, no manual visibility toggles. The same component config used by` npx shadcn` works here too, so existing component repos publish as-is.


Components in a library can be public (show up in the 21st catalog after review), unlisted (installable by URL but hidden), or private (scoped to your team's API key). You can mix all three in the same library; the library organizes, visibility is per-component.


### 2. Add to your agent's rules


Each library's page has a ready-to-paste snippet. Open the library, copy the block, and paste it into` .cursor/rules/components.mdc` ,` CLAUDE.md` , or whatever rules file your editor uses. The core of it is one shell command:


$


```text
npx @21st-dev/cli add @your-scope/<component-name>?api_key=$API_KEY_21ST
```


Wrapped in a few lines of instruction telling the agent to prefer this command over generation, plus a note that private components need` API_KEY_21ST` set in your shell env. That's the whole integration. The agent now knows two things: what library to use, and how to install from it.


### 3. Build


Ask your agent to build something: "create a landing page", "add a settings dialog", "build the dashboard." When it needs a button, a card, a form input, it runs the install command instead of generating yet another bespoke implementation. The component lands in your project as code you can read, edit, and version-control. Just like shadcn, but with *your* components.


## Visibility, scope, and access


- **Personal scope** holds libraries that only you can publish to. Private components in your personal scope are visible only to you. Switch to a team scope to share with collaborators.
- **Team scope** holds libraries the whole team can publish to. Private components in a team library are installable by anyone on that team. They just need an API key that belongs to a team member.
- **Public components** in either scope show up in the 21st catalog after review. Same install command, no API key required.


You set visibility per component. A single library can hold a few public showcase pieces and a long tail of internal patterns nobody outside your team should see.


## Why this matters


The default path for an AI agent is generation. Every UI element is a fresh draw from the model, and the results drift: slightly-wrong spacing, slightly-different focus rings, slightly-different toast styling. Libraries close the loop: components you've already shipped become components the agent reaches for first. The codebase becomes more consistent, not less, the more your agent works on it.


There's also a more practical angle. The components you've published represent real engineering work: accessibility you don't want to redo, tokens you don't want to re-derive, edge cases the design system team already caught. Reusing them is the cheapest way to keep that work alive.


## What's next


This release ships the publish, scope, and rules-based install flow. A few obvious next steps:


- **Native MCP tool.** Today the install runs through a shell command from your rules. The natural next step is an MCP tool that lists a library's catalog and installs without you having to re-list component names. The rules approach is shipping first because it works today with zero new infrastructure on the agent side.
- **Auto-populated rules.** A one-click "copy rules for this library" that picks up the components you've actually published, so the rules file stays in sync as you add new pieces.
- **Cross-team grants.** Today a library is scoped to you or your team. Letting one team grant another install access (without making components fully public) is the next access-control step.


If you have a design system, a shared component set, or even three buttons you keep re-using, publish them into a Library today, drop the rules snippet into your editor, and stop your agent from re-inventing them.


[Open Studio →](https://21st.dev/studio)
