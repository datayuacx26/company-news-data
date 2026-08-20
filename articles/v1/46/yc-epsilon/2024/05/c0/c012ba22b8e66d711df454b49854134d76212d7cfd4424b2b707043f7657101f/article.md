---
schema_version: "1.0.0"
document_id: "c012ba22b8e66d711df454b49854134d76212d7cfd4424b2b707043f7657101f"
company_key: "yc-epsilon"
company: "Epsilon"
source_id: "yc-epsilon-news-import-517fa5d897bb"
canonical_url: "https://www.epsilon-ai.com/blog/ai-climate-science"
published_at: "2024-05-15T00:00:00+00:00"
first_seen_at: "2026-07-23T08:50:08.673710+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:dc336a4fcdca676d53e70b588e50d32a2badd41b6289d61650468d8a5a51281d"
---

# 5 Ways AI is Impacting Climate Science

# 5 Ways AI is Impacting Climate Science


Artificial intelligence, including traditional ML approaches along with newer generative AI models, are having an enormous impact on climate science. From tackling flood prediction, to designing ideal crops, to generating city designs - AI will shape the way society interacts with our climate.


[Eshan Agarwal](https://www.epsilon-ai.com/blog/eshan-agarwal)


[Eshan Agarwal](https://www.epsilon-ai.com/blog/eshan-agarwal)


•


Published on May 15, 2024


•


Table of Contents


- [Traditional ML Approaches](https://www.epsilon-ai.com/blog/ai-climate-science#traditional-ml-approaches)
- [Generative AI Approaches](https://www.epsilon-ai.com/blog/ai-climate-science#generative-ai-approaches)


---


-
-
-
-


Do not index


Below are five specific examples of how artificial intelligence is impacting climate science research. They’re broken into two sections: approaches involving ML prediction and approaches leveraging generative AI.


####


Traditional ML Approaches


Traditional machine learning leverages existing data to make accurate predictions about future events. One theme in applying ML to climate science, is being able to replace physics based models that encode information in the form of heuristics to learned models that can identify key features automatically from the data.


**Crop Breeding** \[1\]


Rising populations and climate change have placed increasing pressure on crop breeding. Prior techniques involved lots of manual trial and error, crossing various plant lines until an ideal candidate appeared. This process can take years.


Now crop breeders can leverage machine learning techniques to discover and optimize genetic traits programmatically for different climates. Researchers can provide data such as DNA sequences, desirable phenotypic traits, and yields and predictive models can do the rest. Geneticist Steven Tanksley describes the process.


“The algorithm projects which genes are associated with which traits under which environ-mental conditions and then determines the optimal combination of genes for a specific breeding goal, such as drought tolerance in a particular growing region, while accounting for genes that help boost yield. The algorithm also determines which plant lines to cross together in which order to achieve the optimal combination of genes in the fewest generations.”


There are some tradeoffs with applying some of these methods. Those can include


1. **Consumer Preferences** - For example, the algorithmic output may eliminate certain traits that affect taste and appearance which impacts consumer buying patterns.


1. **Data collection** - **** collection ground truth data for modeling can be painstaking. For example, to correctly model the amount of kernels on an ear of corn, one might have to manually count the number kernels of kernels for a small sample.


1. **Interaction effects** - while models can identify genes that lead to desirable traits, they still often struggle to prediction interaction effects when many different genes and traits are mixed together.


**Hydrological Forecasting** \[2\]


Hydrological forecasts are predictions about the future state of water in natural and artificial systems, including rivers, lakes, reservoirs, and groundwater. These forecasts are crucial for managing water resources, preparing for and responding to floods and droughts, and supporting various activities such as agriculture, urban planning, and hydropower generation.


One example of how AI can this line of research, is in flood forecasting. In their paper on on regional inundation forecasting, Shun-Nien Yang and Li-Chiu Chang build a sequential neural network to build highly accurate short term predictions of average regional inundation depth (ARID). ARID is a metric used to describe the average depth of flooding over a specified region and is calculated by taking the following ratio:


The researchers leverage recurrent data, which refers to datasets in which the output from one time step becomes the input at the next time step, while adding IoT sensor data and rainfall measurements. The resulting model improves accuracy for short term ARID predictions. This is a huge advantage compared with physical based models which are very compute intensive and limit their short term applicability. In the event of a natural disaster, having accurate short term predictions can save lives and mitigate damage.


**Turbulence Prediction** \[3\]


Accurate turbulence predictions are important for maintaining safety and comfort in commercial aircraft operations. Munoz-Esparza, Sharman, and Deierling build an ML model leveraging regression trees to improve state-of-the-art turbulence prediction.


Statistical ML models allow for learned features that reduce model complexity while maintaining predictive performance. For example, the previous state-of-the-art models required complex simulation and calibration steps that made models less convenient to use in real-time scenarios.


The researchers note that these ML based methods can also reduce false positives during light turbulence conditions. This is a trend in ML based climate science research. Several other works studying rainfall and tropical storms have also found ML methods superior at correctly predicting outlier conditions.


####


**Generative AI Approaches**


While ML methods are great at leveraging existing datasets for predictive tasks, generative AI allows for a whole new suite of use cases. Some examples are below:


**Synthetic data** \[4\]


Data quality and volume is crucial for generating predictive models. Generative AI can help address this bottleneck by creating synthetic data, i.e. data points that mimic an underlying probability distribution but are different than existing samples.


GANs (Generative Adversarial Networks) are leveraged for this task. One model is trained to generate fake data by sampling from a probability distribution and a second model is trained to differentiate the fake data points from the real ones. These two models compete with each other until an optimum level of data creation is reached.


This technique has far reaching applications in geoscience. It can be used to generate realistic seismic images, simulate geological structures, and augment remote sensing data amongst others.


**Urban Planning** \[5\]


Due to the nature of climate change, there is great interest in simulating urban settings in different environmental conditions to estimate design and evolve cities with sustainable infrastructure.


Some examples of generative urban design include


- **Buildings Imagery -** Generative models have been able to produce building designs from a aerial photographs with >90% accuracy


- **Neighborhood Layouts** - graph neural networks have been able to produce neighborhood layouts in over 28 cities in North America. Research suggests that these models are prefered over prior approaches.


- **Flood resistance** - generated urban models have been created to study natural ways to incorporate flood resistance into urban design


**Conclusions**


\[1\][https://www.pnas.org/doi/pdf/10.1073/pnas.2018732117](https://www.pnas.org/doi/pdf/10.1073/pnas.2018732117)


\[2\][https://www.mdpi.com/2073-4441/12/6/1578](https://www.mdpi.com/2073-4441/12/6/1578)


\[3\][https://journals.ametsoc.org/view/journals/apme/59/11/JAMC-D-20-0116.1.xml?tab_body=pdf](https://journals.ametsoc.org/view/journals/apme/59/11/JAMC-D-20-0116.1.xml?tab_body=pdf)


\[4\][https://www.semanticscholar.org/paper/When-Geoscience-Meets-Generative-AI-and-Large-and-Hadid-Chakraborty/3927a679b60dd3d671224fa0bc7297bccbe2d493](https://www.semanticscholar.org/paper/When-Geoscience-Meets-Generative-AI-and-Large-and-Hadid-Chakraborty/3927a679b60dd3d671224fa0bc7297bccbe2d493)


\[5\][https://link.springer.com/content/pdf/10.1007/s43762-023-00111-z.pdf](https://link.springer.com/content/pdf/10.1007/s43762-023-00111-z.pdf)


### Stay Up To Date


Get updates on feature launches, learnings, and industry deep dives!


[Subscribe](https://www.epsilon-ai.com/blog/ai-climate-science#newsletter-in-footer-wrapper)


Written by


[Eshan Agarwal](https://www.epsilon-ai.com/blog/eshan-agarwal)


Founder, CEO


- [Twitter](https://twitter.com/)
- [LinkedIn](https://linkedin.com/)


## Stay Up To Date!


Get emails when we post new features, learnings, or guides


No spam. Unsubscribe at any time.
