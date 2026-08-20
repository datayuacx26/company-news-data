---
schema_version: "1.0.0"
document_id: "4375023aa6aceea08fc6a5d8e7046baf4c3f0cde701520c258563c50fdc73c49"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/corgi-custom-fraud-model-stripe-radar-connect-2"
published_at: "2026-05-19T06:28:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:90a5db4381007c0bbf93b39afeb859afa79d5f68fd340b008e1f37eac0d69ca2"
---

# What Corgi Labs Actually Does on Top of Stripe Radar

## A 1.33 Million Transaction Test


Stripe Radar blocks fraud across its entire network. That's its strength and its limitation. When your merchant's fraud profile diverges from the platform average, Radar over-blocks. Good buyers get declined. Revenue disappears.


We evaluated CORGI on a live Stripe Connect merchant across 19 rolling out-of-time windows (May 2024 to December 2025, approximately 1.33 million transactions). The results changed how we think about what "good enough" fraud prevention actually costs. This article walks through 10 questions that cover every technical detail: how the model works, what data it needs, how it isolates lift from Radar, and where it does (and doesn't) add value.


### 1. Measurable Lift Over Radar on Stripe Connect


We evaluated CORGI across the 10 windows where the model produced actionable signal (AUC 82.7% to 96.6%). The numbers tell a clear story:


**Dispute rate reduction:** 0.41% to 0.17% (59.5% reduction, 1,667 disputes prevented)


**Flag rate:** 0.53% of transaction volume, meaning 99.47% of transactions pass untouched


**Aggregate precision:** 45.7% (nearly half of flagged transactions were true disputes)


**Net revenue impact:** +$508K over the evaluation period ($525K in dispute savings minus $17K in margin loss from false declines)


**How We Isolate Lift from Radar**


Three mechanisms ensure we measure incremental lift, independent of attribution overlap.


CORGI trains exclusively on Radar-approved traffic. Its learned signal is, by construction, orthogonal to what Radar already catches. Any dispute CORGI identifies is one Radar missed.


Radar's outcome_risk_score is included as an input feature. SHAP analysis confirms the model's top predictors are merchant-specific behavioral aggregates (card fingerprint dispute velocity, spend spike detection, email-level volume anomalies) rather than the Radar score itself.


We account for blocked-transaction bias using a statistical estimation framework. Radar blocks a subset of traffic before CORGI observes it, creating missing dispute labels. We address this by training a stratifier on approved transactions, bucketing blocked transactions into the same score bins using isotonic calibration, and running 500 bootstrap iterations where synthetic dispute labels are drawn from the calibrated bucket rates. Metrics are reported as mean ± 95% CI. Validation against approved-only real labels shows tight agreement. For example, card fingerprint dispute history >2 shows 39.4% precision on combined traffic versus 40.9% on approved-only (CI \[38.5%, 40.6%\]), confirming minimal distortion from the imputation.


### 2. Platform-Wide Intelligence Without Cross-Merchant Contamination


CORGI trains separate per-merchant models. Each connected account gets its own feature tables (corgi_features.{account_id}_features), its own model, and its own threshold calibration. There is no shared model across merchants. One merchant's fraud distribution cannot pollute another's.


The "platform-wide intelligence" comes from the feature engineering layer, not model weight sharing. Features like card fingerprint dispute counts across the platform, or cross-merchant velocity signals, are computed as read-only aggregate inputs. A merchant's model can observe that a card has been disputed elsewhere on the platform, but its training labels and decision boundary are scoped entirely to its own transaction history.


This architecture costs more to operate than a single cross-merchant model (which is what Radar does). That's precisely why CORGI captures merchant-specific fraud patterns that Radar cannot.


### 3. Shadow Mode Metrics and Timeline to Statistical Significance


**What We Benchmark**


During shadow mode, CORGI scores every transaction but enforces no decisions. This gives you a direct comparison: you observe the actual outcome of every transaction and can measure exactly which ones CORGI would have blocked, what proportion were true disputes, and what revenue would have been recovered.


**The benchmarked metrics:**


**Approval uplift / good revenue recovered:** transactions Radar would have blocked that CORGI scores as safe, weighted by realized revenue without dispute


**Dispute rate reduction:** disputes CORGI would have prevented among Radar-approved traffic


**Precision and recall** at the selected operating threshold


**Flag rate:** percentage of transaction volume the model would intervene on


**Net revenue impact:** dispute savings minus margin lost on false declines (using a 3× dispute cost multiplier)


**False decline rate:** estimated good transactions blocked


**How Long It Takes**


Using a two-proportion z-test (α=0.05, power=0.80), the required sample sizes per arm are approximately:


-


190,000 payments to detect a 0.1pp absolute change in fraud rate


-


75,000 payments for a 0.2pp change in false positive rate


-


215,000 payments for a 0.1pp change in authorization rate


For a platform processing 60,000 to 80,000 transactions per month, meaningful results on the primary dispute-rate metric typically emerge within 8 to 12 weeks. Higher-volume platforms reach significance faster.


### 4. Where CORGI Sits in the Payment Flow


**The Inference Flow**


CORGI sits between payment method collection and PaymentIntent confirmation. After Stripe tokenizes the card but before authorization is sent to the issuer. Radar still runs at its normal position (during confirmation). Here's the sequence:


1.


Customer enters payment details. Payment Element collects and creates a ConfirmationToken.


2.


Token is sent to the platform backend, then routed to CORGI's pre-auth endpoint.


3.


CORGI runs fraud scoring (LightGBM inference + rule engine, target <2s).


4.


If approved: PaymentIntent.create(confirm: true, confirmation_token: token_id). Radar evaluates here as normal.


5.


If blocked: PaymentIntent is not created. Decline is returned to the frontend.


**Latency**


The CORGI decision adds 100ms to 500ms depending on whether features are cached (Redis/Memorystore) or require a BigQuery lookup. This falls within the typical customer tolerance for a payment confirmation step. The decision runs server-side, so it does not affect page load or Payment Element rendering.


**Operational Risk**


Minimal. CORGI does not modify Stripe's authorization flow or interact with issuers. If CORGI's scoring service is unavailable, the fallback passes through to Stripe's normal confirm flow (Radar only). The integration requires one backend route change (redirect confirm calls through CORGI's endpoint) and a three-line frontend change (swap stripe.confirmPayment() for stripe.createConfirmationToken() + API call). No changes to Radar rules, webhook handlers, or settlement flows.


