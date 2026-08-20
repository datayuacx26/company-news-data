---
schema_version: "1.0.0"
document_id: "c4b80bd077dac937ff0970b119f540d3a1c94f1d86c0200c8eff928ec22d1025"
company_key: "yc-nyckel"
company: "Nyckel"
source_id: "yc-nyckel-news-import-fbb975919637"
canonical_url: "https://www.nyckel.com/blog/bounding-box-object-detection/"
published_at: null
first_seen_at: "2026-07-22T06:43:30.013332+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:90e645abac82ae008a1ae36c10fd7454cca79db489dd7cc9255a2ca99d19ad67"
---

# Are Bounding Boxes Necessary for Object Detection?

One of the primary applications of computer vision is object detection, which is used to locate and count objects in images or video frames. Object detection has thousands of practical applications and is one of the most commonly used forms of computer vision.


Typically, an object detector detects an object by drawing a rectangle (“bounding box”) around the object. This means that the training data also needs to be annotated with bounding boxes, which can be time consuming and tedious.


This begs the question: are bounding boxes always necessary? Are there more efficient ways to do object detection? In this article, we compare two platforms that use different object detection methods to make a case for an alternative to bounding boxes — center points.


## When are bounding boxes unnecessary?


If you want to know the rough size or extent of a detected object, you need bounding boxes. For example, if you’re detecting license plates so you can blur them, you need to know the extent of the plate.


But there are many cases when bounding boxes are unnecessary:


- When you want to detect the count of objects in an image. For example, counting the number of homes visible in aerial photographs.
- When you want to determine the position of objects within the frame without needing to understand their boundaries or size. For example, detecting if your product is on the top, middle, or bottom shelf in a grocery store.
- When you want to detect the presence or absence of an object when the object you’re detecting is small in relation to the rest of the image. For example,[Gardyn detects the presence of specific fruits](https://www.nyckel.com/blog/gardyn-reduces-workload-by-70-while-growing-2x-after-implementing-computer-vision/) in their indoor gardens. Some users would default to using[image classification](https://www.nyckel.com/blog/image-classification/) for this use case. However, since the fruits are very small compared to the entire image, using object detection yields better results.


From our conversations with Nyckel customers, the three bullet points above cover a significant chunk of object detection use cases. Any efficiencies gained from ditching bounding boxes in favor of center points would be compelling.


## Bounding boxes vs. center points


When an object detector uses bounding boxes, it draws rectangular boxes around each object the model detects. For training the model, annotators also need to draw boxes around the targeted objects. Annotating bounding boxes requires multiple clicks, or a click and drag, with extra attention paid to make sure the box doesn’t cover more or less than the bounds of the object. Annotation requires a lot of time and attention to detail.


Fortunately, boxing is not the only way to do object detection. Pointing to an object’s center (“center detection”) is another way and requires only specifying the position of a single point. We predict that annotating training data for center detection models is significantly faster. Let’s do a quick experiment to verify that claim.


## Let’s experiment: annotating bounding boxes vs. center points


Our hypothesis is that annotating with points is faster than bounding boxes. We tested our hypothesis by comparing two platforms that differ in this labeling procedure:[Vertex AI](https://cloud.google.com/vertex-ai/docs/image-data/object-detection/train-model) , which uses bounding boxes, and[Nyckel](https://www.nyckel.com/docs/detection-quickstart) , which uses center points. We measured the time it took to correctly annotate the same 50 images on each platform.


*Note: While we have a clear bias toward Nyckel’s product, we used a contractor who isn’t as familiar with our product as our internal team to conduct this experiment. To exclude one-time learning costs from the experiment, we asked him to familiarize himself with both products before conducting the experiment.*


### The input data


[Roboflow](https://roboflow.com/) has a set of 100 benchmarking datasets, called[Roboflow 100](https://universe.roboflow.com/roboflow-100) , available for this kind of experiment. We used their[grass weeds data](https://universe.roboflow.com/roboflow-100/grass-weeds) and selected 50 images at random from the dataset. Each image contained anywhere from one to 24 weeds. These are the kind of images included in the dataset:


Sample images from the Roboflow 100 dataset


### Our experience annotating with Vertex AI’s bounding boxes


When using Vertex AI, placing boxes near the edge of the frame sometimes resulted in misfires, adding a second or two each time. This was particularly a problem on the left margin because the UI only likes users to drag a rectangle from left to right. Also, correctly getting the minimal bounding box that includes a whole object doesn’t always work on the first try.


User draws bounding boxes to annotate data for object detector


### Our experience annotating with Nyckel’s center points


As you will see in the results below, annotating points was significantly faster. In addition, a noticeable difference with Nyckel is that Nyckel trains the model while you annotate, showing its predictions on training images after you’ve annotated just a handful of them. These model predictions are startlingly accurate, and they guide the eye in annotating subsequent images.


However, the suggested annotations occasionally include false positives and sometimes fail to identify borderline object cases, meaning that Nyckel’s suggestions need to be verified by a human eye during the annotating process.


User annotates data using center points for object detector


### The results


The difference is pretty striking between the two methods of object annotation: a factor of 3 difference! The additional clicks and image inspection time for the bounded box method quickly add up. If your use case doesn’t need bounding boxes, platforms using center-point detection have a clear advantage in annotation time and tedium.


### What about model accuracy?


So, how do the models generated by Nyckel and Vertex AI compare for this data? Great question! Stay tuned; we’ll be answering that question soon through a benchmark of the performance of different object detection products. In the meantime, you can[try us out](https://www.nyckel.com/docs/detection-quickstart) or read about how[Gardyn picked us](https://www.nyckel.com/blog/gardyn-reduces-workload-by-70-while-growing-2x-after-implementing-computer-vision/) over Azure because of better model accuracy.


## Conclusion: Bounding boxes vs. center points


Annotating center points is 3x less time-consuming than annotating bounding boxes. If you only need to detect the presence, number, and/or relative positions of objects, you are working too hard if you are using bounding boxes.


____


Thinking about deploying object detection? With Nyckel, you can annotate, train, and deploy an object detection model in minutes. Explore our[object detection clickable demo](https://www.nyckel.com/docs/detection-quickstart) or[sign up for a free account](https://nyckel.com/contact) to try it out for yourself.
