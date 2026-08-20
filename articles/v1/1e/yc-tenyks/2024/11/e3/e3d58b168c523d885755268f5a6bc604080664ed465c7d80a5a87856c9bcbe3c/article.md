---
schema_version: "1.0.0"
document_id: "e3d58b168c523d885755268f5a6bc604080664ed465c7d80a5a87856c9bcbe3c"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-rss-b52d09f64a8c"
canonical_url: "https://medium.com/@tenyks_blogger/vision-intelligence-sam2-satellite-analytics-for-deforestation-part-1-85aa4d005aa1"
published_at: "2024-11-26T16:25:06+00:00"
first_seen_at: "2026-07-27T06:27:15.777070+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:d8dc37c9848a8948c9f83afbac37da60015a97c062f2dee898a5b0d117027947"
---

# Vision Intelligence: SAM2 + Satellite Analytics for Deforestation — Part 1

# Visual Intelligence: Foundation Models + Satellite Analytics for Deforestation — Part 1


## In Part 1, we explore how to leverage satellite and visual intelligence to estimate deforestation.


[The Tenyks Blogger](https://medium.com/@tenyks_blogger?source=post_page---byline--85aa4d005aa1---------------------------------------)


9 min read


·


Nov 26, 2024


--


Press enter or click to view image in full size


Foundation models are about to disrupt the satellite industry


Satellite imagery has revolutionized how we monitor Earth’s forests, offering unprecedented insights into deforestation patterns.


In this two-part series, we explore both traditional and cutting-edge approaches to forest monitoring, using Bulgaria’s Central Balkan National Park as our case study.


Part 1 examines traditional satellite-based methods, particularly NDVI analysis.


### Table of Contents


1. The power of satellite data for visual intelligence
2. Satellite Data 101: a systems perspective
3. How to measure deforestation using satellite data?
4. Central Balkan Park: a case study
5. What’s next?


## 1. The power of satellite data for visual intelligence


### 1.1 Unprecedented Scale and Access


Satellite imagery has revolutionized our ability to observe Earth at a scale previously unimaginable. While traditional ground surveys might cover a few square kilometers per day, a single satellite image can capture tens of thousands of square kilometers in minutes as shown in Figure 1.


Press enter or click to view image in full size


Figure 1. Three different approaches to acquire data


This capability has been democratized through open-access platforms like[Sentinel](https://sentinels.copernicus.eu/web/sentinel/home) and[Landsat](https://landsat.gsfc.nasa.gov/satellites/) , transforming what was once military-grade technology into a publicly available resource.


For AI practitioners, this means access to petabytes of structured visual data covering every corner of our planet. From tracking urban development to monitoring remote rainforests, satellite data provides consistent, high-quality imagery that serves as the foundation for large-scale environmental analysis.


### 1.2 Temporal Insights and Pattern Detection


The true power of satellite intelligence lies not just in its spatial coverage, but in its temporal dimension. Modern satellite constellations revisit the same location multiple times per week, creating rich time-series data that capture Earth’s dynamic nature (see Figure 2).


Press enter or click to view image in full size


Figure 2. Time-series data allows to understand how objects change


For AI engineers familiar with video analysis, think of it as a slow-motion video of our planet, but with multispectral capabilities beyond visible light.


This temporal richness enables detection of subtle patterns: gradual forest degradation, seasonal changes, or even illegal logging activities that might go unnoticed in single-point-in-time analysis. Each pixel becomes a time series, transforming static image analysis into dynamic pattern recognition.


## 2. Satellite Data 101: A Systems Perspective


### 2.1 Satellite Data Architecture


Modern satellite systems are complex data pipelines (as shown in Figure 3). Remote sensing satellites, equipped with various sensors, capture both visible and non-visible electromagnetic radiation.


Press enter or click to view image in full size


Figure 3. The stages of a typical satellite data pipeline


This data goes through multiple processing stages: from raw sensor data to radiometrically corrected images, and finally to analysis-ready data products.


For AI practitioners, this is similar to data preprocessing pipelines, where raw data undergoes multiple transformations before becoming suitable for model training.


### 2.2 Spectral Dimensions and Resolution Types


Unlike standard RGB images, satellite data operates in multiple dimensions. Modern satellites capture information across various spectral bands, from visible light to infrared and thermal.


Additionally, practitioners need to consider three key resolutions: spatial (pixel size), temporal (revisit frequency), and spectral (number of bands) as illustrated in Figure 4.


Press enter or click to view image in full size


Figure 4. Satellite imagery has many dimensions and resolutions (e.g., spatial and temporal)


These characteristics create trade-offs similar to those in deep learning: *higher resolution means more detailed information but requires more computational resources and storage.*


### 2.3 **Data Access and Processing Infrastructure**


Getting access to satellite data can be either free or paid, as shown in Figure 5.


Press enter or click to view image in full size


Figure 5. Comparison of satellite data providers


The scale of satellite data requires specialized infrastructure. A single high-resolution image can be several gigabytes, and historical analysis might involve petabytes of data.


For AI practitioners, this means understanding both the possibilities and limitations of working with geospatial big data, including data formats (GeoTIFF, NetCDF) and processing architectures.


## 3. How to measure deforestation using satellite data?


Deforestation detection using satellite imagery can be approached through more than one methodology. In this post, we highlight two of them: traditional spectral analysis and modern AI-based techniques.


> Note: The AI-based approach using Foundation Models will be introduced here, setting the foundation for Part 2 of this series, where we’ll explore its implementation.


### 3.1 **Traditional Approach to Deforestation Detection**


Figure 6 shows the entire framework we used to detect deforestation using Sentinel. As described in section 4, we used this process to assess deforestation in one of the largest forests in Central Balkan National Park, Bulgaria.


Press enter or click to view image in full size


Figure 6. NDVI-based framework to compute deforestation


1. **Satellite Data Collection** The process begins with collecting specific spectral bands from satellite imagery. The two crucial bands are:


- Near-Infrared (NIR, Band 8): Captures vegetation’s high reflectance in the NIR spectrum. Higher values (like 0.80–0.90 in the image) typically indicate dense vegetation.
- Red Band (Band 4): Captures chlorophyll absorption. Lower values (like 0.10–0.20 in Figure 6) typically indicate healthy vegetation as chlorophyll absorbs red light. These bands are chosen specifically because healthy vegetation has distinctive reflection/absorption patterns in these wavelengths.


**2. NDVI Calculation** The Normalized Difference Vegetation Index (NDVI) is calculated using the formula: NDVI = (NIR — RED)/(NIR + RED)


- The formula produces values between -1 and 1.
- Higher values (>0.5) typically indicate healthy vegetation.
- In the example, we see resulting values like 0.78, 0.56, 0.80, which are strong indicators of forest cover.
- The normalization in the formula helps account for different lighting conditions and some atmospheric effects.


**3. Forest Classification** Using NDVI threshold to create binary forest masks:


- Areas with NDVI > 0.5 are classified as forest (true).
- Areas with NDVI ≤ 0.5 are classified as non-forest (false).
- This creates a binary mask for each time period.
- The threshold (0.5) is a commonly used value but can be adjusted based on local conditions and forest types.


**4. Deforestation Detection** The final step compares forest masks from different time periods:


- Areas that were forest (true) in the initial mask but became non-forest (false) in the latest mask are identified as deforested.
- The resulting binary map shows deforested areas (marked in the darker shade in the image).


## 4. Central Balkan Park: a case study


Leveraging our framework defined in section 3, we present the results of a deforestation analysis between 2016 and 2021 for Central Balkan National Park.


### 4.1 Geographic and Ecological Context


The Central Balkan National Park represents a significant protected area in Bulgaria and Europe’s forest conservation network (see Figure 7). Established in 1991, it covers 71,669.5 hectares (716.69 km²) of the highest parts of the Balkan Mountains (Stara Planina).


The park, along with its buffer zone covering an additional 129,000 hectares, forms one of the largest protected territories in Europe, earning its designation as a UNESCO World Heritage site.


Press enter or click to view image in full size


Figure 7. Central Balkan size & Elevation


Key characteristics that make it an ideal case study:


- Altitude Range: 500m to 2,376m (Peak Botev), creating diverse microclimates.
- Forest Coverage: ~60% of the territory (approximately 44,000 hectares).
- Biodiversity: Home to 70% of Bulgaria’s plant species, including 2,340 plant species (at least 23 endemics).
- Some of Europe’s largest beech forests.


### **4.2 National Reforestation Context**


The Central Balkan Park study gains additional relevance in light of Bulgaria’s ambitious **national afforestation campaign** .


In 2022, the country launched a transformative[initiative](https://ceenergynews.com/climate/bulgaria-launches-national-afforestation-campaign/) led by then-Deputy Prime Minister Borislav Sandov, setting a target to plant 100 million trees by 2030. This positions our case study within a broader national commitment to forest preservation and expansion.


Main aspects of the initiative:


- Current baseline: ~4 million trees planted annually
- Target: 100 million trees by 2030 (a 25x increase)
- Broad institutional support: Government, presidency, and civil society
- National-scale implementation strategy


### **4.3 Data Source Selection & Preprocessing Pipeline**


The choice of satellite data for the Central Balkan Park analysis requires balancing multiple factors. Sentinel-2’s 10m resolution and 5-day revisit time make was ideal for regular monitoring.


Press enter or click to view image in full size


Figure 8. Unexpected scenarios when dealing with satellite data


Satellite seasonal weather patterns create specific preprocessing challenges. Our pipeline must handle:


- Extraction of data during summer months to minimize cloud coverage
- Storage of band information to optimize future computations
- Creation of dual-resolution versions of images (high and low resolution)
- Addition of labelled versions of images for reference purposes
- Handling of partially obscured or cropped images


The output of our pipeline, for a specific period during 2017, can be seen on Figure 9.


Press enter or click to view image in full size


Figure 9. Deforestation map of Central Balkan National Park in 2017


This deforestation map of Central Balkan National Park (Bulgaria) shows forest changes from July 23–30, 2017.


The 100km x 100km area is represented at 10m resolution, where green indicates intact forest and red shows deforestation. The visualization reveals concentrated deforestation patterns in the southern region, with scattered smaller patches throughout the park.


Notable are linear patterns that might indicate logging roads or natural boundaries. The short time frame (one week) and high spatial resolution (10m) provide a detailed snapshot of forest change dynamics in the protected area.


### 4.4 Results


**4.4.1 Overall Forest Health Score**


Figure 10 shows the evolution of forest health in Central Balkan National Park from 2016 to 2021. The blue line represents the forest’s health score, which increased from approximately 0.49 in 2016 to 0.59 in 2021.


Press enter or click to view image in full size


Figure 10. Evolution of forest health in Central Balkan National Park


A red dashed line marks the Global Forest Watch’s healthy forest threshold (0.50) as of 2020. The park’s health score crossed this threshold in 2017–2018 and has shown consistent improvement since, with a particularly sharp increase between 2020 and 2021.


This positive trend suggests successful forest management and conservation efforts in the protected area.


**4.4.2 Forest Vitality**


Figure 11 shows the NDVI-based forest vitality trends in Central Balkan National Park from 2016 to 2021.


Press enter or click to view image in full size


Figure 11. Forest vitality based on NDVI (Normalized Difference Vegetation Index)


The forest vitality score initially shows a slight decline from 2016 to 2018 (dropping from about 0.9 to 0.85), followed by a steady recovery and improvement from 2018 onwards, reaching approximately 1.0 by 2021.


This gradual improvement aligns with the forest health score trends seen in Figure 10.


**4.4.3 Forest Coverage**


Figure 12 shows forest coverage trends in Central Balkan National Park from 2016–2021, compared against three reference levels (Amazon Rainforest, Mid-Latitude Forests, and Global Average).


Press enter or click to view image in full size


Figure 12. Central Balkan National Park is on par with the global average for “forest coverage”


The park’s coverage fluctuated between 54.4% and 59.3%, with a peak in 2018 followed by slight declines. While consistently below the Amazon (red, ~95%) and Mid-Latitude (green, ~70%) references, it remained close to the Global Average (blue, ~60%). The data indicates relatively stable forest coverage, hovering around 56–58% in recent years despite minor variations.


## 5. What’s Next?


Our analysis of Central Balkan National Park using traditional satellite-based methods revealed an interesting paradox: while forest coverage remains below reference levels (~56–58%), the health and vitality of existing forest areas have significantly improved from 2016 to 2021.


In Part 2, we’ll explore how Foundation Models can enhance our understanding of forest dynamics.


The shift from pixel-based NDVI analysis to Foundation Models in semantic segmentation promises to revolutionize how we monitor and protect forests like Central Balkan. Stay tuned to learn how modern AI can build upon traditional methods to create more robust and responsive forest monitoring systems.


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan


*If you’d like to know more about* ***Tenyks*** *, try*[sandbox](https://www.tenyks.ai/?utm_source=medium&utm_medium=article&utm_campaign=deforestation_part1) *.*
