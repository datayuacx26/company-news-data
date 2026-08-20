---
schema_version: "1.0.0"
document_id: "e66fbf0b4e721f2892320280003cbc7ca63ca5aac70f00a4ce3e1fc853a27801"
company_key: "yc-mars-auto"
company: "Mars Auto"
source_id: "yc-mars-auto-rss-2183801a7a92"
canonical_url: "https://blog.marsauto.com/automated-map-generation-from-a-fleet-of-trucks-540a42f14ac2"
published_at: "2022-07-04T06:01:14+00:00"
first_seen_at: "2026-07-25T13:21:51.802210+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:42ccc42e4f0fae5ea5bc48bf31f772145dc932929be2f9759569ff43fae5bb42"
---

# Automated map generation from a fleet of trucks

# Automated map generation from a fleet of trucks


[Jeeseong Kim](https://medium.com/@jeeseong.kim?source=post_page---byline--540a42f14ac2---------------------------------------)


6 min read


·


Jul 4, 2022


--


Press enter or click to view image in full size


## **INTRODUCTION**


The goal of Mars Auto is to build superhuman driving software that makes fully driverless trucks a reality. To make this possible, Mars Auto solves autonomous driving with pure vision-based machine learning. Without relying on depth-sensing sensors, we train our model in a similar fashion to[how humans drive](https://twitter.com/elonmusk/status/1447588987317547014) . Built on top of a deep neural network, the key to an accurate autonomous driving system depends on the quality of the ground truth. One of the efforts to make our ground truth data reliable is to generate large-scale data in various conditions. The other is to make good quality processed data for our ground truth.


***“Our***[autonomous truck](https://medium.com/r?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DTCX1Owlw0IY) ***is made possible because of our proprietary visual SLAM and vision-based neural network model trained with diverse sets of large-scale driving data.”***


Figure 1. Visualization of Multisession SLAM


## Why Multisession SLAM?


Single session driving can only observe a limited zone and occlusion issue is inevitable. For example, some important lane marking, lane lines, or lane boundaries can be occluded by nearby driving vehicles. On the contrary, with multisession SLAM we can prevent this kind of occlusions by perceiving more information from multiple views. Let’s imagine that you are observing a sculpture. The more you change your viewpoints, the more details could be seen. As a result, multisession SLAM benefits our system with rich information, which allows us to observe occluded areas and the ability to see further.


## Get Jeeseong Kim’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


The advantage of multisession SLAM is that we can merge multiple sub-maps that share common regions. For instance, the Gyeongbu expressway is made up of 4 to 10 bidirectional lanes. Somedays our drivers drive in the last lane, on other days they drive in the second or third lane. The common thing is that all of the driving sessions belong to the Gyeongbu expressway. Consequently, we can merge all of the driving sessions on the Gyeongbu expressway regardless of the driving lanes and time to map the whole Gyeongbu expressway.


Figure 2. Visualization of Multisession SLAM for multiple driving lanes


We generate our temporal aware global map with a diverse set of driving sessions, each of which includes different illumination and weather conditions. Especially on rainy days or snowy days, the acquired frames from cameras can be blurry, so the important visual features can be corrupted and it degrades the constructed map. But if we have other driving sessions that have common regions with rainy or snowy driving, the missing observations from each session can be compensated to improve the constructed map. Consequently, by merging more and more driving sessions in various conditions, our map can be robust in every condition.


## **Multisession SLAM to maximize the value of our diverse driving sessions**


Multisession SLAM can be achieved by merging different single sessions. However, as we are not relying on depth-sensing sensors, it makes the problem extremely hard. Visual sensors are vulnerable to weather or illumination changes, and meter scale depth cannot be directly observed from the camera. In addition, since all the driving sessions are generated at different times and spaces, noise such as IMU sensor noise, and GPS noise is different. Hence, naive approaches such as aligning the sessions with respect to their map point positions directly or GPS coordinates will not work. Figure 3 visualizes the difference between aligning with GPS coordinates and our multisession SLAM method. We can easily check out from the visualization that the naive method fails miserably. Therefore, an accurate single-session SLAM and smooth merging algorithm are essential for robust multisession SLAM.


Press enter or click to view image in full size


Figure 3. (Left) Visualization of our map with our alignment method in bird-eye-view. (Right) Visualization of our 3d map points being aligned in the image. Even though this region is in one lane, merging different driving sessions without our alignment method seems as if has multiple driving lanes. The driving path is converged to mean value, and all feature points are aligned to their actual position.


There are two main ingredients that enable us to create a robust global map in our multisession visual slam system, **1) dense feature extraction** and **2) precise alignment method** , which makes our map robust. Our dense feature extractor boosts up each SLAM, and the precise alignment method fuses multiple driving sessions seamlessly.


1. **Dense Feature Extractor**


The conventional hand-crafted feature extractors (e.g., ORB, SURF, SIFT), suffer from their sparsity and are not robust in extreme illumination changes. Since we do not rely on depth-sensing sensors, a conventional hand-crafted feature extractor is not suitable for our system. Therefore, we built a deep-learning-based feature extractor to improve the feature extraction. As a result, our proprietary feature extractor can detect features in extreme illumination, weather changes, and even blurry images. Since visual SLAM uses only extracted features throughout the whole SLAM session, our dense features is one of the main points that make our map dense. In addition, thanks to our feature extractor every feature extracted from different sessions can be coherent. Therefore merging different sub-maps in our multisession SLAM can be done smoothly. This makes our SLAM so powerful that it can generate a compact, trackable map and support accurate 6-DoF localization.


In SLAM, usually loop closing is the magic part that compensates for all of the accumulated errors. For example, if the agent revisits a certain position, the loop is generated. In this case, we can compensate for the accumulated errors by using the constraint that the revisited positions should be the same. But, loop closing can only happen when the loop exists. Mars Auto targets “warehouse to warehouse” driving so in this case loop is not usually generated. On behalf of loop closing, the key to preventing accumulated error is to make tracking as accurate as possible. Another part of our feature extractor that makes tracking robust is the descriptor. With the help of our robust descriptor, tracking can enhance the accuracy of our single-session SLAM, which leads to powerful multisession SLAM. This makes our tracking more refined and capable of keeping our tracking without any possible outliers.


**2. Precise alignment method**


Lastly, our precise alignment algorithm can fuse an unlimited number of driving sessions seamlessly. Merging the result of multiple single-session SLAM, which are the predicted 6-DoF poses and generated maps for multiple driving sessions is tough. One main reason is due to the similarity of the visual contents in an expressway. It lacks distinct visual features to differentiate one spot of an expressway from another. A visual place recognition approach including the conventional Bag-of-visual-word (DBoW2) and even the deep-learning-based approach could lead to many wrong matchings. To overcome this, we put great efforts to develop a very precise and robust alignment algorithm that is robust to the lack of visual features and weather conditions, and occlusions to be able to merge multiple sessions seamlessly.


## CONCLUSION


Starting this September, we will be collecting more and more driving sessions from a large fleet of trucks. With our multisession SLAM, which can merge limitless number of driving sessions seamlessly. Building a temporal aware global highway map is just the matter collecting a very large scale of data that is covering the whole country.


Building an accurate large-scale map is one of the important jobs for our autonomous truck. Anyone who has an interest in building a super robust large-scale map please[join us](https://marsauto.com/careers) on our journey to the future!
