---
schema_version: "1.0.0"
document_id: "804e00bde79338bf2c6fd9a90556fa1e3f39053b66f4cb2dde51ba5b01e89b0b"
company_key: "yc-monarcha"
company: "Monarcha"
source_id: "yc-monarcha-news-import-ca6488fb1f69"
canonical_url: "https://monarcha.ai/blog/why-aerial-imagery-is-misaligned"
published_at: null
first_seen_at: "2026-07-22T04:46:27.146871+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:37970c979445f9443487babd1c21fd688f139ce76fd3b8cc7ec18e4aa7440f8f"
---

# Why Your Aerial Imagery Doesn't Line Up (And How to Fix It)

If you have ever loaded a drone orthomosaic into ArcGIS or QGIS and watched it land 10 meters away from your parcel boundaries, you are not alone. Misaligned aerial imagery is one of the most common and most frustrating problems in geospatial work. It affects county governments overlaying drone surveys on cadastral data, mining companies comparing satellite captures to historical mine plans, and engineering firms trying to register as-built photography to design coordinates.


## The root causes of misalignment


Aerial imagery can be off for several reasons, and they often compound each other.


**GPS accuracy at capture.** Consumer and prosumer drones typically rely on single-frequency GNSS receivers with 2-5 meter horizontal accuracy. Without RTK or PPK correction, every pixel in the resulting orthomosaic inherits that positional uncertainty. Even survey-grade equipment can drift in areas with poor satellite geometry or heavy tree canopy.


**Datum and projection mismatches.** A drone capture processed in WGS84 will not align with county parcel data in NAD83 State Plane. The difference between WGS84 and NAD83 is roughly 1-2 meters in the continental US, enough to make property boundaries look wrong. Older datasets may use NAD27, which can be off by tens of meters.


**Orthorectification errors.** Photogrammetric processing software builds a 3D model from overlapping images and projects it onto a terrain surface. Errors in the digital elevation model, insufficient image overlap, or poor GCP distribution can introduce localized distortions. The imagery may look correct in the center of the survey area but warp at the edges.


**Temporal basemap shifts.** Google, Bing, and Esri basemaps are composites assembled from imagery captured at different times and processed with different methods. The basemap itself may not be positionally authoritative. Aligning to a basemap that is itself offset will introduce systematic error.


## Why it matters more than you think


A 3-meter offset sounds small, but in practice it creates real problems. In county assessor offices, it means parcel boundaries appear to cross into neighboring lots. In mining, it means drill collar locations plotted on a georeferenced map disagree with GPS survey coordinates. In civil engineering, it means an as-built overlay shows utilities in the wrong position relative to the road centerline.


These discrepancies erode trust in the data. Teams revert to manual field verification, or worse, make decisions based on data they assume is accurate when it is not.


## How to fix it


The standard approach is to add ground control points: surveyed markers with known coordinates that tie the imagery to an authoritative reference frame. With enough well-distributed GCPs, you can correct for GPS offset, projection differences, and local distortions simultaneously.


The problem is that GCP collection is expensive and slow. It requires a survey crew in the field, and each project needs its own set of control. For organizations processing dozens of drone surveys per month, or aligning historical aerial photography where field access is no longer possible, manual GCP workflows do not scale.


## AI-powered aerial imagery alignment


Monarcha's georeferencing engine automates the alignment of aerial and satellite imagery to authoritative reference data. The system identifies matching features between the source imagery and reference datasets, computes a spatial transformation, and applies it to produce correctly aligned output.


This works for drone orthomosaics, satellite captures, scanned aerial photographs, and even historical air photos from decades ago. The result is imagery that aligns with your parcel data, your CAD drawings, and your GIS layers without requiring field-collected ground control.


For government agencies managing hundreds of aerial surveys across a jurisdiction, and for engineering firms processing drone captures on every project, automated alignment eliminates the single biggest bottleneck in getting aerial data into production GIS workflows.
