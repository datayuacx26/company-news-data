---
schema_version: "1.0.0"
document_id: "8cf5a5800273f93982c38e2e53417875d0037ac784f6f3570a435b1ef1dedcee"
company_key: "quanex-building-products-corporation-common-stock"
company: "Quanex Building Products Corporation"
source_id: "quanex-building-products-corporation-common-stock-news-import-b653689950ca"
canonical_url: "https://www.quanex.com/news/window-thermal-ratings/"
published_at: "2026-05-06T11:45:45+00:00"
first_seen_at: "2026-07-23T21:44:38.138934+00:00"
fetched_at: "2026-07-28T22:12:46.079122+00:00"
content_hash: "sha256:5365a819e2268936d48b8c0163d550499e393d4b4bc9c27acb9b2dfa516ba4ae"
---

# Window Thermal Ratings Explained: LEAFF and Modern Fenestration Simulation

## Window Thermal Ratings


### Background and Industry Importance


**Question:** For those unfamiliar, how do window thermal ratings influence real-world product performance and decision-making?


**Answer:** Thermal ratings are the primary method by which energy codes are implemented, whether that be national programs like ENERGY STAR®, or state and local building ordinances. They are referenced by consumers when comparison shopping, and they are used in building envelope modeling software to calculate whole-building energy needs, to size HVAC systems, and to determine long-term costs. Thermal ratings are a primary consideration for purchasers of fenestration products, and manufacturers who do not supply them can be at a significant disadvantage in the market.


## Challenges with Traditional Window Thermal Ratings Modeling


**Question:** Before LEAFF, what challenges existed in how window thermal ratings were calculated?


**Answer:** There are three components of a fenestration product model: the center-of-glass, the frame, and the glass-frame edge. Modeling the center-of-glass is a simple process that can take as little as a minute to obtain values. However, the models that provide the frame and edge values take significantly longer to set up and run.


The standard (pre-LEAFF) method for calculating whole-product performance required simulators to rebuild and run the frame/edge models for every product. This significantly increased the time and associated cost of running multiple glass configurations in the same frame. These increased costs were passed on to manufacturers, raising the cost of innovation and, in some cases, leading to a general unwillingness to make changes at all.


**Questions:** Why did the traditional modeling process become a barrier to innovation?


**Answer:** The longer timeframe and greater cost of obtaining product ratings under the old methodology made it difficult and costly to compare various frame and spacer systems across a range of glass options and get a complete understanding of each system’s strengths and shortcomings.


##


## LEAFF and Changes to Window Thermal Ratings Simulation


**Question:** What is LEAFF, and how did its introduction change the way window thermal ratings are simulated?


**Answer:** LEAFF came about because multiple players in the simulation industry had noticed that, for a given frame/edge model, similar glass configurations showed a linear relationship between the center-of-glass values (Ug, SHGCg, VTg) and their respective whole-product values (U, SHGC, VT).


In other words, by carefully arranging glass configurations into similar groups, a simple equation could be formulated for each group that allowed one to calculate the whole-product values from the center-of-glass values, without redrawing and simulating the frame/edge models each time. Multiple whole-product values could be calculated in minutes rather than hours.


From the product manufacturer’s standpoint, the outputs of this new modeling method would look the same. Values would still be reported as they always had. Product grouping, standard sizes, verification requirements, etc., would not change. The only difference would be how thermal performance values were calculated by the simulator.


**Question:** What role does LEAFF play today within the NFRC 100 modeling framework?


**Answer:** LEAFF has not been as widely adopted as in 2020. This is not due to flaws in the LEAFF methodology itself, but rather to the changes in the structure that surrounds the modeling process. Over the years, many simulation labs have developed their own software tools and processes to improve the efficiency of the old system. They invested heavily in these tools and are understandably reluctant to scrap them and restart the process for a new methodology, even one that promises long-term benefits.


However, that situation is due to change this year. LEAFF became the required methodology for recertifying existing product lines on January 1, 2026, and will become the required methodology for new product lines on October 1, 2026. Again, it is important to note that this will not affect how manufacturers interact with the certification process; it only determines how simulations are done inside the labs. No changes are required on your part if you have products up for certification.


