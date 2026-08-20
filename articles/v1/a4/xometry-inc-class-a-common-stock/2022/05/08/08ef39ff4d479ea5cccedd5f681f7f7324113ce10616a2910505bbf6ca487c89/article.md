---
schema_version: "1.0.0"
document_id: "08ef39ff4d479ea5cccedd5f681f7f7324113ce10616a2910505bbf6ca487c89"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/sheet/nesting-files-for-sheet-cutting/"
published_at: "2022-05-25T11:00:12+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:29:12.077419+00:00"
content_hash: "sha256:4840042c5fbcaddb3aef966a363e3cdc0cdac805d8ea15556bda706f92552f6b"
---

# Nesting Files for Sheet Cutting

## What is Nesting?


In the manufacturing industry, the term "nesting" refers to the process of laying out multiple parts on a single sheet. The nesting process reduces scrap waste and optimizes production. There are also potential savings by submitting a nested file which we will look at in more detail further down this article. You can use dedicated CAD software to automatically nest your parts and export a DXF file, a preferred format for our **[Sheet Cutting service](https://www.xometry.com/capabilities/sheet-cutting/)** . If you have a nested file you're looking to quote, you can upload it straight to the **[Xometry Instant Quoting Engine](https://www.xometry.com/quoting/home/)** , which natively supports DXF files, including ones that have nested parts!


## When Nesting Matters


Whether you should nest your parts or not will be highly dependent on your part geometry. We will use the examples below to help demonstrate when nesting makes a difference and when it does not. First, let's take a look at an example of parts that **do not** benefit from this process.


Diagram of rectangular parts spread across a 31 inch by 15 inch area


This design has four rectangular parts in a 31.4-inch by 15-inch area. If we pick stainless steel 304 at 0.75 inches thick, we get a single set price of $247.90. Now let's see what happens when we nest these parts even closer together.


Diagram of closely nested rectangular parts


Now almost all of the space between the parts is removed, and they fit within an area of 17.5x7.1 inches. With this change, the price stays the same at $247.90. You may wonder why this change did not make a difference. It has to do with the way our pricing algorithm analyzes the parts to calculate material usage. In short, if the bounding boxes of the individual components cannot occupy the same space, there won't be any benefit. Let's look at an example where the component bounding boxes intersect.


### Become an Expert in All things Sheet Metal Fab


[Download the FREE Guide](https://www.xometry.com/resources/design-guides/design-guide-sheet-metal-fabrication/?utm_campaign=xom_technical_content)


Diagram of parts where bounding boxes intersect


In the example above, the bounding box of the u-shaped part is highlighted in yellow, and the other part's in purple. As you can see, when the components are nested, the enclosing boxes share the same area in the middle, which reduces the material usage overall. When the parts are separate, the total area is 288.5 square inches versus 195.5 square inches when nested. This brings the cost from $479.10 for a set of the parts made in 0.75 thick stainless steel 304 down to $366.80, respectively.


### Quote your Laser or Waterjet project


[Get Your Instant Quote](https://www.xometry.com/quoting/home/?processInitialisms=Sheet+Cutting)


## Nested Files With Xometry


As mentioned at the beginning of this article, Xometry supports quoting nested parts for sheet cutting services. Quoting using an adequately prepared DXF file is the ideal method for quoting nested parts. For tips on preparing your DXF file for sheet cutting, feel free to read **[our post](https://www.xometry.com/resources/sheet/preparing-dxf-files/)** on the subject! When quoting nested parts, there are a few advantages and considerations. Please refer to the lists below:


**Advantages:**


- Get multiple components quoted under one line item.
- Avoid uploading and configuring multiple parts.
- Configure your material, finish, and other options with one line item.


**Considerations:**


- Nested parts must all be the same thickness, material, and finish.
- Please do not nest parts if formalized inspections (e.g., Formal Inspection with Dimensional Report). You will need to upload a drawing for each component for inspection purposes.
- We need a 0.25-inch margin around all edges and a 0.125-inch space between each part for waterjet cutting. We need a 0.25-inch margin and 0.03-inch spacing between each part for laser cutting.
- To prevent the loss of small nested components, we recommend adding breakaway tabs for pieces under 2 inches or ones with delicate features.


## Conclusion


In summary, nesting sheet cut parts can be a great way of simplifying your order and reducing material waste, saving on cost. You can use CAD software to automatically nest components and export the file as a DXF. You can get a quote by uploading your nested DXF file to the **[Xometry Instant Quote Engine](https://www.xometry.com/quoting/home/?processInitialisms=Sheet+Cutting)** . With our sheet cutting services, you can configure your order from a wide array of materials, finishes, and feature options.


**Start Your Instant Quote**


STEP · STP · SLDPRT · STL · DXF · IPT · X_T · X_B · 3DXML · CATPART · PRT · SAT · 3MF · JT


[Upload Your Design](https://www.xometry.com/quoting/home/) All uploads are secure and confidential
