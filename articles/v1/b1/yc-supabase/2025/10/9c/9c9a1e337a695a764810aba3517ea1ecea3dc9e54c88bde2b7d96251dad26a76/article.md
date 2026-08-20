---
schema_version: "1.0.0"
document_id: "9c9a1e337a695a764810aba3517ea1ecea3dc9e54c88bde2b7d96251dad26a76"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/login-with-solana-ethereum"
published_at: "2025-10-03T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:caab0b2fc9efa8fff34f45bea87241312b75418c1f644c9139b4db3f36feaed7"
---

# gm 👋 web3, welcome aboard to Sign in with Web3 (Solana, Ethereum)

Today we're announcing[support for Web3 logins using Ethereum and Solana](https://supabase.com/docs/guides/auth/auth-web3) . This has been one of the historically most requested features for Supabase.


It's all based on the widely adopted[Sign in with Ethereum standard (EIP-4361)](https://eips.ethereum.org/EIPS/eip-4361) , which both Ethereum and Solana off-chain wallet authentication is based on. This protocol is widely adopted across all of the popular wallet applications (both software and hardware) today, so building a Web3 application on top of Supabase has never been easier.


## How does it work?#


We wanted to make it simple. The Sign in with Ethereum standard defines a particular message structure, one that looks like so:


`
_12


example.com wants you to sign in with your Ethereum account:


_12


0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2


_12


_12


I accept the ExampleOrg Terms of Service: <https://example.com/tos>


_12


_12


URI: <https://example.com/login>


_12


Version: 1


_12


Chain ID: 1


_12


Nonce: 32891756


_12


Issued At: 2021-09-30T16:25:24Z


_12


Resources:


_12


- <https://example.com/my-web2-claim.json>


`


It's interpreted both by the wallet application, which presents a secure login confirmation dialog, while also being validated by Supabase Auth before issuing a user session.


Most of these details are already handled for you by the Supabase JavaScript SDK, so it's really as simple as calling this in your Web3 app:


`
_10


await supabase.auth.signInWithWeb3({


_10


chain: 'ethereum', // or 'solana'


_10


statement: 'I <3 Supabase!',


_10


})


`


The API is powerful enough to support more modern approaches to building Web3 applications such as using the[Solana Wallet Adapter](https://solana.com/developers/courses/intro-to-solana/interact-with-wallets#solanas-wallet-adapter) system or the[Ethereum Wallet Discovery Mechanism](https://docs.metamask.io/wallet/tutorials/react-dapp-local-state) .


You can configure these on the Supabase Dashboard, or in the Supabase CLI:


`
_10


\[auth.web3.solana\]


_10


enabled = true


_10


_10


\[auth.web3.ethereum\]


_10


enabled = true


`


Don't forget to configure rate-limits and CAPTCHA, as Web3 apps are usually more prone to abuse by bots:


`
_10


\[auth.rate_limit\]


_10


# Number of Web3 logins that can be made in a 5 minute interval per IP address.


_10


web3 = 30


_10


_10


\[auth.captcha\]


_10


enabled = true


_10


provider = "hcaptcha" # or other supported providers


_10


secret = "0x0000000000000000000000000000000000000000"


`


## How we built it?#


At Supabase we cherish our community and our contributors. For this feature, we asked our community for help by co-sponsoring a bounty with the Solana Foundation. We asked them to[help us find a contributor](https://x.com/nickfrosty/status/1880046815950836179) who knows the ecosystem well to help us launch:


And we[found](https://x.com/Bewinxed/status/1880148679165976930) someone great!


> Being in the Web3 space, specifically in Solana since 2021, I've used Supabase on a few projects. So as soon as I saw the tweet saying Supabase needed help implementing Sign in with Solana, I had to jump in!
>
>
> It was interesting seeing the difference when building for Supabase scale, as opposed to building small personal projects. We put in a lot of effort to provide the Web3 community with the great DX Supabase is known for, security and standards compliance.
>
>
> Omar (Bewinxed), Supabase contributor and author of deauth.xyz


Once we made Sign in with Solana available in April, we decided to further our collaboration and continue working on the Sign in with Ethereum implementation. Omar has continued working with us on other exciting features coming soon!


## Start using Sign in with Web3 today#


Real-world use cases for Web3 authentication are already here. Developers are using wallet-based sign-in to power:


- NFT marketplaces where collectors can trade digital assets securely
- DAOs that rely on wallet verification for membership and voting
- Token-gated applications that unlock features based on wallet contents
- DeFi dashboards that let users manage assets without creating yet another password


You can get started right now:


- Check out the[docs for Sign in with Web3](https://supabase.com/docs/guides/auth/auth-web3)
- [Sign up for Supabase](https://www.supabase.com/dashboard) and try it in your project


We can’t wait to see what you build.
