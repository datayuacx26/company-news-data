---
schema_version: "1.0.0"
document_id: "6b544a9dbd1908952cc67392f33d31ba95ff98af1f48c469549e5537127c575b"
company_key: "yc-fulcrum"
company: "Fulcrum"
source_id: "yc-fulcrum-news-import-4f6fb51d9f9d"
canonical_url: "https://www.fulcrumapp.com/blog/wildnote-teams-your-project-data-just-got-a-lot-easier-to-share/"
published_at: "2026-05-29T19:55:55+00:00"
first_seen_at: "2026-07-25T05:56:36.213971+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:f757b40dd2fdd719296f77826d9e5799ff2d071091d47b4d8f570679b966736c"
---

# Wildnote teams: Your project data just got a lot easier to share

Getting project data to clients is one of the most tedious parts of the job for environmental field teams. Sharing what you find shouldn’t be a whole second job.


## Some projects have a final report. Others never really end.


For projects with a defined endpoint — a Phase I ESA, a wetland delineation, a final biological survey report — a well-formatted PDF or CSV delivered at project close is exactly the right tool. Wildnote has always done that well, and Fulcrum carries that forward. The export tools are still there, the PDF report generation is still there, and a final deliverable is still a final deliverable.


But a lot of environmental work doesn’t end with a single report. Some projects run for months or years, with data accumulating continuously and clients or agencies who need to see what’s happening right now — not in the next scheduled email. That’s where a live project data feed changes things.


## Projects where this matters most


- Construction compliance monitoring


. Weekly or daily site inspections, verified by an agency contact to confirm required inspections are complete and corrective actions are underway.
- Stormwater programs


. SWPPP inspections, BMP tracking, and discharge monitoring where the regulatory record needs to reflect current status, not last week’s export.
- Large cultural resource surveys


. Multiple crews collecting across a wide area over many weeks, while project managers and agency contacts need current coverage data.
- Multi-year monitoring programs


. Vegetation, wildlife, or restoration projects where a client tracks cumulative collection progress season over season.


For these projects, the manual export cycle creates a lag between what your team is collecting and what your client or regulator can actually see. A live feed closes that gap without adding any work on your end.


## How project data sharing works: two tools, one concept


Fulcrum gives you two ways to share live project data with people outside your account. Both generate a URL you control. The recipient just opens the link and sees current data. They don’t need an account, a license, or anything installed. You decide what they can see, and you can turn off access at any time.


Data Shares are the fastest path. Enable the share on any app (remember, this is a “form” in Wildnote parlance), copy the URL, send it. Your recipient gets a live feed of your records in whatever format works for them — a spreadsheet, a GIS feed, a map they can open in a browser. It takes about two minutes to set up.


Saved Filters give you more precision. Define a filter first — by site, project, status, or date range — then share only that slice of your data. Your Site A client gets a link that only ever shows Site A records. Your stormwater agency contact gets a link filtered to open corrective actions. Saved Filters also support Esri Feature Service, which connects your records directly into ArcGIS Online or ArcGIS Pro as a live layer — add it once and it stays current.


A high value format for both tools is the map view and Fulcrum gives you two distinct ways to put live project data in front of the people who need it — and they serve different audiences.


[Data Shares](https://help.fulcrumapp.com/en/articles/92480-creating-data-shares-in-fulcrum) gives you an embedded map which is the right tool when your recipient just needs to *see* what’s happening. Drop it into any webpage or client portal and it renders a live map of your records — pins on a map, clickable popups with the field data you choose to show, updating automatically as your team collects. Your agency contact opens the page and sees current inspection coverage. Your client sees monitoring progress without logging into anything or opening any software. It requires a little configuration to make sure the right fields show up in the popups, but once it’s set up it runs itself.


[Saved Filters](https://help.fulcrumapp.com/en/articles/4227703-what-are-saved-filters) gives you the Esri Feature Service, which is the right tool when your recipient works in ArcGIS. Instead of viewing a map on a webpage, their GIS team adds your Fulcrum share URL directly into ArcGIS Online or ArcGIS Pro as a live feature layer. Your records show up in their GIS environment, styled however they need, sitting alongside their other project layers. It updates automatically as your team collects, so nobody is exporting files, passing around shapefiles, or sorting out version control.


For recipients who work in other tools, both Data Shares and Saved Filters also support:


- **CSV** — live feed into Excel or Google Sheets; use =IMPORTDATA() to keep a sheet permanently synced
- **GeoJSON** — connects directly into QGIS or ArcGIS Online as a live layer
- **KML** — add as a Network Link in Google Earth Pro for automatic updates
- **JSON** — for developers building custom dashboards or integrations


## A couple of things worth knowing before you share


By default, shared data includes who created each record, who last updated it, and who it’s assigned to. For anything going outside your organization, enable the Anonymize Data option to strip those staff identifiers out. That way your client sees the field data — not your employees’ names.


Both types of sharing use a unique access token in the URL. Share it with the same intention you’d share a password-protected link. If you need to cut off access, disabling a Data Share immediately invalidates the URL. For a Saved Filter, set it back to private. The Fulcrum Shares Utility gives you a centralized view of everything your organization currently has published, which is useful when you’re managing shares across multiple active projects.


Sharing environmental project data in real time used to be the hard part. With Fulcrum, it’s just part of the workflow. Your field data, your reports, and your project status are exactly where your clients and team need them, when they need them. And when you move from Wildnote to Fulcrum, consider this just the start of what’s now possible.


For full setup details on any of these, the links below have you covered — Data Shares, Saved Filters, and the Shares Utility.


## Watch on demand


[Taming the beast: Streamlining SWPPP, ESA, and remediation workflows](https://www.fulcrumapp.com/resources/webinar/streamlining-swppp-esa-and-remediation-workflows/)


[Creating Data Shares in Fulcrum — full setup guide →](https://help.fulcrumapp.com/en/articles/92480-creating-data-shares-in-fulcrum)


[Learn about Saved Filters →](https://help.fulcrumapp.com/en/articles/4227703-what-are-saved-filters)


[Manage active shares — Fulcrum Shares Utility →](https://shares.util.fulcrumapp.com/)
