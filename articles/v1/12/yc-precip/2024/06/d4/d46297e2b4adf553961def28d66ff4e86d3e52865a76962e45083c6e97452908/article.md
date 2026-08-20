---
schema_version: "1.0.0"
document_id: "d46297e2b4adf553961def28d66ff4e86d3e52865a76962e45083c6e97452908"
company_key: "yc-precip"
company: "Precip"
source_id: "yc-precip-rss-f59c0f39e1b1"
canonical_url: "https://precip.ai/blog/measuring-rain-with-rain-gauges-is-hard/"
published_at: "2024-06-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:06.643563+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:aa430a589f95fac4f543c86ed131c29bd142a7907a1587df5e4c99d1ce7617cf"
---

# Measuring rain with gauges and weather stations is harder than you might think

## Understanding Error Biases in Rain Gauges


Rain gauges and electronic weather stations are popular tools for measuring precipitation, but many people aren’t familiar with the various ways they can sometimes produce inaccurate results. Like all measurement instruments, rain gauges are subject to errors and biases that can affect the accuracy of the data they collect.


In this post, we’ll explore the common sources of error in rain gauge measurements and examples of gauges susceptible to these errors. Understanding the causes of different rain gauge errors can be especially useful when comparing Precip’s local rainfall totals to what you see in your own gauge.


### 1. Wind-Induced Errors


Wind can significantly affect the accuracy of rain gauge measurements. When strong winds blow across the gauge, they can cause undercatch, where the wind diverts rain drops away from the gauge opening, leading to an under measurement of precipitation. This effect is particularly pronounced in open and exposed locations where wind speeds are higher. In addition, rain drop size has a strong relationship with the amount of wind induced error. Larger rain drop sizes are less impacted by wind than small rain drop sizes.


A general rule of thumb is that for every one mile per hour of wind speed, you can expect about a 1% loss in precipitation being caught in the gauge. This is why scientific quality rain gauge installations like the ones we use to train our models have wind shielding installed around the gauge.


### 2. Evaporation Loss


Evaporation from the gauge can occur before the precipitation is measured, especially in hot and dry conditions. This leads to an underestimation of rainfall. The design of the gauge, including its material and color, can influence the rate of evaporation. Open container rain gauges without covers or oil layers are prone to evaporation loss. Automated gauges designed with evaporation-reducing features, such as covered or funnel-type gauges, can minimize this error.


In conditions where evaporation is minimal (cool, humid, low wind), the error from evaporation loss can be negligible, often less than 1%. In arid and hot climates, evaporation loss can be more significant. Studies have shown that evaporation loss in these conditions can range from 1% to as much as 5% of the total precipitation.


For gauges left unattended for extended periods in hot, dry, and windy conditions, evaporation loss can exceed 5%, potentially reaching up to 10% or more in extreme cases.


### 3. Wetting Loss


Wetting loss occurs when water adheres to the walls of the gauge and is not accounted for in the measurement. This is a common issue with manual gauges, where the residue left on the sides can lead to an underestimation of rainfall. Standard cylindrical manual rain gauges are susceptible to wetting loss. Automated tipping bucket rain gauges, with their smooth surfaces and automated measurement systems, reduce the impact of wetting loss.


Studies have shown that wetting loss in standard cylindrical manual rain gauges can range from 0.1 mm to 0.3 mm per event. For gauges that measure total precipitation in small increments, this can result in an error of 1% to 5% per event, particularly significant in light rainfall events.


Tipping bucket rain gauges generally have lower wetting losses, often around 0.1 mm per tip. Since these gauges typically measure larger volumes per tip, the relative error is smaller, often below 1% for moderate to heavy rainfall events.


### 4. Splash-In and Splash-Out


Splash-in occurs when rain drops from outside the gauge splash into it, leading to an overestimation of precipitation. Conversely, splash-out happens when raindrops hit the edge of the gauge and bounce out, resulting in an underestimation. Gauges with wide openings, such as some funnel-type gauges, are more prone to splash-in and splash-out errors. Narrower, deeper gauges, or those with splash guards, can help reduce these errors.


### 5. Instrument Errors


Mechanical or electronic faults in automated rain gauges can lead to erroneous measurements. For example, tipping bucket rain gauges may miss small amounts of precipitation if the bucket does not tip at the correct threshold, leading to an underestimation. Regular checks and calibration are essential for maintaining accuracy in these types of gauges.


### Conclusion


Even though rain gauges are traditionally the most popular way to measure rainfall, they require careful consideration of the various ways they can deliver misleading results. While some errors can be minimized through proper gauge design and placement, others require regular maintenance and calibration.


**Precip’s AI based models are designed to help overcome many of the common errors that rain gauges encounter when measuring rain. Next time you check your rain gauge, be sure to consider which of these factors may have had an effect on the reading you’re getting.**
