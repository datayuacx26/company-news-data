---
schema_version: "1.0.0"
document_id: "2d6c66f60eb1e7bdfd340429d0251a343fadd2c95fbbec0fb02596a6659b9e5d"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/gan-generating-art-computer-vision/"
published_at: "2026-04-21T02:41:00+00:00"
first_seen_at: "2026-07-25T22:08:11.425113+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:cf00c1be499fa66db5db7b33c8f61d7a05d724aa3c79172a686aaea27b5e55e6"
---

# Generating Renaissance Art with Computer Vision

[Matt Brems](https://blog.roboflow.com/author/matt/)


Published Apr 21, 2026 • 2 min read


SUMMARY


**Two high school students built a DCGAN during a 24-hour hackathon to generate abstract images in the style of Renaissance paintings. They used Roboflow to preprocess and scale images, and applied data augmentation through Roboflow to more than triple their dataset size.**


The below post is a guest post written by[Samay Lakhani](https://www.linkedin.com/in/samay-lakhani-b634591a9/?ref=blog.roboflow.com) and[Sujay Sundar](https://devpost.com/sujay9sundar?ref=blog.roboflow.com) , two budding data scientists. Samay currently interns with a Silicon Valley tech company; Sujay currently does research under the mentorship of a CUNY Staten Island neuroscience professor. Both are sophomores at Jericho High School, showing that they're a lot smarter than the average Roboflow employee was in high school. (The post was edited and compiled by Roboflow.)


### Art has the unmatched ability to inspire. That is why we set out to create it.


Inspired by[ThisPersonDoesNotExist.com](https://thispersondoesnotexist.com/?ref=blog.roboflow.com) - a website that generates fake people who look like real people - our team built a[Generative Adversarial Network](https://developers.google.com/machine-learning/gan?ref=blog.roboflow.com) - GAN for short. Our goal was to extract and reproduce the most important features in Renaissance paintings, then generated abstract versions of Renaissance art.


We wanted to show anyone that they can build a working[computer vision model](https://playground.roboflow.com/models?ref=blog.roboflow.com) quickly. We participated in a[24-hour hackathon](https://teenhacksli.com/?ref=blog.roboflow.com) where we grabbed 300 images from around the Internet.


The images we gathered represented the best known paintings from the Renaissance times, like paintings by Michelangelo, da Vinci, and portraits of nobles. We wanted to focus on paintings, so we discarded images of sculptures and 3-dimensional art.


The Mona Lisa. (


[Source](https://en.wikipedia.org/wiki/Mona_Lisa?ref=blog.roboflow.com) .)


We[pre-processed](https://docs.roboflow.com/datasets/dataset-versions/image-preprocessing?ref=blog.roboflow.com) our data using Roboflow by scaling the images down to 32 by 32 pixels. We opted to fit a DCGAN ([Deep Convolutional Generative Adversarial Network](https://docs.pytorch.org/tutorials/beginner/dcgan_faces_tutorial.html?ref=blog.roboflow.com) ). A GAN works by taking in usually one category of input images, then minimizes loss on two main pieces: the generator and discriminator.


- The generator produces noise.
- The discriminator decides if that noise represents the category.


It eventually should converge to create realistic images.


Initially, we did not augment our images. In this first trial, the model failed and produced what looks like a lot of noise.


To attempt to improve model performance, we wanted to[augment our dataset](https://docs.roboflow.com/datasets/dataset-versions/image-augmentation?ref=blog.roboflow.com) with reflections and rotations. Because this was a 24-hour hackathon, we didn’t have the time to create a pipeline that would be easily modifiable.


With a few clicks in Roboflow, we more than tripled our dataset size via data augmentation. Roboflow saved us hours that we would otherwise spend writing and testing code. This helped us achieve our goal: we won the hackathon in the social impact category!


This isn’t our first time using Roboflow. We’ve used[Roboflow’s model library](https://playground.roboflow.com/models?ref=blog.roboflow.com) for proprietary object detection work and gotten over 96%[mean average precision](https://blog.roboflow.com/mean-average-precision/) .


In order to get similar results without Roboflow, we would have to spend hours writing code, then re-writing it for every new application. Roboflow regularly saves us tremendous amounts of time!


Read more[Roboflow customer stories here](https://roboflow.com/customer-stories?ref=blog.roboflow.com) .


*Published: November 21, 2020 Updated: April 4, 2026*


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Matt Brems](https://blog.roboflow.com/author/matt/) . (Apr 21, 2026). Generating Renaissance Art with Computer Vision. Roboflow Blog: https://blog.roboflow.com/gan-generating-art-computer-vision/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Matt Brems


Growth Manager @ Roboflow. Previously solved data science problems across finance, education, politics, and more. Passionate about teaching and empowering others to accomplish more.


[View more posts](https://blog.roboflow.com/author/matt/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Image Augmentation](https://blog.roboflow.com/tag/image-augmentation/)
