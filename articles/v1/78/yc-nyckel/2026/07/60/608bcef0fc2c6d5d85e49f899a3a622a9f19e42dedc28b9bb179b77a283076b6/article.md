---
schema_version: "1.0.0"
document_id: "608bcef0fc2c6d5d85e49f899a3a622a9f19e42dedc28b9bb179b77a283076b6"
company_key: "yc-nyckel"
company: "Nyckel"
source_id: "yc-nyckel-news-import-fbb975919637"
canonical_url: "https://www.nyckel.com/blog/gardyn-image-classification-case-study/"
published_at: null
first_seen_at: "2026-07-22T06:43:30.013332+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:709d665637b7ffd8837ab545783f7681d9e17d24e3eacd617200a8b8caf23c52"
---

# Gardyn Reduces Workload by 70%, Grows 2x After Implementing Computer Vision

[Gardyn](https://mygardyn.com/) makes reliable home growing easy. Their smart, indoor vertical hydroponic systems help customers monitor the growth and health of their plants, notifying customers via Gardyn’s app when their plants need attention.


"Because of the way Nyckel does things, the model trains immediately. This was rather shocking and awesome!"


Sunil Rawal


AI Lead, Gardyn


Gardyn uses Nyckel image classification to monitor the health of its customers’ plants 85% reduction of manual review Accuracy higher than Microsoft Azure Custom Vision Model release cycle reduced from weeks to days Elastic invoke endpoint allows fast batch processing


## About Gardyn


Gardyn’s devices make the nutrition and taste benefits of fresh produce available to anyone: no garden space or green thumb required. “Growing outdoors non-commercially is extremely wasteful of water. We save 95% of the water used,” said Sunil Rawal, Gardyn’s AI Lead.


To help customers keep their plants healthy, Gardyn uses technology that controls pumps and monitors humidity and temperature. But they also need rich data that goes beyond this basic telemetry. To get this, Gardyn added a pair of cameras to its plant systems, capturing multiple snapshots of its customers’ plants per day and sending the images to the cloud for manual review by their botanists.


## The Challenge


Manual botanist review worked well when the company had only a few hundred devices in use. But by the time Sunil joined Gardyn to lead their AI effort, the company had a fast-growing number of devices in use, with a team of botanists manually training the system and addressing customer queries. The botanists couldn’t keep up with monitoring the plants and responding to customer inquiries.


"The moment that we decided to put cameras on was the moment we decided to embrace computer vision wholesale."


Sunil Rawal


AI Lead, Gardyn


Gardyn knew they had a scaling problem. The minimum review and notification frequency for customers had been stretched to two weeks. Gardyn risked harming customer engagement if they extended that period any longer. It was time to automate some of the telemetry review.


“These are botanists. They’re highly trained people. To have them sitting there, looking at pictures of 300 devices every day saying, ‘Yes, it’s fine,’ is just not smart” said Sunil.


It was a top-level company initiative and mission to automate the process, led by the executive team and plant experts, so they could instead shift their time and attention to more creative and engaging tasks that required their skillset.


But finding the right AI solution wasn’t an easy task. The team first tried Microsoft’s Azure Custom Vision platform, which worked well for many simple cases. However, some superficially easy computer vision problems, such as detecting the presence of strawberries, were nearly impossible due to the number of situational variables.


Gardyn needed a computer vision system that could correctly detect a strawberry


Unfortunately, it took a huge investment of time to train a model that was still not nearly accurate enough for distinguishing plant growth classes. This was frustrating and bad news in terms of business growth. Plus, it wasn’t obvious how to tweak the model. Adding more data wasn’t improving the separation of classes. How many more hours of model training service time should one pay for in a situation like that?


"\[Azure's\] workflow was so painfully slow. I couldn’t get the models to a place where I wanted them. I was looking for something fast."


Sunil Rawal


AI Lead, Gardyn


Sunil wanted the ability to iterate the model and a quick model training process. Gardyn was growing at 2x per year, and it was not feasible from a business perspective to invest an unknown number of days and weeks in refining AI model settings.


## The Solution


After performance plateaued on Microsoft’s platform, Sunil found Nyckel.


“The time between me seeing Nyckel on a Google search and having a model was a few minutes,” said Sunil.


Sunil’s experiments with using Azure Custom Vision to classify plants as healthy or in distress yielded mixed results. Custom Vision wasn’t accurately detecting a plant in distress when 90% of the plant was fine but the remaining 10% was distressed. These plants needed attention, but Microsoft’s platform wasn’t able to detect them.


Nyckel, on the other hand, was able to handle these difficult cases with high accuracy in ways Custom Vision couldn’t. Nyckel also allowed fast iteration, increasing productivity and bringing down the time it took Gardyn to create a new model from weeks to just days. Plus, the on-demand scaling of model invokes on Nyckel allowed Gardyn to quickly process a night’s worth of images.


"It worked! It worked after about a thousand training images. The degree of sensitivity to the range of issues in those images was better. It was qualitatively different with Nyckel."


Sunil Rawal


AI Lead, Gardyn


Gardyn had to make sure Nyckel was a system that could be integrated, from a technical and financial point of view, to the rest of its ecosystem. This was not difficult in light of Nyckel’s clear technical and efficiency advantages.


## The Results


Since starting to use Nyckel, Gardyn has **“achieved a 70% reduction in workload over the past year, while growing 2x,”** said Sunil.


Gardyn has continued to improve its Nyckel AI implementation by building a monitoring system to solve the problem of false negatives and false positives. Sending out a message to a customer saying that their plant is diseased and that they should take the steps to fix it when it’s not actually diseased is worrying for the client and harmful to Gardyn’s reputation. Gardyn doesn’t want to look unintelligent when one of its selling points is smart monitoring!


Because of the high business cost of false positives, Gardyn sets a very high confidence threshold for positive flags, only automatically forwarding classifications to customers that reach that very high level of confidence. Gardyn’s team of plant experts inspect lower confidence results daily to manually drive down the false positive rate before sending notifications each morning. They also feed some of this manually inspected data back to the model to continuously improve its performance.


Gardyn’s very active Facebook community of “Gardyners” keeps growing at a breathtaking pace with a very strong level of engagement. Ramping up the power of AI at Gardyn has proved to bring way more value and this is only the beginning… for the Gardyners’ delight! Service was, from the customers’ perspective, unchanged, despite the massive reduction in workload at the backend.


Gardyn continues to deliver an exceptional customer experience — all while using Nyckel in the background to support it.


---


Interested in exploring how computer vision could support your business?[Sign up to give Nyckel a try for free](https://login.nyckel.com/) , or check out our computer vision quick starts for[image classification](https://www.nyckel.com/docs/image-classification-quickstart) ,[image search](https://www.nyckel.com/docs/image-search-quickstart) , and[object detection](https://www.nyckel.com/docs/detection-quickstart) .
