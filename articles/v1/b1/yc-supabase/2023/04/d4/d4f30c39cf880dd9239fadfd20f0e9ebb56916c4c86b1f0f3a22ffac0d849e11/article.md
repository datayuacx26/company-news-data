---
schema_version: "1.0.0"
document_id: "d4f30c39cf880dd9239fadfd20f0e9ebb56916c4c86b1f0f3a22ffac0d849e11"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-auth-sso-pkce"
published_at: "2023-04-13T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:2fb7adaf684cd7fc256358431746661df46f816096f0a4fe760d1cd94c03ae23"
---

# Supabase Auth: SSO, Mobile, and Server-side support

Today we're excited to announce a few new features for Supabase Auth:


1. [Easily add Single Sign-On support to your projects using SAML 2.0](https://supabase.com/blog/supabase-auth-sso-pkce#single-sign-on-support-using-saml-20)
2. [Better support for server-side rendering and mobile apps using PKCE](https://supabase.com/blog/supabase-auth-sso-pkce#server-side-and-mobile-auth)
3. [Native Apple login on iOS](https://supabase.com/blog/supabase-auth-sso-pkce#native-apple-login-on-ios)


## Single Sign-On Support using SAML 2.0#


With Single Sign-On (SSO), your users can login with their company's identity provider (IDP), a critical feature when you're building applications for Enterprises.


Every developer building a B2B application eventually needs the SSO authentication flow to onboard enterprise customers. SSO is a requirement for larger Enterprise customers because it's a standard request in Enterprise Security Policies. Over the past few months, we've been[dogfooding SSO for our own Enterprise customers](https://supabase.com/docs/guides/platform/sso) , and today we're releasing it for you to do the same.


Building SSO into your application isn't necessarily hard, but does come with some complexity. A lot of time can be spent understanding the nuances and details of the protocol - from dissecting the jargon to testing the implementation heavily. It took us months to build it for ourselves. With this release, you will have SSO set up and running in less than an hour so that you can focus on shipping the core features of your product. This feature is available for the[Pro Plan and above](https://supabase.com/pricing) , starting today. This will also be available on the self-hosted version.


### Getting Started with SAML 2.0#


To get started, enable the[“SAML 2.0“](https://supabase.com/dashboard/project/phcnitosaawbzytgyznx/auth/providers)[authentication method in the dashboard](https://supabase.com/dashboard/project/_/auth/providers) . We've added new commands to the[Supabase CLI](https://supabase.com/docs/guides/cli) to help with the configuration process:


`
_13


$ supabase sso --help


_13


Manage Single Sign-On (SSO) authentication for projects


_13


_13


Usage:


_13


supabase sso \[command\]


_13


_13


Available Commands:


_13


add Add a new SSO identity provider


_13


info Returns the SAML SSO settings required for the identity provider


_13


list List all SSO identity providers for a project


_13


remove Remove an existing SSO identity provider


_13


show Show information about an SSO identity provider


_13


update Update information about an SSO identity provider


`


Once you've added a new SSO identity provider to your project, it's as simple as calling the` signInWithSSO()` from the` supabase-js` library:


`
_10


const { data } = await supabase.auth.signInWithSSO({ domain: 'acme.corp' })


_10


_10


if (data.url) window.location.href = data.url


`


### SSO with Row Level Security and multi-tenancy#


As usual, we've engineered this feature around the excellent capabilities of PostgreSQL.


For example, you can use Row Level Security (RLS) to build multi-tenant applications, simply by using the provider's unique identifier in the user's JWT:


`
_10


create policy "Only allow read-write access to tenants" on tablename as restrictive to authenticated using (


_10


tenant_id = (select auth.jwt() -> 'app_metadata' ->> 'provider')


_10


);


`


The journey to enterprise readiness isn't an end goal, it is a continuous process that demands constant attention and maintenance. With Supabase Auth, your team can offload this engineering burden to us and prioritize the features that matter.


## Server-Side and Mobile Auth#


Many developers today are using Supabase to build mobile apps, and server-side rendering is becoming popular (again!). This release will add support for these use cases by introducing the *Proof Key for Code Exchange flow (PKCE)* authentication flow. This improves security for mobile apps and makes building server-first apps simple. Since this is a major update that touches many of the authentication routes, we will be rolling it out gradually over the next few weeks.


### A brief history of Supabase Auth#


When we[launched](https://news.ycombinator.com/item?id=24072051) Supabase Auth, our target was JAMstack developers. In these cases, the protocol used between the user's application and Supabase Auth is known as the[Implicit Grant Flow](https://www.rfc-editor.org/rfc/rfc6749#section-4.2) :


As developers built more complex apps, they encountered two problems with this authentication flow:


- **Server-Side Email Verification Links** Data provided in a URL fragment is only accessible in a browser environment, not on the server. This is problematic for email verification links that redirect users to a server-side route.
- **Challenges with Mobile App Authentication** The implicit grant flow raised security concerns for mobile use cases since[malicious apps could potentially obtain the user session](https://www.rfc-editor.org/rfc/rfc7636#section-1) .


Server-side auth unlocks a number of benefits. Developers can:


- Set cookies on the same domain as the application.
- Enable server-side rendering for protected pages.
- Perform downstream actions after user authentication, such as adding the user to a CRM or sending analytics.


### Introducing PKCE#


To solve these problems, we're introducing support for the *Proof Key for Code Exchange* flow (PKCE, pronounced “pixy”).


The PKCE flow introduces a *code verifier* (a randomly generated secret) and a *code challenge* (the hash of the *code verifier* ). The authorization code is returned as a query parameter so it's accessible on the server. During the PKCE flow:


1. The *code challenge* is sent to Supabase Auth, which returns an *authorization code.*
2. The client sends the *authorization code* together with the *code verifier* to obtain the user's session.
3. Supabase Auth checks if the *code verifier* matches the *code challenge* sent earlier by computing the hash. This renders a malicious attacker's attempt to intercept the authorization code useless, since they need to know the value of the *code verifier* as well.


### Migrating to PKCE on the client#


Over the next few weeks, you'll be able to use it with the Supabase libraries. We've already added PKCE to the[JavaScript](https://supabase.com/docs/reference/javascript/installing) client library and our[auth-helpers](https://supabase.com/docs/guides/auth/auth-helpers) library. If you're using` supabase-js` , you can switch to PKCE by initializing your client with the following option:


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, {


_10


auth: {


_10


flowType: 'pkce',


_10


},


_10


})


`


For client-side auth, that's all you need to do to switch over.` supabase-js` will handle the generation and storage for the code verifier, as well as exchanging the authorization code for the user's session.


### Migrating to PKCE on the server#


Server-side authentication is now a lot easier. Let's look at an example using Next.js.


Install the` next` version of auth-helpers (lets use the` nextjs` version for this example)


`
_10


npm install @supabase/auth-helpers-nextjs@next


`


Then prepare an endpoint for the sign in process. The redirect URL is set to` /api/auth/callback` , which will be implemented next.


`
_28


// api/auth/login


_28


import { NextApiRequest, NextApiResponse } from 'next'


_28


import { createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'


_28


_28


export default async function handler(req: NextApiRequest, res: NextApiResponse) {


_28


// Create the Supabase Client


_28


const supabase = createServerSupabaseClient(


_28


{ req, res },


_28


{


_28


supabaseUrl: process.env.SUPABASE_URL,


_28


supabaseKey: process.env.SUPABASE_ANON_KEY,


_28


}


_28


)


_28


_28


// Start sign in with one-time password


_28


const { error } = await supabase.auth.signInWithOtp({


_28


email: 'foo@example.com',


_28


options: {


_28


emailRedirectTo: 'http://localhost:3000/api/auth/callback',


_28


},


_28


})


_28


_28


if (error) {


_28


res.json(JSON.stringify(error))


_28


}


_28


_28


res.redirect('/')


_28


}


`


Now we can set up the callback API endpoint:


`
_24


// api/auth/callback


_24


import { NextApiRequest, NextApiResponse } from 'next'


_24


import { createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'


_24


_24


_24


// Create authenticated Supabase Client


_24


const supabase = createServerSupabaseClient(


_24


{ req, res },


_24


{


_24


supabaseUrl: SUPABASE_URL,


_24


supabaseKey: SUPABASE_ANON_KEY,


_24


}


_24


)


_24


// check for code in url querystring


_24


const code = req.query.code


_24


_24


if (typeof code === 'string') {


_24


// exchange the auth code for user session


_24


await supabase.auth.exchangeCodeForSession(code)


_24


}


_24


_24


// redirect the user to a server-side protected area in your app


_24


res.redirect('/')


_24


}


`


### Roll out#


Since this is a major update that touches many of the authentication routes, we will roll it out gradually over the next few weeks. You will receive a notification in your dashboard when the feature is available for your project. Reach out to us if you want early access to this feature.


**Update** : Server-Side Auth (PKCE) is now available on all projects. Please refer to our[Server Side Auth Guide](https://supabase.com/docs/guides/auth/server-side-rendering) for further details on how to add PKCE to your project.


## Native Apple login on iOS#


While PKCE support is great, that is not the only news for you mobile app developers out there.


Building apps for iOS requires1 support for native *Sign in with Apple* . We heard the community's requests for native sign-in. We hope you join our excitement to officially announce support for native *Sign in with Apple* .


Your app's iOS Bundle ID can now be configured in the Apple provider section of your project's dashboard.


This is the only prerequisite for triggering a native *Sign in with Apple.* With[supabase-flutter](https://pub.dev/packages/supabase_flutter#native-sign-in-with-apple-example) , this is as easy as:


`
_10


final AuthResponse response = await supabase.auth.signInWithApple();


`


It's that easy! No need to set up deep links, no need to pass any parameters.


We're just starting with Apple login, and soon add support for Google login.


## Wrapping Up#


Supabase Auth aims to continue developing auth features that are secure by default yet simple to implement. We use Supabase Auth for our hosted platform and continuously dogfood the latest version on it. If you are interested to migrate to Supabase Auth, you can check out this blog post on how[Parqet migrated 125,000 users from Auth0 to Supabase Auth](https://kevcodez.medium.com/migrating-125-000-users-from-auth0-to-supabase-81c0568de307) .


## Footnotes#


1.


[App store review guidelines](https://developer.apple.com/app-store/review/guidelines/#sign-in-with-apple)↩
