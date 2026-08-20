---
schema_version: "1.0.0"
document_id: "f81d225fbd2dea49a31930115b5854709fc7f8c707fd08b197245bd0cba659fc"
company_key: "yc-mars-auto"
company: "Mars Auto"
source_id: "yc-mars-auto-rss-2183801a7a92"
canonical_url: "https://blog.marsauto.com/self-driving-2-0-d846625bb327"
published_at: "2022-07-29T18:41:30+00:00"
first_seen_at: "2026-07-25T13:21:51.802210+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:bb72e792a1c09212ffeaf6588db51b01bf6ae18e10ca771db06a227ee3c8caaf"
---

# Self-driving 2.0

# Self-driving 2.0


[Ilsu Park](https://medium.com/@opeltantra?source=post_page---byline--d846625bb327---------------------------------------)


10 min read


·


Jul 29, 2022


--


Press enter or click to view image in full size


From our series A deck


## **Self-driving 1.0**


Despite claims from many self-driving companies about achieving full autonomy, no company has been able to commercialize its technology to date. The vast majority of self-driving companies are still decades away from deploying fully unmanned vehicles on the road. The problem is multifold: they rely on a pre-computed HD map and GPS signals on top of a bunch of rule-based software. Their software is filled with hand-tuned parameters and algorithms. Unlike the perfect driving videos uploaded on their youtube channels, driving is often fragile and risky.


There has been a significant paradigm shift in the way software is written, largely due to the emergence of deep learning approximately 10 years ago. However, the software stack of most companies is not significantly different from the self-driving software that was developed 20 years ago. This is not going to work. Software that human engineers write is impossible to be superhuman by definition. We refer to this traditional self-driving stack as self-driving 1.0.


Press enter or click to view image in full size


2004 DARPA challenge with the same self-driving 1.0 software stack


## **Self-driving 2.0**


Why is it so hard to solve self-driving? Clearly, the reason is that the real world has too many corner cases. Human engineers cannot even imagine all of the corner cases that can happen. It is notoriously difficult to write software that can work perfectly in any situation. Given that almost all companies are clearly failing to commercialize self-driving technology, after spending ridiculously huge amounts of money on R&D, it’s clear that the ancient technical approach is hopeless.


The deep neural network is outperforming humans in many areas.
Following[the paradigm shift from software 1.0 to 2.0](https://karpathy.medium.com/software-2-0-a64152b37c35) , there’s a massive transition happening in the way software is written in many industries. Mars Auto strongly believes that the same strategy should be applied to self-driving as well because it is the only way to solve the self-driving problem and deploy unmanned vehicles on a massive scale.


Over the past few years, we’ve been porting all human-engineered self-driving logic and hand-tuned parameters into an ML model. We will give a specific example of how we ported the lateral control pipeline into an ML model to show why it is superior to the traditional approach.


## **Lateral control example**


At a very high level, the lateral control pipeline consists of several steps, which are **lane detection, depth estimation, tracking, path planning, and vehicle control.** The input to the pipeline is RGB images, and the output of the control algorithm is a desired angle or torque that we should feed into the steering actuator. In this article, we will focus on the steps before vehicle control.


Lane lines are one of the most useful features for path planning. Most companies are developing an algorithm to detect lane lines from point clouds or images. Even companies that rely on HD maps (i.e. companies with pre-defined lane information) usually detect lane lines in real time to increase localization accuracy.


The control algorithm requires the lane line information to be in the 3d space. Therefore, detecting lane lines in the image plane is not enough. We need to unproject them onto the 3d space. If we have dense depth information, just like from LiDAR, the unprojection would be very straightforward. But unfortunately, there’s no depth information in the RGB image. The traditional approach to this problem is to do it using camera geometry with a flat ground assumption.


Flat ground


When a vehicle drives, the camera pitch fluctuates due to the vibration of the vehicle suspension. This makes consecutive image frames look a bit different, resulting in inconsistent lane line predictions between the 2 consecutive frames. When we unproject such images onto 3d space using the camera geometry, the depth of the lane line also fluctuates between the frames. If we use it directly for vehicle control, control output would also fluctuate. This is where the tracker such as the Kalman filter comes in. The tracker exists to smoothen the lane detection outputs between the consecutive frames so that it can be used in the control.


Path planning refers to the computational problem of finding a sequence of valid configurations that moves the object from the source to the destination.


Sounds simple? Unfortunately, the usual lane detection pipeline is very fragile as it inevitably introduces various parameters and assumptions. For example, the post-processing logic for lane detection algorithms requires various hand-tuned parameters and rules. Camera parameters that are used for the projection are not accurate as the camera extrinsic slightly changes all the time due to various factors such as vibration and temperature. The flat ground assumption is just wrong as the real road is never flat. Tunable parameters in the Kalman filter can never be suitable to cover all corner cases in the real world. Hand-tuned parameters and logic can never be good enough to be superhuman.


Let me briefly explain how each step can be removed or ported into an ML model, and become simpler, faster, and safer.


## **Segmentation**


The most straightforward approach for lane detection is semantic segmentation, which predicts pixel-level lane information in the image plane. As pixel-level prediction has too much freedom in the output, it tends to be noisy, especially for unseen data. We need to have a post-processing logic to remove some noise from the output and group pixels that belong to the same lane line together. It isn’t over yet. The output is still pixel-level information. To utilize the output in path planning and control, it needs to be represented as a mathematical curve. Therefore, we also need to have another logic to convert the pixel-level information into some curve parameters.


## Get Ilsu Park’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


The pipeline above is more complicated than it seems, and everything in the pipeline is full of heuristic algorithms and tunable parameters. If we use instance segmentation instead of semantic segmentation, the process can be slightly simplified by eliminating the clustering logic. But it would still be fragile.


Drive with instance segmentation model


## **Curve regression**


If we are to perform curve fitting after segmentation, a better approach would be to predict the curve parameters directly from an ML model. When it comes to lane detection, we already know what lane lines look like. For example, there are no star-shaped lane lines on the road. By using prior knowledge, we can choose an appropriate parametric curve to represent a lane line in the image plane, and let the model predict the curve parameters directly. By doing so, the degree of freedom of the output can dramatically decrease, resulting in increased robustness of the output. We only need to predict N (where N is the number of curve coefficients) * max_number_of_lane_lines outputs as opposed to image_width * image_height * output_channels in the case of segmentation. Also, we can remove all the post-processing logic that was used to derive the curve parameters from the segmentation output. So given an RGB image, we can predict the curve parameters, and feed it into depth estimation directly, such that the curve can be unprojected onto 3d space. Instead of running post-processing and iterative optimization for the curve fitting, we let the model learn how to predict the curve parameters from an RGB image. Is this good enough? Not yet. The depth estimation with the flat ground assumption might be ok on the usual highway, but it is not ok in sharp turns and cities where the road consists of uphill and downhill.


## **Depth estimation**


If we are to ditch the flat ground assumption, we need to have depth information for all image pixels. How can we get dense depth information from RGB images? The most common approach is to use rectified stereo cameras to estimate depth. This approach not only requires more than one camera to work, but it is also slow, sparse, and not accurate for far distances. There are ways to train an ML model that predicts a dense depth map directly from camera images, but it requires huge amounts of data. If we want to replace LiDAR with cameras, we should be able to predict such dense depth maps.


Press enter or click to view image in full size


Useless dense depth map


But when the objective here is just to unproject lane lines onto the 3d space, we only need to have a depth for lane lines.


Given an RGB image, we can train a model that predicts N parametric curves (or sample points) in the 3d space directly. By doing so, we can remove the separated depth estimation step entirely, and drive in the 3d space.


Model prediction in the 3d space


## **RNN**


So far, we’ve been moving most heuristic/ad-hoc algorithms that require a bunch of hand-tuned parameters into a single ML model. The last one remaining is the tracker.


The Kalman filter is one of the best and most widely used state estimators in the robotics industry. It is known to be the optimal linear filter in cases where a) the model matches the real system perfectly, b) the entering noise is “white” (uncorrelated), and c) the covariances of the noise are exactly known. That being said, there’s no way to model the real-world system in a perfect manner mathematically. There is also no way to estimate the covariance of random real-world noises perfectly. Due to this, the Kalman filter is no longer optimal when applied in the real world, although it usually works reasonably well when tuned accordingly.


