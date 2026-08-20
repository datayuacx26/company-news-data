---
schema_version: "1.0.0"
document_id: "82bd34fd86f6e797ad6352ccf2fd79865fc6d6382fb55be7f3fccb4644426df6"
company_key: "yc-knowlify"
company: "Knowlify"
source_id: "yc-knowlify-news-import-29601cf83fbc"
canonical_url: "https://knowlify.com/articles/ai-generated-instructional-videos-student-study"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T22:16:33.817134+00:00"
fetched_at: "2026-08-04T23:10:21.652752+00:00"
content_hash: "sha256:34970c46ef51257b908d134374414099345cef84bf124d8b0c0461f6db4cc26e"
---

# What 170 Students Think of AI-Generated Instructional Videos (2026 Study)

## Where Students Said It Does Not Belong


Poor fit Students Share


Complex, advanced, or nuanced topics 50 31%


Subjective topics such as philosophy, ethics, art, and public speaking 22 14%


Replacing instructor-led lectures entirely 26 16%


Interactive learning such as debugging, algorithm design, and seminars 14 9%


Niche topics with limited source material online 6 4%


The boundary students drew is sharp. Complex material needs elaboration, back-and-forth, and someone who can respond to the specific shape of a student's confusion. As one student explained, discussions "require dynamic back-and-forth perspective sharing and real-time responses to student viewpoints." Nobody claimed a video could do that.


## What Students Are Worried About


Concerns were coded from the same 163 responses. Nine students (6%) reported no concerns at all.


Concern Students Share


Inaccurate or hallucinated information 94 58%


Lack of depth, oversimplification, skipped steps 22 13%


Lack of engagement, monotone delivery, pronunciation errors 21 13%


Fewer interactions with instructors and TAs 16 10%


Undermines the value of tuition 17 10%


Missing instructor insight and personal experience 9 6%


Broader societal impacts including instructor pay and energy use 9 6%


No thorough fact-checking by an educator 7 4%


Cannot ask follow-up questions 6 4%


Instructors may reduce effort and offload teaching 5 3%


Accuracy dominates everything else. At 58%, it was named more than four times as often as the next concern. One student summarized the asymmetry neatly: "although professors can make mistakes, my experience of AI is that it will."


There is a subtler concern in that table worth pulling out. Seven students noted that high production quality can itself be a risk, because a polished video can "lull a false sense of security" and make errors easier to miss. Given that this study also found students rated production quality at 4.52 and half could not detect AI generation, that warning is well placed. The better the output looks, the more the human review step matters.


## What This Means If You Are Building Video


The study was run in a computing classroom, but the findings transfer cleanly to corporate training, customer education, and documentation.


