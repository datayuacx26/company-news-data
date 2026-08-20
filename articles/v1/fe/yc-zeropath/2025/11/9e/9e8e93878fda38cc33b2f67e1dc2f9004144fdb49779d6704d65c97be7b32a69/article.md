---
schema_version: "1.0.0"
document_id: "9e8e93878fda38cc33b2f67e1dc2f9004144fdb49779d6704d65c97be7b32a69"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/django-allauth-account-takeover-vulnerabilities"
published_at: "2025-11-05T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:093d3fe18c8aa775f4b1cab3029e24bf1a5de038c9ec9d3da717ccc66a9bc959"
---

# 7 vulnerabilities in django-allauth enabling account impersonation and token abuse

# 7 vulnerabilities in django-allauth enabling account impersonation and token abuse


*TL;DR* – We audited` django-allauth` and found four major vulnerabilities, two of which enable user impersonation, plus several minor issues. All are fixed in 65.13.0.[Upgrade now](https://allauth.org/news/2025/10/django-allauth-65.13.0-released/) .


---


## Introduction


` allauth/django-allauth` is an extremely popular modular authentication suite for Django, that handles identity and authentication, email verification, and account management, including "social login" via a ton of third-party providers. With over[10,000](https://github.com/pennersr/django-allauth) on GitHub and nearly[2,000,000](https://pypistats.org/packages/django-allauth) monthly downloads on PyPI, it's the one-stop-shop for local auth and social auth (with OAuth/OIDC and SAML providers). With such a broad range of support for so many authentication providers, we thought we'd perform some tests with our scanner.


We found two account impersonation vulnerabilities, as well as some other issues. They’re detailed below.


## Vulnerabilities


### Major Vulnerabilities


Four major vulnerabilities were discovered in this codebase:


- Okta identifiers were mutable.
- NetIQ identifiers were mutable.
- Tokens for deactivated users could be refreshed indefinitely.
- Notion emails are marked as verified.


### Minor Vulnerabilities


- Recovery codes could be cached by reverse proxies (or browsers).
- Insecure HTTP URLs used for OAuth endpoints.
- URI-quoting relied on incorrect Python ordering.


## Breakdown


### Major Vulnerability Details


The most interesting vulnerabilities discovered in` django-allauth` happened to be the most critical, too. Instead of going into a long spiel about each of the issues, I'm just going to list exactly what our scanner discovered and told us.


#### Mutable/non-unique provider-attribute used as UID from Okta and NetIQ


> The function uses the OAuth response field preferred_username as the account UID. Many OAuth/OIDC providers expose both a stable` sub` (subject) claim and optional, user-facing fields like` preferred_username` . preferred_username is often mutable and/or not globally unique. Using it as the canonical provider UID can allow account takeover or accidental account linking: an attacker can change their preferred_username to match another social-account's stored UID (or exploit case differences), and may authenticate as the other user or cause cross-account mapping.


Basically,` preferred_username` is not a unique value for an account from the OIDC identity providers Okta and NetIQ. This means it could be possible to impersonate another user on a Django application using` django-allauth` , by changing your "preferred_username" on Okta or NetIQ to that of another user, and the authentication mechanism would blindly trust it. Want to authenticate as the unique user` zezima` on the application? Just change your` preferred_username` in Okta or NetIQ to` zezima` !


This was fixed[here](https://codeberg.org/allauth/django-allauth/commit/8feef46e0e07b25fc5594c8f268afa247ebc3412) .


#### Token issued to disabled user / no account-state check before issuing tokens


> After resolving the user object from device authorization data, the code assigns` orequest.user` and allows` OAuthlib` to mint tokens without explicitly verifying that the user is still permitted to receive tokens (for example,` user.is_active` ). If the account was disabled after the user authorized the device, this handler may still result in tokens being issued for that account unless downstream validators enforce the check. The handler should explicitly validate required account state and return an appropriate OAuth2 error if the account is ineligible.


Basically, a flag for a user,` is_active` , was supposed to stop authentication (and re-authentication!) as the identity provider. But, it simply didn't. This was fixed[here](https://codeberg.org/allauth/django-allauth/commit/c54edf947c5a1c8c4ff3cddb75c86000ecb2507d) and[here](https://codeberg.org/allauth/django-allauth/commit/39f4a4ce9c891795b00914ca5ec32de72d5369c0) .


#### Notion: emails marked as verified (but they aren't)


> The Notion provider handler (extract_email_addresses) unconditionally sets verified=True and primary=True for the email extracted from the provider payload. The code does not check any provider-supplied 'email_verified' flag or otherwise validate that Notion verified the email address. As a result, a single OAuth response can cause the application to accept an email as verified and primary, enabling account linking or account creation that bypasses the application's normal email verification protections.


Pretty much what it says on the box: Notion passes a user's email address to` django-allauth` , but this address is not documented to be verified. In other words, you could create an account on Notion with anybody's email address without verifying, and then use Notion to login to the application. This could be pretty serious if you've got some website that makes all users with a verified email of` @your-website.com` an admin.


### Minor Vulnerability Details


In addition to the above issues, some smaller issues were also discovered.


#### Recovery codes download view could be cached


When retrieving the recovery codes for an account, the download page may be cached by a proxy (or browser) due to missing` Cache-Control` /` Pragma` headers.


#### Insecure HTTP URLs used for OAuth endpoints.


In various OAuth provider configurations (and other functionality), insecure HTTP URLs were being used, for example:


- [http://www.amazon.com/ap/oa](http://www.amazon.com/ap/oa)
- [http://www.baidu.com/p/](http://www.baidu.com/p/)
- [http://tb.himg.baidu.com/sys/portraitn/item/](http://tb.himg.baidu.com/sys/portraitn/item/)
- [http://www.flickr.com/services/OAuth/authorize](http://www.flickr.com/services/OAuth/authorize)
- [http://instagram.com](http://instagram.com/)
- [http://api.tumblr.com/v2/user/info](http://api.tumblr.com/v2/user/info)
- [http://x.com/](http://x.com/)
- [http://vimeo.com/api/rest/v2?method=vimeo.people.getInfo](http://vimeo.com/api/rest/v2?method=vimeo.people.getInfo)
- [http://www.weibo.com](http://www.weibo.com/)
- [http://me.yahoo.com](http://me.yahoo.com/)


#### URI-quoting relied on incorrect Python ordering


This one is a fun little bug that probably doesn't *really* have any security implications, but I wanted to include it because it's something that a human reviewer wouldn't really pick up. Previously, the code did this:


```text
# Django considers "safe" some characters that aren't so for OAuthlib.
# We have to search for them and properly escape.
unsafe   =     set  (  c   for   c   in   parsed  [  4  ]  )  .  difference  (  urlencoded  )
for   c   in   unsafe  :
parsed  [  4  ]     =   parsed  [  4  ]  .  replace  (  c  ,   quote  (  c  ,   safe  =  b""  )
```


The problem with this code, as our scanner picked up, is that in Python, a` set()` is not ordered, so the replacing of characters which need encoding could occur differently each time. Why does this matter? Well, consider the following URI:


```text
/foo?q=a|b


```


If the URI is normalized with the unsafe characters` |` and` %` starting with` |` , the URI will become:


```text
/foo?q=a%7Cb


```


Note the` %` . On the second pass normalizing for` %` , the URI becomes:


```text
/foo?q=a%257Cb


```


The URI has effectively been double-encoded. Decoding turns this back into` /foo?q=a%7Cb` , but that's not the original URI we started with. This double encoding only happens when the encoding happens in that specific order.


You'd typically want to encode the` %` character first to avoid exactly the problem of double-encoding, however since Python sets are not ordered, this was not guaranteed at all.


## Final Notes


Traditional technical vulnerabilities often have a particular precondition that is closely associated with a bug (for example, user input passing into an os.system() call). For those narrow cases it's possible to find bugs via pattern matching. But actually pentesting apps means exposing behavior that drifts outside intended design constraints, even if that design is implicit. Static analyzers cannot infer a system level spec; however, LLMs can approximate one by reading how concepts are used and then searching for violations, much like a human code reviewer. In our identity audit, our LLM-based SAST inferred rules such as tokens require ActiveUser and social accounts must key on a stable provider sub, then flagged paths that issued tokens to inactive users or trusted mutable usernames.


Thus, the vulnerabilities detailed in this post are effectively differences between **intent** versus **implementation** . Our AI SAST excels at exactly that, asking questions like:


- "Deactivating a user should lock them out – does *every* token path do that?"
- Are the OIDC identifiers used for authentication immutable? They should always be.
- Are the correct assumptions being made about in-built data types for the programming language.
- What did the developer *want* to do when they wrote this code? What did they *actually* do?


Expect the next wave of high impact vulnerabilities in popular codebases to come from edgecases that originate from differences in design and implementation. LLM SASTs will continue to find these types of vulnerabilities as they are utilized against more codebases, pushing far simple programming mistakes, by matching code, configurations, and even documentation, against intended, secure reality of functionality.
