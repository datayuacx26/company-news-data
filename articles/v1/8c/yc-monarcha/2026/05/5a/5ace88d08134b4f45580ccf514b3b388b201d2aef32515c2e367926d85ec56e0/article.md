---
schema_version: "1.0.0"
document_id: "5ace88d08134b4f45580ccf514b3b388b201d2aef32515c2e367926d85ec56e0"
company_key: "yc-monarcha"
company: "Monarcha"
source_id: "yc-monarcha-news-import-ca6488fb1f69"
canonical_url: "https://monarcha.ai/blog/heterogeneous-vs-homogeneous-georeferencing"
published_at: "2026-05-27T00:00:00+00:00"
first_seen_at: "2026-07-24T05:03:32.579277+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:a9a88686c67acbec04520ee98fb0779d0c3977275cd84f2c654c1ce461849904"
---

# Heterogeneous vs Homogeneous Georeferencing: Why the Modality Gap Breaks Legacy Map Pipelines

Most georeferencing tools sold today are good at one specific problem: aligning a clean modern raster to a clean modern reference. Two satellite tiles. A drone orthomosaic and a basemap. A scanned topographic sheet from the 1990s and the current USGS quad. When both pictures come from the same kind of imaging system, the problem is tractable. The same edges, the same continuous-tone textures, the same projection conventions are on both sides of the match.


That is the homogeneous case. It is also the case that the GIS-procurement RFP usually describes. It is not the case that actually lives inside a county records office, a state geological survey, a mining company's exploration vault, or an AEC firm's thirty-year project archive.


The real archive is heterogeneous. A 1947 hand-inked plat of survey sits next to a 1986 mylar utility overlay sits next to a sun-faded photocopy of a redevelopment zoning amendment. The reference data we are trying to match against — OpenStreetMap, modern aerial imagery, a current parcel layer — is none of those things. It is a clean digital raster or vector tile produced by a completely different imaging pipeline.


The gap between those two kinds of pictures has a name in the computer vision literature. It is called the **modality gap** , and it is the single biggest reason traditional georeferencing fails on legacy archives. If you are evaluating AI georeferencing vendors, this is the concept the rest of the conversation has to start from.


## Homogeneous matching: the case GIS already solves


Image matching is the foundational task underneath every georeferencing system, whether the system is automatic or manual. The technician picking a control point in QGIS is doing image matching in their head: they look at a recognizable feature on the source map and find the same feature on the target basemap. An automated system does the same thing with feature descriptors instead of eyeballs.


Classical computer vision built up an arsenal of these descriptors over decades. SIFT, SURF, ORB, AKAZE, and their learned successors like SuperPoint and DISK all encode small image patches as vectors that can be matched against vectors from another image. When the two images come from the same modality — both aerial photos, both satellite tiles, both modern map renders — these descriptors work well enough to support production GIS workflows. Tools like Hugin, Agisoft, and the alignment routines inside ArcGIS Pro and QGIS lean on exactly this lineage.


This is homogeneous georeferencing. The two pictures speak the same visual language, so a matcher trained on one of them works on the other.


## Heterogeneous matching: the case GIS does not solve


Now hand the same matcher a scanned 1947 plat and a current ESRI World Imagery tile. The plat is a hand-inked line drawing on cream paper with monument calls and bearing distance notation. The basemap is a continuous-tone orthomosaic. They describe the same patch of ground. Almost nothing on one side looks like the corresponding thing on the other side. Edges have different statistics. Textures do not exist on the line drawing at all. Colors are uncorrelated. A SIFT descriptor computed at a plat corner and a SIFT descriptor computed at the corresponding aerial corner live in different parts of feature space, and the matcher cannot tell they are the same point.


That is the heterogeneous case. The two pictures are describing the same scene through fundamentally different imaging conventions. It is the same problem a thermal camera and a visible-light camera have when they try to register a face, or a medical CT scan and an MRI of the same organ. The literature calls it **cross-modal matching** , and the central difficulty is the modality gap.


Most off-the-shelf georeferencing tools were never built for this case. They were built for the homogeneous case and then handed heterogeneous data because that is what the archive contains. The result is the experience every GIS director has had: the tool either fails to find enough control points, or it finds wrong ones, or the only workable path is to fall back to a human picking points by hand.


## Why narrow models don't solve it


The historical response to the modality gap was to train narrow, modality-specific feature extractors on small, hand-labeled datasets. One model for satellite-to-satellite. Another for aerial-to-map. Another for thermal-to-optical. Each of these worked acceptably inside its niche and generalized poorly outside it. Survey plats, with their endless variety of cartographic conventions and their refusal to look like any single benchmark, sat almost entirely outside every niche anyone had ever trained on.


The problem is not that the architectures were wrong. The problem is that the niches were too narrow and the data that defined them was too small. A model trained on a thousand satellite-to-map pairs has never seen a sun-faded photocopy of a 1962 zoning amendment, and it does not transfer to one.


## The scale shift: data, not architecture


The thing that has actually moved cross-modal matching forward in the last few years is not exotic architectures. It is data scale. Once you can systematically generate enormous volumes of correctly paired examples that span the full range of imaging styles a real archive can throw at you — hand drawings, mylar, blueprint reproductions, continuous-tone aerials, faded photocopies — the same foundational matching networks can learn to bridge those gaps without specialized per-modality models.


This is the philosophical shift behind Monarcha's approach to legacy georeferencing. The interesting work is not in inventing a better SIFT. It is in building the data engine that produces the matched pairs the matcher needs to see, at the diversity and the volume that a real archive demands. We wrote about how that played out for survey artifacts in[the Survey Map Georeferencer launch post](https://monarcha.ai/blog/survey-map-georeferencer-launch) .


## What this means for buyers


If you are responsible for moving a paper or PDF archive into a modern GIS, the modality-gap concept gives you a concrete way to evaluate vendors. A few questions worth asking on every demo:


- **What modality was the system trained on?** If the answer is "satellite imagery" or "modern aerial photos," you are buying a homogeneous matcher. It will work on the clean modern subset of your archive and struggle on everything else.
- **What is the diversity of the training pairs?** The relevant number is not the headline dataset size, it is how many distinct cartographic styles the model has actually seen paired with a reference. Ten thousand pairs of one style is a narrow model. Tens of thousands across hand drawings, mylars, photocopies, faded blueprints, and modern scans is a bridge model.
- **What happens when matching fails?** Cross-modal matching is not magic. The honest systems surface confidence and let you fall back to manual review on hard sheets. The dishonest ones report a successful match anyway and quietly produce a misregistered raster.
- **Can it batch your actual archive, not a curated demo?** The way you find out whether a vendor has solved the heterogeneous case is to hand them a hundred sheets pulled at random from your worst drawer and see what comes back.


## The category, named correctly


The reason this distinction matters for the broader market is that "AI georeferencing" is currently sold as one category, and it is not one category. Homogeneous AI georeferencing is a useful product. It accelerates the part of the job that was already tractable. Heterogeneous AI georeferencing is a different product. It unlocks archives that were previously impossible to process at scale, full stop.


For most procurement teams, the second product is the one that actually matters. The clean modern scans were never the problem. The drawer of 1950s plats was the problem. The question is whether the vendor on the other side of the table has built a system that closes the modality gap, or one that politely declines to engage with it.


That is the conversation we think the category needs to have out loud, in those terms. If you want to walk through your own archive against this framework, our team is happy to do that on a live call —[get in touch](https://cal.com/james-monarcha/monarcha-enterprise) .
