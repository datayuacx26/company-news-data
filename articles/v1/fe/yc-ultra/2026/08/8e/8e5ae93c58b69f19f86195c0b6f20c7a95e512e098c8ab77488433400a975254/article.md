---
schema_version: "1.0.0"
document_id: "8e5ae93c58b69f19f86195c0b6f20c7a95e512e098c8ab77488433400a975254"
company_key: "yc-ultra"
company: "Ultra"
source_id: "yc-ultra-rss-2323deaa6929"
canonical_url: "https://ultra.io/build-on-ultra/"
published_at: "2026-08-03T21:50:29+00:00"
first_seen_at: "2026-08-03T22:13:52.099760+00:00"
fetched_at: "2026-08-03T22:52:40.984273+00:00"
content_hash: "sha256:0b87cfe55b88fa0f24023a6c2963d160258737db81a58d6c8ef03d8f6194e97f"
---

# Build Your Own App on Ultra, With AI

We’ve talked a lot about building on Ultra without making it easy to start. Today we release our[public knowledge base](https://github.com/ultraio/ultra-agent-kb) ** to give you and your AI Agents all the context needed to get started. What follows is the process our own team used, written the way you’d explain it to someone sitting next to you.


**Who this is for:** anyone with an idea for an app on-chain, from someone who’s never written a line of code to a working developer who just hasn’t touched this particular chain before. What you need is an AI coding agent (Claude Code is a common choice, though any capable one works) and a little patience rather than prior blockchain knowledge.


**What you’ll end up with:** a working smart contract and a simple web app that talks to it. An AI agent builds it. You test it on a practice network first, where nothing costs real money, and you review it in plain English before any of it goes live for real.


**The one core idea to hold onto:** the AI agent doesn’t need to already know Ultra. It learns Ultra by reading a knowledge base written specifically to teach an agent how to build here. Describing the idea and checking the result is your job. The agent does the coding.


---


## Step 0: Before you start


You need three things:


1. **An idea.** Something you want an app to do. It can be as small as “let people post short messages that everyone can see.” It can also be a full game or a small marketplace, though starting small makes the review step in Step 4 easier, and you can always build on something that already works.
2. **An AI coding agent.** Claude Code is a common choice, but any agent that can read files, write code, and run commands on your computer (or in a sandboxed environment) will work. You don’t need a specific one.
3. **A working container tool (Docker).** The knowledge base’s setup path is built around a ready-made development image, so you don’t need to install a blockchain toolchain by hand. Ask your agent to check this is installed and running before anything else. If it isn’t, that’s the very first thing to fix, before any other step.


You do **not** need: existing code, a wallet with funds, or any prior blockchain knowledge. All of that comes later, and only once, at the point where it’s needed.


---


## Step 1: Write your brief


Before you talk to your agent, write down what you want in plain English. A good brief answers:


- **What does the app do?** (one or two sentences)
- **What actions can a user take?** (e.g. “post a message”, “send a tip”, “cast a vote”)
- **What does the app remember?** (e.g. “the last 50 messages”, “each person’s vote”)
- **Does it involve money or anything valuable?** (transferring tokens, buying something, holding a balance). If yes, say so clearly, since this changes how carefully the agent needs to test it.
- **Who can do what?** (can anyone post, or only certain people? can anything be undone or removed, and by whom?)


You don’t need technical language. “Let people send each other a UOS tip with a short note attached” is a usable brief.


---


## Step 2: Point your agent at the knowledge base


Give your agent the knowledge base and ask it to read the top-level guide first. A prompt like this works well:


> “Read the README of the Ultra agent knowledge base I’ve given you, then build me a dapp that \[paste your brief\]. Work on a local practice network first, don’t touch any real network until I explicitly tell you to.”


The knowledge base itself is broken into short, focused guides rather than one long document. One covers how the chain works, start to finish. A few walk through setting up a contract and testing it against a real local chain. There’s a section on the wallet, another on reading chain data once something is live, and a full worked example that goes from nothing to a working tip jar. The last one is a running list of mistakes people have already made, so you don’t have to make them again. Your agent doesn’t need to read all of it up front. The guide tells it which parts matter for which task, and it pulls in the rest as it goes.


---


## Step 3: Let it build and test on a local network


A good agent will, at this stage:


- Set up the toolchain (checking Docker, in particular, since that’s the one thing it can’t work around).
- Write the smart contract for your idea.
- Spin up a **local, private, practice blockchain** : a throwaway network that exists only on your machine, with fake tokens that have no real value.
- Write and run automated tests against that practice network, including tests that check the app correctly *rejects* bad or malicious input, not just that it works when used correctly.
- Build the simple web app that talks to the contract.


Nothing here touches real money or a real network. If your idea involves transferring value between users, make sure the agent’s tests specifically check that people can’t underpay, double-spend, or trick the app with a fake token. Ask it directly: “show me the test where someone tries to cheat this, and show me it fails.” A capable agent should be able to produce this without difficulty; if it can’t explain what it tested for, that’s a sign to slow down.


---


## Step 4: Review it in plain English


Before doing anything else, have a conversation with your agent, don’t just take “done” for an answer. Ask it to walk you through, in plain language:


- What every user-facing action does, in one sentence each.
- What happens to anything of value (tokens): where it goes, who can move it, and whether there’s any path for someone other than the rightful owner to take it.
- What it explicitly tested against, including the “someone tries to cheat” cases above.
- Whether there’s any admin or special account, and exactly what power it has (nothing should let an admin silently take user funds).
- Anything it wasn’t sure about, or any shortcut it took that you should know about.


If any answer is vague, ask again until it isn’t. You’re the reviewer here. The agent did the building. Whether it’s good enough to trust with real money is a decision only you can make.


---


## Step 5: Get ready for the mainnet


This is the point where things become real, so slow down here.


You’ll need a wallet if you don’t already have one. Ultra Wallet is available as a[Chrome extension](https://chromewebstore.google.com/detail/ultra-wallet/kjjebdkfeagdoogagbhepmbimaphnfln) . You’ll also need a small amount of UOS in it. Part of that covers the deployment transaction. More of it usually goes toward RAM, the storage space your contract reserves on the network to hold its code and data. The wallet has a built-in way to acquire UOS directly. If you already hold UOS on Ethereum,[bridge.ultra.io](https://bridge.ultra.io/) moves it over to the native chain. Either way, you end up with UOS sitting in a real wallet, which is what the two rules below exist to protect.


Two rules that don’t bend, no matter how helpful your agent seems:


1. **Never let your agent read, print, display, or transmit a real private key.** A private key is the one thing that can never be undone if it leaks: anyone who has it can take everything tied to that account, permanently. Getting a new one, funding an account, and typing or pasting the key into a signing step are things *you* do, by hand, outside the conversation with your agent.
2. **Never let a deployment to a real network happen without you personally confirming it first.** Before anything is signed and broadcast for real, make your agent tell you (out loud or in writing) exactly which account it’s deploying to, exactly what it’s about to deploy, and roughly what it will cost. Confirm all of that yourself before the signing step runs. This step is real money and, once broadcast, can’t be undone.


A well-built process will naturally pause for you here and hand you the actual signing step to run yourself. If your agent tries to skip past this and do the real signing on your behalf, stop and don’t proceed until it doesn’t.


---


## Step 6: Deploy to the Mainnet


Once you’ve confirmed the details from Step 5, the deployment itself is mechanical: create or use a real account, fund it with enough UOS to cover RAM and the deployment transaction, and run the signing step yourself, with your own key, having already confirmed exactly what it’s deploying.


After that, your agent should verify the deployment worked, reading back from the live network to confirm the contract is there and behaves as expected, before telling you it’s done.


If your app has a web front end, the last piece is hosting it somewhere your users can reach it. This is a normal web-hosting step and doesn’t involve anything blockchain-specific.


---


## Step 7: Share what you built


Once it’s live, tell people what it does. Tell them what it can’t do too, starting with the fact that it will never ask for a private key, and point to where they can check for themselves that what’s running on-chain matches what you’re telling them. A short, honest note like this costs you very little, and it’s a fair thing for a user to expect from something built this way.


---


## Glossary


**Smart contract:** the program that runs on the blockchain and enforces the app’s rules; once deployed, it does exactly what it says, nothing more.


**Local network:** a private copy of the blockchain that runs only on your machine, for testing, with tokens that have no real value.


**Mainnet:** the live public Ultra network, where actions cost real resources and mistakes can be permanent.


**Wallet:** the tool (browser extension) a user connects to sign actions with their own key; your app should never ask to see or hold that key itself.


**Private key:** the secret that proves ownership of an account. Anyone who has it controls the account, permanently. Never share it, never let an AI agent read it.


**Deploying:** publishing your smart contract onto the network so it runs and can be used.


**RAM:** the storage space a contract reserves on the network to hold its code and data. Buying enough of it is part of the cost of deploying, separate from the deployment transaction itself.


## What to try next


Once your first app is live, adding new features (more actions, more app-specific data, a second contract that talks to the first) means running through the same process again, and it goes faster the second time. The review habits in Step 4 and the two rules in Step 5 still apply every time.


This is the same process behind a demo we’ll be showing soon. If you build something first, we’d like to see it.
