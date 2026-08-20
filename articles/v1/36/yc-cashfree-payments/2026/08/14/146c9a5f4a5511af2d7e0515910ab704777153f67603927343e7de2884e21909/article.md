---
schema_version: "1.0.0"
document_id: "146c9a5f4a5511af2d7e0515910ab704777153f67603927343e7de2884e21909"
company_key: "yc-cashfree-payments"
company: "Cashfree Payments"
source_id: "yc-cashfree-payments-rss-98daff448d11"
canonical_url: "https://blogrevamp.cashfree.com/what-is-carding/"
published_at: "2026-08-04T17:30:00+00:00"
first_seen_at: "2026-08-04T18:18:37.984076+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:84099ca2e73367bcd2b47acb517ce254eb46f966afe37ca42cf858f4a696d3c5"
---

# What Is Carding? How It Works and How Merchants Can Prevent It

Table of Contents


Toggle


### TL;DR


**Carding** is a payment fraud technique where attackers use stolen card details to test whether cards are active through merchant checkout pages. Businesses can reduce the risk by implementing **3D Secure authentication** , **velocity checks** , **device fingerprinting** , and **AI-powered fraud detection** to identify and block suspicious payment attempts before they lead to fraud or chargebacks.


Carding is an institutionalised testing process that, without you knowing, turns your checkout page into a validation engine for stolen card data. This means every business that processes online payments needs to understand what carding is, how it works, and how it’s a big risk for your business and customers.


##


What Is Carding and How Does It Work?


Carding is the automated process of validating stolen credit or debit card details by running them through a merchant’s payment gateway in rapid succession. In simple terms, fraudsters use online checkout pages to identify which stolen cards are still active before using or selling them for larger fraudulent transactions.


