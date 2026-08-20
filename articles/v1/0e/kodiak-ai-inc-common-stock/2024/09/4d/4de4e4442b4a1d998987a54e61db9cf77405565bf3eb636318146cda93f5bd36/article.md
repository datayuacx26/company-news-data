---
schema_version: "1.0.0"
document_id: "4de4e4442b4a1d998987a54e61db9cf77405565bf3eb636318146cda93f5bd36"
company_key: "kodiak-ai-inc-common-stock"
company: "Kodiak AI Inc."
source_id: "kodiak-ai-inc-common-stock-rss-dc4af1b0a9ba"
canonical_url: "https://kodiak.ai/news/llms-take-the-wheel"
published_at: "2024-09-26T14:00:00+00:00"
first_seen_at: "2026-07-20T23:18:01.334765+00:00"
fetched_at: "2026-07-28T20:59:23.866660+00:00"
content_hash: "sha256:f603f9b4258e7294c773e00e4f2a2f49ab1a192800bc867cd1dfd9668c3feb10"
---

# LLMs Take the Wheel: How Generative AI Can Improve Autonomous Safety

# LLMs Take the Wheel: How Generative AI Can Improve Autonomous Safety


[Shubham Shrivastava](https://kodiak.ai/news?author=shubham-shrivastava)


|


September 26, 2024


### Can you spot the emergency vehicles on the road up ahead?


Autonomous vehicle development is never static. At Kodiak AI, development isn’t just about tuning models and adding new features: we are constantly searching the technology frontier for new techniques that can improve the safety and efficiency of the Kodiak Driver, our self-driving system. One of the most exciting new technologies we have integrated into our system is Vision-Language Models, which harness the power of generative AI to add new capabilities and improve safety.


While autonomous vehicles necessarily interact with the world through sensors such as cameras, the vast majority of human knowledge is recorded in text. Indeed, it’s impossible for humans to look at a car without the word “car” popping into their heads - visual information and language are deeply intertwined. Combining text-based knowledge and context with images that are actionable for AVs can offer insights that can enhance decision-making in autonomous driving. The key question is how to effectively access and utilize this wealth of information.


That’s where Vision-Language Models (VLMs) come in. VLMs are a variant of AI-based Large Language Models that are trained to concurrently process vast amounts of data from various sources, including text, images, and videos. VLMs mimic how humans learn: we teach children about the world by showing them pictures, reading them stories, and playing videos, so they can learn to understand the world through different senses and understand how different stimuli fit together. VLMs are best known for enabling people to render beautiful images in seconds from a text prompt, but they have profound implications for AV perception.


When humans describe the world, they consider both the events they observe and why those events occurred. By combining visual and textual data and training multimodal decoders in conjunction, VLMs enable AVs to incorporate that human-like basic understanding of the world, and develop a model of how actors interact with each other. Grounding image features through language encodes far richer representations into neural networks, adding depth to an AI model’s understanding and achieving a more advanced level of artificial intelligence. This in turn enables VLMs to understand and navigate complex scenarios using ideas informed by human intelligence and enabling more human-like driving behavior. VLMs therefore offer unprecedented insights into complex driving scenarios, allowing the Kodiak Driver to understand and respond to natural language descriptions, enhancing safety and performance.


> Using the power of VLMs allows the Kodiak Driver to think like a human.


Training and alignment of vision and language features in the joint embedding space through maximization of image-text pair similarity, and zero-shot classification of scenes through VLM inference onboard our autonomous truck’s perception stack.


By understanding the context and nuances of each situation, VLMs enable our trucks to make split-second decisions with remarkable accuracy. Take the emergency vehicle examples up above. Traditional perception approaches, including the ones Kodiak used just a couple years ago, struggle with scenarios like those one because they are definitionally unpredictable: it’s relatively rare to see first responders block entire lanes of traffic. Additionally, identifying the full range of on-road objects ahead of time is not only infeasible, it can lead to dataset distribution biases, as we keep branching the objects into ever increasing categories. VLMs are able to identify unusual cases such as the one above because they have a built-in high-level understanding of what emergency vehicles may look like, and what it means for them to be stopped ahead in an adjacent lane. We can literally feed the VLM a range of text prompts describing critical scenarios during each cycle, and can identify the highest-risk situations, even if we’ve never seen them in our training data set. So by querying “emergency vehicles” on an ongoing basis, we can robustly evaluate the risk that there is an emergency vehicle in the scene.


> VLMs represent a breakthrough in our ability to navigate complex scenes never seen before, conquering the long tail of edge cases.


VLMs therefore greatly enhance the Kodiak Driver’s ability to understand and respond to complex, novel driving scenarios, by recognizing patterns and making connections between what they "see" and what they "know." This innovative approach allows us to prompt the system using natural language, enabling the truck to describe its surroundings and plan its route accordingly. The synergy between language and vision empowers our trucks to navigate with precision, even in the most challenging environments. By aligning visual data with text, VLMs allow the detection of rare events, such as a police car on the highway, without explicit training. This synergy enriches the neural network's representations, enabling precise navigation in challenging environments and improving our ML stack's robustness and adaptability.


Traditional Machine Learning models are incredibly powerful, but their predictions might be completely off when presented with a data sample that is out-of-distribution - i.e. the edge cases that make up the most challenging driving scenarios. While training specialized neural networks such as a multimodal spatio-temporal models or foundation models, we tend to use the rarest of examples we have come across, such as a house being towed on the highway, or a person running across the highway in the middle of the night wearing a dark hoodie. VLMs bring a huge advantage in identifying additional edge cases and incorporating that knowledge into existing models. They also help us generalize to new environments such as off-road terrain, where we need to navigate through soft hanging vegetation and over grass and bushes.


Highway emergencies, like roadside fires and smoke, are difficult to detect using traditional machine learning methods. Vision-Language Models (VLMs) can recognize these scenarios without needing training on specific data samples, enhancing our ability to respond to unexpected events.


VLMs additionally allow the Kodiak Driver to make “zero-shot predictions” i.e. identify and make informed decisions about novel potential hazards that we’ve never seen before in the real world. For example, by grounding vision with language features, the Kodiak Driver can understand that branches are not rigid objects, allowing us to handle new off-road scenarios we’ve never previously encountered.


Detecting sensor occlusions can be surprisingly difficult, given the complexity of identifying thousands of occlusion events to train AI perception models. With VLMs, we can detect occlusion even without having a single sample in our dataset.


There are additional scenarios where VLMs provide capabilities traditional AI perception models can’t. For example, scenarios such as a pedestrian too close to the vehicle are complex from the point of view of a traditional AI algorithm, largely because there simply aren’t sufficient training data points to extrapolate to a broader set of scenarios. So scenarios like the ones above, with Kodiakers Harpreet and Vince standing very close to the cameras, can be difficult to understand and interpret. However, vision-language foundation models can utilize various queries to identify the most intricate situations, even without explicit training data. Again, simply by querying “person close to the camera” we can evaluate the risk that there is a person occluding a camera in a way that no traditional AI perception model can. The rich representations VLMs embody also help us evaluate the likelihood of a specific input sample in the most abstract sense and trigger various events for collecting data, identifying anomaly, finding descriptive scenarios, sourcing semantically similar data, and so on.


Figure - Manifolds of Data Distribution


> Vision-Language Models empower Kodiak to navigate the rarest and unseen scenarios with human-like intelligence, making autonomous driving safer and more reliable.


VLMs require vast computing power to implement in real-time. That’s where Kodiak’s partnership with Ambarella comes in. Ambarella's CV3-AD family of AI domain control SoCs is at the forefront of embedded hardware technology, designed to handle the demanding requirements of autonomous driving. The CV3-AD SoC family’s advanced processing capabilities make it the ideal choice for implementing VLMs in self-driving trucks, ensuring efficient and reliable performance.


Our collaboration with Ambarella will unlock a major opportunity in the application of VLMs to self-driving and inference chips. By harnessing the power of VLMs and the robust capabilities of Ambarella's CV3-AD SoCs, we are poised to set new benchmarks in the autonomous driving industry.


By integrating VLMs into the Kodiak Driver, Kodiak is at the forefront of a technological revolution. As we continue the work of integrating Ambarella's CV3-AD into the Kodiak Driver, we will further solidify our positions as autonomous industry leaders, delivering unparalleled safety and efficiency in real-time autonomous driving.


### Categories


- [Technology](https://kodiak.ai/news?category=technology)
- [Safety](https://kodiak.ai/news?category=safety)
- [Company](https://kodiak.ai/news?category=company)


## More like this


June 3, 2026[Quantifying Safety Knowns and Unknowns: How Kodiak Uses PRA and AI to Measure Risk](https://kodiak.ai/news/pra-quantifying-safety)


May 18, 2026[Kodiak AI Issues Statement on BUILD America 250 Act Bill Supporting Autonomous Trucks](https://kodiak.ai/news/build-america-250-act)
