---
schema_version: "1.0.0"
document_id: "2ac29202488f7e4bd855079494f55602144ab8c869a8c686fbc97fdd8f28db3f"
company_key: "sonos-inc-common-stock"
company: "Sonos Inc."
source_id: "sonos-inc-common-stock-rss-b40e25ea1a3a"
canonical_url: "https://tech-blog.sonos.com/posts/arc-ultra-speech-enhancement-announcing-a-step-change-in-speech-enhancement-using-ai/"
published_at: "2025-05-11T07:04:00+00:00"
first_seen_at: "2026-07-20T23:18:24.938468+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:10076ad0d005226664bc85f1f39c16db5b3c0e63850d7b84e374636267f4ca87"
---

# Arc Ultra Speech Enhancement: Announcing A Step Change in Speech Enhancement Using AI

From an early age I’ve struggled with my hearing from time to time. As a musician and audiophile, admittedly some of this is self-inflicted, but whatever the cause, I’ve become acutely aware of how much hearing loss can impact enjoyment of multimedia content. I’ve also seen how difficult speech has become for older members of my family — and existing speech enhancement features on their TVs and soundbars just didn’t seem to cut it. So, when I had an opportunity to help improve the sound experience for people who struggle with speech content in multimedia, I jumped at the chance.


Almost everyone has struggled with speech in film and TV content at some point. Maybe there’s some loud DIY going on next door, or it’s summer and you’ve got the air conditioning blasting to keep the house cool. It may simply be that you’re not a native speaker, and the enhancement helps to make pronunciation clearer. Or perhaps, like many people, you live with some form of hearing loss, and you find understanding speech in multimedia content hard work. At Sonos, we want to ensure that whatever your reason for needing it, you have the best possible speech enhancement at your fingertips.


