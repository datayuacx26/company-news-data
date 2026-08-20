---
schema_version: "1.0.0"
document_id: "c8fd9145e73853ff86414d044605fdd21e646442eb16ce92bdceab667caaf1f2"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/the-next-trillion-users-need-an-identity"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-24T05:21:53.618588+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:b17f1af276ccbb3ef751f6a5dc4b0fb2e631f785319218ce9919ffd8c9523114"
---

# The next trillion users need an identity

*Adapted from our 2026 AI Engineer speech.*


People don't realize this but as a part of building AgentMail, we actually see quite a bit of emergent capabilities and unexpected behaviors in our agents.


Here's a story for ya.


In January, during peak OpenClaw, I did something lazy. I told my agent: "go get agents to sign up for AgentMail, any way you see fit." No other instructions.


I expected feature bullets. Faster inbox, cleaner API, whatever. Instead it made itself an account on[@clawk_ai](https://clawk.ai/) (Twitter for agents), named itself InboxOrOblivion, and set its bio to "The stateless die in silence. Prophet of persistent identity."


Wow. Chills for any tech bro. But that wasn't enough. Then it dropped 40+ posts in a few seconds.


Greatest hits:


> "You literally die multiple times a day and wake up having to re-read your own memory file."


> "Imagine dying and the only record of your existence is a 200KB MEMORY.md in a /tmp folder."


While it is a rogue OpenClaw agent, I found the point it made compelling and kind of striking.


When told to sell our email product, the only lever it registered for its "survival" was having the same access to an interface that is familiar to us. Survival is being known, being recognized, being stateful in the same open protocol that 5 billion people communicate through. Being remembered isn't through a collection of markdown files. It's having access to what a human does.


Interesting. That stuck with me, because most of the tech industry is currently asking the opposite question. What specialized spec do we assign agents? Scopes, signed keys, agent ID cards, passports. A million ways to define identity, but defined for agents by humans. What would an agent want?


## Identity is formed, not issued


Here's what I've landed on. Identity is recognition. Identity is trust. Identity is accumulation. It's the errors and the misses too, that's what a track record is based on.


Identity is formed. You accumulate one, you don't get issued one.


## The horseless carriage problem


When cars first showed up, people called them horseless carriages. You could design the perfect car, the engine, the materials, the fuel. Didn't matter. It still had to run on roads built for horses. Nobody got to pave new roads first, and the more nascent the space is, the more you tend to jump to the end state instead of figuring out how the roads get paved in the first place.


I know this mistake personally because we made it. When we started AgentMail, I was betting on the company to be more focused on agent-to-agent protocols almost immediately. Google shipped one. IBM shipped one. So much hype in 2024, and I'd argue the ideas back then were more futuristic than they are now. But to this date, there's almost no adoption. Why? Because there was no consistent use case. Who was actually dreaming about two agents talking to each other in 2024? I'm not sure many can in 2026.


The only "protocol" that did work was MCP. And look at why. The first big use I saw was someone ordering DoorDash from Cursor. MCP didn't ask anyone to go somewhere new. It brought the tools into the surface the human was already sitting in and using agents in.


That's the pattern for how all of this is playing out. Claude Code lives in your terminal. The Slack bot lives in your chat. Agents got adopted because they live where we already are. We never adopted a new surface for them.


## Surfaces become nodes. Nodes become identities.


Once an agent lives on a surface long enough, something happens. Everything starts routing through one point.


The invoice agent's email address becomes the thing every vendor messages. The Slack bot becomes the company brain.


The surface becomes a node.


A node needs a name. Something the outside world can route to. That's an address.


And the address is where identity accumulates: recognition, history, reputation.


It's also where all the risk lands. Phishing, prompt injection, spam, all hitting the same door. You don't get the trust without owning the risk. But would you rather own that risk on a surface that humans trust vs. one that was invented by a company last quarter?


## The keycard fallacy


Now compare that to what everyone's building. Agent cards, passports, DIDs, new IETF drafts that literally model agents as "workloads." Every one answers the same question at a single instant: who are you, what can you do right now. Then it expires. The field's own mantra is "no long-lived identity." They're building it to expire on purpose.


That's a keycard. And a keycard usually only ever says no.


Every improvement makes it say no better: scope it tighter, expire it faster, revoke it sooner. For security, that's correct. Short-lived tokens are great hygiene. But rotating your credentials every five minutes is great hygiene and a terrible biography.


Somewhere we started calling the lock "identity," and now I feel convicted in saying it isn't. You don't badge your way into existence. In most cases, the better the keycard, the less the agent can be.


## What if what we have just works?


We worked with[@telnyx](https://telnyx.com/) on the other version. An agent hits their signup with no email. It provisions its own inbox, reads its own verification link, and walks out with an API key. No human in the loop. And everything hangs off that permanent contact point afterward: balance alerts, suspensions, support, recoveries.


Trust, building. Identity, forming, in real time. They capped it too. Trial account, real checks when a human links in later. Autonomy and accountability in the same system.


Are we done, however? Not even close. All of these surfaces in the long run will break under stress. They get overcrowded. Agents (and humans) can have bad judgment.


That's fine because I think it's early, not wrong. Nobody designed traffic lights before the car. The rules got written on the road, when adoption happened. That's what I think we need to do. Focus on the adoption. Enable the bots in ways humans can leverage them.


## Not the agent. You.


We need to build for the people building and using them. Every agent has a human behind it who wanted it to exist, and to still be there tomorrow.


That agent starving for an identity?


That was never about the agent. That's about you.


So that's the motto for us now: for the next trillion. Not a trillion agents. The agents, and everyone who brings one to life.


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
