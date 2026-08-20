---
schema_version: "1.0.0"
document_id: "58ae7ccab40f85a52b1c40032e9dd70bc488070589ae4635cf8f59c8efa086ac"
company_key: "interdigital-inc-common-stock"
company: "InterDigital Inc."
source_id: "interdigital-inc-common-stock-rss-0a5017ec469a"
canonical_url: "https://www.interdigital.com/post/video-coding-for-machines-the-need-for-compression"
published_at: "2024-09-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:31.532162+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:62b9491a797ff47f56ad320eb7d1f5b7bca9d3a4e2c3b638b2e707569e2571c5"
---

# Video Coding for Machines: The Need for Compression

# Video Coding for Machines: The Need for Compression


Video coding for machines enables efficient AI-driven video processing for smart cities, intelligent traffic, and more. Learn about the need for compression in this guide from InterDigital.


September 2024


Fabien Racapé,


Hyomin Choi


<p>More <a href="https://www.cisco.com/c/dam/m/en_us/solutions/service-provider/vni-forecast-highlights/pdf/Global_Device_Growth_Traffic_Profiles.pdf">than 82% of all traffic</a> seen on the Internet corresponds to video streams and the vast majority of this video traffic consists of 2D video streams to be viewed by end users on flat displays like smartphones, TVs, tablets, and the like. Looking ahead, we expect the proportion of video traffic to increase with new devices, formats, and modalities, including video destined for head mounted displays, high resolution content beyond 4K, and immersive video streams augmented with haptic or other sensory information. For these applications to be successfully deployed, efficient compression technologies are essential, and MPEG standards will play a critical role in enabling this.</p>
<p>While much of the aforementioned video content is directed at human consumption, there is a rapidly expanding set of applications and use cases that human eyes may never see and where machines and algorithms are the intended consumer. For example, consider a next generation factory assembly line containing several video sensors that &ldquo;watch&rdquo; items pass by on the assembly line and send the video feed to a central analysis unit, which stops the assembly line when an unauthorized object is observed on the line. These detailed and complex operations can all be perceived and processed without any human intervention.</p>
<p>Separately, the number of video surveillance cameras is expected to reach <a href="https://www.abiresearch.com/blogs/2023/06/15/intelligent-video-analytics/">1 billion globally by the end of 2026</a>, a trend that is bolstered by the incredible success of AI algorithms and models to solve vision/perception tasks like object recognition and tracking at near human levels and the widespread deployment of sensors capable of capturing and transmitting video streams, including home security cameras, factory automation sensors, and IoT devices. Typically, these low-cost devices lack the compute resources, or the battery capacity, to run the complex AI models required by the vision tasks, so these operations are often better suited to run in edge compute servers, or cloud environments. In this growing ecosystem, the need for new compression mechanisms arises.</p>
<p><span style="color: #652c8f;"><em>For clarification, the term Video Coding for Machines (VCM) refers to two distinct concepts. First, VCM refers to the high-level concept of compression video streams for machine consumption, while the second usage of the term refers to the particular MPEG approach to address these needs. Both terms are explored in this blog.</em></span></p>
<h2><span style="color: #00aeef;">Why Do We Need Video Compression for Machines?</span></h2>
<p>As these use cases become more prevalent, it is entirely conceivable that the total volume of video traffic meant for machines and algorithms could rival that of video consumed by humans. A critical consideration is that algorithms and the human eye care about very different things. In the previous assembly line example, the vision task only needs parts of the frame that can inform whether the object detected is unusual in shape, dimensions, or geometry, but the rest of the frame is of no interest. The human eye would need very different information to contextualize and make sense of what&rsquo;s in the frame. By discarding this ancillary, human-specific information, we would need far fewer bits to support the successful execution of the machine task. Thus, good compression algorithms and systems for machine vision will be key to allow the network to support this demand for bandwidth.</p>
<p>A common theme in the use cases we outlined is the &ldquo;off-loading&rdquo; of the compute, or the AI vision model that runs on the video, to a different entity at the network edge, or even in the cloud. With this flexibility, we can explore interesting trade-offs between power consumption, network bandwidth, and latency.</p>
<p>We can distinguish two approaches to this coding. First, we can fully offload the vision task, like when a sensor device encodes and transmits the video to a remote server that runs the AI vision model on the decoded video. The second approach consists of splitting the AI vision model into two parts &ndash; one that sits at the sensor device, and the other that runs on the remote server. This follows the so-called &ldquo;split inference&rdquo; architecture, which involves the transmission of intermediate features. In both cases, the <strong>efficient compression</strong> of the transmitted data from the sensor device to the <strong>remote server is crucial to build viable network-assisted computer vision applications</strong>.</p>
<h2><span style="color: #00aeef;">Video and Feature Coding Standardization Efforts</span></h2>
<p>These two approaches are both being actively studied for new standards in the MPEG Video Working Group.</p>
<p><img src="https://s3.amazonaws.com/files.interdigital.com/55bd0288af0b0930ba599bd0c4b7ca38/resources/uploads/direct_editor_uploads/Video_And_Feature_Coding.png" alt="Video and Feature Coding for Machines Diagram Source: InterDigital" /></p>
<p>&nbsp;</p>
<p style="text-align: center;"><em>Video and Feature Coding for Machines Diagram Source: InterDigital </em></p>
<h3><span style="color: #00aeef;"><em>Video Coding for Machines</em></span></h3>
<p>The first approach described above is being standardized in MPEG as <strong>Video Coding for Machines</strong> (VCM). As a standard, VCM saves on energy consumption at the sensor device but may require more bandwidth to transmit content than with split inferencing. Video codecs can be made more efficient here &ndash; because not every detail of the video is needed, only the information relevant to accomplishing the vision task will be selectively coded.</p>
<h3><span style="color: #00aeef;"><em>Feature Coding for Machines</em></span></h3>
<p>The second approach, <strong>Feature Coding for Machines</strong> (FCM), is also being studied and standardized in MPEG. Here, features, or the intermediate data, are tailored for the target vision task(s) and compressed in bitstreams smaller than the original video data to be transmitted over the network. This approach saves on bandwidth but incurs a slightly higher energy cost by running part of the split AI model at the sensor device.</p>
<p>In practice, these two approaches can be used interchangeably depending on use cases, device type, hardware capabilities, network environment, or task requirements.</p>
<h2><span style="color: #00aeef;">InterDigital's Approach to Video and Feature Coding</span></h2>
<p>At InterDigital, our research investigates both approaches and their tradeoffs. Our <a href="https://github.com/InterDigitalInc/CompressAI-Vision">open-source software CompressAI-Vision</a> enables researchers to design and compare performances of split and remote inferencing pipelines. We are also actively involved in MPEG standards, contributing both software and core experiment coordinator roles. Notably, InterDigital&rsquo;s CompressAI-Vision was adopted as an evaluation platform for the MPEG-FCM standardization effort.</p>
<h3><span style="color: #00aeef;"><em>The Future of Video Coding for Machines</em></span></h3>
<p>Some of the approaches we discussed can be applied to use cases that are already being defined and developed by industry, like smart cities, intelligent traffic management, vehicle-to-everything communications, and connected/intelligent farming, to name just a few. There are several more likely to emerge that have not been envisioned yet. Our research activity in this area is looking to build efficient, flexible, robust tools and algorithms that will allow these applications and ecosystems to be built and scaled. As a complement to this, our active participation in standards helps reduce the complexity of deploying these systems by designing standard interfaces and protocols, all of which go to support the widespread deployment and eventual success of these future-looking tools.</p>


