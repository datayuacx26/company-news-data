---
schema_version: "1.0.0"
document_id: "edc3d51ae54133974346a3c815f7b1c285d864cff66c029e8b32ffb57c49296c"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46458-passkeys-for-supabase-auth-beta"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:b95afb4393af9cb1af707b3cc86c03d623930729053864d22390b5a7c5655db5"
---

# Passkeys for Supabase Auth (Beta)

We're excited to announce the **beta release of Passkeys** for Supabase Auth — a passwordless, phishing-resistant credential built on the[WebAuthn](https://www.w3.org/TR/webauthn-3/) standard.


With passkeys, users sign in with biometrics (Face ID, Touch ID, Windows Hello), a device PIN, or a hardware security key. Supabase Auth stores the public key needed for verification; private key material remains managed by the user’s authenticator or credential provider.


## How does it work?#


Each passkey enrollment or sign-in is a WebAuthn ceremony with three steps:


1. **Options** : the client requests a challenge from Supabase Auth.
2. **Ceremony** : the browser invokes` navigator.credentials.create()` (register) or` navigator.credentials.get()` (sign in), prompting the user to approve with biometrics or a security key.
3. **Verify** : the signed response is sent back to Supabase Auth, which validates the challenge and either stores the new credential or issues a session.


Supabase Auth uses[discoverable credentials](https://www.w3.org/TR/webauthn-3/#discoverable-credential) , so users don't need to type an email or username — the authenticator resolves the account from the credential it already stores.


## Enable passkeys in the Dashboard#


Open **Authentication → Passkeys** in the Dashboard, toggle on **Enable Passkey authentication** , and fill in your[WebAuthn relying party](https://www.w3.org/TR/webauthn-3/#relying-party) details:


- **Relying Party Display Name** : human-readable name shown during the passkey prompt (e.g. "My App").
- **Relying Party ID** : your bare domain (e.g.` example.com` ). No scheme, port, or path.
- **Relying Party Origins** : up to 5 allowed origins (e.g.` https://example.com,https://app.example.com` ).


The Dashboard pre-fills these from your project's Site URL and project name.


Passkeys can also be configured via the[CLI](https://supabase.com/docs/guides/auth/passkeys#cli) and the[Management API](https://supabase.com/docs/guides/auth/passkeys#management-api) .


## Use it from your app#


> \[!NOTE\] The Passkeys API is currently experimental and requires an explicit opt-in as the API may change without notice during the beta phase.


Opt in to the experimental API when creating the client:


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


const supabase = createClient(supabaseUrl, supabasePublishableKey, {


_10


auth: {


_10


experimental: { passkey: true },


_10


},


_10


})


`


**Register a passkey** for an authenticated user — typically from a security settings page or right after sign-up:


`
_10


const { data, error } = await supabase.auth.registerPasskey()


_10


// data: { id, friendly_name, created_at }


`


**Sign in with a passkey** — no email or phone needed upfront; the authenticator picks the account:


`
_10


const { data, error } = await supabase.auth.signInWithPasskey()


_10


// data.session and data.user are set; a SIGNED_IN event is dispatched


`


**Manage passkeys** — list, rename, and delete from the current user's account:


`
_10


const { data: passkeys } = await supabase.auth.passkey.list()


_10


_10


await supabase.auth.passkey.update({


_10


passkeyId: passkeys\[0\].id,


_10


friendlyName: 'Work laptop',


_10


})


_10


_10


await supabase.auth.passkey.delete({ passkeyId: passkeys\[0\].id })


`


## What we'd like to know from you#


- Any bugs or rough edges you hit during passkey registration or sign-in flows.
- Friction when configuring the relying-party settings in the Dashboard, CLI, or Management API.
- Feedback on integrating passkeys in native or mobile flows.
- Suggestions for improving the API ergonomics or documentation.


Drop your feedback in this thread or[open an issue](https://github.com/supabase/auth/issues) .


## Related links#


- Documentation:[Passkey authentication](https://supabase.com/docs/guides/auth/passkeys)
- Dashboard:[Authentication → Passkeys](https://supabase.com/dashboard/project/_/auth/passkeys)
- JavaScript reference:


- [auth.registerPasskey](https://supabase.com/docs/reference/javascript/auth-registerpasskey)
- [auth.signInWithPasskey](https://supabase.com/docs/reference/javascript/auth-signinwithpasskey)
- [auth.passkey (two-step API)](https://supabase.com/docs/reference/javascript/auth-passkey-api)
- [auth.admin.passkey (server-side)](https://supabase.com/docs/reference/javascript/auth-admin-passkey-api)
