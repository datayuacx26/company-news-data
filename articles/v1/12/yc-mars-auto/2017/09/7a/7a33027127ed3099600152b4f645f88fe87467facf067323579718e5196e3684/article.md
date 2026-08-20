---
schema_version: "1.0.0"
document_id: "7a33027127ed3099600152b4f645f88fe87467facf067323579718e5196e3684"
company_key: "yc-mars-auto"
company: "Mars Auto"
source_id: "yc-mars-auto-rss-2183801a7a92"
canonical_url: "https://blog.marsauto.com/create-self-driving-trucks-inside-euro-truck-simulator-2-c64424d528ed"
published_at: "2017-09-05T06:17:49+00:00"
first_seen_at: "2026-07-25T13:21:51.802210+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:b313fc4448a6229f2794e503a7b7427515d5ea4e21758595a88f6eb0a69ad3c1"
---

# Create self-driving trucks inside Euro Truck Simulator 2

Self Driving Cars


Data Science


Machine Learning


Games


Python


# Create self-driving trucks inside Euro Truck Simulator 2


[Gyuri Im](https://medium.com/@imgyuri?source=post_page---byline--c64424d528ed---------------------------------------)


4 min read


·


Sep 5, 2017


--


In the field of Autonomous Driving, several papers employ the game[TORCS](https://sourceforge.net/projects/torcs/) as a testing tool for research. TORCS, which stands for The Open Racing Car Simulator, has been cited over 20 times, and[more than 300 papers](http://www.cse.chalmers.se/~chrdimi/papers/torcs.pdf) have used the game for developing artificial intelligence algorithms.


Press enter or click to view image in full size


TORCS was first released in 1997.


While TORCS is fine, we wanted a more modern simulation environment with realistic graphics and physics. After a few weeks of coding, we came up with[europilot](https://github.com/marshq/europilot) : an open source project that enables you to create self-driving trucks inside Euro Truck Simulator 2 (ETS2).


Truck driving itself inside Euro Truck Simulator 2


We got started on the project out of frustration, when we wanted to create a self-driving program with neural networks. What we found on the web was either partly closed source, hard to build, limited in features, unrealistic graphics, or compatible only with Windows (We don’t have Windows).


Press enter or click to view image in full size


Unlike most games, ETS2 runs on OS X and Linux, as well as Windows.


So we created a tool to control ETS2 with python, which runs on OS X and Linux.


*Quick tip: Steam has*[periodic sales](https://steamdb.info/app/227300/) *that sells ETS2 for $4.99. A quick Google search will find you other sellers that have the game on sale.*


Delving into the project, europilot is a bridge between ETS2 and your python program. The usage can be largely divided into two cases.


## Get Gyuri Im’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


I. Creating driving datasets used for training neural networks.


- By specifying the area of the screen to capture, europilot captures the screen along with the steering wheel data. This data is nicely formatted into a csv file.


II. Testing self-driving programs.


- Europilot can create a virtual joystick driver that can be recognized inside ETS2. A real-time inference network can use this joystick to output the relevant steering commands to control the truck inside the game.


We tried to make the project easy to use. There are example notebook files that takes you through each step. All in all the project is fairly simple, because the project relies heavily on other open source projects. Feel free to dive into the source code.


There are several approaches to creating self-driving programs. In the paper[DeepDriving: Learning Affordance for Direct Perception in Autonomous Driving,](http://deepdriving.cs.princeton.edu/paper.pdf) the authors describe three major paradigms.


> Today, there are \[three\] major paradigms for vision-based autonomous driving systems: mediated perception approaches that parse an entire scene to make a driving decision, and behavior reflex approaches that directly map an input image to a driving action by a regressor … \[and\] a third paradigm: a direct perception based approach to estimate the affordance for driving.


Press enter or click to view image in full size


Three approaches described in DeepDriving[paper](http://deepdriving.cs.princeton.edu/) .


We implemented a “behavior reflex”, or “end-to-end” approach, inspired by the paper[End to End Learning for Self-Driving Cars](https://arxiv.org/abs/1604.07316) . The paper presents PilotNet, a CNN architecture that maps front-facing camera frames to steering commands.


Press enter or click to view image in full size


The training process described in NVIDIA’s[paper](https://arxiv.org/abs/1604.07316)


Our implementation is similar to the model presented in the paper. However our implementation only includes frames from one front-facing camera, and doesn’t have data augmentation. It also has Batch-Normalization after every layer, and a larger input size than the original model in the paper. We found out that even without data augmentation and a training set of only 5 hours, the model worked surprisingly well.


Truck driving on curved road


Truck driving while raining at night


While there is more work to do, we wanted early feedback from the community to help shape the future of the project. Hope you find the project useful, or at least amusing.


*You can visit the project at*[https://github.com/marshq/europilot](https://github.com/marshq/europilot) *.*
