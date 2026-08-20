---
schema_version: "1.0.0"
document_id: "7c4428ecc40b5a76f3ed4dcc81e2c294f42c67ccf3b402f31231feb7c08c5cad"
company_key: "yc-cointracker"
company: "CoinTracker"
source_id: "yc-cointracker-news-import-33dd9daf8236"
canonical_url: "https://www.cointracker.com/blog/nasdaq-and-kraken-are-building-tokenized-stock-infra"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-24T23:51:14.288459+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:839a1550dde54028c8c5ad7ac942b0f9f024edfe82c4d823bfb133e7bf987a44"
---

# Nasdaq and Kraken are building tokenized stock infrastructure

On March 9, 2026, Nasdaq and Kraken[announced](https://www.wsj.com/finance/stocks/nasdaq-partners-with-kraken-in-tokenization-push-135e8112?mod=Searchresults&pos=1&page=1) a landmark partnership to build the first regulated 24/7 tokenized stock trading platform, targeting a 2027 launch. Every headline focused on the market structure angle: 24/7 settlement, fractional ownership, blockchain rails. What nobody asked: if your company holds tokenized NVDA or TSLA shares on-chain, what exactly goes on your balance sheet?


The answer, as of today, is: nobody knows.


And that's a problem your controller needs to start thinking about now.


## What the Nasdaq–Kraken deal actually does


The partnership expands Kraken's existing[xStocks](https://coinpedia.org/news/nasdaq-and-kraken-are-building-24-7-tokenized-stock-trading-launch-set-for-2027/) platform, already processing $25B+ in volume across 85,000+ unique holders in Europe, into a fully regulated venue backed by a major U.S. exchange operator. The key structural details that matter for accounting:


- Each token represents 1:1 ownership of an underlying share, carrying the same CUSIP identifier as the traditional share.
- Tokenized shares confer full governance rights: voting, dividends, corporate actions, identical to traditional equity.
- Settlement occurs on-chain, bypassing the traditional T+1 clearinghouse infrastructure (DTCC).
- U.S. investors are excluded initially; Europe is the primary market. U.S. access is pending SEC approval.


Nasdaq is also separately pursuing SEC[approval](https://www.coindesk.com/business/2026/03/09/nasdaq-and-kraken-are-teaming-up-to-let-you-trade-tokenized-stocks) to make tokenized stocks the official digital version of the shares themselves, not derivatives, not synthetic exposure. If approved, a tokenized AAPL share would be legally and structurally equivalent to a traditional AAPL share, just settled on blockchain.


## The accounting gap no one is talking about


Here is where the conversation has been missing the forest for the trees. Three distinct frameworks could theoretically apply to tokenized stocks, and none of them quite fit.


**ASU 2023-08 (crypto assets): close, but no cigar**


FASB's new crypto accounting standard requires fair value measurement with P&L recognition for covered crypto assets. Tokenized stocks, however, are explicitly excluded. They are securities, not crypto assets as defined under ASU 2023-08. The moment a token carries a CUSIP and confers voting rights, it steps outside the scope of that guidance.


**Traditional equity accounting: almost right**


Under ASC 321 (equity securities with readily determinable fair values), a company holding tokenized NVDA shares would recognize them at fair value, with unrealized gains and losses flowing through net income. This framework is the closest match in substance. The economic exposure is identical to holding traditional shares.


The gap: ASC 321 was written for securities settled through DTCC's infrastructure. On-chain settlement creates open questions around custody, control, and counterparty risk that the standard never contemplated. Who is the counterparty when settlement is a smart contract? Does the absence of a broker of record affect recognition?


**The SEC's January 28 taxonomy: necessary but not sufficient**


The SEC's January 28, 2026 tokenized securities guidance clarified the regulatory classification question: tokenized stocks remain securities, subject to existing securities law, regardless of how they settle. That answers the regulatory question. It does not answer the accounting question.


The SEC's guidance is silent on how to classify on-chain custody arrangements, how to treat unrealized settlement lag in smart contract execution, or how to account for fractional tokens that don't correspond to whole shares.


**The bottom line for controllers:** GAAP guidance for on-chain equity settlement simply does not exist yet. A company holding tokenized NVDA shares today would have to construct its own accounting policy and defend it to auditors using analogical reasoning across multiple partial frameworks. That is a significant audit risk.


## Why none of the existing frameworks quite fit


To illustrate the gap, consider how existing frameworks handle a single tokenized equity position:


Framework Applies to tokenized stocks? Measurement Key gap


ASU 2023-08 No: securities excluded N/A Explicitly out of scope for equity tokens


ASC 321 (equity) Partially Fair value, P&L On-chain custody and settlement not addressed


ASC 946 (investment co.) If entity qualifies Fair Value Narrow entity scope; most corps don't qualify


SEC Jan. 28 guidance Regulatory only N/A Classification only; silent on GAAP treatment


## What this means for your close process


Even if you're not holding tokenized stocks today, the accounting policy decisions your peers make in the next 12 to 18 months will set industry precedent. Here are the specific areas where on-chain equity settlement creates new complexity for the accounting close.


**Custody and control**


Traditional equity positions are held at a broker-dealer, creating a clear custodial chain. Tokenized stocks held in a self-custodied wallet or on an exchange like Kraken create ambiguity around who controls the asset for derecognition purposes under ASC 860. If the token is locked in a smart contract for settlement, does the company still have control during that window?


**Fair value hierarchy**


Under ASC 820, the fair value hierarchy requires using Level 1 inputs (quoted prices in active markets) when available. Tokenized stocks trading 24/7 on thinner on-chain markets may diverge from the underlying equity price, particularly outside traditional market hours. Which price is the authoritative measurement: the on-chain token price at midnight, or the exchange close on the underlying?


**Dividend and corporate action accruals**


If tokenized shares carry dividend rights, the accrual methodology should mirror traditional equity. But smart contract delivery of dividends introduces questions around timing of recognition, particularly if the dividend is settled in a stablecoin rather than fiat.


**Disclosure requirements**


Both SEC Regulation S-X and GAAP require disclosure of significant accounting policies and concentrations of risk. A company holding material tokenized equity positions will need to develop disclosure language that doesn't yet exist in any 10-K filing.


## Practical steps controllers should take now


The 2027 launch timeline may feel distant, but accounting policy development, auditor alignment, and internal control buildout take time. If your company operates in markets where tokenized stocks are already available (Europe, Asia), these questions are urgent today. The firms that develop defensible accounting frameworks now will be positioned to move quickly when U.S. regulatory approval arrives. The ones that wait will be scrambling.


**Initiate an accounting policy memo.** Document your position on classification and measurement of any tokenized equity holdings, even if the position is zero today. Establish the framework before holdings exist.


**Engage your external auditors proactively.** The absence of authoritative guidance means auditors will exercise significant judgment. Align early on the analogical framework you'll use (ASC 321 is the strongest candidate) and the specific custody and fair value questions unique to on-chain settlement.


**Monitor FASB and SEC developments.** FASB has indicated interest in expanding its digital asset agenda. The SEC's tokenized securities taxonomy is likely to be updated as Nasdaq's and NYSE's applications progress.


**Build the right accounting infrastructure now.** Tokenized equity accounting will require real-time on-chain position tracking, cost basis calculation, and audit-ready records at the transaction level. CoinTracker's subledger already handles this for crypto and digital asset positions, and is built to extend to tokenized securities as the regulatory framework matures. Finance teams that have this infrastructure in place won't need to rebuild from scratch when U.S. access opens.


**Watch for industry guidance.** The AICPA's Digital Assets working group and the Center for Audit Quality are likely to publish practice aids as tokenized stocks gain U.S. traction. These will be critical reference points.


---


*CoinTracker Enterprise helps finance teams at crypto-active companies close the books faster, stay audit-ready, and build accounting infrastructure that holds up under scrutiny. If tokenized equity is already on your radar, or you're building out your digital asset accounting foundation,[talk to our team](https://calendly.com/d/cmqg-nfk-sr5) .*


*This post is for informational purposes only and does not constitute accounting, legal, or tax advice. Consult a qualified professional for guidance specific to your situation.*
