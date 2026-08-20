---
schema_version: "1.0.0"
document_id: "d97e29d17444f872754afe1636e7bc25d7a5d8f454def25dfaedd7c14b6a47eb"
company_key: "yc-protocol-labs"
company: "Protocol Labs"
source_id: "yc-protocol-labs-rss-95d08b63fd6d"
canonical_url: "https://pl.xyz/blog/threads-from-the-2025-cypherpunk-retreat/"
published_at: "2025-10-03T00:00:00+00:00"
first_seen_at: "2026-07-22T20:57:16.093704+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:0ca34ae56ffaa0f268bfe23d659f2d7317c8b4556842be1dc09909152c3c46fe"
---

# Threads From the 2025 Cypherpunk Retreat

Last week,[Protocol Labs (opens new window)](http://protocol.ai/) and[Web3Privacy Now (opens new window)](https://web3privacy.info/) hosted a[retreat (opens new window)](https://cypherpunk.camp/) outside of Berlin for 100 leading builders in the privacy-tech space. Conversations ranged across secure communications, decentralized identity, and trusted computing. The bulk of the time was spent in smaller groups working on immediate coordination problems, and the event also provided an opportunity for projects to share and better understand each other's roadmaps.


The retreat was designed to surface issues and align projects. We presented research, discussed assumptions, and found areas to collaborate on accelerating privacy in our systems. The following is a brief summary of the opportunities and discussions from the retreat.


## # Secure Communications Adoption


**P2P Interop/Bootstrapping**
One coordination issue several projects were hoping to advance in Berlin was on constructing a larger anonymity set for peer-to-peer networks. The centralization of DHT bootstrapping/introduction points continues to be one of the most common ways protocols are finding themselves blocked or surveilled. Different protocols had different threat models, where for some the enumeration of users is a primary concern (like wallets), while for others it is the resilience against network censors.


Through several sessions, participants approached some design on what could help nodes look similar, and what sort of deniable introductions could allow nodes to look “generic” and difficult to probe by an active adversary without a direct introduction.


**Dweb Server**
One conversation that has already produced results focused on making it easier to publish content privately by packaging a web server with a Tor onion service. The Internet Archive is interested in how a larger number of entities can host slices of archival data with minimal liability. The initial demonstration and thought process has centered around the following repository:


[https://github.com/vpavlin/onion-press (opens new window)](https://github.com/vpavlin/onion-press)


In addition to the direct node software, there are a set of questions for how nodes should advertise, and how discovery/routing for such a system can work that the group is continuing to think about.


## # Decentralized Identity


**AT Proto**
Two immediate extensions to[AT Proto (opens new window)](https://atproto.com/) revolve around handling payments with an experiment emerging using[cashu (opens new window)](https://cashu.space/) , and bridging the model to private group messaging with work being done by[germ (opens new window)](https://www.germnetwork.com/) .


In addition, a larger question was considered around how different DID users can move towards interoperability and extensibility. The group identified grants that are available for conformance testing which can help bridge “equivalent DIDs” but also came to the conclusion that experimenting with extending DIDs to other domains, like “objects” can be a place to test interop strategies with lower risk.


**Jurisdictions**
A systematic risk is the centralization of physical infrastructure in the US, which has led to a push for more operators in more places. In addition to the[eurosky.social (opens new window)](https://www.eurosky.social/) project that will next convene in November, much of the conversation in Berlin was around the costs of liability around moderation and abuse handling that comes with operating in different jurisdictions. Getting clarity on jurisdictions with low burdens for hosting will be important for prominent platforms to feel comfortable expanding their operations. Several countries in the EU are working towards gaining that clarity. Once they land that, we must have visibility across their implementation to see how well those regulations hold up in practice.


**Wallet Privacy**
Most wallets today leak information about the user to the RPC provider that they use as a backend. A user fully protecting their privacy today must run their own desktop node software along with network anonymity tooling, which rarely exists in browsers or mobile wallets.


In Berlin, a set of conversations considered how additional networking tools between users and RPC, or cryptographic techniques can improve this situation. Two of the efforts that crystallized at the retreat were a push around private identity mechanisms (how to get past passkeys as the local identity, which is being made more possible by things like[ed25519 support (opens new window)](https://blogs.igalia.com/jfernandez/2025/08/25/ed25519-support-lands-in-chrome-what-it-means-for-developers-and-the-web/) ); and on better conformance/interoperability testing so that not every wallet needs to fully reinvent a privacy story (A need identified and pushed by[PSE (opens new window)](https://pse.dev/) ). Both efforts highlight how coordinated work on identity and interoperability can lower barriers for developers and strengthen privacy guarantees for users.


## # Trusted Computing


We recognized that much of the benefit and current adoption here comes from enterprises as a way to better harden their software and operational supply chains. The group strategized on ways to better leverage this need into acceleration of the tech stack, and on whether it was enough to warrant a user-facing push and attempt at broader adoption. There will be further exploration of this landscape turned into a brief position piece for how that adoption might occur.


## # Conclusion


The conversations in Berlin were a first — though nonetheless exciting — step in the greater goal of establishing a fully private, decentralized internet. We plan to continue the conversation in Buenos Aires, during the[Ethereum Cypherpunk Congress (opens new window)](https://luma.com/u2sw5kpv?tk=UEhpqh) . If you’re building in this space and are keen to contribute to the ongoing work, you can go deeper into the research[here (opens new window)](https://anonymity.dev/) .
