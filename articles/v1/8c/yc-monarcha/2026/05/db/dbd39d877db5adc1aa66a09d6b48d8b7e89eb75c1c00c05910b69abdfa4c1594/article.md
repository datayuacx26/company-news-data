---
schema_version: "1.0.0"
document_id: "dbd39d877db5adc1aa66a09d6b48d8b7e89eb75c1c00c05910b69abdfa4c1594"
company_key: "yc-monarcha"
company: "Monarcha"
source_id: "yc-monarcha-news-import-ca6488fb1f69"
canonical_url: "https://monarcha.ai/blog/how-to-georeference-a-scanned-map"
published_at: "2026-05-27T00:00:00+00:00"
first_seen_at: "2026-07-24T05:03:32.579277+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:0872ebccaaecb62e7e4c17474ef37ea8ea00388e221c5478cbacc1db64f7700d"
---

# How to Georeference a Scanned Map: A 2026 Guide (UTM, State Plane, and Custom Grids)

Georeferencing a scanned map means giving every pixel of that scan a real-world coordinate, so the map can be overlaid on satellite imagery, parcel data, drilling logs, or any other GIS layer. The mechanics are the same whether you do it by hand in QGIS, in ArcGIS Pro, or with an automated AI tool: identify the coordinate system, place control points, transform, evaluate, and export. This guide walks the whole process at a level that works for both modern aerials and difficult legacy sheets.


If you only georeference one or two sheets at a time, the manual workflow in QGIS or ArcGIS is fine and this guide will get you there. If you have an archive of hundreds or thousands of sheets, the same steps apply but you almost certainly want to automate the control-point loop —[we wrote about that tradeoff here](https://monarcha.ai/blog/qgis-georeferencer-vs-automated-ai) .


## Step 1: Identify the coordinate reference system


Look at the marginal notations on the source map. Most printed maps tell you what coordinate system they were drawn in. Look for an EPSG code, a UTM zone label, a state plane FIPS reference, a datum name (NAD27, NAD83, WGS84), or a published projection (Lambert Conformal Conic, Transverse Mercator, Albers Equal Area). If the map gives no hints, fall back to historical context: what agency produced it, in what era, for what region. A 1980s county zoning map in the United States is almost certainly state plane on NAD27. A 1950s Chilean geological survey is almost certainly a local UTM zone with a regional datum.


A few practical shortcuts:


- **UTM** grids show a zone number (1 through 60) and a hemisphere. Find the zone label in the margin. The EPSG code is then 326xx for the northern hemisphere (EPSG:32610 = UTM zone 10N on WGS84) or 327xx for the southern hemisphere.
- **State plane** in the United States is specified by a FIPS code per state and zone. The EPSG codes for NAD83 state plane are in the 269xx and 322xx ranges; for NAD27 the codes are in the 26700 range. The authoritative lookup is the NGS State Plane Coordinate System documentation.
- **Local mine and survey grids** often have no published EPSG code at all. You will need a transformation supplied by the surveyor of record, or you georeference the sheet against a known basemap and capture the transformation parameters from the result.
- **When in doubt** , start with a guess and check whether your control points have small residuals. If they don't, the CRS guess is usually wrong before any of the points are.


## Step 2: Assemble a basemap to register against


Pick a reference dataset that overlaps the area covered by the source map. Modern satellite imagery (ESRI World Imagery, Bing, Mapbox), current OpenStreetMap, or the most recent USGS topographic quad are all reasonable starting points. The right choice depends on what kind of features your source map shows. For a parcel map, a current parcel layer is the best reference. For a road map, OpenStreetMap roads. For an aerial photo, modern aerial imagery. The basemap defines the truth your source will be aligned to.


## Step 3: Place ground control points


A ground control point is a location that is identifiable on both the source map and the basemap, with known real-world coordinates. Manually: open both rasters side by side in QGIS, ArcGIS, or another GIS, identify a recognizable feature on the source (a grid intersection, road junction, surveyed monument, building corner), click the corresponding location on the basemap, and repeat until you have a well-distributed set of points. Automatically: an AI georeferencing tool detects and matches points using a feature-matching model and surfaces a confidence score per point. Aim for at least four well-distributed points for a basic affine transform; ten or more for a polynomial or thin plate spline.


Three rules of thumb for placement, whether you are picking points manually or reviewing automatically detected ones:


- **Spread points to the edges** , not just the center. A cluster of points in the middle of the map will give great residuals locally and unpredictable distortion at the corners.
- **Prefer hard, unambiguous features** like grid intersections, surveyed monuments, building corners, and confirmed road junctions over soft features like vegetation edges or shorelines.
- **Cross-check at least one point** per quadrant. The worst georeferencing failures are the ones that look fine on the points the operator placed and are catastrophically wrong everywhere else.


## Step 4: Pick a transformation method


The transformation determines how the source raster is warped to align with the basemap. Affine (six parameters): rotation, scale, and translation, suitable for clean modern scans with little local distortion. Polynomial (first, second, or third order): handles gentle large-scale distortion across the map. Thin plate spline: bends the raster locally to fit every control point exactly, suitable for hand-drawn or distorted source maps but easy to over-fit. Projective: corrects perspective distortion, useful for off-axis photos of paper maps. Pick the simplest transformation that handles your distortion, not the most powerful one.


## Step 5: Evaluate residuals


After the transformation runs, every control point will have a residual: the distance between where the model predicted the point would land and where the point actually landed on the basemap. Modern GIS tools and AI georeferencers both report this per point. Look at the distribution. A few large outliers usually mean a control point was mis-clicked or detected incorrectly; remove or re-place those points and re-run. A pattern of growing residuals at the edges usually means the wrong transformation order was picked. Uniformly large residuals across the whole map usually mean the wrong coordinate reference system was selected.


A few patterns that diagnose common failures:


- One point with a huge residual and the rest small: the point was mis-clicked or mis-detected. Remove or re-place it.
- Residuals growing toward the corners: the transformation order is too low for the distortion in the source map. Try one polynomial order higher, or switch to thin plate spline.
- Residuals uniformly large: the wrong CRS was almost certainly selected. Go back to step 1.
- Residuals fine but the overlay still looks wrong: the basemap and source disagree on a real geographic change (a road that was rerouted, a coastline that shifted). Use older reference data or fall back to features that have not changed.


## Step 6: Export the georeferenced raster


Once residuals are acceptable, export the result. The right format depends on how it will be consumed. GeoTIFF is the universal default: a TIFF with embedded geospatial metadata that any GIS will read. Cloud Optimized GeoTIFF (COG) is a tiled, internally chunked GeoTIFF that streams efficiently from object storage and is the right choice for any web-mapping or pipeline consumer. JPEG2000 is occasionally still required by older systems. Always preserve the original CRS metadata in the output so downstream consumers do not have to re-detect it.


## When to automate the loop


The six steps above are the same whether they take a human a half-hour or an AI tool a few seconds. The question is which one is doing the work. For a one-off sheet on a clean modern source, a competent technician in QGIS is hard to beat for total cost. For an archive of hundreds or thousands of sheets, especially when the archive contains hand-drawn plats, mylar overlays, or faded photocopies that exhibit the modality gap we described in[Heterogeneous vs Homogeneous Georeferencing](https://monarcha.ai/blog/heterogeneous-vs-homogeneous-georeferencing) , the manual workflow stops being economical and an automated pipeline becomes the right call.


If you want to see what an automated pipeline does on your own sheets, the Monarcha team is happy to run a sample batch on a live call —[book a demo](https://cal.com/james-monarcha/monarcha-enterprise) .