Our latest offering does this with the help of Artificial Intelligence, or AI. With AI technologies like ChatGPT being used for everything[from understanding complex concepts to writing code](https://www.forbes.com/sites/bernardmarr/2023/03/01/the-best-examples-of-what-you-can-do-with-chatgpt/) , the topic of AI has never been more popular (or more controversial!). While Large Language Models (LLMs) and other powerful AI technologies are achieving impressive feats in generative modeling (that is: using machine learning models to generate new data), there are other ways in which AI is helping to improve technology.


In this blog post, we’ll introduce the technology behind Sonos’ new Speech Enhancement feature: machine learning models which sit on your soundbar and help to make dialogue crystal clear. To do so, we’ll start with an overview of how traditional approaches for speech enhancement work. This will give us a good foundation for understanding some of the important improvements made possible by AI.


## Speech enhancement 101


Before we get into speech enhancement, it’s useful for us to understand how speech is typically delivered in multichannel audio content. When we talk about multichannel audio, what we typically mean is that we have five or more channels of audio (so we don’t typically count ‘stereo’ as multichannel). In a simple 5.1 multichannel setup, the channels are split up into left, right, center, left surround, right surround, and low frequency energy, or LFE (i.e. 'sub' — this is the '.1' in '5.1').


This is important for speech enhancement because traditional speech enhancement techniques make use of a key factor in multichannel audio: that important dialogue is *almost* *always* delivered on the center channel. While other channels may contain speech information, such as people talking in the background, if the dialogue is important to the story, it will typically be present in the center channel. *Note: this is true for **most** film and TV content, but some content types don’t follow suit, such as games and some sports broadcasts.*


So, if the center channel has so much speech information, surely we can just enhance speech by turning this channel up (or reducing the volume of the other channels)?


That’s certainly part of the solution, although it ignores an important detail: the center channel contains a lot of other audio information too, such as sound effects and music. If you turn up the center channel, you’re not just increasing the relative volume of the speech — you’re increasing the volume of everything else, too!


To get around this problem, a combination of equalization (EQ) and compression is used. EQ is something you’re[probably familiar with](https://www.sonos.com/en-us/blog/what-is-eq) : it allows you to control the volume of different frequencies. Frequency content can also be shaped through the use of *multiband* compression. Compression ‘squeezes’ the sound into a particular dynamic range: making loud sounds quieter and quiet sounds louder. This helps to ensure that everything is audible, and that content is delivered at a consistent volume. Multiband compression is a type of compression which has different settings for different frequency bands.


Common approaches for speech enhancement use EQ and multiband compression to boost frequencies associated with speech — ensuring that speech frequencies are at a consistent level. This results in clearer dialogue which doesn’t boost *all* of the frequencies: making the speech clearer without too much detrimental impact on other types of sound which may be on the audio track.


So, in a nutshell, existing approaches to boosting dialogue apply these processing techniques and reduce the volume of the non-center channels. This sounds fairly simple conceptually, but it takes a lot of work to get right — tuning the parameters to ensure that the speech enhancement delivers a great experience no matter the content. These techniques are very effective, but, depending on the type of content, they can also be insufficient to really clear up the dialogue, or worse: they can be detrimental to the audio quality.


Let’s consider the pros and cons of traditional speech enhancement, and how this affects listening experiences:


### **Enhancing dialogue in an action movie**


The first example we’ll use is an action movie. Action movie audio has a lot going on: dramatic music, dynamic sound effects, and dialogue which can change from a whisper to a shout (and then back to a whisper!) in a matter of seconds. If you typically struggle to understand dialogue, you’re *really* going to struggle with action movies, so these are great examples of where you could benefit from speech enhancement. But there’s one problem: you know all those tricks we just introduced for enhancing speech? Now they’re being applied to a busy audio track. Yes, the speech will generally be clearer, but sometimes the enhancement introduces other problems:


-


The music may sound strange due to the EQ,


-


The sound effects could be less impactful due to the compression,


-


The movie no longer feels as immersive because the volume of the left, right, and surround channels has been reduced.


-


The speech may sound harsh, ‘nasal’, or otherwise unnatural due to the processing that has been applied.


This is obviously not what the creators of the content want you to experience, leading to another concern: these techniques negatively impact *artistic intent* . That is, they alter the listening experience such that it no longer resembles what the creative professionals behind the content — the director, sound artists and designers, etc. — originally intended. But these negative effects aren’t as pronounced for all content types, as we’ll see in the next example.


### **Enhancing dialogue in a documentary**


Now let’s look at how speech enhancement will affect the sound experience for a documentary. Documentary audio has plenty going on, but the content is crafted to educate as much as to entertain. Dialogue tends to be very clear, and it doesn’t have to contend with sound effects and music in quite the same way. As the audio isn’t as busy, there are fewer negative effects of speech enhancement processing.


Your expectations as a listener are also different: you want to be engaged, but it’s not necessarily the immersive experience you expect from an action movie. So, it’s ok if the character of the sound changes a little to improve dialogue clarity. Because of this, traditional methods are typically sufficient for these cases, although they can still suffer from the disadvantages outlined for action movies above.


### **Shared listening**


Some of the described disadvantages of traditional speech enhancement (less impactful sound effects, a less immersive experience, etc.) may be a worthwhile tradeoff for someone who wants or needs to prioritize better speech clarity. However, for many of us, film and TV is a shared experience — and what may be an acceptable compromise for some may negatively impact the sound experience for others in the room. For example, a family film night includes multiple listeners, some of whom benefit from speech enhancement, while others prefer to have speech enhancement off.


Many households have taken to using headphones to address this, but that works against the idea of *shared listening experiences* . If we can reduce the negative effects of speech enhancement, we can instead move towards more *inclusive* shared listening experiences. Enter Arc Ultra’s brand new Speech Enhancement…


## Towards crystal clear dialogue


Now that we’ve learned about traditional speech enhancement techniques, and have an idea of their strengths and weaknesses, we’re ready to talk about our new Speech Enhancement feature. As you may have guessed, the goal of this feature is to deliver crystal clear dialogue no matter the content. Generally speaking, it doesn’t solve all of the problems detailed above, but it does allow us to apply these techniques more aggressively thanks to AI-based speech extraction.


The key to Arc Ultra’s new Speech Enhancement lies in a field of research called *audio source separation* , often simply referred to as *source separation* . Source separation is concerned with separating different sound sources present within audio data. For example, source separation could be used to ‘pull apart’ a song: separating it into all of the different instruments and vocal parts, allowing you to listen to them individually. The kind of source separation we’re interested in for Speech Enhancement is *speech extraction* : extracting the speech from the audio data so that we can process it individually and deliver high quality enhanced dialogue, irrespective of the type of audio we’re working with.


There’s a rich history of source separation research, with a large chunk of this work aimed at telecommunications applications. Many successful techniques have been developed through this research, but film and TV audio presents a unique challenge due to its complexity: sound effects and music are very different to the kind of background sound you expect on a telephone call. Because of this, many of the traditional source separation techniques developed for speech aren’t very effective for these types of audio. At least, that was the case before the advent of Deep Neural Network-based source separation techniques.


Deep Neural Networks, or DNNs, have played a critical role in enabling a huge amount of recent technology, from ChatGPT and impressive image generation to self-driving cars and automatic medical diagnosis. They have a particularly good track record in digital signal processing for de-noising (removing noise from signals), and this makes them a great choice for extracting speech, as really we’re just looking at removing very complex hand-crafted noise. Let’s take a look at how this works.


In this diagram, we see a spectrogram of our audio signal (we’ll just consider a single channel to keep things simple). This is how our neural network sees the data: a time-frequency representation, rather than a time domain representation. In traditional speech enhancement, we’d be applying the processing to what we see in **a** : the entire audio signal.


With Arc Ultra’s new Speech Enhancement, the neural network creates a mask to separate the speech signal from the non-speech signal. This mask is multiplied with the incoming signal **a** to produce signal **b** which contains only the speech information from **a** . One of the key advantages of this method is that it *dynamically adapts* to the incoming audio: generating specialized speech ‘masks’ over 170 times per second.


After surgically extracting the speech content from the audio signal, we process the speech to improve intelligibility and ensure that the speech adheres to a consistent volume level. We then mix this back in with the center channel signal, and send the signal to the speaker. The result is a speech enhancement experience which doesn’t adversely affect the non-speech audio content: giving you excellent dialogue clarity without sacrificing audio quality.


## Neural network tuning


As we’ve learned, neural network-based speech extraction opens up a whole new avenue of speech enhancement techniques. But throwing a neural network into the mix is no simple task: there’s a lot of work that goes into designing a neural network for use in speech enhancement. This includes choosing the right type of network, assembling datasets to train and evaluate the neural network model, and preparing the model for deployment on a soundbar.


If you’re familiar with neural networks, you’ll know that there are many different types of network architecture, and you may have heard terms such as *transformer* , *Recurrent Neural Network (or RNN)* , and *Convolutional Neural Network (or CNN)* . These terms describe neural network *architectures* : structural variations which enable different types of processing. The choice of neural network architecture depends on the problem you’re trying to solve: if you want to recognize objects in an image, you’ll probably use a CNN. If you want to do voice recognition, you may want to use a RNN. These categories are somewhat more nuanced than this, but that’s the general idea.


Selecting an appropriate architecture involves balancing numerous factors, including:


-


**Model size** : the size of the model governs the complexity of data that the model can process. Smaller models are sufficient if you have fairly simple data, and larger models are usually required if your data is complex. But larger models come at a cost: they’re more computationally expensive.


-


**Computational cost** : how much computing power is required to run the model. Depending on the system in question, you may only have a limited amount of compute. This is certainly the case when running on a soundbar: we’re looking to keep computational cost down while achieving high quality output, which brings us to our next and final point.


-


**Performance/subjective quality** : just as bigger models generally come at greater computational cost, they also perform better. In this case, performance is the subjective quality of the extracted speech: does it work well on lots of different types of content? How about different languages?


To balance all of these factors and arrive at a model which produces high quality output suitable for a flagship soundbar, we pitted a variety of DNNs against each other, and evaluated their performance using a combination of objective and perceptual tests. To make things a little more efficient, we started by using automated objective evaluations. These are automated tests we can run to get a variety of scores, which look to capture an impression of the points listed above. This allowed us to filter the number of architectures down from six architectures to just two.


Once we were down to two, we had an important decision to make: do we go with the more computationally expensive option, which — on paper — should produce better results, or do we choose the less computationally expensive option? More specifically, we asked: ***is the expensive model worth the additional computational cost?***


At this point we couldn’t simply rely on automated metrics: what really matters is how the extracted speech sounds to the listener. So, we designed a listening test in which people were asked to listen to the extracted speech from both models and choose which they preferred. The kind of test we employed was a *blind A/B test* : people had to listen to two audio clips and choose the one they preferred. To avoid bias, people didn’t know which model was used to generate the clips they were listening to.


The results of people’s preferences were almost exactly 50/50: there was no statistically significant evidence to support the claim that the more computationally expensive model resulted in better quality extracted speech. Given this, we opted for the less expensive model — but the work didn’t end there.


The next phases of tuning involved incrementally improving the model in response to expert feedback from our Sound Experience team. This team comprises seasoned specialists in tuning audio systems, as well as people with experience in mixing audio for film and TV. The Sound Experience team is responsible for developing and tuning the Speech Enhancement pipeline, and thus deciding how to use the extracted speech in the Speech Enhancement feature.


At each phase, we used the expert feedback to modify the model parameters and update the data used to train the model. Data can be quite a contentious issue in AI: you may have heard lots of people criticizing where certain companies get their training data. We feel that it’s important to source data responsibly, and as such data used in this work was either sourced from open datasets (such as[the DNS challenge dataset](https://github.com/microsoft/DNS-Challenge) ) or was assembled by a professional sound designer. All of this data was combined with our bespoke data augmentation pipeline, which allowed us to make the most of our data by combining it in a variety of ways, resulting in over *two thousand hours* of data.


This dataset contained examples of all the different types of speech and sound effects you’d expect to encounter in film and TV: everything from footsteps to spaceships, as well as speech from a broad variety of languages. All of this was additionally processed with many different effects, to ensure that the model was exposed to the kinds of special effects processing that’s used in film and TV. For example, distortion is often used to simulate the sound of people speaking over radio, and reverb is often used to simulate how speech sounds in different acoustic environments.


The figure below gives an impression of the data used to evaluate the model, and how the model evolved over time. This shows three of key ‘scene types’ in the dataset, which each have different varieties of sounds. For example, ‘machine’ scenes include sounds of machinery and vehicles, and ‘special fx’ scenes contain effects you may associate with sci-fi or supernatural phenomena.


The metric we’re looking at here is a type of *reconstruction error* : if the model is performing well, we will be able to *reconstruct* the speech information accurately, thus what we want is a low reconstruction error. As this is an error metric, lower values are better: as we see here, there’s a huge improvement in reconstruction error as we move through the key model versions. This is particularly pronounced for *special fx* and *fighting* scenes, which were particularly challenging for earlier models.


After months of iterating, we had a model which met the quality criteria set by the Sound Experience team. We prepared it for deployment on the sound bar using the Tract inference engine (you can read more about that in our tech blog post[here](https://tech-blog.sonos.com/posts/optimising-a-neural-network-for-inference/) ), and the next crucial phase of work began: learning how to use real time speech extraction to create a transformative speech enhancement feature.


In the next blog post, we’ll talk about the next step in the process: working with hearing loss experts at the Royal National Institute for Deaf people (RNID), enabling us to tune speech enhancement for the people who will benefit most from the feature.


Share
