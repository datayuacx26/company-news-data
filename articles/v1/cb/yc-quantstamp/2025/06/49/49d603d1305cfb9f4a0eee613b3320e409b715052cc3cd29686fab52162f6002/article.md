---
schema_version: "1.0.0"
document_id: "49d603d1305cfb9f4a0eee613b3320e409b715052cc3cd29686fab52162f6002"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/quantstamp-submits-first-idle-governance-proposal"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:c222a780356b99928de32a18bb6e02f9c837634d930d75721118dd2e514ac158"
---

# Quantstamp Submits First Idle Governance Proposal

## Gov Tokens Allocation Fix in Idle


On December 14th, a minor bug in the governance tokens distribution module in Idle protocol was reported.


The incident does not involve any deposited funds in Idle protocol (Best-Yield or Risk-Adjusted strategies) nor the accrued yield provided by the underlying protocols.


Governance tokens distribution ($IDLE and $COMP) is affected by the bug under specific circumstances, hence resulting in a misallocation of a small number of tokens to liquidity providers. According to the initial assessment, approximately ~150 IDLE and ~1 COMP have been misallocated since the launch of Idle Governance.


The bug has already been mitigated by a joint effort with Quantstamp and Idle team members, and Quantstamp has proposed a patch via a governance proposal, IIP-1. For security reasons, Quantstamp and the Idle team will fully disclose the bug once the on-chain proposal is implemented.


### **Core Facts**


- Assets are not at risk and never have been
- Idle protocol continues its operations and is not paused, you can deposit/withdraw assets anytime, everything is working as expected
- Idle protocol’s contracts can be upgraded on-chain via community governance, so there is no need to withdraw assets or move them to new contracts
- The patch is already running and mitigates possible future issues
- The on-chain proposal will permanently fix the issue (expected implementation in 5 days)


Quantstamp collaborated with the Idle team to investigate this inquiry, identifying the vulnerability and working on both the temporary mitigation patch and the final proposal.


### **Next Steps** ‍


The on-chain proposal, IIP-1, launched by Quantstamp is available[here](https://idle.finance/#/governance/proposals/1) .


Idle Governance has 3 days to cast its vote, in favor or against it. If the “For” vote wins and 4% of IDLE tokens have casted a vote, IIP 1 will be implemented after 2 days (grace period).


If you want to get in touch with the Idle team, feel free to join their community on[Twitter](https://bit.ly/3oCEtxK) ,[Discord](http://bit.ly/36PX9kJ) , or[Telegram](http://bit.ly/2X2RhQY) .
