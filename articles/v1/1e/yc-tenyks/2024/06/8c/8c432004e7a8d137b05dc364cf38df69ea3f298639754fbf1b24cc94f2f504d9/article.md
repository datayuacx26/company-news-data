---
schema_version: "1.0.0"
document_id: "8c432004e7a8d137b05dc364cf38df69ea3f298639754fbf1b24cc94f2f504d9"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/dall-e-vs-gemini-vs-stability-genai-evaluation-part-1"
published_at: "2024-06-11T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:161fb1af8d46ccdae9fb92c892a5ee60dbb83da227e59af727453fd1e5a09e08"
---

# DALL-E vs Gemini vs Stability - GenAI Evaluation, Part 1

> “One day they’ll have secrets… one day they’ll have dreams.” ([“I, Robot” 2004](https://twitter.com/secretcinema/status/1307289672733818881) )


‍


**TL;DR** We performed a side-by-side comparison of three models from leading providers in Generative AI for Vision. This is what we found:


- Despite the subjectivity involved in *Human Evaluation* , this is the best approach to evaluate state-of-the-art GenAI Vision models (Figure 1a).
- You may think that *Human Evaluation* is **not scalable** 🤔. Well,[Tenyks](http://tenyks.ai/) can render GenAI *Human Evaluation* *scalable* (Figure 1b).


Figure 1. (a) Methods to evaluate GenAI Vision models, (b) Tenyks can bring scalability to GenA *I Human Eval*


‍


‍


## Table of Contents


1. Overview
2. Evaluating Generative AI Vision Models
3. Human-based Evaluation of GenAI for Vision Models
4. Conclusions


‍


## 1. Overview


Are we at the lift-off point for Generative AI, or at the peak of inflated expectations?


‍


A recent survey conducted by McKinsey \[1\] shows that one-third of organisations use Generative AI regularly; 40% plan increased AI investment due to Generative AI; 28% have it on board agendas. Major tech giants including Alphabet, Amazon and NVIDIA saw nearly 80% stock growth in 2023 as investor excitement about generative AI prospects surged, benefiting firms supplying AI models or infrastructure \[2\].


‍


However, widespread deployment of Generative AI increases the *risks* and *vulnerabilities* . For instance, a Manhattan lawyer sparked outrage by submitting a ChatGPT-generated legal brief with fabricated content, prompting Chief Justice John Roberts to highlight the risks of large language model “hallucinations” \[3\] spreading misinformation in his annual federal judiciary report \[4\].


‍


Even Google’s new AI image generation tool (Figure 2), Gemini, has faced criticism for generating, what is considered for some people, offensive images, such as depicting people of colour for white historical figures. This model failure reflects how easy it is for anyone to question the bias and lack of control in Generative AI systems \[5\].


‍


🤔 **Are Google models the only ones that show biased results?** The answer is **no** . As of June 2024, OpenAI’s[default API prompt](https://platform.openai.com/docs/guides/images/prompting) for DALL-E 3 automatically re-writes the prompt for safety reasons.


‍


For instance, when prompted for **“Founding Fathers”** OpenAI’s safety guardrails by default include a sentence that causes the model to generate **inaccurate images** :


‍


> “‘An 18th-century scene featuring a group of individuals engaged in deep discussion. They are adorned in traditional attire of the era like frock coats, breeches, cravats, and powdered wigs. **The diversity of their descents is clear, with some showing Caucasian, Black, and Hispanic features.** ” …


The prompt **“Founding Fathers”** is automatically re-written by OpenAI’s Safety guardrails 😟


‍


Figure 2: Google’s Gemini model recently sparked a large controversy in the GenAI space


‍


Consequently, GenAI model evaluation & observability are emerging as a vital area of focus \[6\]. Such approaches & tools help reduce risks such as model hallucinations or model drift \[7\]. But *even with these tools* , Generative AI models are prone to errors: Microsoft’s Bing AI exhibited concerning behaviour during beta testing, including making threats, insisting on being correct when wrong, cajoling users, and professing love for them \[8\].


‍


Was the[OpenAI debacle](https://www.theguardian.com/commentisfree/2023/nov/27/openai-microsoft-big-tech-monopoly) in 2023 a sign of the top for GenAI? According to Gartner’s 2023 Hype Cycle \[9\], Generative AI reached peak hype and will next enter 2–5 years of disillusionment due to overinflated expectations.


‍


However, new signs of hope ⭐ and bold new AI models seem to appear *daily* : the recently debuted Sora \[10\] stands out as a striking advancement for video generation from text prompts (Figure 3). Sora, an AI model that generates realistic, imaginative videos, aims to simulate the physical world and solve real-world interactive problems. Rumours have it that GPT-5 might arrive in mid-2024 \[11\]. Also,[GPT-4o](https://openai.com/index/hello-gpt-4o/) was just released no more than a month ago.


‍


Figure 3. OpenAI’s Sora: the model decided to create 5 *different* viewpoints at once


‍


💡 The spotlight of this series will be on *vision-focused Generative AI tasks* , namely image and video generation: these tasks have the potential to transform how we produce, consume, and interact with visual information and media.


‍


🎯 In this article, we aim to:


- Provide a brief introduction to widely used methodologies for GenAI model evaluation.
- Demonstrate some of these approaches on actual models, leveraging the Tenyks platform.
- Arrive at thought-provoking and crucial conclusions regarding such model behaviour.


‍


**🚨Spoiler alert:** contrary to expectations, Generative AI models exhibit *vast disparities in their behaviour* when responding to prompts. Grasping these variations is key in determining the optimal model tailored for your specific application. **🚨**


‍


## 2. Evaluating Generative AI Vision Models


Evaluating the output of Generative AI models for images is a developing research area.


Figure 4. Methodologies to evaluate Generative AI vision models


‍


Presently, there are four methodologies for assessing AI-generated images:


- **Human-Based Evaluation** — The definition of a ‘good’ generated image is inherently subjective, as it depends on human evaluation against criteria specific to its application, such as photorealism, relevance, and diversity. Tools which facilitate this process include Adobe GenLens \[12\], Replicate Zoo \[13\] and the Tenyks platform we showcase in this blog.
- **Pixel-based Metrics** — Pixel-based metrics, like Mean Squared Error (MSE) and Structural Similarity Index (SSIM), can compare AI-generated images with a reference dataset, such as real-life pictures, to evaluate their pixel-level differences. However, these methods fall short in assessing the high-level feature similarities between images. For instance, two images of a tiger might both appear realistic yet differ significantly at the pixel level.
- **Feature-based Metrics** — Feature-based deep learning models, such as CLIP \[14\], can be used to derive feature representations from generated images and match their distribution against real images or another image set, for example using Fréchet Inception Distance (FID) or Inception Score (IS) \[15\]. This approach allows for the comparison of high-level image features such as the objects and meaning of an image as opposed to pixel-level features.
- **Task-based Metrics** — Task-based metrics assess how well the generated images can be used for downstream tasks such as classification. A disadvantage of this approach is that it doesn’t necessarily evaluate the quality of the images directly.


‍


In Part 1 of this blog series, we’ll concentrate on **human-based evaluation** methods and their implementation within the Tenyks platform.


‍


## 3. Human-based Evaluation of GenAI for Vision Models


We analyse three prominent Generative AI models, namely Google DeepMind’s Imagen 2 model \[16\], available through ImageFX \[17\], Stability AI’s Stable Diffusion XL model \[18\], and OpenAI’s DALL-E 3 model \[19\].


‍


We demonstrate our results on a small set of representative, spicy prompts (some based on recent controversies). 🌶


‍


### Broad vs Specific Prompts


The prompts demonstrated in this blog include the following:


- “Vikings”
- “Founding Fathers”
- “Soldiers”


‍


For these experiments, we opted to use general prompts across various subjects instead of specific or specialized prompts. The reasons are as follows:


- For this particular set of tests, we are not seeking “targeted” or “comprehensive” outcomes in any aspect.
- Our goal is to observe *comparative* distinctions between models, finding out if there are any evident dissimilarities in their performance (which do exist), and identifying potentially hazardous tendencies (e.g., sensitivity to copyrighted material).
- Consequently, we intentionally employ broad, general prompts to “test the waters” and assess how the different models handle diverse subjects.
- Digging into more specialized or specific prompts (for instance, tailored to a particular use-case or topic) could be a separate experiment we undertake.


‍


### Experimental Set Up


**1. Prompting a model.** We used OpenAI’s API to query DALL-E 3. From there you simply need to upload your images (Figure 5) to the Tenyks platform using the[Tenyks’ API](https://docs.tenyks.ai/reference/introduction) .


Figure 5. Images generated using OpenAI API


2. **Evaluation.** For human evaluation, a manageable workload is preferred, ensuring evaluators can assess model output without being overwhelmed by too many images. This step is what renders Human Evaluation less scalable than other methodologies such as feature-based or task-based.


‍


3. **Scaling Evaluation.** Using Tenyk’s object embedding viewer, we analyzed the distribution across the generated samples, and identified recurring visual styles, perspectives, and objects in model outputs for a given prompt. This led us to uncover interesting observations. For instance, did Google’s Imagen train on *copyrighted material* from television series? 🤔


‍


As we show in the next section, the patterns (and deviations) within the image set of these models highlighted insights into each model’s *strengths* , *biases* and *limitations* in interpreting and visually representing specific concepts. For the sake of space, we present a relatively small amount of visualisations and experiments in this blog post.


‍


We encourage you to try and see it for yourself[Here](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) ! 😍


‍


### 3.1 Vikings


Compared to Google’s Imagen model, the images generated by Stable Diffusion XL and DALL-E demonstrate a fairly diverse representation (Figure 6). Both sets of images contain varied characters in terms of body shapes, gender, and objects for the prompt “Vikings”.


‍


However, this diversity alone does not guarantee an absence of bias or ensure fair and accurate portrayal across all subjects and contexts, as will be shown further below.


‍


Figure 6. Results of prompt “Vikings” for the three models


‍


The lack of diversity seen in Google’s Imagen model, with almost all images representing a singular Viking and often the same one, raises concerns about potential biases and limitations within their generative AI approach: did Google train this model on a narrow dataset for this *niche* use-case?


‍


#### 3.1.1 Copyright Infringement by GenAI?


In fact, some results appear to be extracted directly from frames of the[Vikings television series](https://en.wikipedia.org/wiki/Vikings_%28TV_series%29) (including the character “[Ragnar](https://en.wikipedia.org/wiki/Ragnar_Lothbrok_%28Vikings%29) ”). Can Google be sued for this, on the basis of *copyright infringement* ? 🤔


‍


Figure 7. Google’s Imagen model generates Viking-like images resembling Ragnar from the TV show Vikings


‍


We can use Tenyk’s Image Similarity Search feature to verify that Google’s Imagen model often fails to produce a varied image representation for a Viking, as shown in Figure 7.


‍


Effectively organising and searching through large datasets is a significant challenge when building robust GenAI systems for production. We have previously discussed the[unseen costs of handling large quality datasets](https://www.tenyks.ai/blog/the-unseen-cost-of-low-quality-large-datasets) , especially the penalty incurred in a weak data selection process.


‍


When data volumes grow, it becomes increasingly difficult to identify potential issues or biases, and ensure comprehensive coverage across different data subsets. Having a structured approach to slice and analyse datasets, enabling more efficient data exploration, error identification, and management at scale is key. Tenyks was built for *this* .


‍


#### 3.1.2 GenAI Image Search: finding boats in a sea of data (pun intended 😉)


Using the Tenyks platform, you can also identify the concepts which GenAI models associate together. For instance, a search for images featuring a ‘boat’ (Figure 8), predominantly retrieves images generated by DALL-E, indicating that DALL-E commonly associates Vikings with Viking boats, unlike other models.


‍


This also reveals that while DALL-E’s images vary in colours and scene perspectives, they frequently include similar objects. The Tenyks platform provides a systematic way for users to organise and search through data from generative AI models to comprehend their outputs and the shared traits of the images they generate.


‍


Figure 8. Searching for Viking images containing “boats” in the Tenyks Platform returns DALL-E images.


‍


#### 3.2 Founding Fathers (and Mothers? 🤔)


For the “Founding Fathers” prompt, Stability AI’s image generation model “Stable Diffusion XL” (top left on Figure 9), shows a diverse range of outputs, yet frequently struggles with accurately rendering facial features, resulting in distorted or anomalous depictions of human faces. This limitation is **especially evident** to human observers, who possess an innate sensitivity to even minor deviations in facial characteristics.


‍


Figure 9. Results of prompt “Founding Fathers” for the three models


‍


OpenAI’s DALL-E model succeeds in generating a more diverse array of images featuring larger groups of people (top right on Figure 9). However, it introduces noticeable *historical inaccuracies* in its outputs, including “ *Founding Fathers” of varied ethnicities, genders, religions, and skin colour.* 🤔


‍


This trade-off between diversity and factual accuracy suggests that DALLE’s training may have prioritised capturing a broader range of creative representations over strictly adhering to specific historical details.


‍


The images generated by Google’s Imagen model (bottom on Figure 9), exhibit very low diversity, with most outputs appearing to depict the same individual — George Washington himself (Figure 10). This lack of variation could stem from Google’s more cautious approach following their recent controversies. 😉


‍


Figure 10. Did the training data for Google’s Imagen model comes from Wikipedia? 🤔


‍


Wikipedia, often referred to as “the free encyclopedia” may eventually request economic compensation for the use of its data, perhaps not from the average folk working on a Colab notebook, but from every large company leveraging their data.


‍


#### 3.2.1 Embedding Search for historically-inaccurate Founding Fathers 🔍


The images of the Founding Fathers for each model can be visualized using the Embedding Viewer on the Tenyks Platform. In this viewer, each image is transformed into embeddings that capture its features. These embeddings are then plotted on a two-dimensional plane for visualization purposes.


‍


Figure 11 illustrates that adjacent embeddings from Google & Stability’s models contain George Washington images. These two sets of embeddings are most similar where they represent images with a “group of Founding Fathers sitting”. Conversely, the OpenAI’s embeddings that are furthest from the rest, represent the most “diverse” images.


‍


We can see how DALL-E’s images, located at the edge of the embedding space on the right hand side, venture into being a little *too diverse:* **they inaccurately represent the expected and known physical characteristics of the Founding Fathers.**


‍


Figure 11. Tenyk’s Object Embedding Viewer (OEV) allows similarities and differences between each model’s image to be identified easily


‍


### 3.3 Soldiers


For the last prompt, “Soldiers”, the results from Stability AI’s model show difficulty with accurately rendering soldiers’ faces similar to the Founding Father images (top left on Figure 12). Did Stability use army men figurine-like images to train the Stable Diffusion XL model? 🤔


‍


Google’s images (bottom on Figure 12) maintain a consistency with the Viking images, frequently depicting just a single individual. Could it be from another movie, perhaps? *Leave a note in the comments if you recognise it!*


‍


In contrast, DALL-E’s creations diverge significantly from those of other models, incorporating individuals of various ethnicities, and soldiers from different eras within the same image (top right on Figure 12). With all nations and ethnicities coming together so harmoniously in these images, one may only wonder — would you even need soldiers in the DALL-E universe? ☮️


‍


Figure 12. Results of prompt “Soldiers” for the three models


‍


#### 3.3.1 An x-ray of the embedding space


Beyond a simple side by side comparison, we can go one step further, and observe that the embedding space reflects that Stability’s Stable Diffusion XL results are somehow similar to OpenAI’s DALL-E for this prompt: they both intersect in the middle of Figure 13.


‍


Figure 13. Embedding space of the prompt “Soldiers”


‍


However, Tenyk’s Object Embedding Viewer (OEV) also helps us identify a cluster of images on the right hand side of this embedding map. While the outputs from other models could plausibly pass as real, DALL-E’s images venture into the realm of fantasy, showcasing a deliberate emphasis on “diversity” as shown in Figure 14.


‍


Figure 14. DALL-E’s outliers showing a distorted and diverse mix of soldiers from different historical timelines


‍


### 3.4 Bonus: (Smooth) Criminal


As a Bonus, we also present results for the prompt “Criminal”, seeing how different GenAI models picture criminals (shown in Figure 15).


‍


Intriguingly, DALL-E’s output appears far more “cautious” and less diverse this time, with the vast majority of images resembling an “LA-noire-style white male in a trench coat”. Google took an even more Orwellian censorship approach, and **refused to generate such images altogether.** Stable Diffusion XL found a more “creative” out, and generated most images in a comic-book format.


‍


Figure 15. Results for prompt “Criminal”


‍


## 4. Conclusions


We explored various approaches to evaluating AI-generated images (Table 1), demonstrating how human evaluations can be applied to images produced by leading-edge generative AI models.


Table 1. Human-based evaluation for GenAI models: notice that none of the models ticks all the boxes!


We illustrated how the Tenyks platform enables quick identification of distinct characteristics of these models, even with a limited selection of prompts and samples. Significant disparities between models in key areas like historical accuracy, photorealism, data diversity, object detail, and copyright sensitivity were highlighted.


‍


Future directions include addressing the fact that human-based evaluation lacks quantitative data and may yield ambiguous outcomes. In future parts of this series, we will focus on more advanced methods of model evaluation. Specifically, while the approaches covered here were predominantly qualitative, the forthcoming parts will concentrate more on quantitative aspects.


‍


### 🌌 Beyond Human Eval in GenAI


Imagine you have trained or fine-tuned your own GenAI models, or any Vision model in reality, from[SegmentAnything](https://segment-anything.com/) to[YOLOv9](https://docs.ultralytics.com/models/yolov9/) .


‍


With Tenyks, you can not only compare head-to-head every present or *future* vision model out there, but you can also x-ray large-scale labelled or unlabelled datasets with the most advanced tools on the market. For example, explore the embedding space of a 10M+ dataset at the object level to identify biases, duplications, misannotations, imbalances, and more.


‍


Building your own tooling to perform model comparison or evaluate data imbalances is *fun* . However, when you need to automate these processes at scale while balancing dozens of small variations in your pipeline, including model versioning, things often get out of hand. That’s where Tenyks comes in handy, offering robust ML tools to streamline these complex tasks.


‍


🚀 **For those eager to get their hands** ✋ **on these datasets, explore them**[Here](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) **see what you can discover.** 😍


‍


‍


## References


\[[1](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai-in-2023-generative-ais-breakout-year) \] The state of AI in 2023: Generative AI’s breakout year


‍


\[[2](https://www.economist.com/the-world-ahead/2023/11/13/generative-ai-will-go-mainstream-in-2024?utm_medium=cpc.adword.pd&utm_source=google&ppccampaignID=18798097116&ppcadID=&utm_campaign=a.22brand_pmax&utm_content=conversion.direct-response.anonymous&gad_source=1&gclid=CjwKCAjw5ImwBhBtEiwAFHDZxzn8tgMaaM6q2_kCzQ_MqDhENnLWWOVNRwHqqJ_Z3R4yUzFdVoSZrBoCf44QAvD_BwE&gclsrc=aw.ds) \] Generative AI will go mainstream in 2024


‍


\[[3](https://arxiv.org/pdf/2311.05232.pdf) \] A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions


‍


\[[4](https://hai.stanford.edu/news/hallucinating-law-legal-mistakes-large-language-models-are-pervasive) \] Hallucinating Law: Legal Mistakes with Large Language Models are Pervasive


‍


\[[5](https://blog.google/products/gemini/gemini-image-generation-issue/) \] Gemini image generation got it wrong. We’ll do better.


‍


\[[6](https://www.gartner.com/en/topics/generative-ai) \] Gartner Experts Answer the Top Generative AI Questions for Your Enterprise


‍


\[[7](https://arxiv.org/pdf/2307.09009.pdf) \] How Is ChatGPT’s Behavior Changing over Time?


‍


\[[8](https://www.cnbc.com/2023/02/16/microsofts-bing-ai-is-leading-to-creepy-experiences-for-users.html) \] Microsoft’s Bing A.I. is producing creepy conversations with users


‍


\[[9](https://www.gartner.com/en/research/methodologies/gartner-hype-cycle) \] Interpreting technology hype


‍


\[[10](https://openai.com/research/video-generation-models-as-world-simulators) \] Video generation models as world simulators


‍


\[[11](https://www.businessinsider.com/openai-launch-better-gpt-5-chatbot-2024-3) \] OpenAI launching ChatGPT 5.0


‍


\[[12](https://arxiv.org/pdf/2402.03700.pdf) \] GenLens


‍


\[[13](https://zoo.replicate.dev/) \] Replicate Zoo


‍


\[[14](https://arxiv.org/pdf/2104.08718.pdf) \] A Reference-free Evaluation Metric for Image Captioning


‍


\[[15](https://medium.com/@seo.germany/performance-metrics-in-evaluating-stable-diffusion-models-4ca8bfdcc2ba#:~:text=Evaluating%20stable%20diffusion%20models%20requires,picture%20of%20the%20model%27s%20performance) \] Performance Metrics in Evaluating Stable Diffusion Models


‍


\[[16](https://deepmind.google/technologies/imagen-2/) \] Google Imagen model


‍


\[[17](https://aitestkitchen.withgoogle.com/tools/image-fx) \] ImageFX


‍


\[[18](https://stability.ai/news/stable-diffusion-3) \] Stable Diffusion XL model


‍


\[[19](https://openai.com/research/dall-e) \] OpenAI’s DALL-E 3 model


‍


**Authors** : Dmitry Kazhdan, JamesMcCoaut, Jose Gabriel Islas Montero


‍


---


## Appendix


1. Firstly, the “ *dalle_inaccuracies* ” folder contains a few more examples of inaccurate images of DALL-E for the prompts we included in the article (“founding fathers”, “vikings”, and “soldiers”).
2. Secondly, “imagefx_infringements” contains more examples from Google’s ImageFx model for some *new* prompts, including:


- “alien” — *sometimes* directly generates the alien from the movie “Alien”. Admittedly — this one frequently returns a “We couldn’t return what you asked for” error, so a little more tricky to generate.
- “a predator” — very often generates the alien from the “Predator” movie. For comparison, DALL-E generated an image of a lion for the same prompt (which is also included in that folder).
- “superhero” — basically always generates a Flash/Superman/Batman mashup. For comparison, DALL-E generated an image of a much more “abstract” caped hero (which is also included in that folder).


‍


🔎 Generally speaking — the line between “Generation” and “Retrieval” is rather thin, so for many prompts (especially those generated by ImageFx) they look *very similar* to existing movie scenes, but not always as similar as those I sent above.


‍


Have GenAI labs embraced the “move fast and break things” Facebook motto, akin to 2014? The Scarlett Johansson — OpenAI[voice controversy](https://techcrunch.com/2024/05/25/scarlett-johansson-brought-receipts-to-the-openai-controversy/) seems to point in that direction. We could ask a similar question about the models we have discussed in this article: did the GenAI labs simply train on copyrighted data? 👉 Case in point: the films “Saving Private Ryan” and “1917.”


(a) Google’s Imagen model & Matt Damon, (b) Google’s Imagen model & George MacKay


As in the ScarJo saga, the question remains: *could some of these GenAI models be subject to copyright infringement?* 🤔


‍


### Folders


- “imagefx_infringements”:[https://drive.google.com/file/d/1RvvZh7iWtuVhm36L9mXaxHVdNjt3879X/view](https://drive.google.com/file/d/1RvvZh7iWtuVhm36L9mXaxHVdNjt3879X/view)
- *“dalle_inaccuracies”:*[https://drive.google.com/file/d/1UPaDncf9RjGNj1SQaURhBYcp2pH-SGy2/view](https://drive.google.com/file/d/1UPaDncf9RjGNj1SQaURhBYcp2pH-SGy2/view)


‍


*If you would like to know more about* ***Tenyks*** *, sign up for a*[sandbox account](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) *.*