### 5. Where Custom Modeling Creates the Most Value (and Where It Doesn't)


**Biggest Impact Areas**


**False-decline reduction (approval uplift)** is typically the largest dollar lever. Our reference merchant recovered an estimated 5.6% of volume in good revenue that Radar was incorrectly blocking. For most merchants we analyze, false declines cost more than fraud losses. Radar's cross-merchant model over-blocks on merchants whose fraud profile diverges from the platform average.


**Fraud and dispute reduction** delivers meaningful but typically second-order impact. The same merchant saw a 59.5% dispute reduction in active model windows. Radar already does some work here, so the marginal lift is meaningful but usually not as large as the false-decline swing.


**Per-merchant custom modeling** unlocks per-entity rolling statistics that Radar cannot compute: z-score of transaction amount versus card's 180-day mean, all-time-max breach detection, card fingerprint dispute history, and spend velocity relative to behavioral baselines. Radar is a real-time rule engine, not a per-merchant feature store.


**Where We Typically Don't Add Value**


**Issuer authorization optimization:** We do not have direct issuer relationships and do not replace network tokenization, auth retries, or Auth Boost-style services.


**3DS routing optimization at the network level:** We decide whether to challenge with 3DS, but we don't optimize 3DS routing across networks.


**Merchants already running an effective custom fraud stack:** If a platform has built in-house ML on top of Radar, the marginal lift over their current stack will be smaller than the lift over Radar alone. We need to understand what's in place today to set expectations accurately.


### 6. How Shadow Mode Works on Stripe Connect


Shadow mode is a scoring-only deployment. CORGI receives webhook events for every transaction on the platform, scores them in real time, and logs the decision (approve/block + score + contributing features) without taking any action. Radar and existing rules remain fully in control.


**Technical Setup**


We subscribe to Stripe webhooks (charge., payment_intent., dispute., refund.) via the platform's Connect account. Feature computation runs against BigQuery on a rolling schedule. Each incoming transaction is scored against the current model, and the decision is stored with a full audit trail.


For live A/B testing after shadow mode, we use a deterministic hash-based allocation system (SHA-256 of the PaymentIntent ID mod a prime) to split traffic into treatment and control arms. This satisfies SUTVA (Stable Unit Treatment Value Assumption) at the payment level and allows clean causal measurement.


