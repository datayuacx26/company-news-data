---
schema_version: "1.0.0"
document_id: "a078aba7a868452475fa4a38de2231861e0dd1c92e96e95da5021fcda86c22e9"
company_key: "yc-draftaid"
company: "Draftaid"
source_id: "yc-draftaid-news-import-10f3d93d5772"
canonical_url: "https://draftaid.io/blog/section-view-drawings/"
published_at: "2025-12-08T13:12:22.431744+00:00"
first_seen_at: "2026-07-27T01:55:41.495149+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:52146b36957fc22d7d210fe8d6bb26c1829833dec33f888f4406c2fcce8a391d"
---

# Section View Drawings - Uncovering What's Hidden

In the world of engineering, technical drawings function as the universal language that allows ideas to be materialized. Its main objective is to provide all the necessary information as clearly as possible to manufacture and assemble the parts represented in the drawing.


However, sometimes conventional views of an object fail to provide all the necessary information about some interior features. That's especially true when dealing with parts that have intricate internal geometries, cavities, grooves, internal holes, or overlapping components that are not visible in any of the orthographic views - front, top, and side views. This complicates dimensioning and can lead to misinterpretations and costly manufacturing errors.


Fortunately, a simple and practical solution exists - **section views** .


## What Is a Section View?


A section view is a graphic representation that shows the interior features and details of an object in an[engineering drawing](https://draftaid.io/blog/engineering-drawings/) . We achieved this by passing an imaginary cutting plane (indicated by the cutting plane line A-A in the image below) through the part.


The arrows show the direciton of our point of view. Everything that is "behind us", will not be shown on the section view drawing. Everything that the cutting plane cuts, will be hatched, so we can differentiate between empty space and sectioned material.


The main purpose of a section view is to provide clarity for manufacturing. In principle, all the interior features could be shown with hidden lines but this can create quite a bit of confusion. Section views reveal details - such as channels, holes, threads and other internal geometries - that are not visible in standard views without any of the mess of using hidden lines.


## Fundamental Elements of the Section View


### Cutting Plane


This is the imaginary plane that cuts through the object. This plane divides the item into two parts and its location is shown by the dashed cutting plane line, or section line.


The plane should always pass through the axes of symmetry of the internal elements to be observed. As already said, the arrowheads indicate the direction of observation.


The letters residing next to the arrowheads determine the name of the section view.


### Cut View Identification Label


Identifying the cut view is easy because of the last point of letters residing next to the arrowheads. In the example above, both arrowheads have the "A" notation.


Thus, the view will be named "Cut View A-A" or similar, with the name preferably on the upper left side relative to the view. The naming convention is important, so we can easily understand what is depicted. Sometimes the cut views may end up on a separate sheet, making this little addition especially important.


### Cut Surface


This is the part of the object's material that has been intersected by the cutting plane. This surface is the most prominent element in the drawing and is represented by hatching or shading. When reading and interpreting the cuts, it is important to bear in mind that:


- The hatched areas represent cut-through solid areas
- The unhatched areas represent cavities, holes, etc.


There are some exceptions to the last point which we will address later. But pairing the info above with other views in the engineering drawing will help with correct interpretation.


### Hatching


This is the main convention used to represent the areas cut by the cutting plane. Hatching is usually at an angle of 45 degrees to the horizontal. 60º and 30º angles are also quite common, their use mostly dependent on whether the hatching is parallel to the edge of the representation or not.


Sometimes in an[assembly drawing](https://draftaid.io/blog/assembly-drawings/) , two or more parts can be sectioned simultaneously. In those cases, we must differentiate the components by using different hatching angles or spacing. Each part must have a unique hatching direction, typically 45º and 135º are used, or similar.


As a general rule, we must keep in mind that **adjacent pieces should never have the same hatching pattern** , regardless of whether they are made of the same material.


### Materials


Most sectional drawings use a simple 45º hatching pattern. Meaning the sectional view itself does not say anything about the material, and is likely specified elsewhere - mainly the bill of materials.


However, there are conventions for representing specific materials. For example, aluminum is represented by two crossed dashed lines, wood by grain lines, etc.


A big reason for not following these rules is that a lot of companies just do not assign the correct material to each 3D part. Thus, when autogenerating drawing views, CAD programs have no info for assigning the right hatching per material. Often companies just differentiate between metals and non-metal by selecting different hatchings for steel, plastics, rubber, wood, etc.


Below we can see some examples of the most commonly used hatching conventions for representing different materials in the ANSI system. The latest ISO standard to address this is[ISO 128-3:2022](https://www.iso.org/obp/ui/#iso:std:iso:128:-3:ed-2:v1:en) .


## Types of Section Views


There are several options to choose from when creating section view drawings. They all follow the same common logic - show as much as possible while creating as few extra views as possible.


### Full Section View or Total Section


Full section is the most basic type of section view and therefore the most commonly used. In this type of section, the plane passes completely through the entire object, dividing it into two.


One of the parts is removed, allowing the interior of the part to be viewed with all the internal details on show. This full section view is ideal for symmetrical parts, where it reveals all the relevant information at a single glance.


### Half Section View


This type of cut could be considered a variant of the full cut. Its main difference lies in the fact that, unlike the full cut, the piece is not crossed by a single plane, but rather this plane is combined with a second cutting plane perpendicular to the first. This limits the final cutout to a quarter of the piece being cut.


This allows to simultaneously show both the exterior as well as the interior of the piece in its full length. Symmetrical objects like the one above are ideal for that, allowing us to save views and space on the engineering drawing.


### Partial Cut or Broken Out Sections


The broken out sections are especially useful when only a small part of the interior of the piece needs to be shown. In such a case, a full cut would be excessive and unnecessary.


With a partial cut, we can create a localized break precisely in the area we want to show. The broken out section is delimited by a thin, irregular line that simulates a break in the piece. This type of cut is very efficient for showing, for example, the depth of a blind hole, a small cavity, or a groove without having to cut the entire piece.


Broken out sections often depict small areas of a larger view. Creating a separate detailed view of the broken out area can help with accommodating all the details and dimensioning.


### Offset Section


The offset section is one of the most efficient types of section views for showing complex parts. It is especially useful when the internal elements to be shown are not aligned on the same plane, meaning the cutting plane line would only pass through some of them.


The skew or offset section solves this by using a cutting plane that is not a straight line. That is, the cutting line is skewed to pass through the axis of symmetry of all the elements to be shown.


The section will still looks almost the same, discounting for the "steps" that are clearly visible as vertical lines. The sideview of these steps is not included in any way, as the viewpoint is again determined by the arrowheads.


### Revolved Sections


A revolved section view in engineering drawings shows an object's internal shape by rotating a cross-section 90 degrees and placing it directly within the main view.


It is often used for thin features like spokes, ribs, or arms to save space and clarify details without creating a separate view.


Revolved sections are created by imagining a cutting plane that rotates, showing the material's profile where it's cut, usually indicated by a centerline and section lines.


In this example, we can see that the cut is represented in the same view as the part, as an aid to better appreciate the cross-section of the connecting rod. Note that the cutting plane is not parallel to the axis of symmetry, but perpendicular to it.


Sometimes, when the details of the drawing require it, we can make a broken out section of the piece around the revolved section to assure more clarity.


To note, revolved sections are pretty uncommon. So even though they're standardized, using them might still create confusion for someone reading the drawing because of not coming across them that often.


## Elements to Omit


One of the most important rules to keep in mind regarding section drawings is that not everything that the cutting plane intersects should be hatched.


There are elements that, by their nature, are drawn without cutting so as not to distort the understanding of the part. These include:


- Ribs (fins). If the cutting plane passes longitudinally through a rib (a thin reinforcement element), it is not hatched. Doing so would give a false impression of mass and thickness. It is drawn without hatching, as if it had not been cut.
- Radials of gear wheels or pulleys
- Axles, screws, pins, and rivets. These solid connecting elements are not sectioned, as they do not provide relevant internal information and their massive hatching would hinder the reading of the plan.
- Arms of overhanging parts


## Conclusion


Most of the drawings in the average package are usually pretty simple. Anything sheet metal, for example.


As soon as we enter the realm of CNC machining, assembly drawings, weldment drawings, etc., the drawings get exponentially more complex. At the same time, these drawings are often the only form of communication between the drawing room and the manufacturing floor.


So conveying detailed information about these complex parts or structures in a way as simple as possible is essential to avoid confusion, unnecessary communication, delays and errors.


Section views are surely one of the best tools in an engineer's cabinet to ensure that kind of clarity by shedding light on the hidden details.
