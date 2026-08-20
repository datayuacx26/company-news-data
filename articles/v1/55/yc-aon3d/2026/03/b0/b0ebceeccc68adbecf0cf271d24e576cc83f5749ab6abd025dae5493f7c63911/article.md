---
schema_version: "1.0.0"
document_id: "b0ebceeccc68adbecf0cf271d24e576cc83f5749ab6abd025dae5493f7c63911"
company_key: "yc-aon3d"
company: "AON3D"
source_id: "yc-aon3d-rss-fb9d9c5b4952"
canonical_url: "https://www.aon3d.com/applications/dfam-guide-joining-methods/"
published_at: "2026-03-11T19:52:27+00:00"
first_seen_at: "2026-07-26T22:38:07.846328+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:efc275591e56cf94da4a474c49969976d24449195b7ffcd5e0e70d3b59ab5883"
---

# Joining Methods for 3D Printed Assemblies: Threads & Inserts

\[PhD-Reviewed\]


Industrial 3D Printer Buyers Guide


Over 77% of printers marketed for high performance polymers can’t reliably print PEEK or ULTEM. Make your next investment with confidence, not consequences.


[Download Now](https://cta-service-cms2.hubspot.com/web-interactives/public/v1/track/click?encryptedPayload=AVxigLKtcCrWtpf5k1LijSMDjT4pe1612P7SaFHJMt%2B%2FC4fFYofS9OIgKnS7ylLNWhymXLObm6BUeowUWOefhyuQOIQGX5j7dG9hiDhDhcFebeCvloAdICQFjrvyZ3RMV1IlCiajRgIHzaVjlXEp9nOKRWrLYUErjEsMymR%2FzMkDTc5r5LmPZxdN3Z%2FgYyEayQe70jO0GyLRDC%2BlyQBKPV3a&portalId=4776083)


This article is a part of series, AON3D’s Ultimate DFAM Guide. For additional tips and tricks also see:


- [How to Design & 3D Print Stronger Parts](https://www.aon3d.com/material-science/how-to-design-3d-print-stronger-parts/)
- [Engineering Fits: How to Design for 3D Printed Assemblies](https://www.aon3d.com/applications/engineering-fits-how-to-design-for-3d-printed-assemblies/)


Designing reliable joints is a critical part of creating functional 3D printed components. While it may be tempting to rely on adhesives or fully printed threads, these approaches often fail under load or repeated use due to the mechanical limitations of thermoplastics. To achieve durable, high-performance assemblies, it’s important to consider alternative fastening strategies. This guide outlines common methods for creating threaded connections in 3D printed parts—and when to use each.


### 3D Printed Threads


While not impossible to print, **thermoplastics are generally not recommended for threads** , as they are considerably weaker than metals and can easily strip from overtightening. If you choose to 3D print threads, we recommend only doing so for M8 threads or larger and using a polymer with high tensile strength, high tensile modulus, and good elongation at break. For threads smaller, less than M8, design a hole slightly smaller than your bolt/screw, add additional walls in your print settings, and tap the threads after the part is printed. Even then, these options should be not trusted for anything above light-duty applications.


#### Rib Thread Forming


An alternative to tapping parts is to use[crush ribs](https://www.aon3d.com/applications/engineering-fits-how-to-design-for-3d-printed-assemblies/) , a concept discussed previously. Once printed, the screw is used to deform the crush ribs, creating its own threads in the process.


### Embedded Nuts


Another method to join parts is to embed a nut within the layers of a print or on the opposite side of the print. While this method provides a strong hold, it can sometimes be frustrating trying to keep the nuts held in place while assembling your parts.


### Heat-Set Inserts


Source:[LulzBot](https://lulzbot.com/learn/tips-and-tricks-heat-set-inserts)


One of the best methods for adding threads to a 3D printed part is to use heat set inserts, which create permanent threads that provide a strong hold. To use heat-set inserts, follow these step-by-step instructions:


1. Create a counterbore hole with the minor diameter slightly larger than the minimum outer diameter of the insert (See the table below). Adding a small chamfer to the hole allows the insert to seat itself during installation.
2. To accommodate for misplaced material, as the insert is heated and pressed down, make the hole depth at least a few mm deeper than the insert, up to 1.5x the depth of the insert.
3. Use a soldering iron that can reach roughly 350-400°C and we strongly recommend using an installation tip that can be purchased from McMaster-Carr ([example](https://www.mcmaster.com/92160A115/) ). Lastly, seating inserts perfectly straight can be difficult by hand, so we recommend using a heat set insert press. Instructions for building a press can be found readily online, some can even be fully 3D printed.
4. When installing, press the insert roughly 90% into the part. Quickly flip the part and press the part down firmly on a flat surface to set the insert flush with the face of the part.


A small chamfer is added to the opening, allowing the insert to seat itself during installation while additional depth is added to the hole to accommodate misplaced material.


### Embedded Magnets


Magnets are a great solution for modular assemblies, enclosures, and configurable tooling. Magnets can be embedded via press fits, concealed within layers, or glued in, although glues can be messy and don’t always hold well depending on your part’s material.


When adding magnets via press fits, we recommend using[relief features](https://www.aon3d.com/applications/engineering-fits-how-to-design-for-3d-printed-assemblies/) to ensure a snug, mostly permanent fit. Alternatively, you can design a cavity slightly larger than your magnet, 2-3 layers below the surface of your print. Most slicers contain a “pause at layer” feature which automatically pauses the print at the correct height, allowing you to insert your magnets before pressing resume and encapsulating the magnets. Keep in mind that concealing magnets within layers may slightly reduce their holding power, so you may want to add a few additional magnets if using this method.
