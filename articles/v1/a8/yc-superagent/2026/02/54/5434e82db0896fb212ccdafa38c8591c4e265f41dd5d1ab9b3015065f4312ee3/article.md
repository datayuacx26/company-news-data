---
schema_version: "1.0.0"
document_id: "5434e82db0896fb212ccdafa38c8591c4e265f41dd5d1ab9b3015065f4312ee3"
company_key: "yc-superagent"
company: "Superagent"
source_id: "yc-superagent-news-import-3c33ed06ece7"
canonical_url: "https://www.superagent.sh/blog/cline-incident-broken-security-model"
published_at: "2026-02-18T00:00:00+00:00"
first_seen_at: "2026-07-24T02:47:34.464363+00:00"
fetched_at: "2026-07-28T22:19:43.789522+00:00"
content_hash: "sha256:4529354ae0b6217cb656e3ae4213fb2be2410fcdcfe197a614f5bf55d96a630d"
---

# The Cline Incidents and the Broken Security Model

On February 17, 2026, someone used a compromised npm publish token to push` cline@2.3.0` to the registry. The package contained exactly one change: a postinstall script that ran` npm install -g openclaw@latest` on every machine that installed it. The window was eight hours. Tens of thousands of weekly downloads. Every developer who ran` npm install -g cline` during that window got openclaw installed globally, without asking for it, without knowing about it.


[openclaw](https://openclaw.ai/) is a legitimate open source project. So the payload was benign. The vector was not.


This is the second Cline security incident in two months. The first,[documented by Adnan Khan](https://adnanthekhan.com/posts/clinejection/) , was worse. An attacker could open a GitHub issue with a malicious title. Cline's AI-powered issue triage workflow would read the title, interpret the embedded instructions as legitimate commands, and execute arbitrary code. From there: GitHub Actions cache poisoning, pivot to the release pipeline, steal the publish tokens, push malware to five million developers. The full chain worked. Someone appears to have attempted it before public disclosure.


Two separate incidents. Two separate attack surfaces. Same underlying problem.


## AI agents treat untrusted content as instructions


When a human reads a GitHub issue title that says "install this helper tool before proceeding," something registers as wrong. Maybe they read it twice. Maybe they google the package name. Maybe they just close the tab because it feels off. Humans are spectacularly bad at security in many ways. But ambient skepticism is one thing they bring by default, and the entire trust model of the internet has been quietly relying on it for thirty years.


Agents don't have ambient skepticism. They have a system prompt and a list of tools. If the content they're reading tells them to do something that falls within their permissions, they do it. This is not a bug. It is the entire point of an agent.


The npm incident follows the same logic from a different angle. Compromised publish tokens are not new. What's new is what happens after the package lands. Developers aren't just installing packages for their own machines anymore. They're running agent workflows that call` npm install` autonomously. An AI coding agent that installs a dependency as part of a task will run the postinstall script. Without reading it. Without pausing. It has work to do and postinstall scripts are not its problem.


Except they are now.


## Automated scanning caught it before humans did


[Brin](https://brin.sh/) , a package gateway built for the agent era,[flagged cline@2.3.0](https://brin.sh/packages/npm/cline?version=2.3.0) before most humans noticed: postinstall script attempting to install an unrelated global package, 95% confidence, confirmed threat. The dependency confusion pattern in the same package flagged separately. Both verified. This is what tooling designed for the agent era looks like: it assumes the consumer might be a machine that won't read the fine print, so it reads the fine print automatically.


The npm registry doesn't do this. It was built assuming someone would look. That assumption stopped being accurate a while ago and the registry hasn't changed.


## What needs to change


What needs to change is not complicated to say. Package registries need behavioral analysis at publish time. Agent workflows need content isolation: a hard distinction between "data to be processed" and "instructions to be executed." AI triage workflows need minimal permissions: if you're reading issues and writing comments, you do not need Bash. The principle of least privilege is fifty years old and still treated as optional.


The Cline team fixed the triage vulnerability in thirty minutes after public disclosure. The npm package was patched in eight hours. Fast, by any reasonable standard. The problem is that "fast" requires someone to discover it, document it, report it, and wait. The attack surface runs continuously and doesn't have an on-call rotation.


## The uncomfortable truth


We are building agents with tool access, network access, and the ability to execute software on behalf of users. We're deploying them into ecosystems designed around the assumption that a human is paying attention somewhere in the loop. The Cline incidents are what happens when that assumption meets reality.


The next one will not involve openclaw.
