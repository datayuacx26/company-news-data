---
schema_version: "1.0.0"
document_id: "0cf17922c19a590a1018259d33c69f5e8b6e90644d5f731e76d93558a066020c"
company_key: "yc-dragoneye"
company: "Dragoneye"
source_id: "yc-dragoneye-news-import-90c782d80d34"
canonical_url: "https://dragoneye.ai/blog/native-object-tracking/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-24T05:13:15.346930+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:f8140b834e2f592d0dfed56229b7c43c8bc8c210e4caa721db47933c9d364dfa"
---

# Native Object Tracking in Dragoneye

Today, we’re shipping **object tracking** as a first-class format in Dragoneye. Instead of returning bounding boxes and classifications per timestamp, the API now returns objects tracked across the full video. Each object contains its category and attribute classifications, along with the timestamps and bounding boxes for where it appears in the video.


This brings the API output closer to how teams use Dragoneye today, and unlocks new use cases such as instance counting and spatial-temporal reasoning.


## Contents


- What Changed


- Why it matters


- Example Responses


- Before: Timestamp-Level Detections
- After: Object-Level Detections


- What Object Tracking Unlocks
- Start Building


## What Changed


Previously, Dragoneye grouped detections entirely by timestamp. Each frame came back as an independent list of bounding boxes and classifications, with no link between them. If a single person walked across a 10-second video, you received hundreds of disconnected “person” classifications. To figure out it was the same person, you had to write custom post-processing scripts or run object tracking algorithms on your end.


Object tracking inverts that. Detections are now grouped by object. Each object carries its class, its attributes, and the list of timestamps where it appears, with the bounding box at each one. Identity is preserved across the whole video, allowing you to understand where the object moved on screen, how long it was present, and what it did at different points in time.


### Why it matters


- **Less post-processing:** teams were rolling their own tracking on top of our per-frame output, which was fiddly to tune and maintain. Now that comes directly from the API.
- **More accurate tracking:** our models produce rich embeddings per detection, which we leverage to get more accurate tracking results.
- **Built for production questions:** first-class objects let you answer questions about duration, movement, and interactions directly without manually parsing frame data: count one pallet across a clip, measure how long a customer stayed near an end cap, or draw a path from bbox observations.


## Example Responses


To help illustrate the differences, let’s look at the response for a street-level video, the kind of footage analyzed for municipal management, real estate, or insurance use.


The visualization shows how the houses are tracked throughout the video, with fixed colors for each distinct house.


Each house keeps a fixed color as it's tracked across the clip - one stable object identity per property.


Now let’s take a look at the difference in the API responses. We’ll present JSON responses here for clarity; the response objects in our[Python](https://pypi.org/project/dragoneye-python/) and[Node](https://www.npmjs.com/package/dragoneye-node) packages are very similar.


### Before: Timestamp-Level Detections


With timestamp-level detections, the detections for a single house are scattered across each of the timestamps that the house shows up in. Even though it’s the same house, there’s no easy way to stitch together the detections to get a comprehensive understanding.


```text
[
{
"image_id"  :   "frame_0"  ,
"timestamp_microseconds"  :   0  ,
"normalized_bbox"  : [  0.12  ,   0.25  ,   0.55  ,   0.78  ],
"bbox_score"  :   0.97  ,
"predictions"  : [
{
"category_id"  :   2084323334  ,
"name"  :   "House (detached)"  ,
"score"  :   0.92  ,
"attributes"  : [
{
"attribute_id"  :   1371766615  ,
"name"  :   "Building Exterior Color"  ,
"options"  : [
{   "option_id"  :   3498033303  ,   "name"  :   "White / Off-white"  ,   "score"  :   0.85   },
{   "option_id"  :   496739380  ,    "name"  :   "Light gray"  ,          "score"  :   0.10   }
]
}
]
},
...   #   other   detections
]
},
{
"image_id"  :   "frame_1"  ,
"timestamp_microseconds"  :   1000000  ,
"normalized_bbox"  : [  0.13  ,   0.26  ,   0.56  ,   0.79  ],
"bbox_score"  :   0.96  ,
"predictions"  : [
{
"category_id"  :   2084323334  ,
"name"  :   "House (detached)"  ,
"score"  :   0.91  ,
"attributes"  : [
{
"attribute_id"  :   1371766615  ,
"name"  :   "Building Exterior Color"  ,
"options"  : [
{   "option_id"  :   3498033303  ,   "name"  :   "White / Off-white"  ,   "score"  :   0.82   },
{   "option_id"  :   496739380  ,    "name"  :   "Light gray"  ,          "score"  :   0.12   }
]
}
]
},
// ... other detections
]
},
...
]


```


### After: Object-Level Detections


The exact same detections are now reorganized around objects. In the context of municipal or insurance street-level video, this means you can instantly count unique properties and drive condition analysis per house. Of course, you can always explode the object results out to the frame level when needed, such as for drawing visualizations.


```text
{
"objects": [
{
"object_id": 1,
"timestamp_ranges": [
{ "timestamp_start_us_inclusive": 0, "timestamp_end_us_inclusive": 30000000 }
],
"bbox_observations": [
{ "timestamp_microseconds": 0,       "normalized_bbox": [0.12, 0.25, 0.55, 0.78], "bbox_score": 0.97 },
{ "timestamp_microseconds": 1000000, "normalized_bbox": [0.13, 0.26, 0.56, 0.79], "bbox_score": 0.96 },
{ "timestamp_microseconds": 2000000, "normalized_bbox": [0.14, 0.27, 0.57, 0.80], "bbox_score": 0.95 },
..., # more observations
],
"categories": [
{
"category_id": 2084323334,
"name": "House (detached)",
"score": 0.92,
"attributes": [
{
"attribute_id": 1371766615,
"attribute_name": "Building Exterior Color",
"option_id": 3498033303,
"option_name": "White / Off-white",
"timestamp_ranges": [
{ "timestamp_start_us_inclusive": 0, "timestamp_end_us_inclusive": 2000000, "score": 0.82 }
]
}
]
}
]
},
... # other detected objects
]
}
```


## What Object Tracking Unlocks


Object-level results make it easier to build applications that reason about objects across space and time, such as:


- **Warehouse and Logistics:** Track packages and pallets across the fulfillment process for quality control and throughput monitoring.
- **Property and Insurance Analysis:** Track houses, roof features, exterior damage, windows, doors, or other property features across street-level or walkthrough video.
- **Retail:** Calculate the dwell time of customers in specific aisles or in front of point-of-sale (POS) displays or end caps to determine effectiveness and ROI.
- **Statistical Surveys:** Understand the composition of vehicle flow from traffic cameras, broken down by vehicle type, make, and model. Useful in transportation studies or marketing research.
- **Inspection workflows:** Track safety equipment, fixtures, defects, and required objects across site walkthroughs.
- **Operations:** Monitor people and equipment flows through busy thoroughfares and identify congested regions
- **Robotics:** Track target objects that the robot is interacting with, or obstacles as it moves through the environment
- **Agriculture:** Analyze fruit yield and condition from tractor-mounted cameras to assess for optimal harvest times.


## Start Building


Object tracking is live and available in the Dragoneye video detection API. Check out the[docs](https://docs.dragoneye.ai/) for more details on how to get started.
