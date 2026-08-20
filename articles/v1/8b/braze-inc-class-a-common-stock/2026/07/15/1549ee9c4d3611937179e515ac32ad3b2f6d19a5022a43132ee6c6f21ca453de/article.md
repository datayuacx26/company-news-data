---
schema_version: "1.0.0"
document_id: "1549ee9c4d3611937179e515ac32ad3b2f6d19a5022a43132ee6c6f21ca453de"
company_key: "braze-inc-class-a-common-stock"
company: "Braze Inc."
source_id: "braze-inc-class-a-common-stock-news-import-06f37ae7f1b6"
canonical_url: "https://www.braze.com/resources/articles/france-cnil-email-tracking-pixels"
published_at: "2026-07-30T21:43:28.184+00:00"
first_seen_at: "2026-07-31T20:21:51.363238+00:00"
fetched_at: "2026-07-31T20:21:52.761992+00:00"
content_hash: "sha256:319e9fc1e08e615347c5d7abfd171cabdc4ca09f988c63586293eb71d17e3c06"
---

# The new CNIL recommendations: Email tracking pixels in France

***TL;DR***


*The French Data Protection Authority (CNIL) published its recommendations reiterating and reconfirming the European Data Protection Board’s guidelines and position regarding the use of email tracking pixels and the ePrivacy Directive. By assessing your email marketing programs and compliance and making any needed adjustments, you can help to support the continued success of your customer engagement efforts.*


***Key takeaways***


- *In France, the French Data Protection Authority (CNIL) provides guidance to the public on data protection-related topics*
- *The European Data Protection Board previously indicated that the use of email tracking pixels falls within the scope of the ePrivacy Directive, a view reconfirmed by CNIL*
- *Consumer consent to send them emails does not include the right to use tracking pixels in emails, and separate consent must be collected to leverage this tool*


Customer engagement is constantly evolving, and marketers who don't stay on top of new developments (e.g. new industry practices, changing[inbox provider requirements](https://www.braze.com/resources/reports-and-guides/2024-gmail-yahoo-sender-update-faqs) , shifting consumer expectations etc.) run the risk of falling behind the competition.


The EU has longstanding laws which govern the dynamic nature of the email marketing space. Recently, the French Data Protection Authority (CNIL) published its recommendations reiterating and reconfirming the European Data Protection Board’s guidelines and position that the use of email tracking pixels falls within the scope of the ePrivacy Directive. To help you respond, let's explore what it means for your email marketing strategy.


For years, marketers have treated "open tracking" for email as a standard, backend metric. The CNIL’s new recommendations make it clear that if your emails include tracking pixels, you must obtain consent from your recipients in France unless you can apply an exemption under applicable laws.


## **Key aspects of the CNIL guidance:**


- **Specific consent:** Consent to receive a marketing email is *not* the same as consent to be tracked via pixels. These are separate concepts and must be treated separately. For example, users should have the choice to opt-in to your newsletter while also being able to opt-out of email pixel tracking.
- **Possible exemptions:** CNIL provides example use cases where marketers could benefit from exemptions under applicable law based on specific purposes. These exemptions are narrow and specific, and marketers must assess based on their own use case and purposes for using tracking pixels.
- **Management of consent:** The recommendations make it clear that marketers need to manage user consent related to tracking pixels, including withdrawal of consent.
- **The time is now to review your practices:** CNIL’s recommendations were published on April 14, 2026 and marketers should review their own processes to ensure they are compliant.


## **How Braze customers can assess their practices in light of CNIL’s recommendations**


If you are a Braze customer, you likely rely heavily on real-time engagement data to power your orchestration. While Braze is a powerful tool for personalization, Braze customers should review their use of Braze in line with CNIL’s recommendations.


#### **1. Allow users to signal interest via preference centers or landing pages**


CNIL's new recommendations doesn't forbid brands from using tracking pixels; it just makes it clear what the requirements are for its use. The Braze preference center can be used to collect opt-ins for tracking pixels (including creating specific toggles for these purposes) or, alternatively, brands can create a dedicated landing page that allows email recipients to opt in for this sort of tracking.


#### **2. Rethink your "open-based" Canvas triggers**


Many Braze Canvases use "Email Open" as a filter or a branching step. Braze supports disabling open tracking on the user-level by setting the **email_open_tracking_disabled**[field](https://www.braze.com/docs/user_guide/channels/email/email_setup/open_pixel_and_click_tracking) on the user profile to “true” (with support for Amazon SES coming soon). This excludes the open tracking pixel from all future emails sent to this user.


#### **3. Re-evaluate send-time optimization (STO)**


The Braze platform’s[Intelligent Timing](https://www.braze.com/docs/user_guide/brazeai/intelligence_suite/intelligent_timing) feature is a favorite of many email marketers, but it relies on historical message open data to predict the best moment to reach a given user. If you do not have the right from a specific user to leverage tracking pixels to use this kind of data, you could instead leverage other data metrics—for instance, sending based on a user’s "Local Timezone."


## **The bottom line**


By adjusting your Braze strategies and assessing your compliance position now, you aren't just staying compliant; you're building a foundation of trust with your most valuable users.


*Disclaimer: This post is for informational purposes and does not constitute legal advice. Please seek independent legal advice regarding your use of tracking pixels.*
