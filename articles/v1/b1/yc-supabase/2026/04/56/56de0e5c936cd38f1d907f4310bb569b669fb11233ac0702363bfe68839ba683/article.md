---
schema_version: "1.0.0"
document_id: "56de0e5c936cd38f1d907f4310bb569b669fb11233ac0702363bfe68839ba683"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/custom-oauth-oidc-providers"
published_at: "2026-04-08T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:b66dfbab546c071d2b3da59602821a24d085183b25771519a489366b8c262bfd"
---

# Custom OIDC Providers for Supabase Auth

Supabase Auth ships with over 20 built-in social providers: Google, GitHub, Apple, and more. But what about your company's SAML-to-OIDC bridge? A regional identity provider required for compliance? Your self-hosted GitHub Enterprise? Until now, if your identity provider wasn't on the built-in list, you were stuck.


Today we're launching **Custom OIDC Providers** , letting you connect any standards-compliant OpenID Connect identity provider to your Supabase project. Once configured, custom providers work just like built-in ones: same sign-in flow, same client libraries, same RLS enforcement.


## Why We Built This#


Built-in providers cover the most common cases, but not every identity provider fits neatly into that list:


- **Regional compliance** : Government or industry-specific providers mandated in certain regions.
- **Self-hosted providers** : GitHub Enterprise Server, GitLab self-managed, or any on-premise OAuth2 service.
- **Niche providers** : Gaming platforms, healthcare identity networks, or industry-specific SSO systems.


Last year, we shipped[OAuth 2.1 server capabilities](https://supabase.com/blog/oauth2-provider) so your Supabase project can *be* an identity provider. Custom providers complete the picture from the other direction, now your project can also *consume* any external identity provider.


## Adding a Custom Provider#


Supply your provider's issuer URL, client credentials, and scopes. Supabase handles the rest. The discovery document, endpoints, and JWKS are resolved automatically from` {issuer}/.well-known/openid-configuration` .


`
_10


const { data, error } = await supabase.auth.admin.customProviders.createProvider({


_10


provider_type: 'oidc',


_10


identifier: 'custom:my-provider',


_10


name: 'My Provider',


_10


client_id: 'your-client-id',


_10


client_secret: 'your-client-secret',


_10


issuer: 'https://auth.example.com',


_10


scopes: \['openid', 'profile', 'email'\],


_10


})


`


The` openid` scope is always included automatically, and ID tokens are verified against the provider's JWKS.


You can also create providers from the Supabase Dashboard: go to **Authentication > Sign In / Providers** , scroll to the **Custom Providers** section at the bottom, and click **New Provider** .


## User Sign-In#


Once a custom provider is created and enabled, your users sign in through the same flow they'd use with any built-in provider. No special handling needed on the client side.


`
_10


// JavaScript


_10


const { data, error } = await supabase.auth.signInWithOAuth({


_10


provider: 'custom:my-provider',


_10


})


`


`
_10


// Flutter


_10


await supabase.auth.signInWithOAuth(


_10


OAuthProvider('custom:my-provider'),


_10


);


`


`
_10


// Swift


_10


try await supabase.auth.signInWithOAuth(


_10


provider: "custom:my-provider",


_10


redirectTo: URL(string: "my-custom-scheme://my-app-host")


_10


)


`


Behind the scenes, the auth server handles the full OAuth flow: redirecting to the external provider, exchanging the authorization code for tokens, fetching user profile data, and creating or linking the user in your Supabase project.


## Key Features#


### PKCE by Default#


Every custom provider has[PKCE](https://datatracker.ietf.org/doc/html/rfc7636) (Proof Key for Code Exchange) enabled by default. The auth server generates the code challenge and verifier automatically during the authorization flow, no client-side PKCE logic required. This protects against authorization code interception attacks out of the box if supported by the provider.


### Authorization Params#


Append extra query parameters to the provider's authorization URL. Useful for requesting consent screens, offline access, or login hints:


`
_10


{


_10


"prompt": "consent",


_10


"access_type": "offline",


_10


"login_hint": "user@example.com"


_10


}


`


### Multi-Platform Apps#


If your app uses different client IDs per platform (web, iOS, Android), use` acceptable_client_ids` to list additional client IDs accepted for audience validation in OIDC ID tokens:


`
_10


_10


provider_type: 'oidc',


_10


identifier: 'custom:multi-platform-app',


_10


name: 'Multi-Platform App',


_10


client_id: 'web-client-id',


_10


client_secret: 'your-client-secret',


_10


issuer: 'https://app.example.com',


_10


scopes: \['openid', 'profile', 'email'\],


_10


acceptable_client_ids: \['ios-client-id', 'android-client-id'\],


_10


})


`


### Email-Optional Providers#


Not every identity provider returns an email address. Set` email_optional: true` to allow sign-in without one: useful for gaming platforms, device-based identity, or providers that use phone numbers as the primary identifier.


## Managing Providers#


Custom providers are fully manageable through both the Dashboard and the auth Admin API.


- **List** all custom providers
- **Update** any field except` provider_type` and` identifier` : rotate secrets, change scopes, toggle enabled state
- **Delete** providers you no longer need


`
_15


// List all custom providers


_15


const { data, error } = await supabase.auth.admin.customProviders.listProviders()


_15


_15


// Update a provider


_15


const { data, error } = await supabase.auth.admin.customProviders.updateProvider(


_15


'custom:my-provider',


_15


{


_15


name: 'Updated Provider Name',


_15


scopes: \['profile', 'email', 'groups'\],


_15


}


_15


)


_15


_15


// Delete a provider


_15


const { data, error } =


_15


await supabase.auth.admin.customProviders.deleteProvider('custom:my-provider')


`


You can add up to 3 custom providers per project. If you need more,[contact support](https://supabase.com/dashboard/support/new) .


## Getting Started#


1. Go to[Authentication > Sign In / Providers](https://supabase.com/dashboard/project/_/auth/providers) in the Dashboard and scroll to the **Custom Providers** section
2. Click **New Provider** and choose your configuration method
3. Enter your identity provider's credentials and endpoints
4. Copy the **Callback URL** and configure it in your external IdP
5. Call` signInWithOAuth` with your` custom:` provider identifier


For the full API reference and detailed configuration options, check out the[Custom OAuth/OIDC Providers documentation](https://supabase.com/docs/guides/auth/custom-oauth-providers) .
