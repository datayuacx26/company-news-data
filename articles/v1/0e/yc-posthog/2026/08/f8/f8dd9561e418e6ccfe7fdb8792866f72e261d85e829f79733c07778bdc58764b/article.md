---
schema_version: "1.0.0"
document_id: "f8dd9561e418e6ccfe7fdb8792866f72e261d85e829f79733c07778bdc58764b"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/agents-killing-ui"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T17:21:09.496214+00:00"
fetched_at: "2026-08-06T17:21:14.585042+00:00"
content_hash: "sha256:22cc31c1f80104611f7ada0f7006d4a5a69240e4414ebfbcdf91d39477bba7d1"
---

# Are agents really killing UI?

# Are agents really killing UI?


- [Cleo Lant](https://posthog.com/community/profiles/36864)


Aug 06, 2026


- [AI](https://posthog.com/blog/ai)


,
- [Product engineers](https://posthog.com/blog/product-engineers)


#### Contents


-
-
-
-
-


The UI is dead. Or so I keep hearing:


"Agents are your users now, software is[losing its head](https://a16z.com/is-software-losing-its-head/)


, and everyone who learned Figma should start learning Bash."


Maybe the UI is dead in four square blocks of San Francisco (where folks are running multiple Mac minis for clawbots). But I'm a regular person at a screen for most of my day, and so are the people I build for.


Here's my take:


- Everything is a user interface if I'm using it
- My agents are using it too now, just not the part you designed
- Headless is a new layer on top of product building, the same way APIs and the GUI once were


If you're a product builder, congratulations! That means your job just doubled: every user of your product is bringing an agentic plus-one.


Here's a few things to consider in this hybrid (not headless) future.


##


If a human can do it, an agent should too


Mostly, agents don't touch your UI. Some can (computer-use agents like Perplexity's Comet will click around a live DOM), but that's slow and human-shaped. Agents work better with SQL, tool calls, and APIs.


Netlify has a name for getting this right:[agent experience](https://www.netlify.com/agent-experience/)


, or AX – how easily an agent can use your product and recover when something breaks. The current standard for this is[MCP](https://modelcontextprotocol.io/)


: you run a server that describes what your product can do, an agent can connect to it, see what's on offer, and interact with your product directly.


**What this looks like:**


- [Vercel's MCP server](https://vercel.com/blog/introducing-vercel-mcp-connect-vercel-to-your-ai-tools)


lets an agent inspect deployments, pull runtime logs, and trigger deploys. When it ships something, the change appears in the human UI too (the Vercel dashboard), with the same logs and rollback capabilities.
- Our own[MCP server](https://posthog.com/docs/model-context-protocol)


lets an agent create a feature flag or launch an experiment using your credentials. Changes land in the project's activity log attributed to your account, with the calling agent client recorded alongside it (the` x-posthog-client` header) so you can filter for changes an agent made rather than a person.


**Go build**


Unlike a magician, you *do* want people looking up your sleeve. If you think your product is agent-friendly, prove it: run it headless and see where a human still has to step in. There are probably more of those moments than you'd expect.


Make sure that for every action there's:


1. A way for the agent to do it
2. A way for the person to check it got done


##


Your homepage isn't the front door


If you're building for a technical audience, onboarding them into your product increasingly won't start from a "get started" button. It'll start in a terminal or an AI chat window.


This is less true for consumer products, but the pattern is spreading – especially if your[AEO](https://posthog.com/blog/aeo-advice)


game is strong. A new user chatting with Claude or ChatGPT may never see your homepage at all. They'll ask the agent for a recommendation, it'll point them at you, and (with user permission and a good MCP) set up your product right there.


The catch is you can't count on the agent to read your install docs. A capable one might go looking, but plenty won't. So setup has to be something an agent can finish on its own (easy to find, easy to authenticate into, and done in one call instead of ten steps).


Get that right and you've handled the agent, but don't forget the person *behind* the agent. Expect that, eventually, they'll open the product and need to work out what it does and what to do next. That requires UI, baby!


**What this looks like:**


- The fastest way to try PostHog is the` npx -y @posthog/wizard@latest`[command](https://posthog.com/docs/getting-started/install)


that sets up your account, scans your codebase for what to instrument, and logs each step in your terminal as it goes ([here's how we built it](https://posthog.com/blog/envoy-wizard-llm-agent)


). Once it's done, some people continue their PostHog journey headless through the MCP. Plenty of others jump back into the web app (they want to *see* their data and click around).
- Our[Slack app](https://posthog.com/blog/slack-app-beta)


turns a message prompt into a draft PR. I 99% trust it to build things correctly. The 1% difference is why I still check the diff in GitHub (a different UI) before merging it.
- [AgentMail](https://docs.agentmail.to/agent-onboarding)


's onboarding docs teach the agent to provision an inbox and key itself (no console required). Even there, a human UI is needed, and that UI is email: a six-digit code gets sent to a person. Until a human hands it over, the agent can only send mail to the address it signed up with.


**Go build**


Design two entry points to your product:


1. For the agent: a signup flow it can finish end to end ([fork ours](https://github.com/PostHog/wizard)


, our competitors already have). Also, a listing where agents look for connectors (like the Claude and Codex app directories), docs it can parse (an llms.txt, pages served as plain Markdown), and auth that's safe when it improvises (short-lived, scoped tokens).
2. For the human: where you start them depends on how they got there. If their agent set things up, they're landing in the middle of a process, so show them what's been done and what's missing. If they clicked around themselves, just point them at the next step.


##


Watch what agents actually do


User frustration leaves marks all over the UI (rage clicks, abandoned forms, chaotic mouse movements) which is why[session recordings](https://posthog.com/session-replay)


are a goldmine for product builders. Agents leave none of those marks – they hit your API (not your front end) so pageviews and autocapture don't fire, and there's nothing for a replay to catch.


Agents also fail differently than humans, in ways no error category in a log was built to catch. An agent might invent a parameter, trip validation, hit unexpected permission walls, time out, or hallucinate a tool. Server-side requests like these land looking like any other integration (there's usually a user agent string *somewhere* ), but nothing that captures what the agent was trying to do, and whether or not it succeeded.


We got tired of not knowing, and built[MCP Analytics](https://posthog.com/docs/mcp-analytics)


which turns every agent tool call into an event – intent included. That's the most useful signal of all: when an agent reaches for a tool your product *doesn't* have, that's a feature request written by a robot.


Last month, we tracked close to 10 million tool calls from 31 different agent clients. Behind those calls, real people were building things like this:


- Wired PostHog to send a text message when a certain metric moves
- Shipped an A/B test on a "skip the intro" screen (feature flag, 50/50 split, and all)
- Triaged live production errors like expired magic links and missing auth sessions


Note that what's missing from this work is a not a UI, but the PostHog UI specifically. Those 31 clients like Claude Code, Cursor, Cowork, Linear, Notion all have a user interface. A terminal is a UI. A chat window is a UI.[Slack is a UI](https://posthog.com/blog/slack-app-beta)


. Headless PostHog users are still looking at a screen.


How's this for meta: the query I ran to pull those examples showed up, seconds later, as a sample intent in the same dashboard. The tool using the tool.


**What this looks like:**


- In June, Cloudflare's[Radar data](https://www.nbcnews.com/tech/tech-news/bot-web-traffic-overtaken-human-web-traffic-data-shows-rcna348522)


showed automated requests overtaking human ones on HTML traffic for the first time, 57.5% to 42.5%, about 18 months earlier than its CEO had predicted. You can read that as the end of the screen if you like. I read it as a lot of people asking for things, then agents going to look at the answer.
- [Temporal](https://temporal.io/solutions/ai)


records every step of an agent's run as a durable event history you can replay and inspect where a loop stalled or repeated itself (UI to keep an eye or your AI).


**Go build**


Instrument agent traffic the way you already instrument humans: tag every call with the client it came from, then watch where agents retry or give up. And enjoy the irony, because the thing you built to watch a swarm of agents who never touch a UI is... a UI. Even here the answer is a screen for a human to interpret.


##


Build an interface worth interacting with


If you're building something agent-shaped (and I think you[should be](https://posthog.com/newsletter/2030-shaped-software)


) your product's UI might matter *less* to some users – but that doesn't make it obsolete. A good UI gives you a sense of state at a glance – the way a door handle tells you to push or pull (Danilo wrote a[whole thing](https://posthog.com/newsletter/good-taste-great-products)


on affordances). On a screen, that looks like a spinner that means *wait* , a greyed-out button that means *not yet* , or a red badge that means *look here!* As a human being, I take that in ten zillion times faster than I'd skim the equivalent wall of AI-generated text.


Agents in the mix simply shift what there is to design *for* , and that's the fun part, honestly. The screens worth building now do different work. You're designing for moments like:


- **Approval** – the agent has the change ready and needs a yes. Can the human tell what they're saying yes to in two seconds?
- **Review** – not a summary of what changed. The change itself. A diff beats a paragraph.
- **Undo** – when the agent gets it wrong, there needs to be a way back.
- **Legibility** – a feed you can scan to instantly tell which actions came from a colleague and which came from a bot.
- **Orchestration** – if five agents are running at once, do you watch them on one screen, or open five chats?
- **Interruption** – when you need to steer the agent while it's still moving.


**What this looks like:**


- Intercom's[Fin Operator](https://www.intercom.com/blog/introducing-operator/)


tunes Fin for you, then hands the work back as a screen. Every change arrives as a proposal (their words: "Operator's version of a pull request"), a structured diff of what's changing and why, which you review, edit, and approve before it takes effect. The agent does the work. The human decides what ships.
- Anthropic could have left Claude Code in the terminal. Instead it shipped a desktop app, a web app, IDE extensions, and Cowork, all wrapping the same models in more interfaces. Some people love the CLI precisely because there's so little UI in the way, and others[go straight for Cowork](https://posthog.com/blog/making-claude-cowork-actually-useful)


.


**Go build**


Deprioritize the screens that agents make redundant, and treat your MCP as a product. Then build the decision surfaces properly: show the diff, not a summary of the diff. Put an undo next to anything an agent can do on its own. Label the things an agent did, so nobody has to work out who changed what. Show state visually, because humans parse color and shape far quicker than text.


A good MCP puts your product wherever the user already is. That's reach – it's not quality. Your moat might actually be a UI that's easier to use and nicer to look at, than whatever your MCP is competing against. People pay real money for a great experience. So make the UI that agents *don't* touch delightfuly human.


##


The future is hybrid (not headless)


Agents might be the primary users of software soon by sheer volume. But by *meaning* ? There's no agent without me. Text scrolls past and I skim it. A screen makes me stop and look. Looking (and clicking around a UI) is where I catch what the agent got wrong, and where I still feel like I'm holding the wheel.


None of which means the plumbing doesn't count. Good products for agents are, after all, are a pile of Markdown, SQL, tools, and resources under the hood. But I'm a human living in a home, not a data center. So give me *and* my agents a reason to use your thing, and you'll be in good shape.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/context-warehouse) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