All measurements collected in the vehicle are time-series data. Images from cameras are also sequential. We can use GRU to learn the context between consecutive image frames, and eventually replace the Kalman filter with GRU. Training the model to learn/memorize the context from infinite past frames is very tricky. However, when trained correctly, it surpasses traditional tracking algorithms.


Without RNN, With RNN


## **Going end-to-end**


Now the ML model can predict accurate lane lines in 3d space from RGB images. As the model understands that the outputs between the two consecutive frames should be very similar, regardless of camera vibration, it produces lane lines that can be directly fed into the path planner without any additional post-processing or tracking. Lane lines are helpful for lateral control, but a human driver can drive well even when there are no visible lane lines. To drive in any situation, our model not only predicts lane lines but also other information that is necessary to control the steering wheel, such as future vehicle position, pose, acceleration, etc. The path planner is not a typical path planner, utilizing traditional path planning algorithms commonly found in academia. It is just a simple heuristic to merge all the model outputs into a single desired path for the control algorithm.


We previously had a path planner, but we also recently removed it and let the model predict the desired path directly. What was wrong with the old path planner?


First, a human driver not only drives at the lane center but also dynamically adjusts the vehicle's offset to the lane center, depending on factors such as road curvature and speed. Second, as we always drive with traffic, the path planner also needs to consider dynamic obstacles on the road. The output of the model, such as lane lines and future vehicle states, doesn’t consider those factors mentioned above. We can either decide to write a logic for them in the path planner or learn everything end-to-end. Again, hand-engineered rules and parameters can never be reliable. Going end-to-end is the right answer.


The lateral control pipeline has become super simple. We drive with the raw model prediction and call it end-to-end lateral control. This is exactly how a human driver drives a car. This sounds very easy, conceptually, but guaranteeing safety in the end-to-end lateral is an extremely hard problem.


## **Self-driving 2.0 is the answer**


In the above example, we only talked about the lateral control pipeline. However, our technical objective is to build a superhuman driving agent entirely using machine learning. We are on our way to moving all of the driving software that runs on the CPU to the GPU. Our strong belief in the data-driven approach didn’t happen overnight. When we first started this company in 2017, we started by building the traditional self-driving software stack. But we quickly realized that the technology was not suitable if we were serious about deploying fully unmanned trucks in the market. The traditional approach was neither safe nor scalable.


[Three years ago, we decided to build everything from scratch with data and automation in mind.](https://blog.marsauto.com/marking-an-important-milestone-at-mars-auto-a864a77a668d) It was a significant decision with high uncertainty, as very few companies were adopting a data-driven approach. But looking back on it now, it is the best decision that we’ve made since the company’s incorporation.


Most companies have already spent too much time and resources on technology, which will not be enough. We’ve been building the technology that is guaranteed to win.


**Join our team and build the truck for the future.**
