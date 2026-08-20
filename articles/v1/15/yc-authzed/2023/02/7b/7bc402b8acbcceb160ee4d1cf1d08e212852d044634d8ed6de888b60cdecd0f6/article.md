---
schema_version: "1.0.0"
document_id: "7bc402b8acbcceb160ee4d1cf1d08e212852d044634d8ed6de888b60cdecd0f6"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/top-three-caveat-use-cases"
published_at: "2023-02-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:177a937f9d5ce0d43d439df65838cdd038598a3be85c39053a3ea1ea334a3e4b"
---

# Top-3 Most Used SpiceDB Caveat Patterns

In preparation for the general availability of[SpiceDB Caveats](https://authzed.com/blog/caveats/) , planned for the next 1.17.0 release, we thought it would be cool to share some of the recurrent use cases we’ve seen for caveats out in the wild. Caveats allow augmenting your SpiceDB schema with dynamic logic written in[Google's CEL](https://github.com/google/cel-spec) .


Here are top-3 most used caveat patterns:


## 🥉 IP Allowlists


One common use case is to implement conditional access based on the originating IP of an inbound request. Organizations that want to tighten employee access to their systems make sure they can only perform requests from a range of allowed IPs, e.g. as a CIDR block.[GitHub’s IP Allowlists](https://docs.github.com/en/enterprise-cloud@latest/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/managing-allowed-ip-addresses-for-your-organization) functionality provides a good real-world example.


SpiceDB offers first-class support for IP Allowlists via two built-in pieces of functionality: the ipaddress type and the in_cidr method. Let’s see this in action by modeling GitHub’s IP Allowlists! ([Playground Link](https://play.authzed.com/s/Xnq7c0dZ1Wpa/schema) )


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


```text
definition    user    {  }


caveat   expected_ip_range (  user_ip ipaddress, cidr string )    {
user_ip.in_cidr (  cidr )
}


definition    organization    {
relation    resources  :  repository
relation    members  :  user
/*
ip_allowlist_policy is a caveated relationship to members of the organization based on IP Allowlists, and can
be disabled by setting the relationship to user:*
*/
relation    ip_allowlist_policy  :  organization  # members     with   expected_ip_range |    user  :*


permission   policy  =   ip_allowlist_policy
}


definition    repository    {
relation    owner  :  organization     |   user
relation    reader  :  user
relation    writer  :  user
relation    adminer  :  user


permission   read  =    (  reader  +   write )    &   owner -  >policy
permission   write  =    (  writer  +   admin )    &   owner -  >policy
permission   admin  =   adminer  &   owner -  >policy
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


```text
definition    user    {  }


caveat   expected_ip_range (  user_ip ipaddress, cidr string )    {
user_ip.in_cidr (  cidr )
}


definition    organization    {
relation    resources  :  repository
relation    members  :  user
/*
be disabled by setting the relationship to user:*
*/


permission   policy  =   ip_allowlist_policy
}


definition    repository    {
relation    owner  :  organization     |   user
relation    reader  :  user
relation    writer  :  user
relation    adminer  :  user


permission   read  =    (  reader  +   write )    &   owner -  >policy
permission   write  =    (  writer  +   admin )    &   owner -  >policy
permission   admin  =   adminer  &   owner -  >policy
}


```


## 🥈 Session-bound permissions


Another recurrent use of caveats is implementing access conditioned to the existence of a user session or properties of their session: Enforce a policy to allow access to resources only when users have 2FA set up Authorized based on attributes of SAML Session tags or JWT token claims


In the case of JWTs, after the client application verifies an incoming token, it could forward a subset of the claims as part of the SpiceDB request context to enrich the authorization operation.


One example could be access attenuation based on the audience claim (aud). A user has been granted admin permissions on a highly sensitive resource (e.g. administrative UI), but in order to be authorized we also need to make sure the web session was actually intended for the expected domain.


In this example the client application would write relationships with a stored value for the` expected_aud` caveat context key, which would take precedence over anything coming from the client-side. Here is the ([Playground Link](https://play.authzed.com/s/nWnJD5s7x5Gc/schema) ):


zed


1


2


3


4


5


6


7


8


9


10


11


12


```text
definition    user    {  }
definition    organization    {  }


caveat   valid_session_audience (  aud string, expected_aud string )    {
aud  =  =   expected_aud
}


definition    control_plane    {
relation    granted  :  user     with   valid_session_audience


permission   admin  =   granted
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


12


```text
definition    user    {  }
definition    organization    {  }


caveat   valid_session_audience (  aud string, expected_aud string )    {
aud  =  =   expected_aud
}


definition    control_plane    {
relation    granted  :  user     with   valid_session_audience


permission   admin  =   granted
}


```


## 🥇 Time-Bound Permissions


And number one goes to limiting access based on time criteria! Organizations use this approach to further reduce the attack surface, in line with the[principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) . This can be the likes of: only allowing employee access during working hours providing temporal access to support staff for a few hours to troubleshoot a customer problem Improving security posture by making all employee entitlements self-expiring


Since we’ve started with the well-understood GitHub model as an example, let’s see how a hypothetical temporal repository access grant could be implemented ([Playground Link](https://play.authzed.com/s/LQCYcKeAMrqh/schema) ):


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


```text
definition    user    {  }
definition    organization    {  }


caveat   temporal_grant (  grant_duration duration, current_timestamp string, grant_timestamp string )    {
timestamp (  current_timestamp )    -   timestamp (  grant_timestamp )   < grant_duration
}


definition    repository    {
relation    owner  :  organization     |   user
relation    reader  :  user     |   user  with   temporal_grant
relation    writer  :  user     |   user  with   temporal_grant
relation    adminer  :  user     |   user  with   temporal_grant


permission   read  =   reader  +   write
permission   write  =   writer  +   admin
permission   admin  =   adminer
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


```text
definition    user    {  }
definition    organization    {  }


}


definition    repository    {
relation    owner  :  organization     |   user
relation    reader  :  user     |   user  with   temporal_grant
relation    writer  :  user     |   user  with   temporal_grant
relation    adminer  :  user     |   user  with   temporal_grant


permission   read  =   reader  +   write
permission   write  =   writer  +   admin
permission   admin  =   adminer
}


```


## Wrap-up


So there you go, the top-3 most used caveat patterns for SpiceDB! 🏆 If you haven’t yet, feel free to check out the[documentation](https://authzed.com/docs/reference/caveats) 📖 and tinker with it in our recently released playground support! Have a question? Check out the[Discord](https://authzed.com/discord) , where we and the community are discussing all things SpiceDB


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- 🥉 IP Allowlists
- 🥈 Session-bound permissions
- 🥇 Time-Bound Permissions
- Wrap-up
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
