---
schema_version: "1.0.0"
document_id: "942b2aa5c804b2c705fca5e31e7d687a797fde96622fa13e6c4eb8b03bcd98a1"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/reducing-traffic-with-computer-vision/"
published_at: "2026-03-04T23:59:00+00:00"
first_seen_at: "2026-07-25T22:08:11.425113+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:27913c42d78c662a96a4d3e18d5ba72d26f007094ad6c1c6af3e3236e9241bab"
---

# How Transport for Cairo is Improving Commuting for Millions with Computer Vision

[Joseph Nelson](https://blog.roboflow.com/author/joseph/)


Published Mar 4, 2026 • 4 min read


SUMMARY


**Transport for Cairo used computer vision to count and classify vehicles, including taxis, tuk tuks, microbuses, and motorcycles, at intersections in a Ugandan city where no prior traffic data existed, replacing a costly manual volunteer counting approach. The team used Roboflow to manage image data, apply augmentations for underrepresented vehicle classes, and export datasets for training custom detection models, ultimately producing traffic flow data that city planners and transit operators could act on.**


Reducing traffic in well-planned cities where bus routes are well-mapped, subways are running on a predictable cadence, and lanes are well-marked is a significant undertaking.


Now consider improving traffic without predictable routes, maps, and regular modes of transit. It’s a chaotic scene that[CityLab](https://www.citylab.com/transportation/2016/08/a-group-of-young-egyptians-is-leading-cairos-transit-mapping-revolution-public-transport/495452/?ref=blog.roboflow.com) paints well:


*Come rush hour, the streets of Cairo, Egypt, descend into mayhem. Cars sit in gridlock traffic, horns blaring all around. Some commuters wait at bus stops, unsure when the next one will come. Others escape into Cairo’s subway stations, only to find themselves lost in a sea of people. Then there are the informal microbuses, packed past capacity with passengers—and with more hanging dangerously from the sides.*


These are the types of the challenges Transport for Cairo (TfC), a data-driven consultancy that serves primarily African cities, faces in their work everyday. The group exists to help cities reduce congestion while adapting to rapid urbanization – challenges that they note are at the crux of helping emerging cities grow while adapting to climate change.


###
Project Overview


A recent TfC project called on the organization to improve infrastructure in a Ugandan city where data is sparse on how traffic flows, where the greatest bottlenecks are, and what types of vehicles commuters use. A mix of taxis, tuk tuks, microbuses, and bikes zip through the city at a dizzying rate. Bus routes are influx and largely to be determined. Even city officials have little data on what patterns look like, further limiting informed decision making.


Without any traffic data to start, TfC’s client suggested the organization turn to hiring human volunteers to stand at various intersections and take tally of how many vehicles they saw in hour segments. Recruiting hundreds of workers to stand at intersections would be expensive, time-consuming, and inexact. Where TfC pursued this manual approach yielded spotty results: it’s challenging to know the validity of the data and staffers could only be paid for specific points in time. A more precise solution was required.


An example map from the Transport for Cairo team. Designed by Ismail Moneer, Mohamed Khalaf, Nada Hesham, and Sarah S. Khalil. (Transport for Cairo)‌‌


### Computer Vision Informed Traffic Flows


Thus, the TfC team set out to use computer vision to more cheaply, reliably, and accurately count commuters in transit. The Ugandan city officials already had closed circuit TV at many intersections of interest. Thus, the TfC team could apply their efforts to processing the data and training a reliable model to count all forms of mobility.


TfC quickly recognized they needed to train a custom model for their problem. Even though ImageNet (and other models) have classes that identify vehicles like cars and trucks, the unique forms of micromobility zipping across the streets require label nuances not captured in pre-trained solutions. What’s more: the model may need to run inference on-device and parse video feeds, requiring fast inference times in a small compute environment.


TfC data scientist Beshoi Maher turned to Roboflow to process labeled data unique to his team’s challenges. Beshoi noted that having access to Roboflow’s dataset health check informed the team’s data collection practices to quickly get ahead of class imbalances that emerged in the data. Moreover, one-click specialized augmentations allowed his team to increase their dataset size, especially of underrepresented classes seamlessly. When it came time to training, the ease with which Beshoi could export his data to any architecture’s required format made testing various models straightforward.


When it came to needing to prep data for custom computer vision models, Roboflow saved the team what could have been another teammate or twice the time for a single model. As TfC continues to incorporate automated vehicle detection for future cities, Roboflow will be a key tool in their arsenal to process images and train more accurate models, faster.


**About Transport for Cairo**


Transport for Cairo (TfC) provides data, tools and research to improve urban mobility in emerging cities, primarily in Africa. Rapid Urbanization, economic and population growth in times of the climate crisis force us to tackle the complexity and ever-changing urban mobility scene in developing cities. TfC is a disruptive transport consultancy that optimizes existing transport systems and develops flexible and sustainable mobility solutions for our future.[https://transportforcairo.com/](https://transportforcairo.com/?ref=blog.roboflow.com)


Published: August 4, 2020
Updated: March 4, 2026


Related reading:


- [Count Objects Crossing Lines with Computer Vision](https://blog.roboflow.com/count-objects-crossing-lines/)
- [Object Counting using Roboflow RF-DETR](https://blog.roboflow.com/object-counting/)
- [Real-Time Traffic Light Detection with RF-DETR](https://blog.roboflow.com/traffic-light-detection/)


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Joseph Nelson](https://blog.roboflow.com/author/joseph/) . (Mar 4, 2026). How Transport for Cairo is Improving Commuting for Millions with Computer Vision. Roboflow Blog: https://blog.roboflow.com/reducing-traffic-with-computer-vision/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Joseph Nelson


Roboflow cofounder and CEO. On a mission to transform every industry by democratizing computer vision. Previously founded and sold a machine learning company.


[View more posts](https://blog.roboflow.com/author/joseph/)


### Topics


- [Logistics](https://blog.roboflow.com/tag/logistics/)
