---
schema_version: "1.0.0"
document_id: "b09edb3dccc0096f74b002e97735402758be0ddb5ad5b83a7f293678c4b59349"
company_key: "yc-dojah-inc"
company: "Dojah Inc"
source_id: "yc-dojah-inc-news-import-dac996b1a8e0"
canonical_url: "https://dojah.io/blog/dojah-phone-number-check"
published_at: "2026-07-31T07:09:56.305+00:00"
first_seen_at: "2026-07-31T23:32:44.751599+00:00"
fetched_at: "2026-07-31T23:32:45.926816+00:00"
content_hash: "sha256:73e4c14b1d337e2115f8ed934d099f325d25d3dd727ff02490ec41a108e7497c"
---

# How Dojah's Phone Number Check Works: A Product Guide

A fraudster gains access to a customer's account without touching their password. They call the telecom provider, convince them to port the customer's number to a new SIM card, and then start intercepting OTPs. By the time the real customer notices, the account has already been compromised.


[SIM swap fraud](https://dojah.io/blog/sim-swap-fraud-africa-2026) is one of the most common account takeover methods in Africa, and a phone number sitting in your onboarding flow carries more information about that risk than most fintechs ever check for. Beyond confirming that a number exists, a phone number can tell you whether it has recently been swapped to a new SIM, whether it has changed carriers, whether it is a temporary or disposable number, and whether it is behaving in ways commonly associated with fraud.


That is what Dojah's Phone Number Check is built to surface, before a customer completes onboarding and before a fraudster gets the access they are looking for.


This product guide explains how Phone Number Check works and how to implement it in your onboarding flow.


### **What Phone Number Check Actually Verifies**


Dojah’s


[Phone Number Check](https://dojah.io/all-products/phone-number-check) does more than confirm that a number exists and is active. It returns a set of signals that together tell you how trustworthy a number is and how much confidence to place in the person behind it.


At the basic level, it confirms:


- Whether the number is active


- Whether it is a mobile, landline, or virtual number


- The carrier or network provider


- The country associated with the number


- Whether it is a disposable or temporary number


- Social account matches where available


- An overall risk score combining all signals


Beyond these, Phone Number Check surfaces the deeper telecom signals that matter most for fraud detection.


### **The Signals That Catch Fraud Early**


**SIM swap history:** When a fraudster convinces a telecom provider to move a customer's number onto a new SIM, they can intercept OTPs and security messages. A recent SIM swap is a strong enough signal that additional verification should happen before any sensitive action is allowed. A stable SIM history increases confidence that the number is still with its rightful owner.


**Recent port activity:** People switch carriers for legitimate reasons, but a phone number that has recently moved to a new network can warrant closer attention, especially when other risk signals are present at the same time. Phone Number Check identifies recent carrier changes so you can factor them into your overall risk assessment.


**Carrier type:** Phone Number Check identifies the network provider and whether the number is mobile, landline, or virtual. Virtual numbers and certain number types carry a higher association with fraud and may require closer scrutiny depending on your risk policies.


**Number-to-device consistency:** Most genuine users access their accounts from familiar devices. When a trusted phone number appears on an unfamiliar device alongside other risk signals, it can indicate account takeover or device spoofing. This signal adds context to help your team make a more informed decision.


**Social account matching:** Where available, Phone Number Check identifies publicly associated accounts linked to the phone number, enriching the customer profile and adding another layer of confidence during verification.


**Risk score:** Each signal on its own is useful. Combined, they are more powerful. Phone Number Check brings all signals together into a single risk score, making it easier to automate verification decisions without manually interpreting every data point for every user.


### **How Dojah's Phone Number Check Fits Into Your Onboarding Flow**


[Phone Number Check](https://dojah.io/all-products/phone-number-check) works best as an early layer in your verification process, before asking a customer to upload an ID or complete a liveness check. Running it at the point of phone number entry gives your team a risk signal before the user moves any further through onboarding.


Here is how a typical flow looks with Phone Number Check in place:


**Step 1:** The user enters their phone number.


**Step 2:** Phone Number Check screens the number in the background using Dojah's telecom intelligence layer.


**Step 3:** Low-risk users continue through onboarding without interruption.


**Step 4:** Users with elevated risk signals are routed to additional verification steps before moving forward.


**Step 5:** Identity and liveness checks provide further confirmation before the account is approved.


Because the check runs silently in the background, legitimate users experience no added friction. You apply stronger verification only where the signals actually warrant it.


**Implementing it is straightforward:** Phone Number Check is available through the


[Dojah API](https://docs.dojah.io/overview/quickstart) . You pass the phone number at the point of collection, and the API returns the full signal set and risk score in real time, with no extra steps added to the user journey. The full API reference and sample payloads are available at


[docs.dojah.io](https://docs.dojah.io/) .


### **Where Phone Number Check Matters Most for African Fintechs**


SIM swap fraud and account takeover attacks show up across the customer lifecycle, not just at onboarding. Here is where Phone Number Check adds the most value:


- **At onboarding:** Screening a phone number before identity and liveness checks helps you route higher-risk users to stronger verification before an account is created.


- **At login:** A phone number that has recently been swapped or ported is a signal worth checking before granting access to an existing account.


- **Before high-value transactions:** For transfers, withdrawals, or account changes, a real-time phone number check adds layer of confidence that the person initiating the action is who they say they are.


- **Across African telecom networks.** Phone Number Check works across Nigerian, Kenyan, Ghanaian, South African, and broader African networks, returning signals relevant to the fraud patterns showing up in African fintech onboarding today.


### **Get Started With Dojah’s Phone Number Check**


For developers and product managers looking to add phone number intelligence to your onboarding or transaction flow, explore the


[Dojah API documentation](https://docs.dojah.io/) to see the full signal set, sample responses, and implementation guides. You can create a free account to test it directly in the sandbox before going live.


If you want to see how Phone Number Check fits into a broader identity and fraud prevention stack,


[book a demo](https://dojah.io/contact-sales) , and we will walk you through it.


**FAQs**


**1. What does Dojah's Phone Number Check actually verify?** It verifies whether a number is active, identifies the carrier and number type, detects recent SIM swaps and port activity, checks for number-to-device consistency, surfaces social account matches where available, and returns an overall risk score combining all signals.


**2. What is SIM swap fraud and why does it matter at onboarding?** SIM swap fraud happens when a fraudster convinces a telecom provider to move a customer's number to a new SIM card, letting them intercept OTPs and security messages. Detecting a recent SIM swap before onboarding completes lets your team flag the account for additional verification before access is granted.


**3. Does Phone Number Check add friction to the onboarding experience?** No. The check runs in the background while the user continues through the onboarding flow. Only users with elevated risk signals are asked to complete additional verification steps.


**4. Which African markets does Phone Number Check cover?** Phone Number Check works across Nigerian, Kenyan, Ghanaian, South African, and broader African telecom networks.


**5. How does the risk score work?** The risk score combines all available signals- SIM swap history, port activity, carrier type, device consistency, and social matches- into a single output that makes it easier to automate verification decisions without manually reviewing every signal.
