---
schema_version: "1.0.0"
document_id: "31915b7e19cc5956c038af614a4c0f605ffeea0efb5274fc2eecb4cbbc0e2dac"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/qwen3-8-max/"
published_at: "2026-08-06T19:27:26+00:00"
first_seen_at: "2026-08-06T21:53:54.463207+00:00"
fetched_at: "2026-08-06T21:53:56.368839+00:00"
content_hash: "sha256:44a6ae9c28b17727b04321fe1fc5586dbed778c2194c114cfe2d558249113dd6"
---

# Qwen3.8-Max for Vision: Benchmarks, Strengths, and Real-World Tests

[Piotr Skalski](https://blog.roboflow.com/author/skalskip/)


Published Aug 6, 2026 • 6 min read


This week, Alibaba released Qwen3.8-Max, its most capable[Qwen](https://playground.roboflow.com/models/qwen?ref=blog.roboflow.com) model so far. The model has 2.4 trillion total parameters and uses a Mixture-of-Experts architecture, activating around 95 billion parameters for each query.


Qwen3.8-Max accepts text, images, and video as input and returns text. The model is available today through the Alibaba Cloud API, and you can test it immediately in[Roboflow Playground](https://playground.roboflow.com/object-detection/gemini-3-6-flash-vs-gpt-5-6-sol-vs-claude-opus-5-vs-qwen3-8-max?ref=blog.roboflow.com) . Open weights are scheduled for release on August 12, 2026. Alibaba also plans to release a smaller, dense Qwen3.8-27B model on the same day.


Qwen3.8-Max compared with leading VLMs in Roboflow Playground


We tested Qwen3.8-Max across our upcoming VLM benchmark. It delivered the strongest object detection results we have seen from a VLM, while also performing well on counting and visual reasoning. In this post, we examine those results, its more advanced detection capabilities, and the trade-offs around text extraction, latency, and deployment.


## Object detection


Qwen3.8-Max is currently the best object detection[VLM in our benchmark](https://playground.roboflow.com/evals?ref=blog.roboflow.com) .


Most VLMs have clear strengths and weaknesses. One model might handle documents well but struggle with crowded scenes. Another might find large objects while missing smaller instances.


Qwen3.8-Max performed well across satellite imagery, infrared imagery, documents, technical drawings, hand-drawn sketches, diagrams, crowded scenes, and images containing small objects.


Traditional object detection benchmarks often focus on common objects in natural scenes. Production workloads include scanned documents, industrial images, aerial footage, medical packaging, technical diagrams, and unusual camera sensors. Qwen3.8-Max handled many of these domains without task-specific training.


### Prompt with text


Traditional object detection models return bounding boxes, class labels, and confidence scores directly. A VLM generates text, so object detection requires a different workflow.


We ask Qwen3.8-Max to return a JSON list containing a label and four coordinates for each detected object. The response is then parsed and converted into bounding boxes on the original image. Qwen models work best with XYXY coordinates normalized to a range from 0 to 1000.


The requested format has a large effect on accuracy. Changing the coordinate order, scale, or response structure often lowers detection performance. Each VLM family requires a prompt designed around its preferred output format.


### Prompt with boxes


Text labels are not the only way to tell Qwen3.8-Max what to detect. Instead, we can mark one object with a bounding box and ask the model to find all other objects of the same class. For example, we marked a single pill, and Qwen3.8-Max returned boxes around the remaining pills in the image.


This method is useful when a class is difficult to describe with words. The distinction might depend on shape, texture, damage, orientation, or another small visual detail. A bounding box gives the model a direct example of the target.


We also tested positive and negative boxes. Positive boxes show objects the model should find. Negative boxes identify similar objects that should be ignored.


We compared two prompting methods: coordinates included in the text prompt and boxes drawn directly on the image. Both approaches produced very similar results, but drawing boxes directly on the image performed slightly better, reaching 80.8% mAP.


A single box often left the target open to interpretation. In a soccer example, one marked player led Qwen3.8-Max to detect players from the same team rather than everyone on the field. In a Lego example, the model followed the color of the marked brick instead of its shape.


## Object counting


Qwen3.8-Max tied Gemini 3.6 Flash for first place in our object counting benchmark.


One test asked Qwen3.8-Max to count abnormal or defective candies in an image where some candies are larger than the others. The model had to compare the size of all visible candies, identify which ones deviate from the normal size distribution, and count only those that appear anomalous. The final output was required as a single integer.


Qwen3.8-Max handled many visually complex questions but still struggled in dense scenes and tasks with subtle class definitions. In a metal parts counting task, it was asked to count flat metal brackets in a blue bin and return a single integer. The model estimated 10, while the ground truth was 12.


These errors are common in cluttered scenes with overlapping objects, similar shapes, and strong lighting effects like glare and shadow, which can challenge even strong object detection systems.


## Visual reasoning


Visual reasoning combines image understanding with rules, relationships, calculations, or task-specific instructions.


For example, Qwen3.8-Max inspects medication boxes organized by day and verifies whether each patient received the correct dosage. Another workflow checks whether workers have packed every required component of an order before shipment.


The model also handles tasks involving numbers and visual structure. We tested receipt calculations where Qwen3.8-Max had to read selected items, subtract them from the total, and return the remaining amount.


Across our evaluations, Qwen3.8-Max ranks second only to Gemini 3.5 Flash in overall visual reasoning performance, and remains one of the strongest open-access VLMs for structured understanding tasks.


## Data extraction


We also tested targeted data extraction, where the model must locate and return specific information from an image under strict formatting constraints. This was one of the clearest weaknesses in the Qwen3.8-Max results.


Qwen3.8-Max ranked tenth on our data extraction leaderboard, around 10 percentage points behind stronger models such as Gemini 3.5 Flash, Claude Sonnet, and Muse Spark 1.2.


In one example, we used a handwritten note and asked for a specific date located in a defined region of the page, requiring the model to identify the correct area and return the value exactly as written.


In a blister pack example, we asked for an expiration date located in a specific position on the packaging. Even when the location was clearly indicated, the model sometimes misread or hallucinated the value.


## Local deployment


Qwen3.8-Max is a 2.4-trillion-parameter Mixture-of-Experts model with around 95 billion active parameters per query.


The architecture reduces the amount of computation used for each request, but the full model still requires datacenter-scale infrastructure. Most users will access Qwen3.8-Max through an API rather than running the model locally.


Alibaba plans to release the Qwen3.8-Max weights on August 12, 2026. The release will also include Qwen3.8-27B, a dense 27-billion-parameter model aimed at more accessible deployment.


Qwen3.8-27B is a more realistic option for local inference, quantization, and fine-tuning. We do not yet know how close the smaller checkpoint will get to the Max model on object detection, counting, or reasoning.


## Cost and speed


Qwen3.8-Max sits in a similar price range to models such as Muse Spark 1.2, Claude Sonnet, and Gemini 3.5 Flash in our benchmark.


Latency is the larger trade-off. Qwen3.8-Max was among the slowest models tested, taking longer on average than[GPT-5.6](https://blog.roboflow.com/openai-gpt-5-6/) and Claude Fable.


For offline processing, annotation, and workflows where accuracy matters more than immediate feedback, the latency might be acceptable. Real-time systems will need a faster model or a pipeline that uses Qwen only for selected images.


## Conclusion


Qwen3.8-Max leads our object detection benchmark and performs well across image domains where other VLMs often struggle. The model handles satellite images, documents, diagrams, crowded scenes, and small objects without task-specific training.


The upcoming open-weight release on August 12 will be key to understanding how much of this performance carries over to self-hosted deployments and how the Qwen3.8 family evolves beyond API access.


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Piotr Skalski](https://blog.roboflow.com/author/skalskip/) . (Aug 6, 2026). Qwen3.8-Max for Vision: Benchmarks, Strengths, and Real-World Tests. Roboflow Blog: https://blog.roboflow.com/qwen3-8-max/*


[Model Playground Compare VLM Models Side-by-Side](https://playground.roboflow.com/)


### Written by


Piotr Skalski


ML Growth Engineer @ Roboflow | Owner @ github.com/SkalskiP/make-sense (2.4k stars) | Blogger @ skalskip.medium.com/ (4.5k followers)


[View more posts](https://blog.roboflow.com/author/skalskip/)


### Topics


- [Aerial Imagery](https://blog.roboflow.com/tag/aerial-imagery/)
- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Multimodal](https://blog.roboflow.com/tag/multimodal/)
- [OCR](https://blog.roboflow.com/tag/ocr/)
- [Object Detection](https://blog.roboflow.com/tag/object-detection/)
