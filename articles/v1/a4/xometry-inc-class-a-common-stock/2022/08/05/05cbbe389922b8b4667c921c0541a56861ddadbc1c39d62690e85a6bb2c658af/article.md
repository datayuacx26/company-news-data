---
schema_version: "1.0.0"
document_id: "05cbbe389922b8b4667c921c0541a56861ddadbc1c39d62690e85a6bb2c658af"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/3d-printing/what-is-infill-in-3d-printing/"
published_at: "2022-08-23T04:00:21+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:29:12.077419+00:00"
content_hash: "sha256:a3760217f454a0fd02892140dad295c2664ef493033fcfe0d8ee124b2e639175"
---

# Infill in 3D Printing: Pattern and Types

### What is Gyroid Infill?


Gyroid infill is a complex, organic three-dimensional pattern used in 3D printing that forms a continuous, interwoven lattice structure. Gyroid infill's unique design provides more uniform strength distribution compared to many planar infill patterns, making it durable and efficient. The structure of the Gyroid pattern resembles a network of curved surfaces that create interconnected channels, ensuring stress is distributed evenly across the part. The characteristic makes it ideal for parts that require strength and flexibility. Gyroid infill is used in applications where lightweight and durable components are needed, such as automotive and aerospace parts, and in some non-implant medical device prototypes. Gyroid infil is effective when the part is subjected to varying stresses in multiple directions, providing an optimal balance of material efficiency and performance.


### What is the Best Infill Density for 3D Printing?


The best infill density for 3D printing is between 20% and 50%, depending on the specific requirements of the part. A 20% infill density is suitable for decorative parts, prototypes, or non-load-bearing components where print time and material efficiency are prioritized over strength. An infill density of 30% to 50% offers a good balance between strength, material usage, and print time for functional parts that require moderate strength, such as enclosures or brackets. Higher infill densities, ranging from 60% to 100%, are recommended for parts exposed to heavy loads or mechanical stress (structural supports, tool handles, or mechanical components). The density must be selected based on the part’s function, required strength, and available material and time budget. Reducing infill density helps save on material costs and print time, while increasing density improves durability and strength.


### Where Does the Ideal Value of the Infill Percentage Depend?


The ideal value of the infill percentage depends on part function, material type, weight, print time, and load-bearing requirements. The function of the part determines the necessary level of internal support. For example, a decorative part requires a low infill percentage, while a functional component subjected to stress needs a higher rate for durability. Rigid materials like PLA require less infill, while flexible materials like TPU need more to ensure stability. Weight influences the decision, as lightweight parts benefit from lower infill percentages, whereas heavier parts require higher infill to maintain strength. Print time is directly impacted by the infill percentage, with more infill leading to longer print times, which matters for time-sensitive projects. Load-bearing requirements are crucial. Parts subjected to significant forces (mechanical components or structural supports) require higher infill to prevent failure. For example, a lightweight bracket has a 20% infill, while a load-bearing tool handle needs 70% or more.


### What is the Recommended Infill Percentage for Other Simple Exposure Objects?


The recommended infill percentage for simple, non-load-bearing objects ranges from 10% to 20%. The objects, which are not subjected to significant stress or heavy loads, benefit from a lower infill percentage to maintain a balance between material efficiency, print speed, and strength. Simple exposure objects (decorative items, non-load-bearing enclosures, or lightweight fixtures) do not require the high durability found in functional or structural parts. A 10% infill is sufficient for items that serve an aesthetic purpose, while a 20% infill provides extra durability if needed, without affecting print time or material usage. The range ensures that the parts are stable and adequately supported, without the added costs and time associated with higher infill percentages.


### What is the Fastest Infill Pattern?


The fastest infill pattern is the Line pattern because it uses straight lines running in a single direction, requiring fewer movements of the print head and less material. The simple structure results in faster print times compared to more complex infill patterns, which require additional paths and intricate movements. The Line pattern is ideal for parts that do not require high strength or are not subjected to heavy loads. Speed is prioritized over strength in scenarios where the part is a prototype, decorative item, or non-functional component, with the primary goal being to reduce print time and material usage. The Line pattern is effective for lightweight parts, where structural integrity is not critical. The Line pattern offers a fast and efficient solution when time and cost are prioritized over durability (in creating test models or visual mockups).


### What is the Best Infill Pattern for 3D Printing?


