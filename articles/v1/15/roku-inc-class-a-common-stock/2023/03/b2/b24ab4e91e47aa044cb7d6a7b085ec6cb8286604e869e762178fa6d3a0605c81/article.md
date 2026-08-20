---
schema_version: "1.0.0"
document_id: "b24ab4e91e47aa044cb7d6a7b085ec6cb8286604e869e762178fa6d3a0605c81"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/improving-quality-of-experience-through-reinforcement-learning"
published_at: "2023-03-16T21:25:47+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:94efab079dd12aa375b3bd6963d03b3ae1d8f7065ee600690ed7b8e887e132d6"
---

# Improving Quality of Experience through Reinforcement Learning

Roku’s mission is to be the streaming platform that connects and benefits the entire TV ecosystem around the world. One way to achieve that mission is to deliver the best **Quality of Experience** (QoE) for the millions of users of Roku streaming devices.


In this article, we share a recent reinforcement learning (RL) project to demonstrate how the Data Science team uses machine learning to help improve customer experience on Roku streaming devices.


## Optimizing QoE on Roku Devices


We consider and measure three elements when defining Quality of Experience on our devices: **bitrate level** , **rebuffer** , and **oscillation in bitrate levels** .


- **Bitrate level** corresponds with the quality of picture that is served. A high bitrate level delivers better quality pictures.
- **Rebuffer** happens when playback is interrupted while waiting for video to be downloaded from the server.
- **Oscillation in bitrate levels** is variation in bitrate from one video chunk to the next.


Ideally, we want our customers to experience the highest available bitrates while keeping rebuffer and oscillation in bitrates at zero. Since there is variability in internet connection quality, however, we can’t always guarantee the ideal.


In such cases, we have to decide among competing priorities: should we keep bitrate level high, or keep rebuffer and oscillation in bitrates low? We asked ourselves, “How can machine learning help us deliver better QoE than the existing algorithm?”


## Using a Reinforcement Learning Approach


After some research, we decided to leverage reinforcement learning (RL) for this problem. There are a few reasons for this decision.


First, the **absence of ground** **truth** makes supervised machine learning unfit for the problem. Defining the ground truth outcome for each decision about bitrate levels is difficult, given that a currently selected bitrate has impact on not just the current video chunk, but future chunks as well.


Second, the reward function in RL allows us to **consider competing priorities** . By defining the reward function as a weighted sum of the three factors (bitrate level, rebuffer, and oscillation in bitrate levels), we can train a model that optimizes the reward.


Finally, recent work in academia, despite being conducted under simplified environments, gives us some **confidence about the feasibility** of such models.


## Challenges


We encountered several challenges with the model and in this article we highlight some of them along with how they were addressed.


### Considering RL in Practice


First, while we have seen interesting results from using RL in the literature, there’s limited evidence of its **performance on devices** . We also saw results where RL did not compare favorably to algorithms that use buffer-based or rate-based approaches.


Despite these practical concerns, we chose to move forward since Roku’s wide variety of devices and large amount of bandwidth data provide us an opportunity to evaluate RL on a large scale.


### Model Size


Second, because the model is deployed onto devices, we want to use **minimal resources** . A model of several megabytes (MB) is considered too big. Our requirement is expressed in kilobytes (KB). We had to significantly reduce the size of the model to make it viable.


We first tried **quantizing the model** using integer quantization, which reduced the model size to 730 KB. Figure 1 below compares the model sizes after applying quantization and conversion to ONNX format.


Figure 1. Model size comparison using different quantization, before and after converting to ONNX format


**Figure 1. Model size comparison using different quantization, before and after converting to ONNX format.**


We kept pursuing other options, however, because 730 KB was still considered too large. Given our constraints, we explored diverse options such as **removing features** and **hidden layers** , as shown in Figure 2 below. For example, removing two features reduced the model size to 180 KB.


Figure 2. Model size comparison for changing model structure


Meanwhile, we evaluated model performance after each model change, and those results are shown in Table 1 below. We found that model performance was not sacrificed by removing two features. These experiments enabled our RL design decision, and we moved forward.


**Table 1. Performance of models with different model structure.**


Model Performance


baseline Baseline


remove 1 feature Similar


remove 2 features Similar


remove 4 features Worse


remove hidden layer


Worse


### Using a Simulator to Train the Model


One way to train models is **training live on devices** . The problem with that approach is that it requires compute power on the device, and the model’s use of scarce resources leads to a **poor user experience** , especially at start-up time. Live training on a device is unfortunately quite slow, too.


We used a simulator to mitigate these problems. While there are some open source simulators available, we found it difficult to replicate Roku logs data when using them. Therefore, the Roku engineering team developed a simulator which has been shown to match well with the user experience reflected in Roku logs.


### A/B Testing


After developing the model, we noticed that it **helped reduce rebuffers without much loss in bitrate levels** . However, we also noticed that the **oscillation in bitrate levels was much higher** than using the existing algorithm. The question for us became, should we do an A/B test in order to evaluate the RL model before widespread deployment?


In the end, we decided to move forward with A/B testing for three reasons.


1. To start, we asked ourselves, “Do the benefits outweigh the side effects?” In medicine, doctors prescribe medications knowing there might be side effects for the patient. In this model, we concluded the benefit of a much better user experience outweighs the potential negative side effects.
2. Second, while we trust the simulator, we agreed that there’s benefit in verifying it using real-world data. This point is especially important because the simulator was developed to emulate the behavior of our existing algorithm, and thus the model may not actually perform as well in the wild.
3. Finally, even if the A/B testing results do not affirm our hypothesis, we will acquire data to improve both the simulator and the model.


### Deployment Cycle


Because the model is deployed on devices, we must release it as part of a Roku OS firmware release, which happens twice a year. It also means we should do the best possible job of testing the model before releasing it to a customer’s device.


Hence, we coordinated closely with our embedded software engineering teams on their release cycle to make sure we delivered the model on time. We also carried out comprehensive tests to be confident in the stability and accuracy of the model.


## Conclusion


The A/B testing results in Table 2 below show that we improved the rebuffer metric by 1.5x. This result far exceeded the 1.2x improvement we were seeing in simulator environments. It means that customers will fewer instances of stalled videos.


**Table 2. A/B test results.**


Existing algorithm RL Z-score p-value ratio of metric difference


2.16% 1.48% 9.33 <0.0001 1.5


-0.68%


We noted a 4% drop in bitrate level in the test group. However, Figure 3 below shows that most of this drop happened at the low bitrate ventiles. The oscillation levels for the RL model are worse than the existing algorithm and this result was consistent with what we observed in the simulator.


Figure 3. Histogram of bitrate levels weighted by play time. Teal for RL and Red for existing algorithm


In summary, while a lower bitrate and increased oscillation in the bitrate level do results in some negative side effects, these are outweighed by the significant rebuffer improvement driven by the RL model. The overall increase in QoE was proven in A/B testing.


These results gave us confidence that we are on the right track in using RL to improve the customer experience on Roku devices. And knowing that this model reduced rebuffers **by more than 30%** motivates us to continue helping Roku deliver on its mission through data science.


The post[Improving Quality of Experience through Reinforcement Learning](https://engineering.roku.com/improving-quality-of-experience-through-reinforcement-learning) appeared first on[Engineering Blog](https://engineering.roku.com/) .
