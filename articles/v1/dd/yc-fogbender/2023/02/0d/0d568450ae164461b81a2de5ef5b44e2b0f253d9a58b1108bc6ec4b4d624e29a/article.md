---
schema_version: "1.0.0"
document_id: "0d568450ae164461b81a2de5ef5b44e2b0f253d9a58b1108bc6ec4b4d624e29a"
company_key: "yc-fogbender"
company: "Fogbender"
source_id: "yc-fogbender-news-import-46cf0bf99c36"
canonical_url: "https://fogbender.com/blog/how-to-notify-several-customer-teams-at-the-same-time"
published_at: "2023-02-28T00:00:00+00:00"
first_seen_at: "2026-07-21T20:41:04.212025+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:6ad7655b7f94284848d32fa060879ca9d9a1b85f05e3b8a5871b6d09ecd070a1"
---

# How to notify several customer teams at the same time

Occasionally, multiple customers may report the same bug or request the same feature.


When this happens, it’s nice to have a single ticket for the issue in the developer-facing tracker (instead of one per customer), while keeping a record of all customers waiting for its completion.


When using Fogbender, this can be done by associating customer-facing conversations with a ticket in an external system.


In the example below, two customer teams - Tractors Unlimited and Sailing Ships - are waiting for a particular API (the middle- and right-most) panes, and the left-most pane is the internal “hub” for this particular feature.


For example, if someone on the R&D team marks #489 as closed, the customer-facing agents will be notified of this in the internal (green) room.


This internal room associated with issue #489 can also be used to “broadcast” an update to all related customer-facing rooms.


The first step is to compose a message or a block of messages you’d like to broadcast to the customer teams. You can ask others to contribute or proofread; the messages can contain images and code blocks.


For example, below is a possible broadcast for the customers waiting on the completion of issue #489:


Once you’re happy with the content, select all the messages you’d like to share with customers and click “Forward or file N messages”:


This brings up a modal where you can navigate to the “Related rooms” tab and select the customers you’d like to notify:


After clicking “Broadcast to N rooms”, the block of messages is “forwarded” to all selected rooms:


At this point, you may choose to wait for a response from every customer team to confirm that everything is working as expected before closing each customer-facing room.
