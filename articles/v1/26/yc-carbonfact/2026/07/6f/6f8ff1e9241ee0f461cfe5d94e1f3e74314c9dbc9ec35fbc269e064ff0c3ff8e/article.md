---
schema_version: "1.0.0"
document_id: "6f8ff1e9241ee0f461cfe5d94e1f3e74314c9dbc9ec35fbc269e064ff0c3ff8e"
company_key: "yc-carbonfact"
company: "Carbonfact"
source_id: "yc-carbonfact-rss-e3c23fd0e117"
canonical_url: "https://www.carbonfact.com/blog/knowledge/carbon-footprint-cotton"
published_at: "2026-07-06T22:00:00+00:00"
first_seen_at: "2026-07-24T22:18:04.567508+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:dc74ce8b7248a1b1e2f8cf92d4096834241b7b5510ebcd3d5e0121bfdf3d2128"
---

# The Carbon Footprint of Cotton

*"You Ask, We Answer" – brought to you by Carbonfact's Head of Science. Each week Laurent Vandepaer, PhD, together with Carbonfact's Science Associate Vincent Carrières, answers one of your questions about sustainable materials, manufacturing impact, and energy transition in the apparel and footwear industry.*


*Before joining Carbonfact, Laurent led the integration of LCA into the sustainability and innovation efforts at On and performed LCA for other brands like Arc'teryx. Vincent joined Carbonfact after three years as a sustainability expert at Quantis, where he specialized in LCA and corporate footprinting.*


---


> Question: What is the carbon footprint of organic vs. conventional cotton? Does organic cotton consume less water than conventional cotton?
>
>
> *Asked by the Head of Sustainability from an apparel brand*


