---
schema_version: "1.0.0"
document_id: "57a79c21b4d0f1a4b02619a4e20ce55e7a0f54b6cf28506145dd32f29e3eaffa"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/from-static-to-dynamic-db-credentials"
published_at: "2024-04-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:05625b7e83e605531faad389aa101a6785fca55ecbe99b64cc5d2c871f7a0221"
---

# Beware of Your Static Database Credentials

Truth be told, every static database credential, and more broadly authentication token, is a secret waiting to be leaked and exploited by a bad actor. Be it one or ten years from now, the credential is bound to get leaked through one of many avenues and there's plenty to back that up. From[Mercedes-Benz](https://www.securityweek.com/leaked-github-token-exposed-mercedes-source-code/) to[AstraZeneca](https://techcrunch.com/2022/11/03/astrazeneca-passwords-exposed-patient-data/) , it almost seems that no company is immune to credential leaking. That said, there may be a solution after all, one that is common enough to be named but clearly not widespread enough to be employed everywhere.


In this article, we dive into everything wrong with static database credentials and discuss how we can improve to mitigate any risks associated with leaking them.


## What’s wrong with static database credentials?


Most users and applications in the wild authenticate with databases using manually-provisioned, static database credentials. This approach is often problematic for a few reasons:


- If you happen to leak a static database credential, then you give an attacker an indefinite window of opportunity to exploit your database. Moreover since your applications may share database credentials, they may collectively increase the overall risk of leaking secrets throughout your development cycle.
- In the lucky event that you discover the incident, then you may try to manually revoke the database credential as soon as possible. The revocation process, however, may take time and hence introduce yet another shorter window of opportunity for an attacker to exploit your database.
- In any case, you will want to audit the incident to check the details of how it transpired. Depending on whether or not parts of your infrastructure share database credentials, however, you may find it difficult to map a good audit trail.


In a world where one database credential leak could mean an attacker gaining access to a treasure trove of data, it’s clear that we need better practices.


## Introducing the Moving Target Defense


As the Department of Homeland Security states, many systems have been “built to operate in a relatively static configuration. For example, addresses, names, software stacks, networks, and various configuration parameters.” Seeing associated issues, the[Moving Target Defense (MTD)](https://www.dhs.gov/science-and-technology/csd-mtd) was a concept introduced with the intent of “controlling change access multiple system dimensions to increase uncertainty and apparent complexity for attackers, reduce their window of opportunity and increase the costs of their probing and attack efforts.”


While MTD covers a broad range of systems, for the sake of our discussion, it can be seen widely applied in the context of web applications. For example, one common moving target defense mechanism is the refresh token mechanism that is commonly used in web application authentication or protocols like OAuth 2.0.


In the refresh token mechanism, an application may redeem a long-lived token (the refresh token) for a short-lived access token and use that to authenticate with a target service. By doing so, it reduces any risk associated with leaked access token exploitation because an access token would likely be expired by the time it is found anywhere by a bad actor. Put differently, the risk of exploitation reduced with an access token developed into a moving target with short time-to-live (TTL).


## How does Moving Target Defense relate to static database credentials?


When dealing with static database credentials, we can use two moving target approaches to reduce the window of opportunity that an attacker may have of exploiting database credentials: **secret rotation** and **dynamic secrets** .


- Secret rotation at interval: Secret rotation is the practice of rotating database credentials at a specific interval where rotating implies invalidating a set of database credentials in favor of newly-issued ones. For example, you may have a script or application rotate a database credential every 30 days to completely remove the risk of an attacker being able to use a leaked, old database credential. Since database (and other types) credentials often get mishandled, forgotten about, and eventually leaked somewhere like in a public repository where it is swept up and exploited by a botnet; it’s surprisingly common to run into instances of leaked and exploited long-lived credentials, cases that would’ve easily been avoided with proper secret rotation practices in place.
- Secret rotation on-demand: In cases where a secret does get leaked and you need to revoke it immediately, secret rotation can be manually triggered to immediately invalidate and issue a new set of database credentials. Ultimately, both interval-based and on-demand secret rotation are useful to ensure that database credentials are switched out periodically and immediately in the event of an incident.
- [Dynamic secrets](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview) : Lastly, dynamic secrets is the practice of issuing each engineer or application that needs access to database credentials a unique, short-lived credential on-demand that is when requested. This removes the accumulated risk associated with shared database credentials, isolates each party’s access, and most importantly retains the moving target principle for database credentials. In a dynamic secrets scenario, an application requests access to a database from another application (we’ll get to this in a bit) after which it is granted a short-lived access token to do so; the scenario maintains that the application’s database access can be cut off independently and audited if needed.


### How are secret rotation and dynamic secrets implemented in practice?


As you may have guessed, implementing secret rotation and dynamic secrets is not trivial and requires introducing a third-party script or application to orchestrate the mechanisms. The third-party application would be responsible for updating the values of your database credentials as in the secret rotation case and/or issuing database credentials downstream to your applications that request them.


While it’s possible to implement your own secret rotation and dynamic secrets functionality, it’s strongly encouraged to instead use a secrets manager. Using purpose-built tooling ensures that you reap all the benefits of good secrets hygiene including comprehensive visibility over secrets scattered throughout your infrastructure and moving targets applied to your database credentials.


When using a tool like[Infisical](https://github.com/Infisical/infisical) , for example, you set up dynamic secrets by delegating privileged access to the platform so it can issue temporary database credentials (i.e. dynamic secrets) bound under “leases” to applications that request them; upon lease expiration, Infisical automatically revokes the dynamic secrets, so applications must request a new dynamic secret to continue accessing the intended database.


## Conclusion


Many companies fall victim to using static database credentials only to find out, one or ten years from now, that they have accidentally leaked them somewhere across their sprawled infrastructure. From small to large companies and organizations, there’re countless stories to show that this is no rare occurrence. That said, by employing Moving Target Defense principles and specifically practices like secret rotation and dynamic secrets, you can improve your organization’s overall security posture and maximally reduce the window of opportunity that a bad actor may have of exploiting database credentials in the event of a leak.


Onward and upward!
