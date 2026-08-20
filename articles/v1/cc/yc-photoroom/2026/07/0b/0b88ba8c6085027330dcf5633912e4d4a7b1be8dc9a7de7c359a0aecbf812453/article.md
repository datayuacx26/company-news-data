---
schema_version: "1.0.0"
document_id: "0b88ba8c6085027330dcf5633912e4d4a7b1be8dc9a7de7c359a0aecbf812453"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/photoroom-foundation-diffusion-model"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:ecc0cfd9e73fe755aa334517268a6366d19bca8782d354761975ea5968fe9144"
---

# Photoroom foundation diffusion model: why, how, and where do we go from there?

*Walk down memory lane with our machine learning team as we explore how diffusion became a big deal at Photoroom, recent changes to our product, and what the future holds.*


Note to our more tech-savvy readers: some more details towards the end.


## Photoroom and diffusion models, the humble beginning


Ever since Stable Diffusion 1.4 took the world by storm in the summer of 2022, hot on the heels of the[Latent Diffusion](https://arxiv.org/abs/2112.10752) paper (Rombach et al.), it has been obvious that the visual content creation space would never be the same. At Photoroom, we took this as a call to action and started exploring latent diffusion models focusing on Stable Diffusion. Like many, the Photoroom machine learning team is a private ML lab, but we have a strong connection to our users' needs, so we walked backward from there.


Our users were already used to AI magically removing image backgrounds, so we, little by little, ironed out the idea that we should also help them create new ones. From a distance, the reasoning is as follows: people take billions of pictures daily because they have something to show, something to sell, or something to remember. But just because an image was taken doesn't mean that all the pixels are equally important; some are often there by accident, and our users have a good idea of what they want to see instead. Helping people craft the visual content they want perfectly aligns with Photoroom's DNA.


## AI Backgrounds part 1: Magic Studio


Of course, we were not the only ones who followed this path, and we spent the following months investigating many options and connecting elements to define the state of the art.


In November 2022, we proudly unveiled 'Magic Studio ', a novel take on one-shot[Dreambooth](https://arxiv.org/abs/2208.12242) and masked loss. This was a new concept at the time, but it leveraged our existing segmentation expertise. You can learn more about it from our friends at[The Verge](https://www.theverge.com/2022/11/8/23447102/photoroom-app-magic-studio-ai-image-tool-generation) , or[straight from Matt, our CEO](https://twitter.com/matthieurouif/status/1589539286814461953?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1589539286814461953%7Ctwgr%5E2c24f26e25a85a773a6f7ba9044fccd31d1d7876%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.theverge.com%2F2022%2F11%2F8%2F23447102%2Fphotoroom-app-magic-studio-ai-image-tool-generation) .


Developing Magic Studio was not a task we took lightly. It was not something we picked off the shelf or found in literature—it required significant research and development efforts. This was far from our first attempt, but it was the first feature we believed was ready to be shipped.


As hinted by this last comment from The Verge's[Jess Weatherbed](https://www.theverge.com/authors/jess-weatherbed) , there were a few issues though, and this proved prescient:


-


it was slow by consumer standards (around a minute per call, enough to feel a disconnection with the app)


-


the user content was not perfectly maintained


-


the underlying model was not consistently producing aesthetic enough results


## AI Backgrounds part 2: Instant Backgrounds


### A custom model to really solve this


Magic Studio flopped hard; we never got more than a couple thousands daily users on it. It was tough to ship this, as models would become custom on a per-user basis, the backend becomes a lot more complicated. It was relatively slow even if we optimized it a lot. It didn't preserve all the user pixels, which we discovered meant a hard pass from many of them. But at Photoroom, our culture is to learn by doing and shipping. We believe talk is cheap and putting things in front of the users is the only way to cast a light on the blind spots. So that was a hard lesson, but it bore fruits.


We already had a parallel track ongoing in the ML team, trying to get a custom model to invent a matching background at runtime, which would solve the aforementioned issues in one go. It meant that we had time to hone in on its behavior, asynchronous to the users, train it to perfectly respect the contours and lighting. Being purely inference it would be much faster, and be a better fit for most of our users who access Photoroom from a phone. Nothing off the shelf could really help us achieve this, but as the team's expertise around diffusion improved over the months, we got to a point where we knew how to formulate a proper model and guidance and could build up the appropriate dataset.


### The core task, outpainting, looks like inpainting but is a different beast


The results from our AI Backgrounds looks close to the inpainting task. In a way, it's related, but the job from the model’s perspective is quite different:


-


**The model cannot extend anything from within the mask.** Mask boundaries are rigid, while in the inpainting task, the model can grow the preserved content inside the mask


-


There's **a lot less content to anchor the creative process** ; everything from the lighting to the perspective has to come from the (usually small) preserved area


In practice the differences look like this:


***Inputs***


-


*Inpainting*


example inpainting from the RunwayML Stable Diffusion 1.5 release


Something to remark in the above is that ***the model is allowed to grow the background into the mask to solve the task* .**


-


*Outpainting*


Photoroom “Instant Background”, early 2023


What's remarkable about the above is that beyond the inverted completion (we preserved the dog, not the bench), the model cannot expand the preserved content by a single pixel; it has to invent a background that perfectly complements the main subject of the image. The grounding in terms of lighting and perspective is also very limited, everything has to be inferred from the dog posture and appearance.


### A foundation of sorts


This model shipped to wide availability in February 2023, as one of the backbone elements behind our AI Backgrounds feature (formerly Instant Backgrounds), and we built up from there during the year. We iterated on the model training and on the flow of the app, so that the users could quickly get a nice personalized result. We also trained other models to learn from what users liked, to handle multiple modalities (including allowing users to use image prompts), and grow NLP expertise within the team.


Speed was another area we prioritized for many reasons. Increasing speed made it possible to absorb the model's growth, as GPUs were a scarce resource that year. This also served the user experience, as we define "instant" as less than a second, and feel that's necessary when used in a mobile app where users interact with the content in real time. Finally, this allowed us to minimize the carbon footprint for this new feature, whose usage was growing fast.


Finally, our newfound diffusion expertise had other applications. The link is complex, but we invented a new diffusion-backed way to generate volumetric shadows, levelling up our modeling knowledge and dataset crafting skills.


Photoroom volumetric shadows


We generated over 5B images during the year and grew our user base by some large multiplier, but that was only the beginning.


## AI Backgrounds part 3: Training a new foundation model


### Starting point


What could we improve? What was the path moving forward? Well, there were a few salient points:


-


**Our generative AI offering was already very successful and here to stay.** This has justified the long-term investment from the ML team and Photoroom in general.


-


We had **tight latency constraints** . Nobody sells a model, really. What users are interested in is a feature, and this encompasses the model(s), the UI for using them, and the latency or exit surface.


-


**The model quality was excellent for mobile but not enough for wide screens.** Ideally, we would improve on it while keeping the latency under wraps, but the state of the art was moving towards better but slower models.


-


We were **building a big part of the Photoroom IP on a third-party model,** **which felt risky over the long run.** Access conditions could change, and companies don't all last forever. We had no idea what data was used for pre-training, and that impacted our B2B prospects. All this was pointing to a sandcastle.


-


The **state of the art in the field was not completely aligned with Photoroom** . We knew what our users wanted and were best placed to deliver on that. Repurposing a model made with some other intents in mind would only go so far; a lot of focus was on text-to-image models, while many of our users want to preserve some of their pixels and edit their pictures. Model strengths are not necessarily the same; photo editing requires excellent reading of the preserved content and basing the lighting and perspective from there, while text2img creates content out of nothingness.


It was clear that our future success lied in owning more of this space within Photoroom. By training our own model, aligned with our unique needs, and growing all the required expertise, we could steer our course toward a more innovative and competitive future. Leap of faith, here we went !


### Training a new model


How do you train a diffusion model from scratch? What kind of data do you need, how much of it, what's the expected quality density, and what are the compute needs, for real? There are a lot of papers and available materials, but not everything is there, and acquiring knowledge on the matter is quite the journey.


**First topics are around ballparks** : what kind of model architecture and model size, what kind of data and volumes, and how will you train. The topics are tied, of course (a bigger model will typically require more data to converge), and you have to shoot for an expected latency, so the problem space can become quite constrained. We have not published any paper on the topic, but we discovered over time that we took very similar approaches when compared to[PixArt-α](https://arxiv.org/abs/2310.00426) or[Stable Diffusion v3](https://arxiv.org/pdf/2403.03206.pdf) .


-


We decided early for a Transformer based architecture working in a latent space, similar to[DiT](https://arxiv.org/abs/2212.09748) (Peebles, Xie). Transformers are very aligned with GPU architectures, being a natural good match to their bandwidth-per-compute limitations. DiT was not exactly what we needed (class only conditioning, not multimodal), but it was a strong starting point, and we could add or change parts to fix the missing features.


-


We’ve a lot of internal tooling to optimize our models for inference. We serve models on our own backend, and this helped us decide for an appropriate model size. We targeted a still-instant (sub-second from a user's perspective) latency, which meant around a billion parameters. We factored in diffusion models distillation (like[LCM](https://arxiv.org/abs/2310.04378) ), which turned out to be a good move. This in turn gave us some expectations on the volume of data required, in the 10M to 100M range, informed by the literature.


-


We built a stack to process data at scale. This implies some expected filtering, such as removing NSFW and custom filters, similar to the[Data Filtering Networks](https://arxiv.org/abs/2309.17425) paper (Fang et al.). A couple of existing frameworks for that came out after we started, such[as Fondant](https://fondant.ai/en/latest/) and[Nemo-Curator](https://github.com/NVIDIA/NeMo-Curator?tab=readme-ov-file) . We finally used[Mosaic Streaming Dataset](https://docs.mosaicml.com/projects/streaming/en/stable/#) to store our training sets, optimized for throughput at scale.


-


Compute for training would come from a public cloud, and given an optimized stack this was all manageable. We used[Mosaic Composer](https://docs.mosaicml.com/projects/composer/en/stable/index.html) for this, and it prove a great ally.


### Early results and learnings


We trained a model very early based on the above premises, completing an internal demo in July 2023. There were issues, but it was also promising: we could train something like this from scratch.


We called it v1 and started to consider the missing parts. What would it take to completely replace the current production offering?


-


We needed to plug all the holes; some concepts were not covered well enough.


-


We needed to make sure that this model was flexible enough to cover the actual features the users were interested in. Remember, users don't care about the model; they want to get something done, and we missed some of that in this initial offering.


Some samples from an earlier training run


### Photoroom Instant Diffusion


We worked on a v2 of the internal model, trained from scratch and exploring a bit more of the problem space, which taught us how to finally make it flexible enough. We then wrapped things up in a v3, which shipped at the beginning of March 2024.


It is still a Transformer-based model which works in a latent space, but it was architected and trained from the beginning to be good at image edition. This is quite profound and not something we could have done so easily with an off-the-shelf model; we didn't only train with the relatively typical {text, image} pairs but added image understanding and autocompletion via a masking task (reminiscent of Bert or MAE). The model builds a precise knowledge of how image parts are related or can be altered. We're also still instant, which is important for mobile and human-in-the-loop, while the quality went up by a notch –– here's some feedback from our internal pre-release testing: "It's as if I put my glasses on."


### Some learnings


For the ML team at Photoroom, this project meant growing a lot in terms of scope, learning how to handle big datasets, how to augment them, train faster by pre-processing anything static in the compute graph, master a lot more of the model architectural choices and own it when we get it wrong. It's different from re-training an existing model, pros and cons, there's an initial price to pay but we’re well equipped for what's to come.


Writing this blog post with some hindsight, something else we learnt was the importance of knowing our users usage and data better. We’re still adjusting the model for our users, aligning it, improving on some of the app flows that were implicitly tailored to the previous model. There was a discrepancy in between our internal results and what our users saw upon release, and it took us too long to realize that. As often, data and metrics are king, even if the AI world more easily talks about models and fancy training techniques.


### Where do we go from there ?


Are we done? Not quite, it's still the beginning.


It took us some time to train these models, and we've improved over time. One salient point for the future is that we’re working on the tooling and data stack to make iterations easier. A lot of this is not available off the shelf and we’re considering open sourcing some of the stack. We’re basing the core data management on a vector DB, iterating on filtering models and data distribution metrics.


This model is not perfect, it's getting better with user alignment but the state of the art doesn't wait. Diffusion is here to stay and there are better architectural choices waiting to be made. We’ll keep training and iterating, **goal being that at the end of 2024 we’re still instant, but indistinguishable from photography in most cases** . Our model currently lacks the capacity to render the most complex scenes accurately, but that's something we're working to **improve. Simpler scenes like the above are already essentially indistinguishable from a real studio shot, except that users are free to capture them in any place.


Lastly, we are improving in how we derive useful features from this base model, including how we adjust it to users’ expectations and inputs. Precise and long prompts are an expertise, knowing how to combine different manual steps to edit an image is another one, our strength is in using more AI to make it seamless and lower the expertise bar in between the vision and the results. This is a journey, we’re not quite done.


## Some practical outcomes, and why this is foundational


The following are raw outcomes from the app, or from engineering demos, nothing has been retouched. Inference time is typically << 1s. Why is this model foundational ?


### AI Backgrounds


A new outpainting core capacity, as planned. This is pixel perfect up to the objects very boundaries, the model has more capacity than SD1.5 or SD2 and it shows on complex scenes or fine textures. Inference time is very fast, as you can see for yourself in the app, the model correctly reads the light and handle the transparent and reflective parts of the image.


Original Picture from[No Revisions](https://unsplash.com/@norevisions) , reinterpreted with the Photoroom foundation model. At a glance, could you see which was which ?


### AI Expand


A model trained on image understanding and completion means that some tasks become a lot easier, for instance that of extrapolating parts of an image. This is something we call “AI Expand” in the app, and it makes it possible to change your framing a posteriori.


Picture by[Bundo Kim](https://unsplash.com/@bundo) on[Unsplash](https://unsplash.com/photos/a-large-white-boat-floating-on-top-of-a-body-of-water-VRbd6ss_LJI)


### AI Fill


This is the name we use for *inpainting* , and it's one more feature which is naturally covered by this foundation model. In practice, a user can swipe over a part of the image that they want changed, and tell us what they would like in that place.


This is a fun and creative use case, something which would have required a lot of time and expertise a year or two ago.


### AI Erase


Complementary to AI Fill, this feature makes it possible to *erase* something from an image, and to replace it with a non-salient filler. This is a classic from the Phorotoom app, and our new version -shipping soon, Q2 2024- is visibly better at handling complex lighting and objects.


### Upscaling


After you’ve made a great photo, some users might want a higher resolution version, and this is yet another feature that we can use our foundational model for, keeping a fine grained aesthetic. This should ship Q2 2024.


## The team


If you've made it so far, can you guess how many people are in the Photoroom ML team?


We're seven at the moment, with two more people joining soon. That's not a lot in the grand scheme of things, considering we're doing more than "just" diffusion-based model training. This covers all data, model definition and training, optimizing for production, and owning a backend across many use cases (not just the base model training but also segmentation, scene recommendation, or image edition). We’re hiring for senior/staff roles !


Photoroom has a lean and agile team of 60+ employees working across Europe. We're always on the lookout for new talent - check out[our careers page](https://www.photoroom.com/company#careers) for our latest open roles.


## Some technical bits


Following[a Twitter poll](https://twitter.com/BenTheEgg/status/1780655396417315242) , here are some quick somewhat technical facts:


-


*“Size of the dataset and how you curated it”*


-


Dataset is a mix of various sources, including acquired images. We considered around 1B images, but trained on 90M in the end based on NSFW, initial Clip alignment, and a in-house graphics detector. Some learnings: clip alignment was useless, since we re-captioned almost everything using[CogVLM](https://github.com/THUDM/CogVLM) or[Llava](https://github.com/haotian-liu/LLaVA) . Curation could have been better, for instance we used an open source NSFW detector which was not precise enough.


-


Data processing was done in a streaming fashion, from MosaicDataStreaming to MosaicDataStreaming, all cloud hosted ([Fondant](https://fondant.ai/en/latest/) is using a similar architecture). We’re moving on from there to what we think is a better data stack, VectorDB based, but this one sustained the test of scale and is worth mentioning still. We used a light data processing framework which is GPU aware (non blocking CPU-GPU and GPU-CPU transfers for instance), this could change also in the future towards something like[Dask](https://www.dask.org/) or[Ray](https://www.anyscale.com/ray-open-source) . In the end, for a single training run you would probably spend more on GPUs for data processing than for the actual training, for a sub 1B model. Data processing is an investment you can reuse for the next model though, but be prepared.


-


*“Your optimization journey for the lowest latency” / “Inference specific optimization”*


-


We were early on the diffusion optimization wagon, see for instance[this blog post](https://www.photoroom.com/inside-photoroom/stable-diffusion-25-percent-faster-and-save-seconds) or[this one](https://www.photoroom.com/inside-photoroom/stable-diffusion-100-percent-faster-with-memory-efficient-attention) . This is a journey, there's no single silver bullet, and we disclosed[some of our most recent optimizations](https://static.rainfocus.com/nvidia/gtcs24/sess/1697632307620001oFfV/FinalPresPDF/S62726%20-%20Scaling%20Generative%20AI%20Features%20to%20Millions%20of%20Users%20Thanks%20to%20Inference%20Pipeline%20Optimizations_1709061966481001SCtn.pdf) in the 2024 nvidia GTC summit (summary curve here). Something which pays off is having a code base which covers the whole spectrum from research and training to inference, the same model which was trained the day before in pure Pytorch form gets “compiled” into TensorRT and its runtime is dissected. We've experience checking for the complete TensorRT support on a per-op basis, and dive into the ONNX graph if need be.


-


*“Compare your MFU to Keras”*


-


We didn't actually measure our model flop utilization (MFU means “Model Flops Utilization”, flops made good if you pardon the sailing analogy (VMG -Velocity Made Good-). Keras is probably a great deep learning library, we happen to use PyTorch, we don't really have time nor incentive to compare with every library under the sun. What we can report on is power usage, why this is a great proxy, and a throughput metric.


-


GPUs consume power when idling, but that's comparably very little, the real consumption happens when the GPUs are doing actual work, due to transistors having to be filled up and emptied at nearly every clock cycles. This represents electrons flowing in and out, and in turn consumed power. GPUs communicating with each other also consume power, but the transistors involved represent a small fraction of the whole SoC. Thus monitoring GPU power usage is a great proxy to monitor actual work being done, almost all training bottlenecks will show here.


-


Is that related to HFU or MFU ? Technically this is HFU, but if you’re not doing activation checkpointing and are using a big enough batch size this would be very correlated to MFU.


-


In this case our GPUs are reliably around 80% peak consumption, meaning there's some margin to use them a little better, but not a 2x factor. Note that there are some compute graph optimization options which are opened by some training stacks (operator fusion, …), but they don't typically *remove* computations, they make them more packed over time. Some training to inference optimizations do remove some computations, for instance folding batch norm into prior conv layers, and fused-multiply-add do exist, but that's not enough to remove this HFU/power use correlation.


Training on a public cloud, relative power consumption over 16 A100 nodes.


-


A number we can share is that the peak throughput at training time was slightly above 10k images per second (including forward, backward, and optimizer step).


-


As mentioned above, our whole training stack was[Mosaic Composer](https://docs.mosaicml.com/projects/composer/en/stable/index.html) (on top of Pytorch 2.x), digesting[Mosaic Streaming Dataset](https://docs.mosaicml.com/projects/streaming/en/stable/index.html) shards.
