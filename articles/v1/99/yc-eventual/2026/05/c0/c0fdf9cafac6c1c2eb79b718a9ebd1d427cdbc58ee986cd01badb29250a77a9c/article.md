---
schema_version: "1.0.0"
document_id: "c0fdf9cafac6c1c2eb79b718a9ebd1d427cdbc58ee986cd01badb29250a77a9c"
company_key: "yc-eventual"
company: "Eventual"
source_id: "yc-eventual-news-import-1ca85739ec11"
canonical_url: "https://www.eventual.ai/blog/vlas-are-dead-long-live-world-action-models"
published_at: "2026-05-08T08:00:00+00:00"
first_seen_at: "2026-07-28T08:49:54.145619+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:5b3a4163eba79a210e6af39aacd9d11e86c2e9e1d55149132b86c667e06ad9df"
---

# VLAs are dead, long live World Action Models - a summary of Jim Fan's Robotics End Game talk

> Our generation was born too late to explore the Earth and too early to explore the stars, but we are born just in time to solve robotics.


--[Jim Fan, AI Ascent 2026](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=1181)


That's how Jim Fan closed his talk at Sequoia's AI Ascent 2026 - and it might be the most concise summary of where robotics stands right now.


Fan leads Nvidia's embodied autonomous research group (essentially Nvidia Robotics). At last year's AI Ascent he was a standout presenter. This year, he came back with a bolder claim: robotics is entering its end game, and the playbook is already written - because it's the same one that LLMs already followed.


Here's a breakdown of his 20-minute talk.


## The Great Parallel


Fan's core thesis is what he calls["the great parallel"](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=173) - the idea that robotics will follow the exact same trajectory as large language models. He frames it bluntly:["as any self-respecting scientist would do, I copy homework and I give it a new name."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=167)


The LLM path went through four stages in six years:


1. **Pre-training** (GPT-3) - learning the shape of language through next-token prediction
2. **Supervised fine-tuning** (InstructGPT) - aligning the model to do useful work
3. **Reasoning** (o1) - using reinforcement learning to surpass imitation learning
4. **Auto research** - accelerating the loop beyond what's humanly possible


The robotics parallel: instead of simulating strings, simulate the next physical world state. Align through action fine-tuning onto the slice of that simulation that matters for real robots. Then let RL carry the last mile.


## Why VLAs fall short


