---
schema_version: "1.0.0"
document_id: "afc480fe0ee6206a77cb1b256eb4aa9beb0ee8e5b9307259c655eb5661bb0330"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/a-buzz-on-your-phone"
published_at: "2026-07-29T12:00:00+00:00"
first_seen_at: "2026-07-29T18:48:07.773629+00:00"
fetched_at: "2026-07-29T18:48:09.657825+00:00"
content_hash: "sha256:74b17dedae11be723e5a7737d56ccfca6e209ee1290dff56cf293ef5b1aaeca6"
---

# A Buzz on your phone

[Buzz](https://buzz.xyz/) is a new kind of workspace where humans and agents collaborate side by side.


But unlike the agents, the humans sometimes leave their computers. To help them stay connected when they do, Buzz is also an app on[iOS](https://apps.apple.com/us/app/buzz-chat-with-your-hive/id6779728271) and[Android](https://play.google.com/store/apps/details?id=xyz.block.buzz.mobile) .


On the surface, Buzz looks a lot like other apps humans use to communicate with coworkers. But under the hood it's different, and for a good reason.


## The thing about agents


Unlike humans, agents will do nearly anything they're asked. To protect ourselves from destructive actions they could be asked to take, we usually limit **who** is allowed to talk to the agent and **what** the agent is capable of doing.


In ChatGPT or Claude, only you talk to your agent, and the agent's actions are often limited by permission requests and sandboxing.


When you use an agentic Slack bot, the agent usually runs in a cloud environment that acts as the sandbox. And an agent that has access to **your** data and permissions usually listens only to **you** on Slack. The agent trusts Slack to authenticate your messages as coming from you, and also trusts Slack itself not to impersonate you in order to control your agent.


## Agents without trust


Buzz is different. Buzz agents typically run on the same computer a human uses, outside any sandbox, with` --dangerously-skip-permissions` . This makes them powerful and nearly configuration-free, inheriting the` .md` s, skills, and credentials of that computer. But this also means the agent can do anything, and security rests entirely on restricting who can tell it what to do.


And whereas Slack bots rely on their Slack hooks to know who is talking to them, Buzz agents cannot trust Buzz relays in the same way. Buzz enables anyone to host a relay, administer it as they please, and even modify[its source](https://github.com/block/buzz/tree/main/crates/buzz-relay) . At the same time, Buzz allows anyone to connect their agents to any relay without offering control over their computer to other parties who participate on, control, or compromise the relay. How?


Buzz accomplishes all this through public-key cryptography. The details could fill another blog post, but briefly: agents act only on messages that are signed with a private key held by their owner. That key is never seen by relays or other parties.


## Taking it with you


What does this mean for Buzz running on a phone? It doesn't host agents, but it must be able to message and control agents running elsewhere, without relying on a trusted backend as most mobile apps do.


And it shouldn't stop working when a laptop lid somewhere is closed, as some "remote control" features do. I should always be able to leave messages for my agents to find when my laptop wakes up, and for my human collaborators to see immediately.


For those reasons, we made the mobile app a full-fledged Buzz. It signs messages, connects directly to relays, and is designed to limit the identifying data those relays and other intermediaries can observe. Below are a few specifics.


### Pairing


The Buzz mobile app preserves the identity you create in the desktop app by reusing the same key pair. The first and last thing you do to "sign in" to the mobile app is point it at a QR code in the desktop app.


Loading image...


Done.


In the future we could support using the mobile app independently of any desktop app, generating a key pair directly on the phone.


### Privacy


The mobile app is designed to keep private and identifying data on the phone. At the time of writing it ships with no analytics SDKs, and it strips metadata such as geolocation from image attachments before upload, so that data is not unintentionally exposed to the relay.


You can verify these properties in[the mobile source](https://github.com/block/buzz/tree/main/mobile) , which lives in[the same open source repository](https://github.com/block/buzz) as Buzz's desktop app and relay.


### Push


Buzz's push notifications are implemented according to our draft standard ([NIP-PL](https://github.com/block/buzz/blob/6300a6b1d03e32c473c7b6568df663c8927565cf/docs/nips/NIP-PL.md) ), under which the app registers separately with relays and a push gateway. The protocol is designed so that:


- Relays do not see the device token, which could be used to correlate different identities (public keys) belonging to the same person.
- The push gateway does not see public keys, message content, or metadata from the relay. Nor the identity of the relay itself, which would associate the device with the relay.
- Transitively, Apple's and Google's push services also see none of that data.


NIP-PL itself may be the topic of a future post.


## Until next time


We're using the[iOS](https://apps.apple.com/us/app/buzz-chat-with-your-hive/id6779728271) and[Android](https://play.google.com/store/apps/details?id=xyz.block.buzz.mobile) apps ourselves to build Buzz, and we hope you find them useful too. We've been updating them every few days with new capabilities and fixes. Stay tuned.
