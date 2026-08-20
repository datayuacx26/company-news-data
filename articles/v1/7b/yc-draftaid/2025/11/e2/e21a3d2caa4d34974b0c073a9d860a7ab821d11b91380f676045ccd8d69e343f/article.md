---
schema_version: "1.0.0"
document_id: "e21a3d2caa4d34974b0c073a9d860a7ab821d11b91380f676045ccd8d69e343f"
company_key: "yc-draftaid"
company: "Draftaid"
source_id: "yc-draftaid-news-import-10f3d93d5772"
canonical_url: "https://draftaid.io/blog/engineering-drawings/"
published_at: "2025-11-14T10:46:59.303384+00:00"
first_seen_at: "2026-07-27T01:55:41.495149+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:42aa903896713a8cc13ac67aa04626e4783d5ceda99ef98106157f185998effc"
---

# Engineering Drawings - a Complete Guide

Engineering drawings are the bridge between design and manufacturing. They use a shared language of lines, symbols and notation to convey the design intent in a standardized way.


Although CAD programs are able to generate all the views, it is essential to understand which views to include and how to lay on all the necessary information concisely.


Good drawings offer a single way of interpreting the info they entail. Poor drawings create needless back-and-forth communication, lost time and production errors.


Thus, we will go over all the basics to create a baseline for understanding all the key elements - from standards, views and common symbols to dimensioning and tolerancing.


## Engineering Drawing Standards


Standards prevent miscommunication across industries and borders. The two main standards are by **ISO** (International Organization for Standardization) and **ASME** (American Society of Mechanical Engineers).


ISO standard is the more widespread, dominating in Europe, Asia and most of the world. ASME standard prevails in North America.


The main differences are in the method of projection (first angle in ISO vs third angle in ASME), units (metric vs imperial), some symbols and slight variance in the approach to GD&T.


Any drawing must clearly state which standard is applied, typically with a note in the title block. And the chosen standard should be followed throughout the drawing, avoiding any mixing of the two.


Most companies use a single standard throughout all drawings but knowing the differences is still essential for international businesses.


## Sheet Selection


The main **standard for sheet sizes** is ISO, with the most common sizes for engineering drawings being everything between A4 and A0.