The best infill pattern for 3D printing depends on the specific requirements of the part (strength, material usage, and print time). The Line pattern is the fastest and most material-efficient, but offers limited strength, making it ideal for decorative parts or prototypes that do not need to bear significant loads. The Gyroid pattern offers more uniform strength distribution across multiple directions. Gyroid pattern is an excellent choice for parts that need high durability and flexibility (automotive or aerospace components). The Concentric pattern uses circular layers that align with the outer perimeter, offering smooth surfaces and moderate strength, making it suitable for parts where uniformity and aesthetic quality are essential. The Lightning pattern mimics the path of a lightning bolt and is fast to print, making it ideal for low-strength parts that prioritize speed over durability. The Triangular pattern provides superior load-bearing strength, making it suitable for parts exposed to mechanical stress, though it requires a longer print time. The Tri-Hexagon pattern combines triangles and hexagons to optimize strength while reducing material usage, making it ideal for functional parts that need good support and efficiency. The Cubic pattern is solid and strong, making it suitable for load-bearing parts, though it increases print time. The Grid pattern balances speed and strength, making it ideal for parts that require moderate structural support. The Honeycomb pattern provides a good strength-to-weight ratio, making it perfect for lightweight, durable parts (structural components). Gyroid, Cubic, and Tri-Hexagon are optimal choices for functional parts, while Line and Grid work well for lighter, non-load-bearing items. The selection of the best infill pattern must depend on balancing print speed, strength, and material efficiency based on the part's intended use.


#### Can I Use Multiple Infill Patterns or Percentages in One Print?


Yes, you can use multiple infill patterns or percentages in one print. Slicer programs offer the option for variable or adaptive infill, allowing adjustments to the pattern and density in different areas of the print. The feature is helpful for complex parts that require stronger support in certain areas while keeping other sections lighter and faster to print. For example, a part needs a higher infill density in areas that bear weight (mounting points), while a lower infill is applied to non-critical sections. The ability to modify infill patterns and percentages helps optimize material usage, reduce print time, and ensure strength is focused where it is most needed without overusing resources.


## How to Choose the Best Type of Infill?


To choose the best type of infill, follow the five steps below.


1. **Determine the part's purpose** . Identify whether the part is decorative, functional, or structural. Decorative parts require less strength and use faster, more material-efficient patterns (Line or Grid). Functional parts need moderate strength, making patterns like Grid or Triangular suitable. Structural components, which require high strength, must use patterns(Gyroid or Cubic).
2. **Evaluate strength requirements** . Assess whether the part experiences mechanical stress or load-bearing. Choose infill patterns with high strength for parts subjected to high stress (Gyroid, Cubic, or Triangular). Simple patterns (Line or Grid) are sufficient for low-stress parts.
3. **Consider print time** . Determine how quickly the print needs to be completed. Use patterns (Line or Grid)for faster prints, as the patterns use fewer materials and have shorter print times. Choose more complex patterns (Gyroid or Cubic) for slower prints that require maximum strength.
4. **Select material** . Choose the appropriate infill pattern based on the material being used. Rigid materials, like PLA, perform well with lower-density infill patterns, while flexible materials, like TPU, require higher-density infill and patterns(Honeycomb or Gyroid) for added stability.
5. **Check slicer settings** . Ensure the slicer supports the chosen infill pattern and density. Slicers offer advanced features like adaptive infill, which automatically adjusts the infill density based on part geometry, providing optimal strength while saving material.


### Why Should You Use Xometry for Your 3D Printing Infill Requirements?


You should use Xometry for your 3D printing infill requirements because it offers high-quality, reliable, and customizable solutions tailored to the specific project needs. Xometry provides a wide range of infill options, allowing users to select the most suitable pattern and density based on factors (part function, strength, and material efficiency). Xometry optimizes the parts for durability and print time through advanced technology and precise manufacturing. The platform’s flexibility supports various applications, from lightweight models to load-bearing components. Xometry’s extensive network of 3D printing manufacturers guarantees fast turnaround times and competitive pricing, making it ideal for prototyping and production. Automated quoting and design feedback tools further streamline the process, helping clients receive accurate and cost-effective solutions for their infill requirements.


## How much Infill Needed for 3D Printing?


The infill needed for 3D printing depends on the part’s function and strength requirements. An infill density of 10% to 20% is sufficient for prototypes, providing enough structure to maintain the part’s shape without increasing print time or material usage. An infill density of 30% to 50% is recommended for functional parts that require moderate strength (enclosures, brackets, or mounts). The range ensures durability while optimizing material use and print time. A higher infill density of 60% to 100% is necessary to resist deformation or failure under stress for high-strength components (mechanical parts or structural supports). The appropriate density must always balance strength, material usage, and print time based on the specific needs of the part.


### How much Infill is Required to Achieve Maximum Tensile Strength?


Infill required to achieve maximum tensile strength ranges from 50% to 100%. Higher infill percentages, around 80% or more, provide greater internal density, improving resistance to stretching, pulling, or breaking under stress. Infill patterns (Gyroid, Cubic, or Triangular) improve strength distribution within the part, helping resist forces applied from multiple directions. Tensile strength increases as infill density rises because more material is available to absorb and distribute applied forces. The exact percentage required depends on factors (material type, part geometry, and load conditions). For example, parts subjected to high stress (structural supports or mechanical components) require nearly 100% infill, while lighter parts with less[tensile strength](https://www.xometry.com/resources/3d-printing/tensile-strength) perform well with lower infill densities.
