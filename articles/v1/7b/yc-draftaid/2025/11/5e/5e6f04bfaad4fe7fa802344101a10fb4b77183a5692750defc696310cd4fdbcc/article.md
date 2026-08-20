---
schema_version: "1.0.0"
document_id: "5e6f04bfaad4fe7fa802344101a10fb4b77183a5692750defc696310cd4fdbcc"
company_key: "yc-draftaid"
company: "Draftaid"
source_id: "yc-draftaid-news-import-10f3d93d5772"
canonical_url: "https://draftaid.io/blog/assembly-drawings/"
published_at: "2025-11-19T15:51:56.512128+00:00"
first_seen_at: "2026-07-27T01:55:41.495149+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:7b3f29ef4c893b414ae821791e107f6bdde8556bf32f8f5fc57e750ed4cc1661"
---

# Assembly Drawing Types & Tips

Correctly prepared engineering drawings enable engineers, manufacturers and assembly teams to understand how a machine or a mechanism functions, how it is assembled, how to organize production and what the quality requirements are.


Assembly drawings are a subset of engineering drawings. They play a key role in illustrating how individual components come together to form a complete system or machine. Assembly drawings act as a bridge between part production and assembly.


## Types of Assembly Drawings


Assembly drawings can generally be divided into the following categories: general assembly drawings, assembly drawings, subassembly drawings and fabrication drawings.


General assembly drawing


The purpose of a **general assembly drawing** is to provide a general overview of how different parts or subassemblies are arranged in relation to one another. Such drawings are typically used to present an overall layout to clients, designers or production planners. Commonly, it includes an overview of the whole assembly in 2-3 views, with added general dimensions and a bill of materials.


An **assembly drawing** provides a more detailed representation, showing precisely how various components fit together. It includes positional relationships, key dimensions, and assembly instructions, and is mainly intended for engineers and assembly personnel. Detailed views zoom into some specific areas that require more info that is shown in the larger views.


Large assemblies that include hundreds of parts are often comprised of several subassemblies, depicted in **subassembly drawings** . That allows for quicker production because several teams can assemble different sections at a time, only to assemble the whole thing together in the last stage. And subassembly drawings just create much more clarity, as adding 5-6 detailed views, cutout views, etc. will not clutter the drawing yet. But it's something that could happen with a much larger assembly.


