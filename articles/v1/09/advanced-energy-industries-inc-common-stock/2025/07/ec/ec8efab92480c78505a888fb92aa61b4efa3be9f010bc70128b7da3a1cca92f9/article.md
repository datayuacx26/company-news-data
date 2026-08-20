---
schema_version: "1.0.0"
document_id: "ec8efab92480c78505a888fb92aa61b4efa3be9f010bc70128b7da3a1cca92f9"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/how-advanced-energy%E2%80%99s-modified-standards-offer-a-competitive-advantage-for-power-supply-designs/"
published_at: "2025-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:17bc21130209c79a52eb53e996ae494643d4bc555562d6476bd17e45decc678d"
---

# How Advanced Energy’s Modified Standards Offer a Competitive Advantage for Power Supply Designs

[Home](https://www.advancedenergy.com/en-us/) /


[About AE](https://www.advancedenergy.com/en-us/about/) /


[News & Events](https://www.advancedenergy.com/en-us/about/news/) /


[Blog](https://www.advancedenergy.com/en-us/about/news/blog/) /


How Advanced Energy’s Modified Standards Offer a Competitive Advantage for Power Supply Designs


# How Advanced Energy’s Modified Standards Offer a Competitive Advantage for Power Supply Designs


### Posted


July 08, 2025 by


[Conor Quinn](https://www.advancedenergy.com/en-us/about/news/authors/conor-quinn/)


**Blog Summary**


- **Challenges with off-the-shelf power supplies:** Standard power supplies often fail to meet the specific needs of niche or regulated applications, leading to design compromises, increased component count, and integration issues—especially when power is considered late in the design process.


- **Benefits of modified standard power supplies:** Modified standard PSUs offer a cost-effective and faster alternative to full-custom designs. They retain the reliability, certifications, and supply chain stability of standard models while allowing tailored modifications in electrical, mechanical, and firmware aspects.


- **Advanced Energy’s customization capabilities:** Advanced Energy provides a wide range of modifiable PSUs, supporting industries like medical and defense. Their modifications range from simple tweaks to complex redesigns, enabling rapid adaptation to customer needs while maintaining compliance and accelerating time to market.


---


While off-the-shelf power supplies provide a cost-effective solution, they are inherently inflexible, often pushing engineers to compromise on system design. Standard units seldom meet the specific requirements of niche or highly regulated applications, resulting in a gap between design requirements and the capabilities of generic power supplies.


The reasons for this mismatch vary. A system might require an additional low-voltage rail to power auxiliary circuits, or a non-standard output voltage to optimize performance. Alternatively, it might need a unique set of connectors, or a specific physical footprint to fit within a tightly packed enclosure. This can be particularly problematic when power is a late-stage design consideration. By this point a form factor is often set and no room is available for additional hardware - for example a cooling fan to resolve an unexpected thermal issue.


These compromises can also lead to an increased BOM when using off-the-shelf supplies, for example requiring a separate DC-DC converter to create an auxiliary rail. A full-custom design avoids these issues but can be prohibitively expensive and significantly increases time to market – especially if regulatory compliance is required. A better alternative is to leverage a modified-standard supply.


In this blog we examine Advanced Energy's (AE) capabilities, case studies on how modifications can be implemented and how AE offers tailored power supply performance for a competitive advantage. *Figure 1: A non-exhaustive listing showing the major classes of modifications*
*available for Advanced Energy’s standard off-the-shelf components*


**Advantages of a Modified Standard PSU Over a Custom Design**


Using a modified standard power supply has several advantages. This approach avoids the high development costs associated with creating a custom solution and offers a quicker time to market, with mechanical, electrical, and firmware modifications available for sampling within weeks.


Furthermore, the standard power supplies platforms used as the core of the design have already undergone significant testing, with documented reliability figures. Although this approach will not entirely eliminate the reliability risks associated with new product developments, it will substantially mitigate them. A similar principle applies to quality and performance, as the existing platforms have already demonstrated their expected operational capabilities.


The primary advantages of taking a mod-standard route include some less obvious factors. For instance, mod-standard PSUs can often retain the regulatory certifications of the original design.


Additionally, because they are based on existing products, they share many components with standard models. This significantly reduces supply chain risks that would otherwise be associated with small-volume manufacturing of custom designs, preventing end-of-life component issues that would otherwise lead to emergency redesigns.


**Advanced Energy's Modified PSU Offering**


Advanced Energy has a broad portfolio of over 3,000 PSUs, serving the needs of a wide range of industry applications – from manufacturing to defense to medical. AE has the industry’s largest and highest-power range of standard, mod-standard and configurable products, with output power from 0.1 W to 30 kW and output voltage of up to 60 kV.


These products feature leading power densities and efficiencies (up to 98%), with mod-standard solutions ranging from minor tweaks to highly complex, multifaceted changes with the combination of multiple modular power building blocks.


** *Figure 2: The CoolX1800 and LCM600U are both examples of customer configurable PSUs – see below*


More detailed case study examples are below, with recent rapid standard modifications including:


- [CS10M](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/enclosed-psus/cs1000/) base supply: An increase in the number of turns of an inductor to reduce low-frequency noise,
- [LCM600U](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/enclosed-psus/lcm/lcm600/) : Modification of the output connector, plus firmware changes for load-adaptive fan speed and logic signal and timing changes, and additional safety standard certifications,'
- [CoolX](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/configurable-modular-psus/coolx/) : PSUs in the 600 to 3,000 W range modified to implement a constant current setting via PMBus command to suit the V-I requirements of a specific battery for charging in a mobile scanner and have changed firmware to enable operation to 0 A at 180 V in a heater for OLED manufacturing.


Advanced Energy frequently receives requests for modifications to internal and open frame products. These requests include adjustments to output voltage (via digital means or component changes), implementation of EMI enhancements, reduction of leakage current, and the addition of a chassis or cover where it is not already available as a standard option.


A recent example includes being asked to modify a MINT3110 for use in a surgical camera. As was highlighted at the start of the blog, power is often considered late in the design. In this case, the originally selected downstream DC-DC had been quickly integrated and was causing the camera’s PSU to shut down. Time was limited, with the scheduled launch date only weeks away when the issue was identified. Our engineers promptly modified the MINT3110’s overcurrent protection (OCP) timing to accommodate higher peak currents at turn-on, ensuring these adjustments were completed in time for the product’s launch.


*Figure 3: An example of a surgical camera.*


External adaptors are also commonly modified to include custom connectors, over molds, cable lengths, and labeling and branding – including custom colors and packaging. Non-standard listings and verification testing are also common here, as are enhanced IP ratings and drop-test performance requirements, lower leakage currents, EMI enhancements and requirements for lower minimum order quantities.


As an example, Advanced Energy was recently asked to create an external adaptor for an infusion pump. The existing solution suffered from cable fraying and field failures that the existing supplier was unwilling to address.


Basing the solution on the[ME20](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/external-adapters/me/me20-series/) , Advanced Energy implemented enhanced strain relief to improve cable integrity. Being used in a hospital setting, leakage current was reduced to sub-10 µA. An enhanced construction able to withstand up to 30 drops was also implemented, and an IP67-rated enclosure was added to allow clinical-grade cleaning. Finally, the appearance needed to match that of the original, with a custom Pantone enclosure and cable.


**Medical Imaging Equipment Case Study**


Advanced Energy’s modified standard power supplies offer tailored performance at a fraction of the time and budget needed for fully custom power supply development.


So far, the examples given have (mostly) cited relatively simple modifications. A more complex modification can be seen in a request from a medical device OEM designing a power supply for the rotating gantry of its medical imaging system.


*Figure 4: An example of an MRI machine.*


The medical OEM required a modular AC-DC power supply that was operational from 2.85 to 48 V, with a total wattage requirement of 3 kW. In addition, a high-voltage DC-DC supply was required to deliver the bias voltage for a novel detector technology. Both PSUs needed to withstand the gantry’s rotational forces, which are more than 50 G.


Advanced Energy was able to offer rapid customization. For the DC-DC converter, a modified version of the[UltraVolt® High-Power 1C24-N250](https://www.advancedenergy.com/en-us/products/dc-dc-conversion-products/high-voltage-boost-(u-v)/high-power-(-60w-up-to-60kv)/high-power-c/) was used, which offers high-power-densities and has an output range of 125 to 60,000 VDC.


The AC-DC supply was based on three different modular[uMP](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/configurable-modular-psus/ump/ump-gen-ii/) PSUs, all mounted onto a single housing to give a total of nine outputs for the medical scanner. These 1,800 W digitally configurable PSUs meet military shock and vibration standards and feature a very wide input voltage range (85 to 264 VAC). Crucially, by modifying these, rather than developing a custom PSU, full EN60950 ITE and EN60601 medical safety approvals were retained.


By changing even relatively minor facets of its PSUs, Advanced Energy was able to accelerate the customer’s development cycle. This was aided by working in close cooperation with the OEM, including both testing and approval. In addition to this, experienced and expert technical support (local and online) was given throughout the process.


**Getting Started**


A modified standard power supply can offer an ideal mix of customization, affordability, and reliability while ensuring efficiency and quality.


Advanced Energy has over 40 years of experience in developing standard, modified, and custom power supplies. To find out more about this process please get in touch. You can also visit AE’s[custom and modified-standard parts page](https://www.advancedenergy.com/en-us/products/custom-and-modified-standard-solutions/) , which has additional case studies, or download AE’s[rapid modification catalog](https://www.advancedenergy.com/getmedia/61f92b9e-1b1c-46de-879e-097acc0ae8dc/ENG-EP-Rapid_Modification_Brochure-225-03.pdf) .


Share


### Conor Quinn


Advanced Energy


Conor Quinn is Vice President of Marketing for High Volume Products at Advanced Energy. He has over 30 years of experience in the power electronics industry in various design, technology and management roles. He is a past general chair of the Applied Power Electronics Conference and served as co-chair of PSMA’s Power Technology Roadmap. He has also contributed to the development of multiple industry standards including the Power Management Bus (PMBus), Advanced Telecom Computing Architecture (AdvancedTCA) and Micro Telecom Computing Architecture (MicroTCA). Conor received his BE in Electrical Engineering from University College Cork in Ireland and MSEE and PhD degrees from the University of Minnesota.


[More posts by Conor Quinn](https://www.advancedenergy.com/en-us/about/news/authors/conor-quinn/)


Browse


[Categories A-Z](https://www.advancedenergy.com/en-us/about/news/categories/)


Latest from AE


[News & Press Releases](https://www.advancedenergy.com/en-us/about/news/press/)[Videos](https://www.advancedenergy.com/en-us/about/news/videos/)[Trade Shows & Conferences](https://www.advancedenergy.com/en-us/about/news/events/)[Webinars](https://www.advancedenergy.com/en-us/about/news/webinars/)


Join Our Mailing List


Subscribe


Recent Posts


[View on X](https://twitter.com/advenergy)
