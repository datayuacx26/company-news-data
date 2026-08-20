---
schema_version: "1.0.0"
document_id: "50a056d0b513855df6b20b7e2679c00f99aec77289063f78a970646d6f627135"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/email-changes-for-bimi"
published_at: "2025-01-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:4701ebe64aa362dc27d6bac1bdc9927835c10e34b92009b74e28afa8fc2f05e5"
---

# What BIMI's Changes Mean for Email

Our customers regularly ask, *"How do we get an avatar to appear in our company emails?"* Including your logo in your emails can provide credibility and help build trust with your audience.


The answer is more complicated than you might think. To work cross-platform, there's a widely used standard called[Brand Indicators for Message Identification](https://bimigroup.org/) (BIMI).


BIMI is an email standard that allows companies to display their logo next to emails in recipients' inboxes. Until recently, however, most email clients required a trademarked logo to display your avatar.


Traditionally, showing a trademark logo required purchasing a certificate called a Verified Mark Certificate (VMC). For a new company, trademarking a logo can be time-consuming and costly, which slowed the adoption of BIMI by brands. Recent changes in the email industry now enable you to add your avatar to your emails without a trademark.


**Note:** Both methods listed below require your domain[DMARC policy](https://resend.com/docs/dashboard/domains/dmarc) to be set to` p=reject` or` p=quarantine` to allow only authenticated messages from your domain. Get help[setting up DMARC](https://resend.com/docs/dashboard/domains/dmarc) on your domain.


## Gmail with CMC support


[Google recently announced it will support Common Mark Certificates (CMC)](https://workspaceupdates.googleblog.com/2024/09/gmail-additional-bimi-protections.html) , which is already part of the BIMI standard. You can purchase a certificate if you can prove you've used your logo publicly for 12 months. Without the trademark requirements of a VMC. The process of obtaining CMC is similar to obtaining an SSL certificate. We recommend using[Digicert](https://www.digicert.com/) for obtaining the certificate.


After receiving your certificate, recipients using the Gmail web app or the Gmail app will see your logo next to your messages.


While Gmail can show a blue verified checkmark for BIMI support, they reserve it for brands with BIMI support via a trademarked logo (i.e., those with a VMC), so it won't appear if you have a CMC.


[Since BIMI is an open standard](https://bimigroup.org/resources/VMC_Requirements_latest.pdf) , we can see some of the steps these providers will go through to verify your logo usage. One requirement for your logo to meet the 12-month mark is that it must be visible on[archive.org](http://archive.org/) for 12 months.


While CMC is part of the BIMI standard, only Gmail currently supports it, although other email clients may adopt it in the future. For more help implementing BIMI, view our[updated BIMI guide](https://resend.com/docs/dashboard/domains/bimi) , which now includes steps with a CMC.


## Apple Mail


With the release of iOS 18.2, Apple Mail now includes avatar support without a trademark through a proprietary method for Apple Mail they call[Branded Mail](https://www.apple.com/newsroom/2024/10/apple-expands-tools-to-help-businesses-connect-with-customers/) .


Like the BIMI standard, Branded Mail will show your logo when viewing a message in Apple Mail. To set this up, create an[Apple Business Connect](https://www.apple.com/business/connect/) account and verify details about your company with Apple.


Once verified, you can provide a` .heif` ,` .jpg` , or` .png` image for your brand, which will display in Apple Mail.


While Apple supports BIMI, Apple Branded Mail is free and universally supported across newer Apple devices. At launch, the Mac Apple Mail client doesn't support Branded Mail, though we expect support to be added in the future. Branded Mail is a great option if your customer base is iPhone/iPad-heavy.


A side-benefit of Branded Mail is Apple Mail's BIMI support is currently limited to recipients using iCloud email addresses. If your recipient uses Apple Mail with a Gmail address, they won't see your BIMI logo. With Branded Mail, a Gmail address using Apple Mail will see your Branded Mail logo. Remember when I said the answer is more complicated than you'd think?


We spent an afternoon setting up Branded Mail for Resend and found it straightforward, as you'd expect from Apple.


For more help setting up Apple Branded Mail, see our[support guide](https://resend.com/docs/knowledge-base/how-do-i-set-set-up-apple-branded-mail) .


## Conclusion


There are now many options for getting your logo to appear as an avatar, making the process easier and more complicated, as the steps are more confusing than ever.


If these options weren't enough, Yahoo and Fastmail support the BIMI standard without a CMC/VMC.


Some senders have used Apple Branded mail and the BIMI logo for Yahoo/Fastmail support (i.e., BIMI support without CMC/VMC). These two options provide a wide range of email provider support without cost to the brand. While enjoying broad support, senders can then evaluate if a CMC/VMC is worth the expense of expanding to include BIMI Gmail support (i.e., with a CMC/VMC).


Here is the current support for brand avatars, broken down by email provider.


Client Apple Branded Mail BIMI w/ a CMC BIMI w/ a VMC BIMI w/out a VMC or CMC


[Apple Mail](https://support.apple.com/en-us/108340) ✓ X ✓ X


[Fastmail](https://www.fastmail.help/hc/en-us/articles/7002542139663-Using-BIMI-in-Fastmail) X X ✓ ✓


[Gmail](https://support.google.com/a/answer/10911320) X ✓ ✓ X


Outlook X X X X


[Yahoo](https://senders.yahooinc.com/bimi/) X X ✓ ✓
