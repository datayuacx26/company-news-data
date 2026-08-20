---
schema_version: "1.0.0"
document_id: "38107e0f8e53dddefdf346ff3faff2c055b13083db7a129f795d0de35752ac30"
company_key: "yc-sciloop"
company: "Sciloop"
source_id: "yc-sciloop-news-import-c02f926369ce"
canonical_url: "https://www.sciloop.dev/blog/the-art-of-breaking-frontier-ai-models"
published_at: null
first_seen_at: "2026-07-25T23:19:23.543395+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:877db81118141a88d69d9b78bb1e384254a00b117be68ba5119e25694f3d2bd0"
---

# The Art of Breaking Frontier AI Models

The specifics of this guide are for Physics. But the core principles and lessons can be applied in pretty much every other domain e.g. Maths, Biology, Chemistry etc.


## How it all began


When we joined the Y-Combinator Fall 2025 batch, my cofounder and I decided to create reasoning datasets to train frontier AI models across STEM (physics, chemistry, maths and biology). Since we were International Physics Olympiad medalists, we thought a natural start would be to create a few Physics problems since we could write them ourselves. So we talked with another startup,[AfterQuery](https://www.afterquery.com/) , who in turn would sell our dataset to frontier labs they were working with. Before a pilot could begin, AfterQuery asked us to create 10 sample problems for our Physics dataset. So after a long time, my cofounder and I got a chance to do what we absolutely love - Physics.


We were pretty happy, except for the fact that it had been almost 3 years since we seriously studied physics in our high school days. So we began to test the current capabilities of frontier models in physics. I opened ChatGPT 5.2 on one tab, and some physics olympiad past papers on the other. Then, I copy-pasted a physics olympiad problem onto ChatGPT and waited until it came up with a final answer. I myself had not revised that specific topic, so I just cross-checked ChatGPT's final solution with the official solution, and it matched. Then I copy-pasted another problem, then another, and kept doing it, hoping that the model would start failing. Finally, I turned to my cofounder and exclaimed, *"Damn, these AI models have become really good at Physics."*


## The first attempt (and failure)


AfterQuery had asked us to come up with 10 sample problems, but they had vaguely instructed us to create "Olympiad Style" physics problems. I thought it might be a really long shot to break AI models in Physics. Maybe we could get away with creating moderately hard physics problems, even if ChatGPT 5.2 was solving them in one shot. So my cofounder, myself, and two of my physics friends from Pakistan locked in for two days straight and finalized our 10 sample physics problems. The next day, I emailed them to AfterQuery, hoping to begin the pilot program. Two days later, I finally heard back:


> "We reviewed the sample and found errors in 4/10 of the tasks. The difficulty is also not hard – we could synth generate questions at this difficulty."


In other words, the samples we made were so bad that even an AI could generate them. We temporarily gave up on the idea, to move onto something more achievable.


## A glimmer of hope


A month later I randomly came across a benchmark by OpenAI called[FrontierScience](https://openai.com/index/frontierscience/) , and in it there were some physics olympiad problems where frontier models scored a 75 percent accuracy. Seeing that there were still 25 percent of problems where ChatGPT 5.2 was failing, my previously lost hope was ignited. AI models were still not as invincible as I had thought.


In my previous attempts to fail AI models, I had already accepted that frontier models have become too good to be stumped. This underlying assumption was inspired by the fact that frontier AI models got a gold-level performance at International Physics Olympiad 2025. However, upon closer inspection of the results, top AI models scored ~21/30 on the theoretical exam, decent enough to secure a gold (top 10% performance across all human participants), but still noticeably far from the top human performers who scored an almost perfect 30/30.


This gap of 10 points between top human scorers and top AI models motivated me to try to break AI models again. However, this time I was going to try wholeheartedly. I went back to a resource that I used to prepare for the International Physics Olympiad back in the day - Mechanics Handout by Prof Jaan Kalda. This is a collection of some really tricky (and fun) mechanics problems. Again, I started copy-pasting these problems onto ChatGPT one after another, until the AI model finally failed. Actually, it failed 3–4 times, and I felt a little victorious.


However, I did not want to limit our samples to Mechanics problems, I wanted to see if I could find problems where AI would fail in Thermodynamics, Electrical Circuits, and other areas of Physics too. Unfortunately, it was again super hard to find problems the AI model was failing at. Sometimes ChatGPT 5.2 would fail, but when I switched to ChatGPT 5.2 Extended Thinking, it would keep bashing its head for 15 minutes until it came up with the right answer.


This time I was not going to give up without a fight. My cofounder and I set ourselves a goal: **create 10 physics problems where ChatGPT 5.2 Thinking would score a 0% accuracy** . And in about 10 days, we did end up hitting that goal. Here are all the strategies I learnt about breaking AI models.


---


## Strategy 1: Start hard, then push harder


Start with a base problem from one of your favourite books or handouts where a frontier model needs a lot of time to solve correctly. If ChatGPT is giving a correct answer within 30 seconds of thinking, chances are it's pretty far from failing. 3 minutes? The AI model had to think and reason to the best of its ability. 10–15 minutes? The AI model had to work *really* hard to get to the correct answer.


When an AI model thinks for that long, if you examine its chain of reasoning, you would see that perhaps it started with the wrong approach, and kept changing its approach until it stumbled upon something that made sense and led to the correct eventual answer. It is pretty fascinating how similar this approach is to how a real human would solve a hard problem: come up with a bunch of viable strategies and check them one by one until it works out.


But what if a human took really long to come up with the correct answer? You would be pretty sure that if you had made the problem slightly harder, the human might have failed altogether. **The same is the case with AI models** - when they take 10–15 minutes to come up with an answer, slightly increasing the complexity of the problem, adding unnecessary variables here and there for example, can lead to AI's failure (and your victory).


### An example: the cylinder and strip problem


Consider Problem 8 from[Prof Jaan Kalda's mechanics handout](https://www.ioc.ee/~kalda/ipho/meh_ENG2.pdf) :


This is a tough problem, and you can find the complete solution[here](https://physoly.tech/static/files/KaldaMech-122.pdf) . The final answer is *v/2* . This answer is actually independent of the distance *a* or the coefficient of friction *μ* . By understanding the solution, I realized that it did not really matter if there was one strip, or two, or even an infinite number of strips - the solution would remain *v/2* .


So in my next version of this problem, I added two strips instead of one: one at distance *a* from the top of the cylinder, and the other at distance *b* from the bottom. I could have given both strips the same coefficient of friction *μ* (since the answer is independent of it). Yet, just to add extra complexity, I assigned them coefficients of friction *μ₁* and *μ₂* respectively.


The modified version: two strips with different friction coefficients


ChatGPT 5.2 Thinking thought for another 15 minutes, and spat out an answer that had *μ₁* , *μ₂* , *a* and *b* in it. The[correct answer](https://drive.google.com/drive/u/4/folders/1vPn7Sqou5OBt7KJbjd26_LpJQUueXyZY) ( *v/2* ) was supposed to be independent of these variables - I had won.


**Note:** As of today, the latest model is ChatGPT 5.4 Extended Thinking, as opposed to ChatGPT 5.2 Thinking when I created the problem. The new model gets it correctly. A good exercise might be to play with the wording and variables of the problem until the new model fails too.


## Strategy 2: Mix and match concepts


Start with 3–4 moderately hard physics problems, develop a deep understanding of them, and try to mix and match their concepts to create a problem that's more complex than any of them individually. Usually each problem requires a unique insight or trick to solve. This is especially true if the physics problem is elegant rather than too mathematically complicated. If you stack these insights on top of each other, you can make a moderately hard problem really complex.


With each added complexity, you'll usually observe the AI model taking longer and longer to answer - this is a positive sign. If an AI model is able to get an answer correctly after taking some time, I would highly recommend examining the chain-of-thought reasoning of the model. It gives a lot of clues about where the model is struggling, and how you could add another layer of complexity to make it harder for the model to stumble upon the correct answer.


### An example: combining circuit problems


Consider problem 3 of the[Electrical Circuits Handout by Prof Jaan Kalda](https://www.ioc.ee/~kalda/ipho/electricity-circuits.pdf) .


This is a pretty fun problem (explanation is within the[handout](https://www.ioc.ee/~kalda/ipho/electricity-circuits.pdf) as well as in the solution found[here](https://www.scribd.com/document/817654394/Kalda-Circuits-Solutions-Manual-46) ). The key insight to the correct answer is that this configuration is equivalent to the following configuration. Basically resistors 1 through 5, which have 1 ohm resistance each in this problem, can be rearranged into series and parallel combinations like this:


The equivalent series-parallel configuration


ChatGPT was able to get this one.


Now consider[problem 2 from the Electrical Circuits Handout by Prof Jaan Kalda](https://www.ioc.ee/~kalda/ipho/electricity-circuits.pdf) .


The solution can be found[here](https://www.scribd.com/document/817654394/Kalda-Circuits-Solutions-Manual-46) . ChatGPT was able to get this one too.


At that moment, I had a few other problems in mind too (because I was going through this handout problem by problem). So I decided to combine the concepts from problems 2 and 3. I encourage you to go over the solutions of the two problems above, so you'll be able to understand how they naturally gave rise to the one below. (The solution to this problem can be found[here](https://drive.google.com/drive/u/4/folders/1DUysQ5nTQ3fOG1tMsgy3M15emeJFYtT-) .)


The combined problem, merging two seemingly unrelated circuit problems


This time, ChatGPT failed. I felt so happy. This wasn't just adding a few variables here and there. It was **merging two seemingly unrelated problems** . But it worked. It gave me the confidence that I can keep mixing and matching hard problems until I made a truly difficult problem that ChatGPT cannot solve.


## Strategy 3: Bait the model


Examine the chain-of-thought reasoning of the AI model and target its weaknesses. Basically, **bait the model** .


This strategy is useful for scenarios where, in addition to taking a lot of time to solve a problem, the model wasted the first few minutes fixated on something that was either unrelated to the solution or something that would have led to a wrong answer. Consider a human solving a problem. They waste most of the time fixated upon the wrong approach for an hour, but in the last 5 minutes or so, they decide to take some other (correct) approach. What would you do in this situation if your goal was to stump the human test taker? You would introduce unnecessary information (a bait) that would make the first (wrong) approach even more lucrative, so that the human never even gets to attempting the correct approach.


Turns out you can bait not just humans, but models too.


### An example: the phantom magnetic field


Consider the first version of the problem:


The original problem, no magnetic field mentioned


When I gave ChatGPT 5.2 Extended Thinking this problem, it did arrive at the correct solution - but after trying out a lot of wrong approaches. It was particularly fixated on this idea of a magnetic field *B* , which was somehow responsible for the cessation of current in the circuit. I did not mention anything about magnetic fields in the problem; the solution was completely independent of it. However, the model seemed to waste quite a bit of time exploring this imaginary magnetic field.


So I decided to bait the model. I intentionally introduced a magnetic field in the next version of the problem. The model took the bait, and spat out an answer that had magnetic field *B* in it (victory hehe).


The baited version, just one extra sentence: "The system is placed in a uniform magnetic field B"


## Other approaches worth exploring


There are several other approaches you can use to stump AI models. For example, AI models have a noticeable weakness in visual reasoning. They can make subtle errors while reading graph slopes, for instance. You can also keep adding mathematical complexity to the question to divert the model from an elegant, subtle physical trick.


The point is that I was able to learn these ways in about 10 days of effort. If you put in some time, you can definitely use some of these to stump AI models, or better yet, come up with strategies of your own. Making an AI model fail can be pretty hard and time-consuming, so use the strategies that make you feel most confident in your approach.


---


## The hard things about hard things


Making frontier models fail, as of March 2026, is by no means easy. It is pretty hard. For every victory I mentioned, there were many failures. Some of these problems took me 5–6 hours (or more) to come up with. The first few problems took the longest amount of time.


If you spend the first day or two unable to stump an AI model, don't be discouraged. Use your learning, and give it another go. Sometimes I would spend many hours, only to reach dead ends. In these scenarios, I would simply move to a different topic.


You have to understand that AI models are trained on lots and lots of data. For some topics, there is more data available on the internet than others, so the model is naturally stronger in some areas compared to the rest. **Your job is to spot the weaker areas and attack its weaknesses.** Most importantly, don't be discouraged if you happen to try to fail the model in one of its strong areas. Change the problems, move up, move down, left, right and hopefully you'll regain your lost hope, get your confidence back, and stump the AI model.


## The most efficient approach


The problems need to be somehow novel, of course. You obviously do not want to copy some other professor's work word for word. But at the same time, you do not want to reinvent the wheel either. For example, authors of International Physics Olympiad problems spend *months* on each problem because they need to make it totally original and interesting. For you, however, it is okay to get inspiration from existing physics problems and modify or combine them to create harder and better problems. Credit the authors of problems you drew inspiration from. In my opinion, it is totally fine to draw inspiration.


I decided to take inspiration from Prof Jaan Kalda's handouts because I really like them. Feel free to open your favourite problem set books:


- Pathfinder for Olympiad Physics
- Advanced Problems in Physics (SS Krotov)
- Kevin Zhou Handouts
- Any other favourite book of yours


Past papers of various Physics Olympiads are also excellent sources:


- International Physics Olympiad (IPhO)
- European Physics Olympiad (EuPhO)
- Asian Physics Olympiad (APhO)
- Nordic-Baltic Physics Olympiad (NBPhO)
- Indian National Physics Olympiad (INPhO)
- Any other favourite competition of yours


Copy-paste the problems one by one onto ChatGPT and look for patterns where AI models seem to take a long time to answer. Sometimes you would get lucky and, once in a while (1 in 20 problems), you would find one that the AI model cannot solve even without modification. That is a good problem to draw inspiration from.


## Can you actually do this?


Although I have some physics olympiad experience, which helped, I opened my physics notes after 3 years. I was still able to make AI fail after revisiting those concepts. I think if you are a high school student with a good physics curriculum like JEE (Indian kids are especially encouraged to submit these problems), or if you have physics olympiad background at the international level or other local olympiads, you can definitely break these models.


I think you can systematically keep adding complexity until AI fails, and that is a skill you can get used to. If you were to give me the problems I created where the AI model was failing at, of course I would have failed at those problems too. I would have failed at the easier versions too if it was not for the handout solutions I was looking at. An elegant physics problem is like carefully hiding a golden egg. If you are the creator of the problem, you know the solution because you carefully crafted it. You don't need to be able to solve problems at that level yourself to be able to create them. Creating a hard problem and solving a hard problem are two very different skills, and you are absolutely capable of the first one.


---


## Turn your physics skills into income


If you can create a problem that causes a frontier AI model (such as ChatGPT 5.4 Extended Thinking or Gemini 3.1 Pro) to fail, we will pay $200 per accepted problem


, provided you also submit a complete, verified solution.


The bar is straightforward: the problem must genuinely stump a state-of-the-art model. Problems that defeat less capable models (such as Claude or Grok) or ChatGPT without extended thinking are also worth submitting. We evaluate these on a case-by-case basis and may offer partial compensation depending on quality and difficulty.


To be considered, your submission should include the problem statement, a correct and well-explained solution, and a note on which model(s) failed and under what conditions.


If you believe your problem meets the criteria, submit it for review and we'll get back to you.
