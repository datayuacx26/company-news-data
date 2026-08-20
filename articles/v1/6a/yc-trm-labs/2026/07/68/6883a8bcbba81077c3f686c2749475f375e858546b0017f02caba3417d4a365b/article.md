---
schema_version: "1.0.0"
document_id: "6883a8bcbba81077c3f686c2749475f375e858546b0017f02caba3417d4a365b"
company_key: "yc-trm-labs"
company: "TRM Labs"
source_id: "yc-trm-labs-news-import-b34814ebf689"
canonical_url: "https://www.trmlabs.com/resources/blog/screening-htx-beyond-the-uk-designation-why-blockchain-intelligence-matters"
published_at: "2026-07-21T21:45:00+00:00"
first_seen_at: "2026-07-22T17:19:07.599741+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:1ca2ef456be70f8a4c0ab3cb1af51456c784d8cd008b5197ad902d5dde6132ed"
---

# Screening HTX Beyond the UK Designation: Why Blockchain Intelligence Matters

On May 26, 2026, the UK sanctioned one of the world’s largest crypto exchanges for its alleged role in Russian sanctions evasion. In the seven weeks since, TRM has tracked HTX’s on-chain response in near real time. What the exchange did next shows why sanctions enforcement in crypto now depends on tracking behavior as much as addresses.


