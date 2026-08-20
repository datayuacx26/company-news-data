---
schema_version: "1.0.0"
document_id: "4885391f53ddde04363b538b02def994dc7192865010d5568acae49da5e49d55"
company_key: "yc-dojah-inc"
company: "Dojah Inc"
source_id: "yc-dojah-inc-news-import-dac996b1a8e0"
canonical_url: "https://dojah.io/blog/biometric-verification-dojah"
published_at: "2026-07-24T08:43:07.516+00:00"
first_seen_at: "2026-07-25T01:49:05.386345+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:332bcc2ce267e3dab683d475e77aa1d4cc9d4e997deac982fd8a58d1d32eac73"
---

# A Practical Guide to Biometric Verification and Fraud Prevention in Your Dojah Flows

When you are onboarding customers, you need to know that the person behind the screen is actually who they claim to be and that they are a real, living person. Two checks make that possible: liveness verification and face match. This article explains how both work, what they defend against, and how to configure them inside your Dojah flow.


### **The Two Pillars of Biometric Verification**


Dojah's biometric verification suite has two distinct tools. They serve different functions and can be used together to form a complete identity verification layer.


**Feature**


**Primary Objective**


**How It Works**


Liveness Check


Confirms the user is a real, present human being, not a photo or pre-recorded video


Prompts the user to complete physical challenges in real time: blinking, nodding, turning their head


Face Match


Confirms the person completing onboarding is the owner of the submitted ID


Compares a live camera capture against the photo on the ID using OCR and machine learning


### **How the Liveness Check Works**


The liveness check stops identity fraud at the point of entry. A common tactic is holding up a printed photograph or displaying someone else's face on a screen to bypass verification. The liveness check makes that approach fail.


**Camera permissions:** When a customer reaches the liveness step in your onboarding flow, they are prompted to grant camera access. Dojah's widget handles this smoothly across environments:


- **Web browsers:** The widget manages the camera permission prompt directly within the browser, desktop or mobile.


- **Mobile apps:** Dojah provides SDKs for Android and iOS so the camera interface feels native to the app rather than embedded from outside.


**Active challenges:** Once camera access is granted, the system does not take a passive photo. It prompts the customer to perform a series of physical actions in real time:


- Tilting their head left or right


- Blinking


- Nodding up and down


- Opening their mouth slightly


These unpredictable physical movements confirm the system is interacting with a live person and not a static image. If a bot or automated script attempts to feed fake images into your verification flow, the liveness check blocks it.


### **How Face Match Works**


Liveness confirms a real person is present. Face match confirms that the person is the owner of the identity they are claiming.


\[User Uploads ID\] ---> \[OCR Extracts Data & Photo\]


|


v


\[Live Camera Capture\] ---> \[Machine Learning Engine\] ---> \[Match/Mismatch Decision\]


**The process:**


1. **ID submission:** The user uploads an eligible ID document or enters an ID number, such as their BVN or another government-issued ID that carries their photograph.


2. **Face capture:** Once the document is submitted, the widget prompts the user to enable their camera for a live face scan.


3. **Backend analysis:** Dojah's engine compares the live capture against the photo extracted from the ID.


**The technology behind it:**


- **OCR (Optical Character Recognition)** reads and extracts text and image data from the uploaded document, converting a physical ID into a structured digital profile.


- **Machine learning** maps unique facial structures from both the live capture and the ID photo to determine whether they match.


One important capability: if a customer's ID photo was taken several years ago, their appearance may have changed. Dojah's system accounts for age-related facial changes, weight fluctuations, and hair changes to accurately confirm whether the live person matches the document photo.


### **What Biometric Verification Protects Against**


Adding biometric checks to your onboarding flow protects both your platform and your legitimate users from several specific threats.


**Deepfakes:** As AI-generated impersonation becomes more sophisticated, active liveness challenges and real-time facial structure analysis help catch attempts to use synthetic faces to bypass verification.


