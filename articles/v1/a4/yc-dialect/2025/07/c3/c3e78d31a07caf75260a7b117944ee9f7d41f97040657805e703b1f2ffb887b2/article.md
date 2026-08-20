---
schema_version: "1.0.0"
document_id: "c3e78d31a07caf75260a7b117944ee9f7d41f97040657805e703b1f2ffb887b2"
company_key: "yc-dialect"
company: "Dialect"
source_id: "yc-dialect-news-import-4cb56bc42b2e"
canonical_url: "https://www.dialect.to/blog/action-chaining-and-more-now-available-everywhere"
published_at: "2025-07-11T13:05:00+00:00"
first_seen_at: "2026-07-21T16:13:21.108809+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:fe2b98a0f707ab119c07587f3758c4bbe2c80b5b6e2d33f40bcc3aadf71e17e1"
---

# Action Chaining (& More) Now Available Everywhere

We’re happy to share that Action Chaining, Advanced Input Types, Blinks Versioning and Multichain Compatibility are now available in all the major wallets, including Phantom, Backpack, Solflare, OKX, ByBit and others.


Launched in late August, these advanced blinks capabilities let you create more complete & immersive experiences for your users. Now that they’ve been rolled out across all the major wallets, you can reach the widest possible audience with them. So let’s review what these new capabilities are and what you can do with them.


Blinks use a primitive called “Actions.” Actions give your app’s URLs superpowers, making it possible to buy things, swap, stake, etc., from anywhere.


#### Action Chaining


Blinks originally supported embedding just a single action in a blink. Action chaining now let’s you to create a sequence of them.


If you run an e-commerce platform, you might use action chaining to let users:


- Fill in their shipping information
- Make a purchase
- Provide a follow-up email address
- And anything else you might want to do


Here’s an example of a 3-step flow to buy a[Keystone Wallet](https://x.com/KeystoneWallet) right from a blink:


Virtually no changes are needed to start using action chaining; all you need to do is provide a next attribute in the links section of the ActionPostResponse.


Check out our documentation for more information on the[specification](https://docs.dialect.to/documentation/actions/specification/action-chaining) , and a[how-to guide](https://docs.dialect.to/documentation/actions/actions/action-chaining) with links to[examples](https://github.com/dialectlabs/actions/tree/main/examples/hono/examples/chaining) in our github.


This will allow developers to directly embed all the basic experiences you’d need to take many different kinds of actions, and the fact that they’re blinks means those experiences can happen anywhere and everywhere the link is shared.


****


#### **Advanced Input Types**


Advanced input types let you do more with each action. Blinks now support customizable fields & other input types like:


- Select menus
- Checkboxes and radio buttons
- More advance input types like numbers, phone numbers, emails


#### **What’s coming next**


Lastly, sign message is now in development and will be released soon. Sign message will let you talk to offchain services to store information like shipping addresses or interact with crypto native applications like[DRiP](https://x.com/drip_haus) to tip creators, & do so much more.


Sign message is coming soon. Stay tuned.


That’s it. Action chaining, advanced input types, multichain and signed messages are a major step forward for actions & blinks, and are now available across all the major wallets.


We can’t wait to see what you build with them.


Get started today at[https://docs.dialect.to](https://docs.dialect.to/) .
