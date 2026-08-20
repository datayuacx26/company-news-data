---
schema_version: "1.0.0"
document_id: "41f42d87a7fa5b916987c31dcca689f2e7dc0b2bde2ddfa1a66dd80e2948abcd"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/zero-2-2-update"
published_at: null
first_seen_at: "2026-08-13T16:32:36.595168+00:00"
fetched_at: "2026-08-13T16:32:37.605569+00:00"
content_hash: "sha256:c632a504dc506abd6c6afb3a0a77008b03a9e08fd9b5cd596ee56ac221c2d260"
---

# ZERO 2.2: Sharper Text-Prompt Accuracy and Improved Korean Prompt Understanding

Announcements


ZERO 2.2: Sharper Text-Prompt Accuracy and Improved Korean Prompt Understanding


Hyun Kim


Co-Founder & CEO | 2026/08/13 | 10 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


Superb AI has updated **ZERO** , its industry-specific Vision Foundation Model, to version 2.2. The release is now available in **Labs** on Superb Platform, so users can try it immediately with no installation and no training. ZERO 2.2 is also available on **AWS Marketplace** .


ZERO detects objects it has not been trained on using only a text prompt or an example image. It removes the conventional cycle of collecting data, labeling it, and retraining a model every time a new object needs to be detected.


Version 2.2 improves the accuracy and usability of that core capability: prompt-based detection.


# **What’s New?**


## **1. Higher Text-Prompt Detection Accuracy**


Detection performance for objects specified by text has improved significantly.


In internal testing, previous versions sometimes missed objects among multiple targets. In version 2.2, those missed detections have been clearly reduced.


Detecting eight chips in a semiconductor manufacturing line image using only the text prompt “blue chip”


For example, in an image of a semiconductor manufacturing line, entering only the text prompt “blue chip” detects the chips in the image, including those in slightly out-of-focus regions. For manufacturing environments where inspection targets change frequently, this means teams do not need to retrain a model for each new item.


## **2. Korean, Japanese, and Natural-Language Prompt Understanding**


Version 2.2 improves Korean prompt performance and understands descriptive phrases, not just single-word prompts.


Detecting cheese on a food factory conveyor using the Korean prompt “치즈” (“cheese”)


Detecting cup noodles held by a customer in convenience store CCTV footage using the Korean prompt “컵라면” (“cup noodles”)


When the Korean prompt “치즈” is entered for a food factory conveyor image, ZERO detects cheese blocks moving along the line. When “컵라면” is entered for convenience store CCTV footage, it detects cup noodles both on the shelf and in a customer’s hand.


There is no need to translate prompts into English. Teams can use the Korean terms they already use in the field. The same applies to Japanese as well.


Detecting only the person matching the descriptive prompt “orange jacket” among dozens of pedestrians


Descriptive expressions work as well.


In an image of a crowded plaza, entering “orange jacket” detects only the person wearing an orange jacket while excluding other pedestrians. This ability to specify targets using attributes such as color and clothing is especially useful in security and monitoring workflows.


## **3. Clearer Confidence-Score Distinction for Easier Operations**


When applying an object detection model to real-world workflows, teams must set a confidence threshold. If the threshold is too low, false positives increase. If it is too high, missed detections increase. This means operational difficulty depends heavily on how clearly the model separates scores for correct and incorrect detections.


In version 2.2, the score gap between correct and incorrect detections is wider than before, making threshold configuration easier. In internal testing, previous versions required a low threshold of around 0.1. Version 2.2 operates stably around the 0.2–0.3 range. This reflects higher overall confidence in detection results and helps reduce the time required to fine-tune thresholds for field deployment.


## **4. Visual Prompts: Detect Every Matching Object with a Single Example**


For objects that are difficult to describe by name, users can specify the target with a box instead of text. By drawing a box around one example object in an image, ZERO detects all objects of the same type in the scene.


Selecting one tomato with a box detects only tomatoes, excluding other fruits and vegetables with similar colors


Selecting one snack product on a shelf detects every unit of the same product


In an image containing fruits and vegetables, selecting one tomato allows ZERO to detect tomatoes while excluding visually similar citrus fruits or squash. On a convenience store shelf, selecting one specific snack product allows ZERO to detect all matching products on display.


This is especially effective for new products without widely used names, or for visually similar items that are difficult to distinguish through text alone.


# **Performance Validation: First Place at the CVPR 2026 Challenge**


