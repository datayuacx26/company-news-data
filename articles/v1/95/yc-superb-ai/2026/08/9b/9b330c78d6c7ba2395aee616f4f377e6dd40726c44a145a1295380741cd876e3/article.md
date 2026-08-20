---
schema_version: "1.0.0"
document_id: "9b330c78d6c7ba2395aee616f4f377e6dd40726c44a145a1295380741cd876e3"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/3d-gaussian-splatting-korean-home-digital-twin-en"
published_at: null
first_seen_at: "2026-08-14T19:58:41.367478+00:00"
fetched_at: "2026-08-14T19:58:43.787652+00:00"
content_hash: "sha256:8d86cf049232c1a8230f9d811cd229f6c795efa085b2ffd45ff585bac4dfd26e"
---

# Not a Photo, but a Space You Can Walk Into: Reconstructing 50 Korean Homes with 3DGS

Tech


Not a Photo, but a Space You Can Walk Into: Reconstructing 50 Korean Homes with 3DGS


Hyun Kim


Co-Founder & CEO | 2026/08/14 | 5 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


When people hear about building learning environments for robots, many imagine a 360° panoramic tour. But a space you can see is not the same as a space you can enter.


A panorama is valid only from the point where the camera was positioned. What robot learning requires is a space where the viewpoint can move freely — a space where robots can move through, collide with objects, and navigate.


In Phase 2 of Korea’s Sovereign AI Foundation Model Project, or Dokpamo in Korean, we reconstructed 50 Korean home environments into exactly that kind of space.


### **What Is 3D Gaussian Splatting, or 3DGS?**


3D Gaussian Splatting is a 3D reconstruction technique that does not simply stitch together multiple photos. Instead, it learns how light is distributed throughout a space and renders realistic views from any viewpoint. Because the viewpoint can move freely within the result, it becomes a space you can “walk into,” not just view.


# **Not a Scan Built to Look Good, but a Scan Robots Can Use**


There are already many 3DGS demo videos on the internet, and scans captured with high-precision equipment can achieve remarkable visual quality.


Our focus was different.


We focused on creating structurally accurate scans from footage captured with **standard cameras by operators who were not 3D capture specialists** — scans that preserve real-world scale and calibrate ground orientation.


To place a space inside a simulator, the system needs to know “how many meters” something is and “where the floor is.” Looking good on screen and allowing a robot to move through the space according to physical rules are entirely different requirements.


We built for the latter. Visual quality is an area we will continue improving in Q3.


Scans built for viewing and scans built for robot learning have different requirements


# **A Hybrid Structure Combining 3DGS Backgrounds and Physical Meshes**


One point is important to clarify. 3DGS captures an entire space as a whole, which means the objects inside the space are not separated individually.


For this reason, our spatial assets are built in two layers. **3DGS provides the realistic background, while objects that people interact with — such as doors, drawers, dishes, and clothing — are created as separate meshes with physical properties and placed on top.**


Physical interactions, such as a robot colliding with an object or opening and closing it, happen through the mesh. 3DGS makes the scene look realistic. The object assets responsible for interaction will be covered in Part 3.


The two-layer structure of spatial assets — physical mesh objects are placed on top of a 3DGS background


The quality criterion of “collision errors within 10%” is measured on top of this structure. This metric measures the ratio of physically implausible scenes, such as a rendered person passing through a wall or feet sinking into the floor. Because 3DGS point clouds do not provide complete structural information on their own, we also fit the point cloud to wall and floor planes.


# **The Hardest Space: The Laundry Room**


Most of the 50 locations were apartments, so after optimizing the algorithm on one location, we were generally able to apply it stably to others.


The real challenge was narrow rooms. In tight spaces such as laundry rooms, even careful capture cannot secure enough diversity in camera position and angle, making accurate reconstruction difficult. These were the kinds of limitations we identified by reconstructing 50 real spaces ourselves.


# **Proof That It Actually Runs: We Recreated Our Entire Office**


Using the same pipeline, we also created a digital twin of our entire office space. This was an internal project separate from the official Dokpamo deliverables. We placed people and quadruped robots together inside the virtual office and enabled them to walk around without colliding.


During the process, we directly fixed a bug in the widely used Isaac Sim 5.1 environment where person-to-person avoidance did not work properly. We also redesigned the default avoidance logic so it could account for multiple people at the same time.


A similar direction of improvement was later reflected in Isaac Sim 6.0. Between reconstructing a space and using that space to produce training data, this kind of detailed engineering work is required.


# **How the Global Race for Physical Spaces Is Taking Shape**


The race to secure physical spaces as learning infrastructure is already global.


In Germany, the Technical University of Munich (TUM) and NEURA Robotics announced Europe’s largest Physical AI training center, a 2,300-square-meter facility backed by a joint investment of €17 million. In China, more than 40 training centers were established in the past year alone.


Beyond competition over physical facilities, the ability to turn acquired spaces into reusable digital assets will determine how efficiently those facility investments translate into AI training infrastructure.


# **Frequently Asked Questions**


### **Q. How is this different from indoor scanning services such as Matterport?**


Scans built for viewing are designed to be seen. Our spatial assets are designed for robot learning. They preserve real-world scale, align with the ground plane, and separate interactive objects into physical meshes so robots can learn movement, collision, and manipulation inside the space.


### **Q. Which simulators can the 3DGS outputs be used in?**


The assets were built and validated for integration and operation in NVIDIA Isaac Sim.


### **Q. Can the same approach be applied to spaces beyond homes, such as factories or logistics centers?**


Yes. The same pipeline can be applied to other environments, and our office-space demo is one example.


In the next article, we will cover the objects that fill these spaces: 10,000 intelligent object assets.


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Tech How Robots Learn Human Behavior: Building 5,000 4D Behavior Data Assets Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/4d-human-action-data-robot-learning-smpl-en)[Tech 50 Korean Homes Became Robot Training Data: Building 50 3D Spaces, 5,000 Behaviors, and 10,000 Object Assets Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/physical-ai-data-factory-dokpamo-phase-2-results-en)[Tech How ZERO Won the CVPR 2026 Foundational Few-Shot Object Detection Challenge: A Technical Walkthrough of the Winning Solution Hyun Kim Co-Founder & CEO | 10 min read](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
