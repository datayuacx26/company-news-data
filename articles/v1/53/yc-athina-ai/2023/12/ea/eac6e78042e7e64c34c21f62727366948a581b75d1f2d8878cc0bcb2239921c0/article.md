---
schema_version: "1.0.0"
document_id: "eac6e78042e7e64c34c21f62727366948a581b75d1f2d8878cc0bcb2239921c0"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/peace-prompt-engineering-automation-for-clipseg-enhancement-in-aerial-robotics"
published_at: "2023-12-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:d69bf7d171b1f5af6015ce5cbe16e705b6c2a6c1273bf8a1783d310be7144823"
---

# PEACE: Prompt Engineering Automation for CLIPSeg Enhancement in Aerial Robotics

Do not index


Original Paper


[https://arxiv.org/abs/2310.000](https://arxiv.org/abs/2310.00085)


Blog URL


[blog.athina.ai /peace-p...robotics](https://blog.athina.ai/peace-prompt-engineering-automation-for-clipseg-enhancement-in-aerial-robotics)


**Original Paper:**[https://arxiv.org/abs/2310.00085](https://arxiv.org/abs/2310.00085)


**By:**[Haechan Mark Bong](https://arxiv.org/search/cs?searchtype=author&query=Bong%2C%20H%20M) ,[Rongge Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20R) ,[Ricardo de Azambuja](https://arxiv.org/search/cs?searchtype=author&query=de%20Azambuja%2C%20R) ,[Giovanni Beltrame](https://arxiv.org/search/cs?searchtype=author&query=Beltrame%2C%20G)


**Abstract:**


> From industrial to space robotics, safe landing is an essential component for flight operations. With the growing interest in artificial intelligence, we direct our attention to learning based safe landing approaches. This paper extends our previous work, DOVESEI, which focused on a reactive UAV system by harnessing the capabilities of open vocabulary image segmentation. Prompt-based safe landing zone segmentation using an open vocabulary based model is no more just an idea, but proven to be feasible by the work of DOVESEI. However, a heuristic selection of words for prompt is not a reliable solution since it cannot take the changing environment into consideration and detrimental consequences can occur if the observed environment is not well represented by the given prompt. Therefore, we introduce PEACE (Prompt Engineering Automation for CLIPSeg Enhancement), powering DOVESEI to automate the prompt generation and engineering to adapt to data distribution shifts. Our system is capable of performing safe landing operations with collision avoidance at altitudes as low as 20 meters using only monocular cameras and image segmentation. We take advantage of DOVESEI's dynamic focus to circumvent abrupt fluctuations in the terrain segmentation between frames in a video stream. PEACE shows promising improvements in prompt generation and engineering for aerial images compared to the standard prompt used for CLIP and CLIPSeg. Combining DOVESEI and PEACE, our system was able improve successful safe landing zone selections by 58.62% compared to using only DOVESEI. All the source code is open source and available online.


---


###


Summary Notes


####


Enhancing UAV Safe Landing with PEACE: A Leap in Aerial Robotics


Drones, or Unmanned Aerial Vehicles (UAVs), have broadly infiltrated various sectors, including package delivery and aerial surveys.


As their usage expands, ensuring their safety, particularly during landings in complex areas, is crucial. This blog post introduces an innovative solution named PEACE, designed to automate prompt engineering and significantly boost UAV safe landing capabilities.


####


Importance of UAV Safety


The growth of UAVs in populated areas tightens the safety margins during operations, with landings, especially emergency ones, being critical phases.


These UAVs need to autonomously find Safe Landing Zones (SLZs), a challenge that has been difficult to overcome until now.


####


The Evolution of UAV Landing Systems


Previously, automatic UAV landing systems depended on advanced sensors and algorithms, limited by factors like operational altitude and the requirement for pre-mapped areas using SLAM (Simultaneous Localization and Mapping) technologies.


These limitations led to the exploration of more adaptable solutions, like DOVESEI, which still struggled with heuristic prompt selection, setting the stage for PEACE.


####


Introducing PEACE


PEACE (Prompt Engineering Automation for CLIPSeg Enhancement) marks a significant step forward in UAV landing technology by automating the creation of optimized prompts for better image segmentation, thus enhancing the accuracy of identifying SLZs with monocular cameras.


####


System Architecture


PEACE integrates with the UAV's onboard systems and includes:


- **PEACE Prompt Generation** : Automatically crafts optimized prompts for improved image segmentation.


- **Landing Heatmap Generation Service** : Produces heatmaps to aid in selecting landing zones, using a pre-trained semantic segmentation model.


- **Main Processing Node** : Coordinates UAV state and adjusts focus based on the heatmap.


####


Experimental Validation


PEACE underwent tests with high-resolution satellite imagery and the Aerial Semantic Segmentation Drone Dataset, showing significant enhancements in landing zone accuracy over both DOVESEI and traditional prompt systems.


####


Practical Implications and Future Directions


PEACE's development not only represents a technological breakthrough but also has significant implications for UAV operations in safety-critical environments.


It could lead to broader UAV usage in crucial areas like emergency response and urban planning.


Future enhancements might include incorporating few-shot learning and real-world testing to further improve PEACE's capabilities and adaptability.


####


Conclusion


PEACE exemplifies how innovative solutions can address aerial robotics challenges. By automating prompt engineering for better image segmentation, it significantly boosts the safety and reliability of UAV operations.


As UAV technology advances, systems like PEICE will be vital in ensuring these innovations are implemented safely and efficiently, opening new possibilities for their application across various industries.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
