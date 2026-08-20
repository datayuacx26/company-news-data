---
schema_version: "1.0.0"
document_id: "2a316ae21cd275c03d35d32fe83eab320e8feb7fc37c0c9e8e0869c889605d1a"
company_key: "sonos-inc-common-stock"
company: "Sonos Inc."
source_id: "sonos-inc-common-stock-rss-b40e25ea1a3a"
canonical_url: "https://tech-blog.sonos.com/posts/sonos-microphone-system-design/"
published_at: "2023-07-23T21:22:00+00:00"
first_seen_at: "2026-07-20T23:18:24.938468+00:00"
fetched_at: "2026-07-28T22:00:21.221344+00:00"
content_hash: "sha256:afebc9db31d52be10b8fc12aac49d151776ce23c53d662fc9e33815ecbab94e8"
---

# Sonos Microphone System Design

Sonos first introduced microphones into its products in 2017 with the Sonos One, which arrived at a time of excitement around the new voice assistants that other big tech companies were launching. The microphones on the Sonos One and the Beam, which was introduced shortly after, allowed Sonos customers to use their voice to interact with all of the devices in their household. Subsequent products, such as Arc, One Gen 2, Move, Roam and Beam Gen 2 continued to include microphone arrays and advance the ways in which these microphones were used to enable features for the users.