[TRM’s earlier analysis](https://www.trmlabs.com/resources/blog/uk-designates-htx-what-the-biggest-crypto-sanctions-action-yet-means-for-compliance-teams) covered what the designation means for compliance teams. This follow-up covers what HTX has done on-chain since.


## Key takeaways


- On May 26, 2026, the UK (FCDO/OFSI) designated Huobi Global S.A. — the entity behind HTX — under the Russia (Sanctions) (EU Exit) Regulations 2019, the first time a top-five exchange was sanctioned and the first UK use of Regulation 17A against a crypto exchange.
- In the seven weeks since, HTX has stayed operational under the same brand but rebuilt its on-chain plumbing, rotating hot wallets and funding addresses on regular cycles across TRON, Ethereum, BNB Smart Chain, and Solana.
- Address-list screening cannot keep pace. A static block list goes stale within hours and would clear most post-designation HTX activity. Behavior-based attribution follows the rotating infrastructure as new wallets come online.


{{horizontal-line}}


## The UK’s designation of HTX


The UK Foreign, Commonwealth and Development Office (FCDO)[designated Huobi Global S.A.](https://www.gov.uk/government/news/uk-cracks-down-on-backdoor-russian-sanctions-evasion-with-tough-new-measures) — the entity behind the exchange HTX — on May 26, 2026, under the Russia (Sanctions) (EU Exit) Regulations 2019. The designation of HTX was the first time an exchange of its size was sanctioned, and it was the first time the UK applied Regulation 17A of those regulations to a crypto exchange.


According to the FCDO, the UK determined there were reasonable grounds to suspect that HTX supported the government of Russia by providing financial services to entities in the A7 network, including A7 LLC and Garantex. The FCDO alleges HTX moved approximately USD 1.5 billion for Kremlin-aligned entities, and connected the action to Operation Destabilize, the National Crime Agency (NCA)-led operation targeting Russian money laundering infrastructure.


## How HTX responded on-chain


The blockchain keeps a permanent, public record of every transaction, enabling investigators to spot shifts in on-chain infrastructure as they happen. In the weeks after designation, TRM analysis found that HTX restructured its on-chain operations across multiple blockchains, in an attempt to work around that transparency.


HTX kept operating under the same brand, but began rotating its wallet infrastructure on a rapid cycle. The effect of this cycling is a continuous moving target for compliance professionals. A block list built on specific HTX addresses goes stale quickly, because the exchange retires each address and shifts activity to fresh ones faster than a static list can be updated. This keeps a large share of HTX’s live infrastructure outside any fixed address list at any given moment.


TRM graph: HTX rotating hot wallets multiple times per day to process customer withdrawals


## Keeping pace with rapidly changing wallet infrastructure


Screening against a list of known addresses cannot keep pace with infrastructure that turns over this quickly. By the time a single address is identified and distributed, the exchange has already moved on to new infrastructure.


[Blockchain intelligence](https://www.trmlabs.com/glossary/blockchain-intelligence) closes that gap by attributing the behavior that defines the rotating infrastructure instead of chasing one address at a time.


When an entity spins up a new wallet, behavior-based attribution recognizes it and ties it back to the designated entity as it comes online, updating nearly as fast as HTX rotates.[TRM Wallet Screening](https://www.trmlabs.com/blockchain-intelligence-platform/wallet-screening) , for example, checks counterparties against behavior-based attribution rather than a fixed address list, so new wallets are caught as they appear. That keeps screening current against a moving target, surfaces indirect exposure even when the immediate counterparty is a brand-new address, and preserves the on-chain evidence trail that law enforcement and regulators rely on.


Without behavior-based attribution, a compliance program screening against known HTX addresses would clear most post-designation HTX activity, because most of it runs through addresses no list has seen.


## A familiar pattern


HTX’s response fits a pattern TRM has tracked consistently across the illicit-finance ecosystem: well-resourced sanctioned entities rarely disappear. They adapt.


After[Garantex was disrupted in a March 2025 multinational takedown](https://www.trmlabs.com/resources/blog/garantex-grinex-and-the-a7a5-token-a-deep-dive-into-sanctions-evasion-networks) , its operators activated a pre-positioned successor, Grinex, and migrated liquidity through the ruble-pegged stablecoin A7A5 — a reconstitution at the entity level, with a new brand and new jurisdiction.


HTX has adapted differently, but with the same intent. Rather than rebranding, the exchange rebuilt its on-chain infrastructure. TRM’s assessment compared HTX’s post-designation wallet rotation to the initial playbook Garantex adopted after its own 2022 designation. In this way, sanctioned entities are able to stay operational by turning their on-chain footprints into moving targets.


## Compliance takeaways


### 1. Know which regime applies to you


The UK (OFSI) and the EU designated HTX; but the US Treasury’s OFAC has not, so a firm’s obligations depend on its jurisdiction. UK and EU regulated firms face asset-freeze and reporting duties, while firms elsewhere have no automatic freeze obligation from this action. Every firm, regardless of jurisdiction, should still treat HTX as elevated[sanctions](https://www.trmlabs.com/glossary/sanctions) evasion risk and watch for follow-on action given its central role in the A7 network.


### 2. Treat any list of HTX addresses as a point-in-time snapshot


The exchange retires each hot wallet and funding address within hours, so a static block list goes stale faster than it can be updated and will clear most live HTX activity. Screen instead against behavior-based attribution that follows the rotating infrastructure, so new wallets are recognized as they come online.


### 3. Run look-back exercises to identify pre-designation exposure


This includes exposure to A7-network and Garantex-successor entities reached through HTX. Weight what you find by timing and direction: exposure before the designation is different from ongoing exposure after it, and funds sent to HTX raise different questions than funds received from it. Document when each flow occurred so the review supports a defensible decision rather than a raw percentage.


### 4. Segment customers by exposure level


Apply consistent review frameworks tied to sanctions evasion typologies. Set tiered thresholds by risk so analyst attention follows the highest-risk exposure, and use periodic review for lower-risk cohorts. This keeps the alert volume from HTX’s rotating infrastructure manageable without letting the most serious exposure slip through.


### 5. Assess indirect risk by proportion, not distance


Weight transaction size, high-risk jurisdictions, shell company structures, and intermediary patterns; and favor risk-weighted, percentage-based exposure (how much flow reaches a designated entity) over a fixed hop count, which bad actors defeat by adding intermediary hops. Where a hop range is used for monitoring, document the risk-based rationale behind it.


### 6. Apply enhanced due diligence to fiat-pegged tokens with opaque governance


Watch for successor platforms, rebrands, and shared-infrastructure clusters around designated entities. Garantex showed the entity-level version of this with Grinex and the A7A5 token; HTX shows the wallet-level version. Expect the patterns to keep evolving, and treat any single view of the infrastructure as temporary.


TRM continues to track HTX’s on-chain activity and the broader Russian sanctions evasion ecosystem, and will report on how this infrastructure evolves.


{{horizontal-line}}


## Frequently asked questions (FAQs)


### 1. Who designated HTX, and does it apply to my firm?


The UK Foreign, Commonwealth and Development Office (FCDO), through the Office of Financial Sanctions Implementation (OFSI), designated Huobi Global S.A. on May 26, 2026, under the Russia (Sanctions) (EU Exit) Regulations 2019. OFAC and the EU have not designated HTX. UK-regulated firms face asset-freeze, reporting, and suspicious activity report (SAR) duties; firms outside UK jurisdiction have no automatic freeze obligation but should treat HTX as elevated sanctions-evasion risk and watch for follow-on action.


### 2. What did HTX do on-chain after the designation?


HTX kept operating under the same brand but began rotating its deposit and hot wallets — and then the funding addresses that seed them — across TRON, Ethereum, BNB Smart Chain, and Solana, turning its on-chain footprint into a continuous moving target.


### 3. Why doesn’t a block list of HTX addresses work anymore?


Because HTX retires each hot wallet and anchor within hours, a static block list goes stale faster than it can be validated and distributed, and would clear most live HTX activity. Behavior-based attribution recognizes each new wallet as it comes online and ties it back to the designated entity, keeping screening current against the rotation.


### 4. What should compliance teams do now?


Screen against behavior-based attribution rather than static address lists; run look-back exercises weighted by timing and direction (pre- versus post-designation, sent versus received); confirm which sanctions regime applies to your firm; segment customers by exposure level; and apply enhanced due diligence to opaque fiat-pegged tokens and emerging successor or shared-infrastructure clusters.