**Identity theft:** If a fraudster obtains someone's physical ID or buys their details, they still cannot pass a face match check without the victim's physical face present during the live camera capture.


**Banned users attempting re-registration:** If you have previously blocked a fraudulent user, they may try to register again under different credentials or using a family member's ID. Comparing their live face scan against existing user records helps detect that attempt before the new account is approved.


### **Configuring Verification Strictness**


Different businesses carry different levels of risk. A financial institution processing large transactions needs tighter controls than a platform with lower-value activity. Within the Fraud Check settings in the Dojah dashboard, you can adjust the strictness of your biometric rules to match your risk threshold.


**Liveness score strictness:**


- **High strictness:** The system requires multiple precise physical challenges to be completed accurately before confirming the user is human. Best for high-risk operations where security is the priority.


- **Lower strictness:** The check is more relaxed, prioritising speed and convenience while maintaining a baseline security layer.


**Face match strictness:**


- **High strictness:** The live face must be a close structural match to the ID photo. Best for platforms where identity confidence is critical.


- **Lower strictness:** The algorithm allows more variation, making it easier for users with older ID photos or lower camera quality to complete onboarding without false rejections.


***Want to see it in action?***


[Check out Dojah Learnings for a full video breakdown on how to add biometric verification to your onboarding flow.](https://dojah.io/my-learnings)


### **Setting It Up in EasyOnboard**


Configuring biometric verification does not require changes to your codebase. It can be set up directly in the EasyOnboard dashboard.


1. Log into your Dojah dashboard and navigate to the EasyOnboard tab.


2. Select or create the onboarding flow you want to edit.


3. Add a new page to your user journey and select the Liveness Page. This prompts users to complete camera challenges at that stage.


4. Navigate to the Fraud Check settings section within your flow's menu bar.


5. Toggle on the Liveness Check option under fraud prevention settings.


Once enabled, the system automatically links the submitted ID to the live face capture. If the live scan does not match the ID photo, the system flags the attempt and halts onboarding. You are notified immediately so you can review the case before the account is created.


**Integration Methods**


Dojah fits into whatever stack your team is already running. There are three ways to deploy these biometric checks:


**1. Shareable link:** The fastest, zero-code option. Generate a unique link from your Dojah dashboard and share it with customers. They complete onboarding, document upload, and biometric checks through the link, and all verified data comes back to your dashboard.


**2. Mobile SDKs:** For native mobile app experiences, Dojah's SDKs for Android and iOS handle camera permissions, UI overlays, and real-time challenge prompts natively, keeping the experience clean and consistent with your app.


**3. API integration:** For teams that want full control over the UI and data routing, Dojah's API lets you run backend checks, upload documents, and process biometric verification programmatically. Full reference in


[the Dojah Documentation guide.](https://docs.dojah.io/)


Ready to add biometric verification to your onboarding flow?


[Explore the Dojah docs](https://docs.dojah.io/) or


[create your account](https://dojah.io/contact-sales) to see how it works in practice.


**FAQs**


**1. What is the difference between liveness check and face match?** Liveness check confirms that a real, present person is completing the verification, not a photo or video. Face match confirms that the person is the owner of the submitted ID by comparing their live face to the ID photo.


**2. Can the face match check handle older ID photos?** Yes. Dojah's machine learning model accounts for age-related facial changes, weight fluctuations, and hair changes, so a photo taken several years ago does not automatically cause a mismatch.


**3. What happens if a user fails the liveness or face match check?** The system halts the onboarding process and notifies your team immediately. You can review the flagged case and decide whether to allow, reject, or request a retry from the user.


**4. Do I need a developer to set up biometric verification in EasyOnboard?** No. Liveness check and face match can both be configured directly in the EasyOnboard dashboard without any code changes. For teams that want more control over the flow, API integration is also available.


**5. Can biometric verification detect if a banned user tries to re-register?** Yes. By comparing a new applicant's live face scan against existing user records, the system can flag if a previously banned user is attempting to register under different credentials.