**Duration**


We typically run shadow mode for 8 to 12 weeks, depending on volume. The minimum is driven by the label maturity window: disputes typically surface 45 to 90 days after the transaction, so the first month of shadow data only becomes fully labeled two to three months later. For a high-volume platform, statistically significant results on the primary metrics (dispute rate, false decline rate) typically land within this timeframe.


### 7. What Data the Pilot Requires


**Required: Transactional Data**


-


Charges and PaymentIntents: amount, currency, status, outcome, card metadata (brand, country, funding, fingerprint), CVC/AVS check results, Radar risk score and risk level


-


Disputes: reason, status, amount, evidence submitted, resolution


-


Refunds: amount, reason, timing


-


Customers: ID, email, creation date (for velocity and behavioral features)


**Required: Merchant-Level Data**


-


Connected account IDs to scope feature computation and model training per merchant


-


Merchant category or vertical for baseline calibration


**Optional but Valuable**


-


Historical data export (six to 12 months): enables backtest before shadow mode begins


-


3DS authentication results: for challenge rate analysis


-


Behavioral/session data: we offer Corgi Beacon, a lightweight analytics tag that feeds browsing behavior into the model, but this is not required for the pilot


All data access during shadow mode is read-only.


### 8. Starting with Limited, Read-Only Access


Yes. For the pilot, we require:


-


Read-only API key scoped to Payments, Disputes, Customers, and Refunds


-


Webhook subscriptions for charge., payment_intent., dispute., refund.


-


Optional one-time historical export for backtest (recommended: six to 12 months)


We do not need write permissions or broad account access during shadow mode. Write access is only required if and when we move to live decisioning, and even then it is scoped exclusively to PaymentIntent confirm/cancel via the ConfirmationToken flow. No access to payouts, transfers, account settings, or connected account management.


### 9. Required Versus Optional Merchant Attributes


**Required**


Transaction-level fields: amount, currency, card brand, card country, card funding type, card fingerprint, customer ID, and customer email. Plus dispute/fraud/chargeback labels: which transactions were disputed, reason code, and resolution.


These are the minimum inputs for CORGI's core feature families (velocity aggregates, z-score anomaly detection, card fingerprint dispute history, spend spike detection).


**Optional but High-Value**


-


Billing address (postal code): enables geographic anomaly features


-


3DS authentication results: enables challenge rate optimization


-


Product/SKU metadata: enables product-level risk profiling


-


Session and behavioral data (via Corgi Beacon): browsing patterns, referral source, device fingerprint


-


Merchant-level metadata (vertical, average order value, typical customer geography): improves baseline calibration


The model handles missing optional fields natively. LightGBM routes NaN values at each split, so absent features degrade gracefully rather than breaking inference.


### 10. Data Isolation: Your Data Stays Yours


CORGI trains separate, isolated models per merchant (connected account). Your platform's transaction data is used exclusively to build your model. It does not enter any shared training set, and no other customer's data enters your model.


Data isolation is enforced at the infrastructure level. Each connected account's features are stored in dedicated BigQuery tables (corgi_features.{account_id}_*), and model training runs are scoped to a single account's data. There is no cross-account weight sharing, no federated learning, and no aggregate model that blends multiple customers' data.


The one exception (which is opt-in) is platform-level aggregate signals. For example, if a card fingerprint has been disputed across multiple merchants on the same Connect platform, that cross-merchant signal can surface as a read-only feature. This is analogous to what Stripe Radar already does across its network, but scoped to your platform. The model weights remain per-merchant. Only the feature value is shared.


**What This Means for Your Platform**


The pattern is consistent across every evaluation we've run. Radar does good work at the network level, but it leaves money on the table for individual merchants. A custom model trained on your specific transaction data catches what a cross-network model cannot: the behavioral patterns, velocity anomalies, and card-level signals unique to your platform.


For the reference merchant in this evaluation, that gap was worth $508K in net revenue impact and a 59.5% reduction in disputes. Your numbers will differ based on volume, fraud profile, and what you're running today. Shadow mode exists to measure exactly that, with zero risk to your live traffic.


Ready to see what CORGI finds in your data? Book a shadow mode pilot and we'll have your first scoring results within weeks.
