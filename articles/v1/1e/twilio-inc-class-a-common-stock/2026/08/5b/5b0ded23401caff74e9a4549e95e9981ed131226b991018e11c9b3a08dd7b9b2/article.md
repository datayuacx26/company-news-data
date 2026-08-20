---
schema_version: "1.0.0"
document_id: "5b0ded23401caff74e9a4549e95e9981ed131226b991018e11c9b3a08dd7b9b2"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/migrate-programmable-messaging-to-verify"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T12:15:01.558338+00:00"
fetched_at: "2026-08-14T12:15:04.372394+00:00"
content_hash: "sha256:f6b357bb8ab4655192c6f945614b60ab738d5ce55cc35df5ff181756f1eefed7"
---

# Migrate from Programmable Messaging to Verify

Time to read:


-
-
-
-
-


August 13, 2026


**Written by**[Kelley Robinson](https://www.twilio.com/en-us/blog/authors/author.krobinson) Twilion


**Reviewed by**[Matthew Gilliard](https://www.twilio.com/en-us/blog/authors/author.mgilliard) Twilion


---


## Migrate from Programmable Messaging to Verify


The Verify API is a purpose-built solution for sending One-Time Passcodes (OTP) for user verification and authentication via SMS, voice, WhatsApp, email, push and TOTP. Twilio's Programmable Messaging API provides many businesses with the foundation to build their own OTP solutions. However, maintaining an in-house OTP solution can be complex and resource intensive, especially as the messaging landscape and compliance requirements continue to shift. Many companies are migrating to Verify for the same global reliability and unparalleled delivery at scale as Twilio's programmable messaging with the added benefits of:


- One-click[Fraud Guard protection](https://www.twilio.com/docs/verify/preventing-toll-fraud/sms-fraud-guard/) to automatically block[SMS traffic pumping](https://support.twilio.com/hc/en-us/articles/8360406023067-SMS-Traffic-Pumping-Fraud)
- Regulatory and compliance management; Verify is exempt from[A2P 10DLC](https://support.twilio.com/hc/en-us/articles/1260800720410-What-is-A2P-10DLC-) registration when using a provided pooled sender
- Managed sending phone number pool included, including short codes, long codes, toll free, and global alpha-sender IDs*
- Managed worldwide delivery such as sender types and compliance on a global scale
- Stateless API for handling token generation and checking (with an option to bring-your-own code)
- Templatized OTP message translations in[dozens of languages](https://www.twilio.com/docs/verify/supported-languages)
- Multi-channel support for SMS, voice, RCS,[WhatsApp](https://www.twilio.com/docs/verify/whatsapp/) ,[silent network authentication](https://www.twilio.com/docs/verify/sna) ,[email](https://www.twilio.com/docs/verify/email) ,[push](https://www.twilio.com/docs/verify/push) , and[TOTP](https://www.twilio.com/docs/glossary/totp)


This guide provides an introduction to the[Verify API](https://www.twilio.com/docs/verify/api) and a set of guidelines to migrate your application from Programmable Messaging to Verify.


**[Pre-registration required for alpha sender IDs](https://twiliodoer.secure.force.com/SenderId) in certain countries.[Read more](https://support.twilio.com/hc/en-us/articles/223133767-International-support-for-Alphanumeric-Sender-ID) .*


[Dive further into the benefits of the Verify API with this in depth guide.](https://www.twilio.com/en-us/blog/9-reasons-to-use-the-verify-api)


### Migrate to Verify faster with Twilio's AI coding skills


Twilio also publishes an[agent skill](https://www.twilio.com/docs/ai/skills) for this exact migration: **[twilio-migrate-messaging-to-verify](https://github.com/twilio/ai/blob/main/skills/twilio/twilio-migrate-messaging-to-verify/SKILL.md)** . The skill provides your AI coding assistant (Claude Code, Cursor, Codex, whatever you use) a vetted, straightforward reference for the task.


To use the skill:


- [Install the plugin](https://www.twilio.com/docs/ai/skills#install-twilio-skills)
- Point your assistant at your existing OTP implementation and ask it to migrate the flow to Verify: the skill will walk the agent through what Verify replaces.


The skill will use the same guidance covered in this post and apply it directly to your code.


### Migration requirements for sending tokens with Verify


The migration process requires some initial one time set up and then swapping out one API call. Assuming you already have an OTP user experience, your product design can be reused. You'll also get to delete a decent amount of code, which is always satisfying.


**With Verify, you don't need to buy a phone number OR store OTPs.** The three steps to sending a verification code with Verify are:


1. Create a[Verify service](https://www.twilio.com/console/verify/services) (one time setup)
2. Send the SMS code
3. Check the code


With Programmable Messaging, at minimum you *also* need to buy a phone number(s), generate a random numeric code, and store the code in your database to associate it with the end user before sending and checking the OTP. Verify eliminates the need for these additional steps with the option to still use your phone numbers and custom codes if you choose to.


Here's an example of the Programmable Messaging code required to **send an OTP** :


Copy code


```text
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
const client = require("twilio")(accountSid, authToken);


const from = "YOUR TWILIO NUMBER";
const to = "+15558675310";


// generate a 6 digit token
const token = Math.floor(Math.random() * (999999 - 100000) + 100000);
const message = `Your Example App verification code is: ${token}`;


function storeToken(to, token) {
/*
* Required:
*   - Store token with the user's phone number in your database
*   - Set token expiration
*/
}


// send OTP
client.messages.create({ body: message, from, to }).then((message) => {
storeToken(to, token);
console.log(message.sid);
// prompt user for token
});
```


The Verify version of the same code (below) removes the need for:


1. a` from` number
2. generating a token
3. the` storeToken` function


Copy code


```text
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
const client = require("twilio")(accountSid, authToken);


const to = "+15558675310";


// send OTP
client.verify
.services("YOUR VERIFY SERVICE SID")
.verifications.create({ to, channel: "sms" })
.then((verification) => {
console.log(verification.status);
// prompt user for token
});
```


#### Verify Services


Verify uses Services for configuration. To send a Verify API request you will need both your Twilio Credentials and a Service SID. You can create and update a Service in two ways:


1. With the[Verify API](https://www.twilio.com/docs/verify/api/service)
2. In the[Verify Console](https://www.twilio.com/console/verify/services)


Services can be used to edit the name (which shows up in the message template), set the code length (4-10 characters), enable settings like the "do not share warning" and more.


#### Bring your own custom code


We encourage you to use Verify's code generation but you could always use your own logic by providing the` customCode` parameter in the API call shown above. Check out the[customization options](https://www.twilio.com/docs/verify/api/customization-options) to learn more about how to enable this parameter. If you're using custom verification codes we ask that you[provide feedback](https://www.twilio.com/docs/verify/api/verification#update-a-verification-status) that lets us know whether or not the user verified the code. This allows us to proactively monitor our global routing and stay operational.


#### Verification message localization


The Verify API handles localization and translation in dozens of languages from Afrikaans to Vietnamese. Learn more about how we set[default languages](https://www.twilio.com/docs/verify/default-phone-verification-languages) and how to send a localized message in the[documentation](https://www.twilio.com/docs/verify/supported-languages) .


### Migration requirements for checking tokens with Verify


While checking a token isn't hard, the Verify API removes the need to fetch a stored token from your storage.


In Programmable Messaging, checking a token might look like this:


Copy code


```text
const to = "+15558675310";
const token = "123456"; // acquired from UI


function fetchToken(to) {
/*
* Required:
*   - Fetch the token stored with the user's phone number
*   - Ensure the token is not expired
*/
}


// check OTP
const validToken = fetchToken(to);
if (token === validToken) {
// successful
} else {
// failed
}
```


With Verify, checking a token requires an additional API call but removes the need to store the token and fetch it:


Copy code


```text
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
const client = require("twilio")(accountSid, authToken);


const to = "+15558675310";
const token = "123456"; // acquired from UI


// check OTP
client.verify.services("YOUR VERIFY SERVICE SID")
.verificationChecks
.create({to, code: token})
.then(check => {
if (check.status === "approved") {
// successful
} else {
// failed
}
});
```


### Error handling


Verify error codes are different from Programmable Messaging error codes and can be[found in the documentation](https://www.twilio.com/docs/api/errors#6-anchor) . Common errors include:


- [Error 60200 - Invalid parameter](https://www.twilio.com/docs/api/errors/60200) . For example, an invalid phone number.
- [Error 60203 - Max send attempts reached](https://www.twilio.com/docs/api/errors/60203) . See the blog post on[How to test Twilio Verify without getting rate limited](https://www.twilio.com/blog/test-verify-no-rate-limits) .


### Strict E.164 formatted phone number


In Verify, verifications are started with[E.164 format](https://www.twilio.com/docs/glossary/what-e164) phone numbers like this:` +15552317654` . Programmable Messaging is more lenient with the phone number parameter, so things like spaces were allowed. Check out the[Lookup API](https://www.twilio.com/docs/lookup/api#examples) and[this blog post](https://www.twilio.com/blog/international-phone-number-input-html-javascript) for examples on how to format numbers in E.164.


### Best practices


#### Use the Lookup API to convert phone numbers to E.164 format


The free Lookup formatting API call will give you two pieces of useful information:


1. The phone number in[E.164 format](https://www.twilio.com/docs/glossary/what-e164) . Required format for ongoing verification.
2. The country code in[ISO 3166 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements) format (e.g.` US` ,` CA` ,` BR` , etc.). This is necessary to[build an allow list or block list of countries](https://www.twilio.com/blog/allow-list-country-code-lookup/) .


Store both the E.164 formatted phone number and the country in your database for future use.


Learn more in the[Lookup documentation](https://www.twilio.com/docs/lookup/api?code-sample=code-carrier-lookup-with-national-formatted-number&code-language=curl&code-sdk-version=json) .


#### Define allowed countries


If you have a global user base, you can allow all countries. If you are only expecting traffic from a handful of countries then you can create an allow list to help[mitigate toll fraud](https://www.twilio.com/docs/verify/preventing-toll-fraud) . On the flipside, you can also create a block list if there are countries you do not expect traffic from.


### FAQ


#### Why am I getting the same code with new verification requests using Verify?


Verify tokens are valid for 10 minutes and during that period the passcode will not change. To force a new passcode, complete the[verification lifecycle](https://www.twilio.com/docs/verify/api/verification-check) .


#### Are rate limits different in Verify?


Rate limits for Verify SMS include:


- 5 send verification attempts within 10 minutes per unique phone number \[[more info](https://www.twilio.com/docs/api/errors/60203) \].
- 5 check verification attempts per unique phone number \[[more info](https://www.twilio.com/docs/api/errors/60202) \].


Please reach out to[support](https://support.twilio.com/hc/en-us) for more information on rate limits. Most customers find the default rate limits for Verify sufficient, but you can also protect your application with additional[service rate limits](https://www.twilio.com/docs/verify/api/programmable-rate-limits) .


#### What is Verify's pricing?


Verifications base pricing is $0.05/checked verification. Remember that Verify includes the cost of global phone numbers and[Fraud Guard](https://www.twilio.com/docs/verify/preventing-toll-fraud/sms-fraud-guard/) . Learn more on the[Verify pricing page](https://www.twilio.com/en-us/verify/pricing) or[get in touch](https://www.twilio.com/en-us/verify/request-consultation) for volume pricing.


#### What other verification methods does Verify support?


Verify includes support for SMS, RCS, voice,[WhatsApp](https://www.twilio.com/docs/verify/whatsapp/) ,[silent network authentication](https://www.twilio.com/docs/verify/sna) ,[email](https://www.twilio.com/docs/verify/email) ,[push](https://www.twilio.com/docs/verify/push) , and[TOTP](https://www.twilio.com/docs/glossary/totp) .
