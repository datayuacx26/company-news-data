---
schema_version: "1.0.0"
document_id: "f02c227eef677e5eef933e2e572fcdf4365aa9424a9974b0d5e5f5e3be2ad01f"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/send-sms-verification-code-in-5-minutes/"
published_at: "2024-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:409f162c058feb5107fa2445fdec73aa1070b2fc89eff5279a0cc96c237192cf"
---

# How to Send an SMS Verification Code in 5 Minutes

SMS verification codes can significantly enhance your security measures, providing a quick and effective way to verify a user’s identity. Integrating text message verification codes into your authentication process can save time, streamline workflows, and strengthen user trust.


Plivo’s[Verify API](https://www.plivo.com/verify/) makes it easy to start sending one-time passcodes (OTPs) in **one second** or less. In this guide, we’ll explain why SMS verification is so effective and show you how to set it up in just 5 minutes. Our API allows you to send your first OTP in 90% less implementation time than a legacy verification solution. We'll also provide step-by-step instructions to ensure you can quickly and easily incorporate one-time passcodes (OTPs) into your applications.


## Why should I use SMS verification codes?


While SMS verification isn't foolproof, it's a widely used and convenient security measure. One-time passcodes delivered via SMS or voice add an extra layer of security to online accounts, making them more difficult to break into. Accounts with one-time passcodes enabled as part of two-factor or multi-factor authentication make it much harder for hackers to break in, even if they steal your password.


Financial institutions, e-commerce sites, streaming platforms, and delivery apps. For instance, SMS verification can confirm that the person logging into a peer-to-peer payment platform is the account owner. This can help prevent unauthorized purchases or account takeovers.


## Prerequisites to send one-time passcodes


Before we dive in, make sure you complete the following requirements:


1. **Plivo account** : Sign up for a Plivo account if you still need to do so.
2. **API key and token** : Obtain your Plivo API key and token from the Plivo console.
3. **Phone numbers** : Ensure you can access the phone numbers to which you intend to send verification codes.
4. **Programming environment** : Set up your programming environment with the necessary libraries to interact with the Plivo API. Install the Plivo SDK for your programming language: Plivo supports Python, JavaScript, Ruby, and many more languages.


In summary, make sure you have a[Plivo account with an application created](https://www.plivo.com/docs/verify/concepts/applications/) . While creating the application, define the right session expiry, attempt, OTP length, etc. You will also need to get a library/module/SDK for making HTTP requests to Plivo's API (this is available in various programming languages) or directing HTTP requests to[Plivo’s API](https://www.plivo.com/docs/verify/api/session/) .


With these prerequisites in place, here’s how to start sending OTPs.


## Create a session


Plivo’s Verify API can be used with Python, Ruby, Node, GO, PHP, .Net, Java. This article will focus on Python.


### Step 1: Install Plivo SDK


First, you need to install the Plivo SDK for your programming language. For Python, you can use pip:


```text


pip install plivo


```


### Step 2: Configure Plivo Client


Initialize the Plivo client using your Auth ID and Auth Token:


```text
import plivo
auth_id = ‘YOUR_AUTH_ID’
auth_token = ‘YOUR_AUTH_TOKEN’
client = plivo.RestClient(auth_id, auth_token)


```


### Step 3: Send the OTP


Create a Verify session for sending OTP:


```text
response = client.verify_session.create(
app_uuid=‘xxxxx-1215-422e-222-xxxx’,
recipient=‘+xxxxxxxxxxx’,  # dst number
otp=code,
channel=‘sms’,
method=‘POST’,
locale=‘pt_BR’
)
print(response)


```


**Arguments:**


- **recipient (string):** The phone number to which the message is to be delivered. It’s a mandatory parameter.
- **app_uuid (string):** The UUID of the application you want to use for this session. Defaults to the UUID of the default application for your account.
- **otp:** You can specify the OTP in the request if you want to send a custom one instead of a system-generated one.
- **channel (string):** The channel you want to send the code. Allowed values: sms, voice. Defaults to sms.
- **locale:** The locale parameter allows you to customize the language of the OTP message. This is useful if your users are in different regions and prefer different languages.
- **url:** To receive a callback on the final state of OTP delivery.
- **method:** The HTTP method to be used when calling the URL defined above.


**If you created multiple applications, you can send the app_uuid in the request parameter:**


```text
response = client.verify_session.create(
app_uuid=‘xxxxx-1215-4qq2e-222-xxxx’,
recipient=‘+xxxxxxxxxxx’  # dst number
)
print(response)


```


**If you are sending a custom OTP:**


```text
response = client.verify_session.create(
recipient=‘+xxxxxxxxxxx’,  # dst number
otp=code,
)
print(response)


```


**If you want to send the locale parameter:**


```text
response = client.verify_session.create(
app_uuid=‘sss-1c15-4ww3e-ssss-ssss’,
recipient=‘+xxxxxxxxxxx’,  # dst number
otp=code,
channel=‘sms’,
method=‘POST’,
url=‘ https://www.requestbin.com  ’,
locale=‘pt_BR’
)
print(response)


```


### Validate the session


Once the user receives the OTP, they must provide it to your application. You can then verify the OTP using the validate request:


```text
response = client.verify_session.validate(
session_uuid=‘sss-1c15-4d3e-ssss-ssss’,
otp=code
)
print(response)


```


You can request the Plivo support team to configure the **hashmap** so that the OTP will be automatically read from the message, eliminating the need to enter the received OTP on the handset.


**Arguments:**


- **otp (string):** The OTP that you want to validate against a particular session.
- **session_uuid:** The session UUID of the Verify session request.


### Get and list Verify sessions


You can retrieve details of a specific Verify session or list all Verify sessions. This can be useful for auditing and tracking purposes.


```text
response = client.verify_session.get(
session_uuid=‘sss-1c15-4d3e-ssss-ssss’
)
print(response)```
response = client.verify_session.list()
print(response)
</code>
</pre><h2>Start sending SMS verification codes with Plivo</h2><p>While there are plenty of ways to improve the security of your application and protect customers from fraud, a lot depends on your service provider. </p><p>If you’re looking for a reliable and trusted partner, <a href="https://www.plivo.com/">Plivo</a> is the right solution for you. We send messages to audiences in 220+ countries and offer a full suite of products including <a href="https://www.plivo.com/sms/">SMS API</a>, Verify API, <a href="https://www.plivo.com/whatsapp/">WhatsApp Business API</a>, <a href="https://www.plivo.com/voice/">Voice API</a>, and more.&nbsp;</p><p>Interested in reading more about how Plivo can help you strengthen your application’s security? Check out some of our top picks:&nbsp;</p><ul><li><a href="https://www.plivo.com/blog/how-to-add-two-factor-authentication-to-a-python-flask-application-with-plivo/"><strong>How to Add Two-Factor Authentication to a Python Flask Application with Plivo</strong></a>: A step-by-step guide for Python developers.</li><li><a href="https://www.plivo.com/blog/how-to-add-two-factor-authentication-to-a-dotnet-application-with-plivo/"><strong>Adding Two-Factor Authentication to a .NET Application</strong></a>: Learn how to integrate OTP verification in .NET.</li><li><a href="https://www.plivo.com/blog/how-to-add-two-factor-authentication-to-a-ruby-application-with-plivo/"><strong>Implementing Two-Factor Authentication in Ruby</strong></a>: Ruby developers can follow this detailed tutorial.</li><li><a href="https://www.plivo.com/docs/verify/api/overview/"><strong>Verify API Reference Documentation</strong></a>: Comprehensive documentation for developers looking to dive deep into Plivo's Verify API.</li></ul><p><a href="https://www.plivo.com/sms/?kw=plivo%20sms&amp;cpn=21366394957&amp;utm_campaign_type=search&amp;utm_engagement_type=webform&amp;utm_term=plivo%20sms&amp;utm_campaign=Brand-PlivoSMS%7CSearch%7CIndia&amp;utm_source=google&amp;utm_medium=cpc&amp;hsa_acc=2092392810&amp;hsa_cam=21366394957&amp;hsa_grp=164009262715&amp;hsa_ad=701660175751&amp;hsa_src=g&amp;hsa_tgt=kwd-301628672546&amp;hsa_kw=plivo%20sms&amp;hsa_mt=b&amp;hsa_net=adwords&amp;hsa_ver=3&amp;gad_source=1&amp;gclid=Cj0KCQjwsaqzBhDdARIsAK2gqneaj1tnFvVt3H0jEpjdMEjqppYWaeKQgR_gxbbfdsCF4kVURrTlCLQaAhezEALw_wcB"><strong>Preventing SMS Fraud with Plivo</strong></a>: Learn how Plivo’s Verify API protects against SMS fraud.</p><h2>Conclusion</h2><p>By following these steps, you can easily integrate Plivo’s Verify API into your application to manage OTPs for user verification. This process ensures a higher level of security and helps authenticate users effectively.</p><p>That’s it! You should be ready to start sending OTPs for account verification. For full details regarding setting up OTPs with Verify, check our<a href="https://www.plivo.com/docs/verify/api/overview/"> developer resources</a>.</p>
```
