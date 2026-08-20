---
schema_version: "1.0.0"
document_id: "b842a7041c9b891579e716344f47c3de2efd182d7ce9eb72602621ac38ff037e"
company_key: "yc-freshpaint"
company: "Freshpaint"
source_id: "yc-freshpaint-news-import-105ad9035257"
canonical_url: "https://www.freshpaint.io/blog/alert-what-the-ftc-state-complaint-against-hims-hers-means-for-healthcare-marketers-and-privacy-leaders"
published_at: "2026-08-19T12:50:20.126+00:00"
first_seen_at: "2026-07-31T22:18:03.885794+00:00"
fetched_at: "2026-07-31T22:18:05.955025+00:00"
content_hash: "sha256:4867638b715669a04fdb738bfa48a83d39755fc6b91f2f0da38a5cf1fe9e39e9"
---

# ALERT: What the FTC/State Complaint against Hims & Hers Means for Healthcare Marketers and Privacy Leaders

*Disclaimer: Freshpaint does not provide legal advice.*


## **What happened**


This past June, FTC Chairman Andrew Ferguson signaled his agency would focus on a surge of privacy enforcements in the second half of this year. So much so that the media would “have a hard time keeping up.”


Just over a month later, on July 29, the FTC, the California People (via LA County Counsel), and the Utah Division of Consumer Protection[filed a joint complaint](https://www.ftc.gov/system/files/ftc_gov/pdf/Hims-Complaint-Redacted-E-Filed.pdf) against Hims & Hers Health, Inc. in the Northern District of California alleging deceptive and unlawful privacy practices.


## **The privacy allegations**


Alongside unfair and deceptive subscription and cancellation claims, the complaint includes allegations that Hims shared consumers' sensitive health information with advertising platforms — despite marketing the service as “private.”


- The complaint alleges Hims misled consumers about its health information sharing practices with advertising platforms by promising privacy across its homepage, ads, paid influencer posts, and offline media. For example, Hims claimed to be “100% online, private, and secure.”
- The complaint alleges that, through at least May 2024, Hims shared health information with third parties including Meta, Snap, Microsoft, Google, Criteo, Pinterest, Reddit, StackAdapt, TikTok, The Trade Desk, X, and other third parties through their tracking technologies.
- Two mechanisms referenced that are worth noting: customer-list uploads (audience matching) and the Meta Pixel plus the Conversions API (server-to-server).


## **Hims’ response**


In its[response to the FTC lawsuit](https://investors.hims.com/news/news-details/2026/Hims--Hers-Responds-to-FTC-Lawsuit/default.aspx) , Hims pushed back hard against the FTC’s claims, stating the agency’s claims “ignores established state laws and industry standards in telehealth.” Additionally, the telehealth organization has claimed its privacy policy gave users enough information to make informed decisions about their use of their services.


## **Four areas to review this week**


1. **Do your data flows match your privacy claims?** One of the key similarities between the FTC complaints against BetterHelp, FloApp and Hims is that they all focused on deceptive privacy claims. Organizations should consider auditing privacy representations on their sites, in ads, and in any paid/influencer creative the organization approves. Avoid overpromises about privacy practices that cannot be substantiated.
2. **What are you sending server-side, and does it carry health signals?** Organizations should consider conducting an inventory of their CAPI and S2S connections. This inventory should confirm exactly which events and parameters go to each destination, and whether any of them share a condition, diagnosis, treatment, or intake answer.
3. **Are you uploading customer lists to ad platforms?** If an organization is using audiences, they should verify that health information is not being exposed through list sharing or through remarketing ad copy. Beyond that, policies should accurately represent an organization’s use of audiences.
4. **Is it time to talk about consent?** While marketers sometimes want to avoid the topic of consent, it’s a good time to review and update your organization’s consent approach. If you don’t have a consent manager today, it could be beneficial to instate one to avoid disruption as privacy becomes an enforcement focus again. Consent management should be done in accordance with applicable law and in collaboration with your legal team.


## **How Freshpaint can address key privacy patterns**


Freshpaint helps healthcare organizations address common privacy gaps.


- **Identify native trackers exposing data:** Freshpaint Web Tracker Monitor helps you proactively identify if there are unwanted native tracking technologies living on your site so you can quickly remove or replace them.
- **Server-side as an enforcement point:** Freshpaint replaces native advertising and analytics trackers with a BAA or DPA-covered server-side control layer so data can be governed before it is shared with downstream third parties.
- **Consent enforced at collection:** Consent is enforced at the data layer and inherited by every downstream destination, helping prevent the unauthorized sharing of health information.
- **Data minimization by default:** Our recommended allowlists are designed to provide ad platforms with the conversion needed for measurement and optimization while limiting the sharing of diagnosis, condition, drug, or intake-related information with ad platforms/shared downstream. Freshpaint Audiences creates targeted audiences for use in ad platforms without sharing actual health context.


If you have questions or want to discuss your tracking technology privacy posture,[schedule a meeting with one of our experts](https://www.freshpaint.io/contact-sales) .
