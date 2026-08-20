---
schema_version: "1.0.0"
document_id: "0061484f1b6ac3f2335c594db2479f90a5c250021a7421ad232c79a46db5faa7"
company_key: "yc-memberstack"
company: "Memberstack"
source_id: "yc-memberstack-news-import-456c233bea3a"
canonical_url: "https://www.memberstack.com/blog/how-to-welcome-members-with-a-first-time-login-product-tour"
published_at: null
first_seen_at: "2026-07-22T04:01:55.650331+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:fcdf5a6bd89dd16f86f86af7899b2f2017b83174168716a477c123f612e276de"
---

# How to Welcome Members with a First-Time Login Product Tour

**How to Welcome Members with a First-Time Login Product Tour**


[https://www.memberstack.com/scripts/onboarding-tour-for-new-members](https://www.memberstack.com/scripts/onboarding-tour-for-new-members)


Cloneable:


https://webflow.com/made-in-webflow/website/onboarding-tour-for-new-members


Why/Use Cases –


- Structured onboarding experience
- Introducing new features


First impressions matter—especially when a member logs into your platform for the very first time. With this Memberstack script, you can automatically launch a step-by-step product tour that welcomes new members and introduces them to the most important parts of your Webflow site.


Using Intro.js, this script creates a streamlined on-boarding experience that triggers on a member’s initial login. It guides users around your platform by emphasizing key sections and providing informative tool-tips along the way.


In this guide, we’ll walk through how to implement the script step by step, so you can greet new members with a helpful product tour the first time they log in.


**How to welcome new members with a first-time login product tour using Webflow**


In order to set this up, we’re going to use[MemberScript #151 – Onboarding Tour for New Members](https://www.memberstack.com/scripts/onboarding-tour-for-new-members) . Follow the link to get the code you’ll need to add to your page and watch a video tutorial on how to set everything up.


**Setting it up**


After creating all the steps, select the container that houses them and add the attribute *ms-code-tour=”true”* .


Then select the containers for each step and add the following attributes:


- ms-code-step=”1” – or 2, 3, 4, and so on
- ms-code-intro=”TEXT” – replace “TEXT” with the text you want to appear on that step *(e.g. “Welcome to your dashboard” for step 1)*


Now you can just add a code embed on the page where you can paste the CSS which affects the appearance of each step.


**Making it work**


Now that you’ve set everything up, you just need to add the MemberScript #151 custom code to your page, before the closing body tag.


You’ll also need to add the Intro.js code in the header.


**Conclusion**


That’s all you have to do to welcome new members with a product tour the first time they log into your Webflow site.


A first-time login tour helps turn a potentially confusing moment into a guided and welcoming experience. By showing members exactly where to go and what to do, you make it easier for them to feel comfortable and engaged from the start.


With this script in place, your Webflow site delivers a stronger on-boarding experience—helping new members explore your platform with confidence.


If you want to use our demo project to get you started, just click the button below to add it to your project.


Button –[Get cloneable](https://webflow.com/made-in-webflow/website/onboarding-tour-for-new-members)
