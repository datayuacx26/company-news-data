---
schema_version: "1.0.0"
document_id: "ab030784c8405ae7d1be5d3865c25d340aa4d9260ea5627b40474dce1720435c"
company_key: "yc-freeport-markets"
company: "Freeport Markets"
source_id: "yc-freeport-markets-news-import-46c2f164323d"
canonical_url: "https://www.freeportmarkets.com/research-is-freeport-legit.html"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-25T05:46:52.249644+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:8bab067b9fd6c6c511cc2e26fa8f75d440c2df3201bb0465a9825ee85ba0f2ed"
---

# Is Freeport Legit? An Honest Look · Freeport

Let us start with the obvious conflict of interest: you are reading this on Freeport's own blog. We are not a neutral party, and we are not going to pretend to be one. So instead of telling you "yes, trust us," this essay does something more useful. It explains exactly what Freeport is and is not, who is behind it, how your money is actually held, and what is genuinely risky about using it.


## What "legit" should mean


When people ask "is this app legit," they are usually asking three separate questions at once. Is it a real company, or a scam? Will it actually let me withdraw my money? Do I understand the risks of what I am buying?


A scam fails the first two. A legitimate but risky product passes the first two and demands real attention on the third. Freeport is firmly in the second category, and this post treats all three questions honestly.


## What Freeport is and is not


Freeport is a mobile trading app that lets you trade tokenized stocks, crypto,[perpetual futures](https://freeportmarkets.com/research-perpetual-future.html) , and pre-IPO exposure from one account, with trades settling on public blockchains that allow 24/7 trading on leverage. That is the real product, and it works as described;[How Freeport Works](https://freeportmarkets.com/research-how-freeport-works.html) walks through it piece by piece.


Freeport is not a registered broker-dealer. It is not a bank. It is not an exchange. Your assets are not SIPC- or FDIC-insured. When you trade, your order is routed to established on-chain venues, and assets settle into a blockchain wallet tied to your account. Freeport's own terms of service state plainly that it does not provide brokerage, exchange, or custody services in the regulated sense. This is a deliberate design. Freeport is built on public, auditable blockchain rails rather than inside the traditional brokerage system. That choice is what makes the product fast, global, and broad, and it is also why the protections that come with a registered broker-dealer do not apply.


## The people behind Freeport


Freeport is led by founder and CEO Lihong Wang and founder and CTO Bryan Reed. It is a venture-backed startup, not an anonymous operation. It is backed by Y Combinator, which means it passed the diligence of one of the most established startup accelerators in the world, along with Alliance DAO and Informed Ventures, institutional investors with a track record in the space.


Venture backing is not a guarantee of anything; startups are risky by nature. But it does mean real, named, reputable institutions vetted the team and put their own capital behind it, which is the opposite of how a scam is structured. Scams avoid scrutiny; Freeport invited it and passed it.


The founders write publicly, under their own names, about how the product works, including its limitations; this essay is an example. A company willing to publish its own weak points is not behaving like one with something to hide.


## Traction and usage


Freeport launched publicly in March 2026. From launch to the end of May it has grown to roughly 10,000 downloads and processed more than $50 million in cumulative trading volume. Thousands of users have funded accounts and traded.


**~10,000 downloads · $50M+ volume** From public launch in March 2026 through the end of May.


The numbers show a real product that real people are using and trusting with real money. That said, Freeport is still a young product a few months into its public life, not an institution with a decade of history. Early-stage products improve quickly; they are also, by definition, less battle-tested than established ones. Both of those things are true, and you should weigh them.


## How your money is held


When you sign in with Google or Apple, an embedded wallet is created for you on the Solana and Ethereum blockchains using Privy, a provider that is now part of Stripe. Your assets are real on-chain tokens held in that wallet, on public blockchains, not in a private Freeport ledger. You do not manage a seed phrase; the key material is secured in trusted hardware and transactions are signed through Privy's infrastructure. You can withdraw, and assets can be moved to external wallets.


This is not traditional custody, and it is not pure self-custody. A brokerage holds shares for you; pure self-custody means you alone hold the assets. Freeport's model sits between the two: app-login convenience with assets that genuinely live on public chains. It is also not a registered broker-dealer holding insured assets, so there is no SIPC backstop if something goes wrong.


For someone to steal your funds, one of a small number of highly unlikely things would have to happen. They would need to compromise your Google or Apple sign-in, the same credentials that already gate your phone, your email, your bank account, and effectively your digital identity. Or Google or Apple themselves would have to be breached at the account-takeover layer, the kind of event that would make front pages worldwide and that those companies spend billions a year preventing. Or Ethereum and Solana would have to be cryptographically broken, networks that secure hundreds of billions of dollars between them and have never lost consensus. Or Privy's signing infrastructure, run by part of Stripe, would have to fail. Each is a tail-risk event, and the layers are largely independent, so a meaningful compromise requires more than one to fail at once. That is not the same as impossible, and we will not pretend it is; the residual risk falls on you. But the practical security floor is roughly comparable, on the dimensions that matter to a normal user, to the security of the phone you are already holding.


## What is genuinely risky


A legitimate platform can still expose you to real losses. The most common way for Freeport users to lose money will be choosing bad investments that fall in value over time. A few other risks should be stated clearly.


Smart-contract and blockchain risk: the on-chain rails that make Freeport fast also carry protocol bugs and exploit risk. On Freeport, tokens touch other protocols only at the moment of a swap, so even if a decentralized exchange is hacked your held funds are fine; open perp positions, however, are continuously interacting with the Hyperliquid, Ostium, and Lighter protocols.


Leverage: perps can be liquidated, and leverage can erase your stake on an ordinary market move. This is the fastest way to lose money on the platform.


Pre-IPO and private credit exposure is illiquid and opaque. Private companies and their equity and debt are hard to value, may never go public, and the tokenized wrappers carry their own risks, including the legitimacy of the SPVs that back them.


## Why we built it this way


We could have built Freeport as a traditional brokerage and avoided these conversations. We chose the on-chain path because it is what makes the product genuinely better: one account for stocks, crypto, leverage, and pre-IPO exposure; near-24/7 markets; fast settlement; global reach; and no KYC to start. The cost of that choice is that we have to be honest about a different risk and custody model. The most legitimate thing a young financial company can do is tell you exactly what it is and make it easy to take your money out. That is what this post has tried to do.


Read more from the Freeport research team on the Freeport Logbook.


[Freeport Logbook](https://freeportlogbook.substack.com/)
