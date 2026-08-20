---
schema_version: "1.0.0"
document_id: "f25cdfdffb8ad028a2647a55c275e70c42de083c89bd210a85e1dcdbe897810b"
company_key: "yc-memberstack"
company: "Memberstack"
source_id: "yc-memberstack-news-import-456c233bea3a"
canonical_url: "https://www.memberstack.com/blog/how-to-launch-an-onboarding-tour-when-a-member-logs-in"
published_at: null
first_seen_at: "2026-07-22T04:01:55.650331+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:7d11856aba97c5eb2d06adba705559980e9d89d6b698525f29b52b2d6676f281"
---

# How to Launch an On-Boarding Tour When a Member Logs In

## **How to Launch an On-Boarding Tour When a Member Logs In**


[https://www.memberstack.com/scripts/onboarding-tour-for-new-members](https://www.memberstack.com/scripts/onboarding-tour-for-new-members)


Cloneable:


https://webflow.com/made-in-webflow/website/onboarding-tour-for-new-members


Why/Use Cases –


- Dashboard onboarding
- Introducing new features


When members log into your platform for the first time, they may not immediately know where to start. A step-by-step product tour helps solve this by guiding users through the most important parts of your Webflow site right after their first login.


This script uses Intro.js to create a lightweight on-boarding experience that automatically launches the first time a member logs in. It highlights key areas of your product and provides helpful tool-tips to guide users through your interface.


In this guide, we’ll walk through how to implement the script step by step, so your product tour launches automatically the first time a member signs in.


**How to launch an on-boarding tour when members log into your Webflow site**


In order to set this up, we’re going to use[MemberScript #151 – Onboarding Tour for New Members](https://www.memberstack.com/scripts/onboarding-tour-for-new-members) . Follow the link to get the code you’ll need to add to your page and watch a video tutorial on how to set everything up.


**Setting it up**


Once you create all the steps, select the container in which the nest and add the attribute *ms-code-tour=”true”* .


Then select each step container and add the following attributes:


- ms-code-step=”1” – or 2, 3, 4, and so on
- ms-code-intro=”TEXT” – replace “TEXT” with the text you want to appear on that step *(e.g. “Welcome to your dashboard” for step 1)*


Next up, just add a code embed on the page and this is where you can paste the CSS that adjusts the appearance of each step.


**Making it work**


Now that you set everything up, you just need to add the MemberScript #151 custom code to your page, before the closing body tag.


You will also need to add the Intro.js code in the header.


**Conclusion**


That’s all you have to do to launch an on-boarding tour when a new member logs into your Webflow site.


A guided product tour makes the first login experience far more welcoming and intuitive. By showing members exactly where to go and what to do, you reduce confusion and help them get value from your platform faster.


With this script in place, your Webflow site becomes easier to learn and more engaging from the very first interaction—setting the stage for stronger long-term member engagement.


If you want to use our demo project to get you started, just click the button below to add it to your project.


Button –[Get cloneable](https://webflow.com/made-in-webflow/website/onboarding-tour-for-new-members)
