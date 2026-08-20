---
schema_version: "1.0.0"
document_id: "cdb255e5caf7ffb1a541cc53388b7162b84dfcd47794cd6519cbbfaa38ab40b7"
company_key: "airgain-inc-common-stock"
company: "Airgain Inc."
source_id: "airgain-inc-common-stock-news-import-92deaec5a802"
canonical_url: "https://airgain.com/blog/nl-sw-lte-qbg95-migration-faqs/"
published_at: "2021-05-12T19:59:19+00:00"
first_seen_at: "2026-07-24T14:54:55.214562+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:766a47149dc490cc724c51c456038039ad94a9983a26177eee16dd5a86ceca54"
---

# NL-SW-LTE-QBG95 Migration FAQs

The Announcement from Quectel that the BG96 Cellular Module will soon be unavailable has a large portion of our customers migrating to the NW-SW-LTE-QBG95 Embedded Cellular Modem. With a large number of migrations taking place, we have encountered a number of frequently asked questions that we will reference here for your reference.


### NL-SW-LTE-QBG95 Migration FAQs


##### I am having trouble with powering up the modem. Is there a difference?


Yes, the power-up sequence is different from the QBG96 to the QBG95. Please review the[QBG95 datasheet](https://airgain.com/Documentation/Skywire/4G_LTE_Cat_M1_Quectel/1002362_NL-SW-LTE-QBG95_Datasheet.pdf) or the[migration guide](https://airgain.com/Documentation/Skywire/4G_LTE_Cat_M1_Quectel/1002637_NL-SW-LTE-QBG9x_Migration_Guide.pdf) .


##### Can I continue to have pin #20 tied to GND with the QBG95?


No, this will likely cause the modem to reset continuously.


##### Can I continue to use QMI with the QBG95?


No, unfortunately the QBG95 does not support the QMI protocol. We recommend evaluating the PPP protocol as the tested and proven option for connecting the modem.


##### Can I use an older NL-M1DK development kit to test the QBG95?


No, this dev kit does not apply power to the QBG95 appropriately. Please use the new NL-SWDK2 development kit.
