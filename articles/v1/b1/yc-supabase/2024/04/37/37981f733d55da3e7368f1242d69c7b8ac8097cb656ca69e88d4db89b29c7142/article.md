---
schema_version: "1.0.0"
document_id: "37981f733d55da3e7368f1242d69c7b8ac8097cb656ca69e88d4db89b29c7142"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-swift"
published_at: "2024-04-15T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:ad915dbfc1bc110cc321691c4eeb331ffa6f2d3f3eb3a0cb03ce01342672bb87"
---

# Supabase Swift

We are excited to announce that Supabase Swift libraries are now officially supported by Supabase.


This makes it simple to interact with Supabase from applications on Apple's platforms, including iOS, macOS, watchOS, tvOS, and visionOS:


`
_13


let url = URL(string: "...")!


_13


let anonKey = "public-anon-key"


_13


let client = SupabaseClient(supabaseURL: url, supabaseKey: anonKey)


_13


_13


struct Country: Decodable {


_13


let id: Int


_13


let name: String


_13


}


_13


_13


let countries: \[Country\] = try await supabase.from("countries")


_13


.select()


_13


.execute()


_13


.value


`


## New features#


This release includes the following new features:


- WhatsApp OTP:[https://github.com/supabase/supabase-swift/pull/287](https://github.com/supabase/supabase-swift/pull/287)
- Captcha support:[https://github.com/supabase/supabase-swift/pull/276](https://github.com/supabase/supabase-swift/pull/276)
- SSO:[https://github.com/supabase/supabase-swift/pull/289](https://github.com/supabase/supabase-swift/pull/289)
- Simplified Storage uploads:[https://github.com/supabase/supabase-swift/pull/290](https://github.com/supabase/supabase-swift/pull/290)
- Anonymous sign-ins:[https://github.com/supabase-community/supabase-swift/releases/tag/v2.6.0](https://github.com/supabase-community/supabase-swift/releases/tag/v2.6.0)
- Simplified OAuth:[https://github.com/supabase/supabase-swift/pull/299](https://github.com/supabase/supabase-swift/pull/299)


## What does official support mean?#


Swift developers can now integrate Supabase services seamlessly with official support. This means:


- **Direct assistance from the Supabase team** : Get timely and effective help directly from the developers who build and maintain your tools.
- **Continuously updated libraries** : Stay up-to-date with the latest features and optimizations that are fully tested and endorsed by Supabase.
- **Community and collaboration** : Engage with a broader community of Swift developers using Supabase, share knowledge, and contribute to the library's growth.


## Contributors#


We want to give a shout out to the community members who have contributed to the development of the Supabase Swift libraries:


[grdsdev](https://github.com/grdsdev) ,[satishbabariya](https://github.com/satishbabariya) ,[AngCosmin](https://github.com/AngCosmin) ,[thecoolwinter](https://github.com/thecoolwinter) ,[maail](https://github.com/maail) ,[gentilijuanmanuel](https://github.com/gentilijuanmanuel) ,[mbarnach](https://github.com/mbarnach) ,[mdloucks](https://github.com/mdloucks) ,[mpross512](https://github.com/mpross512) ,[SaurabhJamadagni](https://github.com/SaurabhJamadagni) ,[theolampert](https://github.com/theolampert) ,[tyirenkyi](https://github.com/tyirenkyi) ,[tmn](https://github.com/tmn) ,[multimokia](https://github.com/multimokia) ,[zunda-pixel](https://github.com/zunda-pixel) ,[iamlouislab](https://github.com/iamlouislab) ,[jxhug](https://github.com/jxhug) ,[james-william-r](https://github.com/james-william-r) ,[jknlsn](https://github.com/jknlsn) ,[jknlsn](https://github.com/glowcap) ,[Colgates](https://github.com/Colgates) ,[ChristophePRAT](https://github.com/ChristophePRAT) ,[brianmichel](https://github.com/brianmichel) ,[junjielu](https://github.com/junjielu) .


## Getting started#


We've released a[new guide](https://supabase.com/docs/guides/getting-started/tutorials/with-swift) to help you get started with the key features available in Supabase Swift.


Or you can jump into our deep dive to use iOS Swift with Postgres & Supabase Auth:
