---
schema_version: "1.0.0"
document_id: "72c0a46cad3bec9ece7eb0dc59e46ce19204dd368cebeec9a039d20e9537a473"
company_key: "yc-nunu-ai"
company: "nunu.ai"
source_id: "yc-nunu-ai-news-import-9ab4e6dc8961"
canonical_url: "https://nunu.ai/blog/rob-the-robot"
published_at: "2024-12-11T14:00:00+00:00"
first_seen_at: "2026-07-22T22:59:24.636969+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:41eac2211d0a4de495664e5e826885356e3b0ccc1c3f753e3cb40cf4e63ee8a6"
---

# Bridging the Virtual and Physical World with nunu.ai

Hey everyone!


This summer we were lucky enough to work with 4 awesome people -[Deepana Ishtaweera](https://www.linkedin.com/in/deepanaishtaweera/) ,[Robert Jomar Malate](https://www.linkedin.com/in/robjmal/) ,[Zeno Hamers](https://www.linkedin.com/in/zeno-hamers/) and[Zhengyu Fu](https://www.linkedin.com/in/zhengyu-fu-b4992a1bb/) - on an ambitious project we called **Rob the Robot** . This blog will provide a brief overview of the exciting work the team accomplished over the summer.


# The Hypothesis


Our agents are designed to play video games just like humans do: by observing the screen and interacting with keyboard and mouse inputs. We are currently working with game studios to leverage these agents for automated game testing. These vision-based agents work across different games without needing to be trained on any of them. Essentially, they are capable of navigating and acting in any virtual environment (e.g. games).


This raises an exciting question: *How large is the Sim-to-Real gap for our agents?* If our agents can operate effectively in virtual game worlds, could they also generalize to real-world scenarios? This is the task the team set out to explore over the summer.


# The Project


The task seemed deceptively simple: *Apply the exact same agent we use to play video games to a physical robot.* The goal was to evaluate how well our agents generalize from a virtual environment to a physical one and to identify their limitations. By doing so, the project aimed to provide state-of-the-art insights into the current capabilities of embodied AI.


This was a fully unsupervised project, giving the team full autonomy to decide on the best approach. They had 3 months to deliver a working prototype using a quadruped robot with a mounted manipulator.


# The Results


The team’s efforts culminated in four successful demos that showcased the agent’s ability to control a physical robot across various tasks:


**1. Navigation and Object Localization**


Using our agent, they demonstrated not only the capability of navigating to specified locations, but also to detect objects using Rob's cameras.


https://youtu.be/leFAfnitWdg


****


**2. Human-Robot Interaction**


The team enabled interaction with Rob purely through spoken language. Users can give verbal commands, which Rob then executes fully autonomously.


https://youtu.be/x8mC9W242o8


****


**3. Home automation**


Beyond interacting with its environment, the agent could also control it! In this demo, Rob was able to change the colors of LED strips.


https://youtu.be/zsVNaq4caeo


****


**4. Manipulation**


Finally, they showcased Rob's ability to use his mounted manipulator. By having him navigate to and grab a can of Coke, they successfully combined manipulation with navigation!


https://youtu.be/j0IWSN6_Azg


A big thank you to Zeno for editing these videos :)


# Conclusion


The project was a resounding success, exceeding our expectations. The team clearly demonstrated that our agents can control a physical body and perform basic tasks in the real world. However, the project also revealed key limitations in areas like manipulation and latency.


Before we have a million robots in the physical world, we will first see a billion embodied agents in virtual worlds. Fully autonomous embodied AI still has lots of hurdles to clear before it can go beyond cool demos. Some of these hurdles do not exist in games, allowing us to already develop more capable agents in safer, virtual environments. We will continue to focus our efforts on gaming, especially for testing, before we dare to venture onto the physical world.


**A huge thank you to the incredible team who made this project possible!**


[<< back to blog](https://nunu.ai/blog)
