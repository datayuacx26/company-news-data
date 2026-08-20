---
schema_version: "1.0.0"
document_id: "71d2dc1aebd64ad2d8a097a7d63fec3d2ea608102378123d16cba83751706b4d"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/native-mobile-auth"
published_at: "2023-06-27T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:173c5224ee3404a03e96a6f905afd3921a13a1f3976c2f074baffaf2036119dc"
---

# Native Mobile Auth Support for Google and Apple Sign in

Supabase supports OAuth logins with 17 providers including Apple, Google, Microsoft, GitHub, … But for native mobile apps, this meant that developers had to use a web browser to sign in. It’s not an ideal flow for users, who are already used to signing in with the operating system’s native dialogs when possible. Today, we are excited to announce full native support for Sign in with Apple and Google on iOS and Android. But this is not all! Supabase Auth now can now be used with one-tap sign in methods like: Sign in with Apple JS, Sign in with Google for Web or even in Chrome extensions.


## Native Sign in with Apple and Google#


Developers of native iOS and Android apps (using Flutter or React Native) can now take advantage of OS-provided authentication dialogs for Apple and Google. This is available on iOS, macOS, tvOS and watchOS apps in the Apple ecosystem, and all Android variants in the Google ecosystem.


In full transparency, this was always sort-of possible but there were some edge cases that were not covered well with Supabase Auth. We’ve since ironed out the developer experience and made this into a fully supported feature.


Behind the scenes, these native sign in methods use ID tokens. They’re a formalized version of a JWT that is issued by Apple or Google and contain profile information. Supabase Auth now can properly validate the ID tokens and create new or link to existing user accounts based on email similarity.


- Sign in with Apple \[[Web](https://supabase.com/docs/guides/auth/social-login/auth-apple?platform=web) |[React Native](https://supabase.com/docs/guides/auth/social-login/auth-apple?platform=react-native) |[Flutter](https://supabase.com/docs/guides/auth/social-login/auth-apple?platform=flutter) |[Swift](https://supabase.com/docs/guides/auth/social-login/auth-apple?platform=swift) |[Kotlin](https://supabase.com/docs/guides/auth/social-login/auth-apple?platform=kotlin) \]
- Sign in with Google \[[Web](https://supabase.com/docs/guides/auth/social-login/auth-google?platform=web) |[React Native](https://supabase.com/docs/guides/auth/social-login/auth-google?platform=react-native) |[Flutter](https://supabase.com/docs/guides/auth/social-login/auth-google?platform=flutter) |[Android](https://supabase.com/docs/guides/auth/social-login/auth-google?platform=android) |[Chrome Extensions](https://supabase.com/docs/guides/auth/social-login/auth-google?platform=chrome-extensions) \]


## Sign in with Apple JS, Google One Tap and Chrome Extensions#


Although sign in on native platforms was the focus of the team when working on this feature, incidentally we’ve added proper support for Sign in with Apple JS, Google’s One Tap and support for authenticating within Google Chrome extensions.


You can now take advantage of these web frameworks, most notably Google’s One Tap and Automatic Sign-in support for a frictionless onboarding experience for your users.


All you need to do is configure the web frameworks and use the` signInWithIdToken()` method to pass the ID token provided by the Google and Apple libraries.


For example, to use Google One tap you should first[generate an embed code for the Google Sign in Button](https://developers.google.com/identity/gsi/web/tools/configurator) . Register this method as the callback that will receive the authentication response from the button:


`
_10


async function handleSignInWithGoogle(response) {


_10


const { data, error } = await supabase.auth.signInWithIdToken({


_10


token: response.credential,


_10


nonce: 'NONCE', // must be the same one as provided in data-nonce (if any)


_10


})


_10


}


`


## Resources#


- [Login with Apple Guide](https://supabase.com/docs/guides/auth/social-login/auth-apple)
- [Login with Google Guide](https://supabase.com/docs/guides/auth/social-login/auth-google)
