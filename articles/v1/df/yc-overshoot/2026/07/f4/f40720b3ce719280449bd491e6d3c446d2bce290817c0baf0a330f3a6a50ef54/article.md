---
schema_version: "1.0.0"
document_id: "f40720b3ce719280449bd491e6d3c446d2bce290817c0baf0a330f3a6a50ef54"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/vlms-can-see-objects-but-why-cant-they-count-them"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-28T01:56:33.200536+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:67413f231c94de319a1114623268e67340303ab726de2a6effd358d2ceedfb9e"
---

# VLMs Can See Objects. But Why Can't They Count Them?

## A Simple Task That Is Surprisingly Hard


Vision-language models can describe images, read text, answer questions, and explain what is happening in a scene. In many cases, the results are impressive.


But there is one basic visual task that these models are frequently unable to perform reliably: counting.


It is surprisingly non-trivial for a model to count how many objects there are. At first glance, this seems odd. If the model can see objects, you would assume that it should also be able to count them.


However, counting requires more than simple recognition. A model that can report "boxes on the shelf" is not necessarily capable of reporting "47 boxes on the shelf."


Recognition is not the same as enumeration.


## Where Does Counting Go Wrong?


Why can't a model count how many objects there are?


A common issue is ambiguity. Real-world scenes can be quite challenging for a model to interpret. The image might be cluttered, or objects might be bunched together and hard to distinguish. In other cases, the model might be asked to count things that are partially occluded, or objects that are printed on a page.


Before a model can count reliably, it must first decide which visual regions qualify as separate instances. It may mistake a reflection or printed image for a real object, overlook an object that blends into the background, or fail to separate several objects that are grouped closely together. In these cases, the model is not merely counting incorrectly. It is uncertain about what should be counted in the first place.


The task becomes even harder when the question refers to a specific subtype of object. Counting all the eggs in an image is easier than counting only the brown eggs when brown and white eggs are mixed together. Likewise, counting all the clocks is easier than counting only the clocks showing 2:30, especially when several clocks have similar faces but display different times.


Grounding step from LVLM-Count, showing candidate detections for "brown egg" in a mixed-egg image. Source: Qharabagh et al., 2024.


However, even in carefully controlled settings, counting can still be unexpectedly hard. Research using simple geometric shapes has shown a compositional breakdown: VLMs perform significantly worse when asked to count images containing multiple object types, even when the objects are simple shapes like circles, triangles, or squares \[Guo et al., 2025\].


VLMCountBench tests whether VLMs can count simple shapes under different levels of compositional difficulty. Source: Guo et al., 2025.


For example, on VLMCountBench, Qwen2.5-VL-72B scores 0.60 accuracy in the Level 1 setting, where there is only one shape type to count, but drops to 0.45 accuracy in the Level 3 setting, where three shape types must be counted separately \[Guo et al., 2025\].


A related issue occurs when the total number of objects increases. Large-count evaluations have shown that some VLMs perform better on small quantities, often fewer than 20 objects, but performance deteriorates as object counts grow larger \[Qharabagh et al., 2024\].


These results highlight two distinct counting challenges:


- Compositional complexity: different types of objects must be identified and counted separately.
- Count magnitude: the model must keep track of a larger number of individual objects.


## Counting Is Not Simply Prompt Engineering


A natural approach to these challenges is to improve the prompt. After all, if the model fails to count, perhaps it needs to be prompted more carefully.


Research has explored prompting strategies such as spatial decomposition, where the model is asked to count objects in different regions of the image and then combine the partial counts. Similarly, another strategy is type decomposition, where the model is asked to count different object types separately, one type at a time, and then add the total counts together \[Guo et al., 2025\].


However, these decomposition methods are not always effective. In controlled geometric-shape counting settings, spatial decomposition slightly hurt performance, resulting in lower accuracy and higher relative error. Type decomposition caused an even larger performance drop \[Guo et al., 2025\].


Better prompting alone is not always enough.