More information on this at[Linear Energy Analysis for Fenestration (LEAFF).](https://nfrccommunity.org/page/LEAFF)


##


## Efficiency, Product Development, and Use Cases


**Question:** How does LEAFF make it easier for manufacturers to evaluate changes to glazing, coatings, or spacers?


**Answer:** LEAFF separates glazings into groups according to attributes such as number of panes, airspace width, or spacer. Low-e coatings, gas fills, and glass thicknesses can all be in the same group and use the same trendline. Thus, if a manufacturer currently certifies a product with 3mm Solarban 60 and Argon and wants to see how that same system behaves with 4mm Solarban 70 and Krypton, no additional modeling is required. The center-of-glass U-factor (Ug) for the new configuration, easily generated in the WINDOW program, can be plugged into the existing trendline, and the new whole-product U-factor (Uw) calculated using basic arithmetic.


Spacer changes cannot be included in the same group and require a new trendline for each change in spacer platform, secondary sealant, or airspace width. However, the ability to group coatings, gases, and glass thicknesses means that spacer changes, like frame changes, can be compared across a wide variety of glass and gas combinations. It only takes three simulations per spacer to establish a trendline, after which any number of low-e coatings, gas fills, or glass thicknesses can be inserted into the trendlines and the results compared.


**Question:** What types of product lines benefit most from trendline-based thermal ratings?


**Answer:** Product lines with large numbers of glazing options, especially from manufacturers who prefer not to group certified products, submitting only the worst performer in each group, will obtain the greatest benefits from LEAFF.


## Window Thermal Ratings and Energy Codes


**Question:** How do improved thermal ratings help manufacturers comply with evolving energy codes and standards?


**Answer:** Despite recent uncertainty regarding national programs like ENERGY STAR®, state and local codes continue to become ever more stringent, especially in the coastal states and large urban markets. Moreover, national efficiency and rebate programs, when they return, will come back in a rush, and manufacturers won’t want to be caught flatfooted. Continuous improvement is crucial, with a focus on effecting changes to take advantage of new technologies as they become available, rather than a last-minute scramble to meet a recently updated requirement.


However, cost-benefit must always be at the forefront, and the latest shiny new technology may not make sense for a given manufacturer or market. That’s where improved thermal modeling can help. By comparing products both with and without a possible modification, across a wide range of glazing options, manufacturers can evaluate whether the enhancement will provide a real advantage or if sufficient improvement can be attained with a more cost-effective change to the glazing configuration.


**Question:** As performance targets continue to rise, why are accurate and efficient thermal ratings increasingly important for the industry?


**Answer:** Thermal performance is more than just product ratings. Building envelope modeling has become increasingly prevalent in the construction industry, driven by evolving technologies, energy costs, and tightening codes. As models become more complex and detailed, modelers will be asking for more accurate data on fenestration products. LEAFF can assist with this in two ways.


First, the short calculation times of LEAFF make it easy to provide rapid updates of performance values to the building modeler when design specifications change. Similarly, this makes it easier to work with the building modeler to determine a glazing configuration that can meet both thermal and aesthetic requirements.


Second, envelope modeling has become more granular, creating demand for fenestration models that match the products as they will actually be used. Most notably, product performance can change significantly with the product glass-to-frame ratio, so the modeled size matters. The traditional modeling methods were focused on providing rating values for standard sizes to be used in product comparisons. However, as part of the LEAFF initiative, NFRC has developed similar trendline methods to scale thermal values according to installed size. Thus, a single spreadsheet can provide the scaled values for all the product sizes in a building, dramatically increasing the accuracy of the envelope model.


## The Future of Window Thermal Ratings


**Question:** How do you expect window thermal ratings to evolve as simulation tools and materials advance?


**Answer:** Right now, NFRC is focused on bringing more products into the simulation arena. Methods have been added to accommodate fritting and internal blinds, and others are being considered, such as for spandrel glass or horizontal-glazed products. As the industry’s needs evolve, NFRC will continue to expand its capabilities so as to provide fair, accurate, and credible ratings.


**Question:** What advice would you give manufacturers evaluating their approach to thermal ratings today?


**Answer:** Technology is advancing faster than ever, and it is critical tos tay on top of what is available and could provide that competitive edge. Energy efficiency continues to be a driver of fenestration sales, even in the current climate. That said, thermal ratings are only one piece of the puzzle. As always, manufacturers need to understand their own market and select a product portfolio that balances cost, performance, and aesthetic needs.


## Key Takeaways on Window Thermal Ratings


- Window thermal ratings remain critical for energy codes, product comparisons, and building modeling.
- LEAFF reduced simulation time by using trendline-based modeling of center-of-glass to whole-product performance.
- Adoption has been gradual but is becoming more standardized within NFRC workflows.
- Accurate thermal modeling is increasingly important as envelope modeling becomes more granular.


As fenestration performance expectations continue to rise, accurate and efficient window thermal ratings will remain essential to product differentiation and code compliance. Methodologies like LEAFF enable manufacturers to explore design options faster, evaluate cost-performance tradeoffs, and deliver products that meet evolving market demands.


Additional resources and information can be found at:[nfrc.org](https://nfrc.org/)
