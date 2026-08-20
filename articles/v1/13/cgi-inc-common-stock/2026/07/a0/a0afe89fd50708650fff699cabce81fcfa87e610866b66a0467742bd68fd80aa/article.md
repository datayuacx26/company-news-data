---
schema_version: "1.0.0"
document_id: "a0afe89fd50708650fff699cabce81fcfa87e610866b66a0467742bd68fd80aa"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/cybersecurity/best-practices-for-building-strong-mobile-biometric-authentication"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:e747abb4c10a292ce9628a8a7720a444cd2db05e61f4f2bb48cc9b557879bf3f"
---

# Best practices for building strong mobile biometric authentication

Strong authentication of mobile users is an increasingly important issue for enterprises and commercial entities. While it is likely impossible to have perfect authentication security, taking care while engineering an authentication scheme can result in lowering risk to an acceptable level. It is also important to balance security concerns with user convenience so users are willing to use a strong system.


**Use 3-factor (or more) authentication**
Strong authentication cannot be guaranteed with just a single factor, especially since users hate passwords complex enough to be considered strong. Strong authentication starts with upgrading to 2-factor authentication (“something you have”), but even that is not enough for the mobile environment since the “something you have” is usually the phone itself. For example, if your second factor is an SMS message sent to a phone and an attacker gains possession of your phone, he has gained control of that second factor. For the strongest authentication, consider adding a third factor: biometrics.


**Don’t trust the phone any more than you have to**
Mobile phones are under constant attack from nefarious actors. It’s easy to assume that your users’ phones are secure, but this is likely a bad assumption. If an attacker compromises a user’s phone, it is entirely possible that the entire operating system (OS) and application stack are at risk, including locally stored files and configuration data. If you perform your biometric comparisons on the phone, the handshake between phone and server could easily be spoofed, so look for solutions that include a mechanism to prove that the biometric comparison actually has been made.


**Encryption everywhere (especially on the phone)**
Encryption of data is critical for security: both data in transit and data at rest. Cases where data should be encrypted include the communications channel (use SSL/TLS), storage on the server, and storage on the phone. Configuration data on the phone should be encrypted as well, or at the very least hashed for checking authenticity. Do not rely on in-house cryptographic technology; instead use off-the-shelf products and hire experts as needed to help you choose protocols, ciphers and an overall strategy.


**Pick a biometric that is difficult to spoof**
Some biometrics are easier to spoof than others. Relying on a biometric that is easily spoofed is a recipe for an embarrassing disaster. Choose a biometric modality that is spoof-resistant by nature.


**Design for liveness detection**
Liveness detection, also known as anti-spoofing, is critical to secure operation of an unattended biometric system. Be sure to talk with your vendors about how they prevent spoofing. Consider holding a “red team” exercise where technical experts try to compromise your authentication by spoofing the biometric.


**Consider how you will prove identity before enrollment**
This is often the forgotten step in building an authentication system. You must ensure that your system only enrolls authorized users or all of the other security precautions are for naught. Out-of-band messages (such as SMS) would normally be a reasonable way to do this, except that, for mobile users, the device used to receive these messages (the phone) is the same one that is doing the enrollment. This means that an attacker who steals a user’s phone will also receive the out-of-band message, thus defeating the purpose entirely.


In summary, designing an application for mobile users that offers robust security requires strong authentication, and passwords simply are not sufficient unless they are so long and complex as to be difficult for users to use. Two- or three-factor authentication is the way to go, coupling “something you know” with “something you have” and “something you are.” When designing systems like these, you must consider the right biometric, based on the accuracy of the biometric, the vendor’s approach to defeating spoofing attacks, and users’ perceptions of the biometric.


Learn how CGI[identity and access management services](http://www.cgi.com/en/information-security/identity-and-access-management) help our clients ensure that only authorized users gain access to their organizational information.
