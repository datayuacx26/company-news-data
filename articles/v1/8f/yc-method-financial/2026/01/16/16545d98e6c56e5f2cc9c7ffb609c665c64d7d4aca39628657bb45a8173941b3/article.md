---
schema_version: "1.0.0"
document_id: "16545d98e6c56e5f2cc9c7ffb609c665c64d7d4aca39628657bb45a8173941b3"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-4e3125e6e0a6"
canonical_url: "https://methodfi.com/blog/method-for-commerce-one-connection-for-the-full-wallet"
published_at: "2026-01-13T00:00:00+00:00"
first_seen_at: "2026-07-22T04:10:42.808298+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:6f220aa5b34f83e2d7088129afd9ff971cb7e28df154d4c74c3a8d95c62c3c93"
---

# Method for Commerce: One Connection for the Full Wallet

E-commerce is one of the most optimized industries on the planet. Buy a product online today and you will get to enjoy millions of dollars worth of A/B testing: the pay-per-click ads you see on Google, the UI and UX of the website, the layout and copywriting on the product page, the upsells in the checkout flow. In commerce, the last drops of optimization juice have been squeezed out of everything.


Well, *almost* everything. What about the part where customers enter a 16-digit card number?


Companies spend billions of dollars to reduce friction and increase conversion, and yet card connection is a frontier that has hardly been touched.


-


Why do customers still need to manually input their card numbers?


-


Why can merchants not help customers make the most of their full wallets (e.g. via rewards)?


-


Why are there still so many problems with fraud, and so many false positives?


-


Why do so many customers get declined, or hit their credit limit, and then drop off?


For merchants, this status quo means that they have no idea if a payment will go through until after they try to charge it. They don’t know what other cards a user might hold. They don’t even know if the person making the purchase is the cardholder.


And this unnecessary friction is frustrating for regular customers, too. They still have to enter 16-digit card numbers every time they make their first purchase on a website. They still don’t know if their card will be declined for some unknowable reason. They may not even have the card they’d like to use with them on hand (and thus, not be able to use it). Customers know it as well as anyone: friction decreases the likelihood of a purchase.


## Method unlocks more revenue for commerce with better connections


You could imagine a different version of the world where none of this was a problem. A world where a customer would simply provide their name and phone number, and their full wallet would be connected automatically. In this version of the world, customers would abandon checkout less often. Merchants would be able to know which card was least likely to cross a credit limit. Fraud identification would be far more accurate, and false positives would happen less often.


This is the version of the world that Method has unlocked for commerce. Over the past year, we have taken the infrastructure we built for liability connectivity and applied it to commerce flows for companies like[Bilt](https://methodfi.com/customers/bilt) and[Sezzle](https://sezzle.com/) .


## Less friction


Inputting payment information is the highest-friction point of checkout, which means it’s also where people tend to drop off.


This friction is compounded by how big the average wallet is and how fast it evolves. The typical American holds roughly 4 credit cards, and in a rewards-hungry economy, active consumers open new cards constantly. For merchants, this is usually a missed opportunity: consumers want to put spend on these new cards to hit sign-up bonuses, but the friction of manually entering new digits gets in the way.


With Method, all of that friction vanishes. Customers just type in their name and phone number, and we connect their full wallet right away. This way, people can put spend on whichever card they’d like and they never have to enter 16 digits in order to do so.


Method takes this seamless connection a step further, transforming each card into a persistent payment token. These tokens represent secure, evergreen forms of payment. Users don’t have to re-enter their information every time they replace their card, and merchants don’t need to bear a complex PCI burden.


[Sezzle](https://sezzle.com/) , the buy now pay later company, built a flow for this to reduce friction at checkout and to reduce chargeback volume with better authentication (more on that in a minute).


## Less fraud


It may sound counterintuitive that a product can reduce both payment friction and fraud at the same time. Shouldn’t easier to pay also mean easier to hack? Not in Method’s case.


Standard fraud tools are a collection of heuristics; they guess based on signals like IP and device. This sometimes works, but it still misses a lot of fraud and turns up plenty of false positives.


Method goes further with two approaches. We start with a user’s credit report, matching their PII (name, address, etc.) to their liabilities and making them available for payment. This lets us start with verified ownership rather than checking it later. Separately, we can verify a user’s PII against the ANI (name on card) and AVS (billing address registered with the card issuer). This reduces fraud *and* reduces false positives.


We can also check available credit before payment authorization. If a user doesn’t have enough credit to make the purchase, we can stop them from attempting that purchase (saving decline fees!) and suggest a different card to use if they have available credit elsewhere.


## More value


Today, companies only get to see the (usually one) card that the user connects. This is just one piece of a much larger puzzle. Bilt, the rewards network, had this very problem: they wanted to encourage users to spend across their merchant network, and they wanted to reward those users accordingly. But the Bilt team knew many of their users were spending on cards that Bilt didn’t have access to.


So[Bilt designed a branded flow with Method Card Connect](https://methodfi.com/customers/bilt) , which made it far easier for users to connect their full wallets and earn more points. This resulted in a 148% (!) increase in user card connections.


## The future of commerce is data-first


One constant with human inventions is that we often think of them long before they become possible.


Many of the things Method does for commerce (instant wallet connection, better fraud detection, more ways to unlock value) are things that merchants have wished they had technology to do for years. The reason customers have been stuck entering 16 digits manually is because the alternative was impossible.


Method is unlocking a world where commerce is more optimized than ever and customers have smoother, more valuable experiences as they make purchases. You can[read our API docs here](https://docs.methodfi.com/reference/introduction) or[experience the live wallet connection yourself here.](https://mthd.fi/commercedemo)
