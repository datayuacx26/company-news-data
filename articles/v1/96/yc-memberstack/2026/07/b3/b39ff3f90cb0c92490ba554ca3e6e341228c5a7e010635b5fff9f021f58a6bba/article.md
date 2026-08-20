---
schema_version: "1.0.0"
document_id: "b39ff3f90cb0c92490ba554ca3e6e341228c5a7e010635b5fff9f021f58a6bba"
company_key: "yc-memberstack"
company: "Memberstack"
source_id: "yc-memberstack-news-import-456c233bea3a"
canonical_url: "https://www.memberstack.com/blog/how-to-verify-user-phone-numbers-via-whatsapp-before-form-submission"
published_at: null
first_seen_at: "2026-07-22T04:01:55.650331+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:d7c8c569420fbbbaef1e0206a058cdae81b3947a115e8245eddaebd1ffda2750"
---

# How to Verify User Phone Numbers via WhatsApp Before Form Submission

**How to Verify User Phone Numbers via WhatsApp Before Form Submission**


https://www.memberstack.com/scripts/otp-verification-via-whatsapp


Cloneable:


https://webflow.com/made-in-webflow/website/otp-verification-via-whatsapp


Why/Use Cases –


- Lead qualifications
- Secure account submissions


Collecting phone numbers through forms is common, but verifying that those numbers are real can be challenging. With this script, you can require users to confirm their phone number through a one-time password (OTP) sent via WhatsApp before submitting a form on your Webflow site.


By verifying numbers in real time, you reduce spam submissions and ensure the contact information you receive is legitimate. Since many users already rely on WhatsApp daily, the verification process feels familiar and convenient while adding a meaningful layer of security.


In this guide, we’ll walk through how to implement the script step by step, so you can add WhatsApp-based OTP verification to your forms.


**How to verify user phone numbers via WhatsApp before a form submission on Webflow**


In order to set this up, we’re going to use[MemberScript #152 – OTP Verification via WhatsApp in Webflow](https://www.memberstack.com/scripts/onboarding-tour-for-new-members) . Follow the link to get the code you’ll need to add to your page and watch a video tutorial on how to set everything up.


**Setting it up**


The first thing you’ll need to do is set up an account on[whatsauth.com](http://whatsauth.com/) .


Once you’re in, create an app and click on *Try it* once it’s finished. Look at the page URL and copy your API key, we’re going to need this later.


Now go to your form, click on the field where users are supposed to enter their phone numbers and add the attributes:


- data-ms-code=”phone-number”
- data-ms-member=”phone-number”


You can add additional attributes if you want to change how the verification component looks and feels:


- data-ms-placeholder=”Click to validate phone number”
- data-ms-primary-color=”#2D62FF”
- data-ms-secondary-color=”#27AE60”
- data-ms-btn-text=”Verify via WhatsApp”


**Making it work**


Now that you’ve set everything up, you just need to add the MemberScript #152 custom code to your page, before the closing body tag. Replace the existing API key in the custom code with the one you copied earlier.


Unless there’s a reason that you want the code sitewide, it would probably be best to use the code on just the pages where it would be relevant.


Don’t forget to also add the code for WhatsApp authentication from whatsauth.com to your site’s header.


**Conclusion**


That’s all you have to do to verify user phone numbers via WhatsApp before they submit a form on Webflow.


WhatsApp OTP verification helps ensure that the phone numbers collected through your forms are genuine and reachable. This improves lead quality, reduces spam, and adds an extra level of trust to your submission process.


With this script in place, your Webflow forms become more secure and reliable, helping you collect verified contact details while maintaining a smooth user experience.


If you want to use our demo project to get you started, just click the button below to add it to your project.


Button –[Get cloneable](https://webflow.com/made-in-webflow/website/otp-verification-via-whatsapp)
