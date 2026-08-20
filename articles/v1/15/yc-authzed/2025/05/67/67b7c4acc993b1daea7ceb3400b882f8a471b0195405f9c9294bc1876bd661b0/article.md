---
schema_version: "1.0.0"
document_id: "67b7c4acc993b1daea7ceb3400b882f8a471b0195405f9c9294bc1876bd661b0"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/introducing-the-authzed-cloud-api"
published_at: "2025-05-28T12:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:a1ad1dd9bd0d24b807ee24014aa17921272ed3712f5db0b32b762b0887cd19ec"
---

# Introducing The AuthZed Cloud API

## Infrastructure for Authorization


For the team at AuthZed, our mission is to fix access control. The first step is creating the foundational infrastructure for others to build their access control systems upon. Infrastructure for Authorization, you say? Didn't infrastructure just go through its largest transformation ever with cloud computing? From introduction to the eventual mass adoption of cloud computing, the industry has had to learn to manage all of the cloud resources they created. In response, cloud providers offered APIs for managing resource lifecycles. Our infrastructure follows this same pattern, so today we're proud to announce the[AuthZed Cloud API](https://www.postman.com/authzed/spicedb/collection/5fm402n/authzed-cloud-api) is in Tech Preview.


## AuthZed Cloud API


The AuthZed Cloud API is a RESTful JSON API for managing the infrastructure provisioned on AuthZed Dedicated Cloud. Today, it is able to list the available permissions systems and fully manage the configuration for restricting API-level access to SpiceDB within those permissions systems.


As with all Tech Preview functionality, to get started, you must reach out to your account team and request access. Afterwards, you will be provided credentials for accessing the API. With these credentials, you're free to automate AuthZed Cloud infrastructure in any way you like! We recommend getting started by[heading over to Postman](https://www.postman.com/authzed/spicedb/collection/5fm402n/authzed-cloud-api) to explore the API. Next, why not break out a little bit of curl?


Listing all of your permissions systems:


shell


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


```text
curl --location 'https://api.$YOUR_AUTHZED_DEDICATED_ENDPOINT/ps' \
--header 'X-API-Version: 25r1' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $YOUR_CREDENTIALS_HERE' | jq .[​​{
"id": "ps-8HXyWFOzGtk0Yq8dH0GBT",
"name": "example",
"systemType": "Production",
"systemState": {
"status": "RUNNING"
},
"version": {
"selectedChannel": "Rapid",
"currentVersion": {
"displayName": "SpiceDB 1.41.0",
"version": "v1.41.0+enterprise.v1",
"supportedFeatureNames": [
"FineGrainedAccessManagement"
]
}
}
}]


```


shell


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


```text
curl --location 'https://api.$YOUR_AUTHZED_DEDICATED_ENDPOINT/ps' \
--header 'X-API-Version: 25r1' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $YOUR_CREDENTIALS_HERE' | jq .[​​{
"id": "ps-8HXyWFOzGtk0Yq8dH0GBT",
"name": "example",
"systemType": "Production",
"systemState": {
"status": "RUNNING"
},
"version": {
"selectedChannel": "Rapid",
"currentVersion": {
"displayName": "SpiceDB 1.41.0",
"version": "v1.41.0+enterprise.v1",
"supportedFeatureNames": [
"FineGrainedAccessManagement"
]
}
}
}]


```


Take note of the required headers: the API requires specifying a version as a header so that changes can be made to the API in the future releases.


I'm eager to see all of the integrations our customers will build with API-level access to our cloud platform! Look out for another announcement coming *very* soon about an integration that we've built using this new API, too!


*Join us on the mission to fix access control.*


[Schedule a call](https://authzed.com/call) with us to learn more about how AuthZed can help you.


On this page


- Infrastructure for Authorization
- AuthZed Cloud API


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)
