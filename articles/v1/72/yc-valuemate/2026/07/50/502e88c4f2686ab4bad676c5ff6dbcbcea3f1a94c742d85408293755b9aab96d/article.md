---
schema_version: "1.0.0"
document_id: "502e88c4f2686ab4bad676c5ff6dbcbcea3f1a94c742d85408293755b9aab96d"
company_key: "yc-valuemate"
company: "ValueMate"
source_id: "yc-valuemate-news-import-ff956c5b920c"
canonical_url: "https://www.valuemate.ai/blog/an-appraisers-guide-to-tricky-upd-data-collection-scenarios"
published_at: null
first_seen_at: "2026-07-24T05:50:05.419224+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:cc2fc08342b5fcdf7e211091654cf0166681870f65438b7ba483931aa879052f"
---

# An Appraiser's Guide to Tricky UPD Data Collection Scenarios

While there's a lot of talk about the high-level changes with UAD 3.6 and UPD, the most value is in the details. The "what do I actually do when I'm at the property?" questions. Let's explore a few specific, and potentially tricky, data collection scenarios using the latest guidance from Fannie Mae and Freddie Mac.


### The Garage Conundrum: Attached, Detached, and on Different Levels


How we document a garage is getting more specific. A detached garage requires you to complete all garage-specific fields and also invoke the 'roomType' data array. If the garage has no finished rooms, enter 'Other_Room' for the roomType and 'Garage' for the otherRoomTypeDescription.


But what about an attached or built-in garage that's on a different level from the main living area? The guidance states this should still be reported simply as an attached or built-in garage. The more complex 'unit array' data structure is reserved for truly detached structures.


### Seeing Double: How to Report Multiple, Identical Features


What do you do when a property has more than one of the exact same feature? The GSEs provide a perfect example: a condominium unit with two separate balconies. The instruction is to use the enumeration 'Balcony' for each one. You report the feature type twice.


This isn't just a simple count; you must provide all the necessary data elements and at least one photo for each individual feature. You can no longer just write "two balconies" in a free-text comment field. Each one is its own discrete entry in the dataset.


### Turning Up the Heat: Reporting on Multiple Heating Systems


The data collector must select all heating types that apply. If a home uses Hot_Water_Baseboard heating in one area and Wall_Furnace heating in another, you must select both. Another example: a property with Central_Hot_Air heated by a Gas_Furnace requires both heating types to be reported.


### The Big Takeaway


The transition to UAD 3.6 and the UPD is about more than just a new-look report; it's a fundamental change in how we collect and structure property data. Details that might have been grouped together or briefly described in an addendum now require their own discrete, structured data entries. Understanding these nuances is what will make for a smooth and successful transition.