Every business that[accepts online payments](https://www.cashfree.com/payment-gateway-india/) should understand how carding works because it can significantly impact[payment success rates](https://www.cashfree.com/blog/what-is-payment-success-rate/) , increase chargebacks, and expose merchants to financial and reputational risks.


The process begins with obtaining stolen payment information.


Fraudsters acquire batches of stolen[Primary Account Numbers (PANs)](https://www.cashfree.com/PAN-verification/) , usually sourced from data breaches, phishing attacks, skimming devices, malware, or dark web marketplaces. Their goal is to identify cards that are still active, have available credit or funds, and can successfully pass authorisation. In effect, a merchant’s checkout page unknowingly becomes a validation tool for stolen payment credentials.


The core mechanism involves submitting large volumes of low-value or zero-value transactions through a[merchant’s payment page](https://www.cashfree.com/payment-pages/) . Automated bots rapidly enter card numbers, expiry dates, and CVVs at a scale no human operator could match.


It’s important to understand that fraudsters are not trying to purchase products at this stage. Their objective is simply to validate stolen card details. Once a card successfully authorises, it becomes significantly more valuable and can later be used for fraudulent purchases or sold on underground marketplaces.


For merchants, one of the earliest warning signs is a sudden spike in failed authorisations accompanied by a surge in low-value transactions from unfamiliar devices or locations. In many cases, this indicates an active carding attack rather than genuine customer activity.


##


The Carding Attack Lifecycle


Modern carding is no longer a manual process. Today’s attacks are typically carried out by organised cybercriminal groups using automated tools, bot networks, and specialised infrastructure designed to maximise the value of stolen payment data.


The lifecycle generally consists of four stages, with merchants becoming the primary target during the validation phase.


### 1. Acquisition


Attackers first obtain stolen card data through large-scale data breaches, phishing campaigns, malware, skimming devices, or compromised eCommerce websites. These stolen card details are often bought and sold on underground forums using anonymous networks and cryptocurrency.


In some cases, attackers also inject malicious scripts into vulnerable websites to capture payment information directly during checkout or trick users into entering their card details through fake login pages, shipping alerts, or security update scams.


### 2. Validation


Once attackers obtain stolen card details, automated bots begin testing them against real merchant checkout pages.


These bots typically submit thousands of low-value transactions, often between ₹1 and ₹5-to determine whether a card is still active. If a transaction receives an authorisation response, the card is considered “live” and moves to the next stage of the attack.


### 3. Sorting and Resale


Validated cards become significantly more valuable.


Attackers organise them based on factors such as the issuing bank, BIN (Bank Identification Number), card network, geographic region, and estimated spending limit. These verified cards are either sold to other cybercriminals at a premium or retained for future fraudulent activity.


### 4. Monetisation


In the final stage, validated cards are used for high-value purchases, account takeovers, subscription fraud, or gift card purchases. These[fraudulent transactions](https://www.cashfree.com/blog/fraudulent-transactions-how-to-identify-and-prevent/) often result in[chargebacks](https://www.cashfree.com/blog/chargebacks-meaning-how-they-works-types-causes-prevention/) , financial losses, and increased scrutiny for the merchants whose checkout pages were used during the validation process.


To avoid detection throughout the attack lifecycle, fraudsters continuously rotate IP addresses, device fingerprints, browsers, and proxy networks, making automated traffic appear similar to legitimate customer behaviour.


##


What Merchants Must Understand About Carding


Carding attacks are designed to imitate legitimate customer behaviour while testing stolen card details at scale. Rather than targeting a single transaction, attackers rely on automation, distributed infrastructure, and sophisticated techniques to validate thousands of payment credentials within a short period.


Understanding how these attacks operate helps merchants identify suspicious activity before it impacts payment success rates, increases chargebacks, or affects relationships with acquiring banks.


Three common attack patterns are used in most carding campaigns.


### 1. Botnets and Automated Checkout


Attackers use automated bot frameworks to programmatically fill payment forms, submit card details, and rotate through thousands of stolen cards far faster than any human could.


To avoid detection, these bots increasingly mimic legitimate user behaviour by simulating mouse movements, typing patterns, and realistic browsing sessions, allowing them to bypass basic behavioural checks.


### 2. BIN Attacks


Rather than generating completely random card numbers, attackers exploit predictable Bank Identification Numbers (BINs), the first six to eight digits of a payment card that identify the issuing bank and card programme.


Using known BIN ranges, automated tools generate sequential or algorithmically valid card numbers before testing them against merchant checkout pages. Many of these bots use the Luhn algorithm to eliminate mathematically invalid card numbers before submission, increasing the efficiency of their attacks.


### 3. Credential Stuffing and Proxy Rotation


To bypass velocity-based fraud controls, attackers continuously rotate IP addresses through residential proxy networks while changing device fingerprints, browser signatures, session cookies, and user-agent strings.


As a result, thousands of fraudulent payment attempts can appear to originate from different users, devices, and geographic locations, making them significantly harder to detect using traditional security measures.


Instead of focusing solely on how attackers execute carding, merchants should monitor internal payment metrics that typically change during an attack.


**Some of the most common warning signs include:**


- A sudden drop in overall authorisation rates.
- A sharp increase in declined transactions, particularly across specific BIN ranges.
- Higher infrastructure usage caused by thousands of payment attempts that were never intended to result in genuine purchases.


***Also read:[Payment Fraud: Types, Detection & Prevention Guide for Businesses](https://www.cashfree.com/blog/payment-fraud-types-detection-prevention-guide/)***


##


Common Types of Carding Attacks


Carding attacks vary depending on the attacker’s objective. Some are designed to validate stolen payment cards, while others target customer accounts, stored-value instruments, or additional authentication factors.


Understanding these attack types helps merchants recognise suspicious payment behaviour earlier and strengthen their fraud prevention strategy.


1. **Card Testing:** Also called card cracking, this attack is about testing the cards with a small, often ₹10 authorisation attempt run across a payment page purely to confirm whether a card is live, without intent to complete a real purchase.


1. **BIN Attacks** : Sequential or algorithmic generation of card numbers within a known BIN range, exploiting predictable issuer numbering patterns.


1. **Credential Stuffing:** Reused username/password combinations from unrelated breaches are tested against e-commerce accounts with saved cards, converting an account takeover into a payment fraud event.


1. **Gift Card Cracking:** Bots attempt to differentiate gift card or wallet account numbers to identify ones carrying a live balance, then drain or resell them at a premium price. This type of attack is functionally identical to BIN attacks but targets closed-loop instruments instead of open card networks.


1. **CVV and One-Time-Password (OTP) Farming** : Attackers pair validated card numbers with social-engineering campaigns or malicious apps to harvest the CVV or OTP needed to clear additional authentication layers.


##


How Carding Impacts Merchants and Payment Gateways


Carding attacks don’t just result in fraudulent transactions, they create a ripple effect that impacts authorisation rates, increases chargebacks, raises operational costs, and can even damage a merchant’s relationship with payment processors and acquiring banks.


The three biggest areas of impact are authorisation performance, chargeback exposure, and processor-level risk.


- **Authorisation Rate Degradation** : An increase in the number of failed or suspicious authorisations pulls down your overall approval rate. The issuing banks and card networks track these authorisation rates for every merchant they onboard.


Consequently, a depressed authorisation rate makes other networks and banks treat all your traffic, including legitimate customers, with more scrutiny, creating friction that suppresses conversion for real buyers.


- **Chargeback Ratio:** When validated cards are later used for fraudulent purchases elsewhere, or when a carding attempt slips through and completes a transaction, the cardholder disputes it.


Chargebacks that exceed network thresholds, and this rate is commonly set around 0.9%–1% of transaction volume, can trigger monitoring programs like Visa’s VDMP or Mastercard’s Excessive Chargeback Program, carrying fines and increased processing costs.


- **Processor and Acquiring Bank Risk Flags** :[Payment aggregators](https://www.cashfree.com/blog/payment-aggregators/) and acquiring banks monitor merchant accounts for abnormal decline patterns. Repeated carding activity on a merchant ID can lead to reserve requirements, rate hikes, or in severe cases, account termination.


- **Infrastructure and Operational Cost** : Bot-driven card testing generates transaction volume that consumes[gateway API](https://www.cashfree.com/blog/payment-gateway-apis/) capacity, server resources, and fraud-review bandwidth, all without producing revenue. As these processes cost every merchant, the more attack attempts happen, the worse it gets for the merchants.


##


How to Detect Carding Attacks


Detecting carding requires monitoring transaction patterns rather than relying on a single fraud signal. Since attackers constantly adapt their techniques, merchants should analyse payment velocity, authorisation outcomes, customer behaviour, and session activity together.


Some of the most common indicators of a carding attack include:


- **An unusually high number of payment attempts** compared to completed purchases, particularly for low-value or round-number transactions such as ₹1, ₹5, ₹10, or ₹100.
- **A sudden spike in failed authorisations** occurring within a short period, often involving similar transaction amounts.
- **Multiple card numbers submitted** from the same IP address, browser session, or device fingerprint in rapid succession.
- **Sequential or closely related card numbers** from the same BIN range repeatedly hitting the payment page.
- **Checkout sessions with little or no browsing activity** , where users land directly on the payment page without viewing products or adding items to the cart.
- **Billing or shipping locations that don’t align with the issuing country or region of the payment card** , especially when combined with other suspicious signals.
- **Repeated cart abandonment immediately after failed authorisations** , followed by another payment attempt within seconds using different card details.
- **Clusters of declined transactions** caused by invalid CVVs, insufficient funds, or expired cards occurring within a narrow time window rather than normal shopping behaviour.


While any one of these signals may not indicate fraud on its own, multiple indicators appearing together often suggest an active carding attack that requires immediate investigation.


##


How Payment Gateways Help Prevent Carding


Preventing carding requires a layered security approach that combines authentication, transaction monitoring, behavioural analysis, and intelligent fraud detection. Since no single security measure can stop every attack, merchants should rely on multiple fraud prevention controls working together.


Below are some of the most effective security measures used by[modern payment gateways to detect and block carding attacks](https://www.cashfree.com/risk-shield-payment-gateway/) .


- **3D Secure (3DS) Authentication:** Adding a bank-side authentication step (OTP, biometric, or app-based approval) forces attackers to clear a barrier that automated scripts generally cannot solve at scale, and this shifts the liability toward the issuer in most disputed cases.


- **Browser Validation:** Malicious bots also pretend to run a specific browser, and these operate through user agents to avoid being detected. So you need to have browser validation that involves checking every browser to ensure they are what’s really claimed. In other words, check if the JavaScript agent is making calls in an expected way and operating in a way expected from human users.


- **CVV Verification Enforcement** : Requiring CVV on every transaction narrows the pool of usable stolen data, since CVVs are less commonly exposed in bulk breaches than other card information and expiry details.


- **API Security:** Several eCommerce websites use credit card APIs to facilitate transactions, and these APIs are a part of the package offered by payment gateways. Since these APIs are vulnerable to attack through JavaScript injection or rerouting of data, every eCommerce website needs to have a combination of TLS and strong authentication protocols, offered by OAuth and OpenID.


- **Address Verification Service (AVS** ): Cross-checking the billing address against the issuing bank’s records on file adds friction that random or breach-sourced data frequently fails, breaking the set cycle of attempts bots make to gain access to the money.


- **Velocity Checks and Rate Limiting** : Capping the number of transaction attempts permitted from a single IP, device, card, or session within a defined time window directly interrupts the rapid-fire testing pattern carding depends on. If the attack stops, the bots are programmed to move on to the next.


- **CAPTCHA Systems** : Deploying friction proportionally, like starting with invisible checks and escalating to CAPTCHA only for suspicious sessions, is meant to block bot traffic while preserving conversion for genuine shoppers who can easily answer questions and solve CAPTCHA.


- **Device Fingerprinting and Behavioural Biometrics** : Tracking device, browser, and interaction patterns are tracked and monitored. So things like typing speed, mouse movement, and session history are used to determine shopping behaviour and compare it against the current pattern to distinguish scripted bot behaviour from human checkout flow, even when IPs and card numbers are rotated.


- **Real-time Risk Scoring** : Payment gateways that score each transaction against velocity, geolocation, BIN risk, and historical fraud signals before authorisation can auto-decline or route high-risk attempts to manual review before they reach the acquiring bank.


- **Low-value Transaction Monitoring** : Since card testing frequently uses minimal amounts to avoid detection, flagging repeated sub-threshold transactions from the same source is a high-signal, low-cost control.


For merchants operating through a payment aggregator or gateway like[Cashfree Payments](https://www.cashfree.com/) , most of these controls- 3DS, AVS, velocity limits, and risk scoring- are available as configurable, gateway-managed layers rather than something built in-house, since gateway-level fraud engines see attack patterns across their entire merchant base, not just one storefront.


##


How Cashfree Payments Helps Prevent Carding


Cashfree Payments’ fraud prevention infrastructure, **[RiskShield](https://www.cashfree.com/risk-shield-payment-gateway/)** , is designed to identify and block suspicious payment activity before it reaches the issuing bank.


Every transaction processed through Cashfree Payments is evaluated in real time using multiple fraud signals, including transaction velocity, device fingerprinting, BIN reputation, geolocation, behavioural patterns, and historical fraud intelligence gathered across its merchant network.


This cross-merchant visibility enables RiskShield to recognise attack patterns that may first appear on one business and proactively help protect others before the attack spreads.


Businesses using Cashfree Payments also have access to configurable fraud prevention controls without requiring extensive in-house development, including:


- Enforced CVV verification
- Native 3D Secure authentication with OTP or biometric verification
- Address verification checks
- Velocity limits for repeated low-value transactions
- IP and device-based risk controls


Rather than automatically approving or rejecting every suspicious transaction, RiskShield can intelligently challenge high-risk sessions or route them for additional verification. This helps businesses reduce fraud while maintaining healthy authorisation rates and minimising unnecessary declines for genuine customers.


***Learn from the best:[How growing businesses tackle disputes and chargebacks](https://www.cashfree.com/blog/how-growing-businesses-tackle-disputes-and-chargebacks/)***


##


Conclusion


Carding transforms a merchant’s checkout page into an unintended validation tool for stolen payment credentials. If left undetected, it can lead to declining authorisation rates, higher chargeback ratios, increased operational costs, and greater scrutiny from acquiring banks and payment processors.


The most effective defence is a proactive, layered fraud prevention strategy that combines authentication, behavioural analysis, transaction monitoring, and real-time risk scoring. Detecting suspicious patterns early helps businesses protect revenue, maintain customer trust, and deliver a secure payment experience.


### Protect Your Business from Payment Fraud


Detect suspicious payment activity early with AI-powered fraud prevention, intelligent risk scoring, and secure payment infrastructure from Cashfree Payments.


[Learn More](https://merchant.cashfree.com/merchants/signup?source-action=Blog&action=Sign%20Up&button-id=StartNow_BlogFooterCTA)


##


Frequently Asked Questions


#### Is carding the same as credit card fraud?


Carding is a type of payment fraud, but it specifically focuses on validating stolen credit or debit card details before they are used for fraudulent purchases. In most cases, attackers test cards through small automated transactions to determine whether they are active and can successfully complete payments.


#### How can merchants detect a carding attack?


Merchants should monitor unusual payment patterns such as repeated low-value transactions, spikes in failed authorisations, multiple payment attempts from the same IP address or device, sequential BIN activity, and abnormal checkout behaviour. Using AI-powered fraud detection and real-time monitoring can help identify these attacks early.


#### Can small businesses become targets of carding attacks?


Yes. Businesses of all sizes can become targets because attackers often look for checkout pages with weaker fraud prevention controls. Even small merchants can experience increased chargebacks, reduced authorisation rates, and higher operational costs if carding attacks go undetected.


#### Does 3D Secure (3DS) stop carding attacks?


3D Secure significantly reduces the risk of fraudulent transactions by adding an additional authentication step, such as an OTP or biometric verification. While it doesn’t eliminate every type of attack, it is one of the most effective controls when combined with velocity checks, device fingerprinting, and real-time risk scoring.


#### What role do payment gateways play in preventing carding?


Modern payment gateways use multiple security layers, including AI-powered fraud detection, device fingerprinting, behavioural analytics, velocity checks, and risk scoring to detect suspicious activity and block fraudulent payment attempts before they impact merchants.


---


**In case you missed it:**


- [Fraudulent Transactions: How to Identify and Prevent Them](https://www.cashfree.com/blog/fraudulent-transactions-how-to-identify-and-prevent/)
- [Payment Fraud: Types, Detection & Prevention Guide for Businesses](https://www.cashfree.com/blog/payment-fraud-types-detection-prevention-guide/)
- [Fake Payment Screenshot Scams: How to Identify & Prevent UPI Fraud](https://www.cashfree.com/blog/fake-payment-screenshot-scams/)
- [Fake UPI Payment App Scams: Spot Fake PhonePe APKs & GPay Apps](https://www.cashfree.com/blog/spot-fake-upi-payment-app-scams/)
