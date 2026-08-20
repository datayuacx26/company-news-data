---
schema_version: "1.0.0"
document_id: "7bf8856ef5d6752deb745f9d953a34888e525e092ae3ddfc0c0321edd9253122"
company_key: "aci-worldwide-inc-common-stock"
company: "ACI Worldwide Inc."
source_id: "aci-worldwide-inc-common-stock-rss-ca67c52d46dd"
canonical_url: "https://www.aciworldwide.com/blog/real-time-payments-pay-by-bank-orchestration-strategy"
published_at: "2026-06-04T23:52:41+00:00"
first_seen_at: "2026-07-25T01:10:51.435202+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:c8d559414dfd13437fbdd6581f8c373e8e1dec13f1f744d7cba42055d29c4f2f"
---

# Real-time payments outrun the rules: Craig Ramsey on pay-by-bank and orchestration

**ACI Worldwide’s[Craig Ramsey](https://www.aciworldwide.com/author/craig-ramsey) , who leads account-to-account payments and joins the sovereignty without fragmentation panel at Payments Unleashed EMEA, on what stays slow after a payment moves in an instant, why the rules are catching up market by market, and what a bank should modernize first.**


Craig Ramsey leads account-to-account payments at ACI, giving him a close view of how banks and merchants actually move money across batch systems like ACH,[high-value RTGS transfers](https://www.aciworldwide.com/solutions/aci-high-value-real-time-payments) , instant rails that make funds available within seconds, and card networks. The technology has changed faster than the rules around it. A bank can now route a payment to the rail that best fits it, by cost, speed, and risk, and send it instantly when speed is what matters. What has not moved as fast, he says, is the surrounding rulebook on liability, recovery, and dispute, much of it still shaped for a slower, single-rail, nationally bounded world. The money can arrive in seconds; what happens if it goes wrong is still measured in days.


Ramsey is Global Head of Account-to-Account Payments at ACI Worldwide. At Payments Unleashed EMEA in London, he joins the main-stage panel on sovereignty without fragmentation and moderates a banking breakout. He is candid about where the technology has moved ahead of the rules, and what he would fix first.


## When the rails outrun the rulebook


Banks and providers support many rails — batch ACH, high-value RTGS, and instant systems like FedNow and SEPA Instant Credit Transfer, with card networks alongside them — and they want a[platform that sends each payment over whichever one fits](https://www.aciworldwide.com/solutions/payments-hub) , by cost, speed, and risk. The problem is that the rules still sit rail by rail, with different requirements for message formats, liquidity, dispute handling, and fraud liability. The capability is in place; the rulebooks underneath it are not aligned.


---


***“The industry has built real-time, multi-rail payment capability,”* Ramsey says, *“but policy is still defined by single-rail, slower, and nationally bounded assumptions.”***


---


The rules are also moving. In euro-area countries, banks and other in-scope payment service providers had to be able to[receive instant euro transfers from January 2025 and to send them from October 2025](https://finance.ec.europa.eu/news/new-eu-rules-make-instant-euro-payments-faster-and-safer-2025-10-10_en) . Separately, they must now verify the payee before a transfer is authorized, and an instant transfer has to make the funds available within 10 seconds. Ramsey’s point is not that policy has failed to move, but that it is moving unevenly, with each jurisdiction setting its own liability, fraud, and operating rules, which is what makes orchestrating across them hard.


## Real-time money, slower-moving risk


Instant payments changed how quickly funds become available. They did not, by themselves, replace the controls, recovery processes, and liability models that slower clearing windows used to support. When processing took longer, there was time to intervene, return, investigate, or recover before the funds were gone, and fraud controls, liability models, and dispute mechanisms were all built around that time. Authorized push payment fraud can now move at instant speed, while the rules on who carries the loss vary sharply by market. In the UK, qualifying domestic APP scam claims over Faster Payments and CHAPS are covered by a reimbursement regime, with the sending and receiving payment firms splitting the cost 50:50 and most eligible victims repaid within five business days. The US draws the line differently: Regulation E separates transfers a customer never authorized from payments they knowingly made, though a fraudster using credentials obtained by deception may still count as unauthorized, and some providers go beyond the legal minimum. Those are materially different liability regimes, and a firm operating across both has to build for each.


## Sovereignty without fragmentation


Sovereignty is the subject of his panel, and Ramsey does not see it as a binary. The drive for domestic control, through local rails, pay-by-bank, and data-residency rules, sits alongside the need to work across borders: corporates and fintechs scaling one product into many markets, and banks that would rather reuse infrastructure than rebuild it country by country. Reconciling them is far from settled, because a bank can meet every local requirement and still be left stitching together different KYC and AML standards, data-residency obligations, and scheme-specific overlays for each market it enters.


## What real-time will, and will not, do


Ramsey rejects the idea that retail instant payments will replace everything else. *“RTGS and ACH remain strong payment types in their own right,”* he says, with use cases that keep them in place for years. High-value settlement over RTGS, which is itself real-time, and batch processing over ACH each do work the retail instant rails are not built for. What instant payments displace depends on the market: often cash and checks, sometimes batch transfers or cards, sometimes very little. Where instant is increasingly the default is in new payments — it is becoming the destination rail of choice for transactions that did not exist on the older systems, starting with the small-value, machine-to-machine flows tied to the Internet of Things. Some digital transactions are also heading this way, especially wallet-to-wallet movements where immediate fiat settlement is not always the point.


Ask him which one system solved a problem at scale, Faster Payments, SEPA Instant Credit Transfer, FedNow, or UPI, and he will not pick one. *“They all solve a problem,”* he says, *“it’s just not one-problem fits all.”* Each addressed something different for the people who use it, whether speed, security, or faster reuse of money. ACI’s own research[recorded 266.2 billion real-time transactions globally in 2023, up 42.2% year over year](https://www.aciworldwide.com/real-time-payments-report) , with almost one-fifth of all electronic payments now real-time and more than a quarter forecast by 2028.


## Where pay-by-bank works, and where it does not


On pay-by-bank, the economics and the execution are at different stages. Avoiding card interchange can lower the cost of acceptance in the right cases, though it is not a clean saving on its own: a merchant still pays provider and transaction fees, integration and operating costs, fraud and dispute handling, and reconciliation. Online, with a logged-in or returning customer, it works well, because the customer is already known, the merchant can prompt them directly, and a familiar request inside a purchase they are already making can reassure on security. In a physical store it is harder, and the in-store proposition is less mature in most markets. Adoption is already substantial, though: Open Banking Limited reports more than 17.9 million live user connections across the UK, and open-banking payments reached more than 37 million in March 2026 alone, though that total includes use cases well beyond merchant checkout.


On the customer side, his point is about timing. What people want, he says, is certainty about what they can spend the moment they pay, instead of waiting for a debit to post and then reconciling a cleared balance against an available one. As he puts it, why should they?


## Where one budget should go


Given a single modernization priority over the next 18 months, his answer is consolidation. *“Consolidating multiple payment flows into one place,”* as he puts it, so that new products, new rails, and new regulation are easier to take on. In practice that points less at a single processing engine than at unified control: routing and orchestration, data, monitoring, and fraud management run together, even where several fit-for-purpose rails still sit underneath. That does not mean building one giant point of failure. The shared control layer still has to be resilient and independently recoverable, or consolidation simply creates a different risk. The question for banks is the reverse: what should deliberately stay local, rail-specific, or independently recoverable?


## Hear Craig Ramsey in London


Craig Ramsey, Global Head of Account-to-Account Payments at ACI Worldwide, joins the main-stage panel on sovereignty without fragmentation and moderates a banking breakout at[Payments Unleashed EMEA](http://events.aciworldwide.com/paymentsunleashedemea) . The event opens with a reception on June 29 at 12th Knot, Sea Containers, and runs a full day on June 30 at the Hilton London Bankside, with sessions on real-time payments, fraud and scam liability, and payments sovereignty across Europe and the Middle East.


Routing across rails, who carries the loss when an instant scam succeeds, and what to modernize first are the questions he will be working through.


**Register for Payments Unleashed EMEA – London 2026**


Registration is complimentary, and applications are reviewed for a senior payments audience.[Register today to secure your place](http://events.aciworldwide.com/paymentsunleashedemea) .
