---
schema_version: "1.0.0"
document_id: "09f8a95c24998db6211baad17a393bbe63b8265eda6a2e86852f356c61a95356"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-news-import-815987d58dd2"
canonical_url: "https://product.hubspot.com/blog/passkeys-the-future-of-login-at-hubspot"
published_at: "2024-12-03T00:00:00+00:00"
first_seen_at: "2026-07-21T23:16:14.524102+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:550ae117a203b1c78dbf7a76eff067189a68e5424a14b03d239a1e409e6acbb9"
---

# Passkeys: The Future of Login at HubSpot

Logging in to a website, app or service is something we rarely think about. We’ve been trained to provide a username and password since the early days of the internet. It’s second nature to us, like riding a bike. Yet, this mindless task is one of the biggest risks we face when protecting our accounts.


There have been advancements and additional protections made to try and limit those risks, things like 2-Factor Authentication, sign-in with social services (e.g. Google, Microsoft, Apple), and Single Sign-On, but most of them are still backed up by a password.


It’s time for HubSpot to look to the future. A future where you don’t have to remember 12 characters with a special character, a capital letter, and a number. It’s time for a future without passwords. One that uses your device's built-in biometric capabilities. A future that’s faster, easier and more secure. It’s time for passkeys. Now available in Beta!


**What’s wrong with a password?**


On the surface, passwords seem like a secure protection method, but there are a number of challenges and risks that exist:


1. It takes a high cognitive load to remember something with complex requirements.[The average person has 168 accounts with passwords](https://nordpass.com/blog/how-many-passwords-does-average-person-have/)
2. That high cognitive load increases the likelihood of re-use across websites and services.[More than 6 in 10 admit to reusing passwords](https://www.lastpass.com/-/media/3c627ed089e84bc39ca2bf6bf1d7cdec.pdf) . A password manager can help with this, but they are not used by everyone.
3. It comes with high user friction when a user forgets their password, needs to reset it, or needs to constantly input 2FA codes.
4. A password is phishable ( using social engineering techniques to trick users into accessing a fake website and divulging personal information)


, arguably the biggest risk of the password. As[reported](https://www.verizon.com/business/resources/reports/dbir/) in the annual Verizon Data Breach Investigation Report, compromised credentials are the number one cause of data breaches.


**Enter Passkeys**


In the movie “ *Back to the Future”,* Doc Brown famously says “Roads, where we’re going we don’t need roads”. Passwords are like the roads we used to travel on, and passkeys are the future, where passwords are no longer needed.


Passkeys came about in 2022 when the FIDO Alliance[announced](https://fidoalliance.org/apple-google-and-microsoft-commit-to-expanded-support-for-fido-standard-to-accelerate-availability-of-passwordless-sign-ins/) an updated authentication standard that would work across the major platform providers (Microsoft, Apple, Google) that leverage device biometrics or PIN codes. To understand a bit more about how passkeys work:


*For a less technical explainer:*


Imagine a passkey like a bank safety deposit box. The box has two physical keys that are needed to open it. The bank has a key, and you have a key. To open the box, you need to verify who you are (usually your ID) and present your physical key to match up with the bank’s key. One key alone will not open the box.


*For the more technical explainer:*


A passkey works by using public key cryptography. During passkey creation, a key pair is created. The public key is stored with the website or service, and the private key is stored and protected on your device. When logging in with a passkey, a random cryptographic challenge is presented that can only be completed by the corresponding private key. To complete the challenge, you will be prompted by your device to authenticate yourself using biometrics, PIN, or another verification (e.g. from your password manager) and then the challenge gets verified by the public key. Once the challenge is successfully completed, you’ll be authenticated and logged in.


The other unique characteristic of passkeys is they sync across your devices. For example, if you set up a passkey on your Mac laptop, it syncs across your iCloud Keychain so that it will work with your iPhone. The same goes for your Google account. For those that have cross-platform needs (e.g. Android phone with a Mac Laptop or an iPhone with a PC), a password manager is a great option to store your passkeys for cross-platform compatibility. This syncing characteristic greatly reduces lockout risks and has native built-in recovery mechanisms within your existing ecosystems.


Passkeys address the risks of passwords with its unique characteristics:


**Risks**


Passwords


Passkeys


Have to remember it


Yes ❌


No ✅


Can be reused across multiple websites


Yes ❌


No ✅


Needs a separate 2FA action to protect


Yes ❌


No ✅


Phishing resistant


No ❌


Yes ✅


**Security meets convenience**


Not only are passkeys more secure, they also make account sign-ups and login faster, all while reducing support overhead for forgotten passwords, resets, and 2FA lockouts. Don’t just take my word for it - thanks to some other organizations who have implemented passkeys and published their results:


1. [Intuit - 15% Login Success Rate Improvement, 70% faster sign in](https://fidoalliance.org/case-study-intuits-roi-from-passwordless-customer-authentication/)
2. [Yahoo Japan - 25% decrease in forgotten passwords, 2.6X faster authentication time](https://web.dev/case-studies/yahoo-japan-passkeys)
3. [Kayak - 50% faster sign in](https://developers.googleblog.com/en/how-kayak-reduced-sign-in-time-by-50-and-improved-security-with-passkeys/)
4. [Mercari - 3.9x faster authentication time](https://fidoalliance.org/mercaris-passkey-authentication-speeds-up-sign-in-3-9-times/)


**How to setup passkeys at HubSpot**


*Passkeys are available for HubSpot on the web, coming soon to our mobile app*


Passkey setup is fast and easy:


- Login to your HubSpot account
- Navigate to your user security settings
- Click set up a passkey


- If you don’t see this setting, it is due to your device or browser not being eligible to support passkeys


- Complete the prompt to register your passkey (native Apple, Google, or PC prompt, or your password manager)
- All set!


To login with a passkey


- You’ll see a login with a passkey button on the login screen
- Click login
- Complete the authentication (Biometrics, Face ID, etc)
- You’re logged in!


*For admins: restrict your login types for your portal*


In addition to passkeys, we have recently launched an additional admin security feature called Allowed Logins. This security setting allows you to set which login methods your users are allowed to access your portal with. You can disallow password-based logins, and enforce other login methods, including passkeys. Check out our Allowed Logins[Knowledge Base Article](https://knowledge.hubspot.com/account-security/restrict-which-log-in-methods-users-can-use-to-access-your-account?hs_preview=WiyNOafI-177591690403) to learn more.


We’re excited to offer our customers this opportunity to elevate their security posture for individuals and organizations.


For more information, check out our passkey[Knowledge Base](https://knowledge.hubspot.com/account-management/set-up-and-log-in-with-passkeys) article
