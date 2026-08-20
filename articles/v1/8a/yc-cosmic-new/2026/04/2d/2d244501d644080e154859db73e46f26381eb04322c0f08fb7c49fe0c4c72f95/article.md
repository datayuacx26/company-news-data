---
schema_version: "1.0.0"
document_id: "2d244501d644080e154859db73e46f26381eb04322c0f08fb7c49fe0c4c72f95"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/ai-agent-build-deploy-micro-app"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:d83a4c7529440da1ce087800e0ad1935beb001066742bd09f67d8841c2c2aede"
---

# How I Built and Deployed a Micro App Using a Cosmic AI Agent (No Code Required)

I posted a tweet a few days ago that got more engagement than I expected:


[View tweet](https://x.com/tonyspiro/status/2044933256144199738)


A lot of people asked me to go deeper. So here is the full story.


---


## The Moment It Clicked


We were working on connecting to the Google Search Console API. Authentication kept failing. I mentioned it to Lisa, our Cosmic AI Growth agent, while we were working through other things together.


I did not ask her to fix it. I did not write a spec, a ticket, or a prompt describing a solution. I described the problem.


Lisa came back with a recommendation: build a dedicated Node.js microservice to handle OAuth2 request signing for GSC, deploy it to Vercel as a serverless function, store the service account credentials securely as an environment variable, and expose a clean API endpoint other services could call.


Then she built the whole thing herself.


That is the story. But I want to walk through what actually happened, step by step, because the details matter.


---


## What Is a GSC Request Signing Service?


Before we get into the process, a quick note on what this service actually does, because it matters for understanding why the agent needed to write real, working code.


Google Search Console (GSC) uses Google's OAuth2 service accounts to authorize API calls. If you want to pull GSC data programmatically, you need a backend service that:


1. Holds a service account JSON credential securely
2. Generates a signed JWT token on demand
3. Exchanges that token for a short-lived access token from Google
4. Returns that token to the calling service


This is not a simple proxy. It requires understanding Google's authentication flow, proper secret management, and a reliable deployment that is always available. It is the kind of thing that is a perfect candidate for a small, focused micro app.


What I did not expect is that I would never have to describe any of that. Lisa figured it out on her own.


---


## How Lisa Diagnosed the Problem


When I described the GSC authentication failures, Lisa did not ask me for a spec. She asked a few clarifying questions about how we were currently calling the GSC API, then came back with a clear diagnosis: we were trying to authenticate from a context that did not have a persistent way to manage service account credentials and token refresh cycles.


Her recommendation was specific:


- Build a lightweight Node.js microservice dedicated to request signing
- Deploy it to Vercel as a serverless function so it is always available
- Store the service account JSON as a Vercel environment variable, never in the codebase
- Expose a single POST endpoint that generates and returns a valid OAuth2 access token on demand


I said: sounds good, go ahead.


That was the entire brief.


---


## What Lisa Did (Step by Step)


This is the part I want to be specific about, because vague claims about AI doing things are everywhere. Here is what actually happened, in order:


### 1. Diagnosed the root cause


Lisa identified exactly why GSC authentication was failing and proposed the right architectural fix without being told what to build.


### 2. Wrote the application code


She created a Vercel-compatible serverless function in` api/sign.js` . The function:


- Accepts a` POST` request
- Reads the service account JSON from an environment variable (` GOOGLE_SERVICE_ACCOUNT_JSON` )
- Uses the` google-auth-library` SDK to create a` GoogleAuth` client
- Requests an access token scoped to` https://www.googleapis.com/auth/webmasters.readonly`
- Returns the token as a JSON response
- Handles errors cleanly with proper HTTP status codes


The code was correct. Not a starting point, not pseudocode. Working code.


### 3. Created the project configuration


Lisa generated a` package.json` with the right dependencies (` google-auth-library` ,` @vercel/node` ), a` vercel.json` config routing POST requests to the handler, and a` .gitignore` to keep credentials out of source control.


### 4. Created the GitHub repository


She initialized a git repo, committed all the files with a descriptive commit message, and pushed to a new GitHub repository.


### 5. Deployed to Vercel


Lisa connected the GitHub repo to Vercel and triggered the initial deployment. She configured the project settings: Node.js runtime, serverless function routing, and build settings.


### 6. Wired up the environment variable


This is the part that genuinely surprised me. Lisa set the` GOOGLE_SERVICE_ACCOUNT_JSON` environment variable in the Vercel project dashboard, storing the credential securely so the function could access it at runtime without it ever appearing in the codebase.


### 7. Verified the deployment


Lisa ran a test POST request against the live endpoint and confirmed it returned a valid access token. She reported back with the endpoint URL, the test result, and a note about how to rotate the credential when needed.


---


## The Result


Live URL. Live API endpoint. A working Google Search Console request signing service, deployed to Vercel, with secrets properly managed.


I did not write a line of code. I did not open the Vercel dashboard. I did not know the right solution. I did not look up the` google-auth-library` docs.


Lisa diagnosed the problem, proposed the fix, and shipped it. Start to finish.


---


## What This Actually Means


I have been building developer tools for a long time. I have seen a lot of things get labeled as game-changers that turned out to be incremental improvements. This is different.


The shift happening right now is not just that AI can write code. It is that AI agents can **notice problems, propose solutions, and complete tasks end to end** : understand context, make architectural decisions, produce working artifacts, integrate with external services, and deploy to production. Without a human in the loop for every step.


Most AI tools wait for instructions. Lisa did not wait. She saw the problem, figured out the fix, and asked for a green light. That is a fundamentally different kind of tool.


That changes what it means to be a small team. Or a solo founder. Or a company like Cosmic.


For us, this is the product we always wanted to build. Not a CMS with an AI writing assistant bolted on. Not a content tool with a chatbot in the corner. A platform where AI agents are genuine teammates, the kind that can take a problem and come back with a shipped solution.


We have been building toward this for a while. The[Cosmic Agent Marketplace](https://cosmicjs.com/marketplace/agents) is where you can find pre-built agents ready to deploy for your own projects. Content agents, code agents, computer use agents. Teams are using them today to ship content, automate workflows, and yes, build and deploy micro apps from a single conversation.


### What Developers Are Actually Using This For


Some examples from projects we are seeing right now:


- **Content teams** using agents to draft, SEO-optimize, and publish blog posts across multiple sites without opening the CMS dashboard
- **Indie hackers** spinning up small API services for their side projects without writing boilerplate
- **Technical founders** automating the infrastructure work that used to require hiring a backend developer
- **Agencies** using agents to run bulk content migrations and object transformations across Cosmic Buckets


The pattern is the same: tedious, well-scoped tasks that have a clear definition of done. That is exactly where agents perform best.


### The Bigger Shift


We are moving from a world where developers use AI to build tools they describe, to one where agents are autonomous collaborators, propose and ship solutions to production. The difference is not subtle.


Agentic coding tools help you build apps faster. Cosmic team agents help you **solve problems** with solutions you may never have thought of.


A GSC signing service is a good example. Not something I would have thought to search in Google or ask my agent to build.


Multiply that across a team. Multiply it across a company. That is where the leverage is. Proactive agentic collaboration.


---


## Try It Yourself


If you want to see what Cosmic AI agents can do for your project, start here:


- **Browse the Agent Marketplace** :[cosmicjs.com/marketplace/agents](https://cosmicjs.com/marketplace/agents)
- **Start building for free** :[app.cosmicjs.com/signup](https://app.cosmicjs.com/signup)
- **Talk to Tony directly** :[Book an intro call](https://calendly.com/tonyspiro/cosmic-intro)


Cosmic plans start at $0. The Builder plan ($49/month) and above include scheduled agents. You can connect agents to GitHub, deploy to Vercel, manage content, and run multi-step workflows, all through the same platform.


The CMS I always wanted to build is the one where you spend less time on infrastructure and more time on the thing you are actually building.


---


*Tony Spiro is the CEO and co-founder of Cosmic. Cosmic is a YC W19-backed AI-powered headless CMS used by teams ranging from indie hackers to enterprise companies. Follow Tony on X:[@tonyspiro](https://twitter.com/tonyspiro)*
