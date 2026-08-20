---
schema_version: "1.0.0"
document_id: "a9b9bc502e939fa57edb69757d7b20508f9526d4939b2ad89fd1e6893a6c14bc"
company_key: "yc-diversion"
company: "Diversion"
source_id: "yc-diversion-news-import-29544632d9e8"
canonical_url: "https://www.diversion.dev/blog/diversion-product-updates-may-highlights-05-26"
published_at: "2026-05-31T00:00:00+00:00"
first_seen_at: "2026-07-21T16:38:14.982062+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:6fd8fd545c5d3ff6b607b4df156c652c3786e6853e6ce1697b6cc078f9f5a1db"
---

# Diversion Product Updates: May Highlights (05/26)

Dear community,
This month, the sloth learned a few new tricks - we hope they will be useful! Ready to see what’s new in Diversion?


### **🦥 Meet SlothBot: Diversion’s new code helper**


SlothBot is here in free research preview! Built directly into the Diversion Desktop app, SlothBot connects to your repo so you can ask questions about your code, fix bugs, and create boring boilerplate, so you can focus on your creative tasks.


**As always, we take privacy seriously, and your data always stays yours.**


Your prompts and data are never used for training. You can opt-out from this feature in the organization settings (requires organization admin permissions).


SlothBot is available as a free research preview, with features, pricing, and availability expected to evolve based on your feedback.[Explore SlothBot here](https://docs.diversion.dev/slothbot) .


### **🔐 Sign up or sign in with GitHub**


One less password to remember. You can now sign into Diversion using your GitHub account.


Just one thing to keep in mind: each Diversion account uses one sign-in method. Existing email/password accounts can’t later switch to GitHub login, and GitHub-based accounts can’t switch back to email/password, so choose the method you’d like to keep using for that account.[Sign in with GitHub here](https://app.diversion.dev/signin?) .


### **🧠 Diversion-Ask: keep the “why” behind AI code changes**


AI-generated code is great. Forgetting why something changed three days later? Less great...


If you use Claude Code, Diversion-Ask automatically captures your Claude Code sessions and links them to the exact Diversion commits they produced. Then, you can query your history with /diversion:ask and get answers grounded in your actual code context.


Ask things like “Why was this file changed?” or “What did we discuss about auth last week?” - without digging through old chats, commit messages, or your own memory. 😌


Get Diversion plugin for Claude Code[here.](https://github.com/DiversionCompany/diversion-claude)


### **🔔 Webhook branch filtering**


Your integrations can now be a little more selective. Webhooks can be filtered by branch, so they only fire on the branches that matter - like main or release branches - instead of reacting to every single event.


### **📁 Self-serve repo transfer between orgs**


Moving repos between organizations no longer requires a support ticket. Organization owners can now transfer repositories directly from the app, with approval from the receiving organization’s owner before the transfer is completed.


### **🏢 Owning organization on dashboard repo cards**


If you work across multiple organizations, this one will save you a few “where does this repo belong again?” moments. Repo cards on the dashboard now show which organization owns each repo, making it easier to stay oriented at a glance.


### **👥 Invite collaborators right after creating a repo**


You created the repo, now don’t forget the humans 😇


After creating a new repository, Diversion now helps you invite collaborators right away, helping teams avoid that classic “wait, who actually has access?” moment before work starts moving.


### **🎮 Unreal Engine plugin: pre-submit validation**


Pre-submit validation failures in UE now properly block **Submit** . Previously, Epic’s submit dialog treated validation results as advisory and still allowed failed submissions through. Your team’s validation rules are now enforced as intended.
