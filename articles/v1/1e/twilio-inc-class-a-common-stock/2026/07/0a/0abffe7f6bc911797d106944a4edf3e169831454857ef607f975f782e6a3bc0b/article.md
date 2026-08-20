---
schema_version: "1.0.0"
document_id: "0abffe7f6bc911797d106944a4edf3e169831454857ef607f975f782e6a3bc0b"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/integrations/okta-byok-verify"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:fa5ed69c0f2d1c329c81bd6d53bebb716c39cd55cddb057beeee5c61e84f9f7e"
---

# How to Bring Your Own Keys for Okta Telephony with Twilio Verify

Time to read:


-
-
-
-
-


July 07, 2026


**Written by**[Kelley Robinson](https://www.twilio.com/en-us/blog/authors/author.krobinson) Twilion


**Reviewed by**[Paul Kamp](https://www.twilio.com/en-us/blog/authors/author.pkamp) Twilion


---


## How to Bring Your Own Keys for Okta Telephony with Twilio Verify


Okta requires companies to[bring their own telephony](https://support.okta.com/help/s/article/bring-your-own-telephony-required-for-sms-and-voice?language=en_US) (BYOT) if they want to send SMS one-time passcodes (OTPs) or phone calls for multi-factor authentication or account recovery.[Twilio Verify](https://twilio.com/verify) is an out-of-the-box solution for sending OTPs, and now Okta makes it straightforward to get set up with Verify in just a few steps.


This post will cover how to provide Okta with the necessary API keys to configure your telephony and be up and running in no time.


## Prerequisites for Adding Twilio Verify to your Okta Org


Before you get started, you'll need the following account details:


- A Twilio Account ([login](https://twilio.com/console) or[create](https://twilio.com/try-twilio) one for free if you don’t have one yet)
- Twilio Account SID (` ACxxx…` , find it at[twilio.com/console](https://twilio.com/console) )
- Twilio Auth Token (also find it at[twilio.com/console](https://twilio.com/console) )
- Verify Service SID (` VAxxx…` , find or create one here:[twilio.com/console/verify/services](https://twilio.com/console/verify/services) )
- Enable[Custom Code](https://www.twilio.com/docs/verify/api/customization-options) on your Verify Service.


Custom code allows Okta to set the verification code in the request. This is necessary for integrating with Okta's telephony workflows.


We strongly recommend using Verify for SMS and Voice use cases for its turnkey API, strong fraud prevention, managed global senders, ongoing routing optimization, managed compliance and more. In addition to its friendly onboarding and ease of use for MFA, Verify's telephony channels also continue to be a great option for[account recovery](https://www.twilio.com/en-us/blog/best-practices-multi-factor-authentication-mfa-account-recovery) .[Learn more about the benefits of Twilio Verify](https://www.twilio.com/en-us/blog/9-reasons-to-use-the-verify-api) .


## Bring your own key


Inside your Okta admin console, navigate to **Customizations** >> **Telephony Providers** . Select **Twilio** and paste in your **Account SID** , **Auth Token** , and **Verify Service SID** .


Okta encrypts and securely stores your credentials.


Okta automatically sends verification feedback to Twilio to let us know when the verification has been approved in Okta. This will ensure accuracy of your logs.


## Test and run


After you've saved your telephony details, you can add SMS and Voice as MFA and recovery options to your Okta configuration.[Learn more about configuring telephony in the Okta documentation.](https://help.okta.com/en-us/content/topics/telephony/telephony-how-to-tasks.htm)


Then add a phone number to your user and test the end to end flow following Okta's instructions for[configuring end user phone authentication](https://help.okta.com/oie/en-us/content/topics/identity-engine/authenticators/configure-phone.htm) .


After end to end testing with a real login, head to the[Twilio Console Verify Logs](https://console.twilio.com/us1/monitor/logs/verify-logs) . Select a Verification ID to open its Verification details page and view more comprehensive information such as different actions and outcomes that happened during the Verification's lifecycle and any[related error codes](https://www.twilio.com/docs/api/errors#60000-69999) , if applicable.


Once you have the feedback enabled, you will have access to the[Verify SMS Fraud Insights dashboard on Twilio Console](https://console.twilio.com/us1/monitor/insights/verify/verify-fraud-insights) . The dashboard illustrates the impact fraud could have had without intervention, and also allows you to discover trends and insights that you can use to better optimize your product against fraud.


To view your dashboard, go to[Twilio Console](https://console.twilio.com/) and navigate to **Monitor > Insights > Verify > Fraud** which will open the Overview tab. There, you'll find several sections relating to your Fraud metrics


## Customize your Okta telephony with Twilio Verify implementation


If you need a more customized approach, learn[how to set up Twilio Verify with Okta inline hooks](https://www.twilio.com/en-us/blog/okta-byot-verify) . The inline hook approach requires more configuration but allows you more flexibility to do things like[Lookup a phone number before sending an OTP](https://www.twilio.com/en-us/blog/filter-voip-before-otp-verification) or anything else you might want to do in code.


That's it! Now you can monitor performance of your Verify OTPs with the[insights dashboard](https://console.twilio.com/us1/monitor/insights/verify/verify-insights) and explore more options in the[Verify API documentation](https://www.twilio.com/docs/verify) .
