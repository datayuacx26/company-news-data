---
schema_version: "1.0.0"
document_id: "646c0b8b06da9cf2c2047b04a68fa0f8a83f41268987e5daa535693cd290d4b3"
company_key: "sonos-inc-common-stock"
company: "Sonos Inc."
source_id: "sonos-inc-common-stock-rss-b40e25ea1a3a"
canonical_url: "https://tech-blog.sonos.com/posts/arc-ultra-speech-enhancement-delivering-inclusive-sound-experiences/"
published_at: "2025-05-27T16:38:49+00:00"
first_seen_at: "2026-07-20T23:18:24.938468+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:0a51255333f50b8cd66aa1f4cbf83e44f9b4fe773020d1233af99c8e887e0e81"
---

# Arc Ultra Speech Enhancement: Delivering Inclusive Sound Experiences

At Sonos, we know how to make great speakers, but **that doesn’t mean we know how to deliver a great sound experience to every listener** . In fact, even[the award-winning film mixers on our sound board](https://www.sonos.com/en-gb/blog/sonos-sound-experience-creators) can’t guarantee great sound; that’s because great sound depends as much on the listener as it does on the speaker system. This is why we’ve partnered with[the Royal National Institute for Deaf people (RNID)](https://rnid.org.uk/) to develop an inclusive approach for how we tune our speakers.


In the last Sonos Tech Blog[post](https://tech-blog.sonos.com/posts/arc-ultra-speech-enhancement-announcing-a-step-change-in-speech-enhancement-using-ai/) we saw how recent advances in AI have opened up new possibilities for speech enhancement: making it possible to deliver excellent dialogue clarity without the compromises required of traditional methods. These technological advances are great, but what really matters is how we use them to create **excellent shared sound experiences for everyone** .


To achieve this Sonos teamed up with researchers from the RNID: the UK’s national charity supporting more than 18 million people who are deaf, have hearing loss or tinnitus. Their work spans a broad range of activities, from[educating the public about hearing health](https://rnid.org.uk/information-and-support/take-online-hearing-check/) , to advising companies and the public sector on the adoption of new technologies. Their deep understanding of human hearing made them the perfect partner to work with on developing truly inclusive speech enhancement: allowing Arc Ultra to deliver excellent sound experiences for a broader audience.


Before we dive in and learn more about the collaboration, we invite you to take a look at[this short video](https://youtu.be/f4ZxggStIzM) which captures a few first-hand accounts from the study participants.


## Learning more about hearing loss


RNID gave Sonos a Hearing Loss 101 – teaching our researchers and engineers about the prevalence of hearing loss, the various forms it takes, and the technologies that are available for those living with hearing loss.


We learned that approximately **one in three adults in the UK** and just under **one in four adults in the USA** live with some degree of hearing loss. These statistics get worse with age, with hearing loss affecting **more than 50% of adults over 50** , and **80% of adults over 70** in the UK. The majority of those affected have mild to moderate hearing loss: these people are often fine with clear speech in quiet environments, but are likely to struggle if there’s background noise, or if the speech isn’t clear (such as in the case of sound effects applied in movies or TV programs).


If we look at what this means for enjoyment of film and TV, we see that people in the demographics who are most affected by hearing loss also watch the most TV. This tells us that an improved speech enhancement feature can positively impact a large number of people.


Broadcaster's Audience Research Board data on average hours spent watching TV (in minutes) by age groups between 2018-2023. Table is replicated from the 2024 Ofcom Media Nations Report


The RNID taught us that hearing loss can impact people in a variety of ways:


-


It could affect both ears (bilateral) or just one (unilateral).


-


Hearing loss may affect how you hear the whole frequency spectrum, or just part of the frequency spectrum (for example, certain types of noise-induced hearing loss can be very frequency-specific).


-


It may also cause compression of the comfortable dynamic range of sounds: making it harder to hear quieter sounds, but also making loud sounds uncomfortable (or even painful).


These can be experienced individually or in complex combinations. This huge variety of types of hearing loss alone makes designing a speech enhancement feature very challenging, but it’s not the only factor we need to consider: there are also a huge array of hearing aid solutions.


Arguably, if there are so many hearing aid solutions, why would people need speech enhancement at all? It’s true that, for some people, a hearing aid is sufficient to allow them to enjoy multimedia content with little or no speech enhancement. But, for many people, hearing aids add another layer of complexity:


-


Their hearing aids may work well for some content, but not for all content. For example, they may not work well in very challenging, dynamic content, or content in which the speech has been heavily processed.


-


Hearing aid use can be fatiguing and they may prefer not to use them when watching TV.


-


Hearing aids usually have multiple different ‘programmes’ which different people will use in different way


-


Hearing aids alone may simply not be sufficient.


When we combine these factors – different types of hearing loss, hearing aids, and various ways they can be used – we start to see just how difficult it is to design a speech enhancement feature that works for everyone. It quickly becomes obvious that there’s no ‘one size fits all’ solution (something our new Speech Enhancement feature on Arc Ultra tackles through the introduction of[multiple speech enhancement levels](https://support.sonos.com/en-gb/article/make-dialogue-easier-to-hear-with-speech-enhancement) ).


With such a complex problem to solve, it was clear that there was only one way to go about solving it: by working with real people, who would benefit most from the feature.


## Involving the hearing loss community


The RNID recruited a range of participants to help us learn more about the experiences of individuals with hearing loss. Some of these participants attended workshops in which they shared detailed accounts of their listening experiences. The majority of participants took part in listening tests designed and run by the RNID researchers. These tests were carefully designed to provide Sonos with the information needed to create the best possible sound experience for people who use speech enhancement.


The listening tests were split into two phases. The first phase took place earlier in development, before the Sonos Sound Experience team began tuning the new AI-powered speech enhancement feature. This allowed the learnings from the first phase to feed into the tuning process. The second phase took place once initial tuning had been completed, allowing us to see how well the tunings worked for people, and providing an opportunity to incorporate further feedback. Let’s take a look at what each of these phases of listening tests involved.


### Phase one: learning how to tune AI-powered Speech Enhancement


The first phase of tests looked to answer a few key questions:


-


Do people like this approach to speech enhancement?


-


How much enhancement do people want?


-


What are the negative effects of extreme enhancement?


To answer these questions, we gave the participants control over a continuous range of speech enhancement options, using the speech extraction neural network described in[the previous blog post](https://tech-blog.sonos.com/posts/arc-ultra-speech-enhancement-announcing-a-step-change-in-speech-enhancement-using-ai/) . Starting with no enhancement, participants could increase the balance of speech relative to non-speech: maintaining the overall volume while reducing the level of the non-speech content with respect to the extracted speech signal (represented as *enhancement level* in the figure below).


In order for this to be a fair test of people’s preferences, each participant watched the same selection of video clips. Choosing these was a little tricky: we needed to ensure that we covered a wide range of content, but we also had to minimize the amount of time required for the tests. This is because it gets quite tiring focusing on content over long periods of time – shorter tests are both more enjoyable for participants, and more accurate as feedback isn’t affected by listening fatigue.


Discussions between the sound experience team and the RNID researchers resulted in a selection of nine clips covering a broad range of genres including documentary, travel, action, and scifi/fantasy. These clips not only varied in genre, but also in speech content: some contained mostly clear dialogue, some contained speech with special effects, and a few clips were set in crowded environments. The clips were between one and a half minutes to three minutes in length, allowing participants to watch each clip multiple times to compare the effect of different settings.


#### How much is too much?


The RNID researchers described the results of the phase one tests as *overwhelmingly positive* , which is obviously a great thing to hear! The results showed that all participants preferred to have some level of enhancement rather than having no enhancement. Additionally, we saw that generally higher levels of enhancement were preferred for people with moderate or severe hearing loss – although many people with mild hearing loss also gravitated towards higher enhancement settings.


However, we also learned that particularly high levels of enhancement were a little too much for some participants, as – despite all participants preferring enhancement over no enhancement – 44% of participants identified levels at which they found the speech enhancement unpleasant. This wasn’t particularly surprising – in fact, the surprising thing was that 56% of participants *didn’t notice any negative effects* , even at the highest levels of enhancement.


The results from the phase one tests told us that we could push the speech enhancement more than we had originally intended: not only did participants tolerate enhancement at high levels, ***they preferred using it at higher levels** than expected* . This introduced a new challenge: how do we balance high levels of enhancement with artistic intent, to make sure viewers are getting the best possible experience?


### Phase two: getting the balance right


The Sound Experience team worked hard on tuning the new speech enhancement feature following the first phase of listening tests, and by the time phase two rolled around they had three prospective tunings to try. Each of these were meticulously crafted, combining the extracted speech with a variety of audio processing techniques. This allowed the system to deliver the clearest possible dialogue while also preserving the stunning cinematic presentation that Sonos customers expect from our speakers. The question was now: which of these tunings should make it into the product?


For the second phase of testing, the RNID researchers had to design a testing protocol which allowed us to determine which settings participants preferred, but in such a way that participants weren't overwhelmed with choice. Just as with the phase one tests, this needed to be achieved while also allowing participants to watch a broad variety of content. There was also a third layer of complexity: we needed to keep the settings anonymous to ensure that people weren’t biased by concepts such as ‘high enhancement’.


As with phase one, a broad selection of content was chosen: eight clips, spanning everything from daytime TV to Hollywood scifi/action films. Speech enhancement levels were anonymized by giving participants a choice of colors, instead of enhancement levels. Each color corresponded to one of four speech enhancement settings, including an ‘off’ setting, giving participants the option of choosing no enhancement. To ensure the tests could be carried out efficiently, the participants were presented with a series of pairwise comparisons, as we see here.


The first choice participants had to make was between white and green (moderate vs. no enhancement). Once they’d made this choice, participants were then asked to select from another two choices, dependent on their first selection. If participants chose green, they were asked to select between green and purple (no vs. mild enhancement). If participants chose white, they were first asked whether they thought white was sufficient. If they answered yes, they were asked to choose between white and purple (moderate vs. mild enhancement). If they answered no, they were asked to choose between white and red (moderate vs. high enhancement).


As in phase one, the results of these tests painted a clear picture: there was an overwhelming preference for higher levels of enhancement. As we see below, at least 75% of participants selected the *moderate* or *high* levels of enhancement for all clips. However, it’s important to recognise that, irrespective of their level of hearing loss, people have different preferences. This is why a variety of options is so important – something validated by several participants preferring the *off* and *mild* settings for various clips.


These tests, and the discussions we had with participants, confirmed to us that people were enjoying the improved speech enhancement made possible by incorporating AI. We learned that participants generally preferred the two higher levels of enhancement, but also that every tuning had its place: people valued the flexibility afforded by a variety of levels. Since these tests, our Sound Experience team has continued to refine the feature based on what we learned throughout the process, and we’re excited to see how Arc Ultra’s new Speech Enhancement feature will improve home theater experiences for our customers.


The RNID collaboration was a huge success, allowing us to bring lived experience into the design process and, from that, create a product which delivers premium Sonos sound to a broader spectrum of listeners. In the next post, we’ll explore the Beta data to see how people using Arc Ultra’s new Speech Enhancement feel about the feature, and how it’s changed their listening experiences at home.


Share
