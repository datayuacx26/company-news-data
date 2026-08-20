---
schema_version: "1.0.0"
document_id: "06c38cb05cb3a06b827029bf0c3c25e8e6ecc23630199c3259935fb824746bdb"
company_key: "yc-authologic"
company: "Authologic"
source_id: "yc-authologic-news-import-90df22b47317"
canonical_url: "https://authologic.com/blog/post-omnisummit-its-clear-where-identity-is-heading"
published_at: "2026-04-20T22:00:00+00:00"
first_seen_at: "2026-07-21T08:36:12.295828+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:cdc9eccd1c1dc1fb338eef5ea00c51fa0e9e49660c288f8ca192f7f3a63e62ca"
---

# Post-OmniSummit: It's clear where identity is heading

Last Thursday, we held the first edition of Authologic OmniSummit in Warsaw.


OmniSummit, Warsaw — 2026/04/16


We built the agenda around a single idea: the digital identity stack has enough distinct layers that no single company can show you the whole picture.


So we brought in the people building each one:


- **Rafał Sionkowski** from Poland's Central IT Centre on the EUDI Wallet rollout
- **Dawid Wróblewski** from Samsung on the hardware security layer underneath
- **Lucyna Janas** from Google on the Digital Credentials API and Google Wallet
- And our own team on OmniID


Each from a different position in the stack, all pointing at the same shift. Here's what that shift looks like, put together.


## This didn't start last week


Samsung, Apple, and Google have spent years building secure vaults inside phones – like KNOX Vault and Secure Enclave – initially to protect payment card data. Mobile wallets started with payments because that was the closest use case to consumers at the time. Then came tickets, hotel keys, loyalty cards. The phone wallet slowly became part of everyday life.


In March 2022, Phoenix Sky Harbour became the first airport to accept a digital driver's license from an iPhone at a TSA checkpoint. For the first time, a phone wallet replaced a physical identity document in a regulated context.


eIDAS 2.0 entered into force in May 2024. Every EU member state is required to make a digital identity wallet available to its citizens by December 2026. A year later, many companies will be legally required to accept it in KYC/AML processes.


On September 15, 2025, Safari 26 shipped with native Digital Credentials API support. Two weeks later, Chrome 141. Firefox is next. For the first time, a website can invoke a user's identity wallet directly from the browser. The browser initiates the call, but the actual data transfer happens outside it: the wallet passes a cryptographically signed credential directly to the verifier, which validates the signatures independently.


550 organizations across 26 EU countries have already started Large Scale Pilots for the EUDI Wallet. Rafał Sionkowski showed us the timeline for Poland – a launch of the European wallet integrated with the mObywatel app as early as December this year.


There's just one problem with all of this.


## Not everywhere at once


As people say these days: the future is already here, it's just not evenly distributed. This infrastructure won't arrive everywhere at the same time. 21 US states have a Mobile Driving License (mDL), the rest don't. Poland has mObywatel, France has a different app, and in some EU countries the rollout will slip by a quarter or two past the deadline. First-generation methods continue to operate alongside wallets: BankID, document photos with selfie.


Your customers are in all of these places, using all of these methods.


## That's why we've built OmniID


The problem that's emerging is no longer "will this infrastructure exist." It's how to use it here and now, in a world that's still uneven. OmniID is the layer that connects what's currently still fragmented. From the user's perspective, it's a single button. From the business's perspective, it's one integration. Everything else happens in the background.


If a digital wallet is available, we use it natively. If the user doesn't have a wallet yet, we verify through bank-issued eID or another first-generation eID. If the user has neither, we offer a wide choice of traditional methods based on document and facial photos.


Instead of deciding which method to implement, you let the system find the right path for each specific user, at each specific moment.


But OmniID isn't just a bridge for the transition period. When this infrastructure goes mainstream, OmniID will remain the simplest possible access point to it – no need to rebuild the integration, track changing standards, or adjust to differences between countries. And perhaps most importantly: this isn't only about identity itself. The wallet is starting to carry other data: ownership, professional credentials, and financial data.


## Join our Early Access Program


Companies that use this moment early will build an experience that customers will use to benchmark every service that follows. OmniID was announced now precisely to enable that first move for organizations that want to be first. For them, we're opening an **Early Access Program** : a chance to deploy OmniID today, shape the product direction, and step into what we named the theme of OmniSummit's first edition: *A New Era of Digital Identity in Europe* .


Reach out to your account manager or book a meeting[here](https://app.cal.com/team/authologic/demo-meeting) .
