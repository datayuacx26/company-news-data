---
schema_version: "1.0.0"
document_id: "47557b41f938fc43acb329d923b5dbdb727c2854e305dab65171c649ce6c9566"
company_key: "yc-onesignal"
company: "OneSignal"
source_id: "yc-onesignal-rss-a77922638bdd"
canonical_url: "https://onesignal.com/blog/understanding-the-recent-email-tracking-guidance-from-france-and-italy/"
published_at: "2026-08-13T20:56:35+00:00"
first_seen_at: "2026-08-13T22:26:26.437440+00:00"
fetched_at: "2026-08-13T22:26:27.373356+00:00"
content_hash: "sha256:02ce68a183093c13647554481bfbb6bdc972d0921b6d4bac1c68da8514453ec4"
---

# Understanding the Recent Email Tracking Guidance from France and Italy

The French (CNIL) and Italian (Garante) data protection authorities published new guidance on the use of email tracking pixels. These are **not new laws** . Instead, they clarify how each regulator interprets existing privacy rules as they apply to tracking pixels in email.


This generally focuses on recipients located in France or Italy rather than citizenship alone. However, how these rules apply to individual recipients can depend on the circumstances and applicable privacy laws.


The guidance generally treats email open tracking similarly to other tracking technologies, such as website cookies. For many common marketing and analytics uses of open tracking, senders are expected to obtain consent before tracking a recipient's email opens. There are some exceptions for specific technical, security, deliverability, and service-related purposes, and the exemptions are not identical between France and Italy.


It also emphasizes transparency around what is being tracked and why, as well as giving recipients an easy way to withdraw their consent without having to unsubscribe from email entirely.


All of this is primarily focused on **open tracking** . Click tracking is not directly covered by the French recommendation, although CNIL notes that similar principles can be used when evaluating tracked links. Requirements and interpretations around click tracking may continue to evolve.


### What should you do today?


The most important thing is **don't panic or rush into making major changes.** There are still practical questions that senders, email service providers, and the broader industry are working through.


One of the biggest challenges is location. Many senders don't collect country or location data for their subscribers, which makes it difficult to know which recipients may be covered by the French or Italian guidance. There are also open questions around how global senders should handle these requirements and what approaches will become standard across the industry.


For now, familiarize yourself with the guidance and consult with your legal advisor. Some steps to consider are:


- **Audit -** Review and map how you currently use email open tracking, particularly if you send email to recipients in France or Italy
- **Fix your consent collection going forward** - For any new email address you collect, consent to tracking should be obtained at the point of capture, alongside your existing email consent, not after the fact
- **Handle your existing list** - For recipients already in your database, CNIL required that you notify them and offer an opt-out by July 14. Under Garante you will need to notify them around mid-October 2026. Because these are time sensitive, we recommend you consult your legal advisor as soon as possible.


We will continue to follow developments as regulators and the email industry work through these requirements.


**If your organization determines that changes are necessary now, OneSignal's existing functionality provides options for limiting or disabling email tracking. We've outlined those options below.**


### How can you disable open tracking in OneSignal today?


You have a few options available with OneSignal's current functionality.


### 1. Blanket approach


You can turn off tracking for emails coming from your domain by deleting the[CNAME record](https://documentation.onesignal.com/docs/en/email-dns-configuration#cname-records) provided in your OneSignal Email settings from your DNS registrar.


Once this record is removed, **open and click events will no longer be logged for emails sent from that domain going forward.** This is the simplest approach, but it also means losing these engagement metrics across your email program.


### 2. Create a secondary subdomain without tracking


For more control, you can[create a secondary sending subdomain](https://documentation.onesignal.com/docs/en/senders#sending-domain) without the CNAME record provided by OneSignal. Emails sent using this subdomain will not log open or click events.


This allows you to continue sending emails with tracking from your existing subdomain while using the secondary subdomain for recipients or campaigns where you do not want tracking enabled.


**For example** :


**email.example.com** → tracking enabled
**email-notracking.example.com** → tracking disabled


This approach allows you to separate tracked and non-tracked email without turning off tracking across your entire email program.


### How can you target recipients who should not be tracked in OneSignal?


There are a couple of approaches you can use today to identify recipients who should receive emails without open tracking.


### 1. Start collecting consent for open tracking


If you choose to collect tracking consent, you can capture that preference when someone provides their email address. This can be done alongside your existing signup or email consent process by clearly asking whether the recipient agrees to email open tracking.


You can then store that preference in OneSignal as a tag, such as:


**open_tracking_consent = true open_tracking_consent = false**


This allows you to build segments based on tracking preferences. Recipients without the appropriate tracking consent can be sent email using your secondary subdomain with tracking disabled.


Recipients should also have an easy way to change their preference later. For example, you could include an **"Email tracking preferences"** link near your unsubscribe link that directs recipients to a preference page you control.


When a recipient changes their preference, update the corresponding tag in OneSignal so future emails can be sent using the appropriate tracking configuration.


### 2. Use location data you already collect


If you already collect location information and can reliably identify recipients in France or Italy, you can use that data to create separate OneSignal segments.


Those segments can then receive email using your secondary subdomain with tracking disabled, while other recipients can continue receiving email through your existing sending configuration.


If you don't currently collect location data, we would not suggest trying to determine a recipient's location based solely on their email address or mailbox provider. An **@gmail.com** , **@outlook.com** , or similar address does not reliably tell you where that recipient is located.


### What OneSignal is doing


We're closely monitoring the guidance from France and Italy and how regulators, senders, and other email providers respond. There are still important questions about how these requirements should work in practice, especially for senders that don't know the location of every recipient.


**For now, we're not making immediate changes to how email open tracking works in OneSignal.** Open data plays an important role in email analytics, audience engagement, and deliverability workflows. Making broad changes before the requirements and common implementation patterns are clearer could have a meaningful impact on customers without necessarily solving the underlying challenges.


We're continuing to evaluate what additional product capabilities may be appropriate as regulatory expectations, customer needs, and industry implementation patterns become clearer. As we learn more, we'll share updates with our customers and provide guidance on any changes they may need to make.


### Disclaimer


*This article is for informational purposes only and does not constitute legal advice. The French and Italian guidance is still evolving, and how it applies to your email program depends on your specific circumstances, including where your recipients are located, how you collect data, and how you use open tracking. As the sender, you are best positioned to assess your compliance obligations. We recommend consulting legal counsel before making changes to your tracking practices.*
