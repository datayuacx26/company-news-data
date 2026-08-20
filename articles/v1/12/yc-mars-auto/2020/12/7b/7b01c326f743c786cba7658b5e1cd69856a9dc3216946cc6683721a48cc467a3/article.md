---
schema_version: "1.0.0"
document_id: "7b01c326f743c786cba7658b5e1cd69856a9dc3216946cc6683721a48cc467a3"
company_key: "yc-mars-auto"
company: "Mars Auto"
source_id: "yc-mars-auto-rss-2183801a7a92"
canonical_url: "https://blog.marsauto.com/automated-large-scale-data-generation-for-autonomous-vehicle-59de8b26357e"
published_at: "2020-12-08T14:18:36+00:00"
first_seen_at: "2026-07-25T13:21:51.802210+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:5d3b683257f126bf452f4246b25b4e5822f0a1482823568a50b04a2774a6d98a"
---

# Automated large scale data generation for autonomous vehicle

# Automated large scale data generation for autonomous vehicle


[Cong Jie](https://medium.com/@ncjlee78?source=post_page---byline--59de8b26357e---------------------------------------)


3 min read


·


Dec 8, 2020


--


Over one year ago we built our pure vision neural network-based autonomous truck system to drive from[Seoul to Busan](https://www.youtube.com/watch?v=FgPIluHDP4g) fully autonomously. Our perception system improves according to the increasing amount of good quality ground truth data. It is necessary to build a scalable data acquisition pipeline loop from collecting raw driving data from our trucks, ground truth data annotation, to training a neural network. We developed in-house ML-powered annotation tools with humans in the loop to acquire ground truth data to fuel our perception system. It is a tedious and expensive (in terms of time and human resource) process as it requires proper training for our annotators to have consistent annotation and annotated data quality checks. As our perception system takes advantage of both spatial and temporal information, annotating long sequence of video frames become even more expensive. Scaling up to million miles of driving video data collected from our trucks becomes infeasible.


### Outsource data annotation too does not scale


Press enter or click to view image in full size


Figure 1: Manual data annotation by human


Another alternative for data annotation is to outsource the job to a third party company. Many self-driving car companies opt for this approach but it comes with a very high price tag. One of the very well known third party companies in the industry is[scale ai](https://scale.com/) . Some autonomous vehicle open-dataset such as the[Lyft Level 5](https://self-driving.lyft.com/level5/data/) and[nuScenes](https://scale.com/open-datasets/nuscenes) are curated by the third party service. While third-party services usually rely on ML-powered with the human in the loop data annotation, turning raw driving video data into a useful ground truth is very costly. Take an example of a single frame extracted from our raw driving video data as shown in figure 1., annotating the image consists of 5 lane lines and 12 vehicles with the[single image on-demand pricing](https://scale.com/pricing) (as of the date of posting the annotation cost 0.08/Image + 0.08/Annotation) would cost USD 1.44 for this particular video frame. Extrapolating this to an hour driving video of 20 FPS would cost USD 103,680! Note that, this is just an example, our perception system does not rely on 2D image plane ground truth for learning. These approaches whether by in-house manual annotation or outsource annotation simply do not scale!


### **Automatic data annotation**


With the goal of deployment to a large number of trucks in mind, being able to automate this process, is one of the keys to scalability. Since then, with a very small team of engineers, we revamped our data acquisition pipeline to get rid of the expensive process by developing sensor fusion algorithms to automate the job. Unlike most self-driving car sensor suites which are equipped with multiple depth-sensing sensors (i.e.,[LiDAR](https://en.wikipedia.org/wiki/Lidar) ), with the absence of LiDAR in our system, generating absolute scale ground truth data in 3D becomes very challenging. Instead of relying on additional hardware to perceive depth (which can increase the cost to our system), we took a more challenging approach by solving this problem with maths and algorithms. With a monocular camera as our main forward sensing sensor and other built-in sensors measurements across space and time obtained from our trucks, our algorithm is able to perceive absolute scale depth for both static scene and dynamic moving objects in 3D as shown in figure 2. from our generated data.


Figure 2: Automated data annotation without human


Today, while our trucks are being used to do the[logistic shipping from warehouse to warehouse](https://medium.com/mars-auto/marking-an-important-milestone-at-mars-auto-a864a77a668d) , we are able to turn the raw video data automatically into map and useful ground truth data of many highway routes in South Korea under different traffic, weather, and lighting conditions. This in turn enables us to leverage our perception system to the next level.


## Get Cong Jie’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


In Mars Auto, we solve many challenging problems to build an autonomous truck system.[Join us](https://www.notion.so/marsauto/Mars-Auto-is-hiring-de1d2a9da06f4845a6100b82d692a689) on our journey to build the future!
