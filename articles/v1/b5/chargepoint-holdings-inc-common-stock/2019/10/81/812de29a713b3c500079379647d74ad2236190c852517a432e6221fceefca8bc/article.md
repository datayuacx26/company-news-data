---
schema_version: "1.0.0"
document_id: "812de29a713b3c500079379647d74ad2236190c852517a432e6221fceefca8bc"
company_key: "chargepoint-holdings-inc-common-stock"
company: "ChargePoint Holdings Inc."
source_id: "chargepoint-holdings-inc-common-stock-news-import-2405547b27fc"
canonical_url: "https://www.chargepoint.com/engineering/moving-mountains-power-management"
published_at: "2019-10-03T19:06:17+00:00"
first_seen_at: "2026-07-21T13:13:26.870476+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:bce5b80ef15ed6080c19f00d896baab25119dadd052b949da6a81e786657e789"
---

# Moving Mountains with Power Management

A ChargePoint Level 2 charging station delivers approximately 7 kilowatts (kW) of power while charging an electric vehicle (EV). For reference, an average single family residence in America consumes between 1-2 kW per hour. Though charging stations have the potential to significantly increase energy consumption where they are installed (potentially leading to steeper utility charges),[ChargePoint Power Management](https://www.chargepoint.com/products/power-management/) helps customers keep their utility charges as low as possible.[Power Management](https://www.chargepoint.com/blog/charge-more-evs-power-management/) is enabled by a Power Sharing Algorithm (PSA) that uses machine learning to flatten out demand peaks.


### How Demand Charges Could Get Mountainous


Electrical energy is delivered via a complex network that includes: power generation, transmission, distribution and transformers that are used to convert from high transmission voltages to lower voltages. This network, known as the "electric grid", provides electricity to consumers and businesses in the form of a service drop. The service drop is connected at a "revenue meter" that records the power and energy consumption of all downstream loads.


Utilities charge for energy consumption in two ways: energy charges and demand charges. Energy charges are based on the amount of energy used and are measured in kilowatt-hours (kWh). Demand charges are based on the consumer's max power consumption over 15 minutes during the month. Utilities measure power (kW) over clock-aligned 15 minute periods (12:00-12:15, 12:15-12:30, etc.), calculate the average power and then take the max value over the entire billing period. That single value is used to determine the demand charge component of the energy bill.


The grid's max power capacity is the ultimate limitation shared by all consumers in the area, and is very costly to expand. Power consumption greater than the max power capacity overloads network components, potentially causing severe damage. Utilities use demand charges to encourage consumers to spread their energy consumption over time in order to: a) avoid overloading network components in the short term, and b) cover the cost of expanding the grid's max power capacity in the long term.


As a consumer's demand for electricity increases, demand charges begin to comprise larger and larger percentages of the consumer's total electricity costs. In general, that percentage falls somewhere between 30 and 70%.


Take, for instance, a large company office building that contains 64 charging ports. The graph below shows the daily peak power drawn by these ports within a three-week period.


Notice how this graph resembles a steep mountain. The steepness correlates to higher demand charges due to the ports drawing electricity at a high power rate or level for a relatively short time - which impacts the grid's max power capacity at that time.


### Moving Mountains


Fortunately, we've managed to solve the demand charge challenge using Power Management software. Power Management allows us to move this metaphorical mountain and thus reduce our customers' demand charges - without requiring drivers to change their behavior at all.


How? There are two paths: 1) Reduce the peak power output during busy times, and 2) Schedule charging so it occurs during off-peak hours.


For path 1, we start by allowing our customers to define an electrical site topology with power sharing device groups that represent transformers, panels or virtual device groups. Each power sharing device group has an aggregate peak power limit that can be shared by all charging ports within that device group. This graph demonstrates an electrical site topology with charging ports that are connected to one transformer and two panels in a hierarchy.


Based on the application, our customer then selects a specific policy that will govern how the power distribution will occur (i.e., "equal share with round robin" or "first come, first serve" etc.). This power sharing policy guides our PSA to dynamically monitor and control the power levels of all charging ports.


ChargePoint's PSA employs machine learning technology to ensure that all charging ports are able to use the highest aggregate amount of power while complying with the aggregate peak power limits of a site's power sharing device groups. To accomplish this, the algorithm continuously learns and adapts to how each vehicle's algorithm draws power and dynamically adjusts the power being offered to the vehicle with respect to other vehicles sharing the same device group power limitations.


For instance, say that a vehicle has historically been charging at a lower power level than it has been offered (perhaps due to heat which causes the vehicle's battery charging algorithm to reduce the amount of power drawn). The algorithm will learn and react by offering a lower power level for this vehicle, while safely and gradually increasing the power dispensed to other vehicles.


The PSA can also monitor the revenue meter in real-time, tracking the 15-minute average power amount. This can be used as input to calculate a dynamic peak that accounts for both the charging load and the overall site load. If the meter is not approaching a monthly peak, the PSA finds no reason to limit charging. As a peak is neared, the PSA will curtail charging activity in order to keep the total site load below a specific threshold. Over time, the PSA will learn the load profile of the entire site, be able to calculate an optimal peak power threshold and anticipate periods of peak demand.


Essentially, the algorithm continuously analyzes and appropriately adjusts power levels to achieve optimal charging performance - all while ensuring that the power shifting between vehicles does not exceed the aggregate limits of the site electrical topology or total site load. Thus, our customers are able to reduce peak power usage during peak hours, thereby reducing their demand charges.


Scheduling charging during non-peak hours is another way to reduce demand charges. This is done by configuring the aggregate peak power limit of any device group according to a set schedule (i.e., restricting the power level to 0 kW during peak hours of 9 a.m. to 5 p.m. and bumping it up to 180 kW during the non-peak hours of 5 p.m. to 9 a.m.). Scheduled charging not only reduces demand charges, but also may reduce energy charges (because certain hours of the day have lower energy rates).


### Lower Peaks, Lower Bills


ChargePoint's PSA continuously monitors and dynamically optimizes power levels of charging ports based on the electrical site topology, the defined power sharing policy, the vehicles' algorithms to draw power and, in some cases, the site revenue meter.


Notice how the second graph shows a significant reduction in the daily peak power drawn by those same 64 ports after using ChargePoint Power Management over the course of one week. Peak power levels have been reduced and are now spread out over time, resulting in less variable peak power loads and consequently reduced demand charges.


We also deploy Power Management technology to support higher numbers of charging ports than would have been possible at a particular site, given the transformer or panel's max power capacity.


Whether the goal is to reduce demand and energy charges or simply to install more ports without surpassing a site's max power capacity, ChargePoint Power Management gets the job done. It's absolutely key to providing customers with better experiences and lower bills.


If you think you have what it takes to work with a brilliant team of freethinkers to reinvent mobility, check out our opportunities.


[See Openings](https://www.chargepoint.com/about/opportunities/)


The information contained herein is considered proprietary and for informational purposes only and is not to be used or replicated in any way without the prior written authorization of ChargePoint, Inc.