With microphones being designed into many of Sonos’ new products, there were many opportunities to use these microphones not just for voice control, but for a variety of other acoustic sensing applications. An obvious application was to use the onboard microphones for[Trueplay](https://tech-blog.sonos.com/posts/trueplay-spectral-correction/) to adapt the speaker’s sound for its environment. Trueplay has been available for many years as a way to optimize the sound of Sonos speakers in a room by using the microphones in a phone or tablet to measure the speaker. However, newer portable speakers, like Move, are unlikely to stay in one place all the time like a plug-in speaker may. Using the onboard microphones to enable auto Trueplay means that instead of triggering a Trueplay tuning through the Sonos app, this audio tuning can adapt to your environment in real-time or whenever the device is moved from one location to another. With the release of the Era 300 and Era 100 products, Sonos has again made an advancement in Trueplay by introducing Quick Tuning. This new feature uses the device’s on-board microphones, rather than the microphone on a phone or tablet, to tune the acoustics for its placement in the room, a first for Sonos’ plug-in speakers.


The Sonos Roam introduced[Sound Swap](https://tech-blog.sonos.com/posts/how-near-ultrasonic-audio-adds-spatial-awareness-to-the-sonos-system/) , which uses inaudible near-ultrasonic audio signals to detect and identify nearby speakers to which the Roam can send or receive its playback audio stream. Doing this detection acoustically makes use of hardware (speakers and microphones) that are already in the product to enable an exciting new experience. In 2022, Sonos introduced[Sonos Voice Control (SVC)](https://tech-blog.sonos.com/posts/on-device-voice-control-on-sonos-speakers/) , which again uses the microphone arrays on our products for voice control of the system, but with a focus on speed and privacy.


### Microphone Engineering Design Process


The microphone system engineering process starts with the kick-off of a new product design. A microphone systems engineer works with other engineers in many different disciplines to work on this design. The industrial designers provide perspectives on where visual elements, like the acoustic ports, work in the design and what those ports could look like. Mechanical engineers provide input on how microphones can be mounted and ported, discuss materials that can be used in the acoustic port designs, as well as what manufacturing processes (molding, CNC, etc) we can use to support the acoustic design. Electrical engineers work with the microphone systems engineer to ensure that power and signals are available where the mics need to be and that the electrical interfaces between the microphones and the rest of the electronics are compatible. The audio systems engineers coordinate on how transducer placement and tuning may affect microphone performance in different locations on the physical product. All of the decisions that this team of engineers make also affect radio systems design, product integrity, user experience and other disciplines who are all contributing to making a great product.


One of the first decisions that the microphone systems engineer makes for a product is the type of microphone that will be used. Like most modern consumer electronics designs, all of the microphones in Sonos’ products are using micro electro-mechanical systems (MEMS) technology. MEMS microphones are very small, have great acoustic performance, can be mounted on PCBs with other electronic components with standard soldering processes, and are reliable and consistent. MEMS microphones are available from a variety of manufacturers with different electro-acoustic performance characteristics and different analog and digital interfaces. Many of the systems-on-a-chip (SoC) that Sonos uses have a pulse density modulation (PDM) input, which is the most-common digital interface available on a MEMS microphone. Modern PDM microphones have great electro-acoustic performance and are increasingly more power-efficient, which is always a focus of the product design.


MEMS Microphone on a PCB


The microphone systems engineer must help to define the acoustic requirements that are needed to support the microphone-enabled features in a product. For example, Sound Swap requires near-ultrasound acoustic sensing, so the microphone needs to have a good response close to 20 kHz. The microphones need to have a low noise floor (or high signal-to-noise ratio) so that they can pick up quiet voice commands from across a room. These microphones are usually mounted in a product in close proximity to some very loud transducers; the microphones need to pick up this selfsound without overloading or distorting so that the speaker can either cancel that sound to hear other sounds in the room (like voice commands) or measure the response of that output sound in the room for supporting Quick tuning or auto Trueplay. In such close proximity to these playback transducer arrays, the sound pressure level can exceed 110 dB SPL, so we use microphones that have an acoustic overload point (AOP) at least 10 dB above that.


Along with selecting the specific microphones that a product will use, we must also select how many microphones to use and where they will be placed. All of Sonos’ products have an array of microphones, rather than a single microphone, in order to support signal processing that uses this array to improve voice recognition and capture room acoustics from diverse positions. When selecting the number of microphones to use, we need to balance the improvement in performance of using more and more microphones with the added processing power it would take to support these and with the increased manufacturing cost of including these in our products. The microphone systems engineer works closely with the software teams who are integrating the signal processing that uses these microphone arrays. This collaboration includes simulating, modeling, and physically prototyping various microphone array concepts that may differ in both the number of microphones and the geometry of the array. This prototyping work ultimately informs the decisions about the final design of these arrays. Depending on the requirements of a specific product, the array may end up as a two-dimensional cluster on the top of the product, like on Move and Era 100, or a more spread-out array like on Arc where the microphone array spans more than one meter!


Cross-sections of COMSOL model setup and results


Where on the product the microphones are placed is not the only factor that affects the microphone array performance. The individual MEMS microphone components have their own electro-acoustic characteristics and how these are acoustically mounted in the product ultimately affects their performance. These microphones are not directly exposed to the acoustic environment; rather they are acoustically ported through the shell of the product and the design of this port can have a significant effect on the response of the sound received by the microphone. We simulate the length and diameter of this port, as well as any protective layers (acoustic mesh or membrane) to understand what design will work best acoustically given the constraints of the industrial and mechanical design, as well as the materials being used for the assembly. Differences of tenths of a millimeter can have significant effects on the microphone response, so we do many iterations of our simulations to identify what we want to build. We use COMSOL for running these finite element modeling (FEM) simulations. This tool lets us simulate the effects of the port geometry, including thermo-viscous acoustic effects, in-line resistive materials and small shifts in the assembly process, as may be seen in manufacturing. These simulations are much faster and cheaper than building and measuring dozens of prototype microphone ports, though we do often build 3D-printed microphone port prototypes once we’ve narrowed down the designs to the best candidates. We measure these port prototypes to correlate with our simulations and ensure that what we want to build into the full system will perform as we expect.


Simulated microphone system frequency responses


###


### Hardware Prototyping


We build prototypes of all of our products with increasing levels of maturity through the development cycle. Our first acoustic prototypes may be built from MDF and may not look like the final product, though they are still very useful for audio and acoustic development. These prototypes allow the microphone systems engineer to have a first listen at how the product might sound and can be used to highlight potential challenges to the microphone system design. At this stage, we experiment with microphone placement using flex PCB-mounted microphone evaluation boards that we get from our microphone suppliers. This gives us the flexibility to stick microphones anywhere on the prototype and take measurements at potential mounting locations.


Example of early hardware prototypes used to measure microphones in different positions


As we start to build the first prototypes that resemble the industrial design intent, we are including microphones on PCBs that are connected to the main electronics of the design. At this early prototyping stage, we often include more microphones in different locations (or, at least, provisions for plugging in more mics) than we will eventually include in the final product. This allows us to measure and evaluate different positions and use measurements like the microphone frequency responses, selfsound levels and room impulse responses to assess which locations are best-suited to meet the product requirements and enable the mic system to perform at its best to support the required features.


Eventually, we are building off-tool prototypes at our factories. These prototypes look very much like their final production form. With this more-mature acoustic, industrial, mechanical and electrical design, we can measure the performance of the mic system in many different scenarios. At the factory, we are developing production tests that we use to ensure that the product is performing as-designed as it comes off of the production line. In the lab, the microphone systems engineer is evaluating the prototypes in great detail. These measurements happen both in our anechoic chambers and in our listening room labs. The anechoic chambers allow us to measure the acoustic performance of the product on its own without any influence from echoes or reverberation in a room. We measure the acoustic responses of the microphones themselves in the product, how the microphones interact with the audio playback system, check for unwanted mechanical or radio frequency interference, and check the results of our measurements against the original simulations performed at the beginning of the project. We check that all microphones in the array are performing similarly to each other (assuming that’s the intent!), check that the electronics and the software receiving the microphone signals are functioning properly, and ensure that the assembled prototype units are performing consistently.


Era 100 in an anechoic chamber for measurement


###


### Software Features and Experiences


Once the microphone system design is stable and performing as we want, we can use the results from these measurements to inform software and other feature development. Trueplay, Sonos Voice Control, and other mic-enabled features use these acoustic measurements to inform their tuning and development. The microphone systems engineer works closely with the software engineers to ensure that the algorithms running to support the mic-enabled features are performing well. These teams work closely together to tune algorithms like the acoustic echo canceller (AEC) and[multi-channel weiner filter (MCWF)](https://tech-blog.sonos.com/posts/noise-reduction-for-distant-voice-recognition-in-smart-speakers/) . As these processing blocks are developed, the microphone systems engineer may be testing their performance under different conditions, such as in-room audio playback with different content playing while voice commands are issued. This helps to validate that the AEC and MCWF algorithms perform well in an environment like Sonos’ users will ultimately be in. Features like auto Trueplay and Quick Tuning benefit from incorporating the microphone’s in-system response of both of the acoustic port and microphone system, as well as the transfer function from the playback system to the microphone system. Including the response of the hardware and acoustic system plays an important role in driving the development of these features.


### Conclusion


Sonos uses microphones to enable an increasingly-diverse set of features and the development cycle to enable these features involves cross-functional cooperation between a diverse set of engineering teams. For more details on these features, check out the other Sonos Tech Blog articles linked from this article.


Share
