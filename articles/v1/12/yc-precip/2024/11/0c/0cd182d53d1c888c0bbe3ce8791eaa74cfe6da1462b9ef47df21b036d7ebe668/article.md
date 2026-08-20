---
schema_version: "1.0.0"
document_id: "0cd182d53d1c888c0bbe3ce8791eaa74cfe6da1462b9ef47df21b036d7ebe668"
company_key: "yc-precip"
company: "Precip"
source_id: "yc-precip-rss-f59c0f39e1b1"
canonical_url: "https://precip.ai/blog/how-snow-totals-get-calculated/"
published_at: "2024-11-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:06.643563+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:176e8c68a5aebd00564d6b33579aa7d4bc9ff981627d6cacb2756da7294e2274"
---

# What you need to know about measuring snow

# The difference between snowfall totals and rain totals


Measuring rainfall is Precip’s specialty, but what happens during the winter season when all that precipitation comes down in its white (sometimes) fluffy form? The answer depends largely on if you want to see recent snow totals, or if you are interested in finding out how much snow is still on the ground at a particular location. For now, we’ll cover just snowfall accumulation, and later we’ll dig into the topic of snow depth.


The main difference between measuring snowfall vs rain is not necessarily in how it is measured, but rather in how we calculate the actual amount of accumulation. The same techniques used to measure rainfall totals work when it is snowing.


Precipitation is measured using radar data and is initially reported in liquid equivalent amounts. You can think of that as the amount of liquid measured in a rain gauge if you had collected the accumulated snow, melted it, and then poured it into the gauge.


As we measure precipitation the data is analyzed to determine if the measurements being taken are liquid (rain) or frozen (snow). If we detect that the precipitation is snowfall, we then apply an additional step of converting the liquid measurement to a snow total based on the Snow Liquid Ratio (sometimes called Snow Water Equivalent).


## Detecting snowfall


Once precipitation is being measured, we determine if it is frozen or not. For that we use data from the[RTMA (Real-Time Mesoscale Analysis)](https://www.nco.ncep.noaa.gov/pmb/products/rtma/#RTMA_RU) to analyze surface temperature and web-bulb temperature. RTMA integrates data from multiple sources, including METAR observations, GOES satellite data, and high resolution numerical weather models to deliver accurate temperature analysis over the U.S. at 2.5km resolution. Our algorithm assumes that surface temps below freezing where the wet-bulb temperature is below 2 degrees Celsius lead to frozen precipitation accumulating on the ground.


## Applying Snow Liquid Ratios (SLRs)


Once we know if the observation was snow, the next step is to translate the liquid equivalent into a snowfall total. To do this we use what is called a snow to liquid ratio (SLR) that represents how much liquid would be left from melting an inch of snow and pouring it into a rain gauge.


Snow liquid ratios are challenging to estimate because they vary greatly depending on a number of factors that aren’t easy to measure or simulate. Studies have shown that common SLRs vary between 6:1 and 18:1. The method we use at Precip is to extract values from the most recent run of the HRRR model when available and otherwise fallback to a 10:1.


When we’ve determined that the precipitation observed was likely snow, we then lookup the appropriate SLR for that exact location and month of the year and apply the SLR as a multiplier to the measured liquid equivalent producing a final snow total for that hour.


# Snow depth vs snowfall accumulation vs snow cover


Now that we’ve covered how Precip calculates snowfall amounts, we can move into the much more complex problem of analyzing snow cover and snow depth. While detecting snow cover is quite easy (it can be detected using 1km resolution MODIS satellite data), determining depth is vastly more difficult.


Snow depth = Snowfall accumulation + compaction + drift + melting + sublimation. Keeping track of all these variables requires a sophisticated mass/energy balance model that goes well beyond just determining how much snow accumulated. Thankfully the National Weather Service maintains the Snow Data Assimilation System (SNODAS).


## The SNODAS model


According to official documentation, SNODAS includes procedures to ingest and downscale output from the HRRR and RAP models and to simulate snow cover using a physically based, spatially-distributed energy- and mass-balance snow model. It also includes procedures to assimilate satellite-derived, airborne, and ground-based observations of snow-covered area and snow water equivalent (SWE) collected by NOHRSC. The output of SNODAS is a comprehensive daily snow report including the following snowfall data attributes:


- Snow Water Equivalent
- Snow Depth
- Snow Melt Runoff
- Sublimation from Snow Pack
- Sublimation of Blowing Snow
- Solid Precipitation (24 hour total)
- Liquid Precipitation (24 hour total)
- Snow Pack Average Temperature


At Precip, we use the SNODAS model to generate our snow depth maps and chart the snow depth over time.


# The best sources for snowfall data


Calculating **snow totals** requires additional steps beyond simply measuring precipitation as it happens. Frozen precipitation must be detected and converted to the appropriate snow equivalent using SLR’s. Websites like[www.nohrsc.noaa.gov](https://www.nohrsc.noaa.gov/) offer 4km resolution snowfall total maps while[Precip](https://precip.ai/) offers 1km versions where you can easily pinpoint a specific location.


Generating a **snow depth map** requires modeling a highly dynamic system and the team at NOHRSC has the best model for this complex snow analysis.
