---
schema_version: "1.0.0"
document_id: "939be0fde65a825b8cef1bc20a330ec4102021e3ae50e5ea16618ff1a980eee0"
company_key: "yc-notabene"
company: "Notabene"
source_id: "yc-notabene-news-import-e4585f8c666e"
canonical_url: "https://notabene.id/post/alex-says-yes-to-stablecoins"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-26T08:22:57.520792+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:e04c33ebf90af934475bec210da1a4e916d0be5f5886a8f0754c018a75db99b2"
---

# Alex says "yes" to stablecoins

Meet Alex, VP of Finance at Notabene.


Every month, a customer asks Alex the same question: "Can we pay you in stablecoins?" And every month, Alex understands exactly why they're asking. Stablecoins are faster than a wire, cheaper than a card, and they don't care that it's a Saturday or a bank holiday in another timezone. For a finance team trying to close the books and keep cash moving, that's genuinely appealing.


For a long time, though, Alex's honest answer was closer to "yes, but."


The "but" wasn't for lack of interest, but rather an operational hurdle. When a stablecoin payment landed, it arrived as a transaction hash. No business name, no invoice number, nothing tying the money to the customer who sent it. Before any of it could touch the books, someone on Alex's team had to play detective: whose payment is this, which invoice does it close, is the amount even right? Multiply that across a month of payments and you've turned a faster rail into slower accounting.


Then there was the chain problem. One customer wanted to pay USDC on Base. Another only held funds on Ethereum. A third was set up on Solana. Each one kicked off the same tedious negotiation about which network everyone could live with, a back-and-forth that ate hours nobody wanted to spend when there was real work waiting.


So the friction was never the stablecoins themselves. It was that the payment systems around them hadn't been built for the way a finance team actually operates.


Notabene CEO Pelle Braendgaard and Sam Broner (Founder and CEO of The Better Money Company) explain on an episode of Stack Chats:


> ‍ *"You talk to the accounting guy and they're like, wait, so you're saying I can finally bill someone seven hundred and eighty-nine dollars and eighty-seven cents? Thank you, that is exactly what I needed."*


([Watch the full episode](https://notabene.id/post/stack-chats-feat-sam-broner) of Stack Chats with Pelle and Sam)


### What changed


[Notabene Flow](https://notabene.id/flow) was built for Alex's side of the table. It's the payment network that authorizes every B2B invoice before it settles and reconciles it as it arrives. The invoice reference, the counterparty's verified identity, and the payment context travel with the money, so finance teams reconcile as they receive instead of investigating after the fact. The transaction hash stops being a mystery to solve.


Alex also gets to set the rules. Flow lets him specify exactly which stablecoins and which chains he's willing to accept, the way he'd set any other treasury policy. His customer sends a[payment link](https://flow.link/) , pays through their own provider, and Alex receives a clean, reconciliation-ready record on the other end. No chasing references. No crypto expertise required on either side. *‍*


> *"How does plumbing work? You just turn the sink on, but there's 200 years of infrastructure under these streets... the simple thing is you get to flip the tap."*


([See how you can turn any invoice into a stablecoin payment link](https://notabene.id/post/how-to-generate-a-payment-link-from-any-invoice-on-notabene-flow) with Notabene Flow)


### The part that makes the answer a clean "yes"


There was still one gap: what happens when Alex wants to settle in his preferred stablecoin, but the customer holds a different one? That's where[our partnership with The Better Money Company](https://www.linkedin.com/posts/notabeneflow-stablecoinpayments-stablecoins-ugcPost-7473043708867670016-dJu8/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAABwlHwB0KP1S7dmfZMvbRimhQIB2KlN2OU) comes in.


Better Money runs a stablecoin clearinghouse. Instead of trading one stablecoin for another on the open market and eating the slippage, it clears at par, going directly to the issuers the way banks settle between themselves. Integrated into Flow, it means the payer can send whatever stablecoin they hold, on whatever chain, and Alex receives exactly what he asked for, in his preferred stablecoin, down to the cent.


For Alex, all of that machinery disappears. The question comes in, and the answer is just "yes."


**Interested in giving your finance team the same convenience that Alex now has?**[Schedule a quick conversation](https://notabene.id/sign-up-for-notabene) and we'll show you all you need to know to get started with Notabene Flow.


‍
