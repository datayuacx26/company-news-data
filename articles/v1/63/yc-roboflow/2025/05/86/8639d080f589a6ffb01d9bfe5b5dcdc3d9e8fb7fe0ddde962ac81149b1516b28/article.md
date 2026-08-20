---
schema_version: "1.0.0"
document_id: "8639d080f589a6ffb01d9bfe5b5dcdc3d9e8fb7fe0ddde962ac81149b1516b28"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/computer-vision-robotic-poker-dealer/"
published_at: "2025-05-09T16:14:56+00:00"
first_seen_at: "2026-07-28T00:08:32.096830+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:5af39c9e677711136b6ba96a52447505d4d7b18779931c22b61f1d2a8f5767a7"
---

# Using Computer Vision to Develop a Robotic Arm Poker Dealer

[Contributing Writer](https://blog.roboflow.com/author/contributing-writer/)


Published May 9, 2025 • 9 min read


SUMMARY


A team from the University of Manchester Robotics Society built a robotic arm that deals poker hands, tracks chips, and recognizes player moves in real time using RF-DETR and Roboflow. The vision pipeline identifies individual cards, computes pot values, and determines in-game states, then transmits that structured game state data to the arm's control system via inverse kinematics and trajectory planning. This post walks through the computer vision architecture, model training, community card detection, chip counting, and hand detection components of the system.


What if a robotic arm could deal poker hands, track chips, and recognize player moves, all in real time? Go behind the scenes with a team from the University of Manchester, as they show how they used[RF-DETR](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) and Roboflow to build a fully vision-powered robotic poker dealer.


## How to Build a Robotic Poker Dealer


Poker is a high-stakes card game in which players bet on the strength of their concealed cards relative to their opponents, with a strategy centered on probability, psychology, and deception. The poker dealer is responsible for overseeing a poker game and maintaining a seamless exchange of hands. They play a crucial role in ensuring that the game is conducted within the regulations, whilst not being influenced by the players in any measure. In such competitive environments, the integrity of the dealer is paramount.


Traditionally, poker games are based on human dealers who, despite their best efforts, can potentially be influenced or make mistakes. We attempted to minimize this drawback by designing an automated robotic poker dealer that uses[computer vision](https://roboflow.com/?ref=blog.roboflow.com) to understand the game state and make decisions independently.


Within our project's monitored environment, the dealer is directly responsible for dealing cards to the players, flipping the community cards, and collecting folded cards as required. We developed a computer vision serial pipeline to identify poker cards, compute the pot, and determine in-game states and actions, employing Roboflow's tools to optimize the dealer arm's movements.


The arm has 6 degrees of freedom - allowing it to perform a wide array of movements; this calls for establishing an adaptable system using computer vision as a base to configure necessary parameters to instruct the arm's movements, and for recognition of the game state. The simulative AI decision making is driven by transmission of the game state data, extracted using computer vision to command the hardware using inverse kinematics and trajectory planning. In this blog on the project, we focus on building the vision system of the acting dealer arm.


## Vision System Architecture for the Robotic Arm Poker Dealer


The robot arm must be able to distinguish between various poker game situations. Therefore, we must construct a vision system that encompasses all possible variables and provides adequate, accurate information for the arm to act upon. Some key pillars of the system include:


**1. Camera Configuration** : The key objectives are to identify playing cards and poker chips. To achieve that, we require two parallel camera threads that provide a bird's-eye view and a side-on view. The two camera devices (in our case) are Android phones connected to the host device using[Android Debug Bridge](https://developer.android.com/tools/adb?ref=blog.roboflow.com) **** (ADB). This allows us to avoid using expensive depth-sensing cameras to calculate pot values, while providing us with high quality video feeds at no extra cost. The functions of both camera views are the following:


- **Bird's-eye View:** This provides a top-down view of the poker table. This perspective will be a constant video feed and is useful for the identification of playing cards, hand detection for checking, folding, and to aggregate player, card, and pot-specific areas.
- **Side-on view:** This is essential for identifying the poker chips, how many there are, and their colors in order to calculate the pot values to differentiate between a call and a raise.


**2. Card Detection** : A custom-trained[RF-DETR Object Detection Model](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) utilizing the bird's-eye view is trained on a large dataset from the[Roboflow Universe](https://universe.roboflow.com/search?q=poker&ref=blog.roboflow.com) to acclimate it to various exposure, brightness, and lighting conditions.


**3. Pot Detection** : We trained an[RF-DETR Instance Segmentation Model](https://roboflow.com/model/yolov8-instance-segmentation?ref=blog.roboflow.com) to calculate the quantity of each denomination in a chip stack to evaluate the pot. Since the side-view is specific to this project, we fine-tuned the pretrained model on a custom dataset made using Roboflow's tools. Configuring the chip color denominations at the beginning of the game allows the arm to calculate the total value of the pot, based on the detected chips.


**4. Hand Detection for Checking** : We utilized a pre-trained model from the Roboflow Universe to detect hand movements for player check actions. This approach saved substantial development time while delivering accurate results. The technical details of our hand detection implementation are covered in the Model Training section.


**5. Game State Evaluation** : The model inference results provide the information used to evaluate players' actions and the game state, subsequently determining the arm's next action(s). Each possible player action is detected / processed as follows:


- Folds: Analyze the bird's-eye frame to detect fold events using **DBSCAN clustering** - looking for the edges on the back of the cards under flexible thresholds to factor in lighting.
- Calls & Raises: The value of the pot is calculated using the side-on camera. If the change in value is equal to the previous change in value, or the first addition, then this move must be a raise. If the change in value is more than the previous change, this must be a call.
- Checks: At the beginning of each round, before the first raise, each player is allowed to check. This is signaled by the player waving their hand over the table which is detected through the bird's-eye view.


## System Integration for Developing a Robotic Arm Poker Dealer


At the core of our architecture is the[bird's-eye view camera thread](https://github.com/jalliet/poker-robot-arm-24-25/blob/computer_vision/vision/src/threads/birds_eye_camera.py?ref=blog.roboflow.com) , which captures the primary video feed and shares frames with other components through a thread-safe mechanism. Our[fold detection thread](https://github.com/jalliet/poker-robot-arm-24-25/blob/computer_vision/vision/src/threads/fold_detection.py?ref=blog.roboflow.com) uses DBSCAN clustering to identify cards placed face-down in player areas, while a separate thread handles chip detection and counting from the side-view camera. All these detection threads feed their results into a central[event aggregation thread](https://github.com/jalliet/poker-robot-arm-24-25/blob/computer_vision/vision/src/threads/event_aggregator.py?ref=blog.roboflow.com) , which consolidates all detected events, resolves the current game state, and transmits appropriate commands to the robotic arm through our vision-to-motion API.


This modular, multi-threaded approach enables high-performance processing while maintaining the responsiveness needed for a real-time poker dealer.


## Building the Robotic Arm Poker Dealer System with Roboflow's Tools


Now, let's dive into the construction of our vision system. As previously discussed, the dealer arm must be able to concurrently recognize key elements of a poker game setup, and infer the state of the game. Roboflow's platform enables straightforward and rapid development and testing of vision models on custom and community data sets, which we utilized extensively.


### Computer Vision Model Training


We approached using[vision models](https://playground.roboflow.com/models?ref=blog.roboflow.com) in different ways - using pre-trained models, expanding pre-existing datasets and developing our own from scratch - depending on the performance and specificity required for each task.


### Community Card Detection


We went for the model with the best model size to training time trade-off: the **RF-DETR** object detection model. **RF-DETR** also facilitates real-time inference better than alternatives like **GroundingDINO** , etc. We used a varied dataset to boost performance; however, with 53 unique cards (or classes), that would require us to collect upwards of 10,000 images (with augmented ones). We found that this[dataset](https://universe.roboflow.com/augmented-startups/playing-cards-ow27d?ref=blog.roboflow.com) contains an exhaustive overview of most real-world scenarios.


```text
from roboflow import Roboflow
from ultralytics import YOLO


# Import the dataset from Roboflow Universe


rf = Roboflow(api_key="xxxxxxxxxxxxxxxxx")
project = rf.workspace("augmented-startups").project("playing-cards-ow27d")
version = project.version(4)
dataset = version.download("yolov8")


# Ensure the GPU is being used while training if available
device = 'cuda' if torch.cuda.is_available() else 'mps' if  torch.backends.mps.is_available() else 'cpu'


# Training
model = YOLO('yolov8l.pt')
results = model.train(data="/content/Playing-Cards-4/data.yaml",
epochs=50,
patience = 25,
batch=16, imgsz=640,
device=0, workers=8,
pretrained=True, val=True,
plots=True, save=True, save_period = 20, show=True,
lr0=0.001, lrf=0.01, fliplr=0.0, cos_lr=True,
amp=False, dropout=0.1)


```


The above snippet refers to the training procedure and hyperparameter tuning for the **RF-DETR** object detection model based on the[Roboflow notebooks](https://github.com/roboflow/notebooks?ref=blog.roboflow.com) . Training finished after just 45 epochs; the inclusion of the patience (early stopping) parameter playing a key role at that.


### Poker Chip Counting


Training a custom model was critical for a functional chip counting subsystem. More straightforward vision techniques such as Hough lines, contour analysis and edge detection had low success rates in recognizing chips in stacks due to wide-ranging test conditions, lighting, and a small margin for error. We opted for instance segmentation over objection detection and semantic segmentation to account for the individual differences of each chip, particularly under difficult conditions where chips may not be perfectly stacked. Again, we used **RF-DETR** due to its accuracy and low training time.


Due to the specialized nature of the task, as well as the need for optimal performance in chip counting, we created our own custom dataset to cater to the range of conditions in testing. We felt that this significant time investment was worthwhile because the chip counting must be accurate in order to determine if a player has called or raised.


Roboflow accelerated the development of this substantially through the pre-processing and augmentation tools from the labeling interface. The model was trained on **1280x1280** images due to the small size of each chip. Roboflow's pre-trained vision model integration for annotation was particularly helpful - an initial mini batch of images was used to train a model that was used to aid with the tedious annotation process of the final, full-size data set.


For more information on the specifics of this thread, please check out our[chip detection thread code](https://github.com/jalliet/poker-robot-arm-24-25/blob/computer_vision/vision/src/threads/chip_detection.py%7D%7Bchip%20detection%20thread%20code?ref=blog.roboflow.com) or the sample chip detection model training snippet below.


```text
from roboflow import Roboflow
from ultralytics import YOLO


# Import the custom dataset from the Workspace
rf = Roboflow(api_key="xxxxxxxxxxxxxxxxx")
project = rf.workspace("cv-poker").project("pokervision-srkne")
version = project.version(2)
dataset = version.download("yolov8")


# Ensure GPU is being used for training if available


# Instance segmentation training
model = YOLO('yolov8l-seg.pt')
results = model.train(data="/content/PokerVision-2/data.yaml",
epochs=100,
patience = 10,
batch=2, imgsz=1280,
device=0, workers=8,
pretrained=True, val=True,
plots=True, save=True, save_period = 5, show=True,
lr0=0.001, lrf=0.01, fliplr=0.0, cos_lr=True,
amp=False, dropout=0.1)


```


From the graphs of both fine-tuned models in Figure 5, the mAP50-95 scores achieved were **90.179** for Object Detection and **89.414** for Instance Segmentation; acceptable results for the absolute minimum requirements of this project.


### Hand Detection


Training a custom model can be both time-consuming and computationally intensive, so using a pre-trained model significantly reduces the workload. We found a suitable[pre-trained, open-source model](https://universe.roboflow.com/lab-8utlg/hand_detect-xhlhk?ref=blog.roboflow.com) from the Roboflow Universe capable of detecting hands, which we then used to identify player checking at the start of each round.


Deploying models from Roboflow Universe is straightforward and efficient with the Roboflow's[InferencePipeline](https://inference.roboflow.com/using_inference/inference_pipeline/?ref=blog.roboflow.com) , which can process video streams directly within Python. The implementation of our hand tracking system can be found directly on[GitHub](https://github.com/jalliet/poker-robot-arm-24-25/blob/computer_vision/vision/src/threads/hand_tracking.py?ref=blog.roboflow.com) .


We incorporated a custom sink into the pipeline to track hand positions within the playing area and to detect hand waving motions. To accurately recognize a wave, we verified that the detected hand consistently moved across multiple consecutive frames within a defined area, rather than appearing briefly or remaining stationary. For the next iteration of this vision system, we are considering using the[RF Trackers](https://roboflow.github.io/trackers/develop/?ref=blog.roboflow.com) library for such tasks, thereby simplifying our game logic API for hand and game state tracking.


## Develop a Robotic Arm Poker Dealer with Computer Vision


We have successfully integrated our computer vision system with the robotic arm's hardware through a robust API and Bluetooth communication layer. This vision system serves as the foundation for the arm's movements, providing real-time game state information for appropriate decision-making and physical actions.


Looking ahead, our next iteration will focus on expanding the arm's capabilities beyond dealing to include playing as a participant. We also plan to enhance the hardware design based on insights gained during this initial development phase, addressing previously unforeseen challenges and making the system more robust for extended gameplay sessions.


**Contributors:** Joshua Alliet1, Kshitij Jha1, Jacob Laity1, and Sahit Sahni1
1[University of Manchester Robotics Society (RoboSoc)](https://uom-robosoc.com/?ref=blog.roboflow.com) - Poker Arm Project


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Contributing Writer](https://blog.roboflow.com/author/contributing-writer/) . (May 9, 2025). Using Computer Vision to Develop a Robotic Arm Poker Dealer. Roboflow Blog: https://blog.roboflow.com/computer-vision-robotic-poker-dealer/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Contributing Writer


[View more posts](https://blog.roboflow.com/author/contributing-writer/)


### Topics


- [Case Studies](https://blog.roboflow.com/tag/case-studies/)
- [Object Detection](https://blog.roboflow.com/tag/object-detection/)
- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
