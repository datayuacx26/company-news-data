---
schema_version: "1.0.0"
document_id: "394cbbe01894dfab83990ee4397b58ec19848276844f287164b7004cd2725b77"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-auth-passwordless-sms-login"
published_at: "2021-07-28T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:5614370d1b6d4bb7f17630fd8f03ec8c217afcd4edeb3a8f6d0250c7f99497a2"
---

# Supabase Auth v2: Phone Auth now available

Since launching Supabase Auth[last summer](https://news.ycombinator.com/item?id=24072051) it's proven to be a key part of the Supabase Stack. We receive a constant stream of feature requests and community PRs resulting in a long list of external providers including GitHub, Discord, Azure, Apple and more.


Supabase Auth is similar to Auth0 and Firebase Auth with one major difference - the user data lives in your own database, reducing lock-in, and making the auth system more extensible. You can write native PostgreSQL Row Level Security policies to determine which data your users should (or should not) have access to. It can even be used in conjunction with other Supabase features, such as[Storage](https://supabase.com/storage) , to control access for specific files and buckets.


Today we're announcing some major new features for our[fork](https://github.com/supabase/gotrue) of Netlify's[GoTrue](https://github.com/netlify/gotrue) Auth server.


## Phone Auth is here!#


Your users can now log in using their mobile with SMS-based OTPs (one-time password).


### Passwordless SMS login#


Users can log in using a passwordless SMS based OTP with` supabase-js` , or directly with the Auth API.


After logging in, the user will receive a six-digit One Time Password. The OTP can be easily verified.


### SMS login with passwords#


Phone Auth can be used in conjunction with a password. Using this flow, your users can subsequently log in with either an OTP or a phone + password combo.


### Choose an SMS Provider#


Supabase Auth supports[Twilio](https://www.twilio.com/) as an SMS provider, with more options coming soon. Simply plug your Twilio credentials into your Auth Settings in the Supabase Dashboard to get started.


Check out the[documentation](https://supabase.com/docs/guides/auth/phone-login/twilio) to get started with Mobile OTPs, or watch the[Youtube guide](https://youtu.be/akScoPO01bc) .


### Multi-Factor Auth coming soon#


Phone Auth is available today on all new and existing Supabase projects. We've also laid the groundwork for mobile Multi-Factor Auth and will be offering that as an option soon.


## Even more OAuth providers#


The community has contributed tons of OAuth providers, and today we're announcing two more.


Shoutout to[@ph1p](https://github.com/ph1p) who contributed[Twitch](https://supabase.com/docs/guides/auth/auth-twitch) as our latest OAuth provider! The Supabase team added[Discord](https://supabase.com/docs/guides/auth/social-login/auth-discord) last month, bringing the total OAuth Providers to ten.


You can request more providers on our[Auth repo](https://github.com/supabase/gotrue) and Pull Requests are, of course, always welcome.


## Generate Confirmation Links#


To make life easy for developers, the Supabase hosted platform manages all Auth-related emails, including confirmation, recovery, invite, and passwordless "magic-link" emails. The templates are customizable and we even offer the ability to bring your own SMTP provider.


Some of our power users require a little more flexibility however. We've had a lot of requests to dynamically generate email content, especially for sending internationalized emails. To handle situations like these, today we're adding the ability to generate confirmation, invite, recovery, and magic links via an API endpoint.


We've exposed this functionality in` supabase-js` , and it can be invoked with the use of your` service_role` admin key (which means you should only be calling this function from a backend and not from the client itself).


## What's next?#


The next major item on the list is MFA (Multi-Factor Authentication) - which includes TOTP ([Time-Based One Time Password](https://en.wikipedia.org/wiki/Time-based_One-Time_Password) ).


Find out how Mobbin is using Supabase Auth to[manage 200,000 users](https://supabase.com/blog/mobbin-supabase-200000-users) .


Anything else you want to see or can help implement in Auth? Reach out on[Discord](https://discord.supabase.com/) and give Auth a try by[creating a project](https://supabase.com/dashboard/) on Supabase!