The technology behind this update has been validated at a leading international conference. In June 2026, Superb AI took[first place in the Foundational Few-Shot Object Detection Challenge at CVPR](https://superb-ai.com/en/resources/blog/1st-place-cvpr-2026-few-shot-challenge-en) , the world’s largest computer vision conference.


The task was to detect objects across 20 specialized domains, including X-ray, thermal, and aerial imagery, using only 10 example images per class. Superb AI achieved an mAP of 53.9, outperforming the second-place team at 51.6 and the organizer baseline at 33.3. It also ranked first in five of the seven evaluation categories. This result marked a significant rise from fourth place in the same challenge in 2025 to first place in 2026. The technical approach behind the winning solution is covered in detail in our[winning solution walkthrough](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en) .


ZERO was designed from the beginning for deployment in industrial environments. It was trained on approximately 900,000 curated images selected from Superb AI’s proprietary industrial image dataset of around one billion images, and demonstrated stronger performance than existing open models across 37 industrial datasets.


Details on the training data composition and evaluation results are available in the[technical paper on arXiv:2507.04270](https://arxiv.org/abs/2507.04270) .


# **Why It Matters: Vision AI That Starts Without Training**


The highest cost in Vision AI adoption is not the model itself. It comes from[data preparation](https://superb-ai.com/en/resources/blog/cvpr-zero-data-flywheel-en) . Every time the detection target changes, teams must repeat the cycle of collecting images, labeling them, and retraining the model.


ZERO replaces that cycle with prompts.


A single line of text or one example box is enough to begin detecting a new object. In version 2.2, the accuracy and reliability of those results have improved even further. The impact is especially strong in environments where detection targets change frequently, such as manufacturing quality inspection, logistics inventory management, security monitoring, and retail store analytics.


# **Two Ways to Use ZERO 2.2**


## **Try It Directly on Superb Platform**


ZERO 2.2 is available in **Labs** on Superb Platform.


Upload a sample image or your own image, enter a text or box prompt, and check the detection results directly. No separate installation or infrastructure setup is required.


## **AWS Marketplace: Deploy Directly into Your Existing Cloud Infrastructure**


ZERO 2.2 is also available on[AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-xm2s45njvoiqk) . It can be deployed as an Amazon SageMaker endpoint and called through an API.


For companies already operating on AWS, ZERO can be adopted using an existing AWS account without a separate contract process. Pricing is usage-based, and a free trial is available.


[Explore ZERO on AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-xm2s45njvoiqk)


# **Recommended Reading: CVPR 2026 Series**


- [\[Part 1\] Superb AI’s ZERO Takes 1st Place in the CVPR 2026 Foundational Few-Shot Object Detection Challenge](https://superb-ai.com/en/resources/blog/1st-place-cvpr-2026-few-shot-challenge-en)
- [\[Part 2\] How ZERO Won the CVPR 2026 Foundational Few-Shot Object Detection Challenge: A Technical Walkthrough of the Winning Solution](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en)
- [\[Part 3\] “Lightweight Means Fast—and Fast Wins”: An Interview with CVPR-Winning ML Engineer Kyeongryeol Go](https://superb-ai.com/en/resources/blog/cvpr-win-interview-en)
- [\[Part 4\] Three Years of the Few-Shot Object Detection Challenge: Mapping the Global Vision AI Landscape](https://superb-ai.com/en/resources/blog/cvpr-challenge-global-vision-ai-landscape-en)
- [\[Part 5\] How to Restart an AI Project That Stalled for Lack of Data—with Just 10 Images](https://superb-ai.com/en/resources/blog/cvpr-zero-data-flywheel-en)


****


# **FAQ**


## **Q. What is ZERO?**


ZERO is Superb AI’s industry-specific Vision Foundation Model. It detects objects it has not been explicitly trained on using only text prompts or example image prompts, such as boxes. It does not require separate data collection, labeling, or retraining.


## **Q. What has improved in ZERO 2.2?**


ZERO 2.2 improves text-prompt detection accuracy, Korean and descriptive natural-language understanding, missed detection rates, confidence-score separation for easier threshold setting, and visual-prompt detection performance.


## **Q. Does ZERO support Korean prompts?**


Yes. ZERO supports Korean words such as “치즈” (“cheese”) and “컵라면” (“cup noodles”), as well as descriptive phrases that include attributes, such as “오렌지색 자켓” (“orange jacket”).


## **Q. Where can I try ZERO?**


You can try ZERO for free in **Labs** on Superb Platform. If you use AWS, you can deploy ZERO from AWS Marketplace as a SageMaker endpoint and adopt it with usage-based pricing.


## **Q. How has ZERO’s performance been validated?**


ZERO ranked first in the CVPR 2026 Foundational Few-Shot Object Detection Challenge with an mAP of 53.9. Its evaluation results across 37 industrial datasets are also included in the public technical paper on arXiv:2507.04270.


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Announcements Superb AI’s ZERO Takes 1st Place in the CVPR 2026 Foundational Few-Shot Object Detection Challenge Hyun Kim Co-Founder & CEO | 10 min read](https://superb-ai.com/en/resources/blog/1st-place-cvpr-2026-few-shot-challenge-en)[Announcements Celebrating Superb AI’s 8th Anniversary — 130 Million Images, Korea’s First Vision Foundation Model, and the Next Chapter of Physical AI Hyun Kim Co-Founder & CEO | 5 min read](https://superb-ai.com/en/resources/blog/8th-anniversary-en)[Announcements We're heading to NVIDIA GTC — and we have a lot to show you! Hyun Kim Co-Founder & CEO | 2 min read](https://superb-ai.com/en/resources/blog/superb-ai-nvidia-gtc-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