**Deploy AI video where students already said it works.** Foundational concepts, syntax and procedure, visual processes, summaries and refreshers, and supplemental explanations of material that also exists in writing. This maps almost exactly onto the highest-volume, lowest-differentiation content in most[training video libraries](https://knowlify.com/articles/best-ai-video-tools-training-education) and most[new-hire onboarding programs](https://knowlify.com/articles/ai-onboarding-videos-scale-new-hire-training) .


**Keep a human in the loop and say so.** Accuracy was the top concern by a wide margin, and four percent of students specifically worried that nobody had reviewed the video end to end. In the study itself, each video included roughly 10 minutes of human verification time. Build that step into your process and tell users it happened.


**Do not position AI video as a replacement for a person.** The data on this is unambiguous. Comfort sits at 46% when AI video is described as instructional material and drops to 10% when it is framed as a substitute for the instructor. The framing changes the reception more than the content does.


**Expect the format to carry more weight than you think.** The single most consequential design choice in this study was removing the avatar. Animated explainers scored 4.52 on production quality in a literature where avatar-based videos had previously been called distracting and uncanny. If your audience is skeptical, animation is the safer format.


## How the Videos Were Made


For teams evaluating the production economics, the paper documents the workflow precisely.


The researchers uploaded GitHub's official Markdown formatting documentation as a PDF to[Knowlify](https://create.knowlify.com/) and used a one-shot prompt to generate each video. The platform produced a narration script, a scene-level storyboard, and voiceovers, all reviewable and editable before final render.


Production metric Result


Planning, reviewing, and configuring 8 to 11 minutes per video


System generation time 3 minutes 48 seconds average


Human verification About 10 minutes per video


Total time per 3-minute video At most 25 minutes


Cost per video at study-period pricing Roughly $50 to $60


Twenty-five minutes of total human and machine time per finished three-minute video is the number worth holding onto. Traditional instructional video production is typically quoted per finished minute in the hundreds to thousands of dollars and measured in days or weeks. For a fuller cost comparison framework, see our guide on[measuring the ROI of AI video](https://knowlify.com/articles/measure-roi-ai-video-enterprise-learning-development) .


## Limitations Worth Stating


The authors are explicit about the boundaries of this work, and the findings are more useful when those boundaries are respected.


It was a descriptive study, not a controlled comparison. It did not test whether AI videos outperform human-recorded videos or written documentation. The 86% quiz average shows learning happened, not that it happened better.


The students were enrolled in upper-level CS courses. Even though most had little Markdown experience, they had substantial background in programming and technical notation, which likely made the material easier to absorb. Results may differ for introductory learners.


The content was simple and short. Three-minute videos on basic syntax are close to the ideal case for this format, and the students themselves flagged complex material as a poor fit. Nothing here says AI video handles advanced topics well.


The qualitative themes come from one coding process and are open to other interpretations, which the authors acknowledge and address by publishing their codebook process and representative quotes.


## Disclosure


Knowlify provided the research team with early access to the platform and complimentary video credits for this study. As stated in the paper's acknowledgements, Knowlify had no role in the study design, data collection, analysis, interpretation, or the decision to publish. We are writing about this research because it is independent, and we think the parts that are unflattering to AI video are as interesting as the parts that are not.


## Key Takeaways


- 170 computing students at two US universities watched three AI-generated instructional videos built with Knowlify and rated them 4.52 out of 5 for production quality, 4.43 for accuracy, and 4.39 for helpfulness.
- Half the students could not clearly tell the videos were AI-generated, the highest-variance item in the survey.
- Students averaged 86% on a post-viewing knowledge test despite most having little or no prior familiarity with the topic.
- Support drops sharply at substitution: 68% would not prefer AI video over instructor-recorded video and 62% would not trust it equally.
- Students endorsed AI video for simple topics (27%), visual topics (15%), summaries (12%), and supplemental material (12%).
- Accuracy is the dominant concern at 58%, named more than four times as often as the next issue.
- Removing AI avatars appears to be a significant reason the videos were received well, since prior research found synthetic presenters distracting and uncanny.
- Each three-minute video took at most 25 minutes of combined design, generation, and human verification time.


---


## FAQ


### Do students like AI-generated instructional videos?


Yes, when the videos supplement rather than replace instruction. In a 2026 study of 170 computing students, 92% agreed the AI-generated videos were professionally produced, 92% said the videos helped them understand the material, and 88% said the content was accurate. However, only 10% said they would prefer AI video over videos recorded by their own instructor, so approval is conditional on the videos being an addition rather than a substitution.


### Can students tell if a video is AI-generated?


Often not. In the 2026 study, only 50% of upper-level computer science students agreed they could clearly tell the videos were AI-generated, while 35% disagreed and 15% were neutral. This was the only item in the survey without consensus. The videos used animated visual walkthroughs rather than AI avatars, which the researchers suggest is a key reason detection was so difficult, since synthetic faces and voices are the usual giveaway.


### Do students learn as well from AI-generated videos?


Students in this study learned the material well, averaging 4.3 out of 5 (roughly 86%) on a knowledge test after watching, despite most reporting little or no prior familiarity with the topic. Platform logs confirmed they watched 76% to 81% of each video on average. Important caveat: the study was descriptive rather than comparative, so it measured short-term knowledge acquisition and did not test AI video against human-recorded video or written documentation.


### What topics work best for AI-generated educational videos?


Students identified four categories. Simple or foundational topics such as vocabulary and basic syntax were named by 27% of students. Topics that benefit from visualization, such as data structures and step-by-step processes, were named by 15%. Topic summaries and exam review were named by 12%, and supplemental explanations that reinforce a concept a second way were also named by 12%. Students specifically flagged complex, subjective, and interactive topics as poor fits.


### What are students most concerned about with AI videos in education?


Accuracy, by a wide margin. 58% of students raised concerns about inaccurate or hallucinated information, more than four times the rate of the next concern. After that came lack of depth (13%), lack of engagement (13%), reduced interaction with instructors (10%), and the sense that AI video undermines the value of tuition (10%). Only 6% of students reported no concerns at all.


### Should AI videos replace lectures?


The students in this study said no, clearly. 68% disagreed that they would prefer AI videos over instructor-recorded videos, 62% disagreed that they would trust AI video equally, and 16% specifically named replacing instructor-led lectures as an inappropriate use. The researchers conclude that AI videos can play a meaningful role in education without displacing the instructor presence that students value.


### How long does it take to make an AI instructional video?


In this study, each three-minute video took at most 25 minutes end to end. That breaks down as 8 to 11 minutes of planning, reviewing, and configuring, 3 minutes 48 seconds of average system generation time, and about 10 minutes of human verification. The researchers generated the videos by uploading GitHub's official Markdown documentation as a PDF to[Knowlify](https://create.knowlify.com/) and using a single prompt.


### Why did the researchers avoid AI avatars in the videos?


To remove social presence as a confounding variable. Prior research had found students perceived AI-generated appearances and voices as distracting and uncanny, even when the script was identical to a human-produced version, which made it impossible to separate reactions to AI instruction from reactions to synthetic humans. By using animated visual walkthroughs with no talking heads, this study isolated the question of whether AI-generated instruction works. The high production-quality ratings suggest the choice mattered.


---


## References


1. [Knowlify](https://knowlify.com/)
2. [learning science principles](https://knowlify.com/articles/learning-science-principles)
3. [Student Perceptions and Preferences Regarding AI-Generated Instructional Videos in Computing Education](https://arxiv.org/abs/2607.28203)
4. [turning technical documentation into video](https://knowlify.com/articles/ai-video-technical-documentation-developer-experience)
5. [training video libraries](https://knowlify.com/articles/best-ai-video-tools-training-education)
6. [new-hire onboarding programs](https://knowlify.com/articles/ai-onboarding-videos-scale-new-hire-training)
7. [Knowlify app](https://create.knowlify.com/)
8. [measuring the ROI of AI video](https://knowlify.com/articles/measure-roi-ai-video-enterprise-learning-development)