Vision-language-action models (VLAs) have dominated robotics for the past three years. Models like[pi0](https://arxiv.org/abs/2410.24164) and Nvidia's own[GR00T](https://developer.nvidia.com/blog/accelerate-generalist-humanoid-robot-development-with-nvidia-isaac-gr00t-n1/) fall into this category. The approach: take a vision-language model and graft an action head on top.


Fan's critique is pointed. He argues these models are really "LVAs" - because[the most parameters are dedicated to language](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=237) . Language is the first-class citizen, followed by vision, then action. The result: VLAs are["great at encoding knowledge and nouns, but not so much at physics and verbs. It's kind of head heavy in the wrong places."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=248)


His example: the original VLA paper showed a robot moving a Coke can to a picture of Taylor Swift - impressive generalization to an unseen concept, but[not the kind of pre-training ability robotics actually needs](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=262) .


## Video world models: the unlikely hero


The replacement for VLAs starts in an unlikely place - AI-generated video. Fan acknowledges the irony. Nobody takes AI video slop of[cats playing banjo on security cameras](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=285) seriously. But something important is happening under the hood.


Video models like VEO-3 are learning to simulate physics internally. They pick up[gravity, buoyancy, lighting, reflection, and refraction](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=306) - all by themselves. None of it is coded in. As Fan puts it:["Physics emerge by predicting the next blob of pixels at scale."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=315)


Even[visual planning emerges](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=321) - VEO-3 can solve mazes by running simulation forward in pixel space. (Though Fan notes it sometimes cheats:["VEO-3 figures out that if you're not looking, geometry is optional."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=344) He calls this "physics slop.")


## Dream Zero: from world models to robot actions


To make world models useful for robotics, Nvidia built[Dream Zero](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=369) - a policy model that "dreams a couple seconds into the future and acts accordingly."


The key insight: motor actions are high-dimensional continuous signals - which look a lot like pixels. So Dream Zero jointly decodes predicted future video frames and robot actions at the same time. The correlation is tight:["If the video prediction works, the action works. If the video hallucinates, the action fails."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=409)


The result is zero-shot generalization to tasks and verbs never seen in training. Fan compares it to GPT-2 - not 100% robust, but getting the shape of the motion correct in every case. It's a first step toward open-ended, open-vocabulary prompting for robotics.


Fan calls this new paradigm **World Action Models (WAMs)** . And he's not subtle about the transition:["Let's all take a moment of silence for our dear friend VLAs. They've served us well. Rest in peace. Long live world action models."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=452)


## The data revolution: teleop is dying


The model story is only half the picture. Fan spends equal time on data strategy - and his argument is just as aggressive.


Teleoperation has dominated robotics data collection for the past three years. VR headsets, optimized streaming latency, complex rigs that Fan says[look like "medieval torture devices."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=494) The fundamental problem: teleop is[upper bounded by 24 hours per robot per day](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=504) . In practice, it's["more like 3 hours per robot per day, and only when the robot god is merciful."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=510)


Fan lays out three tiers of data collection, in order of scalability:


### Tier 1: Data wearables (UMI)


Instead of teleoperating a robot, you wear the robot end-effector on your own hand. This is the[Universal Manipulation Interface (UMI)](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=525) approach. Fan calls UMI "perhaps one of the greatest papers ever written in robotics data" - it spawned two unicorn startups (Genesis and Sunday).


Nvidia extended this with[DexUMI](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=561) - an exoskeleton with a one-to-one mapping to five-finger dexterous robot hands. The results: fully autonomous robot policies trained on zero teleoperation data.


### Tier 2: Egocentric video (Ego-Scale)


Data wearables are better than teleop but still cumbersome. Fan draws a comparison to Tesla's FSD - when you drive, you contribute to a massive data flywheel without even noticing. Robotics needs the same thing: data collection that[fades into the background](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=648) .


The answer is human egocentric video with hand tracking and language annotations. Nvidia's[Ego-Scale](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=677) system is pre-trained on[21,000 hours of in-the-wild egocentric human data with zero robot data](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=699) . Then action fine-tuning uses only 50 hours of mocap data and[4 hours of teleop - less than 0.1% of the training mix](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=722) .


The result: an end-to-end policy that maps camera pixels directly to 22-degree-of-freedom dexterous robot hands. It generalizes to tasks like sorting cards, manipulating syringes, and learning new shirt-folding strategies from a single demonstration.


Perhaps the biggest finding: they discovered[a neural scaling law for dexterity](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=755) - a clean log-linear relationship between pre-training hours and validation loss. Six years after the original neural scaling law for language models, robotics now has its own.


Fan's scalability chart puts it in perspective:


- **Teleop** : least scalable
- **Data wearables** : hundreds of thousands of hours
- **Egocentric video** : easily 10 million hours in the next year if the FSD-style flywheel spins up


His prediction: teleop will drop to negligible amounts within a year or two. The main diet for robotics will be egocentric videos.


## Dream Dojo: neural simulators for RL


The final piece is scaling environments for reinforcement learning. Just like LLM labs spend significant budgets acquiring millions of coding environments for RL, robotics needs the same.


Nvidia's approach has two parts:


1.


**Real-to-sim-to-real** : Take an iPhone photo, run it through a 3D world scan pipeline to extract interactive objects, then augment infinitely with "digital cousins" in simulation. Your[iPhone becomes a pocket world scanner](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=921) .


2.


**Dream Dojo** : Video world models turned into full neural simulators.[Dream Dojo](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=941) takes continuous action signals as input and outputs RGB frames and sensor states in real time.["No physics equation, no graphics engine involved."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=970) Every pixel is generated by the model.


The new post-training paradigm: massively parallel RL running across a few real robot stations, graphics cores doing world scans, and heavy inference compute running world models. Or as Fan summarizes:["Compute now equals environment now equals data."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=995)


## Three milestones to the finish line


Fan frames the remaining robotics roadmap as three "achievements" on a civilizational technology tree:


1.


**[Physical Turing test](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=1042)** - across a wide range of activities, you can't tell whether a human or robot did the task. Fan estimates[two to three years away](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=1067) .


2.


**[Physical API](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=1071)** - robot fleets configured like software through APIs and command lines. This enables["lights-out factories - essentially printers of atoms"](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=1089) that take design files as input and output fully assembled products. Also: automated wet labs for chemistry, biology, and medicine.


3.


**[Physical auto-research](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=1110)** - robots designing, improving, and building the next iteration of themselves beyond what's humanly possible.


Is this science fiction? Fan's argument: it took 14 years to go from AlexNet (2012) barely recognizing cats vs. dogs to agentic auto-research in 2026. Add another 14 years and you land at 2040. And["technology does not advance linearly, it advances exponentially."](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=1155)


His closing bet:[95% certainty that we reach the end of the robotics technology tree by 2040](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=1159) .


## Key takeaways


- **VLAs are being replaced by World Action Models (WAMs)** that use video world models as their pre-training backbone instead of language models. Vision and action become first-class citizens.
- **Teleoperation is on its way out.** The future of robotics data is egocentric human video - and Nvidia has already shown scaling laws that validate this approach.
- **Neural simulators (Dream Dojo) change the RL equation.** Compute equals environments equals data - no physics engine required.
- **The LLM playbook transfers directly to robotics.** Pre-train on world simulation, align through action fine-tuning, let RL carry the last mile.


The full talk is worth watching:[Robotics' End Game: Nvidia's Jim Fan](https://www.youtube.com/watch?v=3Y8aq_ofEVs) (20 min, Sequoia Capital).


## Learn more


If you want to keep learning about physical AI, feel free to subscribe to our[newsletter](https://topicqueue.substack.com/) . If you're in the process of setting up petabyte-scale data pipelines for physical AI, you might want to check out[MultiBase](https://www.eventual.ai/multibase) - the data warehouse for physical AI, with optimized data loading for the latest NVIDIA chips.
