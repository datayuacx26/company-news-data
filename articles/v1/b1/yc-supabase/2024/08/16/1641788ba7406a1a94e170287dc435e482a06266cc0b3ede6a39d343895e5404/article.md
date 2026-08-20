---
schema_version: "1.0.0"
document_id: "1641788ba7406a1a94e170287dc435e482a06266cc0b3ede6a39d343895e5404"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/third-party-auth-mfa-phone-send-hooks"
published_at: "2024-08-14T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:9b15e7e0588dad70b90a22aeeead74e1bfc000588abe30b93de20689b1ac3cf5"
---

# Supabase Auth: Bring-your-own Auth0, Cognito, or Firebase

Today we have 3 new announcements for Supabase Auth:


1. Support for third-party Auth providers
2. Phone-based Multi-factor Authentication (SMS and Whatsapp)
3. New Auth Hooks for SMS and email


Let's dive into each new feature.


## Support for third-party Auth providers#


The headline feature today is[third-party Authentication](https://supabase.com/docs/guides/auth/third-party/overview) .


Supabase is a modular platform. We've been designing it so that you can choose which products you use with Postgres. You can use our own products (like Supabase Auth) or external products (like Auth0), and *in theory* the experience should be just-as-delightful.


Until today, using third-party auth products required developers to translate JWTs into a format compatible with Supabase Auth. This is difficult and unmaintainable.


So we fixed it. Today we're adding first-class support for the following third-party authentication products:


1. [Auth0](https://supabase.com/docs/guides/auth/third-party/firebase-auth)
2. [AWS Cognito](https://supabase.com/docs/guides/auth/third-party/aws-cognito) (standalone or via AWS Amplify)
3. [Firebase Auth](https://supabase.com/docs/guides/auth/third-party/firebase-auth)


Firebase Auth is currently under a private-alpha release stage, as we're still improving the security developer experience when using it.[Register your interest](https://forms.supabase.com/third-party-auth-with-firebase) and someone from the team will reach out.


Migrating auth providers can be costly and technically challenging, especially for applications with large user bases. You can use Supabase's native auth offering alongside your third-party authentication provider to achieve a disruption-free migration.


All of the third-party providers are supported in the Supabase CLI, so you can evaluate, test, and develop your integration for free.


The Supabase client supports third-party auth like this:


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, {


_10


accessToken: async () => {


_10


const accessToken = await auth0.getTokenSilently()


_10


return accessToken


_10


},


_10


})


`


## Phone-based multi-factor authentication#


We've extended MFA to[support SMS and WhatsApp](https://supabase.com/docs/guides/auth/auth-mfa) .


We have a strong conviction that all applications should have access to an open and secure authentication provider. Secure-by-default should not be a luxury: developers should have affordable access to security best-practices.


[Almost two years ago](https://supabase.com/blog/mfa-auth-via-rls) we launched[MFA with TOTP (app authenticator)](https://supabase.com/docs/guides/auth/auth-mfa) free of charge. Since then, we've heard a common complaint from developers: app authenticators can be hard to adopt for non-techies. Phone-based MFA is for those developers who want to provide a more accessible MFA experience for their users.


No security product is infallible! MFA with SMS can come with some hidden security drawbacks - please evaluate your application's risk tolerance for SIM-swapping attacks.


The code looks like this:


`
_14


// Send an SMS or WhatsApp message to the user


_14


const { data: { challengeId } } = await supabase.auth.mfa.challenge({


_14


factorId,


_14


})


_14


_14


// To verify the code received by the user


_14


await supabase.auth.mfa.verify({


_14


factorId,


_14


challengeId,


_14


code: '123456',


_14


})


_14


_14


// The user's \`aal\` claim in the JWT


_14


// will be upgraded to aal2


`


## Auth Hooks for SMS and Email#


We've added a few new[Auth Hooks](https://supabase.com/docs/guides/auth/auth-hooks) , which supports HTTP endpoints as a webhook now.


**Email Hooks**


We've heard the (rather loud) feedback that the built-in email templates (based on the Go templating language) can be limiting. There's been a lot of development in email rendering libraries like[Resend's React Email](https://resend.com/blog/react-email-2) . To help make this available for developers, we've added a["Send Email" Auth Hook](https://supabase.com/docs/guides/auth/auth-hooks/send-email-hook) , which you can use to customize your emails and how they are sent.


**SMS Hooks**


Supabase Auth has built-in support for popular SMS sending providers like Twilio, Messagebird, Textlocal and Vonage, but we realize this choice can be limiting.


Today we're launching a new["Send SMS" Auth Hook](https://supabase.com/docs/guides/auth/auth-hooks/send-sms-hook) . You no longer need to use the built-in provider - you can implement your own by specifying a HTTP endpoint that receives a POST request when a message needs to be sent.


## Getting started#


Check out the docs for more details on how to get started:


- [Third-party Authentication](https://supabase.com/docs/guides/auth/third-party/overview)
- [Multi-factor Authentication](https://supabase.com/docs/guides/auth/auth-mfa)
- [Auth Hooks](https://supabase.com/docs/guides/auth/auth-hooks)
