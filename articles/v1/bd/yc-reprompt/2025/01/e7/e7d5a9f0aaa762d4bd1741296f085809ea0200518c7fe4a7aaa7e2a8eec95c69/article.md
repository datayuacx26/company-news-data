---
schema_version: "1.0.0"
document_id: "e7d5a9f0aaa762d4bd1741296f085809ea0200518c7fe4a7aaa7e2a8eec95c69"
company_key: "yc-reprompt"
company: "Reprompt"
source_id: "yc-reprompt-news-import-f6b2fbe9777c"
canonical_url: "https://repromptai.com/blog/announcing-scout-integration-with-esri-arcgis"
published_at: "2025-01-10T00:00:00+00:00"
first_seen_at: "2026-07-22T11:34:11.939783+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:1d3d4e113bacd8979b0b24642ba51b9558d092d64cbfb3ce3259d75d05c8b4b2"
---

# Announcing Scout integration with ESRI ArcGIS

Our Scout geospatial model automates repetitive GIS workflows— and starting today Scout can work directly with ArcGIS Online to digitize a parcel boundary from imagery, edit the attribute of that feature with web research, and cross-reference it using street-side imagery from Mapillary. All without a single mouse-click.


We can ask Scout to trace and save the boundary of a school and parking directly in ArcGIS Online. It's also able to edit the feature to save attributes like the number of spaces in the region.


## Capabilities


Today the model supports a few common repetitive tasks in ArcGIS


**Digitizing outlines of features** : Generates precise geofences automatically. Given a table of places and the type of geometry, Scout can find, trace, and save each one within ArcGIS.


**Keep attributes of features up to date using web research** : Scout can research a list of attributes for your features using a web browser, and edit them to match the most up to date data.


**Cross-reference aerial and street-side images:** By using Mapillary, Scout can find and georeference objects using street-level imagery to update features in ArcGIS


**Validating data across the Web, PDFs, Aerial and Street-level Imagery**


Here we ask Scout to cross-check a city speed limit with the actual signage on the road. The model navigates to the same lat/long in Mapillary to find the sign and mark the 25mph speed limit as confirmed for the road feature in ArcGIS.


Today, Scout understands visual sources— Mapillary, KartaView, Google Street View or aerial imagery from the ArcGIS Living Atlas — and has a robust web browser to look up information from official public sources.


## How it works


Scout processes visual data directly from the browser. Under the hood, it uses multiple ai models to understand imagery, tabular data, and the ArcGIS interface itself. An LLM planning module uses perception data and agent state to decide the next plan. Finally it operates the browser through clicking, typing and navigating like a user would. The agent can be configured to run in a local browser, or scaled to hundreds of remote machines.


## What's Next


The integration is available now for ArcGIS Online subscribers.


Scout automates manual tedious tasks in GIS analysis. Instead, teams can set up Scout workflows and let the agent handle the technical execution and focus on the analysis. You can[schedule a demo here](https://cal.com/lukasm/20-min-meeting) .
