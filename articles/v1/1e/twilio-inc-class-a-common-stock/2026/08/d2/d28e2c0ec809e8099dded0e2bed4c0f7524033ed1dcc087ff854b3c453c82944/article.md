---
schema_version: "1.0.0"
document_id: "d28e2c0ec809e8099dded0e2bed4c0f7524033ed1dcc087ff854b3c453c82944"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/integrations/verify-phone-number-replit"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T16:06:27.343495+00:00"
fetched_at: "2026-08-13T16:06:28.718566+00:00"
content_hash: "sha256:554edfb18294c5d17897030578437cc271880cdfb71e4c7ce7d42da929094e30"
---

# How to verify a user's phone number in your Replit app

Time to read:


-
-
-
-
-


August 12, 2026


**Written by**[Kelley Robinson](https://www.twilio.com/en-us/blog/authors/author.krobinson) Twilion


**Reviewed by**[Paul Kamp](https://www.twilio.com/en-us/blog/authors/author.pkamp) Twilion


---


## How to verify a user's phone number in your Replit app


Whether you want to validate users at sign up, prove phone number ownership, or just make sure you're communicating with a real person, you need phone verification. Phone verification is an important feature in nearly every modern application and verifying that number helps ensure every future phone number interaction can be trusted.


[Twilio Verify](https://www.twilio.com/docs/verify) handles sending and checking SMS One-Time Passwords (OTPs) for phone verification so you can focus on your application’s logic. And when you build and host your application on[Replit](https://replit.com/) , you can have a working prototype in minutes.


In this post, I’ve put together a sample application that lets you vote on the name for my puppy, *after* you've verified your phone number, providing a useful gate that limits voting to one vote per phone number. To build along, you can grab the[code on GitHub](https://github.com/twilio-samples/replit-verify) or see the[sample application live on Replit](https://replit-verify--kelleyrobinson.replit.app/) , or follow the instructions below to add Verify into your own Replit application.


*My very real puppy*


## Prerequisites


Before we dive in, make sure you have the following ready:


- A **free Twilio account** .[Sign up here](https://twilio.com/try-twilio) or login.
- A **free**[Replit account](https://replit.com/) .
- **A Twilio Verify Service:** Create one in the[Twilio Console](https://1console.twilio.com/go?to=/account/__account__/us1/verify/services) . (Twilio Console => Verify => Services). Keep that **Verify Service SID** (it starts with VA...) handy for the next step.


## Step 1: Create your Replit application


Head over to[Replit](https://replit.com/~) and click the option to **"** **Import Code or Design"** . Choose **GitHub** then paste in the sample repo:[https://github.com/twilio-samples/replit-verify](https://github.com/twilio-samples/replit-verify) . This uses a lightweight Node.js and Express stack to serve the front end and handle routing for verifications.


## Step 2: Store secrets securely


**Never** paste your credentials directly into your source code. Instead, use Replit’s built-in Secrets manager. If you haven't done this before, check out our guide on[How to Store Twilio Credentials Securely in Replit](https://www.twilio.com/en-us/blog/developers/tutorials/integrations/store-twilio-credentials-replit-secrets) .


Add these variables ([create a new API Key](https://1console.twilio.com/go?to=/account/__account__/us1/keys-credentials/api-keys) in the console):


- TWILIO_ACCOUNT_SID
- TWILIO_API_KEY
- TWILIO_API_SECRET
- TWILIO_VERIFY_SERVICE_SID


## Step 3: Sending the Verification Code


The logic for sending and checking verification codes lives in[/lib/verify.js](https://github.com/twilio-samples/replit-verify/blob/main/lib/verify.js) .When a user submits their phone number, we tell Twilio to send them an SMS with a few lines of code:


Copy code


```text
const twilio = require("twilio");
const client = twilio(TWILIO_API_KEY, TWILIO_API_SECRET, { accountSid: TWILIO_ACCOUNT_SID });
const verifyService = client.verify.v2.services(TWILIO_VERIFY_SERVICE_SID);
function sendVerificationCode(phoneNumber) {
return verifyService.verifications.create({
to: phoneNumber,
channel: "sms",
});
}
```


This` sendVerificationCode` function call tells Twilio to generate and dispatch a One-Time Password (OTP) to the user via SMS. Using a temporary, single-use code ensures that the person signing up actually owns the phone number, without you needing to build custom code-generation or expiration logic yourself. Twilio manages all of the telephony, including the sender number, so you can spend less time on compliance and more time with your teething fluff ball.


## Step 4: Checking the Verification Code


Once the user receives the 6-digit code, they’ll input it into your app. The app then sends that code back to Twilio to verify it’s correct:


Copy code


```text
const twilio = require("twilio");
const verifyService = client.verify.v2.services(TWILIO_VERIFY_SERVICE_SID);
async function checkVerificationCode(phoneNumber, code) {
const check = await verifyService.verificationChecks.create({
to: phoneNumber,
code,
});
return check.status === "approved";
}
```


And that` checkVerificationCode()` function is Verify’s gate – if a user enters a fake code, they’ll never get to the next step, whatever that is in your app. If they enter the actual code,` check.status` comes back as` approved` , and you can let your users move onto the next step…whether you’re running a puppy name voting app or gating signups.


And with that, you’re ready to deploy and test it out!.


## Test it out!


Now hit that big **Run** button in Replit.


Once the app is deployed, you’ll want to open it up and vote! Hit the **Vote** button next to your favorite name:


That will prompt you to enter your phone number for verification. Enter it[in E.164 format](https://www.twilio.com/docs/glossary/what-e164) , then click **Confirm Vote** .


If everything is set up correctly – and you enter the right code – you’ll see the vote counter tick up by one after verifying your number. Try voting again and you should be blocked from voting twice.


## Debugging common issues


Vote not counted? Common errors you might hit include:


- **Phone format:** Make sure you're using[E.164 format](https://www.twilio.com/docs/glossary/what-e164) (e.g., +1234567890).
- **Secrets:** Double-check that your[Replit Secrets are mapped correctly](https://www.twilio.com/en-us/blog/developers/tutorials/integrations/store-twilio-credentials-replit-secrets) .
- **Rate limits:** If you're testing repeatedly, requesting a code to the same number more than 5 times in 10 minutes will[start returning errors](https://www.twilio.com/en-us/blog/test-verify-no-rate-limits) .


## Next steps


Congratulations! You've just built phone verification into your application. Next, to take the puppy name voting app further, you could add a Replit database to store the phone numbers that have already voted in your app (a requirement for productionalizing something like this!), persist the vote counts outside of local memory, or extend voting to include the ability to submit new name ideas.


You can use the same Verify Service to verify users via WhatsApp, Email, or Voice all by changing the channel parameter in the request. If you're looking for more options, check out[everything you can do with the Verify API in our documentation](https://www.twilio.com/docs/verify/api/) . In the meantime, support your local animal shelter!


*Kelley Robinson is a Developer Evangelist at Twilio specializing in Authentication and Identity. The puppy in question was adopted from*[CGHS](https://cghs.org/) *, is actually 4 now, and somehow answers to all of the disparate name options, including several others not listed.*