Sometimes the model needs a better visual pipeline altogether.


## One Approach: Divide and Count With Visual Assistance


An alternative approach to improving performance is to improve the pipeline around the model.


As discussed earlier, one prompting strategy is to ask the model to count different regions separately. LVLM-Count takes this idea further by physically dividing the image into smaller sub-images before asking the model to count them.


This method does not involve additional training. Instead, it combines a grounding model, a segmentation model, object-aware division, and a VLM counting step to create a better counting process \[Qharabagh et al., 2024\].


LVLM-Count pipeline for divide-and-conquer object counting. Source: Qharabagh et al., 2024.


At a high level, this counting pipeline works like this:


1. **Image + question are given as input.**
2. **The target type to count is extracted from the question.**
3. **A grounding model locates the relevant region of the image.**
4. **A segmentation model identifies the target instances.**
5. **The image is partitioned in an object-aware way, so that the crop does not cut through objects.**
6. **The VLM is used to count the cropped images.**
7. **The counts from the sub-images are added together.**


This is a divide-and-conquer approach to visual counting.


Unlike spatial-decomposition prompting, LVLM-Count actually partitions the image before counting, so the model processes smaller, object-aware sub-images rather than reasoning over the full scene at once.


The key to making this method work is the object-aware partitioning. A naive approach, such as splitting the image vertically down the middle, risks cutting target objects in half, which can lead to double-counting.


Object-aware partitioning avoids this issue by partitioning the image in a way that respects object boundaries.


This method works by first estimating split locations based on the masks of the target objects. The masks are used to sample pixel locations, whose horizontal positions are then analyzed to calculate potential split locations. Using mean-shift clustering, natural breaks between groups of pixels can be estimated, which helps determine the split locations on the x-axis \[Qharabagh et al., 2024\].


Finally, the split path is estimated using the masked regions as obstacles. By considering the mask pixels as blocked space and the unmasked pixels as free space, the split path can be found using the A* search algorithm \[Qharabagh et al., 2024\].


In simple terms, the algorithm finds empty space in the image, then splits the image along that empty space, separating groups of target objects without cutting through them.


This approach is very different from simply asking the model to count more carefully.


## Why Does This Work?


This divide-and-conquer approach helps because it changes the problem. Instead of being asked to count many objects in a large image, the VLM is asked: "How many objects are there in this smaller crop of the image?" The crop contains fewer objects, making the scene easier to analyze.


The results are meaningful.


On FSC-147, a standard benchmark for object counting, LVLM-Count improves performance across multiple VLMs. GPT-4o's mean absolute error, or MAE, decreases from 25.57 to 17.86. Gemma 3's MAE decreases from 30.59 to 20.25, and Qwen2's MAE decreases from 34.18 to 22.29.


Similarly, on Emoji-Count, a benchmark involving emoji graphics, the improvements are especially striking for Qwen2. Its MAE decreases from 78.05 to 24.43. GPT-4o's MAE decreases from 23.57 to 16.57, and Gemma 3's MAE decreases from 21.39 to 16.16 \[Qharabagh et al., 2024\].


## Where Do We Go From Here?


Still, this does not mean that VLM counting is solved. More recent research suggests that counting remains difficult even for modern frontier models. Studies published in 2026 have also found failures on simple shape-counting benchmarks for Qwen3-VL. These findings suggest that the problem may extend beyond object recognition. A model may internally represent the approximate quantity while still failing to map that representation to the correct numerical answer \[Pang et al., 2026\].


Counting demonstrates an important limitation of vision-language models in real-world applications. These models are often competent at recognizing what is in an image, but they can still be limited in their ability to count objects reliably. This means that, for some practical applications, they may not be ready for deployment as standalone counting systems.


More broadly, counting highlights the value of hybrid vision systems. VLMs may struggle when asked to solve the entire task on their own, but they can become more reliable when paired with specialized tools for grounding, segmentation, and image partitioning.
