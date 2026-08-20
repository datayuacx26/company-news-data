---
schema_version: "1.0.0"
document_id: "a25359dde646afedd87d987858ad1c0b6dd3432c03e70f0a55e367aea7aa2401"
company_key: "yc-sift"
company: "Sift"
source_id: "yc-sift-rss-c48c6d88eecb"
canonical_url: "https://sift.com/blog/multi-accounting-fraud-in-marketplaces-and-igaming/"
published_at: null
first_seen_at: "2026-07-20T23:24:10.644571+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:73b972d6979a097663cf35964a538298ec5b048706b5f84b49ba6f3441f01e5f"
---

# Multi-accounting Fraud in Marketplaces And iGaming: What it is And How to Stop it

- [Fraud](https://sift.com/blog/category/fraud/)
- [Prevent Fraud](https://sift.com/blog/category/prevent-fraud/)


# Multi-accounting Fraud in Marketplaces And iGaming: What it is And How to Stop it


Multi-accounting fraud occurs when a single person or coordinated fraud ring operates several accounts on the same platform in an attempt to exploit…


[Ben Price](https://sift.com/blog/author/bprice/) Jul 10, 2026


Multi-accounting fraud occurs when a single person or coordinated fraud ring operates several accounts on the same platform in an attempt to exploit promotional offers, evade enforcement actions, or manipulate platform economics.


For marketplace operators and iGaming platforms, multi-accounting is one of the most persistent and costly fraud types in the[Fraud Economy](https://sift.com/blog/the-evolving-fraud-economy-how-ai-is-reshaping-digital-risks/) , both because it directly erodes the financial incentives that drive user acquisition and because it is designed specifically to fly below the radar of rules-based detection systems.


## **What multi-accounting fraud looks like by platform**


Multi-accounting fraud will look a bit different depending on the platform that it’s taking place on. Understanding what fraudsters are after is the starting point for understanding how they operate.


- **On iGaming platforms:** Multi-accounting is primarily driven by bonus abuse and rule evasion, and because online gambling platforms invest heavily in welcome bonuses, free bets, and promotional credits to acquire new players, it’s a perfect pick for this scheme. Fraudsters create new accounts to repeatedly claim these bonuses, with no intention of becoming a genuine player. In regulated markets, they also use multiple accounts to circumvent responsible gambling limits or self-exclusion decisions, bypassing player protection frameworks the operator is legally required to enforce.


- **On commerce marketplaces:** Multi-accounting enables banned sellers to re-enter the platform under new identities after enforcement action. It also allows coordinated networks of accounts to manipulate review systems, inflate seller ratings through fake purchases between connected accounts, or operate multiple storefronts that appear independent but function as a coordinated scheme. Buyer-side multi-accounting is used to exploit referral programs, new-user discounts, and limited-quantity promotional offers.


## **How fraudsters build and operate multiple accounts**


The challenge of multi-accounting detection is that fraudsters put a lot of effort into making their accounts appear independent. They use separate email addresses, distinct phone numbers, and different payment methods for each account. They also vary registration timing and source IP addresses to avoid triggering velocity-based rules. On[iGaming platforms](https://sift.com/blog/preventing-fraud-in-the-igaming-industry/) , they may operate accounts across different deposit and withdrawal cycles to avoid drawing suspicion through identical financial patterns.


The infrastructure behind large-scale multi-accounting operations includes device farms (like physical phones or tablets controlled remotely), mobile device emulators running multiple virtual instances, residential proxy networks that assign each account a different residential IP address, and identity fabrication tools that generate coherent but false profile data.


But despite all of this extra effort fraudsters invest in making accounts appear independent, they still leave relational traces. They reuse device hardware components across “different” devices. They share network infrastructure in ways that are invisible to per-account rules but detectable through graph analysis. They display behavioral patterns, including typing rhythms and navigation sequences, that are consistent across accounts even when surface attributes differ.


## **Detection signals for multi-accounting**


To effectively spot multi-accounting, you need to look at accounts as nodes within a larger network instead of trying to evaluate each one in isolation.


- **Device fingerprinting and hardware signals:** Even when fraudsters use different devices or emulators for different accounts, the same hardware-level signs (including GPU configuration, audio context, screen configuration, and battery behavior) will often appear across profile changes and device resets. Advanced device fingerprinting finds connections between accounts that share underlying hardware, even when their software attributes differ.


- **Graph-based link analysis:** Shared IP infrastructure, sequential registration timing from the same subnet, identical or near-identical behavioral patterns, and shared payment method metadata create edges between accounts that reveal coordinated networks. Graph analysis over these relationships turns individual account risk signals into a network-level fraud picture.


- **Behavioral biometrics at scale:** When fraud teams analyze behavioral patterns across accounts with suspected relationships, consistent typing cadences, identical navigation patterns, or unusually similar session timings across accounts registered under different identities are powerful indicators of the same person operating multiple accounts.


- **Velocity and timing patterns:** Multi-accounting fraud typically involves a burst of registrations followed by rapid bonus or promotion exploitation. Accounts registered within a short time window and exhibit similar first-session behavior while moving directly to bonus activation are exhibiting a common multi-accounting pattern.


## **How Sift supports multi-accounting fraud prevention**


Sift assesses thousands of signals throughout the entire user journey (across registration, login, and transaction) to produce a risk score which estimates the multi-accounting risk.


Sift’s network intelligence combines single-platform detection with cross-platform data, where a device associated with multi-accounting on one marketplace raises risk signals on iGaming platforms and other Sift customers. This gives fraud teams advance warning of known multi-accounting infrastructure.


Dynamic Friction is a perfect solution, as it allows iGaming and marketplace operators to apply verification requirements selectively to only high-risk registrations and sessions, without adding friction to the majority of legitimate users who do not exhibit multi-accounting signals. Workflows enable fraud teams to build and fine-tune detection rules without engineering involvement, responding quickly as fraudsters adapt their methods.


## **The real cost of multi-accounting fraud**


The most visible cost of[multi-accounting fraud](https://sift.com/blog/preventing-promo-bonus-and-multi-account-abuse-for-igaming/) is a company’s promotional budget. iGaming platforms that don’t properly control bonus abuse find themselves stuck with corrupted player acquisition economics, as promotional spend reaches fraudsters rather than real players. Marketplaces that fail to prevent seller re-registration after enforcement action undermine the trust infrastructure that makes the platform worth using for legitimate buyers.


Other less visible costs compound more slowly over time. This includes review queue burden from investigating multi-account complaints, regulatory exposure for iGaming operators that fail to enforce responsible gambling restrictions, and the erosion of platform data quality when fake account activity distorts the signals used to drive product and fraud decisions.


Multi-accounting fraud rewards the platforms that take network-level detection seriously and penalizes those that evaluate each account in isolation.


If your business is experiencing multi-accounting fraud and struggling to control it, then Sift might be a great option for you.[Sift](https://sift.com/solutions/digital-commerce/) uses machine learning technology to identify and respond to emerging threats like multi-accounting fraud before it causes damage to your business.


**How is multi-accounting different from account creation fraud?**


[Account creation fraud](https://sift.com/solutions/account-creation/) refers broadly to any fraudulent registration of a fake account. Multi-accounting is a specific case within that category where the same fraudster or fraud ring creates and operates several accounts on the same platform simultaneously, usually to exploit specific platform economics like sign-up bonuses, referral programs, or seller reputation systems. All multi-accounting involves account creation fraud, but not all account creation fraud involves multi-accounting.


**Can iGaming operators use IP blocking to stop multi-accounting?**


IP blocking catches unsophisticated multi-accounting but is easily bypassed. Fraudsters use residential proxy networks that assign different residential IP addresses to each session, which makes it pretty much impossible to link accounts by IP address alone. IP blocking also carries false-positive risk: households and workplaces where multiple users share an IP address would be affected. While it can be helpful, truly effective multi-accounting detection uses IP data as just one signal of many, rather than as a primary control.


**What is bonus abuse and how is it related to multi-accounting?**


Bonus abuse is the practice of exploiting promotional offers in ways that were not intended by the platform, most commonly by repeatedly claiming welcome bonuses using multiple accounts. Multi-accounting is the mechanism that allows for systematic bonus abuse on a large scale. A fraudster who registers 50 accounts on an iGaming platform to claim the welcome bonus 50 times is committing both account creation fraud and bonus abuse. The two terms are related but distinct:[bonus abuse](https://sift.com/blog/preventing-promo-bonus-and-multi-account-abuse-for-igaming/) describes the goal, while multi-accounting describes the method.


Share post on:


-
-
-
-