And finally we have **weldment drawings** . It is both an assembly drawing (includes 2+ parts) as well as a **fabrication drawing** (used as input in the manufacturing stage). Weldments drawings need to 1) clearly show the positional relationship of the different parts, using dimensions, 2) show where the weldments are going to be and 3) indicate the welding technology, quality class, etc. with the use of[welding symbols](https://draftaid.io/blog/weld-symbols-drawings/) .


Regarding point 1, it's better to design notches and holes that define the positioning of parts in a single way. That can reduce the fabrication time, need for fixtures, and most importantly, frequency of mistakes.


Assembly drawing types


## The Purpose


Assembly drawings play a crucial role in the engineering design and manufacturing process. They serve as the **primary link between conceptual design and practical implementation** , transforming theoretical ideas into physical products. Clear and accurate assembly drawings help speed up the whole production process, avoid errors and delays.


One of the key purposes of assembly drawings is to **visually communicate the relationship between components** . They illustrate how each part fits within the overall system, indicating connections, interfaces, and movement where necessary. This allows engineers, assemblers, and quality control teams to ensure that the final assembly meets both functional and dimensional requirements.


Assembly drawings are also essential for standardization and documentation. In many industries, such as automotive, aerospace and mechanical engineering they **serve as legal and technical records** that define the final product configuration. This ensures that all stakeholders, regardless of their location or role have a common reference for the design intent and assembly process.


Over the last few years, 3D models have started to accompany drawings more widely. Pretty much all drawings today are done in CAD software based on 3D models, so they exist anyway. Now, the use of tablets on the shop floor has risen dramatically, to aid the assembly and fabrication teams in getting a better understanding of the build. Mostly, it is still the luxury of in-house production teams, because sending out full assemblies to 3rd parties is still seen as a significant IP risk.


## Creating Assembly Drawings


When creating assembly drawings, it is essential to fully understand how the construction or system functions, and to define the purpose and audience of the drawing.


The requirements differ significantly depending on whether the drawing is intended for a client, designer, manufacturer, assembler or maintenance technician. The subject determines the level of detail, the number and types of views and the additional information to include.


### Planning and Preparation


First, **define the drawing's purpose and target audience** .


An assembly drawing for getting the customer approval, for example, will differ in scope and detail from one prepared for production.


A customer does not need much specificity in the drawings. A general assembly drawing that captures the function and general dimensions is usually enough. Adding and isometric and an exploded view may help, especially if the customer is non-technical. But the drawing should not include much more technical info, as it's just for conveying the big picture.


On the other hand, manufacturers need detailed drawings. The package will be mostly comprised of part drawings depicting detailed parts, with all the dimensions, tolerances, material info, etc. that will be fed into CNC machines. But detailed drawings are also necessary for the assembly team. Info on the assembly sequence where order matters, welding symbols, fastener specification are all important for getting the work done as it was intended already in the design stage.


At this point, the designer should:


- Review the design intent and the function of the assembly
- Identify which components will be included
- Determine the required level of detail
- Decide on the types of views and sections
- Establish the drawing scale and format according to company or industry standards.


Proper planning ensures that the resulting drawing is clear, consistent and fit for its purpose.


### Splitting into Subassemblies


Then, consider the possibility of **dividing your project into a few subassemblies** . A subassembly is a group of components that are first assembled separately before being integrated into a larger final assembly. They serve several important purposes:


- They simplify the overall assembly process by breaking down a complex system into manageable sections.
- They improve clarity in documentation, allowing each subassembly to have its own drawing, Bill of Materials (BOM) and revision history.
- They enhance collaboration between teams, for example: one group may be responsible for a hydraulic subassembly while another focuses on the electrical module.
- They facilitate maintenance and service operations as subassemblies can be replaced or upgraded without disassembling the entire machine.


In assembly drawings, subassemblies are usually represented as individual units with their own item numbers in the main BOM.


Each subassembly can have its own drawing and BOM, a unique drawing number or code, separate assembly instructions and in many cases an exploded view showing how its internal components fit together.


When including subassemblies in the main assembly drawing, it is important to clearly distinguish between purchased components, manufactured parts and subassemblies. To avoid different mistakes, maintain consistent revision control across all hierarchical levels and ensure all part numbers and subassemblies are correctly correlated with BOM. Clear graphical representation for showing subassemblies simplifies outlines or shaded groups to reduce visual clutter.


Subassemblies thus play a vital role in organizing complex designs and maintaining traceability between different levels of the product structure. In modern PLM (Product Lifecycle Management) systems, subassemblies are managed as independent, but linked digital objects, enabling seamless updates and synchronization across design, manufacturing and maintenance documentation.


### Bill of Materials - BOM


Once you know the purpose of your drawings, and whether and how you're going to divide the final assembly into parts, it's time to move on to the actual drawing. The aforementioned attributes help you decide on sheet size - what do you have to fit there?


And the first item on the sheet is the **Bill of Materials** .


Every assembly drawing includes it, in the bottom right corner. It's fundamental document that lists all components forming the assembly. The BOM supports purchasing, manufacturing and assembly operations by ensuring all required parts are identified and traceable.


A well prepared BOM includes item numbers which are linked to the drawing by using balloons and leader lines. Each part or subassembly is defined by a unique number or code to differentiate between them. The description does what it says - describes the part.


With very simple parts, the description can include the material specification and part dimensions, so they can be manufactured without a separate part drawing. For example, "S235JR - 50x5x1200 mm" tells us we need a 50x5 mm profile with a length of 1200 mm made from S235JR.


Additionally, quantities of each component are listed. This becomes the basis for purchasing standard parts and ordering custom manufacturing. Showing material specification is also option where it is applicable.


Exploded view and BOM


Sometimes the BOM table can become very long. In such situations it is advisable to place the BOM on a separate drawing sheet rather than reducing the size of the drawing views to fit everything onto a single page.


### Selecting the Views


**Selecting the right views is critical** . The chosen views clearly illustrate how the parts fit together and how the assembly functions.


Common view types include:


- **Front, top, and side views** - to show the general layout and proportions,
- **Sectional views** - to reveal internal details and hidden connections,
- **Detailed views** - to focus on small or critical areas,
- **Exploded views** - to demonstrate the order of assembly and the relationships between parts.


An effective assembly drawing includes the **minimum number of views necessary** to fully describe the structure and its function. Each view is logically placed on the drawing sheet to ensure readability and a natural visual flow. Views are aligned, properly scaled and labeled with clear titles.


The only exception is the **isometric view** that can be positioned pretty loosely in relation to the other main views.


[Section views](https://draftaid.io/blog/section-view-drawings/) reveal internal details and hidden connections. Full sections cut through the whole assembly to show everything in its path. Half sections show both internal and external features, and are mostly used with symmetrical assemblies. Offset sections follow stepped cutting planes to capture multiple features at once, that would otherwise need several section views to show.


Broken-out section expose specific internal areas without full sectioning. This is especially convenient if the area you want to show is pretty small. That can be combined with a **detailed view** to zoom into the area for better clarity.


Additional graphical elements such as center lines, reference planes and datums can help define the alignment and positioning of parts within the assembly. The goal is to provide a visual representation that is both technically precise and easy to interpret.


### Dimensioning


Assembly drawings do not need all the dimensions, as individual parts and the corresponding part drawings have defined most of it already.


The purpose of dimensioning in assembly drawings is to describe how different parts fit together, how they relate to each other, and how the movements in the system work. The **dimensions must ensure correct alignment and positioning** during assembly.


Every[engineering drawing](https://draftaid.io/blog/engineering-drawings/) shows the **overall size** of the part or assembly and assembly drawings are no exception.


**Center distances** between mounting holes or other critical features should be shown. These are directly related to assembly and will aid the team to find the right items to fit. Of course, all parts should come with part number markings but that is not always the case. So dimensional info can help with differentiating between parts.


Moving parts need extra care. **Dimensioning the clearances** for them is essential to ensure proper working conditions. Similarly, the **range of motion** (e.g. stroke length) needs more info on the technical drawing, so it can be accounted for.


Another dimension type to include is interface **dimensions affecting fit** with other assemblies (in case of subassemblies). For example, include engineering fit tolerances, e.g. H7, if the "shaft part" is in another assembly.


### Add Notes


Some information cannot be conveyed graphically. Therefore, assembly drawings often include **written notes** , that usually sit on top of the BOM.


Instructions like assembly guidelines will prove helpful during the assembly phase, speeding up the whole process by ensuring correct sequences and avoiding needless mistakes.


Notes can also include instructions that apply to all parts within the assembly. For example, the requirement telling to deburr all sharp edges of a weld assembly. Or notes about surface roughness.


All in all, notes should be used whenever there is no other way of communicating something important. But keep in mind to limit the length of the texts as much as possible.


Assembly notes above BOM


### Checking & Standard Compliance


Once the technical drawing is complete, it must undergo a thorough review to ensure accuracy, completeness and compliance with relevant standards.


This process prevents costly errors and maintains consistency across all engineering documentation.


Engineers should verify that all necessary dimensions, annotations, views and details are included. Item numbers correspond to the BOM, and part references match the latest revisions.


In this phase, it is important to check whether your technical drawing also **complies with relevant standards** , such as[ISO 128](https://www.iso.org/standard/65296.html) ,[ISO 1101](https://www.iso.org/obp/ui/#iso:std:iso:1101:ed-4:v1:en) or[ASME Y14](https://www.asme.org/codes-standards/y14-standards) . Compliance with international standards ensures global readability and reduces the risk of misinterpretation.


Also review that the title block includes the project name, project number, scale, author, checker, date, revision number and whatever else your internal company standard requires (the title block is almost always customized at a given company).


The final check means taking a step back and imagining a situation where you knew nothing about this project. What happens if you were receiving this drawing package? Can you assemble everything correctly based on these instructions (that's what those drawings are, essentially)?


Revisit the previous steps until the answer is a resounding "yes".


### Finalizing


After reviewing, the drawing is **approved and finalized for release** .


This includes administrative and technical steps to ensure the document is ready for production, assembly or maintenance.


Key finalization tasks include:


- Revision control and documentation of design changes,
- Approval signatures by responsible personnel,
- File naming and storage following company conventions,
- Exporting to appropriate formats (PDF, DWG, STEP),
- Linking the drawing to related parts, BOMs and 3D models in the[PDM](https://www.autodesk.com/eu/solutions/pdm-product-data-management) /PLM system.


Finalized drawings are typically distributed digitally, ensuring all stakeholders work with the latest and most accurate version.


## Conclusion


When creating assembly drawings, it is essential to understand how the mechanism or product functions, and who the intended user of the drawing is. This defines the necessary level of detail.


Follow international standards and best practices to guarantee your drawings are uniformly understood. Getting very creative can raise questions if the receiving end has been working with conventional drawings for the past 30 years.


Keep the amount of information minimal - add only what is necessary for assembly. Even if adding something is not technically wrong, additional clutter will just raise the probability of human mistakes.


Striving for clarity and minimalism while defining everything necessary will ensure quick and slick production, as well as less need for maintenance down the road.
