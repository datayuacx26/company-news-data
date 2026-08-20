---
schema_version: "1.0.0"
document_id: "60a4867ececb141249f58a28139e7938714277d39b8e57d03d09b14cbb235f4b"
company_key: "yc-memberstack"
company: "Memberstack"
source_id: "yc-memberstack-news-import-456c233bea3a"
canonical_url: "https://www.memberstack.com/blog/how-to-highlight-key-features-for-new-members-using-a-guided-tour"
published_at: null
first_seen_at: "2026-07-22T04:01:55.650331+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:9a4273a50f556cae08855363377c5ce11945aa3abaafc70cf6c57756bf1e068b"
---

# How to Highlight Key Features for New Members Using a Guided Tour

**How to Highlight Key Features for New Members using a Guided Tour**


[https://www.memberstack.com/scripts/onboarding-tour-for-new-members](https://www.memberstack.com/scripts/onboarding-tour-for-new-members)


Cloneable:


https://webflow.com/made-in-webflow/website/onboarding-tour-for-new-members


Why/Use Cases –


- Highlight core platform tools
- Introduce workflow steps


When new members join your platform, they may not immediately know which features matter most. A guided product tour helps solve this by visually highlighting key parts of your Webflow site and explaining what they do. With this Memberstack script, you can automatically launch a tour that walks new members through the platform’s most important tools.


This script uses Intro.js to deliver a simple on-boarding flow that starts automatically on a user’s first login. It walks members through your interface by spotlighting important features and offering clear, helpful tool-tips.


In this guide, we’ll walk through how to implement the script step by step, so you can guide new members through your platform and highlight its most important features.


**How to highlight key features for new members using a guided tour on a Webflow site**


In order to set this up, we’re going to use[MemberScript #151 – Onboarding Tour for New Members](https://www.memberstack.com/scripts/onboarding-tour-for-new-members) . Follow the link to get the code you need to add to your page and watch a video tutorial on how to set everything up.


**Setting it up**


Once all the steps have been created, select the container where they’re nested and add the attribute *ms-code-tour=”true”* .


Then select the container for each step and add the following attributes:


- ms-code-step=”1” – or 2, 3, 4, and so on
- ms-code-intro=”TEXT” – replace “TEXT” with the text you want to appear on that step *(e.g. “Welcome to your dashboard” for step 1)*


Finally, just add a code embed on your page where you can paste the CSS that stylizes the appearance of each step.


**Making it work**


Now that you set everything up, you just need to add the MemberScript #151 custom code to your page, before the closing body tag.


You’ll also need to add the Intro.js code in the header.


**Conclusion**


That’s all you have to do to highlight key features for new members using a guided tour on a Webflow site.


A guided tour that highlights key features helps new members quickly understand what your platform offers and how to use it. By pointing users toward the most valuable tools early on, you create a smoother and more engaging onboarding experience.


With this script in place, your Webflow site becomes easier to explore and more intuitive—helping members discover the features that matter most from the start.


If you want to use our demo project to get you started, just click the button below to add it to your project.


Button –[Get cloneable](https://webflow.com/made-in-webflow/website/onboarding-tour-for-new-members)
