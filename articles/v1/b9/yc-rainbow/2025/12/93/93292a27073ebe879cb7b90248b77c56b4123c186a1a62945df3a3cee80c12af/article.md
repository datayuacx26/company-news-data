---
schema_version: "1.0.0"
document_id: "93292a27073ebe879cb7b90248b77c56b4123c186a1a62945df3a3cee80c12af"
company_key: "yc-rainbow"
company: "Rainbow"
source_id: "yc-rainbow-atom-0b2ea2826f28"
canonical_url: "https://github.com/rainbow-me/rainbowkit/releases/tag/%40rainbow-me%2Frainbowkit%402.2.10"
published_at: "2025-12-07T08:10:49+00:00"
first_seen_at: "2026-07-25T20:24:22.592367+00:00"
fetched_at: "2026-08-20T01:52:49.727604+00:00"
content_hash: "sha256:8f9b38072493b968627bdea3167377c71cf1428135e677b44ae85e67fe06d521"
---

# @rainbow-me/rainbowkit@2.2.10

### Patch Changes


-


[e74f604](https://github.com/rainbow-me/rainbowkit/commit/e74f6044a26f1f6141b59137257657aa3821e320) : Improve UI on the mobile connect flow to hint to users that they can horizontally scroll to see additional wallet connectors


-


[eb72c37](https://github.com/rainbow-me/rainbowkit/commit/eb72c37883d14806713c096fee65434025132f4d) : Fix Gemini wallet connector to use` icon` instead of` icons` in` appMetadata`


-


[e58367e](https://github.com/rainbow-me/rainbowkit/commit/e58367e90bae780d6402b3fe0615d58dc3a89fcc) : Fix mobile visibility for Coin98, CLV, SafePal, Frontier, and BeraSig wallets.


-


[b7b7b43](https://github.com/rainbow-me/rainbowkit/commit/b7b7b43eb30ae907b2d389e448fb223859915ae8) : Rename the Argent wallet connector to` readyWallet`


-


[507f583](https://github.com/rainbow-me/rainbowkit/commit/507f5835274adfde00eda0a5b0ea7c046f7c6f93) : Add additional wallet flags to` isMetaMask()` to detect impersonating providers.


-


[16963de](https://github.com/rainbow-me/rainbowkit/commit/16963debf387ba24e8f90dae708302c0f7fe7b7d) : Add` ctrlWallet` wallet connector to replace` xdefiWallet` . XDEFI Wallet has been rebranded to CTRL Wallet.


-


[6c745a5](https://github.com/rainbow-me/rainbowkit/commit/6c745a5a51d33e7e3c99ec93e7a46263e3d4ec2f) : Disable third-party connector telemetry by default for user privacy. h/t[@TimDaub](https://github.com/TimDaub)


**To opt-in to WalletConnect analytics:**


With` getDefaultConfig` :


```text
const    config    =    getDefaultConfig  (  {
/** ... **/
walletConnectParameters  :  {
telemetryEnabled  :  true  ,
}  ,
}  )  ;
```


**To opt-in to Base Account telemetry:**


```text
baseAccount  .  preference    =    {
telemetry  :  true  ,
}  ;
```


**To opt-in to MetaMask analytics:**


```text
metaMaskWallet  .  enableAnalytics    =    true  ;
```