It is important to note that[there are a lot of different standards](https://en.wikipedia.org/wiki/Paper_size) , and depending on the regional or company preferences, ANSI paper sizes are also pretty common.


**Scale** indicates the ratio between drawing size and real-life part size. Full scale (1:1) shows parts at actual size, meaning you can use a ruler to measure the drawing and get the actual dimensions. Reduction scales like 1:2 shrink large parts to fit the sheet. And enlargement scales like 2:1 magnify all parts.


The drawing scale is in the title block. Sometimes different views on the drawing use various scales, e.g. a detail view can be an enlargement of 2:1 while the rest of the drawing is 1:10. In such cases, the specific view must note its scale.


## Types of Drawings


The aim of the drawing is to convey design intent and other info as simply as possible, without any extra clutter. Drawings come with a variety of different purposes, from helping to produce the parts to helping put the whole assembly together. Thus, we have a range of engineering drawing types that fulfil slighly different functions.


### Part Drawings


Part drawings say everything necessary about a single part.


They are detailed drawings - fully dimensioned and toleranced. Usually, there is also info on the surface quality, notes on heat treatment and coatings, as well as part material.


Parts are mostly shown in the 3 main views, adding an isometric view for the fourth.


### Production Drawings


Flat pattern drawing


The info presented on the production drawing depends on the level of automation in manufacturing as well as the process. Most of the machines out there today are CNC and can read the data straight from vector drawings.


For laser or plasma cutting, it means the flat 2D part and only the outlines. Any dimensioning, use of center lines, etc. must be removed. Some of the part info will still be present in the title block. Also,[sheet metal drawings](https://draftaid.io/blog/sheet-metal-drawings-guide-machinists/) require an additional view of the flat pattern to include important info for cutting and bending.


Bending requires the bending sequence and positioning of bending lines with annotations about the radii. The drawing above gives all that besides the bending sequence which is not critical in this specific case.


CNC machining jobs are largely automated with CAM that creates the toolpaths based on the 3D model. Manual checks will be performed based on the part drawings.


So ideally, all parts would have both the production drawing as well as the part drawing accompanied for manufacturing.


### Welding Drawings


Welding drawings are also for production but we can categorise them separately. They must show the exact relative positioning of the assembly parts. Ideally, positioning features like notches determine the fits in a single way.


In that case, the only dimensions necessary are general dimension. Part dimensions are often defined in the **Bill of Materials (BOM)** to ensure the avoidance of any mixups during assembly.


Of course, all necessary info on welding must be present - applicable welding standards/codes, quality level or inspection class, weld type, etc. mostly conveyed by using the[different weld symbols](https://draftaid.io/blog/weld-symbols-drawings/) .


Coating info can also be added to the welding drawing if this is the next process in line and done with the help of the same manufacturing partner.


### Assembly Drawings


Assembly drawing


[Assembly drawings](https://draftaid.io/blog/assembly-drawings/) show how the parts fit together, aided by general dimensions and part ID numbers that can be traced back to the BOM.


Clarity here is essential, so the assembly team can move quickly without putting much effort toward figuring out how to solve something.


All the fasteners should be included in the drawing as well as the BOM. The purchasing department will use the info on those drawings to ensure all necessary components get ordered.


In case there are some difficult areas, cutout views and detail views can help with the legibility of the drawing. Exploded views are also common to highlight the sequence of installing as well as the proper relation of all parts to each other.


### Installation Drawings


Whereas assembly drawings outline everything to get the assembly right, installation drawings help get the finished product installed properly on site.


It shows the relation of the, for example, industrial conveyor to factory facility. It identifies the foundation size, height and all features to account for anchoring possibilities, bolting locations, connections to utilities like electrics and piping, etc.


All in all, installation drawings must include all necessary info so the field construction crew can set everything in place and in working order.


## Line Types


Any engineering drawing is made up of lines. Different-looking lines carry specific meanings and are applicable in certain situations.


**Continuous lines** represent all visible edges and outlines. Traditional engineering drawing principles say that outlines should use thicker lines, and inner lines thinner but CAD software mostly use the same thickness everywhere.


**Dashed lines** , or hidden lines, show part features that would be otherwise invisible in the view. However, dashed lines can also cause quite of bit of clutter. Once applied, the view may be overburdened with lines everywhere, making the whole very messy. So cutout views or other projections are usually the preferred option.


**Center lines** mark axes of cylindrical features like holes or pins, or parts like shafts.


**Phantom lines** show either other parts and how they are positioned relative to the main item in a part drawing format or alternative positions of an assembly, e.g. open and closed position of a valve.


**Extension lines** are two parallel lines that extend from part features for clarity and leaving room between the part and the info.


**Dimension lines** show the extent of the feature being measured. They have arrowheads usually at both ends. For linear dimensions, the value sits on top of the line.


**Leader lines** have an arrow pointing toward a feature, and text, symbols or other annotation on the leader to convey relevant info.


**Cutting lines** show the position of the cutting plane that creates the cutout view.


**Hatching lines** are used in cutout views to denote the cut solid material. The hatching differs based on the material, and more common materials like metal, concrete, etc. have dedicated hatching styles.


**Break lines** show the start and end of the break in broken views. Everything that falls between the start and end is not shown on the drawing.


## Drawing Views


Engineering drawings use 2D views to give all the necessary input for the next stage in the production cycle about a 3D object. Accomplishing that requires a selection of different views to represent the part or assembly in the most efficient manner while not losing any accuracy.


### Orthographic Projection Methods


**Orthographic projection methods** enable the depiction of 3D items in 2D without losing accuracy. The objects do not convey perspective, like they do in real life or in artistic paintings. Thus, there is no relative distortion and any line can be measured with a ruler and multiplied or divided by the scale.


There are 2 versions to of orthographic projections to choose from.


The ISO standard uses **first angle projection** . The central view is the front. And the part is then rolled around or flipped to show other sides of the part in a multiview drawing. Meaning the top view is under the front view.


The ASME standard uses **third angle projection** . Again, the central view is the front. And as the standards must definitely differ from each other, everything else is inverse compared to the European standard. Meaning the whole logic of turning and flipping works the other way around, and the top view is on top of the front view.


### Types of Views


After the projection method is clear, it's time to lay out the **primary views** . There are six of them - front, top, right and left side, bottom, rear. They show the object from perpendicular viewing angles. Most drawings only use 2-3 primary views - front, top and either of the side views - as this is commonly enough for fully defining the part without any redundancies.


**Isometric views** show the part in 3D without dimensions. They help manufacturing teams visualize complex geometry before reading the dimensioned orthographic views. For simple parts like flat plates, the isometric view is not really necessary.


[Section views](https://draftaid.io/blog/section-view-drawings/) cut through any objects to reveal internal features. The cutting lines on the main view show exactly what has been cut and where. The cut can use a single plane or add some steps to include features that would otherwise require several section views, as they don't lie on the same plane.


A small cluster can sometimes include most of the complexity a part entails. **Detail views** allow enlarging of such areas to improve the legibility of technical drawings.


**Auxiliary views** add a perpendicular view of an otherwise angled surface. They display the true size and shape of surfaces that are distorted in other views.


Long parts with uniform cross-sections and repetitive feature patterns get downsized with **broken views** that essentially cut out a large portion of the part. One objective is to save space while also naturally enlarging some of the other features that can now be shown without using detail views. It's important to note while this is a common practice, broken views should never be used on production drawings because the CNC machines need the whole part geometry as input.


**Exploded views** separate assembly components along an axis to show the sequence of assembly as well as all the parts with clarity.


## Dimensioning


Dimensions are an essential part of all technical drawings, excluding production drawings as discussed earlier. They define the size and location of features.


Over-dimensioning is the most common beginner mistake. Adding every possible dimension creates cluttered drawings where critical measurements get lost. A part with 50 dimensions is harder to manufacture correctly than one with 20 well-chosen dimensions.


A few pointers regarding dimensioning:


1. Follow hierarchy in the **dimension placement** . Smaller dimensions are closer to the part, larger dimension further out. This way you avoid crossing of extension and dimension lines.
2. Avoid **dimension chains** . Not everything can be made exact to size. So if there is a dimension from A to D, do not add all the intermediary dimensions - A to B, B to C and C to D. One of them must be "left open", if A to D is given anyway. Leave the least important one open.
3. Following the last sentence, **functional dimensions** take priority. Some dimensions are just more important than others, most often its ones that determine how well a part mates with another in an assembly.
4. Avoid **duplicate dimensions** . If one view includes a dimension, do not include it in another view. For example, if the front view has the width shown, do not show it again in the top view.
5. Check for **completeness** . Once the drawing is done, check that all dimensions can be either read straight from the drawing or derived from existing dimensions.


## Tolerancing


Manufacturing processes have inherent variability. A CNC mill might hold ±0.05 mm, while laser cutting typically achieves ±0.1 mm. **Tolerances define acceptable variation** .


Most engineering drawings include a block that defines allowable tolerances for all untoleranced dimensions - **general tolerances** . With ISO standards, it's commonly[ISO 2768](https://www.iso.org/standard/85741.html) with a specification about the tolerance class that applies. General tolerances can define tolerances for linear dimensions (like the table above), angular dimensions, straightness and flatness, perpendicularity, etc.


More critical features may need specific tolerancing. In those cases, the tolerance is added next to the nominal dimension. It defines both the **upper and lower deviations** . In case of **bilateral tolerances** , the allowable deviations are the same size in both directions.


Holes and shafts often make up critical pairs in terms of ease of assembly, function and longevity. So there's a whole separate tolerancing system called **limits & fits** for that.


Limits & fits helps to create parts with specific tolerances that pair up well, keeping the use-case in mind. They do not display any upper or lower deviations but use shorthand like 25H6 (meaning Ø25 +0.000/-0.013 mm) or 40g7 (meaning Ø40 -0.009/-0.034 mm) that is widely understood within the industry.


**Geometric dimensioning and tolerancing** is another big area of the overall tolerancing system. GD&T can get pretty complex but is also indispensable for high-toleranced parts that are made using manufacturing processes that enable great accuracy, like machining or turning.


GD&T provides an opportunity to define different features in a more precise way, adding many more options compared to regular tolerancing, like controlling form, orientation, location and runout of features.


## Common Symbols in Technical Drawings


Symbols are part of the common language to convey info about engineering projects as concisely as possible.


**Ø** - diameter. Precedes dimension values for cylindrical features like holes and shafts.


**□** - square feature. Can be used to indicate a square feature on a drawing, like a square tip of a shaft that would otherwise not be evident from the side view.


**R** - radius. Indicates arc dimensions with the arrowhead touching the arc and leader having the dimension.


**⌴** - counterbore hole. Indicates a counterbore hole with added diameter and depth.


**⌵** - countersink hole. Indicates a countersink hole with added diameter and depth.


**↧** - depth. Defines the depth of a feature like hole.


**Ra/Rz** - surface roughness. Ra is the arithmetic mean roughness, Rz is the mean roughness depth.


**Thread notation** - specifies fastener threads using standardized callouts like M8 (metric).


**Welding symbols** - communicate joint type, weld size, and process using standardized notation on a reference line with an arrow pointing to the joint.


## Information Blocks


Information blocks provide essential info that is missing from the geometric representations. They are somewhat standardized but usually company-specific, as any engineering drawing must only contain the essentials which differs between companies and industries.


**Title block** sits in the bottom right corner. It includes the part or assembly name, part or assembly number, material, scale, projection method, applicable standards, company info, the engineer's name and sometimes the company logo.


**Revision table** is usually in the upper right corner and tracks drawing changes over time. Every revision receives a letter or a number, the date of the update and description of changes. The idea is to prevent using old drawings as well as highlighting the changes made. Sometimes a part has been made by the same manufacturer for years, and proper annotations help noticing the changes.


**Bill of materials (BOM)** often sits right on top of the title block and lists all the components in an assembly - item numbers, part names, part numbers, quantities and materials specs. The item numbers are used to show the corresponding parts in the drawing with balloons.


**Tolerances block** specifies the general tolerances that apply to all dimensions that have no other tolerances specified. It usually includes linear and angular tolerances.


**Notes section** describes everything that is not covered by the engineering drawing itself or any of the info blocks. These include comments about finishing, inspection requirements, manufacturing instructions, etc. These are general notes that apply to the whole drawing as opposed to local notes attached to specific features.


## How to Make a Great Engineering Drawing?


Let's go step-by-step, using the basics we learned here in the article in the same sequence.


**Choose the standard**


We started with the standards, and touched upon them in multiple instances later on. The most logical basis for choosing the standard would be the region your technical drawings will be used in. If you make drawings for US manufacturers, go with ASME. If you and your partners are in Europe, ISO is the way to go.


**What are you depicting?**


Is the drawing for giving all info for part production, quality control, assembly, welding, or something else? Think about that before putting anything on the drawing. You can then visualize the layout, what has to be shown and choose the right sheet size and scale for the drawing based on the former.


**Add the essentials**


All the main info blocks will have to be in place. This again helps you better understand the layout and make use of the space available to you. Title block must be on every drawing, others like BOM, tolerances, notes, revisions, etc. depend on the type of engineering drawing you're making.


**Add the views**


Put down the minimum number of views necessary to give a complete overview of the object. With flat parts, this may be only the top view with the sheet thickness marked down in the title block or with a callout.


More complex parts or assemblies may need the addition of several views, like cross sectional views, detailed views, etc.


**Dimensioning**


Place dimensions where they make sense. Critical features that will have to be checked during quality control have to be dimensioned, not derived from other dimensions.


Add tolerancing where large fluctuations in size may disrupt the functionality of the parts or negatively impact the part's lifetime. At the same time, be as loose as possible to keep the manufacturing costs minimal. Consider the accuracy your chosen manufacturing process can guarantee when going about tolerancing.


Check if all the dimension callouts are clearly legible. Does the fully dimensioned engineering drawing tell me the positions and sizes of each critical feature?


**Missing info**


Is there any welding info that should be present? What about surface finish requirements or heat treatment?


Add notes if something cannot be laid out with visuals only.


**Final check**


Maybe you've worked on this project for 2 months, or a year, and every nuance about it is self-evident to you. But the people who will be working based on your technical drawings will not have that luxury.


Put yourself in their shoes, as someone who is seeing that drawing for the first time. You may not even know its use, how and what it assembles with, etc. Try to reconstruct the part in your head only based on what you see in the drawing.


If this is possible, you're ready to send the drawings out for manufacturing.


### Automating Technical Drawings


Companies tend to work out their own way of making drawings over time. The basics will overlap with the standards, but close cooperation with the same suppliers over time renders some of the standard info useless and vice versa.


It's possible to use a[drawing automation tool](https://draftaid.io/) that learns your company-specific approaches and generates the necessary engineering drawings automatically based on 3D CAD files.


Contemporary technology has come a long way from in-CAD automated drawings that tend to be quite useless (it's usually quicker to make a drawing from scratch than use those even as a basis). So make use of the options available to you to speed up your processes while maintaining high quality.
