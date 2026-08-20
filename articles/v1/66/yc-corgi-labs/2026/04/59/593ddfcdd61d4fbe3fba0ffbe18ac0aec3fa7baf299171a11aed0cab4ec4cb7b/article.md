---
schema_version: "1.0.0"
document_id: "593ddfcdd61d4fbe3fba0ffbe18ac0aec3fa7baf299171a11aed0cab4ec4cb7b"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/resources/list-management"
published_at: "2026-04-23T14:51:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-22T22:57:19.890747+00:00"
content_hash: "sha256:9e0aa163ec2b1fe4cd351ba5f6eeb543c1845ba1ce84f8f6a342c772ab8c48b1"
---

# List Management

The List Management page is where payment operators build and maintain the reusable value lists that power Corgi's fraud detection rules. Every allow list, block list, and attribute collection lives here, whether it ships as a Corgi default or you author it yourself. Spend a few minutes on this page and the rules you write elsewhere in Intelligence become dramatically easier to reason about, because each rule can simply ask "is this value in that list?" instead of hard-coding individual values inline.


## What Are Lists?


Lists are collections of values (emails, card fingerprints, IP addresses, customer IDs, etc.) that can be referenced in your fraud rules. For example, you might create an email block list containing known fraudulent email addresses, and then create a rule that blocks any transaction matching an entry in that list.


This indirection is the whole point. One list can feed many rules, and updating the list propagates everywhere it is referenced. When a new bad actor shows up, you add the value once rather than editing every rule that needs to know about it.


## Default Lists


Corgi provides several default block lists out of the box, pre-populated with known bad actors. Default lists are managed by the system and updated automatically, so you inherit ongoing threat intelligence without operational overhead.


List


Purpose


Card Fingerprint Block List


Known fraudulent card fingerprints.


Email Block List


Known fraudulent email addresses.


Email Domain Block List


High-risk email domains.


Client IP Country Block List


Countries associated with high fraud rates.


Customer ID Block List


Known fraudulent customer identifiers.


Charge Description Block List


Suspicious charge descriptions.


Card Country Block List


Card-issuing countries to block.


Client IP Address Block List


Specific IP addresses to block.


Because these lists are system-managed, you cannot edit their entries directly, but you can reference them from your own rules exactly like any custom list.


## Custom Lists


You can create custom allow and block lists for any supported attribute type. Custom lists are where your team's institutional fraud knowledge accumulates: the email domain that keeps chargebacks, the BIN range tied to a gift-card abuse pattern, the customer ID you want to always approve.


Attribute Type


Description


Card Fingerprint


Unique card identifier.


Email


Customer email address.


Email Domain


Domain portion of the email (case-insensitive matching).


Customer ID


Your internal customer identifier.


Country


Country code.


IP Address


Client IP address.


Charge Description


Transaction description string (case-sensitive or case-insensitive matching).


Card BIN


Bank Identification Number (first 6 to 8 digits of card number).


String


Flexible string matching for custom attributes.


Choose the type that matches the attribute your rules will evaluate. Email Domain and Charge Description are worth calling out because they offer case-insensitive (and for Charge Description, case-sensitive) matching options, which matters when fraudsters vary capitalization to evade exact-match checks.


## List Details


The main table shows all lists with:


Column


Description


Name


List name and whether it's a Default list.


Alias


The programmatic reference used in rule conditions.


Type


The attribute type the list matches against.


Items


Number of entries in the list.


Created By


Who created the list (System or a team member).


Created


When the list was created.


The Alias column is the one you will use most often when authoring rules, because it is the short identifier your rule conditions actually reference. Pick aliases that read well inline, because they will appear in every rule that consumes the list.


## Creating a New List


Click **+ New List** to create a custom list. You'll need to specify:


Field


Description


Name


A descriptive name.


Alias


A short identifier for use in rule conditions.


Type


The attribute type.


Entries


Add values manually or import them in bulk.


Bulk import is the faster path when you are seeding a list from an existing spreadsheet, CSV export, or fraud analyst's investigation notes. Manual entry is better for one-off additions discovered in day-to-day review. Once the list exists, you can add or remove entries at any time, and every rule referencing the list picks up the change on the next evaluation.
