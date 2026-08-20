---
schema_version: "1.0.0"
document_id: "764248a49ffadad1300d1a8b9f17245ca64983df1960e17bb80ad12b190ef8de"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/computer-vision-american-sign-language/"
published_at: "2026-05-01T03:15:00+00:00"
first_seen_at: "2026-07-25T22:08:11.425113+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:ccf75b47e27570308c26cdf4eb4cc3a8069023455aa6228fe4ea73044d553f8c"
---

# Using Computer Vision to Help Deaf and Hard of Hearing Communities

[Joseph Nelson](https://blog.roboflow.com/author/joseph/)


Published May 1, 2026 • 8 min read


Summary


**Data scientist David Lee built a computer vision model to recognize American Sign Language alphabet from images using Roboflow.**


*The below post is a lightly edited guest post by David Lee, a data scientist using computer vision to boost tech accessibility for communities that need it. David has open sourced all materials related to this project on*[his GitHub](https://github.com/insigh1/GA_Data_Science_Capstone?ref=blog.roboflow.com) *. The post is also available on*[David's blog](https://daviddaeshinlee.medium.com/using-computer-vision-in-helping-the-deaf-and-hard-of-hearing-communities-with-yolov5-7d764c2eb614?ref=blog.roboflow.com) *.*


*David has open sourced his*[American Sign Language computer vision dataset](https://universe.roboflow.com/david-lee-d0rhs/american-sign-language-letters?ref=blog.roboflow.com) *.*


---


What would you do if you couldn’t hear anymore? What if you could only use your hands to communicate?


The English alphabet in sign language, identified by computer vision.


A (no sound) video of the full American English Sign Language.


Simple tasks like ordering food, discussing financial matters at the bank, explaining your conditions at the hospital, or even talking to friends and family may seem daunting when they can no longer understand you.


### Consider This


The deaf community is often in situations where verbal communication is the norm. Moreover, access to qualified interpretation services isn’t feasible in many cases, which can result in underemployment, social isolation, and public health challenges.


To give these members of our community a greater voice, I attempted to answer this question as my Data Science boot camp capstone project at General Assembly:


> Can computer vision bridge the gap for the deaf and hard of hearing by learning American Sign Language?


If ASL can accurately be interpreted through a machine learning application, even if it starts with just the alphabet, we can mark a step in providing greater accessibility and educational resources for our deaf and hard of hearing communities.


### Self Reflection and Why I Chose this Project


In my former profession I often would find and focus on problems, or inefficiencies in processes. When I brought up the point and created shortcuts in the form of Excel macros, I would suggest improvements in workflows and was often told:


**“If it ain’t broke, don’t fix it!..”**


As this was outside the realm of my job description of sales, I regretfully found myself in a hole for a long time often thinking to myself:


**“but I can fix it..”**


In short, I’ve buried good ideas and intentions long enough to people that wouldn’t listen to me. I had experienced a form of mental isolation before COVID-19, and it made me think about how much harder it must be for the deaf community. That is why I chose this project.


I had no idea if this would even work, but I wasn’t going to let the unknown stop me this time.


*David's full project presentation is also on YouTube:*


David Lee describes the A-B-Cs with sign language and computer vision.


### Data and project awareness


The decision was made to create an original image dataset for a few reasons. The first was to mirror the intended environment on a mobile device or webcam. These often have resolutions of 720 or 1080p. Several existing datasets have a low resolution and many do not include the letters “J” and “Z” as they require movements.


The letter request form above was created with an introduction to my project along with instructions on how to submit voluntary sign language images with dropbox file request forms. This was distributed on social platforms to bring awareness, and to collect data.


### Data Warping and Oversampling


A total of 720 pictures were collected for this project, and several were on my own hands due to time constraints. As this is a very small dataset, manual labeling with bounding box coordinates were made using the labelImg software, and a probability of transformations function was made to create several instances of the same image with changes in each image with adjusted bounding boxes.


Several choices in the types of augmentations were made from research that was done by Shorten, C., Khoshgoftaar.


Here is an example of what that looks like:


The data increased from 720 to 18,000 images after augmentations.


### Modeling


[YOLOv5](https://playground.roboflow.com/models/ultralytics/yolov5?ref=blog.roboflow.com) was chosen for modeling. This was released on June 10th of 2020, and is still in active development. Even though this isn’t created by the original YOLO authors, YOLO v5 is said to be faster and lightweight, with accuracy on par with YOLO v4 which is widely considered as the fastest and most accurate real-time object detection model.


90% of the augmented images were used for training, and 10% was reserved for validation. The model was trained for 300 epochs using transfer learning with the YOLOv5 pre-trained weights.


Hands for sign language with data augmentation applied


Training demonstrates continuous model improvement.


A mAP@.5:.95 score of 85.27% was achieved.


To understand[mAP (Mean average precision)](https://blog.roboflow.com/mean-average-precision/) scores, you need to understand intersection over union (IoU). Basically, it’s the area of overlap between the actual bounding box and the predicted bounding box divided by the union of the two.


It gives us a confidence level in the prediction and counts the ones over a 50% confidence threshold. The mAP@.5:.95 score is the mean of these levels between 50% and 95% in steps of 5% which shows how well the object detection model performs in this dataset.


### Image Inference tests


I had reserved a test set of my son’s attempts at each letter that was not included in any of the training and validation sets. In fact, no pictures of hands from children were used for training the model. Ideally several more images would help in showcasing how well our model performs, but this a start.


Test set sign language image examples from my child's hand


Out of 26 letters, 4 did not receive a prediction (G, H, J, and Z)


Letters that were incorrectly predicted were:


- “D” predicted as “F”
- “E” predicted as “T”
- “P“ predicted as “Q”
- “R” predicted as “U”


Time for webcam video tests that were recorded.


### Video Inference Tests


Here is another look at the video posted at the start.


(No Sound) Video inference on the a test video for sign language alphabet identification


Even though several images were on my hand for training, the fact that the model was able to perform pretty well on such a small dataset, and still provide good predictions without feeling slow, is very promising!


More data will only help in creating a model that could be utilized in a wide variety of new environments.


As shown on the clip, even a few letters that were partially off screen had fairly good predictions.


Probably the most surprising thing is that the letters J and Z which require movements were recognized.


### Other Tests


Left-handed:


Given nearly all images in the training set were on the the right hand, I was pleasantly surprised to see the model generalize fairly well to my left hand. This indicates the horizontal flip augmentation aided model training.


My son’s hand:


Given the training set did not include any of my son's hand, I was pleased with the performance.


Multiple instances:


Although sign language is not used like the video here, it shows that multiple people can be on screen and the model will be able to distinguish more than one instance of the language.


### Limitations to the Current Model


I’ve discovered a few areas that can be improved on in future iterations of the model.


Distance:


Many of the original pictures were taken from my phone on my hands, the distance of my hand to the camera was very close, negatively impacting inference at further distances.


New Environments:


This video is from volunteers that were not used in any of the model training. Although the model picks up a lot of the letters, the prediction confidence levels are lower, and there are more misclassifications present.


Background Interference:


This test was done to verify that different backgrounds hurt the model’s performance.


### Conclusion


> Computer vision can and should be used in marking a step in greater accessibility and educational resources for our deaf and hard of hearing communities!


The fact that the model performs pretty well using only a small dataset cannot be ignored! Even in new environments with different hands it does a pretty good job at detection. There are a few limitations that can be addressed simply with more data to train on. With a few adjustments, and a lot more data, I expect a working model that can be expanded to far more than just the ASL alphabet.


### Next Steps


Next steps for model improvement.


I believe this project is aligned with the vision of the National Association of the Deaf in bringing better accessibility and education for this underrepresented community. If I am able to bring awareness to the project, and partner with an organization like the NAD, I will be able to gather better data on the people that speak this language natively to push the project further.


The technology is still very new, and there are some limitations on models that can be deployed on mobile devices. For my final project, I primarily implemented the Data Science process to find out if this can actually work. I’m really happy with the results and I’ve already trained a smaller model that I’ll be testing for mobile deployment in the future.


### Final Remarks


I mentioned earlier in the article that I made the mistake of sitting on my ideas for years. I’ve been told once before “You are a dime a dozen” from a former boss letting me go for missing sales targets and I was convinced of my “lack of qualifications” for too long.


I’m grateful for the experience that General Assembly gave me in giving me confidence in my technical abilities and filling a personal gap of not finishing college many years back. I’ve graduated the program on Monday with some of the brightest people I’ve ever met, and I’m certain that I can finally use my gifts to make the world around me a little bit better.


What I’ve learned in this project is that computer vision can help our deaf and hard of hearing neighbors and give them the voice they deserve with technology that is available today.


If you have made it this far… Thank you… I’d love for you to share what I’m working on with your friends to hopefully make projects like this one my livelihood one day.


**Resources**


- [Cudnn install guide](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html?ref=blog.roboflow.com)
- [Install Opencv](https://www.codegrepper.com/code-examples/python/how+to+install+opencv+in+python+3.8?ref=blog.roboflow.com)
- [Roboflow augmentation process](https://docs.roboflow.com/datasets/dataset-versions/image-augmentation?ref=blog.roboflow.com)
- [Heavily utilized research paper on image augmentations](https://journalofbigdata.springeropen.com/articles/10.1186/s40537-019-0197-0?ref=blog.roboflow.com#Sec3)
- [Pillow library](https://pillow.readthedocs.io/en/latest/handbook/index.html?ref=blog.roboflow.com)
- [Labeling Software labelImg](https://github.com/tzutalin/labelImg?ref=blog.roboflow.com)
- [Albumentations library](https://github.com/albumentations-team/albumentations?ref=blog.roboflow.com)


**Special Thanks**


Joseph Nelson, CEO of[Roboflow.com](https://roboflow.com/?ref=blog.roboflow.com) , for delivering a computer vision lesson to our class, and answering my questions directly.


And to my volunteers:
Nathan & Roxanne Seither
Juhee Sung-Schenck
Josh Mizraji
Lydia Kajeckas
Aidan Curley
Chris Johnson
Eric Lee


And to my General Assembly DSI-720 instructors:
Adi Bronshtein
Patrick Wales-Dinan
Kelly Slatery
Noah Christiansen
Jacob Ellena
Bradford Smith


This project would not have been possible without the time all of you invested in me. Thank you!


Read more[Roboflow customer stories here](https://roboflow.com/customer-stories?ref=blog.roboflow.com) .


*Published: October 19, 2020 Updated: May 1, 2026*


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Joseph Nelson](https://blog.roboflow.com/author/joseph/) . (May 1, 2026). Using Computer Vision to Help Deaf and Hard of Hearing Communities. Roboflow Blog: https://blog.roboflow.com/computer-vision-american-sign-language/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Joseph Nelson


Roboflow cofounder and CEO. On a mission to transform every industry by democratizing computer vision. Previously founded and sold a machine learning company.


[View more posts](https://blog.roboflow.com/author/joseph/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
