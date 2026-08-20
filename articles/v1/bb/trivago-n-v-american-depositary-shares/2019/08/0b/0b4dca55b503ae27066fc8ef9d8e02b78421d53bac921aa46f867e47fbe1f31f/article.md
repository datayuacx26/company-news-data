---
schema_version: "1.0.0"
document_id: "0b4dca55b503ae27066fc8ef9d8e02b78421d53bac921aa46f867e47fbe1f31f"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2019-08-21-spajourneywithimageconcepts/"
published_at: "2019-08-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:acb40aaa1c18092389f0ec94b561e923c695bde62e7247f3592e3a51072daf55"
---

# Machine Learning and Bathtubs - How Small Visual Changes Improve User Experience

> While searching for “Spa and Wellness hotels in Berlin…” I land on trivago. Surprisingly the main images of the hotels exactly reflect the spa concept that I am searching for. It helped me better compare hotels on the list for finding my ideal accommodation for my vacation!


This was the user experience we were looking for when we kicked off the Image Concepts project at trivago. The users with clear hotel search intent who are looking for a specific concept hotel before coming to trivago are redirected to the landing pages related to that particular topic.


*Screenshots above:- Difference in trivago landing page for users searching hotels with spa and wellness.*


And this is where we wanted to switch all the main images of hotels to reflect the user intent coming from SEM ads containing spa concepts. With such concept-based main images, we wanted to empower our users to compare accommodations in a more immediate way to help them find their ideal hotel.


Based on the analysis of current trivago user behaviour with regard to several hotel concepts such as ‘business’, ‘luxury’, ‘family-friendly’, ‘romantic’ and ‘spa’, we decided to start with ‘Spa and Wellness’! We wanted to address the frequent search queries by trivago users such as “Hotels with sauna in..”, “Indoor pool hotels in ..”, ” “Hotels with gym and massage facilities in … ”, etc. and SEM ads containing spa and wellness concepts.


## How do we approach the image labelling as a team?


Well, the easiest option would have been to handpick images related to spa and wellness and deliver them to the frontend to be displayed as the hotel main image on matching the pool of queries. Of course the manual approach would work for a limited number of hotels, but it is impossible for our trivago inventory with roughly 100+ million active images related to 3+ million hotels and alternative accommodations around the globe.


So the approach went on to be more automated where we can have an end-to-end system to label images in our inventory. Adapting computer vision technologies coupled with machine learning, we could not only tackle this concept for millions of images, but also easily extend it for other useful concepts in the future.


The project was completed within a cross-functional team of Software Engineers, Data Scientists and Product Managers at trivago. We embraced the best of both Scrum and Kanban techniques to optimise all the DevOps tasks and machine learning experiments related to the project.


For images at trivago, we have an end-to-end serverless infrastructure pipeline designed and hosted on AWS. For the limited scope of this article, we would not go into the details of how we handle efficient tagging and delivery of millions of incoming images. This article will focus more on the journey and learnings with developing models for image predictions related to Spa & Wellness.


## What actually defines Spa & Wellness for our users?


One of the most difficult questions we as a team had to answer at the very beginning was what actually defines Spa & Wellness for our users. From a cloud of various possible keywords, we decided on 6 tags, namely Sauna, Pool, Gym, Yoga, Hot-tub and Massage. The decision was based on the level of abstraction we would like to address with our business model and the results from user research.


*Illustration above:- Classes used in this project to define Spa and Wellness in the hotel domain: Sauna, Pool, Gym, Yoga, Hot-tub and Massage.*


## How do we gather training data for this concept?


