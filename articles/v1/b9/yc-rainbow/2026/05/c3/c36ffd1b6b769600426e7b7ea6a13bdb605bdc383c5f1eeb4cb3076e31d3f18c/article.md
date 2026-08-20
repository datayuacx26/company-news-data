---
schema_version: "1.0.0"
document_id: "c36ffd1b6b769600426e7b7ea6a13bdb605bdc383c5f1eeb4cb3076e31d3f18c"
company_key: "yc-rainbow"
company: "Rainbow"
source_id: "yc-rainbow-atom-0b2ea2826f28"
canonical_url: "https://github.com/rainbow-me/rainbowkit/releases/tag/%40rainbow-me%2Frainbowkit%402.2.11"
published_at: "2026-05-06T08:42:36+00:00"
first_seen_at: "2026-07-25T20:24:22.592367+00:00"
fetched_at: "2026-08-20T01:52:49.727604+00:00"
content_hash: "sha256:346683cf8a71d66312da664b5675404402d78c4182090ff78d791841cb45f8ba"
---

# @rainbow-me/rainbowkit@2.2.11

### Patch Changes


-


[a40b1f4](https://github.com/rainbow-me/rainbowkit/commit/a40b1f41623550fa961284c18e4da0a49131066c) : Migrate the Base connector to canonical` base` naming, while preserving backwards-compatible aliases` baseAccount` and` coinbaseWallet` .


-


[3672dc6](https://github.com/rainbow-me/rainbowkit/commit/3672dc6ad3eb3ab161eb5109260abb24bcdedf62) : Added Anchorage Digital wallet support with the` anchorageDigitalWallet` wallet connector.


-


[1043d88](https://github.com/rainbow-me/rainbowkit/commit/1043d88f2994eb8e0384177f594a552903107025) : Added MeCo Wallet support with` mecoWallet` wallet connector.


-


[f52657f](https://github.com/rainbow-me/rainbowkit/commit/f52657fc22a5ae7439a04fbe1a10f2d3e5cca85b) : Exposed` RainbowKitProviderProps` and` WalletButtonRendererProps` as public type exports to support Custom Wallet Button scenarios.


-


[4f2de17](https://github.com/rainbow-me/rainbowkit/commit/4f2de17b1611295110b1348caf6b19551dca0be5) : Fixed a crash that could occur when selecting a wallet while multiple browser wallet extensions were installed and the specific injected wallet was missing. Wallet-specific injected connectors now bind only to their matching provider instead of falling back to available defaults.


-


[bc4625c](https://github.com/rainbow-me/rainbowkit/commit/bc4625c096233356039318275f25ef587a1f11f0) : Fix recent transaction tracking so failed transactions no longer prevent an app's own transaction receipt wait from settling.


-


[25c4c2b](https://github.com/rainbow-me/rainbowkit/commit/25c4c2b2637e25b9e6391da9ce8d4afdb6efd52b) : Improved SSR safety to prevent WalletConnect initialization warnings and mitigate localStorage API availability changes in Node.js v25 and above.


-


[f52657f](https://github.com/rainbow-me/rainbowkit/commit/f52657fc22a5ae7439a04fbe1a10f2d3e5cca85b) : Fixed` useWindowSize` triggering a state update after unmount, which could surface as a React warning.


-


[eb4251d](https://github.com/rainbow-me/rainbowkit/commit/eb4251d9012b9a28e4e15f9fb24c8b60624e8cce) : The` AuthenticationAdapter.createMessage` API can now return a promise, so dApps can fetch or construct a custom SIWE message asynchronously. This enables server-side SIWE message creation before prompting the wallet, while preserving existing synchronous behavior.


See the[server-side message creation docs](https://github.com/rainbow-me/rainbowkit/blob/%40rainbow-me/rainbowkit%402.2.11/docs/custom-authentication#server-side-message-creation) for guidance.


-


[b0f6d52](https://github.com/rainbow-me/rainbowkit/commit/b0f6d525ea7a149d4d0e3a1df39405b0f060a04b) : fix: harden useCoolMode against malicious wallet icon URLs


The cool mode particle animation built image elements via` innerHTML` , which
parses its input as HTML. A malicious EIP-6963 wallet could supply a crafted
icon URL containing injected attributes (e.g.` onerror` ) that would execute
in the dApp's origin when a user interacts with the wallet button.


Switched to` document.createElement('img')` with property assignment so the
icon value is always treated as a plain URL rather than markup.


-


[f2523a9](https://github.com/rainbow-me/rainbowkit/commit/f2523a9517a438e3c213b4245004828da1b6bb27) : Updated MetaMask wallet icon
