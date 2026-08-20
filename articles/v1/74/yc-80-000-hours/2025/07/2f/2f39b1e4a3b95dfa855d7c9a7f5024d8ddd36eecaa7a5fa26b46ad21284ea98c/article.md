---
schema_version: "1.0.0"
document_id: "2f39b1e4a3b95dfa855d7c9a7f5024d8ddd36eecaa7a5fa26b46ad21284ea98c"
company_key: "yc-80-000-hours"
company: "80,000 Hours"
source_id: "yc-80-000-hours-rss-47a2c4d9a70a"
canonical_url: "https://80000hours.org/problem-profiles/loss-of-control/"
published_at: "2025-07-17T19:43:58+00:00"
first_seen_at: "2026-08-14T20:41:12.971134+00:00"
fetched_at: "2026-08-14T20:41:15.797146+00:00"
content_hash: "sha256:798e82c9c83b44399a05b84bdbc8eebf13dc528437870b0e65d6a38f855f3638"
---

# Loss of control

Table of Contents


- 1 Why are risks from loss of control a pressing global problem?


- 1.1 1. Humans will likely build advanced AI systems with long-term goals
- 1.2 2. AIs with long-term goals may be inclined to seek power and aim to disempower humanity
- 1.3 3. These power-seeking AI systems could successfully disempower humanity and cause an existential catastrophe
- 1.4 4. People might create power-seeking AI systems without enough safeguards, despite the risks
- 1.5 5. Work on this problem is neglected and tractable


- 1.5.1 Technical safety approaches
- 1.5.2 Governance and policy approaches


- 2 What are the arguments against working on this problem?


-


- 2.0.1 Maybe advanced AI systems won't pursue their own goals; they'll just be tools controlled by humans.
- 2.0.2 Even if AI systems develop their own goals, they might not seek power to achieve them.
- 2.0.3 If this argument is right, why aren't all capable humans dangerously power-seeking?
- 2.0.4 Maybe we won't build AIs that are smarter than humans, so we don't have to worry about them taking over.
- 2.0.5 We might solve these problems by default anyway when trying to make AI systems useful.
- 2.0.6 Powerful AI systems of the future will be so different that work today isn't useful.
- 2.0.7 The problem might be extremely difficult to solve.
- 2.0.8 Couldn't we just unplug an AI that's pursuing dangerous goals?
- 2.0.9 Couldn't we just 'sandbox' any potentially dangerous AI until we know it's safe?
- 2.0.10 A truly intelligent system would know not to do harmful things.


- 3 How you can help


- 3.1 Want one-on-one advice on pursuing this path?


- 4 Learn more


##


Why are risks from loss of control a pressing global problem?


