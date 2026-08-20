---
schema_version: "1.0.0"
document_id: "61db79a92b522de927c759efa9a10470c01303cdd5b8e9df69ae387758e9aef3"
company_key: "yc-dojah-inc"
company: "Dojah Inc"
source_id: "yc-dojah-inc-news-import-dac996b1a8e0"
canonical_url: "https://dojah.io/blog/fraud-as-a-service-africa"
published_at: "2026-07-30T10:10:45.646+00:00"
first_seen_at: "2026-07-30T11:08:12.737836+00:00"
fetched_at: "2026-07-30T11:08:14.123654+00:00"
content_hash: "sha256:0d10a10e16424a63827030c52b4e97403b59e77416151ec5fea0ea0b0bb66758"
---

# Fraud as a Service: What It Is and How to Defend Your Platform Against It

A fraudster does not need to build an entire phishing operation from scratch. They can subscribe to a platform, choose a target such as a fintech or crypto exchange, and access a ready-made phishing kit configured for that platform. When fraud teams block one version, the kit can be updated and deployed again in a new form.


This is Fraud as a Service (FaaS), a model that makes organised attacks easier to scale against digital platforms, including African fintechs. A


[Microsoft investigation into a phishing-as-a-service platform](https://blogs.microsoft.com/on-the-issues/2025/09/16/microsoft-seizes-338-websites-to-disrupt-rapidly-growing-raccoono365-phishing-service/) found that one subscription service operated across 94 countries and compromised at least 5,000 accounts. The case shows how a single fraud tool can support multiple actors executing the same attack approach.


This article explains what Fraud as a Service is, how FaaS platforms operate, how they target African fintechs, and what it takes to defend against these attacks.


### **What Fraud as a Service Actually Is**


Fraud as a Service (FaaS) makes sophisticated fraud available as a subscription. Instead of building phishing kits, fake platforms, or identity bypass tools themselves, fraudsters can pay for ready-made services that are maintained, updated, and designed to make organised attacks easier to run.


Here's what makes FaaS different:


- **It operates like a business, not just a tool:** FaaS providers run their operations much like legitimate software companies. They offer subscription plans, release regular updates, and run communities where subscribers share techniques and improve attack methods. Instead of building their own tools, fraudsters simply subscribe and launch attacks.


- **The barrier to entry is much lower:** Running a coordinated fraud campaign no longer requires advanced technical skills. Pre-configured tools handle much of the technical work, making it easier for more actors to target multiple African fintechs using the same attack infrastructure.


- **The tools evolve faster than detection rules:** When your fraud team updates its detection rules to stop one attack pattern, FaaS providers can quickly release a new version of the toolkit to every subscriber. While defenders respond to the last attack, fraudsters are already using the next one.


As Farouk Juniad, Head of Fraud at Kuda, notes in the **Dojah Fraud Insights Report 2025** , *"Fraud is now its own industry, capitalized like a Tier-1 bank. And it's evolving faster than most institutions can respond."*


Knowing how FaaS works is only the first step. To defend against it, you also need to understand the tools these platforms put in the hands of fraudsters.


### **What a FaaS Platform Typically Includes**


A FaaS platform is not just a single fraud tool. It combines different attack capabilities that help fraudsters move from identity creation to account access and fund movement.


Here's what a typical FaaS platform includes:


- **Fake platform interfaces:** Fraudsters create convincing copies of login, payment, and withdrawal pages used by fintechs, banks, and mobile money platforms. These fake interfaces are designed to capture credentials and OTPs before users realise they are interacting with a fraudulent platform.


- **Phishing kits built for specific platforms:** FaaS providers offer ready-made phishing kits configured for specific fintechs, banks, and exchanges. This means your platform may face multiple attackers using the same toolkit and the same attack method rather than separate, unrelated schemes.


- **Rapid kit updates:** When your fraud team blocks one attack pattern, FaaS providers can quickly update the toolkit and distribute new versions to subscribers. This constant update cycle allows attackers to change tactics while detection rules are still catching up.


- **Identity bypass tools:** FaaS platforms can include fake KYC document generators, liveness bypass tools, and AI-generated identity materials designed to pass onboarding checks. In February 2026, threat actors advertised


[West African identity datasets on underground forums](https://techpoint.africa/guide/nigeria-bank-fraud-ai-deepfake-identity/) , highlighting an active supply chain supporting identity fraud and account creation at scale.


- **Mule recruitment networks:** FaaS operations often rely on networks of recruited individuals who open accounts, receive funds, and move money before platforms can detect and stop the activity. These networks help fraudsters move funds quickly across accounts and platforms.


The advantage of FaaS does not come from one tool alone. It comes from combining multiple capabilities into coordinated attacks that individual platform controls were not designed to handle.


### **How FaaS Is Being Used to Target African Fintechs**


African fintechs are a growing target for FaaS operators because high onboarding volumes and expanding digital services create more opportunities for organised fraud. At the same time, fraud groups now have access to tools, identity sources, and distribution networks that make large-scale attacks easier to execute.


These attacks typically follow these patterns:


- **Coordinated onboarding attacks across platforms:** FaaS tools allow fraud groups to automate account creation across multiple fintech platforms using fake or farmed identities. Your platform may detect suspicious applications individually, while the wider campaign remains hidden across other platforms.


- **Identity and credential theft:** Fake platform interfaces and phishing kits do more than steal funds from existing accounts. They collect credentials and identity data that can be reused to create fraudulent accounts or support additional attacks.


- **Fund movement through mule networks:** After fraudulent accounts are created and funded, mule networks move money quickly across accounts and platforms. This allows fraud groups to move funds before your detection systems can identify the full pattern.


The challenge is that FaaS attacks are designed to look like separate events inside each platform. The signals that reveal the wider operation often appear only when identity, device, behavioural, and transaction activity are viewed together.


### **Why Rule-Based Detection Struggles Against FaaS**


The challenge with defending against FaaS using rule-based detection is structural. Rule-based systems identify patterns they have been configured to detect, while FaaS tools are designed to change those patterns and avoid previously identified signals.


This creates three detection challenges for fraud teams:


#### **1. The tools evolve faster than detection rules**


When your team updates a rule to detect a known attack pattern, FaaS operators can modify their tools and distribute new versions quickly. This creates a cycle where fraud teams are often responding to the latest attack instead of preventing the next variation.


#### **2. Each FaaS tool targets a specific control gap**


FaaS attacks combine specialised tools built to bypass individual fraud controls. Liveness bypass tools target biometric checks, fake document generators target identity verification, and mule networks exploit transaction monitoring gaps. A platform relying on one layer of defence remains exposed through other parts of the fraud chain.


#### **3. The wider attack pattern is hidden within individual platforms**


A coordinated FaaS campaign can appear as separate events when viewed from one platform alone. Individual accounts, devices, or transactions may not look unusual, but connected signals across multiple activities can reveal the larger operation.


Defending against FaaS requires a different detection strategy, not just stronger individual checks.


### **What Defending Against FaaS Actually Requires**


Fighting FaaS attacks requires more than stronger individual checks. The platforms that withstand these attacks connect identity, device, behavioural, and transaction signals so that a weakness in one layer can be detected through another.


A layered detection approach needs four key capabilities:


#### **1. Identity signals beyond document and liveness checks**


Document verification and liveness checks are important, but they do not show the full risk picture. Device signals, environment data, and behavioural indicators during onboarding add more context when an identity appears suspicious.


#### **2. Behavioural monitoring after onboarding**


FaaS-created accounts can pass onboarding and only reveal risk through later activity. Monitoring account behaviour after signup helps identify unusual activation patterns, transaction behaviour, and activity that does not match the verified customer profile.


#### **3. Transaction monitoring for coordinated patterns**


Fraud networks often distribute activity across multiple accounts to avoid individual thresholds. Looking at transaction relationships, shared signals, timing patterns, and fund movement helps reveal coordination that a single-account view can miss.


#### **4. Connected signals across the fraud lifecycle**


A single unusual signal may not be enough to trigger action. However, when a new device, unusual behaviour, and transaction changes appear together, the combined risk becomes clearer. Connected detection allows each layer to strengthen the others.


This is the detection layer


[Dojah’s Profiled Risk](https://profiledrisk.com/) is built to provide.


### **How Dojah's Profiled Risk Helps You Defend Against FaaS Attacks**


FaaS attacks succeed when fraud checks work in isolation. Profiled Risk takes a connected approach by bringing together identity, behavioural, device, and transaction signals, so risk is assessed across the customer lifecycle instead of at a single point in time.


Profiled Risk supports layered fraud detection in four key ways:


- **Connected identity verification signals:** Document, biometric, and liveness checks are assessed alongside device and environment signals, giving you a broader view of risk during verification. A session that passes one check can still be flagged when connected signals indicate unusual activity.


- **Continuous behavioural monitoring:** Profiled Risk continues assessing account activity after onboarding, helping you detect behavioural drift, unusual account activity, and changes that increase risk as customer behaviour evolves.


- **Transaction risk in context:** Transaction activity is evaluated alongside identity, device, and behavioural signals rather than in isolation. This helps your team investigate suspicious activity using a more complete view of customer risk.


- **Real-time risk profiles:** Risk scores are not fixed after onboarding. Profiled Risk continuously updates each customer's risk profile as new signals emerge, helping your team respond to changing risk before fraud escalates.


Profiled Risk brings identity, behavioural, and transaction intelligence together, helping fraud teams detect the connected attack patterns that FaaS is designed to exploit.


[Explore Dojah's Profiled Risk to see how it helps you detect and respond to organised fraud across the customer lifecycle.](https://profiledrisk.com/)


### **Frequently Asked Questions on Fraud as a Service**


**1. What is Fraud as a Service (FaaS) in African fintech?**


Fraud as a Service (FaaS) is a model where fraud tools are sold as ready-made services. Instead of building attack infrastructure themselves, fraudsters can subscribe to phishing kits, identity bypass tools, and other services. This has made Fraud as a Service in African fintech more accessible and easier to scale.


**2. Why is Fraud as a Service a growing threat?**


FaaS allows multiple fraud actors to use the same tools at the same time. This makes organised fraud in African fintech faster to launch and harder to detect with traditional fraud controls.


**3. Why can't rule-based detection stop FaaS attacks?**


Rule-based systems detect known patterns, while FaaS providers continually update their tools to avoid those rules. Effective FaaS fraud prevention in Africa requires connected identity, behavioural, device, and transaction signals.


**4. What does effective FaaS fraud prevention look like?**


It combines identity verification, behavioural monitoring, transaction monitoring, and continuous risk assessment. Connecting these signals helps uncover coordinated attacks that isolated checks can miss.


**5. How can fintechs defend against organised fraud?**


Build layered detection that continues after onboarding. Monitoring identity, behaviour, devices, and transactions together helps your team detect coordinated fraud earlier and respond before losses escalate.
