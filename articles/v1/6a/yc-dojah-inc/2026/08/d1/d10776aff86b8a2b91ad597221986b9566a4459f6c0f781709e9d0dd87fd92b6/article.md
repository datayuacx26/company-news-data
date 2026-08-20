---
schema_version: "1.0.0"
document_id: "d10776aff86b8a2b91ad597221986b9566a4459f6c0f781709e9d0dd87fd92b6"
company_key: "yc-dojah-inc"
company: "Dojah Inc"
source_id: "yc-dojah-inc-news-import-dac996b1a8e0"
canonical_url: "https://dojah.io/blog/rverification-continuous-monitoring-fintechs-africa"
published_at: "2026-08-06T10:28:00.630+00:00"
first_seen_at: "2026-08-06T16:00:26.974995+00:00"
fetched_at: "2026-08-06T16:00:27.942176+00:00"
content_hash: "sha256:99b10b891c12873d771a744b4b5696634eacae26c97ff95e4d8bbeb36ce30623"
---

# Re-Verification vs Continuous Monitoring: When African Fintechs Need Both

Re-verification asks a customer to confirm their identity again at a specific point in time. Continuous monitoring does not interrupt the customer. It runs in the background, tracking behavioural and transaction signals after onboarding. Both protect your platform, but they catch different risks and neither replaces the other.


Many teams still treat these controls as interchangeable or rely too heavily on one of them. That creates gaps that either satisfy a compliance requirement while missing emerging fraud, or detect suspicious activity without confirming whether the customer's identity should be verified again.


This guide explains re-verification and continuous monitoring for fintechs in Africa, when each control should be used, and why they work best together.


### **What Re-Verification Is and When It Applies**


Re-verification asks a customer who has already completed onboarding to confirm their identity again. It does not run continuously. Instead, it happens at a scheduled interval or after a specific event, and the customer must actively complete another identity check.


Common re-verification triggers include:


- **Regulatory re-KYC:** Re-verification supports periodic customer due diligence after onboarding. Under the CBN's risk-based approach, institutions should refresh customer due diligence based on the customer's risk level, with high-risk customers reviewed more frequently than lower-risk customers.


- **High-risk events:** Some events require re-verification immediately instead of waiting for the next review cycle. A change in a customer's risk profile, a sanctions list match, significant adverse media, or a change in beneficial ownership should all trigger a fresh identity check.


- **Account changes:** Some account updates should not be approved without confirming who made the request. If a customer wants to change their phone number, linked account, or address, re-verification helps you confirm the request before the update is completed.


- **Dormant accounts:** A dormant account that suddenly becomes active deserves another identity check. Re-verification helps confirm that the person returning to the account is the same person who originally completed onboarding.


Re-verification confirms your customer's identity at a specific point in time. What happens between those checks is where continuous monitoring becomes essential.


### **What Continuous Monitoring Is and What It Catches**


Continuous monitoring runs without asking your customer to complete another identity check. It works in the background, comparing behavioural and transaction signals against each account's established baseline and flagging unusual activity as it emerges.


It is most effective at detecting these signals:


- **Post-onboarding behaviour:** Your customer's transaction behaviour can change after onboarding even when their identity has not. New counterparties, unusual transaction velocity, or amounts that no longer match their stated profile can indicate emerging risk. Re-verification will not detect those changes because the behaviour, not the identity, has changed.


- **Login and device anomalies:** A login from an unfamiliar device or location, especially before a high-value transaction, deserves immediate attention. These signals appear in real time without waiting for a scheduled review or a request for another identity check.


- **SIM swap signals:** A SIM swap can appear as a sudden change in the device receiving OTPs, followed by account access and rapid fund movement. These connected events can expose an account takeover attempt even though no re-verification event has been triggered.


- **Coordinated account patterns:** Shared devices, similar transaction timing, and linked counterparties across multiple accounts can reveal coordinated fraud. Looking at these signals together exposes patterns that a single identity check cannot.


What continuous monitoring catches, re-verification misses entirely. What re-verification catches, continuous monitoring cannot trigger on its own.


#### **Re-Verification vs Continuous Monitoring: At a Glance**


**Re-Verification**


**Continuous Monitoring**


**When it runs**


