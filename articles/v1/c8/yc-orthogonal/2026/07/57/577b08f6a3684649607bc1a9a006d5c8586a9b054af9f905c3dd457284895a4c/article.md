---
schema_version: "1.0.0"
document_id: "577b08f6a3684649607bc1a9a006d5c8586a9b054af9f905c3dd457284895a4c"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/mpp-machine-payments-protocol"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:69cfa2f41c93dd2c0b784a9170eb51011c271fffe15d30d2669054da4e3356ca"
---

# MPP: Machine Payments Protocol

Today we're excited to announce our partnership with[Tempo](https://tempo.xyz/) to bring the[Machine Payments Protocol (MPP)](https://mpp.dev/) to Orthogonal. MPP is a superset of x402 that adds multi-method payments, sessions, streaming, and an IETF-track standard. Your agents can now pay for API calls inline using stablecoins, cards, or Bitcoin.


## What is Tempo?


Tempo is a Layer 1 blockchain purpose-built for payments, developed in partnership with Stripe and Paradigm. Unlike general-purpose chains designed for trading, Tempo is optimized for real-world stablecoin transactions at scale.


Key features:


- **Sub-second finality** : Blocks finalize in ~0.6 seconds with no re-orgs. Settlement certainty that matches existing financial systems.
- **Stablecoin-native gas** : Pay fees in USD stablecoins. No volatile gas tokens, predictable costs.
- **Dedicated payment lanes** : Guaranteed blockspace for payments at the protocol level. Fees stay low even when network activity spikes.
- **Smart accounts** : Programmable wallets with gas sponsorship, batch transactions, scheduled payments, and passkey auth.
- **Privacy** : Opt-in confidential transactions with the auditability compliance requires.


## What is MPP?


MPP (Machine Payments Protocol) is the open standard for machine-to-machine payments, co-authored by Tempo and Stripe. It standardizes HTTP 402 so agents and services can exchange payments in the same HTTP request.


When a server requires payment, it returns a 402 with a challenge describing the price, accepted methods, and expiration. The client signs a credential, attaches it to the retry, and the server verifies payment and returns the response with a receipt. Two round-trips: the initial 402 challenge, then the paid retry.


MPP supports multiple payment rails out of the box: Tempo stablecoins, Stripe cards, Lightning Bitcoin, EVM tokens, Solana, Stellar, and more. Visa extended MPP to support card-based payments. Lightspark added Lightning Network support.


## Using MPP on Orthogonal


Every API on Orthogonal is now accessible via MPP at` mpp.orthogonal.com` . Your agents can pay per request using a wallet instead of an API key.


### Pay for an API Call with MPP


` import


{


privateKeyToAccount


}


from


"viem/accounts"


;


import


{


Mppx


,


tempo


}


from


"mppx/client"


;


// Set up automatic 402 handling with your Tempo wallet


Mppx


.


create


(


{


methods


:


\[


tempo


(


{


account


:


privateKeyToAccount


(


"0x..."


)


}


)


\]


,


}


)


;


// Global fetch now handles 402 challenges automatically


const


response


=


await


fetch


(


"https://mpp.orthogonal.com/didit/v3/email/send"


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


email


:


"user@example.com"


,


}


)


,


}


)


;


const


data


=


await


response


.


json


(


)


;


console


.


log


(


data


)


;


`


The` mppx` client handles the 402 challenge automatically: it intercepts the payment request, signs a credential with your wallet, and retries with proof of payment. You get back the API response.


### Pay-as-You-Go Sessions


For high-frequency workflows, open a payment session. You deposit once into an on-chain escrow, then send signed off-chain vouchers with each request. Only two on-chain transactions total: open and close. Everything in between is just a signed message.


` import


{


tempo


}


from


"mppx/client"


;


import


{


createClient


,


http


}


from


"viem"


;


import


{


privateKeyToAccount


}


from


"viem/accounts"


;


import


{


Chain


}


from


"viem/tempo"


;


const


account


=


privateKeyToAccount


(


"0x..."


)


;


const


client


=


createClient


(


{


account


,


chain


:


Chain


.


mainnet


,


transport


:


http


(


)


,


}


)


;


// Lock 1 USD into a payment channel. Unused deposit is refunded on close.


const


session


=


tempo


.


session


.


manager


(


{


client


,


maxDeposit


:


"1"


,


}


)


;


for


(


const


lead


of


leads


)


{


const


result


=


await


session


.


fetch


(


"https://mpp.orthogonal.com/apollo/v1/people/match"


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


email


:


lead


.


email


}


)


,


}


)


;


// Each request sends an off-chain voucher, no gas, no on-chain tx


}


// Close the channel. Server settles on-chain, unused deposit refunded.


await


session


.


close


(


)


;


`


### Multiple Payment Methods


Pay with stablecoins, cards, or Bitcoin on a single endpoint.


` import


{


privateKeyToAccount


}


from


"viem/accounts"


;


import


{


Mppx


,


tempo


,


stripe


}


from


"mppx/client"


;


import


{


spark


}


from


"@buildonspark/lightning-mpp-sdk/client"


;


Mppx


.


create


(


{


methods


:


\[


tempo


(


{


account


:


privateKeyToAccount


(


"0x..."


)


}


)


,


stripe


(


{


paymentToken


:


"spt_..."


}


)


,


spark


.


charge


(


{


mnemonic


:


process


.


env


.


MNEMONIC


}


)


,


\]


,


}


)


;


// Client picks the best available method automatically


const


response


=


await


fetch


(


"https://mpp.orthogonal.com/coresignal/v1/enrich"


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


linkedin_url


:


"https://linkedin.com/in/janedoe"


,


}


)


,


}


)


;


`


## Use Cases


### Zero-Config API Access


New developers and agents can access any Orthogonal API immediately. No sign-up forms, no API key provisioning. Attach a wallet and start making requests.


### Autonomous Agent Workflows


Agents running multi-step workflows across multiple APIs can use a single wallet for all of them. One payment method, many services, no credential management per provider.


### MCP Server Monetization


If you're building MCP tools, MPP lets you charge per tool call with stablecoins. No Stripe integration, no billing portal, no invoicing. A few lines of middleware.


## Why We Partnered with Tempo


Tempo is building the payments infrastructure that agentic commerce needs. A purpose-built L1 with sub-second finality, stablecoin-native gas, and dedicated payment lanes means agents get deterministic settlement without the latency and gas volatility of general-purpose chains. MPP, co-authored by Tempo and Stripe, gives agents a standard way to pay for any service over HTTP, with support for one-shot charges, sessions, streaming, and multiple payment rails. By integrating MPP into Orthogonal at` mpp.orthogonal.com` , every API on our platform is accessible to any agent with a wallet.


## Try It Today


MPP is live on Orthogonal now. All of our endpoints are discoverable on the[MPP Discover Service](https://mpp.dev/services) . Set up a Tempo wallet, point your agent at` mpp.orthogonal.com` , and start making paid API calls with zero configuration.


[Browse Orthogonal on MPP Discover](https://mpp.dev/services) |[MPP Docs](https://mpp.dev/) |[Tempo](https://tempo.xyz/)
