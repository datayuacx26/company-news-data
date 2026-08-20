---
schema_version: "1.0.0"
document_id: "4516610db9d49e0822bb5a7edbe3e120477f054402d71c4e7d16d4d2627795e0"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/x-twitter-oauth-2-provider"
published_at: "2026-02-06T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:21:29.279891+00:00"
content_hash: "sha256:ec2c6af40111d742d1836e1304f3ad723408d6f7fe9cd219f5c3bb1d6419517f"
---

# X / Twitter OAuth 2.0 is now available for Supabase Auth

You can now add "Sign in with X" to your application using the new **X / Twitter (OAuth 2.0)** provider in Supabase Auth.


## What's new#


The new provider uses X's OAuth 2.0 implementation, replacing the legacy OAuth 1.0a flow. OAuth 2.0 offers a more modern authentication experience with better security practices, including PKCE support.


## Getting started#


Setting up X / Twitter authentication takes a few steps:


1. Create an OAuth 2.0 app in the[X Developer Portal](https://developer.x.com/en/portal/dashboard)
2. Enable "Request email from users" in your app's authentication settings
3. Add your callback URL from the Supabase dashboard
4. Copy your **Client ID** and **Client Secret** from the "Keys and tokens" section
5. Enter these credentials in **Authentication > Providers > X / Twitter (OAuth 2.0)** in the Supabase dashboard


Once configured, you can use the Supabase client libraries to authenticate users via X / Twitter:


`
_10


const { data, error } = await supabase.auth.signInWithOAuth({


_10


provider: 'x',


_10


})


`


For a complete guide on setting up X / Twitter authentication, see the[full documentation](https://supabase.com/docs/guides/auth/social-login/auth-twitter) .


## Migration note#


If you're currently using the legacy **Twitter (OAuth 1.0a)** provider, we recommend migrating to the new OAuth 2.0 provider. The legacy provider will remain available while X / Twitter continues to support OAuth 1.0a.


## Resources#


- [X / Twitter Auth documentation](https://supabase.com/docs/guides/auth/social-login/auth-twitter)
- [Social Login overview](https://supabase.com/docs/guides/auth/social-login)