Collecting clean ground-truth is one of the most crucial stages before training a machine learning model for any kind of prediction, so we invested quite some time in this. At the beginning we exhausted several public domain image inventories. We then made the best use of some of the existing handpicked image tags from our partner hoteliers. This followed extensive manual qualitative analysis. In order to further align the images semantically, we rolled out jobs on the tool[Amazon Mechanical Turk](https://www.mturk.com/) . This is a tool that allowed us to prepare questionnaires related to a particular image that human beings around the globe could answer. We leveraged this to ask questions such as “Is this a hot-tub? Is it made of wood? Are there people in it?” for several thousands of images. Based on the combination of answers, we had a rule-based system to determine the semantics behind these images. After some iterations, we ended up with 2,000 images for each of the categories of Sauna, Pool, Gym, Yoga, Hot-tub and Massage.


## What machine learning models did we use?


We began with investigating the state-of-the-art neural network architectures for image classification problems. We looked into two major architectures:


- [Recurrent Neural Networks](https://en.wikipedia.org/wiki/Recurrent_neural_network) (RNN): RNNs are good for extracting correlations. Since they always have the present and the recent past as the input, they manage to learn sequences in data to better predict correlations. In our case, we wanted to address the problem of multiple tags for a given image. For instance, the idea would be RNN would shrink the probability of the presence of a bathtub when we already have tags like bed and table lamp in a particular image.


- [Convolutional Neural Networks](https://en.wikipedia.org/wiki/Convolutional_neural_network) (CNN): CNNs are best suited for image classification and segmentation problems. The reason being these networks have kernels with local connectivities that act like feature detectors while convolving across every group of pixels in an image. Thus they take into account spatial information which mimics the human visual system. Also the pooling layer of a typical CNNs help reduce the very high dimensionality of images. Finally the fully connected layers computes probability scores for prediction of individual classes for a given image.


For the scope of this concept, we limited ourselves to having only one label per image. So we went ahead only with CNNs. We evaluated 5 different CNN architectures with our dataset, namely[InceptionV3](https://keras.io/applications/#inceptionv3) ,[VGG16](https://keras.io/applications/#vgg16) ,[Xception](https://keras.io/applications/#xception) ,[ResNet50](https://keras.io/applications/#resnet) ,[InceptionResnetV2](https://keras.io/applications/#inceptionresnetv2) .


*Diagram above:- Typical architecture of a Convolutional Neural Networks.*


Given the fact that we had very specific classes with a clean but small amount of training data (2000 of each class, being divided into training, validation and test in the ratio 3:1:1), we decided not to train the models from scratch (i.e. with random initialization), but to use models that are pre-trained on real-world datasets such as[ImageNet](http://www.image-net.org/) . We later fine-tuned the CNNs by replacing the last fully connected layer and retrain some of the weights by continuing backpropagation with the Spa & Wellness groundtruth. We got much faster and better results with this approach of transfer-learning because the pre-trained CNNs have lower level image features such as edges and colour blobs already learnt. We fine-tuned the weights further to make it progressively more specific to the detailed features of our Spa & Wellness classes.


We also performed some data augmentation while training the models such as mirroring of individual images to further increase the limited data-set of 2,000 samples per class.


Technology-wise we used Python as the main programming language with libraries such as[TensorFlow](https://www.tensorflow.org/) ,[Keras](https://keras.io/) ,[OpenCV](https://opencv.org/) ,[scikit-learn](https://scikit-learn.org/) and[Pandas](https://pandas.pydata.org/) . We have been heavy users of customized[Jupyter notebooks](https://jupyter.org/) for sharing code and visualizations between the team for reviews and knowledge sharing.


## How did we evaluate and know when is it good enough?


In order to conclude which CNN architecture to go forward with, we observed the validation accuracy over multiple iterations of training. We tuned the various neural network[hyper-parameters](https://towardsdatascience.com/what-are-hyperparameters-and-how-to-tune-the-hyperparameters-in-a-deep-neural-network-d0604917584a) such as learning rates, dropouts, batch size, etc as and when required. We constantly used our validation set to trade off[bias vs. variance](https://towardsdatascience.com/understanding-the-bias-variance-tradeoff-165e6942b229) to make sure the model doesn’t overfit or underfit for our use-case.


*Chart above:- Definition of Precision, Recall and Accuracy in terms of True Positive (TP), False Positive (FP), True Negative (TN), and False Negative (FN).*


As far as prediction scores are concerned, we considered[Precision and Recall](https://en.wikipedia.org/wiki/Precision_and_recall) at the beginning. Precision in our case would be how many images tagged as Spa are really Spa. And Recall would be how many of the total Spa images have been tagged correctly. We preferred to judge the models based on Precision over Recall or Accuracy ─ the reason being we wanted to minimize the number of False Positives for our use-case. Other metrics we took into consideration included[F-Scores](https://en.wikipedia.org/wiki/F1_score) ,[ROC curves](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) and[Confusion Matrix](https://en.wikipedia.org/wiki/Confusion_matrix) .


Finally we had[VGG16](https://keras.io/applications/#vgg16) as the optimal model after trading off different evaluation metrics for our test data-set.


## How did we handle images that are not related to Spa & Wellness?


Although the main goal was to teach the model to identify if an image belongs to one of the Spa & Wellness classes, it was also very important to make sure the model negates out all classes that are beyond this concept.


*Results above:- Incorrect prediction of a bedroom image by the model as a massage with 98% probability.*


The first challenge we faced was with Bedrooms images. Being a hotel inventory, almost 60% of our images are Bedrooms. It was interesting to note that the trained model began predicting several Bedroom images as Massage. The reason being obvious from a logical point of view that the model is basically trained to recognize patterns. And it failed to distinguish the patterns between bedrooms with pillows and white bed-covers and actual massages.


So in order to deal with this situation, we introduce a new 7th class called the Bedroom with the same number of training samples, and re-trained the model. This definitely improved the results and catered to the confusion caused earlier.


We conducted some further analysis to confirm the same. The frequency histogram above shows the performance of the trained model when we predicted 5,000 positive Spa & Wellness classes (marked in green shifted towards the right) and negative classes (marked in orange shifted towards the left). Having higher orange bars near 0.0 and higher green bars near 1.0 for all the classes confirmed that we have tackled the problem successfully.


*Graph above:- Frequency histogram for model prediction of individual tags: X-Axis:- Probability range from 0.0 - 1.0; Y-Axis:- Green:- Frequency of Positive Class (Green) and Negative Class (Orange).*


But still we had a lot of non-spa classes in the inventory apart from Bedroom that needed to be negate out and thresholding just the class probabilities of Spa & Wellness class wasn’t enough. So we gathered another 10,000 Non-Spa classes as training samples and trained the model with unbalanced data-set putting more weightage on the 8th Non-Spa class. As a next step, we would discard the classes of Bedroom and Non-Spa predictions from the model as a rule based approach.


After this iteration, we finally had a working model that catered to all our requirements.


## How smart or dumb did our model turn out to be?


We reached an average[Precision and Recall](https://en.wikipedia.org/wiki/Precision_and_recall) of ~85% and ~89% for the individual Spa & Wellness classes. We further analysed the[confusion matrix](https://en.wikipedia.org/wiki/Confusion_matrix) and the scores were good enough for the business requirement considering the large amount of images we have in the inventory.


*Results above:- Confusion matrix in percentage for prediction by the model on the 7 classes with 400 samples in each.*


After some further data analysis on the confidence of the model on the trivago data-set, we came up with optimum thresholds for individual class probabilities. For instance in-case of Hot-tub we could assign an image as a Hot-tub only when the model predicts it to be >80% probability of this class. But for trickier classes such as massage, the probability threshold lies as high as 97%. At the end we also ranked the individual classes based on preference of displaying main image of a hotel when more than one Spa & Wellness class is available in the image gallery of a hotel.


*Images above:- True Positive prediction of the model for Spa & Wellness classes of Yoga, Sauna, Gym, Massage (clockwise) for main image of a hotel.*


Just to explain how tricky it can become at times for the model, here is a pair of True Positive and False Positive prediction:


*Images above:- Left: True Positive prediction by the model for the class Hot-tub; Right: False Positive prediction by the model for the class Hot-tub.*


This shows us how the intelligence of the model is limited to pattern recognition in images and there can still be edge cases we should consider. But it is always a trade-off between the error-rates when we are rolling out a system to tag a growing inventory of images as large 100 Million.


But there were other times, when we felt like our own model was just trolling us — like here when it tagged the following images as, well, ‘pool’:


*Image above:- Left: True Positive prediction by the model for the class Pool; Right: False Positive prediction by the model for the class Pool.*


Conducting A/B testing with this model in production on the live website of trivago showed us immense potential of such user-centric projects with users more engaged in exploring unique content in trivago before clicking out.


## Looking forward


Images are one of the most useful sources of information for users when looking for the ideal hotel. There are several potential directions which can add user-value such as extending the same process for other hotel concepts like romantic, family friendly, business, luxury, etc. with multiple tags per image and re-using tags across different concepts. Possible directions include simply switching the default main images and sorting hotel galleries based on combination of unique selling points that a hotel offers and the typical behaviour of individual users. With directions such as these, we can empower our users even more when planning their travel and looking for hotels.
