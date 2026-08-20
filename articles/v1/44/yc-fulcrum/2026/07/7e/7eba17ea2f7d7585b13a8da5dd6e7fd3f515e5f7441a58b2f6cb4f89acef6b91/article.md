---
schema_version: "1.0.0"
document_id: "7eba17ea2f7d7585b13a8da5dd6e7fd3f515e5f7441a58b2f6cb4f89acef6b91"
company_key: "yc-fulcrum"
company: "Fulcrum"
source_id: "yc-fulcrum-news-import-4f6fb51d9f9d"
canonical_url: "https://www.fulcrumapp.com/blog/wildnote-teams-your-forms-just-got-a-lot-smarter/"
published_at: "2026-07-01T20:23:12+00:00"
first_seen_at: "2026-07-25T05:56:36.213971+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:d5493f3f25080b2ec1caed250cac6f773c8d1bcf97213d8cfb561402afae74ea"
---

# Wildnote teams: Your forms just got a lot smarter

*Explore calculation fields you control, location data that fills itself in, and hierarchical artifact classification built for fieldwork.*


## You shouldn’t need a developer for that


Wildnote’s pre-built templates — USACE wetland delineation, California Rapid Assessment Method, Vegetation Monitoring, for example — come with calculation fields such as plant dominance, habitat condition scores, and percent cover totals already built in. If you used those templates, the math was already there, and it works well.


The challenge comes when you need to change something, or when building a custom field form from scratch. Adding or modifying a calculation means asking Wildnote to do it.


In Fulcrum, you do this yourself. Every app supports calculation fields, and you build and modify them directly in the app builder. The expression editor uses JavaScript, and Fulcrum’s documentation covers the patterns that come up most in environmental workflows. For most use cases, you’re working with basic arithmetic, conditional logic, and text — the kind of expressions anyone comfortable with a spreadsheet can handle.


The more powerful capability is validation.


## Calculation fields catch mistakes while on site


Calculation fields in Fulcrum can evaluate logic and display a result inline on the form, on the device, before the record is submitted.


For example, when a field crew observes a rare or special-status species, the form can recognize it and prompt for additional documentation — confidence level, photos, behavioral notes — before the crew leaves the site. A water quality form can compare readings against regulatory thresholds and display pass or fail for each value before anyone packs up. Or a habitat assessment can compute a condition score on the device so the ecologist knows before leaving whether a plot needs a follow-up visit.


It’s easy for a field technician to fat-finger a pH reading, entering 84 instead of 8.4.


While a basic range check flags these typos instantly, timing is everything. Catching the mistake while the crew is still on-site costs nothing; discovering it during report review can force a site revisit.


## Your GPS already knows where you are. Your forms can too.


Every Fulcrum record captures a GPS location. Fulcrum’s robust API and data events let you use that location to automatically populate fields in the field form when a record is created.


Section, township, and range is often handed off to GIS after the fact, creating one more task for a team that’s already at capacity. When a crew member pins a location in Fulcrum, a data event can pull it from the Public Land Survey System at the moment of record creation, along with city, county, and state. It never becomes a GIS task at all.


Weather works the same way. A data event can query a weather service at the time of record creation and write current temperature, wind speed, sky cover, and precipitation directly into the field form. For biological surveys where conditions are required protocol data, that removes a manual entry step and means everyone on the crew is pulling from the same source rather than noting conditions from memory.


Both of these features are user-configurable. You set them up in the app builder.


## Every artifact has a type. Your form should reflect that.


Cultural resources fieldwork means classifying what you find, and what you find often doesn’t fit a simple dropdown. A historic artifact scatter might include tin cans, glass bottles, worked lithics, ceramics, and metal objects. Each category has its own typology for interpretation and site dating.


A corner-notched projectile point is different from a side-notched one. A machine-made bottle is different from a hand-blown one. Those distinctions show up in period attribution, and period attribution shapes everything else that happens with the site. Getting the classification right in the field is what makes the data useful.


Scrolling a flat list of 80 artifact types to find what you need is slow. It also invites the wrong selection. Classification fields work differently.


Fulcrum’s classification fields let you build a hierarchical taxonomy and navigate it level by level. For a historic artifact scatter, that might look like:


- Material → Prehistoric → Lithic → Projectile Point → Corner-Notched
- Material → Historic → Glass → Bottle → Machine-Made
- Material → Historic → Metal → Tin Can → Hole-in-Top


At each step the list narrows. By the time your crew makes a selection, they’re choosing from a short, contextually relevant set, not scanning an undifferentiated list. The result is faster selections, fewer wrong picks, and data that comes back typed correctly the first time. (And Fulcrum’s AI-powered Audio FastFill takes it further by letting you navigate and populate classification fields entirely by voice. But more on that another time.)


The[Classification Set Utility](https://classification-set-utility.util.fulcrumapp.com/) lets you build and manage these taxonomies and import them into your apps. Teams working across multiple project types such as prehistoric sites, historic period, or industrial, can maintain separate sets and assign them to the right forms.


## Good for the field. Better for the office.


Fulcrum is a field operations management platform, and that distinction shows up most clearly in how work moves between field and office. These features all do the same thing: move data quality upstream to the point of collection. The project manager’s review gets faster, the report is more defensible, and the firm’s institutional data stays usable across projects and seasons.


Next month, look for automation insights and how to connect Fulcrum to the rest of your tools so the right things happen the moment a record comes in.


[Check out external data and APIs](https://help.fulcrumapp.com/en/articles/92711-using-external-data-or-apis)[→](https://help.fulcrumapp.com/en/collections/50527-working-with-data-events-or-calculation-fields)


[Learn more about calculation fields →](https://help.fulcrumapp.com/en/collections/50527-working-with-data-events-or-calculation-fields)


[Explore the classification set utility →](https://classification-set-utility.util.fulcrumapp.com/)