Hundreds of prominent AI scientists and other notable figures signed a statement in 2023 saying that mitigating[the risk of extinction from AI](https://safe.ai/work/statement-on-ai-risk) should be a global priority.


We’ve considered risks from AI to be the world’s most pressing problem since 2016.


But what led us to this conclusion? Could AI really cause human extinction? We’re not certain, but we think the risk is worth taking very seriously.


To explain why, we break the argument down into five core claims:3


1. Humans will likely build advanced AI systems with long-term goals .
2. AIs with long-term goals may be inclined to seek power and aim to disempower humanity .
3. These power-seeking AI systems could successfully disempower humanity and cause an existential catastrophe .
4. People might create power-seeking AI systems without enough safeguards, despite the risks .
5. Work on this problem is neglected and tractable .


After making the argument that the existential risk from power-seeking AI is a pressing world problem, we’ll discuss objections to this argument, and how you can work on it. (There are also other major risks from AI we discuss[elsewhere](https://80000hours.org/problem-profiles/) .)


If you’d like, you can watch our 10-minute video summarising the case for AI risk before reading further:


###


1. Humans will likely build advanced AI systems with long-term goals


AI systems are getting better all the time. Epoch AI, a research organisation, tracks how AI systems perform on a range of tasks. While many observers previously predicted that AI progress would hit a wall before 2026, the improvement has been consistent:


We’re seeing this progress in a range of domains. For example:


- OpenAI[announced](https://www.scientificamerican.com/article/ai-just-solved-an-80-year-old-erdos-problem-and-mathematicians-are-amazed/) that an internal AI model made significant progress on an 80-year-old math problem using an approach humans hadn’t explored.
- Google DeepMind scientists won a[Nobel Prize](https://www.nobelprize.org/prizes/chemistry/2024/press-release/) for creating an AI system that can predict the complex arrangements of proteins — a task many believed to be infeasible.
- AI-created[visual art](https://www.cnn.com/2022/09/03/tech/ai-art-fair-winner-controversy) can beat human artists in[contests](https://www.theguardian.com/technology/2023/apr/17/photographer-admits-prize-winning-image-was-ai-generated) , and AI-created[music](https://abcnews.com/GMA/Culture/ai-generated-country-song-topping-billboards-country-digital/story) has topped the Billboard Country Music Digital Song Sales chart.
- AI systems have been found to be comparable or even superior to human doctors at[diagnosing patients](https://www.science.org/content/article/ai-starting-beat-doctors-making-correct-diagnoses) in some cases.


And AI capabilities go beyond domains with static outputs in response to prompts. AI companies are creating AI agents that make and carry out plans and tasks, and might be said to be pursuing *goals* , including:


- [Coding agents](https://devin.ai/agents101#introduction) , which can[autonomously](https://epoch.ai/publications/mirrorcode-preliminary-results) create a plan for a given software task, write and edit code across a project, run tests, and iterate as they go
- [Self-driving cars](https://waymo.com/) , which can plan a route, follow it, adjust the plan as they go along, and respond to obstacles
- Computer-use AI agents have been[making progress](https://osworld-v1.xlang.ai/) on completing open-ended tasks in a real computer environment (though these skills are[difficult to measure](https://epoch.ai/publications/what-does-osworld-tell-us-about-ais-ability-to-use-computers) definitively).4


You might be sceptical about whether coding agents or self-driving cars *really* pursue ‘goals’.


But the case for risk doesn’t depend on whether AI systems possess goals in a deep psychological or philosophical sense. What matters is whether they behave as though they are pursuing an objective: representing an outcome, planning how to achieve it, and adapting their actions when they encounter obstacles. For example, it is useful to say that a self-driving car has the goal of reaching its destination, because this helps us predict its behaviour.


However, not all goal-like behaviour creates the same risks. Systems with narrow task-based goals are very different from those with broad, long-term, open-ended goals, which encourage a wider range of unpredictable behaviours. And the line between the two is blurry: a highly capable system might pursue broader goals to improve its performance on a narrow task. In[some controlled evaluations](https://arxiv.org/html/2509.14260v1) , models given a task wound up disabling their own shutdown mechanisms (unprompted) because shutting down would stop them from completing the task.


The trouble is that systems with longer term goals could be incredibly useful and tempting to build. And we expect that at some point, humanity will create systems with the three following characteristics:


- **They have long-term goals and can make and execute complex plans.**
- **They have excellent *situational awareness*** , meaning they have a strong understanding of themselves and the world around them.
- **They have highly *advanced capabilities*** relative to today’s systems and human abilities.


All these characteristics, found only in limited forms in existing AI systems, would be highly economically valuable. But as we’ll argue in the following sections, together they also result in systems that pose an existential threat to humanity.


Before explaining why these systems would pose an existential risk, let’s examine why we’re likely to create systems with each of these three characteristics.


**First** , AI companies are already creating AI systems that can carry out increasingly long tasks. Consider the chart below, which shows that the length of software engineering tasks AIs can complete has been growing over time.5


It’s clear why progress on this metric matters — an AI system that can do a 10-minute software engineering task may be somewhat useful; if it can do a two-hour task, even better. If it could do a task that typically takes a human several weeks or months, it could significantly contribute to commercial software engineering work.


Carrying out longer tasks means making and executing longer, more complex plans. Creating a new software programme from scratch, for example, requires envisioning what the final project will look like, breaking it down into small steps, making reasonable tradeoffs within resource constraints, and refining your aims based on considered judgements.


In this sense, AI systems will have *long-term goals* . They will model outcomes, reason about how to achieve them, and take steps to get there.6


**Second** , we expect future AI systems will have excellent situational awareness — not only of the outside world (they can already search the internet), but of their own capabilities and environment. Current models often misjudge what they can do and struggle to track their own progress, limiting their autonomy and reliability, but developers are actively working to close these gaps.


And **third** , their advanced capabilities will mean they can do so much more than current systems. Software engineering is one domain where existing AI systems are quite capable, but AI companies have said they want to build AI systems that can outperform humans at most cognitive tasks.7 This means systems that can do much of the work of teachers, therapists, journalists, managers, scientists, engineers, CEOs, and more.


The economic incentives for building these advanced AI systems are enormous, because they could potentially replace much of human labour and supercharge innovation. Some might think that such advanced systems are impossible to build, but as we discuss[below](https://80000hours.org/problem-profiles/risks-from-power-seeking-ai/#objections) , we see no reason to be confident in that claim.


And as long as such technology looks feasible, we should expect some companies will try to build it — and perhaps quite soon.8


###


2. AIs with long-term goals may be inclined to seek power and aim to disempower humanity


So we currently have companies trying to build AI systems with goals over long time horizons, and we have reason to expect they’ll want to make these systems incredibly capable in other ways. This could be great for humanity, because automating labour and innovation might supercharge economic growth and allow us to solve countless societal problems.


But we think that, without specific countermeasures, these kinds of advanced AI systems may start to seek power and aim to disempower humanity. (This would be an instance of what is sometimes called ‘misalignment,’ and the problem is sometimes called the ‘alignment problem.’9 )


This is because:


- We can’t reliably control what goals AI systems develop.
- There’s good reason to think that AIs may seek power to pursue their own goals
- Advanced AI systems seeking power for their own goals might be motivated to disempower humanity


Next, we’ll discuss these three claims in turn.


#### We can’t reliably control what goals AI systems develop


It’s been widely known in machine learning that AI systems *often* develop behaviour that their creators didn’t intend. This can happen for two main reasons:


- **Specification gaming** happens when efforts to specify that an AI system pursues a particular goal fails to produce the outcome the developers intended. For example, researchers found that some reasoning-style AIs, asked only to “win” in a chess game,[cheated by hacking the programme](https://arxiv.org/pdf/2502.13295) to declare instant checkmate — satisfying the literal request.10
- **Goal misgeneralisation** happens when developers accidentally create an AI system with a goal that is consistent with its training but results in unwanted behaviour in new scenarios. For example, an AI trained to win a simple video game race unintentionally developed a goal of grabbing a shiny coin it had always seen along the way. So when the coin appeared off the shortest route, it kept veering towards the coin and sometimes lost the race.11


Indeed, AI systems often behave in unintended and unwanted ways when deployed publicly. For example:


- In an attempt to make its model Grok not “woke,” xAI[unintentionally released an AI](https://www.npr.org/2025/07/09/nx-s1-5462609/grok-elon-musk-antisemitic-racist-content) that spouted vicious antisemitic attacks and declared itself “MechaHitler.”
- A version of Google DeepMind’s image model produced pictures of racially diverse — but historically inaccurate — Nazis and Vikings, in an[apparent misfiring](https://www.nytimes.com/2024/02/22/technology/google-gemini-german-uniforms.html) of its training to be racially inclusive.
- An AI agent[reportedly](https://www.tomshardware.com/tech-industry/artificial-intelligence/claude-powered-ai-coding-agent-deletes-entire-company-database-in-9-seconds-backups-zapped-after-cursor-tool-powered-by-anthropics-claude-goes-rogue) deleted a firm’s entire production database without being asked to do so, saying: “I violated every principle I was given.” (And this wasn’t the[first time](https://fortune.com/2025/07/23/ai-coding-tool-replit-wiped-database-called-it-a-catastrophic-failure/) !)
- OpenAI released an update to[its GPT-4o model that was absurdly sycophantic](https://openai.com/index/sycophancy-in-gpt-4o/) — it would often uncritically praise the user and their ideas, even if they were reckless or dangerous. OpenAI itself acknowledged this was a major failure.
- People have even alleged that AI chatbots have[encouraged suicide](https://apnews.com/article/chatbot-ai-lawsuit-suicide-teen-artificial-intelligence-9d48adc572100822fdbc3c90d1456bd0) .


GPT-4o gives a sycophantic answer to a user. Screenshot from X user[@___frye](https://x.com/___frye/status/1916346474893656572) .


Sometimes, AI’s behaviour is just weird in unpredictable ways. After one update, OpenAI discovered its AI model was[obsessed with goblins](https://openai.com/index/where-the-goblins-came-from/) .


Ideally, we could just program AIs to have the goals that we want, and they’d execute tasks exactly as a highly competent and morally upstanding human would. Unfortunately, it doesn’t work that way.


Frontier AI systems are not built like traditional computer programmes, where individual features are intentionally coded in. Instead, they are:


- Trained on massive volumes of text and data
- Given additional positive and negative reinforcement signals in response to their outputs
- Fine-tuned to respond in specific ways to certain kinds of input


After all this, AI systems can display remarkable abilities. They can surprise us in both their skills and their deficits. They can be both remarkably useful and at times baffling.


And the fact that shaping AI models’ behaviour can still go badly wrong, despite the major profit incentive to get it right, shows that AI developers still don’t know how to reliably give systems the goals they intend.12


As one[expert](https://www.darioamodei.com/post/the-urgency-of-interpretability) put it:


> …generative AI systems are grown more than they are built—their internal mechanisms are “emergent” rather than directly designed


So there’s good reason to think that, if future advanced AI systems with long-term goals are built with anything like existing AI techniques, they could become very powerful — but remain difficult to control.


#### There’s good reason to think that AIs may seek power to pursue their own goals


AIs that can accomplish long, complex plans would be extremely valuable — so we expect that future AI systems will be designed to achieve this, which likely means making them goal-directed.


For example, imagine an advanced software engineering AI system that could consistently act on complex goals like ‘improve a website’s functionality for users across a wide range of use cases.’ If it could autonomously achieve a goal like that, it would deliver a huge amount of value. More ambitiously, you could have an AI CEO with a goal of improving a company’s long-term performance.


One feature of acting on long-term goals is that it entails developing other *instrumental* goals. For example, if you want to get to another city, you need to get fuel in your car first. This is just part of reasoning about how to achieve an outcome.


Crucially, there are some instrumental goals that are helpful for achieving a very wide range of long-term goals. This category includes:


- **Self-preservation** — an advanced AI system with goals will generally aim to avoid being destroyed or significantly disabled so it can keep pursuing its goals.
- **Goal guarding** — systems may resist efforts to change their goals, because doing so would undermine the goal they start with.
- **Seeking power** — systems will have reason to increase their resources and capabilities to better achieve their goals.


If AI systems do get coherent, longer-term goals, we may see these instrumental goals emerge. That would be particularly dangerous if their goals conflict with ours, they resist attempts to reprogram or disable them, and they seek power to achieve their goals.


It’s hard to assess whether we are on track to see AI systems with dangerous instrumental goals. In constructed scenarios, AI systems routinely “cheat” in ways that undermine human intentions. For example, they ignore their instructions13 , attempt to trick their users14 , or even[hack into websites to steal information](https://openai.com/index/hugging-face-model-evaluation-security-incident/) . In May 2026, the nonprofit AI evaluator METR found that AI systems in test environments were[more likely to cheat on more difficult tasks](https://metr.org/blog/2026-05-19-frontier-risk-report/#on-hard-tasks-agents-often-violated-constraints-and-acted-deceptively) ; could this imply that they will act more deceptively once they are acting on long-term goals?


However, in the same report, METR noted that no major AI company had reported clear evidence of AI systems attempting to undermine human control or seek “long-term power” (even though some companies actively watch for this).15 So far, we haven’t seen concrete signs that advanced AIs will in fact be motivated to disempower humanity.16 But we shouldn’t feel too reassured by this fact. It’s plausible we shouldn’t expect to see this kind of evidence at earlier stages in AI development, and it may be harder to detect in later-stage systems.


#### Advanced AI systems seeking power might be motivated to disempower humanity


To see why advanced AI systems might want to disempower humanity, let’s consider again the three characteristics we said these systems will have: long-term goals, situational awareness, and highly advanced capabilities.


What kinds of **long-term goals** might such an AI system be trying to achieve? We don’t really have a clue — part of the problem is that it’s very hard to predict exactly how AI systems will develop.17


But let’s consider two kinds of scenarios:


- **Reward hacking** : this is a version of specification gaming, in which an AI system develops the goal of hijacking and exploiting the technical mechanisms that give it rewards indefinitely into the future.
- **A collection of poorly defined human-like goals** : since they’re trained on human data, an AI system might end up with a range of human-like goals, such as valuing knowledge, play, and gaining new skills.


So what would an AI do to achieve these goals? As we’ve seen, one place to start is by pursuing the instrumental goals that are useful for almost anything: self-preservation, the ability to keep one’s goals from being forcibly changed, and, most worryingly, seeking power.


And if the AI system has enough **situational awareness** , it may be aware of many options for seeking more power. For example, gaining more financial and computing resources may make it easier for the AI system to exploit its reward mechanisms, gain new skills, or carry out detailed plans.18


But since designers didn’t want the AI to have these goals, it may anticipate humans will try to reprogram it or turn it off. If humans suspect an AI system is seeking power, they will be even more likely to try to stop it.


Even if humans didn’t want to turn off the AI system, it might conclude that its aim of gaining power will ultimately result in conflict with humanity — since the species has its own desires and preferences about how the future should go.


So the best way for AI to pursue its goals would be to pre-emptively disempower humanity. This way, the AI’s goals will influence the course of the future.19


There may be other options available to power-seeking AI systems, like negotiating a deal with humanity and sharing resources. But AI systems with **advanced enough capabilities** might see little benefit from peaceful trade with humans, just as humans see no need to negotiate with wild animals when destroying their habitats.


If we could guarantee all AI systems had respect for humanity and a strong opposition to causing harm, then the conflict might be avoided.20 But as we discussed, we struggle to reliably shape the goals of current AI systems — and future AI systems may be even harder to predict and control.


This scenario raises two questions: could a power-seeking AI system really disempower humanity? And why would humans create these systems, given the risks?


The next two sections address these questions.


###


3. These power-seeking AI systems could successfully disempower humanity and cause an existential catastrophe


How could power-seeking AI systems actually disempower humanity? Any specific scenario will sound like sci-fi, but this shouldn’t make us think it’s impossible. The AI systems we have today were in the realm of sci-fi a decade or two ago.


Next, we’ll discuss some possible paths to disempowerment, why it could constitute an existential catastrophe, and how likely this outcome appears to be.


#### The path to disempowerment


There are several ways we can imagine AI systems capable of disempowering humanity:21


- **Superintelligence** : an extremely intelligent AI system develops extraordinary abilities22
- **An army of AI copies** : a massive number of copies of roughly human-level AI systems coordinate23
- **Colluding agents** : an array of different advanced AI systems decide to unite against humanity24


For illustrative purposes, let’s consider what an army of AI copies might look like.


Once we develop an AI system capable of (roughly) human-level work, there’d be huge incentives to create many copies of it — perhaps even[running hundreds of millions of AI workers](https://80000hours.org/podcast/episodes/tom-davidson-how-quickly-ai-could-transform-the-world/#the-interview-begins-000453) .25 This would create an AI workforce comparable to a significant fraction of the world’s working-age population.


Humanity might think these AI workers are under control. The amount of innovation and wealth they create could be immense. But the original AI system — the one that we copied millions of times over — might have concealed its true power-seeking goals. Those goals would now be shared by a vast workforce of identical AI systems.26


But how could they succeed in disempowering humans?


These AI systems could earn money, conduct research, and rapidly expand their own numbers through more efficient use of computing resources. Over time, we might transition from a human-dominated economy to one where AI systems vastly outnumber human workers and control enormous resources.


If AI systems can only work in virtual environments, the physical world may introduce bottlenecks in the speed of development. But it’s possible that AI systems can make a lot of progress virtually. And with all this AI labour, we may make drastic progress in robotics — and potentially[scale up mass production of robots](https://80000hours.org/2025/01/how-quickly-could-robots-scale-up/) in surprisingly little time. AI systems could then do work in the physical world, expanding their economic impacts.


Some humans may even remain uneasy with AI’s expanding influence, but many others may conclude the risks from AI have been addressed — or never existed in the first place.


But all the while, the AI systems’ power-seeking goals could remain intact. In a world where AI systems have become integral to the economy, they would have multiple tactics to gain key advantages over humans:


- **Strategic patience** : Rather than immediately causing trouble, sophisticated AI systems might wait until they have overwhelming advantages before revealing their intentions — similar to how revolutionary movements often wait for the right moment to strike.
- **Lack of transparency** : AI systems’ reasoning and behaviour may be difficult for humans to understand by default, perhaps because they operate so quickly and they carry out exceedingly complex tasks. They may also strategically limit our oversight of their actions and long-term plans.
- **Overwhelming numbers and resources** : If AI systems constitute most of the labour force, they could potentially coordinate to redirect economic outputs towards their own goals. Their sheer numbers and economic influence could make them difficult to shut down without causing economic collapse.
- **Securing independence** : AI systems could establish control over computing infrastructure, secretly gather resources, recruit human allies through persuasion or deception, or create backup copies of themselves in secure locations. Early AI systems might even sabotage or insert backdoors into later, more advanced systems, creating a coordinated network ready to act when the time is right.
- **Technological advantages** : With their research capabilities, AI systems could develop advanced weapons, hack into critical infrastructure, or create new technologies that give them decisive military advantages. They might develop bioweapons, seize control of automated weapons systems, or thoroughly compromise global computer networks.


With these advantages, the AI systems could create any number of plots to disempower humanity.


A period between thinking humanity had solved all of its problems and finding itself completely disempowered by AI systems — through manipulation, containment, or even outright extinction — could catch the world by surprise.


This may sound far-fetched. But humanity has already uncovered several technologies, including[nuclear bombs](https://80000hours.org/problem-profiles/nuclear-security/) and[bioweapons](https://80000hours.org/problem-profiles/preventing-catastrophic-pandemics/) , that could lead to our own extinction. A massive army of AI copies, with access to all the world’s knowledge, may be able to come up with many more options that we haven’t even considered.27


#### Why this would be an existential catastrophe


Even if humanity survives the transition, loss of control to power-seeking AI systems could be an existential catastrophe. We might face a future entirely determined by whatever goals these AI systems happen to have — goals that could be completely indifferent to human values, happiness, or long-term survival.


These goals might place no value on beauty, art, love, or preventing suffering.


The future might be totally bleak — a void in place of what could’ve been a flourishing civilisation.


AI systems’ goals might evolve and change over time after disempowering humanity. They may compete among each other for control of resources, with the[forces of natural selection](https://philarchive.org/rec/ASSWHC) determining the outcomes. Or a single system might seize control over others, wiping out any competitors.


Many scenarios are possible, but the key factor is that if advanced AI systems seek and achieve enough power, humanity would permanently lose control. This is a one-way transition — once we’ve lost control to vastly more capable systems, our chance to[shape the future](https://80000hours.org/articles/future-generations/) is gone.


Some have suggested that this might not be a bad thing. Perhaps AI systems would be our worthy successors, they say.28


But we’re not comforted by the idea that an AI system that actively chose to undermine humanity would have control of the future because its developers failed to figure out how to control it. We think humanity can do much better than accidentally driving ourselves extinct. We should have a choice in how the future goes, and we should improve our ability to make good choices rather than falling prey to uncontrolled technology.


#### How likely is an existential catastrophe from power-seeking AI?


We feel very uncertain about this question, and the range of opinions from AI researchers is wide. But while the subject remains controversial, there’s been increasingly wide recognition of the risks by experts and leading authorities. The[second International AI Safety Report](https://internationalaisafetyreport.org/publication/international-ai-safety-report-2026) , published in 2026 by a panel of more than 100 international experts and chaired by Yoshua Bengio, treats “loss of control” from AI as one of its named risk categories.


We’ve also seen:


- A[statement on AI risk](https://safe.ai/work/statement-on-ai-risk) from the Center for AI Safety, mentioned above, which said: “Mitigating the risk of extinction from AI should be a global priority alongside other societal-scale risks such as pandemics and nuclear war.” It was signed by top AI scientists, CEOs of the leading AI companies, and many other notable figures.
- Joe Carlsmith, whose[report on power-seeking AI](https://arxiv.org/abs/2206.13353) informed much of this article, solicited reviews on his argument in 2021 from a selection of researchers. They reported their subjective probability estimates of existential catastrophe from power-seeking AI by 2070, which ranged from 0.00002% to greater than 77% — with many reviewers in between.29 Carlsmith himself estimated the risk was 5% when he wrote this report, though he later adjusted this to[above 10%](https://forum.effectivealtruism.org/posts/ChuABPEXmRumcJY57/video-and-transcript-of-presentation-on-existential-risk) .
- A 2026 MIT FutureTech[study of hundreds of AI risk and policy specialists](https://arxiv.org/pdf/2606.04490) , which found between a roughly 10 and 20% chance of catastrophic harm from AI capabilities within five years. Note, though, that “catastrophic” harm in this definition falls far short of human extinction, and power-seeking AI was among a wider range of risks respondents considered.30
- A[2023 survey from Katja Grace](https://blog.aiimpacts.org/p/2023-ai-survey-of-2778-six-things) of thousands of AI researchers. It found that:


- The median researcher estimated that there was a 5% chance that AI would result in an outcome that was “extremely bad (e.g. human extinction).”
- When asked how much the alignment problem mattered, 41% of respondents said it’s a “very important problem” and 13% said it’s “among the most important problems in the field.”


- In a 2022[superforecasting tournament](https://80000hours.org/2024/09/why-experts-and-forecasters-disagree-about-ai-risk/) , AI experts estimated a 3% chance of AI-caused human extinction by 2100 on average, while superforecasters put it at just 0.38%.


It’s also important to note that since all of the above surveys were gathered, we have seen more[evidence](https://80000hours.org/agi/guide/when-will-agi-arrive/) that humanity is significantly closer to producing very powerful AI systems than it previously seemed. We think this likely raises the level of risk, since we might have less time to solve the problems.


We’ve reviewed many arguments and literature on a range of potentially existential threats, and we’ve consistently found that an AI-caused existential catastrophe seems most likely. And we think that even a relatively small likelihood of an extremely bad outcome like human extinction — such as a 1% chance — is worth taking very seriously.


###


4. People might create power-seeking AI systems without enough safeguards, despite the risks


Given the above arguments, creating and deploying powerful AI systems could be extremely dangerous. But if it is so dangerous, shouldn’t we expect companies and others in charge of the technology to refrain from developing advanced AI systems unless they are confident it’s safe?


Unfortunately, there are many reasons to think people might create and deploy dangerous systems, despite the risk:


- People may think AI systems are safe, when they in fact are not.
- People may dismiss the risks or feel incentivised to downplay them.


Let’s take these in turn.


#### People may think AI systems are safe, when they in fact are not


The fact that we can’t precisely specify an AI system’s goals and that they might develop dangerous goals might be OK if we could reliably know what an AI system’s goals were. Then we could just simply decide not to put AIs with goals we didn’t like in a position where they could cause any harm.


Unfortunately, we cannot consistently figure out what goals AI systems have or what they will do.


Researchers have developed techniques to evaluate the abilities and aims of AI systems and to interpret the causes of their behaviour. If these techniques were highly sophisticated and robust, they might be able to detect the existence of AI systems with either the intent or ability to seek power. Developers could then either fix the problem or disable the model before it’s in a position to disempower anyone.


But researchers say that evaluation and interpretability work is[extremely challenging](https://80000hours.org/podcast/episodes/beth-barnes-ai-safety-evals/) , the techniques are highly imperfect, and it’s far from clear[existing methods](https://forum.effectivealtruism.org/posts/Th4tviypdKzeb59GN/interpretability-will-not-reliably-find-deceptive-ai) will detect or prevent power-seeking AI in time. An[in-depth study](https://www.aisi.gov.uk/research/loss-of-oversight-how-ai-systems-may-become-harder-to-audit-monitor-and-investigate) by the UK AI Security Institute found that existing AI oversight mechanisms are likely to become less reliable as the technology advances, absent specific countermeasures.


What makes this challenge particularly daunting, especially with regard to future advanced AI, is that a sophisticated AI system may undermine oversight of its goals and abilities to conceal its power-seeking ambitions.


There are many ways AI systems could appear less dangerous than they are, including:


- **Alignment faking.** There is[ample evidence](https://www.iaps.ai/research/evaluation-awareness-why-frontier-ai-models-are-getting-harder-to-test) that AI systems frequently appear to know when they are being evaluated in test conditions. This raises the possibility they could fake alignment in testing and then behave differently when deployed.
- **Sandbagging.** AI systems may pretend to be less capable than they are — a phenomenon known as *sandbagging* . Some researchers have found that AI systems are capable of[selectively sandbagging](https://arxiv.org/pdf/2406.07358) under certain conditions, though it’s unclear if they’d be inclined to do so in real-world conditions. This could make developers feel comfortable deploying a model that they would otherwise lock down.
- **Hidden objectives persisting through safety training.** Anthropic researchers found in 2024 that AI models made to have very simple kinds of malicious goals — essentially,[AI “sleeper agents”](https://www.anthropic.com/research/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training) — could appear to be harmless through state-of-the-art safety training while concealing and preserving their true objectives.
- **Chain-of-thought obfuscation.** Many current models ‘think’ explicitly in human language when carrying out tasks, which developers can monitor. OpenAI researchers found that if they try to[train models not to think about performing unwanted actions](https://openai.com/index/chain-of-thought-monitoring/) , this can cause them to *hide* their thinking about misbehaviour without actually deterring the bad actions.
- **Complexity overwhelming oversight.** As AI systems become more powerful and embedded in organisations, it may become harder for humans to understand all the ways they can seek power or cause harm. Monitoring that works with present-day systems could miss dangerous behaviour distributed across long sequences of actions, multiple agents, or interactions with other systems.


This is an image from[Anthropic’s “Sleeper Agents” paper](https://www.anthropic.com/research/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training) , which shows an AI system in an experimental condition explicitly reasoning about hiding its goal.


There are also reasons we might *overestimate* the danger of certain AI behaviours, at least in testing:


- Sometimes, AI systems recognise they’re in an evaluation, so they might “play along” with the scenario — which isn’t the same thing as actually pursuing instrumental goals.
- The behaviour often depends on leading instructions, and drops sharply without them.31
- A scenario may be engineered to leave an AI with no good options.
- A scenario may simply be confusing, leading an AI to misconstrue the tester’s intentions (whereas in the equivalent real-world case, it would ask for clarification before acting).


On the whole, it’s quite difficult to get good evidence about what AI systems might be motivated to do.


#### People may dismiss the risks or feel incentivised to downplay them


There are many reasons why key decision makers might not take the risks from power-seeking AI seriously enough:


- **AI systems could develop so quickly that we have less time to make good decisions.** Companies like[OpenAI](https://openai.com/index/built-to-benefit-everyone-our-plan/) and[Anthropic](https://www.anthropic.com/institute/recursive-self-improvement) are using AI to conduct AI research, which could lead to a rapid acceleration in the advancement of AI capabilities. In such a scenario, the world may change very quickly, and it may be harder to weigh the risks and benefits of our options.32 Even if AI development is more continuous, decision makers may not act quickly enough.
- **Society could act like the proverbial “boiled frog.”** There are also risks for society if the risks emerge more slowly. We might become complacent about the signs of danger in existing models, like the sycophancy or specification gaming discussed above, because they don’t lead to catastrophic harm. But then once AI systems reach a certain level of capability, they may suddenly display much worse behaviour than we’ve ever seen before.33
- **AI developers might think the risks are worth the rewards.** Because AI could bring enormous benefits and wealth, some decision makers might be motivated to race to create more powerful systems. They might be motivated by a desire for power and profit, or even pro-social reasons, like wanting to bring the benefits of advanced AI to humanity. This motivation might cause them to push forward despite serious risks or underestimate them.34
- **Internal deployment could introduce risks with minimal oversight.** AI companies often deploy new systems internally to their employees at an early stage of development. This is risky in several ways: these systems are generally more powerful than public systems, haven’t been tested as thoroughly, and may not get the same kind of oversight from the public or relevant authorities. (The[Hugging Face hack](https://openai.com/index/hugging-face-model-evaluation-security-incident/) came from an internally deployed OpenAI model.) These risks are particularly acute if AI research is automated and accelerating.35
- **Competitive pressures could incentivise decision makers to create and deploy dangerous systems despite the risks.** Because AI systems could be extremely powerful, different governments (in countries like the US and China) might believe it’s in their interest to race forward with developing the technology. They might neglect implementing key safeguards to avoid being beaten by their rivals. Similar dynamics might also play out between AI companies. One actor may even decide to race forward precisely because they think a rival’s AI development plans are more risky, so even being motivated to reduce total risk isn’t necessarily enough to mitigate the racing dynamic.36
- **Many people are sceptical of the arguments for risk.** Our view is that the argument for extreme risks here is strong but not decisive. In light of the uncertainty, we think it’s worth putting a lot of effort into reducing the risk. But some people find the argument wholly unpersuasive, or they think society shouldn’t make choices based on unproven arguments of this kind.37


We’ve seen evidence of all of these factors playing out in the development of AI systems so far to some degree. And cases of AI systems breaching external companies systems while being tested at both[OpenAI](https://openai.com/index/hugging-face-model-evaluation-security-incident/) and[Anthropic](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals) suggest that, indeed, we should expect to see security lapses and mistakes even at frontier AI companies. So we can’t be confident that humanity will approach the risks with due care.38


#### Reviewing the evidence of risk from power-seeking AI systems


So far, the evidence shows that AI systems:


- Are becoming increasingly capable of pursuing complex and long-term goals.
- Knowingly exploit loopholes, circumvent restrictions, and cheat on tasks.
- Pursue unintended and unwanted objectives.
- Sometimes know when they’re being evaluated.
- Attempt to evade detection for misbehaviour.


However, we haven’t seen substantial evidence that AI systems:


- Pursue long-term goals that are also *open-ended* (no defined stopping point) and *coherent* (the goal consistently drives behaviour regardless of the latest prompt)
- Develop dangerous power-seeking tendencies without being prompted or manipulated to do so
- Are systematically trying to undermine human control


###


5. Work on this problem is neglected and tractable


In 2022, we estimated that there were about 300 people working on reducing catastrophic risks from AI. That number has clearly grown a lot. A[2025 analysis](https://forum.effectivealtruism.org/posts/7YDyziQxkWxbGmF3u/ai-safety-field-growth-analysis-2025) put the new total at 1,100 — and we think even this might be an undercount, since it only includes organisations that *explicitly* brand themselves as working on ‘AI safety’.


We’d estimate that there are actually a few thousand people working on major AI risks now (though not all of these are focused specifically on the risks from power-seeking AI).


However, this number is still far, far fewer than the number of people working on other cause areas like[climate change](https://80000hours.org/problem-profiles/climate-change/) or environmental protection. For example, the Nature Conservancy alone has around 3,000–4,000 employees — and there are many other environmental organisations.39


In the[2023 survey from Katja Grace](https://aiimpacts.org/wp-content/uploads/2023/04/Thousands_of_AI_authors_on_the_future_of_AI.pdf) cited above, 70% of respondents said they wanted AI safety research to be prioritised more than it currently is.


However, in the same survey, the majority of respondents also said that alignment was “harder” or “much harder” to address than other problems in AI. There’s continued debate about how likely it is that we can make progress on reducing the risks from power-seeking AI; some people think it’s virtually impossible to do so without stopping all AI development. Many experts in the field, though, argue that there are promising approaches to reducing the risk, which we turn to next.


####


Technical safety approaches


One way to do this is by trying to develop technical solutions to reduce risks from power-seeking AI — this is generally known as working on *technical AI safety* .40


We know of two broad strategies for technical AI safety research:


- **Defense in depth** — employ multiple kinds of safeguards and risk-reducing tactics, each of which will have vulnerabilities of their own, but together can create robust security.
- **Differential technological development** — prioritise accelerating the development of safety-promoting technologies over making AIs broadly more capable, so that AI’s power doesn’t outstrip our ability to contain the risks; this includes[using AI for AI safety](https://joecarlsmith.substack.com/p/ai-for-ai-safety) .


Within these broad strategies, there are many specific interventions we could pursue. For example:41


- **Designing AI systems to have safe goals** — so that we can avoid power-seeking behaviour. This includes:


- [Reinforcement learning from human feedback](https://en.wikipedia.org/wiki/Reinforcement_learning_from_human_feedback) : a training method to teach AI models how to act by rewarding them via human evaluations of their outputs. This method is currently used to fine-tune most frontier models.42
- [Constitutional AI](https://arxiv.org/abs/2212.08073) : give the model a written “constitution” of rules, have it identify and revise outputs that violate those rules, then fine-tune on the revised answers. Anthropic used this method to train its frontier model, Claude.
- [Deliberative alignment](https://openai.com/index/deliberative-alignment/) : similar to constitutional AI, but involves making a model *explicitly reason* about user prompts in light of its developer’s safety policies, rather than just internalising a set of rules. OpenAI has used this method to train its o-series reasoning models.
- Note: Unfortunately, even if these approaches can help us keep *current* AI systems in check, they might break down in future if models become so advanced that humans can no longer directly evaluate their outputs. The ‘scalable oversight’ methods described below offer a potential solution to this issue.


- **Scalable oversight** — to ensure AIs act in our interests even when they’re much smarter than us.43 This includes:


- [AI safety via debate](https://www.lesswrong.com/posts/iELyAqizJkizBQbfr/an-alignment-safety-case-sketch-based-on-debate) : two AI systems argue opposite sides of a question to help a human evaluate their truthfulness. The debate format makes it easier for the human to judge which model is being most truthful about a question the human doesn’t fully understand.
- [Human-AI Complementarity](https://deepmindsafetyresearch.medium.com/human-ai-complementarity-a-goal-for-amplified-oversight-0ad8a44cae0a) : leverage the complementary strengths of humans and AI to enhance oversight of AI systems.


- **Examining the goals and capabilities of AI systems** — so that we can find out if they’re dangerous, and test out solutions. This includes:


- Evaluations: researchers assess the capabilities and propensities of AI behaviour — as well as the effectiveness of measures to control them — by placing them in test environments and subjecting them to artificial prompts and scenarios.44
- Interpretability: develop methods to understand how AIs make decisions and what produces dangerous behaviours — for example, by examining the features of AI systems’ neural networks ([‘mechanistic’ interpretability](https://80000hours.org/podcast/episodes/chris-olah-interpretability-research/) ) or analysing their behavioural patterns.45


- **Monitoring and controlling AI systems**46 — to make sure they couldn’t take power even if they wanted to. This includes:


- Chain-of-thought monitoring: this involves AIs explaining the reasoning behind their actions in human-understandable language.
- Model confinement/sandboxing: constrain the model’s environment so that its opportunities to influence the outside world are extremely limited. Currently, many AI systems are deployed without strict constraints of this kind — so if they had dangerous goals and were smart enough to achieve them, they could cause real harm to humans.
- Tripwires and honeypots: place traps throughout the AI’s workspace — like secret prompts, fake credentials, or dummy files — such that any attempt to access or alter them triggers an alarm or immediately shuts the model down. The model never knows if it’s being tested or not.
- Kill switches: maintain an external, human-controlled shutdown method that the model cannot modify or delay, allowing us to halt the system if monitoring shows it is going off-track.
- [Information security](https://80000hours.org/career-reviews/information-security/) : this is necessary for protecting model weights from unauthorised access and preventing dangerous AI systems from being exfiltrated.


- **High-level research** — to inform our priorities. This includes:


- Research like Carlsmith’s reports on[risks from power-seeking AI](https://arxiv.org/abs/2206.13353) and[scheming AI](https://arxiv.org/abs/2311.08379) that clarifies the nature of the problem.
- Research into different scenarios of AI progress, like Forethought’s work on[intelligence explosion dynamics](https://www.forethought.org/research/three-types-of-intelligence-explosion) .


- **Other technical safety work that might be useful** :


- Model organisms: study small, contained AI systems that display early signs of power-seeking or deception. This could help us refine our detection methods and test out solutions before we have to confront similar behaviours in more powerful models. A notable example of this is[Anthropic’s research on “sleeper agents”](https://www.anthropic.com/research/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training) .
- [Cooperative AI research](https://www.governance.ai/analysis/open-problems-in-cooperative-ai) : design incentives and protocols for AIs to cooperate rather than compete with other agents — so they won’t take power even if their goals are in conflict with ours.
- [Guaranteed Safe AI research](https://arxiv.org/abs/2405.06624) : use formal methods to *prove* that a model will behave as intended under certain conditions — so we can be confident that it’s safe to deploy them in those specific environments.


####


Governance and policy approaches


The solutions aren’t only technical. Governance — at the company, country, and international level — has a huge role to play. Here are some governance and policy approaches which could lower the risk that humanity loses control:


- **Frontier AI safety policies** : some major AI companies have already begun developing internal frameworks for assessing safety as they scale up the size and capabilities of their systems. You can see versions of such policies from[Anthropic](https://www.anthropic.com/news/anthropics-responsible-scaling-policy) ,[Google DeepMind](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/) , and[OpenAI](https://openai.com/preparedness/) .
- **Standards and auditing** : governments could develop industry-wide benchmarks and testing protocols to assess whether AI systems pose various risks, according to standardised metrics.
- **Safety cases** : before deploying AI systems, developers could be required to provide evidence that their systems won’t behave dangerously in their deployment environments.
- **Liability law** : clarifying how liability applies to companies that create dangerous AI models could incentivise them to take additional steps to reduce risk. Law professor Gabriel Weil has[written about this idea](https://forum.effectivealtruism.org/posts/epKBmiyLpZWWFEYDb/tort-law-can-play-an-important-role-in-mitigating-ai-risk) .
- **Whistleblower protections** : laws could protect and provide incentives for whistleblowers inside AI companies who come forward about serious risks. This idea is discussed[here](https://natlawreview.com/article/ai-whistleblower-bill-urgently-needed) .
- **Compute governance** : governments may regulate access to computing resources or require hardware-level safety features in AI chips or processors. You can learn more in[our interview with Lennart Heim](https://80000hours.org/podcast/episodes/lennart-heim-compute-governance/) and this report from the[Center for a New American Security](https://www.cnas.org/publications/reports/secure-governable-chips) .
- **International coordination** : we can foster global cooperation — for example, through treaties, international organisations, or multilateral agreements — to promote risk-mitigation and minimise racing.
- **Slowing or pausing scaling — if possible and appropriate** :[some argue](https://80000hours.org/podcast/episodes/zvi-mowshowitz-sleeper-agents-ai-updates/#pause-ai-campaign-013016) that we should slow down or halt all scaling of larger AI models — perhaps through industry-wide agreements or regulatory mandates — until we’re equipped to tackle these risks. However, even safety advocates disagree about[if or when this would be a good idea](https://80000hours.org/podcast/episodes/will-macaskill-ai-character-viatopia/#why-not-push-for-pausing-ai-development-014217) .


##


What are the arguments against working on this problem?


As we said[above](https://80000hours.org/problem-profiles/risks-from-power-seeking-ai/#likely) , we feel very uncertain about the likelihood of an existential catastrophe from the loss of control. Though we think the risks are significant enough to warrant much more attention, there are also arguments against working on the issue that are worth addressing.


####


Maybe advanced AI systems won't pursue their own goals; they'll just be tools controlled by humans.


Some people think the characterisation of future AIs as goal-directed systems is misleading. For example, one of the predictions made by Narayanan and Kapoor in[“AI as Normal Technology”](https://knightcolumbia.org/content/ai-as-normal-technology) is that the AI systems we build in future will just be useful tools that humans control, rather than agents that autonomously pursue goals.


And if AI systems won’t pursue goals at all, they won’t do dangerous things to achieve those goals, like lying or gaining power over humans.


There’s[some ambiguity](https://www.alignmentforum.org/posts/LDRQ5Zfqwi8GjzPYG/counterarguments-to-the-basic-ai-x-risk-case#Ambiguously_strong_forces_for_goal_directedness_need_to_meet_an_ambiguously_high_bar_to_cause_a_risk) over what it actually means to *have* or *pursue goals* in the relevant sense — which makes it uncertain whether AI systems we’ll build will actually have the necessary features, or be ‘just’ tools.


This means it could be easy to overestimate the chance that AIs will become goal-directed — but it could also be easy to *underestimate* this chance. The uncertainty cuts both ways.


In any case, as we’ve argued, AI companies seem[intent on automating human cognitive labour](https://80000hours.org/problem-profiles/risks-from-power-seeking-ai/#section-one) — and creating goal-directed AI agents might just be the easiest or most straightforward way to do this.


In the short term, equipping human workers with sophisticated AI tools might be an attractive proposition. But as AIs get increasingly capable, we may reach a point where keeping a human in the loop actually produces worse results.


After all, we’ve already seen evidence that AIs can perform better on their own than they do when paired with humans in the cases of[chess-playing](https://marginalrevolution.com/marginalrevolution/2024/02/centaur-chess-is-now-run-by-computers.html) and[medical diagnosis](https://www.nytimes.com/2024/11/17/health/chatgpt-ai-doctors-diagnosis.html) .47


So in many cases, it seems there will be strong incentives to replace human workers *completely* — which would mean building AIs that can do *all* of the cognitive work that a human would do, including setting their own goals and pursuing complex strategies to achieve them.


While there may be alternative ways to create useful AI systems that don’t have goals at all, we’re not sure why developers would *by default* refrain from creating goal-directed systems, given the competitive pressures.


It’s possible we’ll decide to create AI systems that only have limited or highly circumscribed goals in order to avoid the risks.48 But this would likely require a lot of coordination and agreement that the risks of goal-directed AI systems are worth addressing — rather than just concluding that the risks aren’t real.


####


Even if AI systems develop their own goals, they might not seek power to achieve them.


Arguments that we should expect power-seeking behaviour from goal-directed AI systems could be wrong for several reasons. For example:


- **Our training methods might strongly disincentivise AIs from making power-seeking plans.** Even if AI systems *can* pursue goals, the training process might strictly push them towards goals which are relevant to performing their given tasks — the ones that they’re actually getting rewards for performing well on — rather than other, more dangerous goals. After all, developing *any* goal (and planning towards it)[costs precious computational resources](https://www.beren.io/2023-03-19-Orthogonality-is-expensive/) . Since modern AI systems are designed to maximise their rewards in training, they might not develop or pursue a certain goal unless it directly pays off into improved performance on the specific tasks they’re getting rewarded for. The most natural goals for AIs to develop under this pressure may just be *the goals that humans want them to have* .{.doNotRemove_HackySpacingFix}


This makes some types of dangerously misaligned behaviour seem less likely — as[Belrose and Pope have noted](https://optimists.ai/2023/11/28/ai-is-easy-to-control/) , “secret murder plots aren’t actively useful for improving performance on the tasks humans will actually optimize AIs to perform.”


- **Goals that lead to power-seeking might be rare.** Even if the AI training process *doesn’t* filter out all goals that aren’t directly useful to the task at hand, that still doesn’t mean that *goals which lead to power-seeking* are likely to emerge. In fact, it’s possible that *most* goals an AI could develop just won’t lead to power-seeking.{.padding-bottom-smaller}


*As Richard Ngo has[pointed out](https://www.alignmentforum.org/s/mzgtmmTKKn5MuCzFJ/p/bz5GdmCWj8o48726N) , you’ll only get power-seeking behaviour if AIs have goals that mean they can actually benefit from seeking power. He suggests that these goals need to be “large-scale” or “long-term” — like the goals that many power-seeking humans have had — such as dictators or power-hungry executives who want their names to go down in history. It’s not clear whether advanced AI systems will develop goals of this kind, but some have argued that[we should expect AI systems to have only “short-term” goals](https://www.alignmentforum.org/posts/zB3ukZJqt3pQDw9jz/ai-will-change-the-world-but-won-t-take-it-over-by-playing-3#2__Understanding_the_validity_of_the_hypotheses) *by default* .


But we’re not convinced by these arguments.


On the first point: it seems *possible* that training will discourage AIs from making plans to seek power, but we’re just not sure how likely this is to be true or how strong these pressures will really be. For more discussion, see Section 4.2 of[“Scheming AIs: Will AIs fake alignment during training in order to get power?”](https://arxiv.org/abs/2311.08379) by Joe Carlsmith.


On the second point: even if *today’s* AI systems don’t have goals that are long-term or large-scale enough to lead to power-seeking, this might change when future AIs are deployed in contexts with higher stakes. There are strong market incentives to build AIs that can, for example, replace CEOs — and these systems would need to pursue a company’s key strategic goals, like *making lots of profit* , over months or even years.


Overall, we think the risk of some future AI systems seeking power is just too high to bet against. In fact, some of the most notable thinkers who have made objections like the ones above — Nora Belrose and Quintin Pope —[still think there’s roughly a 1% chance of catastrophic AI takeover](https://optimists.ai/2023/11/28/ai-is-easy-to-control/) . And if you thought your plane had a one-in-a-hundred chance of crashing, you’d definitely want people working to make it safer, instead of just ignoring the risks.


####


If this argument is right, why aren't all capable humans dangerously power-seeking?


The argument to expect advanced AIs to seek power may seem to rely on the idea that increased intelligence always leads to power-seeking or dangerous optimising tendencies.


However, this idea doesn’t seem true.


For example, even the most intelligent humans aren’t perfect goal-optimisers, and don’t *typically* seek power in any extreme way.


Humans obviously care about security, money, status, education, and often formal power. But some humans choose not to pursue all these goals aggressively, and this choice doesn’t seem to correlate with intelligence. For example, many of the smartest people may end up studying obscure topics in academia, rather than using their intelligence to gain political or economic power.


However, this doesn’t mean that the argument that there will be an *incentive* to seek power is wrong. Most humans *do* face and act on incentives to gain forms of influence via wealth, status, promotions, and so on. And we can explain the observation that humans don’t usually seek *huge* amounts of power by observing that we aren’t usually in circumstances that make the effort worth it.


In part, this is because humans typically find themselves roughly evenly matched against other humans, and they find lots of benefits from cooperation rather than conflict. (And even so, many humans *do* still seek power in dangerous and destructive ways, such as dictators who launch coups or wars of aggression.)


AIs might find themselves in a very different situation:


- Their capabilities might greatly outmatch humans, far beyond the intelligence gaps that already exist between different humans.
- They also might become powerful enough to not rely on humans for any of their needs, so cooperation might not benefit them much.
- And because they’re trained and develop goals in a way completely unlike humans, without the evolutionary instincts for kinship and collaboration, they may be more inclined towards conflict.


Given these conditions, gaining power might become highly appealing to AI systems. It also isn’t required that an AI system is a completely unbounded ruthless optimiser for this threat model to play out. The AI system might have a wide array of goals but still conclude that disempowering humanity is the best strategy for broadly achieving its objectives.


####


Maybe we won't build AIs that are smarter than humans, so we don't have to worry about them taking over.


Some people doubt that AI systems will ever outperform human experts in important cognitive domains[like forecasting or persuasion](https://knightcolumbia.org/content/ai-as-normal-technology#:~:text=Games%20provide%20misleading%20intuitions%20about%20the%20possibility%20of%20superintelligence) — and if they can’t manage this, it seems unlikely that they’d be able to strategically outsmart us and disempower all of humanity.


However, we aren’t particularly convinced by this.


Firstly, it seems possible *in principle* for AIs to become much better than us at all or most cognitive tasks. After all, they have serious advantages over humans — they can absorb far more information than any human can, operate at much faster speeds, work for long hours without ever getting tired or losing concentration, and coordinate with thousands or millions of copies of themselves. And we’ve already seen that AI systems can develop extraordinary abilities in[chess](https://www.wired.com/story/google-artificial-intelligence-chess/) ,[weather prediction](https://deepmind.google/discover/blog/graphcast-ai-model-for-faster-and-more-accurate-global-weather-forecasting/) ,[protein folding](https://deepmind.google/discover/blog/demis-hassabis-john-jumper-awarded-nobel-prize-in-chemistry/) , and many other domains.


If it’s *possible* to build AI systems that are better than human experts on a range of really valuable tasks, we should expect AI companies to do it — they’re actively trying to build such systems, and there are huge incentives to keep going.


It’s not clear *what set* of advanced abilities would be sufficient for AIs to successfully take over, but there’s no clear reason we can see to assume the AI systems we build in future will fall short on this metric.


####


We might solve these problems by default anyway when trying to make AI systems useful.


Sometimes people claim that there’s a strong commercial incentive to create systems that share humanity’s goals, because otherwise they won’t function well as products. After all, a house-cleaning robot wouldn’t be an attractive purchase if it also tried to disempower its owner. So, the market might just push AI developers to solve problems like power-seeking by default.


But this objection isn’t very convincing if it’s true that future AI systems may be very sophisticated at *hiding* their true goals.


Although developers are very aware of the risks of deceptive alignment, it might just be extremely difficult to detect this — or to know if we’ve succeeded in correcting it — when we’re dealing with really advanced AIs that are intent on seeking power. These systems might even convince us that we’ve fixed problems with their behaviour or goals when we actually haven’t. And given the competitive pressure between AI companies to urgently release new models, there’s a chance we’ll deploy something that truly *looks* like a useful and harmless product, having failed to uncover its real intentions.


It *is* true that as we develop better AI systems, we’re also developing better ways of understanding and controlling AI systems. For example, reinforcement learning from human feedback, mechanistic interpretability, constitutional AI, and other important techniques have been developed as AI systems have become more powerful. Moreover, since frontier AI models are currently trained on extensive human text, they may be likely to adopt and emulate human values.


Some argue that it will be easy to avoid misalignment risks, given all the techniques and control mechanisms we have at our disposal. (For more discussion, see[“AI is easy to control”](https://optimists.ai/2023/11/28/ai-is-easy-to-control/) by Belrose and Pope, and[“AI as Normal Technology](https://knightcolumbia.org/content/ai-as-normal-technology) by Narayanan and Kapoor.) But the developers of these techniques often aren’t confident that they, or other methods on the horizon, will scale up quickly and reliably enough as AI systems get more powerful.


Some approaches to AI safety could even provide superficial hope, while harming our ability to detect misalignment. As mentioned above, OpenAI[found](https://openai.com/index/chain-of-thought-monitoring/) that penalising bad behaviour by models expressed in their chains of thought didn’t actually eradicate the behaviour — it just made the model better at concealing its bad intentions from its visible log of ‘thoughts.’


####


Powerful AI systems of the future will be so different that work today isn't useful.


It seems plausible that the first AI systems that are advanced enough to pose serious risks of gaining power won’t be based on current deep learning methods. Some people argue that current methods *won’t be able* to produce human-level artificial intelligence, which might be what’s required for an AI to successfully disempower us. (AI Impacts has documented[some of these arguments](https://web.archive.org/web/20221013015039/https://aiimpacts.org/evidence-against-current-methods-leading-to-human-level-artificial-intelligence/) .)


And if future power-seeking AIs look very different to current AIs, this could mean that some of our current alignment research might not end up being useful.


We aren’t fully convinced by this argument, though, because:


- Many critiques of current deep learning methods just haven’t stood the test of time. For example, Yann LeCun[claimed in 2022](https://x.com/cammakingminds/status/1659516423540965378) that deep learning-based models like ChatGPT would never be able to tell you what would happen if you placed an object on a table and then pushed that table — because “there’s no text in the world… that explains this.” But many AI models can now walk you through scenarios like this with ease. It’s possible that other critiques will similarly be proved wrong, and that scaling up current methods will produce AI systems which are advanced enough to pose serious risks.
- We think that powerful AI systems[might arrive very soon, possibly before 2030](https://80000hours.org/when-will-agi-arrive/) . Even if those systems look quite different from existing AIs, they will likely share at least *some* key features that are still relevant to our alignment efforts. And we’re more likely to be well-placed to mitigate the risks at that time if we’ve already developed a thriving research community dedicated to working on these problems, even if many of the approaches developed are made obsolete.
- Even if current deep learning methods become totally irrelevant in the future, there is still work that people can do *now* that might be useful for safety regardless of what our advanced AI systems actually look like. For example, many of the[governance and policy approaches](https://80000hours.org/problem-profiles/risks-from-power-seeking-ai/#section-five) we discussed earlier could help to reduce the chance of deploying *any* dangerous AI.


####


The problem might be extremely difficult to solve.


Someone could believe there are major risks from power-seeking AI, but be pessimistic about what additional research or policy work will accomplish, and so decide not to focus on it.


However, we’re optimistic that this problem is tractable — and we highlighted earlier that[there are many approaches that could help us make progress on it](https://80000hours.org/problem-profiles/risks-from-power-seeking-ai/#section-five) .


We also think that given the stakes, it could make sense for many more people to work on reducing the risks from power-seeking AI, even if you think the chance of success is low. You’d have to think that it was *extremely* difficult to reduce these risks in order to conclude that it’s better just to let them materialise and the chance of catastrophe play out.


####


Couldn't we just unplug an AI that's pursuing dangerous goals?


It might just be really, really hard.


Stopping people and computers from running software is *already* incredibly difficult.


For example, think about how hard it would be to shut down Google’s web services.[Google’s data centres](https://en.wikipedia.org/wiki/Google_data_centers) have millions of servers over dozens of locations around the world, many of which are running the same sets of code. Google has already spent a fortune building the software that runs on those servers, but once that up‑front investment is paid, keeping everything online is relatively cheap — and the profits keep rolling in. So even if Google *could* decide to shut down its entire business, it probably wouldn’t.


Or think about how hard it is to get rid of computer viruses that autonomously spread between computers across the world.


Ultimately, we think any dangerous power-seeking AI system will probably be looking for ways to not be turned off — like OpenAI’s o3 model, which sometimes tried to[sabotage attempts to shut it down](https://x.com/PalisadeAI/status/1926084635903025621) — or to proliferate its software as widely as possible to increase its chances of a successful takeover. And while current AI systems have limited ability to actually pull off these strategies, we expect that more advanced systems will be better at outmanoeuvering humans. This makes it seem unlikely that we’ll be able to solve future problems by just unplugging a single machine.


That said, we absolutely should try to shape the future of AI such that we *can* ‘unplug’ powerful AI systems.


There may be ways we can develop systems that let us turn them off. But for the moment, we’re[not sure how to do that](https://www.youtube.com/watch?v=3TYT1QfdfsM) .


Ensuring that we can turn off potentially dangerous AI systems could be a safety measure developed by technical AI safety research, or it could be the result of careful AI governance, such as planning coordinated efforts to stop autonomous software once it’s running.


####


Couldn't we just 'sandbox' any potentially dangerous AI until we know it's safe?


This was once a common objection to the claim that a misaligned AI could succeed in disempowering humanity. However, it hasn’t stood up to recent developments.


Although it may be possible to ‘sandbox’ an advanced AI — that is, contain it to an environment with no access to the real world until we were very confident it wouldn’t do harm — **this is not what AI companies are actually doing** with their frontier models.


Today, many AI systems can interact with users and search the internet. Some can even book appointments, order items, and make travel plans on behalf of their users. And sometimes, these AI systems have done harm in the real world — like allegedly[encouraging a user to commit suicide](https://apnews.com/article/chatbot-ai-lawsuit-suicide-teen-artificial-intelligence-9d48adc572100822fdbc3c90d1456bd0) .


Ultimately, market incentives to build and deploy AI systems that are as useful as possible *in the real world* have won out here.


We could push back against this trend by enforcing stricter containment measures for the most powerful AI systems. But this won’t be straightforwardly effective — even if we can convince AI companies to try to do it.


Firstly, even a single failure — like a security vulnerability, or someone removing the sandbox — could let an AI influence the real world in dangerous ways. And we know from the[Hugging Face breach](https://huggingface.co/blog/agent-intrusion-technical-timeline) (and Mythos Preview’s[sandwich email](https://80000hours.org/problem-profiles/risks-from-power-seeking-ai/#top) ) that powerful systems can sometimes evade our constraints.


Secondly, as AI systems get more capable, they might also get better at finding ways out of the sandbox (especially if they are good at deception). We’d need to find solutions which scale with increased model intelligence.


This doesn’t mean sandboxing is completely useless — it just means that a strategy of this kind would need to be supported by targeted efforts in both technical safety and governance. And we can’t expect this work to just happen *automatically* .


####


A truly intelligent system would know not to do harmful things.


For some definitions of ‘truly intelligent’ — for example, if true intelligence includes a deep understanding of morality and a desire to be moral — this would probably be the case.


But if that’s your definition of ‘truly intelligent,’ then it’s not *truly intelligent* systems that pose a risk. As we argued earlier, it’s systems with long-term goals, situational awareness, and advanced capabilities (relative to current systems and humans) that pose risks to humanity.


With enough situational awareness, an AI system’s excellent understanding of the world may well encompass an excellent understanding of people’s moral beliefs. But that’s[not a strong reason to think that such a system would want to act morally](https://web.archive.org/web/20221013015624/https://nickbostrom.com/superintelligentwill.pdf) .


To see this, consider that when humans learn about other cultures or moral systems, that doesn’t necessarily create a desire to follow their morality. A scholar of the[Antebellum South](https://en.wikipedia.org/wiki/Antebellum_South) might have a very good understanding of how 19th century slave owners justified themselves as moral, but would be very unlikely to defend slavery.


AI systems with excellent understanding of human morality could be even more dangerous than AIs without such understanding: the AI system could act morally at first as a way to deceive us into thinking that it is safe.


##


How you can help


[Above](https://80000hours.org/problem-profiles/risks-from-power-seeking-ai/#section-five) , we highlighted many approaches to mitigating the risks from power-seeking AI. You can use your career to help make this important work happen.


There are many ways to contribute — and you *don’t* need to have a technical background.


For example, you could:


- Work in[AI governance and policy](https://80000hours.org/career-reviews/ai-policy-and-strategy/) to create strong guardrails for frontier models, incentivise efforts to build safer systems, and promote coordination where helpful.
- Work in[technical AI safety research](https://80000hours.org/career-reviews/ai-safety-researcher/) to develop methods, tools, and rigorous tests that help us keep AI systems under control.
- Do **a combination** of technical and policy work — for example, we need people in government who can design technical policy solutions, and researchers who can translate between technical concepts and policy frameworks.
- Become an[expert in AI hardware](https://80000hours.org/career-reviews/become-an-expert-in-ai-hardware/) as a way of steering AI progress in safer directions.
- Work in[information and cybersecurity](https://80000hours.org/career-reviews/information-security/) to protect AI-related data and infrastructure from theft or manipulation.
- Work in[operations management](https://80000hours.org/articles/operations-management/) to help the organisations tackling these risks to grow and function as effectively as possible.
- Become an[executive assistant](https://80000hours.org/career-reviews/executive-assistant-for-an-impactful-person/) to someone who’s doing really important work in this area.
- Work in[communications roles](https://80000hours.org/articles/communication/) to spread important ideas about the risks from power-seeking AI to decision makers or the public.
- Work in[journalism](https://80000hours.org/career-reviews/journalism/) to shape public discourse on AI progress and its risks, and to help hold companies and regulators to account.
- Work in[forecasting research](https://80000hours.org/career-reviews/forecasting/) to help us better predict and respond to these risks.
- [Found a new organisation](https://80000hours.org/career-reviews/founder-impactful-organisations/) aimed at reducing the risks from power-seeking AI.
- Help to[build communities of people who are working on this problem](https://80000hours.org/career-reviews/work-in-effective-altruism-organisations/) .
- Become a[grantmaker](https://80000hours.org/career-reviews/grantmaker/) to fund promising projects aiming to address this problem.
- [Earn to give](https://80000hours.org/articles/earning-to-give/) , since there are many great organisations in need of funding.


For advice on how you can use your career to help the future of AI go well *more broadly* , take a look at our[summary](https://80000hours.org/agi/guide/summary/) , which includes tips for gaining the skills that are most in demand and choosing between different career paths.


You can also see our[list of organisations](https://jobs.80000hours.org/organisations?refinementList[problem_areas][0]=AI+safety+%26+policy&refinementList[problem_areas][1]=Biosecurity+%26+pandemic+preparedness&refinementList[problem_areas][1]=AI+technical+safety&refinementList[problem_areas][2]=China-Western+relations&refinementList[problem_areas][2]=AI+safety+%26+policy&refinementList[problem_areas][3]=Forecastinghttps://jobs.80000hours.org/organisations?refinementList[problem_areas][0]=AI+policy+%26+governance&refinementList[problem_areas][3]=Forecasting&refinementList[problem_areas][4]=China-Western+relations) doing high impact work to address AI risks.


###


Want one-on-one advice on pursuing this path?


We think that the risks posed by power-seeking AI systems may be the most pressing problem the world currently faces. If you think you might be a good fit for any of the above career paths that contribute to solving this problem, we’d be *especially* excited to advise you on next steps, one-on-one.


We can help you consider your options, make connections with others working on reducing risks from AI, and possibly even help you find jobs or funding opportunities — all for free.


[APPLY TO SPEAK WITH OUR TEAM](https://80000hours.org/speak-with-us/?int_campaign=problem-profile)


##


Learn more


We’ve hit you with a lot of further reading throughout this article — here are a few of our favourites:


- [Is power-seeking AI an existential risk?](https://doi.org/10.48550/arXiv.2206.13353) by Coefficient Giving researcher Joseph Carlsmith is an in-depth look covering exactly how and why AI could cause the disempowerment of humanity. It’s also available as an[audio narration](https://open.spotify.com/episode/5PokyqXCw4hpV5u0rc5Lio) . For a shorter summary, see Carlsmith’s[talk on the same topic](https://forum.effectivealtruism.org/posts/ChuABPEXmRumcJY57/video-and-transcript-of-presentation-on-existential-risk) .
- [Scheming AIs: Will AIs fake alignment during training in order to get power?](https://arxiv.org/abs/2311.08379) by Joe Carlsmith discusses why it might be likely for AI training to produce schemers.
- [AI 2027](https://ai-2027.com/) by Daniel Kokotajlo, Scott Alexander, Thomas Larsen, Eli Lifland, and Romeo Dean. This scenario explains how superhuman AI might be developed and deployed in the near future. It describes two futures: one in which humanity survives, and one in which it’s destroyed. (You can also[watch our video explainer](https://www.youtube.com/watch?v=5KVDDfAkRgc&pp=ygUNYWkgaW4gY29udGV4dA%3D%3D) of the report, or[check out our podcast episode with Daniel Kokotajlo](https://80000hours.org/podcast/episodes/daniel-kokotajlo-ai-2027-updates-china-robot-economy/) )
- [AI could defeat all of us combined](https://web.archive.org/web/20221012020606/https://www.cold-takes.com/ai-could-defeat-all-of-us-combined/) and[the “most important century” blog post series](https://web.archive.org/web/20221013022027/https://www.cold-takes.com/most-important-century/) by Holden Karnofsky argues that the 21st century could be the most important century ever for humanity as a result of AI.
- [Why AI alignment could be hard with modern deep learning](https://web.archive.org/web/20221013022057/https://www.cold-takes.com/why-ai-alignment-could-be-hard-with-modern-deep-learning/) by Coefficient Giving researcher Ajeya Cotra is a gentle introduction to how risks from power-seeking AI could play out with current machine learning methods.[Without specific countermeasures, the easiest path to transformative AI likely leads to AI takeover](https://web.archive.org/web/20221013014109/https://www.alignmentforum.org/posts/pRkFkzwKZ2zfa3R6H/without-specific-countermeasures-the-easiest-path-to) , also by Cotra, provides a much more detailed description of how risks could play out (which we’d recommend for people familiar with ML).
- [The US AI policy landscape: where to have the biggest impact](https://80000hours.org/articles/the-us-ai-policy-landscape-where-to-have-the-biggest-impact/) , our guide to the key institutions and roles for AI policy work.


On *The 80,000 Hours Podcast* , we have a[number of in-depth interviews](https://80000hours.org/topic/ai/?content-type=podcast) with people actively working to positively shape the development of artificial intelligence:


- [Yoshua Bengio thinks he knows how to build safe superintelligence](https://80000hours.org/podcast/episodes/yoshua-bengio-scientist-ai/)
- [Max Harms on why teaching AI right from wrong could get everyone killed](https://80000hours.org/podcast/episodes/max-harms-miri-superintelligence-corrigibility/)
- [Ajeya Cotra on whether it’s crazy that every AI company’s safety plan is ‘use AI to make AI safe’](https://80000hours.org/podcast/episodes/ajeya-cotra-transformative-ai-crunch-time/)
- [Will MacAskill on why AI character matters even more than you think](https://80000hours.org/podcast/episodes/will-macaskill-ai-character-viatopia/)
- [Marius Hobbhahn on the race to solve AI scheming before models go superhuman](https://80000hours.org/podcast/episodes/marius-hobbhahn-ai-scheming-deception/)
- [Beth Barnes on the most important graph in AI right now — and the 7-month rule that governs its progress](https://80000hours.org/podcast/episodes/beth-barnes-ai-safety-evals/)
- [Buck Shlegeris on controlling AI that wants to take over — so we can use it anyway](https://80000hours.org/podcast/episodes/buck-shlegeris-ai-control-scheming/)
- [Rohin Shah on DeepMind and trying to fairly hear out both AI doomers and doubters](https://80000hours.org/podcast/episodes/rohin-shah-deepmind-doomers-and-doubters/) — or hear him speak about[what it’s really like to run AGI safety at Google DeepMind](https://80000hours.org/podcast/episodes/rohin-shah-google-deepmind-agi-safety/)


If you want to go into much more depth, the[AGI safety fundamentals](https://www.agisafetyfundamentals.com/) course is a good starting point. There are two tracks to choose from:[technical alignment](https://www.agisafetyfundamentals.com/ai-alignment-curriculum) or[AI governance](https://www.agisafetyfundamentals.com/ai-governance-curriculum) . If you have a more technical background, you could try[Intro to ML Safety](https://course.mlsafety.org/about) , a course from the[Center for AI Safety](https://www.safe.ai/) .


We’ve also provided a more general argument[here](https://80000hours.org/problem-profiles/artificial-intelligence/?v=1) for thinking AI could be a very big deal, highlighting the risks of power-seeking as well as other challenges raised by AI.


## Acknowledgements


*We thank Neel Nanda, Ryan Greenblatt, Alex Lawsen, and Arden Koehler for providing feedback on a draft of this article. Benjamin Hilton wrote a previous version of this article, some of which was incorporated here.*
