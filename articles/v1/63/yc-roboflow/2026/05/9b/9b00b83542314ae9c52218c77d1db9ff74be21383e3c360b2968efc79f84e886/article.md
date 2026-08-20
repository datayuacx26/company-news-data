---
schema_version: "1.0.0"
document_id: "9b00b83542314ae9c52218c77d1db9ff74be21383e3c360b2968efc79f84e886"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/street-mural-computer-vision/"
published_at: "2026-05-01T14:02:00+00:00"
first_seen_at: "2026-07-25T22:08:11.425113+00:00"
fetched_at: "2026-07-28T21:44:49.635544+00:00"
content_hash: "sha256:c788adfd43f986cce9120ab00e50977b05982ec2d38dab9a6771bedc9ac85d6d"
---

# Bringing Street Murals to Life with Computer Vision

[Joseph Nelson](https://blog.roboflow.com/author/joseph/)


Published May 1, 2026 • 4 min read


SUMMARY


**Software engineer Yuri Fukuda built a mobile computer vision app that lets users point a phone at the 3,300-square-foot When Women Pursue Justice mural in Brooklyn and identify the 90 women depicted. She captured photos from multiple angles, labeled them, applied image augmentation in Roboflow to handle varied lighting and orientation, and trained an image recognition model that powers an augmented reality experience tied to community history.**


In Bedford–Stuyvesant, Brooklyn (BedStuy), Yuri Fukuda regularly walks by a mural that showcases prominent female leaders. Since October 2005, a stunning 3,300 square foot mural,[When Women Pursue Justice by ArtMakers-NYC](http://www.narpa.org/reference/when_women_pursue_justice-mural_-artmakers-nyc?ref=blog.roboflow.com) , has had a central place in the BedStuy art scene.


The mural celebrates 90 women who have been key in causes like advancing voting rights, health and environmental standards, and more over the last 150 years in the United States.


**When Women Pursue Justice** is located at 498 Greene Avenue in Bedford-Stuyvesant, Brooklyn, NY. (Credit:


[ArtMakers-NYC](http://www.narpa.org/reference/when_women_pursue_justice-mural_-artmakers-nyc?ref=blog.roboflow.com) )


[Yuri Fukuda](https://www.yurifukuda.com/?ref=blog.roboflow.com#/) is an R&D Lead Senior Software Engineer, technologist, and mom living in Brooklyn, New York. Yuri is invested in using new technology to easily empower others to communicate their ideas. Her day job at EachScape focuses on enabling media companies to create their own mobile applications without writing code.


To Yuri, the importance of *When Women Pursue Justice* has only grown with time. Yuri noted that the four-story mural, which took 17 artists and five interns a full summer to compete under lead artist Janet Braun-Reinitz, has once been slated for demolition in 2014 before community organizing prevented said destruction.


Images of


**When Women Pursue Justice** by Janet Braun-Reinitz. (Credit:


[ArtMarkers-NYC](http://www.narpa.org/reference/when_women_pursue_justice-mural_-artmakers-nyc?ref=blog.roboflow.com) )


Given recent advances in image recognition and a passion for using technology to create new experiences, Yuri wondered how she could use computer vision to enable those in her community to more effectively connect with the world around them – especially *When Women Pursue Justice* .


### Building a Computer Vision Enabled Street Mural Application


Yuri set her sights on creating a mobile application that would allow the user to point their phone at the mural and better understand who it profiles. Through exposing the mural's subjects, Yuri intends to bring greater awareness to the women whom have helped shape our society.


So, Yuri set out to capture dozens of photos of the mural from various perspectives. She labeled the images with[LabelImg](https://blog.roboflow.com/labelimg/) , uploaded them to Roboflow, and created a much larger dataset of her source images like applying[image augmentation](https://docs.roboflow.com/datasets/dataset-versions/image-augmentation?ref=blog.roboflow.com) that would help her model learn what the mural looked like in various lightings, at different angles (based on how the user was oriented to the mural), and more. *(You can now*[annotate your images](https://docs.roboflow.com/annotate?ref=blog.roboflow.com) *directly in Roboflow!)*


From here, she closely followed a tutorial on how to create an object detection application so she could deploy the solution to her Android device.


Yuri then used a test set of images to try to out her application.


Yuri's application identifies women feature in the mural. (Credit:


[Yuri Fukuda](https://www.instagram.com/p/CHa7xXrn2sj/?ref=blog.roboflow.com) )


To her delight, the application successfully identifies the first three figures she labeled in her training data: Dorothy Day, Audre Lorde, and Shirley Chisholm.


### What's Next


Yuri notes that this is only the beginning for her work:


> "I wanted to create an initial prototype to show what is possible with computer vision and augmented reality on street murals. Based on the feedback I receive, I plan to adapt my development."


Yuri notes that her initial training dataset is incredibly limited and will benefit from labeling more of the figures highlighted in the mural as well as capturing photos in different lighting conditions. "I tried the application when it was raining, and I realized I need more images from gray days like this in my training data!"


*Considering building your own augmented reality mobile application? Read*[how to design an augmented reality app](https://blog.roboflow.com/designing-augmented-reality-computer-vision-apps/) *and how we built*[a computer vision app for board games](https://blog.roboflow.com/creating-boardboss-a-mobile-application-that-improves-boggle/) *.*


Yuri is developing a hub where neighbors can participate in community-based technology projects and share ideas, learn, and build. She strongly encourages others to take up creating their own mobile apps that can enrich art in their communities as well. "I'm a beginner to computer vision, but with resources like Roboflow's excellent documentation, I was able to create my app in one week."


We're elated to support work like this. As always, we cannot wait to see what else you build.


Read more[Roboflow customer stories here](https://roboflow.com/customer-stories?ref=blog.roboflow.com) .


*Published November 15, 2020 Updated May 1, 2026*


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Joseph Nelson](https://blog.roboflow.com/author/joseph/) . (May 1, 2026). Bringing Street Murals to Life with Computer Vision. Roboflow Blog: https://blog.roboflow.com/street-mural-computer-vision/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Joseph Nelson


Roboflow cofounder and CEO. On a mission to transform every industry by democratizing computer vision. Previously founded and sold a machine learning company.


[View more posts](https://blog.roboflow.com/author/joseph/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
