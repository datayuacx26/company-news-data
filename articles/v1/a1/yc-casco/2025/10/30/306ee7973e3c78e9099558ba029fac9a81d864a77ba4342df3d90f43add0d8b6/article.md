---
schema_version: "1.0.0"
document_id: "306ee7973e3c78e9099558ba029fac9a81d864a77ba4342df3d90f43add0d8b6"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/the-importance-of-rate-limiting"
published_at: "2025-10-13T20:26:00+00:00"
first_seen_at: "2026-07-21T12:40:29.812234+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:a014050e32684836c2b62c9ea256790addf7e684a783ec46c9ebfbee7c53cc97"
---

# Why You Need Account-Based Rate Limits

# Why You Need Account-Based Rate Limits


Written by


[Lucas Romano](https://www.linkedin.com/in/22733185981325918762283657061/)


on


Mon Oct 13 2025


Rate limiting traffic is an age-old practice to stop bad requests from malicious actors. It’s usually applied on functions like login, sign-up and forgot password. The practice of rate limiting is a form of defense against different attack types with the most common being account takeovers. Like many aspects of application security one can be as granular with it as they choose, and use a simplistic approach or an extremely complex multi-faceted one. This article focuses on the rate limiting at the application-level. The goal here is to discuss a balanced approach any application developer can take.


## Why Rate Limit


The most simple answer is to stop bad requests from interacting with your application. Many types of attacks don't need many requests to work, things like command injections or server-side request forgeries only require one bad request to get through for the exploit to cause havoc. On the inverse very trivial attacks like account takeovers that don't require extensive application security knowledge can be among the most devestating. Large-scale botnets can simply hammer login endpoints to breach accounts over time. While this is a difficult foe, the positive here is that attacks like this don't work without a large volume of network traffic operating against a target. Regular[penetration testing](https://casco.com/supervised) can help identify missing or poorly configured rate limits before attackers exploit them.


## Attack Chain Example


Defense-in-depth is a term that applies well to how developers need to think when applying a defensive measure like rate limiting. There isn’t a single approach that’s the “end all be all” of stopping an attack like account takeovers, it takes multiple layers. Before diving into practical approaches you can take to implement rate limiting let’s walk through a scenario.


Imagine there’s a bank that has a simple login page containing only a login dropdown and password reset dropdown. The security measures they use are:


- Mandatory 2FA for all account logins
- IP rate limiting
- Cryptographically secure unique link for forgotten password flow


Superficially this seems secure, even if an attacker can get a user’s password, they won’t have their 2FA code. On top of that, they’ll have to change IP addresses frequently which will cause delays in the attack.


Let’s walk through how an attack on this login page could look:


**Step 1: Reconnaissance** - A threat actor performs OSINT and finds an email & password combination for a high net worth individual on a breach forum


**↓**


**Step 2: Infrastructure** - The threat actor has access to a large scale botnet consisting of over 1M unique IPv4 addresses belonging to residential ISPs


**↓**


**Step 3: Initial Access** - The email & password works for the bank but the attacker is presented with a 2FA challenge now in the form of a 6-digit value, the 2FA code has a timeout of 15 minutes


**↓**


**Step 4: Brute Force Attack** - Utilizing their botnet and a basic script they send 10k 2FA codes per second with each code coming from a new IP. They successfully bruteforce the 2FA code in a few minutes.


The bank in this situation had what seemed like good defenses, IP rate limiting, mandatory 2FA, a secure link for forgotten passwords instead of a short code. However the vulnerabilities were easy to exploit, if IPs get blocked just use many, if 2FA is weak just bruteforce it. This is how attackers think and act, they try to find any hole and turn it into a disaster. While this situation seems improbable, it happens more than you think.[Oftentimes catastrophic breaches don’t come in the form of a 0-day, they occur due to a lack of limiting resources that can access an application like what was described above.](https://www.humansecurity.com/learn/blog/credential-stuffing-and-account-takeover-attacks-remain-nagging-business-problems/)


## Remediation Implementation


Let’s continue using our situation above as an example, take a moment and think “what would have stopped this attack?”. There’s more than one answer, as there tends to be with real applications. Most things aren’t cookie cutter in cybersecurity.


Focusing on the initial problem, discovered credentials, let’s dive in. This isn’t something any company can stop a threat group from gathering as users can have their passwords exposed in many ways. Developers can only prepare for ‘when’ this happens, not ‘if’.


2FA is designed to prevent problems like exposed credentials, but it didn’t here. Instead the attackers bypassed IP rate limiting to bruteforce the insecure 2FA code. What would have stopped this all? Two things.


### Account-Based Rate Limiting


IP rate limiting is the first step of defending against automated attacks, but it has many weaknesses. As more and more routers become compromised more and more IP space becomes part of maliciously controlled botnets. Modern day threat actors are able to rotate a new request coming from a new IP at even a 1:1 ratio. Take for example this credential stuffing attack that[used 91.3 million distinct IP addresses for 107.9 million login attempts](https://datadome.co/threat-research/tale-of-a-heavily-distributed-credential-stuffing-attack/) which completely defeated IP-based rate limiting. Not all threat actors can achieve this as they need access to IP space to do so, but many have that access.


The fix is rate limiting accounts (or by session for authenticated functions). It doesn’t matter how many IPs an attacker owns if they can only send 2-3 requests to the same account in an hour as bruteforcing and even forms of credential stuffing simply won’t be feasible.


With account-based rate limiting, ensure the end user has a way to bypass an account lockout so attackers cannot cause a denial of service to users. This bypass could be something like a cryptographically secure link emailed to the user that can’t be feasibly bruteforced.


### Securely Implemented 2FA Code


In the situation above the attackers got the correct 2FA code due to its excessive expiration time and low entropy. These configurations allowed the attacker to successfully bruteforce it, so how should 2FA be implemented?


There needs to first be a low expiration time, a 2FA code shouldn’t exceed 1-5 minutes depending on the application. Longer expiration times give attackers more opportunity to attack.


The next change would be using a larger and harder to guess value. A 6-digit number only has one million possibilities, whereas an 8-character alphanumeric value has trillions.


## Summary


Security is like a cut diamond, there are many facets to consider. Rate limiting is just one layer in a comprehensive security strategy—learn more about the broader landscape in our[OWASP Top 10 2025 guide](https://casco.com/blog/owasp-top-10-2025-navigating-the-new-security-landscape) . The biggest takeaway from this article should be the importance of proper rate-limiting, and the key implementations of it which are:


- IP rate limits
- Account rate limits
- Session rate limits


- This is mainly geared towards authenticated APIs to prevent one session token from being passed around multiple IPs in an attack


- Proper 2FA code entropy and short TTLs
- Account lockout bypasses via cryptographically secure email links