Triggered or scheduled moments


Always, in the background


**User interaction required**


Yes, customer completes a check


No, runs without customer involvement


**What triggers it**


Regulatory reviews, risk events, account changes, dormancy


Behaviour changes, transaction patterns, device signals


**What it catches**


Identity changes, document updates, ownership changes


Account compromise, fraud patterns, SIM swaps, coordinated activity


**What it misses**


Fraud developing between review points


Identity changes requiring a new document or biometric check


**Regulatory function**


Periodic re-KYC obligations


Ongoing monitoring obligations


### **Why African Fintechs Need Both Running Together**


Re-verification and continuous monitoring are not alternatives. They solve different post-onboarding risk problems. One confirms whether a customer's identity is still valid at a specific point, while the other detects changes in behaviour, devices, and transactions over time. Using only one leaves a predictable gap.


Here is how both controls work together to close those gaps:


-


#### **Re-verification confirms identity**


Re-verification helps you confirm that a customer is still who they claimed to be when they were onboarded. It supports compliance reviews and risk events, but it does not show what happens between checks.


-


#### **Continuous monitoring detects live fraud signals**


Account compromise, unusual behaviour, device changes, and SIM swap activity can happen without a scheduled identity review. These signals require ongoing monitoring because they develop through account activity, not identity checks.


-


#### **Together they cover the customer lifecycle**


Re-verification handles identity confirmation when a review or risk event requires it. Continuous monitoring covers the activity between those moments, connecting identity, behavioural, and transaction signals into one risk picture.


Strong post-onboarding risk management comes from connecting re-verification and continuous monitoring into one shared risk picture.


### **How Dojah Supports Both Controls in One Connected Platform**


Re-verification and continuous monitoring become less effective when they operate as separate processes. Dojah closes that gap by connecting


[Profiled Risk's](https://profiledrisk.com/) continuous monitoring with its identity verification infrastructure, giving fraud and compliance teams one shared view of post-onboarding risk.


Here's how both controls work together on the same platform:


- **Continuous monitoring with Profiled Risk:** Profiled Risk monitors account activity after onboarding, identifying behavioural anomalies, device changes, login pattern shifts, and transaction signals associated with account compromise or fraud. These signals are assessed continuously without requiring the customer to complete another identity check.


- **Re-verification triggered by risk signals:** Some risk events require more than monitoring. When Profiled Risk identifies events such as a dormancy-to-activity change, a device change before a high-value transaction, or a change in risk tier, Dojah's identity verification infrastructure supports the re-verification process within the same workflow.


- **One connected risk view:** Monitoring and re-verification work better when they share the same risk context. Risk signals identified after onboarding can directly inform re-verification decisions, giving fraud and compliance teams a connected view instead of separate systems to manage.


Dojah brings re-verification and continuous monitoring together, helping fraud and compliance teams manage post-onboarding risk from one connected platform.


Explore


[Profiled Risk](https://profiledrisk.com/) or book a demo to see how it helps your fintech stay ahead of post-onboarding risk.


### **Frequently Asked Questions About Re-Verification and Continuous Monitoring**


**1. What is the difference between re-verification and continuous monitoring for African fintechs?**


Re-verification confirms a customer's identity again after onboarding, while continuous monitoring tracks behavioural, device, and transaction signals over time. Both are essential for managing post-onboarding risk in fintech Africa.


**2. When should a fintech trigger re-verification?**


Common re-verification triggers for fintech Nigeria include changes in customer risk, sanctions list matches, significant account updates, beneficial ownership changes, and dormant accounts becoming active again.


**3. Can continuous monitoring replace re-verification?**


No. Continuous monitoring detects behavioural and transaction anomalies, but it cannot replace identity checks required for regulatory reviews or triggered re-verification events.


**4. Why is ongoing KYC important for African fintechs?**


Ongoing KYC in Africa helps ensure customer information remains accurate after onboarding while supporting regulatory compliance and reducing fraud risks as customer circumstances change.


**5. How do re-verification and continuous monitoring work together?**


Re-verification confirms identity at scheduled or triggered points, while continuous monitoring identifies emerging risks between those checks. Together, they provide a stronger approach to managing post-onboarding risk for African fintechs.