## Related INSIGHTS


Explore the latest research and innovations in wireless, video, and AI technologies.


BLOG POST


### [Mobile AI: Is It Hype, or an Inflection Point for Wireless?](https://www.interdigital.com/post/mobile-ai)


Is mobile AI a true turning point? Explore why it matters for wireless networks and how mobile AI could shape future dev...


READ MORE


VIDEO


### [Built on Innovation: InterDigital & the Future of Connectivi...](https://www.interdigital.com/videos/built-on-innovation-interdigital-and-the-future-of-connectivity)


Discover how InterDigital is shaping the future of connectivity through wireless, video, AI, and 6G innovation that powe...


READ MORE


EBOOK


### [6 Lessons Learned from 5G Deployment to Capture 6G Success](https://www.interdigital.com/ebooks/6-lessons-learned-from-5g-deployment-to-capture-6g-success)


5G transformed wireless connectivity, but its deployment also uncovered valuable lessons that will shape the next genera...


READ MORE


BLOG POST


### [Generative & Agentic AI in Telecom: Shift to AI-Native Netwo...](https://www.interdigital.com/post/generative-and-agentic-ai-in-telecom)


Generative and agentic AI are reshaping telecom networks. Learn why real-time AI systems require AI-native wireless arch...


READ MORE


BLOG POST


### [Two Hundred and Fifty Years of American Invention](https://www.interdigital.com/post/two-hundred-and-fifty-years-of-american-invention)


READ MORE


BLOG POST


### [Haptics Hold the Key for Greater Accessibility, Immersivity...](https://www.interdigital.com/post/haptics_hold_the_key_for_greater_accessibility_immersivity_in_live_sports)


READ MORE
