---
schema_version: "1.0.0"
document_id: "d5c42db3e235fdb7e120b5e41f314f0bb5b07b5f3d378128182be01d17916e3b"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2026-06-16-towards-search-memory-and-more"
published_at: "2026-06-16T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:e25fcce273dfa3bb8529e5780ee92b635b915b6cb3cb7033695913e2d3a2bf39"
---

# Towards Search, Memory, and More

[← Back to Posts](https://tinfoil.sh/blog)


# Towards Search, Memory, and More


Jun 16, 2026


•


8 min read


Sacha Servan-Schreiber


We're excited to announce two features on our roadmap for Tinfoil Chat: semantic search and memory across chats. In this post, we share some of the behind-the-scenes engineering work surrounding backup and sync that is currently underway to make these features possible.


**Semantic search.** The first feature we're working on is semantic search across your chat history. Under our[original sync system](https://tinfoil.sh/blog/2025-09-24-private-chat-backups-local-first) , every chat was encrypted with a key that lived on your device, so we couldn't do the obvious thing and index your conversations on our servers — the data was encrypted, making it impossible for us to query over it. We've since deployed an improved backup and sync architecture that makes it possible to index and search your chats inside a secure enclave, while fully preserving the privacy guarantees you already have when you choose Tinfoil.


**Memory.** The second feature on our roadmap is memory. Memory across chats — and especially across chats within a project — can be thought of as an extension of semantic search. It's one of the most requested features from our users, but one that required careful planning and engineering to keep it private, secure, and performant.


Both capabilities will meaningfully improve the experience of our chat applications, and we're excited to bring them to Tinfoil. Below, we explain the infrastructure we've built to support more powerful capabilities in Tinfoil Chat.


## Extending the Enclave Model


In our[first blog post on sync](https://tinfoil.sh/blog/2025-09-24-private-chat-backups-local-first) , we described our approach to chat backups and sync using client-side encryption. Client-side encryption is a great paradigm. It's easy to understand and has a very simple trust model: data only leaves your device encrypted. Unfortunately, this also means that all processing of your data can only happen on your device. Some workloads, like running a frontier AI model, are far too heavy to run locally, so this would put the best models out of reach. To get around this limitation, our inference stack, and the services surrounding it, use secure enclaves to extend the trusted processing space from your device to our backend services. These backend services all run inside secure enclaves and power AI inference as well as tools like web search, without exposing your data.


In the same way, we have rearchitected our backup and sync system to use an enclave instead of running on your device. This extends the existing enclave model we already use for inference. Because your data is only ever decrypted inside an attested enclave (hardware that can cryptographically prove which code it is running) whose code you can verify, and which Tinfoil itself cannot peer into, the trust and privacy guarantees are exactly the same as those you already rely on every time you send a prompt to one of our models.


## Re-architecting Encrypted Sync and Backups


Originally, encryption and sync happened entirely on the client. The backend was used only to store encrypted blobs of chats and attachments. The encryption key lived locally in the application (and was synced between your devices using passkeys), which meant Tinfoil services had no way to decrypt the blobs stored in the backend. This model was simple, but it introduced several limitations when it came to adding more features:


- Multi-device sync was prone to race conditions and versioning issues, since each device acted in its own silo and the server saw only opaque blobs it could not merge.
- It made features like search and memory hard to build without maintaining a large index and memory database on the client, something that is especially difficult to do with browsers, which have limited storage.
- It prevented collaborative features such as sharing a conversation with other people unless everyone involved also had your personal encryption key.


These issues sent us back to the drawing board to rearchitect the foundations of chat backup and sync that could support a rich future set of features. The new design had to honor two non-negotiable principles:


1. **Tinfoil should never see your encryption key.** We've put enormous effort into making our inference endpoints fully private; if Tinfoil ever obtained the key used to back up your chats (even if for a brief moment) we'd lose all guarantees we've worked towards.
2. To scale across devices, avoid client-side storage bottlenecks, and give users the best quality of life, we need to do processing in the cloud (inside secure enclaves).


The natural solution was to move the sync logic into a secure enclave. This is a deliberate evolution of our previous design. Whereas backups used to be encrypted entirely on your device, your key is now sent to an attested enclave at sync time. We want to be precise about what that does and doesn't change. Sending your key off your device may sound like a step back from "the key never leaves your device," but as we explain next, it doesn't weaken the privacy guarantee because everything continues to be processed inside secure hardware enclaves.


Crucially, your encryption key is still never exposed to Tinfoil. It is sent only to an attested secure enclave *operated* by Tinfoil, the same environment that already processes every chat you have with a Tinfoil-hosted AI model. Therefore, the threat model and privacy guarantee surrounding your conversations do not change: only you can ever access your private chats.


**Figure 1:** The original client-side sync scheme compared to the new enclave-based architecture. In the new scheme, encryption and processing move from your device into a secure enclave, where processing covers plaintext operations like de-duplicating and reconciling synced chats, building search indices, and distilling memories.


The sync enclave is fully stateless: it manages only sync and encryption logic. First, it verifies that the format and version are correct and that the provided key matches the key expected for your account. The enclave then encrypts your chats and attachments and writes them to secure storage. The storage layer sits outside the enclave boundary, but everything that leaves the enclave is already authenticated and encrypted. Finally, the enclave discards the key and zeroes out all encrypted memory, providing full statelessness between user requests. As with all of our security-critical infrastructure, you can audit all this for yourself by looking at the[open source sync code](https://github.com/tinfoilsh/confidential-sync) .


## New Architecture, New Features


With this architecture now deployed, we can build enclave-based, server-side semantic search by constructing an index over your chat history inside the enclave. When you're not active, these search indices, and other data derived from your chats, are stored, encrypted, in the same place as your chats are now. The way this works is that your device sends your personal encryption key to the attested enclave; the enclave decrypts your chats, builds a semantic search index, and stores that index encrypted under the same personal key used to encrypt the chats themselves. Only you possess the key and only you can recover this information.


In this new system, when you initiate a search for a particular chat in your chat history, your personal encryption key is sent to the attested enclave that decrypts the stored index and retrieves the relevant chats for your query.


Memory works as a natural extension of this architecture. Just as the enclave can build a search index over your history, it can distill durable memories and store them encrypted. When you start a new chat, the relevant memories are retrieved and decrypted inside the enclave, so your assistant benefits from what it has learned without that context ever being exposed to us. At inference time, your request is augmented with your personal encryption key, which allows the model (also running inside the secure enclave) to fetch relevant memories given the conversation context and provide a personalized response.


## Looking to the future


Search, memory, and multi-user collaboration have become important features for AI chat assistants. In many ways they're also the most personal features of AI assistants. Your list of conversations reveals more about you than your browsing history, making such features feel creepy on other platforms. Tinfoil will make it possible to enable such features without asking you to give up ownership of your personal data in return. We are once again proving that choosing between your privacy and talking to powerful AI isn't a tradeoff you need to make. Such features are a stepping stone to our goal of building powerful AI agents that can know everything about you, without third parties knowing everything about you.


As our interactions with AI become more personal and powerful, safeguarding the privacy of your conversations, your thoughts, and your memories becomes both a measure of quality and a way of preserving human relevance. AI should be an exoskeleton that enables humans rather than replaces them, and technical privacy safeguards — encryption and secure hardware that make it impossible for third parties to access your conversations — are a prerequisite for that. We're excited to keep improving our product and appreciate all the feedback and feature requests we've received from our users thus far.


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2026-06-09-auditing-frontier-model-without-weights)[Next Post](https://tinfoil.sh/blog/2026-06-23-confidential-computing-overhead)