Durable, breathable, and renewable, cotton is one of the most widely utilized natural materials in the fashion and textiles industry. According to the[Textile Exchange Materials Market Report 2025](https://textileexchange.org/app/uploads/2025/09/Materials-Market-Report-2025.pdf) , cotton remains the second most widely produced fiber after polyester, accounting for 19% of global fiber production in 2023/24.


Conventional cotton farming often involves heavy use of chemicals. Pesticides applied to cotton can pollute the soil and both ground and surface water, with potential drift affecting nearby crops. Additionally, the widespread use of synthetic fertilizers to boost yields contributes to water contamination and substantial greenhouse gas emissions.


The cotton market has long lacked the transparency needed to obtain reliable data. Complex global supply chains with multiple intermediaries make it hard to trace cotton from farm to finished product, while inconsistent data collection and varying standards across regions and[Life Cycle Assessment](https://www.carbonfact.com/blog/knowledge/lca-fashion-textile) (LCA) studies obscure cotton's true environmental and social impacts.


This is now beginning to change: recent work, including the 2026 Textile Exchange cotton LCA and a new farm-level study on organic cotton in India, brings more recent, region-specific data to a field that has long relied on outdated or aggregated figures.


We cover it all in the article below – let’s dive in!


#### TL;DR


- Cotton's carbon footprint varies significantly by geography and production system.
- Carbonfact integrated the 2026 Textile Exchange LCA, correcting several long-flagged data quality issues in organic cotton datasets.
- A new farm-level study in India (OCA & South Pole, ~18,000 farms) confirms organic cotton produces significantly lower GHG emissions than conventional.
- Organic cotton does not necessarily use less water than conventional – water impact is driven by geography, not only by farming method.


## Cotton Life Cycle Assessment: Organic vs. Conventional Cotton Fabric


Organic cotton is produced from the same plants as conventional cotton, but the key difference lies in its cultivation. Organic farming methods avoid using synthetic pesticides, chemical fertilizers, and other synthetic substances, focusing instead on natural processes to support plant growth and soil health.


-


Cultivation:


The growth of cotton plants, with organic farming using natural inputs like compost rather than synthetic fertilizers.


-


Harvesting and ginning:


Raw cotton is harvested and the fibers are separated from the seeds.


-


Opening, cleaning, blending, and carding:


Raw cotton is cleaned and prepared for spinning.


-


Spinning, weaving, or knitting:


Cotton fibers are spun into yarn and then woven or knitted into fabric.


-


Dyeing, printing, and finishing:


The final stages where the fabric is dyed, printed, and finished for use.


This process is followed for both organic and conventional cotton, though organic methods aim to minimize environmental impacts at every stage.


## Carbonfact Integrates the 2026 Textile Exchange Cotton LCA


In March 2026, Textile Exchange published a long-awaited[cotton LCA report](https://textileexchange.org/knowledge-center/reports/cotton-life-cycle-assessment/) covering country averages, organic, regenerative, and recycled cotton.


This report is a significant step forward for cotton LCA data. It addresses the main gaps flagged in previous studies (outdated data, missing regional resolution, inconsistent field-emission modeling) and follows the industry-aligned Cascale Cotton LCA Methodology.


Carbonfact has integrated the new Textile Exchange datasets for Organic cotton and Regenerative cotton into the platform. The table below shows the previous values (sourced from EF 3.1), the new values, and why the change matters.


Dataset


Previous (EF 3.1 source)


New (TE LCA source)


Difference


Interpretation


Cotton, Organic, US 0.08 kg CO2e/kg 1.77 kg CO2e/kg +2100% Previous GHG impact for US organic cotton has been flagged as underestimated, being significantly lower than any other geographies and even than recycled cotton.


Cotton, Organic, India 4.82 kg CO2e/kg 1.46 kg CO2e/kg -70% Previous GHG impact for IN organic cotton has been flagged as overestimated, being +16% more important than for IN conventional cotton. Indian cotton being >50% of total organic production, the correction was an important priority to address.


Cotton, Organic, Turkey 0.64 kg CO2e/kg 0.95 kg CO2e/kg +48% Moderate upward revision based on more recent country-representative inventory data.


Cotton, Regenerative, India - 1.72 kg CO2e/kg - New dataset - no previous value available.


Cotton, Regenerative, Peru - 0.49 kg CO2e/kg - New dataset - no previous value available.


Cotton, Regenerative, Turkey - 0.89 kg CO2e/kg - New dataset - no previous value available. Close to the Turkish organic value, suggesting limited GHG differentiation at country-average level.


Our cotton figures and the new Textile Exchange study are built on two different reference databases: we use ecoinvent, while the study uses Sphera. Numbers from different databases can vary, so we updated cotton only where the new data brings a clear improvement over our previous EF3.1 values (some of which had been flagged as faulty), where important data gaps were encountered (regenerative cotton datasets), and where it stays consistent with the rest of our materials datasets.


We've held off on a few cases: conventional and recycled cotton (until the Textile Exchange data is integrated into ecoinvent, ensuring consistency with the rest of our materials database), organic cotton from Brazil (where we first want to verify alignment with our own land-use-change methodology before integrating), and organic cotton from Tanzania (a small producer with data still too uncertain to rely on).


As ecoinvent updates its own cotton data to match this new study, we'll bring the rest in line.


For Carbonfact’s clients, this matters for sourcing decisions and reporting – product footprints for organic and regenerative cotton now reflect more recent, region-specific inventory data.


### Textile Exchange Cotton LCA: Key Insights


There are several important considerations to keep in mind when interpreting the LCA findings:


What "regenerative" means in this study and what it doesn't


There is no universal definition of regenerative agriculture. In this study, regenerative systems are certified or self-identified and use practices such as reduced tillage, organic fertilizers, and integrated pest management.


Important caveats: regenerative systems are highly site-specific, several of their benefits (soil carbon, biodiversity) only materialize over time, and the study uses screening-level indicators rather than direct measurements. The data should not be read as a definitive ranking of regenerative vs. other systems.


The datasets are directional, not a basis for country-level sourcing decisions


Textile Exchange is explicit on this point: results carry meaningful uncertainty (especially around organic yields, irrigation water, nutrient content of organic fertilizers, and pesticide rates). Recommended use is hotspot analysis, Scope 3 reporting, scenario modelling and benchmarking, not switching suppliers or countries based on the numbers alone.


The LCA framework alone does not tell the full story


Textile Exchange's LCA+ approach adds three dimensions that sit outside our standard 16-indicator framework: soil health (assessed via soil organic carbon), biodiversity (assessed indirectly via land use, eutrophication, pesticides, water), and a human rights screening based on a 20-question survey and expert interviews.


The human rights survey notably found that the cotton cultivation system (organic/regenerative/conventional) is


*not* the main driver of social impacts – local legislation, infrastructure, and mechanization matter more.


These dimensions should be considered alongside the LCA results when making sourcing decisions.


## Cotton Carbon Emissions Differ Per Region


The table below shows the analysis before/after Carbonfact's 2026 Textile Exchange LCA integration on cotton datasets. Modifications were only performed on 6 out of 29 cotton datasets in our emission factor database. No edits were made to conventional cotton regional differences – those remain sourced from EF 3.1, pending integration in ecoinvent.


GHG


Material type Country Before TE LCA implementation After TE LCA implementation % difference


Cotton, Conventional Australia 1.92 1.92 0%


Cotton, Conventional Brazil 5.73 5.73 0%


Cotton, Conventional China 7.48 7.48 0%


Cotton, Conventional Egypt 4.4 4.4 0%


Cotton, Conventional India 4.17 4.17 0%


Cotton, Conventional Kyrgyzstan 2.77 2.77 0%


Cotton, Conventional Mongolia 2.24 2.24 0%


Cotton, Conventional Pakistan 3.45 3.45 0%


Cotton, Conventional Tajikistan 2.85 2.85 0%


Cotton, Conventional Turkey 3.25 3.25 0%


Cotton, Conventional United States 6.07 6.07 0%


Cotton, Conventional Uzbekistan 2.67 2.67 0%


Cotton, Conventional World 5.27 5.27 0%


Cotton, Organic Brazil 5.35 5.35 0%


Cotton, Organic China 1 1 0%


Cotton, Organic India 4.82 1.46 -70%


Cotton, Organic Kyrgyzstan 1.15 1.15 0%


Cotton, Organic Pakistan 1.15 1.15 0%


Cotton, Organic Tajikistan 1.15 1.15 0%


Cotton, Organic Turkey 0.64 0.95 48%


Cotton, Organic Tanzania 3.52 3.52 0%


Cotton, Organic United States 0.08 1.77 2233%


Cotton, Organic World 3.07 3.07 0%


Cotton, Recycled World 0.05 0.05 0%


Cotton, Recycled, Post-consumer World 0.06 0.06 0%


Cotton, Recycled, Pre-consumer World 0.05 0.05 0%


Cotton, Regenerative India - 1.72 -


Cotton, Regenerative Peru - 0.49 -


Cotton, Regenerative Turkey - 0.89 -


Regional differences in cotton emissions are influenced by synthetic fertilizer use, irrigation, and energy sources for ginning. Organic systems generally show lower emissions than conventional, though results vary by geography depending on local farming practices and climate conditions.


Note that these figures are intended for hotspot analysis and benchmarking – the Textile Exchange LCA report explicitly states that results should not be used for direct comparisons between countries or farming systems.


One correction stands out: EF 3.1 data showed organic cotton in India with a higher footprint than conventional – a data quality issue flagged by our Science team. This has since been corrected: Carbonfact's integration of the 2026 Textile Exchange LCA brings the Indian organic cotton value from 4.82 kg CO₂e/kg down to 1.46 kg CO₂e/kg, reflecting more representative inventory data.


For a closer look at organic cotton farming in India, a dedicated LCA study from the Organic Cotton Accelerator and South Pole, based on primary data from ~18,000 Indian farms, provides grounded evidence – covered in the next section.


## Organic Cotton in India: The OCA & South Pole LCA


India accounts for over 50% of global organic cotton production .


[The LCA study](https://organiccottonaccelerator.org/news_article/oca-launches-regional-lca-for-india-setting-a-new-standard-in-organic-cotton-impact-measurement/)


from the Organic Cotton Accelerator (OCA) and South Pole provides a detailed primary-data assessment of Indian organic cotton.


The study covers the production of 1 kg of lint cotton from field to ginning gate, and draws on primary data from approximately 18,000 organic farms across India over three years.


### GHG Emissions of Cotton


According to the study, below are the GHG emissions by production system:


- Organic systems show 36–58% lower GHG emissions than conventional irrigated cotton in the same study.


- The hybrid organic system reaches 1.14 kg CO₂e/kg lint, the lowest value in the dataset.


Production System


Description


GHG Emissions (kg CO2e / kg lint)


Conventional Irrigated Standard farming with synthetic fertilizers and pesticides, water supplied through irrigation ~2.70


Organic Irrigated Certified organic farming (no synthetic inputs), water supplied through irrigation ~1.73


Organic Rainfed Certified organic farming relying on rainfall rather than irrigation ~1.53


Hybrid Organic Organic inputs combined with optimized field management practices to further reduce on-field emissions ~1.14


The main emissions hotspot in the OCA study is N₂O from on-field fertilizer application, not fertilizer production or irrigation. This differs from the 2021 Better Cotton Initiative LCA (see next chapter), where upstream fertilizer production was the largest contributor. The difference is likely methodological: the OCA study uses farm-level primary data.


It is also worth noting that the "Conventional" group in the OCA study represents farms in areas where organic programs are active, not a national average, and likely reflects better-than-average practices compared to the broader Indian conventional sector.


### Limitations of the OCA & South Pole LCA results


The study has a few limitations:


- The study assumes no conversion (turning non-agricultural land into farmland) has occurred because farms are family-owned and passed down through generations – a proxy that may not hold across all surveyed farms.


- Results apply only to OCA-enrolled farms in India and should not be used as a proxy for other regions or production systems. Brands relying on these figures for their own footprints will need strong farm-level traceability.


## Better Cotton Initiative: Cotton Life Cycle Assessment Study


In 2021, the


Better Cotton Initiative (BCI) commissioned[an LCA study](https://bettercotton.org/better-cotton-releases-our-first-study-on-ghg-emissions/) comparing the carbon footprint of Better Cotton versus conventional farming methods.


The Better Cotton Initiative promotes sustainable cotton farming by improving water management, reducing pesticide use, and enhancing soil health. Better Cotton is not a specific type of cotton but operates as a labeling system for cotton grown under BCI guidelines.


These guidelines include criteria such as efficient water use, reducing synthetic pesticide reliance, improving soil health through crop rotation, and ensuring fair labor practices. Find more on[BCI principles and criteria here.](https://bettercotton.org/what-we-do/defining-better-our-standard/)


The study examines emissions from production to ginning in key regions like India, Pakistan, China, Brazil, and the U.S., aiming to reduce the environmental impact of cotton farming.


Here are some important factors highlighted by the BCI study when evaluating the LCA of cotton:


-


Importance of fertilizer production:


**** The BCI study reveals that fertilizer production is the largest contributor to GHG emissions in cotton production, accounting for 47% of total emissions on average. Organic cotton farming relies on natural fertilizers like compost and manure instead of synthetic nitrogen fertilizers, though the breakdown of these organic fertilizers in soil can still release nitrous oxide.


-


Irrigation:


**** Irrigation is the second-largest contributor to GHG emissions in cotton production, accounting for 17% of total emissions on average. Pumping, transporting, and distributing water requires energy, which generates emissions. The contribution of irrigation varies considerably by region, depending on rainfall, irrigation methods, and water sources.


-


Fertilizer application:


The application of nitrogen fertilizers results in emissions of nitrous oxide (N₂O), a potent greenhouse gas. Although fertilizer production is responsible for a larger share of emissions, the application itself contributes 12% of total emissions on average.


-


Ginning:


The ginning process, which involves separating cotton fibers from seeds, consumes energy and generates GHG emissions. The emissions intensity of ginning depends on the energy source used and the efficiency of the ginning plants. Ginning accounts for 11% of total emissions from Better Cotton production.


-


Other sources:


**** Other sources of GHG emissions, such as crop residue management, field operations, pesticides, and transportation of cottonseed to the gin, collectively contribute a relatively small share of total emissions.


(Source:


[2021 Better Cotton Releases Study on GHG Emissions](https://bettercotton.org/better-cotton-releases-our-first-study-on-ghg-emissions/) )


Over 200,000 farm assessments from 2015-16 to 2017-18 analyzed, using the Cool Farm Tool to calculate GHG emissions. Better Cotton provided primary data on inputs, farm sizes, production, and locations, with gaps filled through desk research where necessary.


In the study, it seems surprising that the emissions from fertilizer production are much higher than emissions from fertilizer application – a discrepancy that is not specifically addressed in the BCI study. As the study is from 2021, they likely used Global Warming Potential (GWP) values from the previous assessment report.


### Limitations of the BCI Cotton Study LCA Results


The study has several important limitations:


- Land use change impacts are not integrated into the analysis, only discussed qualitatively.
- Data is self-reported, which may introduce bias.
- [Other studies suggest](https://www.nature.com/articles/s41598-022-18773-w)


higher fertilizer application rates, warranting further investigation.


Note:


While we cite these results for reference, this does not constitute an endorsement of BCI cotton.


## What is the Water Usage for Producing Cotton Fabric?


The[challenge in comparing water usage](https://www.forbes.com/sites/brookerobertsislam/2021/10/15/organic-water-saving-claims-false-declares-cotton-myth-busting-report/)


between organic and conventional cotton lies in the fact that regional factors, such as climate and irrigation methods, play a significant role, making broad claims difficult . The water impact can happen at every stage of cotton production but is likely to be higher during farming and dyeing, especially if these processes occur in water-deprived countries.


The EF 3.1 database provides water scarcity impact scores about water use relative to the local scarcity of water in different countries using the AWARE method. This is expressed in m³ deprived, meaning how much water the farms "take away" from the available local water supply.


The global average for organic cotton is 125.6 m³ deprived while it is 87.28 m³ deprived for conventional cotton, indicating a higher water stress caused by organic cotton. For comparison:


The average water use for organic cotton is higher because some countries that grow a lot of organic cotton use much more water than others. For example, countries like **Kyrgyzstan, Turkey,** and **Tajikistan** contribute a significant share of the global market, with 10%, 10%, and 5% respectively, and are driving up the global average:


-


Kyrgyzstan:


444.24 m³ of water per kilogram of cotton


-


Turkey:


188.61 m³ of water per kilogram of cotton


-


Tajikistan:


464.23 m³ of water per kilogram of cotton


Despite these regional differences, there is **no conclusive evidence** from critically-reviewed studies that prove organic-cotton farming uses less water than conventional methods. Whether cotton is grown organically or conventionally does not necessarily determine the amount of irrigated water used.


## Organic Cotton Fraud and Certifications


The total volume of organically farmed cotton is uncertain, largely due to widespread fraud in the market. This fraud typically occurs when conventional cotton is deliberately mislabeled as organic to capitalize on premium prices and growing demand. Several factors enable this deception:


- Weak supply chain traceability
- Inconsistent certification processes
- Limited oversight, especially in regions with fragmented supply chains
- Ability to blend or substitute non-organic cotton undetected


Organic cotton certification standards were developed to address these issues. These standards require farmers to:


- Maintain soil health
- Avoid synthetic fertilizers
- Prohibit hazardous pesticides
- Ban GMO use


Here's an explanation of each part of the **Organic Cotton Certification** system:


Governments:


To be legally sold as “organic”, raw cotton must come from farms certified under government-regulated organic standards. For example, India’s National Programme for Organic Production (NPOP) certifies a large portion of the world’s organic cotton, aligning with international standards.


Voluntary Standards Scheme Owners:


Standards like Textile Exchange's Organic Content Standard (OCS) and the Global Organic Textile Standard (GOTS) use chain-of-custody models to track organic cotton volumes throughout the supply chain.


Certification Bodies:


Governments and voluntary standard organizations like GOTS and Textile Exchange do not conduct certifications themselves. Instead, certification bodies verify compliance at farms and facilities, with different bodies focusing on different aspects of the supply chain.


## Should Brands Switch to Organic Cotton?


Organic farming systems have the potential to maintain and enhance the health of soils, ecosystems, and communities by utilizing ecological processes, biodiversity, and locally adapted cycles, instead of relying on external inputs that may cause harm.


For example, rather than applying synthetic nitrogen fertilizers – which contribute to significant greenhouse gas emissions during production and use, and can negatively impact soil health and water quality – organic farmers use methods like crop rotation and green manures to naturally enrich the soil.


Brands can support farmers transitioning to organic cotton by partnering with them to promote low-carbon practices, investing in programs that encourage organic farming, and purchasing "in-conversion" cotton (cotton grown by farmers who are in the process of transitioning from conventional to organic farming methods) to help share financial risks during the certification period.


## **About Carbonfact**


The cotton supply chain is among the longest and least transparent in the fashion industry, making brands susceptible to new regulations on supply chain due diligence and the demand for substantiating green claims with reliable, verifiable data.


Carbonfact is the Environmental Data Platform built specifically for apparel and footwear brands, as well as manufacturers, to measure the environmental impact of their products and take actionable steps to track and reduce their footprint.


Carbonfact's platform displays[detailed process](https://www.carbonfact.com/lca-for-fashion) steps not only for products, but also for each material and fabric used in your products. You can filter through your materials by supplier, raw material type, and any of your own custom properties.


Our[Product Impact Simulation tool](https://www.carbonfact.com/carbon-management-platform#:~:text=Run%20what%2Dif%20scenarios%2C%20on%20a%20product%2Dlevel) enables you to run what-if scenarios on a product level, where you can experiment with different materials,


preparation techniques, suppliers, renewable electricity share, or transportation methods, and build concrete company-level decarbonization scenarios.


*(Carbonfact's platform)*


Curious to learn more about the environmental impact of other key materials? Deep dive into our previous series:


[Carbon footprint of wool 🐑](https://www.carbonfact.com/blog/knowledge/carbon-wool)


[Carbon footprint of polyester 🛢️](https://www.carbonfact.com/blog/knowledge/yawa-polyester)


[Carbon footprint of leather 🐄](https://www.carbonfact.com/blog/knowledge/leather-carbon-impact)


Do you have questions about sustainable practices, manufacturing impacts, or energy transitions in the apparel and footwear industry? We'd love to hear from you! Comment, send us a DM on[Linkedin,](https://www.linkedin.com/search/results/all/?heroEntityKey=urn%3Ali%3Aorganization%3A75770625&keywords=Carbonfact&origin=ENTITY_SEARCH_HOME_HISTORY&sid=0Sw) o


r email:youaskweanswer@carbonfact.com


###### References


1. [Study on GHG Emissions](https://bettercotton.org/better-cotton-releases-our-first-study-on-ghg-emissions/) – Better Cotton
2. [Materials Market Report 2024](https://textileexchange.org/knowledge-center/reports/materials-market-report-2024/) – Textile Exchange
