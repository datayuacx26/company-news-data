---
schema_version: "1.0.0"
document_id: "80194d8053d5b3e123c090e99baf1aad3675bfd2ad358a3ac5410f2f16360e0a"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/quantstamp-audits-ichis-onetoken-repository"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:622923d265c12d983a381a8154ca39975cbaacb608e256801be78bd83def6417"
---

# Quantstamp Audits ICHI's oneToken Repository

Quantstamp completed an audit of the[ichi-oneToken repository](https://github.com/ichifarm/ichi-oneToken) that corresponds to the ICHI protocol. We audited the repository at the[09fca74 commit](https://github.com/ichifarm/ichi-oneToken/commit/09fca7431a18028779d36427ab3ccdf947e43d40) for the initial report and the[11c210e commit](https://github.com/ichifarm/ichi-oneToken/commit/11c210ee87509128f35ed6636f0cb3d77733e74b) for the final report. The ichi-oneToken repository contains contracts that govern the deployment of oneToken instances.[Click here](https://certificate.quantstamp.com/full/ichi) to view the public audit report.


*Resolved means that an audit finding was either fixed or mitigated. Acknowledged means that an audit finding was left as is. Clients often provide their reasoning for acknowledged items within the audit report.*


### ICHI’s Decentralized Monetary Authority


The ICHI protocol enables cryptocurrency communities to create and govern their own stablecoin through a Decentralized Monetary Authority (DMA). Community stablecoins created through ICHI are called oneTokens. oneTokens are pegged to 1 USD and minted by depositing a combination of stablecoins (like USDC) along with the oneToken’s *member coin* . For instance, to mint a oneWBTC token (an instance of a oneToken), users need to deposit USDC and WBTC.


*A list of oneTokens approved by ICHI governance.*


Stablecoins are governed by the communities that created them through a DAO-like governance model called the Decentralized Monetary Authority (DMA). Governance is managed by the oneToken holders. In the oneWBTC token holder community, each oneWBTC token gives the holder 1 vote during governance decisions, which may include how the Treasury and Collateral Reserve are managed.


When a oneToken like oneWBTC is minted using USDC and WBTC, WBTC tokens are stored in a Community Treasury. oneWBTC token holders can choose to use treasury funds however they would like. For instance, they may decide to use the treasury to earn yield in DeFi or offer a grant to individuals or organizations that further the development of the oneToken ecosystem.


The USDC used to mint a oneToken instance is stored in a Collateral Reserve; should oneToken holders decide to redeem their tokens for USDC, it is withdrawn from this reserve. Governance is also in charge of managing the Collateral Reserve, which may include choosing to earn yield with the Collateral Reserve in DeFi.


### ICHI Governance of oneToken Ecosystem


While each oneToken community governs their own stablecoin, ICHI token holders are in charge of the governance of the larger ecosystem. ICHI governance is needed to approve the deployment of new oneTokens along with governing the overarching parameters for oneToken instances that include things such as:


- The types of collateral that are acceptable to mint oneTokens (currently USDC)
- The strategies that oneToken governors can use to invest their respective Treasuries and Collateral Reserves in DeFi
- Oracles that can be used to track the value of digital assets


### Future Plans


ICHI plans to launch stablecoins for dozens of other projects in the coming months.


### Increasing Access to Finance


ICHI has not only created an innovative DeFi product that enhances access to stablecoin issuance, they have also taken the safety of their users seriously by choosing Quantstamp as their security provider for the ichi-oneToken repository. Quantstamp looks forward to continuing to secure the assets in your digital nation and working with future projects that also intend to increase access to the financial infrastructure of tomorrow.
