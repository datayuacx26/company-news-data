---
schema_version: "1.0.0"
document_id: "ae89e6631cad055fd7f3d74bafafc4faa7425c2a2434ed0cd3553c19f26fb179"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-09-24-private-chat-backups-local-first"
published_at: "2025-09-24T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:c0e4383cd934af8bb278138534c1168066f85f54447143904e432146dad19e19"
---

# Private Chat Backups with Local-First Principles

[← Back to Posts](https://tinfoil.sh/blog)


# Private Chat Backups with Local-First Principles


Sep 24, 2025


•


7 min read


Sacha Servan-Schreiber


# Introduction


When you have a private conversation with[Tinfoil Chat](https://chat.tinfoil.sh/) , that privacy shouldn't end when you back up your discussion or sync your chats across devices. With traditional cloud services, we've grown used to the seamless experience of backups and automatic syncing. However, it's easy to forget that in doing so, we're trading ownership of our data for convenience. But what if we could have both convenience *and* control of our own data?


Inspired by end-to-end encrypted messaging like Signal and WhatsApp, we've added encrypted chat backups and multi-device syncing to Tinfoil Chat. We believe in the principles of[local-first software](https://www.inkandswitch.com/local-first/) , which advocate for applications where users retain ownership of their data while still enjoying the benefits of cloud computing. Our chat backup and syncing system extends our end-to-end privacy guarantees to persistent cloud storage.


# The Privacy Problem with Server-Side Backups


Cloud applications offer many benefits: seamless synchronization across devices and little to no storage constraints. Yet they come with a cost: your data is accessed, stored, and controlled by the service provider in the process. As such, your data becomes subject to their rules, their security practices, and their privacy policies.


For AI conversations, this trade-off is particularly notable. The chats we have with AI often contain our personal and private thoughts. Incidentally, this is also why your data is so valuable to advertising networks and foundation model providers like OpenAI.


When these conversations are backed up to traditional cloud storage, they are immediately subjected to several security risks:


- **Server-side access** : Your provider can read your stored conversations and train AI models on them. Even if they promise "no logging" and "no training" you don't know what else they might be up to with your data — you have zero control!
- **Breach vulnerability** : Data sitting on servers becomes a target for hackers,[as was recently seen in the Tea app](https://www.bbc.com/news/articles/c7vl57n74pqo) hack.
- **Policy volatility** : Terms of service can change, as[OpenAI users recently discovered](https://arstechnica.com/tech-policy/2025/06/openai-says-court-forcing-it-to-save-all-chatgpt-logs-is-a-privacy-nightmare/) when a court ordered them to preserve all ChatGPT logs — *even deleted conversations.*
- **Service termination** : When a company shuts down, your data can disappear with it, as[23andMe users are learning](https://ag.ny.gov/press-release/2025/attorney-general-james-sues-23andme-protect-new-yorkers-genetic-data) the hard way.


# Our Local-First Approach: You Own Your Data


In line with the local-first philosophy embraced by privacy-focused tech products, we built a backup system where the primary copy of your data lives on *your* device, encrypted with keys that never leave your control. The cloud is then only used to provide synchronization and encrypted backup storage, without ever gaining the ability to access your data.


## Architecture Overview


Our system consists of three components, as illustrated in Figure 1:


**Figure 1:** Architecture overview of Tinfoil's local-first chat backup system showing the relationship between client-side encryption, server orchestration, and cloud storage.


1. **Tinfoil Client** : Handles all encryption/decryption operations locally in your browser or on the mobile app. The keys never leave your control or get shared with Tinfoil.
2. **Tinfoil Servers:** Manage authentication and orchestration of backups.


Importantly, all sensitive operations happen locally on your device. Tinfoil and the cloud storage only orchestrate access, without ever seeing your data.


## Your Device, Your Keys, Your Data


Before any conversation leaves your device, it's encrypted using AES-GCM with a 256-bit key. This provides the highest level of post-quantum security for your conversations, ensuring secure long-term storage. Here is a sketch of how it works:


## Zero-Access Architecture


The Tinfoil server authenticates reads and writes to cloud storage buckets:


1. **Encryption:** The client encrypts all data using the local encryption key and sends the encrypted blobs to the orchestration server.
2. **Authentication** : The server uses your existing Tinfoil session to authenticate you.
3. **Storage:** The server stores the encrypted blob in the cloud.


The server *never* handles encryption keys or gets access to your plaintext conversation data. The server is only a traffic router ensuring the right people access the right storage locations at the right times. This means that even under a court order, we couldn't decrypt your conversations — we don't have the keys!


## Storing keys locally? Is that safe?


On the mobile app, all key material is stored securely inside Apple's keychain, which prevents access to key material by other applications.


However, things are more tricky in browser-based environments. A major problem with local storage in the browser is vulnerability to Cross-Site Scripting (XSS) attacks. An attacker that can inject malicious JavaScript into the application code can steal the encryption keys that are stored locally. This concern, while valid in many other contexts, is less of a concern in Tinfoil Chat as we explain below.


**Figure 2:** Encryption key stored in browser local storage, accessible to the application for client-side encryption and decryption operations.


## Understanding the Trust Boundary


With Tinfoil Chat, the client-side JavaScript **is** the full application. Users must trust the entirety of the front-end JavaScript code because it:


- Handles all message processing and rendering
- Manages the secure enclave communication for AI inference
- Performs the encryption/decryption and has access to the keys


If an attacker can modify this code, they don't need to steal keys — they can simply intercept messages as they're displayed or typed. Therefore, the encryption key stored in the browser doesn't expand the attack surface relative to, e.g., keystroke logging that could be orchestrated by the malicious JavaScript. This is why we make the application code open source and transparent. You can check for yourself that the code doesn't contain backdoors and is doing what it says it does.[View the source code on GitHub](https://github.com/tinfoilsh/tinfoil-webapp) .


## But why not use HTTPOnly cookies?


A standard approach for protecting encryption keys in browser environments is to store them in HTTPOnly cookies, which are inaccessible to JavaScript and thus protected from XSS attacks. While this approach has merit in traditional web applications, it doesn't align well with our local-first architecture for several reasons:


**1. Cross-Device Sync Becomes Impossible** : HTTPOnly cookies are tied to a specific browser and cannot be accessed by JavaScript. This means users cannot export their encryption keys to sync with other devices or conversation recovery purposes. Since the whole point of our backup system is to enable multi-device access to your conversations, HTTPOnly cookies would prevent such a feature from working.


**2. Limited Security Benefit in Our Threat Model** : While HTTPOnly cookies would protect against key extraction in some XSS scenarios, they don't meaningfully improve security in our context because, as mentioned above, if an attacker manages to inject malicious JavaScript into Tinfoil Chat, that alone is enough to intercept messages as they're typed or displayed. The encryption key doesn't expand the attack surface.


## Defenses


While XSS attacks are mitigated by our threat model, we nonetheless implement robust defenses to reduce the risks from this attack vector. In particular, we set a strict Content Security Policy and enforce standard access controls on encrypted backups, preventing unauthorized access, even if someone has the decryption key.


# The Local-First Promise


Our chat backup system demonstrates that the ideals of local-first software aren't just theoretical — they're achievable today both for messaging applications like Signal and for AI chat applications as well. With our new backup and sync system, you get:


- **Privacy** : Even Tinfoil can't access your backed-up conversations
- **Multi-Device Sync** : You can access your chats from any device with your keys
- **Full Control** : Your data is only accessible by you and nobody else


# Beyond Backup


This backup system is a testament to how AI services should handle user data. By adopting local-first principles, we've built an AI chat system where:


- Privacy isn't a feature to be toggled on and off by providers
- AI conversations receive the same confidentiality expectations as human ones
- Your thoughts remain yours, even when backed up and synced


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-09-02-qwen3-coder-private)[Next Post](https://tinfoil.sh/blog/2025-12-18-browser-native-verification)
