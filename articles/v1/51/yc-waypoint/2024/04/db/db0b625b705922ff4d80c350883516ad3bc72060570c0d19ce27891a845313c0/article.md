---
schema_version: "1.0.0"
document_id: "db0b625b705922ff4d80c350883516ad3bc72060570c0d19ce27891a845313c0"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-news-import-ffde4d661374"
canonical_url: "https://www.usewaypoint.com/blog/prevent-threading-on-gmail-with-the-x-entity-ref-id-email-header"
published_at: "2024-04-26T00:00:00+00:00"
first_seen_at: "2026-07-22T19:31:17.685303+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:db684017a4762537d44fe7b183a5d8be08dfafd9b39fa3f28cc3951497f4770b"
---

# Prevent threading on Gmail with the X-Entity-Ref-ID email header

## Problem


When sending transactional emails from your application that contain the same subject and sender (and within a short time frame), Gmail will sometimes treat these emails as part of the same conversation and put them into a thread.


More often than not, you probably want to separate these emails and force Gmail not to thread your transactional emails into a conversation.


## Solution


Luckily, this can be easily be done by setting the` X-Entity-Ref-ID`[email header](https://www.hostinger.com/tutorials/email-headers/) to a unique value.


Once this is set, all subsequent sends will show up as separate emails threads.


## Looking for an easier way?


[Waypoint](https://www.usewaypoint.com/) is an email API with a built-in[email template builder](https://www.usewaypoint.com/) . On Waypoint, we automatically set the` X-Entity-Ref-ID` to the message ID on Waypoint to prevent threading on all transactional email notifications.


*Screenshot of Waypoint’s powerful transactional email template builder*
