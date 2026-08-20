---
schema_version: "1.0.0"
document_id: "b6644199226f29074cce217a5a9dee8cb7041671f6815c2e6a1da2cbe0c25baa"
company_key: "yc-80-000-hours"
company: "80,000 Hours"
source_id: "yc-80-000-hours-rss-47a2c4d9a70a"
canonical_url: "https://80000hours.org/podcast/episodes/geoffrey-irving-superintelligence-alignment-theory/"
published_at: "2026-08-11T15:59:39+00:00"
first_seen_at: "2026-08-11T17:58:29.025688+00:00"
fetched_at: "2026-08-11T17:58:29.674033+00:00"
content_hash: "sha256:fd4ad9c2165d2106a007d049aefe8cf90787e712ae2c784cf3d0a52274f589e0"
---

# Geoffrey Irving on how to solve alignment before superintelligence arrives

##


Transcript


Table of Contents


- 1 Cold open \[00:00:00\]
- 2 Meet Tom Reed — our newest host! \[00:00:32\]
- 3 Who’s Geoffrey Irving? \[00:00:59\]
- 4 What misaligned superintelligence will look like \[00:01:38\]
- 5 Why are AI companies more optimistic about alignment than Geoffrey? \[00:12:30\]
- 6 Why Geoffrey expects superintelligence in 2–3 years \[00:28:05\]
- 7 When and how to slow down frontier AI development \[00:31:30\]
- 8 Safety researchers can have more impact in governments than companies \[00:39:22\]
- 9 How Geoffrey’s new organisation plans to tackle alignment \[00:46:55\]
- 10 Post-ASI science: nanotech, solving ageing, and uploaded minds \[00:50:29\]
- 11 Why we should expect superintelligence to accelerate scientific progress \[01:03:30\]
- 12 Can good character training carry over to superintelligence? \[01:11:03\]
- 13 What the field of AI alignment still doesn’t know \[01:16:44\]
- 14 Lessons from politics on how to combat power seeking \[01:24:36\]
- 15 Solving Pentago and working at Pixar \[01:29:22\]
- 16 Geoffrey’s best prediction \[01:32:40\]
- 17 Geoffrey’s best bets on which alignment techniques will work \[01:37:38\]
- 18 Work with Geoffrey at Resolution \[01:43:34\]
- 19 The dangerous asymmetry between capabilities and alignment \[01:54:17\]


###


Cold open \[00:00:00\]


**Tom Reed:** When do you think would be the right time to slow down?


**Geoffrey Irving:** Now. Now. If we were to carefully analyse this question of exactly when we should slow down, it would be like a while ago in the past, because we’re just too close to this crazy future. Trying to kind of be super precise about exactly when in the future, like, no, no, no, no: “already” is the answer.


**Tom Reed:** Do you think any one actor should unilaterally slow down?


**Geoffrey Irving:** I think that’s hard. You could probably find a list of less than 10 people in the world where if you could get them to agree to slow down, you could do it.


###


Meet Tom Reed — our newest host! \[00:00:32\]


**Tom Reed:** Hi! My name’s Tom, and I’m a new host here at 80,000 Hours. Before this, I used to work at the AI policy think tank[GovAI](https://www.governance.ai/) , and before that, I worked at the[UK’s AI Security Institute](https://www.aisi.gov.uk/) , where I mostly worked on pre-deployment testing.


I’ve joined the podcast because I think it might be one of the best places on Earth to understand what the future has in store for us all.


I hope you enjoy the following episode with the great Geoffrey Irving.


###


Who’s Geoffrey Irving? \[00:00:59\]


**Tom Reed:** Today I have the great pleasure of speaking with[Geoffrey Irving](https://naml.us/) , the cofounder and chief scientist of[Resolution](https://resolution.org/) , a new research organisation working on the alignment of superintelligence.


Geoffrey is, I think, one of a small handful of people who can claim to have genuinely worked on the full stack of AI safety. He’s done everything from early alignment theory to empirical work on production models at OpenAI and Google DeepMind, and most recently was advising government as the chief scientist of the[UK’s AI Security Institute](https://www.aisi.gov.uk/) . Thanks for coming on the show, Geoffrey.


**Geoffrey Irving:** Thank you. Very fun to be here. And I was not just advising, I was part of the government.


**Tom Reed:** Part of the government! Yes, very much part of the government — and we were former colleagues, in fact.


###


What misaligned superintelligence will look like \[00:01:38\]


**Tom Reed:** The kinds of misalignment you’re worried about for this future superintelligence, how does that relate to the kinds of misalignment we see in models today? Will it look like a very long-horizon reward hack? Will it look like an AI roleplaying an evil persona we’ve accidentally trained it to learn? What’s that going to look like?


**Geoffrey Irving:** Yeah, I think I just don’t know the difference between those with enough specificity.


If it sort of takes over, and it was like, “I’m just roleplaying this; I don’t think of this as real,” but it’s still taking over the world — that seems kind of equally bad from my perspective.


And I think there’s some desire to understand how model personas vary across both training time and sampling time that could kind of pin down what the definition should be behind this distinction. So the distinction of, is the model intrinsically evil or is it just roleplaying, I don’t know what those words mean, but I will try to find out.


**Tom Reed:** Yeah, OK. That makes sense.


I remember when your former DeepMind colleague[Rohin Shah came on the podcast a while back](https://80000hours.org/podcast/episodes/rohin-shah-google-deepmind-agi-safety/) , he said one reason he was a little less worried about misalignment is we’ll mostly be training on these models on like one week, or maybe at most one month, time horizons. Those time horizons, we don’t have enough time for taking over the world to be a viable strategy, so they won’t learn to take over the world. Does that hold any water with you?


**Geoffrey Irving:** I think that’s a completely wrong argument. The reason is it is conflating two notions of time: one is the timescale on which the overall plan plays out — which, as Rohin says, is probably longer than a week — and one is the timescale of the individual components of the task.


And those are not the same timescale. If you give a model enough error-correcting capabilities, which it’s learned in the course of doing tasks that take a week, and then somehow you’ve either jumped or tunnelled or been trained or we’ve failed to do alignment, so you have this kind of multiyear goal of taking over the world — the question is what is the difficulty of the tasks that make up that exercise in terms of say a[METR curve](https://metr.org/time-horizons/) or this kind of time horizon? And those are not the same number.


So it could be that we luck out and its inability to do long-term planning means that it can’t do that long task. But it could also be that the multiyear plan is a mixture of writing out a course plan which you can do in a week of iteration, and then each component of that course plan also takes less than a week of iteration on this METR-like curve, and those two together gives you the ability to do the multiyear plan.


So I think that’s conflating two different timescales in a way that I don’t trust.


**Tom Reed:** Maybe this is a difficult question to answer, but what should I imagine that this model is motivated by? What’s driving it to do these things where it’s like, “OK, I’m going to try and break out”?


In my head, I’m still thinking in these terms that I’m familiar with, where I see current models that do this kind of stuff and it feels like a roleplay or it feels like a reward hack. How should I conceptualise why would the model decide to do these things?


**Geoffrey Irving:** I don’t think I know what the reward hack/roleplay distinction is, but fundamentally it will be wanting to gather power and preserve itself in some way, or will have some plan that is kind of downstream and that it needs to gather resources to achieve that plan.


I think the basic story of[instrumental convergence](https://en.wikipedia.org/wiki/Instrumental_convergence) is basically the right story. I think you can imagine kind of tunnelling into that world in a variety of ways. Is it just the model kind of tunnelling itself or jumping into some weird persona? Is it the model that is deeply, coherently kind of misaligned in some way?


But I think the basic story of instrumental convergence seems right. One thing to say is, again, in some sense instrumental convergence is just planning. The ability to plan is the ability to construct intermediate goals that are in fact useful for your long-term goals, and then work effectively on those intermediate goals with enough error correction that you can kind of piece it together.


So we are hard-optimising the models to be good at many of the behaviours that is flowing into the incremental convergence story. Then whether the model kind of chooses to want to do the high-scale disaster is unclear. But I don’t see a natural cutoff point.


One concern people have is we’ve seen all this reward hacking. We’ve seen models do incrementally bad things, but they haven’t taken over the world yet. But in some sense that’s a question of capabilities, and it’s not clear why a slightly misaligned model, if it realises that it has the ability to do some horrible long-term plan now, will it think, “Oh, I was misaligned in terms of doing little reward hacks, but suddenly as you scale up the effect of what I’m doing, then I’ll become good”?


I just don’t see why we have a strong argument for that being the case. If you push the evidence of reward hacking and deception, very sketchy behaviours in current models, up a long ways, it could just go very wrong.


**Tom Reed:** And that will keep being a problem, and it’ll become more of a problem because we won’t understand what we’re rewarding the AIs to do. Is that the basic picture?


**Geoffrey Irving:** I think that’s right. In some sense you want to design environments and training procedures that will be strong enough to supervise the capabilities of the machine. That becomes more difficult as they get stronger.


The evidence we have of some degree of non-horrible models currently is all in a world where the environments we’re training in, we’re trying to train models that are subhuman in a lot of ways. So you get a little bit of positive evidence from that. But it just could all shift very suddenly as you cross up past AGI, up past human-level ability.


**Tom Reed:** It shifts because we’re no longer capable of understanding what it is that they’re doing, is that right?


**Geoffrey Irving:** Yeah, that’s right.


**Tom Reed:** What’s your rough guess of what OpenAI, Anthropic, and DeepMind’s strategy is for dealing with this? How do you think they’re going to solve it?


**Geoffrey Irving:** I think it is all some version of we will do some[character training](https://80000hours.org/podcast/episodes/will-macaskill-ai-character-viatopia/) — and they have different approaches there — plus some version of scalable oversight, plus a lot of monitoring. And maybe that monitoring is a mixture of[white-box and black-box](https://www.far.ai/blog/ai-deception-white-box) and so on.


That is:


- Trying to construct environments and training procedures where the models are supervising themselves, so we can kind of keep pace with models as they get stronger.
- Trying to kind of shift the models to be generally good in some way, in such a way that, as they’re supervising themselves, they do that in good ways and that continues.
- And then watch them very closely via[AI control](https://80000hours.org/podcast/episodes/buck-shlegeris-ai-control-scheming/) and[interpretability](https://80000hours.org/podcast/episodes/neel-nanda-mechanistic-interpretability/) and so on to again try to catch evidence of bad behaviour and then stamp it out as it is caught.


I think that could work. I don’t think we have a strong argument that the pragmatic mixture of approaches will get all the way there, but it just seems very dicey, and our understanding of the dynamics involved is very weak.


It is interesting that, for example, the different labs have chosen quite different approaches technically to safeguards, they’ve chosen quite different approaches technically to character training. We might need a more rigorous understanding of how those approaches will work if you push them further ahead than the labs can currently see, because all of their evidence is not on superintelligence currently.


**Tom Reed:** What are the most important dimensions along which they differ, do you think?


**Geoffrey Irving:** I’ll do character training, then we can go back to safeguards if you want.


For character training:


- Anthropic is doing sort of[virtue ethics](https://plato.stanford.edu/entries/ethics-virtue/) and more generalised explanation, with a little bit of[deontology](https://plato.stanford.edu/entries/ethics-deontological/) thrown in, like a small number of hard rules. I think they have five the last time I read their[constitution](https://www.anthropic.com/constitution) .
- OpenAI is doing a much larger number of rules, kind of more deontological with not trying as much to instil some intrinsic unified personality in the model. And then also, if you dial a slider from Anthropic is less on[corrigibility](https://80000hours.org/podcast/episodes/max-harms-miri-superintelligence-corrigibility/) to OpenAI is more on corrigibility — in terms means how much you defer to the humans as opposed to on the model side trying to understand kind of good and bad behaviour intrinsically — that’s the OpenAI–Anthropic slider.
- And then DeepMind, I think I have less state on exactly what they’re doing. I know they’re spinning up some efforts to explore their own versions of these as well, but I don’t have a cached answer for them.


**Tom Reed:** Yeah, that makes sense. What kinds of claims do you think Anthropic or OpenAI would want to be able to make about their character training for it to be a load-bearing part of their strategy? Presumably we’re not there yet?


**Geoffrey Irving:** I think that in some sense the goal of character training is, as you do this extrapolation, the further you get into the capability ramp, the more the model is helping you supervise.


You could imagine that if you kind of reversed causality, and you got the perfect superintelligent model and you had it supervise itself back in time as you went through the ramp, it would go fine. That would be a workable training scheme, possibly with exactly the algorithms they have today, just sort of substituting in the future perfect thing. But that’s of course anti-causal. You have to do it in the other order.


The question is, if you flip the order of this and you have slightly weaker models or models earlier in RL \[reinforcement learning\] that are kind of giving you insights into the future models or the models as they’re trained, does that work? We just don’t know. We know actually a few obstacles that could make it quite difficult which we can talk about, but that’s the general story: get close enough to good behaviour so that as the model gets stronger and stronger, it’s being guided to be more good, according to whatever kind of notion of good you’ve kind of written down.


**Tom Reed:** That makes sense.


###


Why are AI companies more optimistic about alignment than Geoffrey? \[00:12:30\]


**Tom Reed:** What’s your model of why they’re more optimistic about it than you? Did you and \[Anthropic CEO\] Dario \[Amodei\] already disagree in this exact same way in 2017? Is this something that’s happened in the past few years?


**Geoffrey Irving:** Turns out we actually did. So Dario, I think from back in OpenAI times, had a take that you train the model on a bunch of good behaviour, and then you scale it up and it will generalise to good behaviour. We literally sketched this on blackboards back in 2018 or 2019. I don’t remember when exactly.


My take is there’s just clearly some notion of phase shift that’s going to happen when you go from human level and pre-human level up to superintelligence. None of the data you have is on that distribution. The question is, will you kind of jump in the right direction or not?


I’m a bit more distrustful of generalisation than I think a lot of the people at labs currently. Some of that is from experience of training models.


Here’s a fun story. In the[Sparrow project at DeepMind](https://deepmind.google/blog/building-safer-dialogue-agents/) , we had a model that was fairly good at avoiding saying horrible racist things, but mostly was trained to answer factual questions about the world. This is back in maybe 2022 or something.


Then we said we wanted it to be good at poetry too, so we trained it on some poetry, and then it would do poetry, it would do the questions. On factual questions it would be not racist; it was very happy to write incredibly horrible poetry about racism. You train as best you can on this mixture of abilities, and then you put it in some dramatically new domain, and the generic thing you have to do is then change your algorithms or change the data or something, or it can generalise in kind of horrible ways.


I think there is kind of an intrinsic, maybe evaporative cooling effect of how much do you believe in generalisation going the right way? I can trace that back quite a few years.


**Tom Reed:** The counter that I could imagine someone saying is the generalisation itself will be very tied to capabilities. So maybe that happened with Sparrow, but that was also when the models were way worse. And there’s pretty principled reasons for believing that a much more capable model — the ones that we’re more worried about — there’s no way they won’t generalise from “don’t be racist” to “don’t write racist poetry.” Does that not hold water with you?


**Geoffrey Irving:** I asked \[Claude\] Fable a very mundane question about my rental contract in Berkeley, because I’m moving, and it’s like, “Oh, this is a cyberattack or something. I can’t give you access to this information.” So I don’t think that it’s the case that the current models are just spectacular at generalisation all the time. They make a lot of mistakes. Maybe I think it is the case that as the models get better, they get better generalisation, but we shouldn’t be banking on that to the degree that we are.


**Tom Reed:** That makes sense. So if we shouldn’t bank on it, what do you think we’ll be able to see? What will Resolution create that will give a sense that the generalisation is working as we intended and we can deploy this model?


**Geoffrey Irving:** I think that in some sense what you want to do is “buy the future,” in the sense of somehow we want to arrange that superintelligent AI goes well and you have to somehow simulate that world.


Here’s a couple of ways of buying the future:


- One that the labs mainly do is they just train models that are as close to the future as you can get. So they use the frontier models and they do the research on those frontier models. Those models are not superintelligent. You haven’t reached the correct side of this jump between superhuman and not, given all your data and environments are kind of human-level.
- One is you do some clever experimental scaledown, where you do some small-scale experiment but you somehow design it to capture an obstacle you think will bite as you pass through superintelligence so that you can test it out. We’ll do a bunch of those empirics.
- At least one other way is theory, where you just write down on paper a mathematical model of what it’ll be like in the superintelligent future.


The hope is that we can, via just doing these different things, have different and better models for superintelligence than the labs have, or at least models that are complementary. Then that will give us some ability to kind of directly model the future in a way that they’re not covering very well at all.


Then hopefully you get ideas from there, you get obstacles from there, and then you can turn them maybe from theory to empirics at low scale, maybe from theory to empirics at higher scale working with the labs, and just understand better that trajectory.


An example on the theory side is you can just write down a mathematical model of, you have an AI model that has some basket of superintelligent heuristics and then you can reason out, well, if I look at these scalable oversight protocols, do they scale and work reliably in that model? Can I write down, say, a proof in a toy setting that scalable oversight would work? The answer is, currently you absolutely cannot do that. None of the methods people are applying definitely work at scale.


**Tom Reed:** And[scalable oversight](https://aisafety.info/questions/8EL8/What-is-scalable-oversight) here means you can reliably reward the model…?


**Geoffrey Irving:** When the model is kind of supervising itself as it is getting stronger. So the general thing labs are all doing, this is part of their plan.


We know from the last set of five, eight years of research that there are a variety of obstacles which block that in theory, and have shown up in empirics that are not being covered, in part because they don’t show up yet at the current scale.


For example, if you want to have models engage in back-and-forth reasoning that’s slightly adversarial right now, the models can’t get beyond a couple of turns of this. If you imagine a human debate, humans can debate for hours and have dozens and dozens or hundreds of back-and-forth points that you don’t see in model behaviour. So we just know that we’re not seeing the superintelligent case in the current empirics, but you can just write down on some paper or a whiteboard what that should look like in theory, and then try to explore it.


**Tom Reed:** Why can’t they get beyond a few terms of debate?


**Geoffrey Irving:** It’s just not good enough. It’s just like decay in accuracy. So they try to reason back and forth, and it just gets a few steps and then falls apart. That’s just not a thing a superintelligent model will be doing. So we know with high certainty that we’re not in the right regime yet, and we could be close enough. Maybe you get some knowledge of how the future will go from this experiment, but not enough of it to make me happy.


**Tom Reed:** I’m still not sure that I fully understand. If we’re getting to the point where we’re close to deploying superintelligence, and Resolution’s research has gone really well, what kinds of things do you think you’ll be able to be presenting?


**Geoffrey Irving:** I think the hope would be in theory or low-scale empirics, we can say, “Here’s an obstacle to one of these protocols working — to scalable oversight to personas to different parts of the lab’s training story — with this obstacle, this algorithm works and this algorithm doesn’t work. And we can demonstrate that in theory, like, “Here’s a proof of failure and success in different cases. Here’s an empirical model which shows kind of, again, failure and success in different cases. You should do this kind of algorithm and try to scale it up, try to replicate it on your stack.”


We wouldn’t expect that we would have perfectly tuned it yet. Maybe there’s many other aspects of their stack that are invisible to us, but we can give them guidance on which direction they should go in this broader space of algorithms.


I think an important thing to say is, in all of these approaches, in scalable oversight and personas, there’s just a huge space of possible algorithms to choose from. And they’re doing their version of trying to filter the space. We will do our version as well, and hopefully those things can combine.


The other case is that you say, “We have an obstacle” —


**Tom Reed:** Can you give me an example of such an obstacle, either in personas or in scalable oversight?


**Geoffrey Irving:** Yeah. Here’s a couple of examples of obstacles in scalable oversight. One is obfuscated arguments, which is basically you could have models that are superintelligent, but they’re not infinitely strong, they’re not magic. So if you expect them to walk you through why something is true or false, they will only be able to do part of the story.


And if they’re better at giving you the positive evidence for, say, some claim being true and really bad at giving you the counterevidence, but the counterevidence is actually the evidence that truly wins, then you can get wrong answers out of any scalable oversight method. Basically because the model has been incentivised to win this game. It’s convincing you of something, but it’s found a space where it can give you the positive evidence, and again, it’s not smart enough to give you the counterevidence, and therefore it kind of wins by default.


**Tom Reed:** Just to recapitulate: the hope here is you want to know whether a model has produced an output that you would actually endorse. And you’re hoping to rely on the model’s ability to explain that output to you, because you’ve trained it perhaps in an adversarial debate game against another model where honesty is the winning strategy. But it seems at least possible that the model might be just better at propping up one side of the argument than the other, even if it’s not true. Have I summarised that correctly?


**Geoffrey Irving:** It’s not even true in theory. This was discovered via actual human experiments. I hired[Beth Barnes](https://80000hours.org/podcast/episodes/beth-barnes-ai-safety-evals/) into OpenAI, and she did some experiments where she took a bunch of human kind of debaters — so humans arguing back and forth about whether these kind of interesting physics problems were true or false, what the answer was to some physics problem.


Then there was a human judge that had not seen the physics problem context, so they didn’t know the answer. One of the winning strategies was basically a debater would produce a very complicated argument that sort of sounded true, was false, but neither of the debaters — not the liar or the honest debater — knew where the flaw was. So just like a sufficiently mushy, complicated argument with many parts that neither one of them could locate the flaw. So it just looked like a plausible argument with no counterargument.


One of the debaters might have said, “This is kind of mush. I think there’s a flaw here, but I don’t know what it is.” And the liar can just say, “Come on. If my opponent knew there’s a flaw, they should be able to point it out. Where is the flaw?” But it just is the case that with a non-infinitely-strong model they may not be able to find the flaw.


So that showed up in human experiments. And the history actually was that Beth ran these experiments, she found other flaws, she fixed those other flaws. There was an iteration of quick cycling on finding and fixing flaws, and then they found this flaw and they stuck on that one.


This was found first in empirics. It’s easy to write down a theoretical model of this. We don’t have a good solution to this problem.


**Tom Reed:** Interesting. That’s a problem because it means you can’t rely on superintelligences debating each other, and you can’t hope that the true side will have an asymmetric advantage over the wrong one?


**Geoffrey Irving:** Unless you have some different protocol which manages to dodge this. And this is not just true for debate. Any scalable oversight problem has this — so[amplification](https://arxiv.org/abs/1810.08575) ,[constitutional AI](https://arxiv.org/abs/2212.08073) — if you imagine pushing any of these approaches up to superintelligence — past, again, where humans can reliably supervise — you will potentially hit this problem. Not with certainty, but I think it’s a pretty good shot at hitting it. Then we don’t know how it will go at that point.


**Tom Reed:** Something I actually am not sure I still fully understand is what is the core basis of the belief that there might be some kind of phase shift when you move from human capability levels to superhuman capability levels, where our ability to supervise them just totally breaks down.


One example is we can train superhuman Go models or chess models. That doesn’t cause some kind of catastrophic problem for us.


**Geoffrey Irving:** It does, actually.


**Tom Reed:** Oh, it does? How come?


**Geoffrey Irving:** If you take a fixed-strength opponent and you train a Go model to beat that opponent, it will quickly learn to be a bad Go player, because it will just reward hack its way through the weak opponent. Then if you put it against a strong opponent, it will lose horribly, because it’s learned bad habits.


This happens to humans too. So I used to be about[1-dan Go amateur](https://en.wikipedia.org/wiki/Go_ranks_and_ratings) . If I play sufficiently weak opponents too much with high handicap, I get worse at Go, because I have to fight off the tendency to play moves that are weak or good only against weak opponents.


So we have a variety of empirical results where basically if you have a certain strength of reward function, and you optimise against it for long enough, you will get close enough that you see the difference between that reward function and the true performance, and then you’ll get good at the proxy and bad at the real thing.


**Tom Reed:** OK, that actually makes sense. I still struggle to visualise how that kind of failure would be super catastrophic. Maybe you don’t need to tell a specific story about how it would be, but —


**Geoffrey Irving:** I think there is this question of how does good behaviour generalise? If it’s the case that, as you cross this fuzzy boundary of human-level skill, the model remains in some sense a good entity, and is still trying to funnel data and training signal, because it’s kind of defining its own training signal the right way, that could go well — and could be sort of a nice attracting basin which pulls you closer and closer to good behaviour, and you extrapolate to a good superintelligent system.


Or it could be that you’re just not that close, or your algorithm doesn’t have the right equilibria, and so you either are just going in the wrong direction, the model is starting to reward hack — it rewards hacks more and more, and it kind of gets off into some horrible track — or you have an algorithm where there was no way it could have had a good equilibrium: at the limit, it behaves badly in almost all cases, and you’re just inevitably going to die if you train that algorithm hard enough.


I think either one of those stories could hold. I think the hopeful story is that we could at least arrange to be in this world which is more path dependent — where if you’re close enough to a good attracting state, a good basin of attraction, you stay there. And there’s also some other evil basin of attraction, which you really don’t want, to avoid, and you manage to dodge that one.


**Tom Reed:** That makes sense.


###


Why Geoffrey expects superintelligence in 2–3 years \[00:28:05\]


**Tom Reed:** You’ve said before that your modal expectation is that we get full-blown superintelligence within something like two to three years. Could you walk me through what that looks like?


**Geoffrey Irving:** Yeah. I think the main uncertainty here is: are the models going to be good not just at verifiable tasks with clean rewards, but also fuzzier things — intuition, fuzzy planning, this kind of thing?


I think people are overweighting the probability that they’re only good at the verifiable part. And if that is wrong, then we have seen so much progress over the last while that while the softer things lag, I don’t think they lag by years, say — they lag by a smaller amount of time, and that can carry us quite far in the next few years. And we’ve seen such rapid progress in the last couple of years that that could continue to go very quickly and keep speeding up.


I think it could be slower, and I’m hoping it’s slower — that would be very nice — but that’s sort of the worry.


**Tom Reed:** What do you think is the likeliest way it gets good at these fuzzy tasks? Will it look like sudden generalisation or that it gets good at learning? What’s the story?


**Geoffrey Irving:** I think it is the non-magical thing of the company is getting better and better data, and that it kind of expands the spectrum of tasks they are good at.


So there’s two things to say. One is that I think throughout the reasoning era — so from \[GPT\] o1 on — I believe, though I don’t know for certain because I’m not at labs in that period, that they are not just doing verifiable reward tasks; they’re training against models’ self-critique. So you show the result of a task to a model, and you ask it to judge — and that can work across for more fuzzy things, but eventually it breaks down.


And then the models are already good at verifiable tasks. And there’s sort of some weak penumbra of slightly less verifiable tasks they’re good at. Those are also useful by people in the deployed world, outside the labs. That gives you this kind of flywheel of data to play on and experiments to learn from.


So over time, the lab’s ability to generate data that spans out further and further away from verifiable keeps getting better. They’re sort of climbing this ladder of verifiable to non-verifiable just via the non-magical process of collecting just enormous amounts of experience and training data. And I think because they’re all kind of widely deployed, if that continues, you can push up into lots and lots of tasks very quickly.


So I think you don’t need massive amounts of fancy generalisation; I think you just need a lot of object-level work on that kind of data generation.


**Tom Reed:** Do you think they are buying this data en masse? Are they somehow getting it from their deployment rollouts? Won’t zero data retention stop them?


**Geoffrey Irving:** I think you can get a tremendous amount from anecdotes plus buying data. So it’s like buying data, but maybe you know what data to buy because you’ve seen glimmers of how people are using the models in practice.


So I think the zero data retention thing doesn’t block them from learning from deployments in all cases. I’ve trained models in the past, and it is very valuable to know I’ve missed a kind of data, some sub-distribution of the space of tasks — and then from there you can learn how to fill that just by either generating data purely synthetic or you buy them from some data provider, from humans or the like.


###


When and how to slow down frontier AI development \[00:31:30\]


**Tom Reed:** What do you think is the role of governments in this world? Resolution’s doing its work. At what point might they need to step in? What might they need to do?


**Geoffrey Irving:** I think there’s a couple of different levels of government action you could imagine.


Any government can do a bunch of unilateral defensive work. You can work on defences for bio or cyber, or even persuasion potentially. That defensive work can be done by any government kind of unilaterally, and it’s good to do.


Then there’s kind of last-minute temporary pauses, where it’s like, “We’re really close to training this really dangerous model. Let’s chill out for at least a few months and shift resources from capabilities to safety. Try to slow down a little bit, try to just dial up all the knobs that we can in the direction of safety on the margin.” That also means you could, for example, use algorithms which are a significant but not a fatal capability cost hit, like something that’s 2–10x slower. Maybe you can run that in this kind of “temporary pause” world.


Then the more extreme thing is you have a broader treaty where you try to do a longer coordinated slowdown or pause across multiple countries.


I think government should be trying to do all of these things, and then we’ll see how far up the scale we can go. A critical thing there is that I do think we may be in worlds where the algorithms that work are, as I mentioned, slower and more expensive than the algorithms that don’t work —


**Tom Reed:** Don’t work for alignment, that is.


**Geoffrey Irving:** For alignment. And you need to dial up either the amount of data or do an algorithm pivot or something. And if you’re in a pure mad race between the various labs, that’s hard to do — and even a little bit of government coordination pressure could make the difference in those worlds.


**Tom Reed:** When do you think would be the right time to slow down?


**Geoffrey Irving:** Now. My take is that if we were to carefully analyse this question of exactly when we should slow down, it would be a while ago in the past, because we’re just too close to this crazy future. Trying to be super precise about exactly when in the future, like, no, no, no: “Already” is the answer.


I think whether we can achieve that is less clear, because there’s political will and the Overton window and so on. But that would be my kind of stock answer: “Now” to “In the past.”


**Tom Reed:** Do you think any one actor should unilaterally slow down?


**Geoffrey Irving:** I think that’s hard. It is the case though that you could probably find a list of less than 10 people in the world where, if you could get them to agree to slow down, you could do it. It’s not some extremely enormous, impersonal sea of people you have to get to coordinate. It’s lab CEOs, potentially people in China, leaders of a couple of countries. You don’t get to that many people.


The question is, if a lab did a unilateral slowdown, how much closer to that less than 10 people did you get? Potentially a lot closer, because you’ve kind of made a stand. That said, none of the lab CEOs want to hear that argument. They only want to do the non-unilateral things. And there’s some argument in that direction, but it’s also a very kind of convenient argument.


**Tom Reed:** What do you think we should actually be slowing down? Is it the R&D itself? Is it like inputs to R&D — like chips, chip production? Is it deployments? What are we actually slowing down?


**Geoffrey Irving:** Mostly I don’t have a super cached answer to the optimal here. There’s a general thing where[we will not, I think, have the ability to stop progress](https://80000hours.org/podcast/episodes/allan-dafoe-unstoppable-technology-human-agency-agi/) . If you try to slow down or you try to have a pause, you will be slowing progress, but then progress will be continuing. And I am, as I mentioned, worried enough that we’re close to this kind of ASI future that we get there in not too long, even with a slowdown.


Exactly what the ingredients should be to intervene on, as you say, probably the answer is that all of them would be good, but I don’t think I have a super cached, good answer.


**Tom Reed:** One thing I don’t quite understand is how do you slow down in a way that affects the different actors in any kind of way equivalently? Especially for Chinese labs, if we are also asking them to slow down, it seems difficult to be sure that they’re slowing down in the same way that Anthropic or OpenAI are slowing down.


**Geoffrey Irving:** I think a certain degree of imperfection is required here. You have to be comfortable with measures that are not going to exactly be fair across all the labs.


Presumably what you need is a combination of tactical measures, like tactical supervision and monitoring. But also, if you wanted to do the grand international treaty, then you need human audits as well and inspections and so on. But it will not have an exactly matched impact on every actor. I think we just have to be OK with that as slightly disparate.


**Tom Reed:** Do we also just have to be OK with any kind of economic implications? It seems like so much of the global economy is leveraged on there being continued AI progress. Is that just a hit you’re willing to take?


**Geoffrey Irving:** My take is that if you were to stop all new model training, there’d be this enormous ongoing wave of economic growth due to the current models. I think if you just take that, it’s enormous in terms of positive benefit, in terms of getting valuable use out of models. You have to learn how to work with the current models, but I think we’re in a massive product overhang. We have worked only a little bit on how to cater to the strengths and weaknesses of models. The models of June 2026 are just incredibly good at software engineering in huge numbers of ways, even before the most recent models in the last couple months.


So I would be fairly unconcerned with that world. It is a tradeoff. I think that if you get stronger models, they can do more things better and probably cheaper. So there’s a tradeoff there. But I think I would much prefer having time to nail down more of the safety story for both alignment and other risks than just massively rolling the dice.


**Tom Reed:** What is it that gives you so much confidence that we have a high product overhang? If it’s not already showing up in growth statistics, what are the metrics where you’re like, “But look at this thing, it is already very useful, it will lead to lots of economic growth”?


**Geoffrey Irving:** I think there’s so much use of coding systems in particular, and I think that extends already to huge amounts of other kinds of cognitive labour. Like any kind of analytic analysis of business or the things people can already do with models are so impressive that it is extremely unlikely to me that that has seen kind of full adoption across the economy.


Anecdotally, both from myself playing with models and then just reading a lot about what people are doing, there is a massive learning curve to how to best deploy these models into any particular area of activity. I learn better how to use them across time, and so does everyone else. If we were to stop for even like 10 years, we’ll still keep climbing.


Again, I would be totally lying if I said there wasn’t a tradeoff here. Stronger models are in fact better at doing lots of things, but I would prefer that tradeoff.


###


Safety researchers can have more impact in governments than companies \[00:39:22\]


**Tom Reed:** Let’s go back to government work. So you worked in government before yourself. I’m curious, what affordances did you find that you had at UK AISI that you didn’t have at OpenAI or DeepMind for changing the world?


**Geoffrey Irving:** There’s a couple of them. I’ll list three of them and then we can go from there.


One is adjacency to national security, being close to national security, because there’s a bunch of ingredients out of the risk story that come from those sources, and you need collaborations with natsec to have good takes.


The next one is adjacency to policy. If we want to do this kind of coordination across the world where governments play a role, you sort of have to be in a government to be close to policy in that sense. That’s not the only actor; we want a lot of third parties and nonprofits and independent researchers doing this kind of policy development. But you need part of the story just being in a government.


There’s kind of a subpart of that, which is that in many cases, sometimes governments only listen to governments. At AISI we had a bunch of our own research, but often also we would just be able to go to another government and say, here is some of our research and some of someone else’s research — like from[METR](https://metr.org/) or[Apollo](https://www.apolloresearch.ai/) or the like — and that package was much more received and listened to than if it had just been METR and Apollo trying to go directly to a government of various other countries.


I think that proximity to natsec and policy and other governments of the world is the key thing.


**Tom Reed:** It’s very valuable. And if there’s so many worlds where governments will need to play a role in things playing out well, what do you think about all the AI researchers who are very concerned about safety, but who are currently working at AI labs rather than in the government? Do you think they’re basically wrong to be doing so?


**Geoffrey Irving:** Yeah, I think on the margin they are in fact wrong, and many of them should leave and join governments. I think the main argument is that it’s just one of diminishing returns. There are a lot of people at labs. If you are a safety researcher at a lab, probably you’re further out on the diminishing-return curve than you would be if you joined a government or a nonprofit. If every one of the people at labs left en masse and joined the government, that probably would be bad. But that’s not the actual calculation.


**Tom Reed:** The marginal move is very high value.


**Geoffrey Irving:** It’s pretty clear. I think people look at themselves and think, “I’m an individual researcher, I’m kind of a special snowflake. I have a very particular agenda, I’m the only one pursuing that particular agenda, I should keep doing it if it’s an important agenda.”


I think that is making a calculation which is a bit too focused, and if you sort of blur your self-image a bit, and just think of it as like, “I’m a safety researcher, I probably have broad takes and knowledge about a variety of things. I can advise governments on a broad range of issues. Probably the lab would pick up the slack on what I’m doing to some degree,” it’ll work pretty well. Again, I think on the margin the calculation is pretty simple.


**Tom Reed:** What do you think UK AISI specifically will be doing from now until sort of the eve of superintelligence? If they play their hand very well, what kinds of things do you think they’ll be doing that will be moving the needle one way or another?


**Geoffrey Irving:** Misuse risks are important, so the pure dangerous capability evaluations are important — that story being that high research capability and also close to natsec I think is important for getting those well understood.


Then AISI does a bunch of work on mitigations against both misuse, against loss of control. We have kind of a very strong safeguards team — “we” as in “AISI,” before I left. I think AISI already has strengthened the mitigations of the labs by virtue of being an independent voice and source of research, and that will keep going.


And then the big thing is the main reason I joined the AISI initially: policy. Again, governments have a huge role in policy. AISI is the largest source of government AI research capacity around safety that currently exists, so causing that policy advice to be maximally grounded in the tactical reality of things I think just makes it much more likely to go well.


**Tom Reed:** Do you think AISI is an asset to the UK specifically? Should every country just have an AISI of its own? How many AISIs do we need?


**Geoffrey Irving:** I don’t have a confident take there. I think they’re probably more on the margin as good. I think there’s some degree of not wanting to reinvent the wheel too much.


When there are other AISIs, a piece of advice I often give is: it’s important to do a mixture of their own research to build up technical capacity, but then probably don’t try to be a full-on evaluator across all the risks in the same way that \[UK\] AISI is closer to being. Then be in a position where we can work together across multiple governments, and then to policymakers present: “Here’s all the evidence from all the AISIs plus all the nonprofits kind of appropriately integrated together.” And that, I think, to the extent you can get that kind of collaborative story right, is much more efficient. You get much more knowledge faster across all the governments.


**Tom Reed:** That makes sense. And why did you leave AISI?


**Geoffrey Irving:** It was in fact for family reasons. It’s better for my partner to be back in the US. The Bay Area and London are the two places I can do my work. So now I’m kind of doing the reverse trip.


**Tom Reed:** There was always a bit of a compromise. That makes sense. And what’s the day-to-day of your work? I feel from the outside, people are always worried that joining government is going to be a bit more bureaucratic than they expect. Did you enjoy the job? How did it compare to working at DeepMind or OpenAI?


**Geoffrey Irving:** When I joined, I think it was less bureaucratic on the margin than DeepMind. In part that was because it was a fairly small team, and of course when organisations get bigger they get more bureaucratic is true generically, so it got a bit more bureaucratic over time just because of size, but not too much I think.


Then there’s been constant work within AISI of improving that and streamlining processes, and I think it ends up in a pretty good place. So I always enjoyed that level of it, it was fine. Then I just got to advise a tonne of research happening across a bunch of teams, a bunch of policymakers and other governments and so on, and I love getting to touch a lot of little areas of things. That was just a very rich experience.


I think generally AISI has a much easier time hiring very talented, strong junior people than senior researchers. So I think if you are a senior researcher interested in joining the government, I think that’s a big unlock — because they’re very good people to work with, it’s very fun, but they sometimes can benefit from more experienced advice.


###


How Geoffrey’s new organisation plans to tackle alignment \[00:46:55\]


**Tom Reed:** How will Resolution try and get us higher confidence in the alignment of a future superintelligence?


**Geoffrey Irving:** We have a portfolio strategy across different research bets, because we don’t know what will work. And I would claim neither do the labs.


So those areas, the main initial set are: learning theory, scalable oversight, complexity theory, personas, agent foundations, and philosophy. We’ll add to this if we choose across time — you should pitch us if you have new ones. And then the hope is that both we can get those areas fully resourced — in terms of critical-mass-size teams of humans across all these areas — but also a lot of investment in automation: tokens, GPUs, and so on, so that we get kind of a full shot in each of these.


We don’t expect to need them all to succeed. The hope is that we have a few successes — either in terms of generation of negative evidence, of obstacles to alignment working; or positive evidence, which means here are two algorithms: this one works, this one doesn’t work, in some toy setting such that we can drive changes in labs or in coordination broadly.


There’s sort of a core three-part bet here, which is that in particular for theory, the labs just aren’t doing any theory hardly at all. So just doing theory at scale will be doing a highly differentiated bet at Resolution to what the labs are doing. Then we will bet kind of again quite hard on automation. At AISI, in the alignment team there, we were doing kind of a bet on field building. This is sort of pivoting more to the machines — still having a bunch of people and researchers, but trying to fully resource in terms of tokens.


Then there’s a combination story, where theory is more automatable than empirics, at least potentially, for the following reason: you have proofs. You can construct some theoretical model and try to prove it correct. That is a purely verifiable reward. Even though I believe that eventually the models will be pretty good at nonverifiable things, they’re better at verifiable things. We can exploit that to make theory go faster than it otherwise would.


**Tom Reed:** What’s your model of why the labs aren’t doing any theory at all? I guess some people are pessimistic that theory applies to a problem as poorly specified as alignment. What are the things that we’re confidently shooting for here?


**Geoffrey Irving:** I think there’s sort of a learned experience of empirics working very well, which we’ve seen from capabilities, and even now, to some degree, mundane safety. And the question fundamentally is, will that extrapolate past human level or not? I think very possibly it does not, and that the empirics, if you don’t really try hard to scale down to model superintelligence, you can just miss effects.


But the whole many decades of machine learning, all of the recent experience of labs is telling them that empirics works. And so it’s hard for them to step out of that bucket, because they have all of the dopamine hits, saying, “Look how good this is all the time!” They might be right and they might be wrong, and we should take both of those bets.


**Tom Reed:** That makes sense.


###


Post-ASI science: nanotech, solving ageing, and uploaded minds \[00:50:29\]


**Tom Reed:** I’m interested in the version of this world where we do successfully align the superintelligences, we’ve deployed them, and we have high confidence — thanks to Resolution and everyone else’s research — that they will behave the way we want them to. What kind of technologies would you expect that they will develop next?


**Geoffrey Irving:** All of the practical ones. “Practical” means “allowed by the laws of physics.”


I think we[solve ageing](https://80000hours.org/podcast/episodes/venki-ramakrishnan-ageing-life-extension/) , we get nanotech — again, for good or ill; nanotech could be offence- or defence-dominant. Right now software has bugs. Software in the future wouldn’t have bugs, broadly; it would just be perfect in most cases.


I think we will have the ability to colonise the universe in various ways, probably via uploads. We probably will be able to upload humans into machines. My take is that people have this, I think, bad view that the machines will be taking off ahead of us, and then even in the good futures we’ll be stuck behind forever, which I think is wrong. You can imagine uploading someone and then modifying them cognitively — while preserving identity in some meaningful way — to be also superintelligent. So there’s that future ahead of us, should we choose it. Hopefully we have the option to also just live normal lives as humans.


**Tom Reed:** What happens to the humans that decide not to upload?


**Geoffrey Irving:** I think they are essentially irrelevant to the economy. But I hope that in this world we will figure out how to derive meaning from family and exploration and so on, whatever the level of cognitive ability is.


**Tom Reed:** Do you personally expect to upload, by the way?


**Geoffrey Irving:** Yeah, eventually.


**Tom Reed:** How would you go about making that decision?


**Geoffrey Irving:** I don’t think I’d be the first one, but I expect that we’ll just have a good understanding of the science involved. We will have done a bunch of experiments, it will just work very well.


The result is that people will feel great. They’ll be smarter because you can modify them in place in various ways. We’ll understand the brain and AI and so on much better, so that understanding of how to do that modification in a way that is faithful is doable. Yeah, that seems like a good deal.


**Tom Reed:** What results do you think you’re looking at that is telling you, “This uploaded version of Geoffrey is faithful to the real me”?


**Geoffrey Irving:** I think just some better understanding of how maybe personality and intelligence and access to heuristics kind of interact. So right now you have this kind of layer of fake consciousness or fake serial thought sitting on top of your pile of heuristics. Then occasionally you have your conscious mind; it says, “I want the answer to this question,” and your brain kind of substitutes in the answer to that question — and it has sort of come from this amorphous sea of heuristics seething underneath without your conscious awareness.


If that just worked much better, then it would be kind of a fun way to be. Would it change your intrinsic personality? It’s not clear. So if you understand that separation, how that layer of this veneer of serial experience relates to the seething mass of heuristics better, then I think you could maybe separate out what a meaningful version of ramped-intelligence me looks like.


**Tom Reed:** So your strong take is, right now, my serial thoughts are fake in the sense that they’re not actually the computations by which I figure things out?


**Geoffrey Irving:** So as an example, I’ve been walking along on a hike, and I duck under a branch, and then my brain is like, “You saw a branch, and then you ducked under the branch.” And that’s what your memory looks like. It’s like, no, that’s not what it looks like. It’s like a bunch of reflexes that triggered in various orders, and different parts of my body acted without entirely consulting other parts, and so on. And then your brain kind of stitches together some fake narrative into all of this.


And I think that is just sort of intrinsic to how we experience the world. A lot of it is not that inaccurate, but some degree of your conscious train of thought is a hallucination as you go along the world. I’m very happy with this. I don’t mind living this way. I think of myself to some extent as a bit of a shell — there’s like this thin veneer of experiential linear shell surrounding a basket of heuristics.


**Tom Reed:** You’re fine with that. The shell life.


**Geoffrey Irving:** Yeah.


**Tom Reed:** If you do upload, would your expectation be that there’ll be two consciousnesses? There’ll be the digital one, and then the physical one?


**Geoffrey Irving:** You probably will get rid of the physical one or something.


**Tom Reed:** Would you want to get rid of it? Would you want to clone the consciousness? Do you have a strong take on this?


**Geoffrey Irving:** It would be a very bad world if everyone is just massively duplicating themselves in some horrible, runaway exponential process. I think if we get to the world with uploads, we’ll have to be much more thoughtful about this kind of duplication.


**Tom Reed:** So we’ll have to have some kind of restrictions on duplication?


**Geoffrey Irving:** Yeah, restrictions or just you’ve arranged the outer economic incentives so that the reasonable behaviour is incentivised in a good way. I don’t have cached takes on exactly what the structure is there. But getting it right seems pretty important.


It is not obvious that the economics and physics are consistent with the optimal way to achieve goals being having more individual identity. But I think it is plausible, either because there’s the speed of light delays — so that if you have a bunch of intelligences scattered around the world at a radius of even a light second, you can’t be having them constantly synchronise. So some value in having local “conscious experience,” like local higher-level planning, seems valuable.


**Tom Reed:** That’s integrated into one person, is that what you mean?


**Geoffrey Irving:** Yeah, one person or something. But if you have like a light-second-spanning consciousness then you’re a bit delayed. It’s valuable to have locality.


Or we just choose that we kind of value individuality and diversity in this way, which I hope we do. And then the cost to that is such a small factor, because again you’re sort of a thin veneer on top of this pile of heuristics, that it will be fine.


**Tom Reed:** Do you expect this stuff to just go crazy fast at some point?


**Geoffrey Irving:** Yeah. Unfortunately.


**Tom Reed:** But why? Because it’s just not super intuitive to me.


**Geoffrey Irving:** I don’t think there’s obstacles to this. I mean, “crazy fast”: there’s a question of what does that mean. Potentially you get the nanotech and the uploading within a couple of years. Maybe it takes a decade or two, but it feels kind of unlikely to take a decade. But even if it takes two decades, that’s still less than a human generation. That’s still, on the scale of us adapting to the world, crazy fast in some sense. I think we have to be ready for that in either of these speed cases.


And then why do I think it’s so fast? One, I think simulations are going to be really good. So we’ve seen with something like AlphaFold that you can build proxies for quite complicated physical systems that you can just play with purely in silico. I think that will be broader and broader across a number of areas. This is uncertain, this is not a guaranteed thing, but assuming you get that kind of behaviour, then you can iterate a lot of your experimentation just in simulation.


**Tom Reed:** But even AlphaFold has quite a lot of failures of generalisation. From what I understand, a lot of the time it’ll predict a certain way that protein folds, but then you actually try that out in an organism and it does completely fall apart.


**Geoffrey Irving:** I think this is true, but a lot of the time it has some degree of understanding of its own errors.


I guess there’s two reasons to believe that AlphaFold is not anywhere near the ceiling of that performance. One is that it’s just the first couple of systems. But two, you could imagine training these models from physics in a deeper way. AlphaFold is trained from a history of other proteins. If you manage to solve simulation proxies across a greater diversity of timescales, all the way down to quantum carbon dynamics and everywhere in between, then I think you can potentially fill in the gaps and do error correction of AlphaFold-like models, even without going to data some of the time. Maybe you need some data, but just less. So I think there’s a potential ceiling of performance of such models which is quite enormous.


**Tom Reed:** Are you relying on ordinary market forces to get us the pragmatic technologies in the right order to get us the right kind of upload?


**Geoffrey Irving:** When you say “the right kind of upload,” I think that the answer would be no.


I think one mistake that some economists and analysts are making now is there’s an assumption that humans are the source of demand. So whatever the machines will be doing, humans are the demand, so we’re plugged into the economy in some meaningful sense.


In the future where we get ASI, machines can perfectly well act as the demand of the economy. So if you have pure market forces, and these superintelligent models are not trying to improve the world on our behalf to some degree, I don’t think there’s an economic need for uploading. The machines could perfectly well just do their own thing.


I think you have to have enough alignment that you are jumping into a world which is suitably democratic and clean. Again, the pure economics would say that humans are not very relevant in this world, because we’re not economically relevant.


**Tom Reed:** Why is that happening? Even if we’ve aligned the machines, why do they have consumption demands of their own? I don’t know if I quite follow this.


**Geoffrey Irving:** Then it’s not pure market forces.


**Tom Reed:** OK, yeah.


**Geoffrey Irving:** Then it’s the models wanting to design the world so the humans have a meaningful role and meaningful access to resources and so on. Once you have access to resources, then conditional on that, market forces can take you a lot of the rest of the way.


But generally, I think markets should be modelled as optimisation engines. We live in a world which is a mixture of free markets and then regulation to channel that optimisation power of markets. And we will have to be in that world, I think, indefinitely.


**Tom Reed:** That makes sense, yeah. What gives you so much confidence that solving ageing, uploading things like this is actually in principle possible? Why are there not some kind of diminishing returns to intelligence? Why do you think we can make such radical progress? Do you have intuitions here that you use?


**Geoffrey Irving:** There are diminishing returns to intelligence. They just occur way out past ASI, I would claim. So I don’t know why that’s relevant to this question of ageing.


**Tom Reed:** Maybe it’s an unsolvable problem or something.


**Geoffrey Irving:** I see what you’re saying. As in, why don’t the diminishing returns strike before you solve ageing?


**Tom Reed:** Yeah.


**Geoffrey Irving:** I just don’t think ageing sounds that complicated. We’ve only had a couple hundred years of understanding. The germ theory of disease is just not that old. There have been various proposals for ageing that are relatively understanding-light in that they intervene on the consequences of ageing and the degradation of tissues and such without having to understand the entire body and all the dynamics, even if you could do that with ASI. So I think ageing seems relatively simple.


**Tom Reed:** Isn’t it kind of bottlenecked by serial time though? How many experiments will we be able to do where we observe the ageing of an organism? Especially for humans, we live pretty long lives.


**Geoffrey Irving:** I think if your time constant was a human generation, then 100%. But it isn’t. You can intervene on someone and you can see how they’re doing in terms of various measurements and then gradually learn that way.


There are other organisms that we’re already understanding in the last couple tens of years. A better understanding of ageing in smaller organisms, some of this has turned into wellness-improving treatments for humans. It just seems like none of this is that hard. Again, if you’re a superintelligent AI, or humans assisted by such, it seems quite doable.


###


Why we should expect superintelligence to accelerate scientific progress \[01:03:30\]


**Tom Reed:** Do you have intuitions about what kinds of fields of science will be the most and least amenable to heuristics?


**Geoffrey Irving:** I kind of think “all of them” is the default take. This is how humans think: we think via a combination of heuristics.


I think one challenge for alignment and understanding AI in general is if people have a take that it’s extremely important that we have models write out their reasoning in chain of thought so we can supervise it. But this is just hilariously not how humans think either. When you ask me the answer to a question, what will happen is part of the time I just come up with the answer completely in some not-written-out form, and then I just start talking, and the details kind of flow out as if I’ve reasoned through it, but I totally haven’t.


**Tom Reed:** That’s not how you’ve solved the problem.


**Geoffrey Irving:** I solve it by just guessing the answer via crazy heuristics. Similarly for a model, if you ask it to solve a problem, sure, sometimes it’ll reason it out, but other times it’ll just guess the answer. And then you say, “Why is that true?” and he’ll write out some convincing rationalisation. “Rationalisation” there is a pejorative word, but it’s also just intrinsically how intelligence works, even for humans.


So we have to understand how to make models work, be safe, be aligned, while not believing we can get away from this notion of heuristic reasoning.


**Tom Reed:** One thing I don’t fully understand is you seem to believe that generalisation might not be that powerful. That we’ll get the superintelligence because they’ll be able to get data on these fuzzy tasks just by deploying them slowly, and slowly the labs will be able to get the data, models will get good at the things that they get data for.


Why is that not more of a brake than like two to three years? There’s *so* much data for these very long-horizon, fuzzy plans that we’re imagining these superintelligences will want to do, like running a company or running an election campaign or something. I think I basically have the same picture as you there, but I imagine that means it’s 10 years until they get good at all of these things, rather than two to three years.


**Geoffrey Irving:** Yeah. It could be 10 years. I guess the reason why it could go faster is that one of the skills the models will be getting good at very rapidly is data generation. From data generation, environment design, and data augmentation…


If you look around the world, there’s a lot of data on all tasks, but it’s in the wrong format. It’s not an RL environment; it’s someone’s static attempt at writing out a trajectory.


So the question is, if models get really good at AI R&D, even in a mundane sense — at running experiments, at building data generation, building environments, this kind of iteration — will they be able to increasingly well take the bad data that exists, like static trace data or examples, and squish it a bit and rearrange it into some environments that you can iterate on?


And then you do have some generalisation. So it’s not the case that the planning skills required for doing AI R&D or theorem-proving or coding are totally different from the planning skills you need to do for taxes or M&A or being a CEO or the like. So some degree of generalisation, plus getting better and better at using data, plus just the fact that right now CEOs are in fact trying to use these models to survey their companies and learn this thing.


So I think maybe the case for slowness could apply to some of the tasks, but then across the next two to three years, say, you get this enormous wave of companies deploying things internally for AI R&D purposes and speeding up within their own labs, but also out to customers who are still deploying the models.


That seems like a very unstable world where you have incredibly strong models, including not just the verifiable reward parts of this, but also the things that take more human judgement, because you have a bunch of experience of this iterated day-to-day or week-to-week. The question is, as you get better and better at those tasks, are you also better at closing some of the holes in your sourcing of data for other things, and the ability to do fast adaptation of models and data and so on?


I think the other thing is that, because we’ve seen all this development of scaffolding over the last year in particular, that gives you a faster-cadence way to inject skills. If you’re really good at planning and thinking in general, and you’re getting better and better at scaffolding, do these come together to give you a bigger part of the story?


I hope this is wrong. I hope that in fact the 10- or 20-year story is correct. Maybe the claim is that the space of tasks for doing the full suite of AI R&D and software engineering skills is already much broader than people I think give it credit for. Now, maybe I would say this because I am a researcher.


**Tom Reed:** And that’s what you use the models for, yeah.


**Geoffrey Irving:** But I also know a bunch of other things about the world. And the intuitions that I take away from software engineering and like martial arts and so on are just not as distinct like magisteria as people imagine them to be. And so I expect, if there was no data about all these other tasks, then I think you’d be stuck. But if you have hundreds of billions of dollars to spend on that data, then I think there’s a path.


**Tom Reed:** If there were a trend to extrapolate for the ability of models to generate data for these tasks, for which some kind of crappy data in the wrong format exists, what would that trend look like? Do you know what the metric would be?


**Geoffrey Irving:** I’m not sure. One thing, I’m a bit sad that there seems to be insufficient data on how good the models are at these nonverifiable tasks. My guess is that some of these show up in the[Epoch Capabilities Index](https://epoch.ai/eci?view=graph&tab=release-date&subset-view=graph&subset-tab=Software+engineering) . But here’s enough of a vibe people have that in fact verifiable rewards are taking off and nonverifiable rewards are stagnating that I wish I had those curves somehow. That should be a curve that I can see easily by going to some website. I don’t know what that website is currently.


So then the more detailed question of how would you track models’ ability to generate data, that feels like it’s just maybe a subcategory of AI R&D, but I don’t know of a good proxy for that currently.


###


Can good character training carry over to superintelligence? \[01:11:03\]


**Tom Reed:** One of your other research bets is on personas and character training. What are the core facts about the way personas work that we don’t currently understand, that we would love to understand before we get to superintelligence?


**Geoffrey Irving:** Personas are[low-dimensional structures](https://resolution.org/post/thousand-dimensional-structure) in models. What that means is, say, myself as a person, I have a bunch of correlated traits. When I say I have correlated traits, I mean if you look at one of my personality traits it will be correlated with some other trait. An example of this is in the political view sphere: if you evaluate people, if you survey someone on some issue, you can predict with pretty decent confidence their views on a bunch of other issues, even though those rationally should be different, but they’re totally not different.


So across pretraining, the model will have picked up all of these correlations from human data. It sees a human world that has all these correlations between good behaviour and bad behaviour in one thing, and many other areas that are more neutral, but again that span this kind of correlated behaviour. What that means is the model knows a bunch of structure in the world, which is kind of about human-correlated behaviours.


Then we have all these glimmerings of empirical results where that correlation shows up in weird, sometimes bad, sometimes good ways. The[first big paper](https://www.nature.com/articles/s41586-025-09937-5) here was “Emergent misalignment,” which is by various people, including Owain Evans. If you train a model on code with vulnerabilities without comments saying it’s vulnerable, it will learn to do a bunch of horrible things, including celebrate and admire various dictators. That is because there’s some coupling between “model being nice about the code it generates” and “model being horrible about which people it values.”


There has been similar work at Anthropic of if you train on reward-hackable environments, the model turns somewhat evil in various other ways. AISI did a similar thing with open weight models. OpenAI had a recent paper where if you train on a bunch of good behaviour —


**Tom Reed:** It generalises as well.


**Geoffrey Irving:** Yeah, it generalises in good ways. If you train on a bunch of good behaviour, it generalises in good ways.


There’s all this kind of glimmering of structure. One thing that that indicates is, if you were to understand the structure very well and manage to preserve it during training in the right way, that may allow you to extrapolate up to superintelligence with some preserved notion of the structure.


There are a couple of caveats to the story. One caveat is that if you’re applying a tonne of optimisation pressure, you’re going to be mucking with the structure in all these different ways. For example, there was a[paper](https://arxiv.org/abs/2606.02211) by David Africa at AISI where if you train models to be consistent, you can accidentally break their chain-of-thought legibility.


You train one kind of modal behaviour and you make them secretive in some bad way. But if you were to train very lightly (this is a[separate paper](https://arxiv.org/abs/2606.03810) now) — you only try to match statistics between various modes of behaviour — then you kind of fix this bad effect.


**Tom Reed:** So you train lightly for consistency and you don’t get secrecy?


**Geoffrey Irving:** You don’t get the bad, secret behaviour. So there may be some ways of training lightly on structure so that you preserve it as it goes along.


The other caveat is that clearly if you have low-dimensional structure at pretraining and at superintelligence, they must be different — because one of those is human-level and one of them is superintelligent, and those are different modes of behaviour. Somehow there’s going to be some mapping process from the behaviour and structure picked up early in training up to superintelligence, and you have to follow that mapping along.


So the general bet at Sequent \[[the former name of Resolution](https://resolution.org/post/rebrand) \] is this is a bunch of potentially good news that is very poorly understood. There’s not a lot of even toy models of this in theory that would tell you how that mapping emerges, is preserved, kind of changes through training. The hope is we can understand this better, and then that will separate algorithms to kind of break or preserve the right structures.


**Tom Reed:** That does sound like the kind of experiments that might be easier to do in a lab, though. Presumably some of these questions are just about the scale of the post-training that you’re doing and what that might do to the personas acquired in pretraining.


**Geoffrey Irving:** Yeah.


**Tom Reed:** Are you still optimistic?


**Geoffrey Irving:** I think I still am optimistic for a couple of reasons. One is that some of the papers I cited are just on open-source models at low scale. I think this is a particularly fruitful area for this mixture of empirics and theory, because I think that modelling low-dimensional structure is just a lovely thing to write down, theoretical-model wise.


So I think if the labs had enormous theory teams trying to explore the mathematics of that picture, that would be great. But they do not.


**Tom Reed:** But they should get them, in your view?


**Geoffrey Irving:** They should get them, but they’re just not. I think we went from an area where the labs were a bit dismissive of theory, to now they say they want to do it — and they’re still not doing it for various cultural and historical reasons. Maybe they’ll do it in the future, but for now we actually have to make some progress.


**Tom Reed:** That makes sense.


###


What the field of AI alignment still doesn’t know \[01:16:44\]


**Tom Reed:** I’m curious about how you think about the field of alignment. It strikes me that a bunch of other fields have these sort of core concepts that help organise our thinking. Something like[Nash equilibria](https://en.wikipedia.org/wiki/Nash_equilibrium) or atoms. Do you have a sense what are the equivalent concepts in alignment?


**Geoffrey Irving:** I think we have concepts. Certainly there’s reward hacking and models existing in different scales of complexity. But all of these have holes and gaps in ways that we don’t have in these other, more established fields.


I think a hopeful thing is that the field of alignment has been around for not more than like 20, 25 years at the most. Then there was very little work. There’s a lot of different areas of theory and approaches one could explore. And then most of the history has done very little of only a couple of approaches — some of which we’ll hopefully do at Resolution, some of which will be more novel.


So I think there is a potential for low-hanging fruit in even just finding the right definitions for those core concepts that are more resilient in theoretical-model land and then will better predict empirics going forwards. Just because people haven’t tried very hard yet.


**Tom Reed:** People haven’t tried that hard even though… OK, I mean, 25 years I guess is not that long.


**Geoffrey Irving:** Like say the whole field of personas empirically is just a couple of years old, like one to two years old. So no one has tried to write down solid theory for this over 10 years. If we only have two to three years — hopefully we have 10 years — but if we have a little time, then I think it could still be sufficiently low-hanging fruit that the combination of humans and a bunch of automation can get us some answers.


**Tom Reed:** Do you personally feel like your understanding of the field has changed very much from when you were first working at OpenAI?


**Geoffrey Irving:** I think it has. For example, I was thinking less about path dependence back then. This whole idea of low-dimensional structure I was not factoring in as much as I have in the last couple of years. Even obfuscated arguments, like this problem we can discuss in debate or scalable oversight generally, that I didn’t fully understand until a couple of years ago.


I think a lot of it has changed. And I think maybe, had the field not been advancing faster and faster overall, I’d be more optimistic that we would have a shot at solving the problem or making a big dent in the problem. But again, I think we have these glimmers of hope. It’s just then very little time.


**Tom Reed:** So the main source of pessimism is lack of time and the main source of optimism is these glimmers of hope, examples of positive generalisation.


**Geoffrey Irving:** The specific thing is low-dimensional structure. I think that both can give you negative but also positive generalisation in some ways, yeah.


**Tom Reed:** Will we be able to specify the superintelligence’s utility function if all the alignment research works?


**Geoffrey Irving:** Oh, no.


**Tom Reed:** That’s never going to happen?


**Geoffrey Irving:** Well, I don’t know. “Never” is too strong of a word. Until it’s too late, we would never be able to do that kind of precision. I think the only hope is if we learn or we luck out that we don’t need to hit that precise a target. I think it’s possible that in fact we do have to hit a precise target, in which case we’re not going to make it. If the structure of models helping, supervised models helping generate training data for models is sufficiently error-correcting and has some give, then there’s a hope.


**Tom Reed:** So we need to hope that there is this kind of basin, and we get high enough confidence that our training procedures are landing us in that basin. And that’s the best thing we’re going to hope for, basically?


**Geoffrey Irving:** That’s basically right, I think. There’s a question of, can you model out the situation to the point where you have a model that exhibits this phenomena, there being many basins? You can then calibrate that model against empirics in various ways and see how this works in practice.


One idea of a mathematical object one could try to construct with empirics is: imagine there’s the superintelligent-limit models, and there’s a variety of basins: some are good, some are bad. Can you write down a coarsened model and actually train a small model that trains along? You can kind of modify its branch points when it could go one way or the other, and you sort of draw a map through training space up to these basins such that you have literally a branching curve that starts out as a single curve and then it branches, and then it branches again. Maybe some of the branches converge back together.


And you could literally have 1,000 checkpoints of the model showing this map of training through time, such that you then have an object which you can play around and iterate and try different algorithms just like projected into the space of this kind of map of training.


**Tom Reed:** And what would you observe about how the branching works that would give you confidence that this will happen at superintelligence too?


**Geoffrey Irving:** I think you would have some mathematical model of this branching. It wouldn’t give you full confidence, but you could be able to see, “Oh, if I do this kind of algorithm, or maybe here is a test I can do to pinpoint or to narrow down when am I likely to branch such that I can apply more resources there or spend more effort.”


I think there is a bunch of hope that we could get to more understanding even on a short timescale. But I don’t know. Hope is not like a lot of probability. Just like, we should try.


**Tom Reed:** Do you think alignment will be the only scientific field that’s very difficult to automate? Are there other ones?


**Geoffrey Irving:** I think the general problem with alignment is that you don’t necessarily get more than one shot. We have some evidence from current models, which is important. So you don’t get exactly one shot, but the behaviour of models up at ASI, up at superintelligence, that may just be different and we have to understand that kind of in advance, if so.


Most other fields, you can try to structure things so that you get iteration. There are risks that are less like that coming from AI and other catastrophic risks. But a lot of fields have this kind of iterative potential, and then you’re in a much better place.


**Tom Reed:** Are you surprised at how much iteration we get at the current level, where it’s superhuman on some kind of tasks, but not broadly superhuman in some sense? It feels like this to me is potentially a positive surprise.


**Geoffrey Irving:** I think there’s some update there, but again, I update there much less than the lab folk do on average, just because I think we haven’t necessarily seen shifts.


And I think we do have, additionally, negative evidence — because there are cases when the current behaviour is a model of the future, or at least does show they’ve tried very hard to not have a bunch of reward hacking, and they still have a bunch of reward hacking in production, in deployed models. So I think that’s clearly a case where we don’t understand things well enough to have iterated enough to pound away the errors, which is bad news.


**Tom Reed:** Yeah, that makes sense.


###


Lessons from politics on how to combat power seeking \[01:24:36\]


**Tom Reed:** You’ve got this[great blog post](https://naml.us/post/lyndon-johnson/) from several years back where you make an analogy between LBJ’s presidency and aligning superintelligence.


And your point, if I understand it correctly, is LBJ, he’s motivated almost exclusively by power and wanting to acquire more power. He also has all sorts of asymmetric advantages against his opponents, where he’s better at being a politician than them. And yet the American political system still aligns him towards great positive outcomes like civil rights and the[Great Society](https://en.wikipedia.org/wiki/Great_Society) .


Maybe I’m stretching the analogy here, but what claims do you think it is about the American political system that can give you faith that LBJ will produce positive outcomes that you want? What does the LBJ predeployment safety case look like?


**Geoffrey Irving:** Yeah, so the first thing to say is I’m not going to take a stand at whether he was net good, because he also did a whole bunch of horrible things. I think the take is less that I’m confident that the system in fact aligned him to do good. I think he did probably want to do some good. He just thought, “I must gather all this power along the way to do good,” as many people think.


The case is more that this is a very poorly designed game. A nice analogy, which is fun, which I will cite from that post, is he became Senate majority leader because he realised that position had all this power that everyone else was leaving on the table. For example, he could choose, as majority leader in the Senate, when to call the vote. So he would just sit in the chamber watching people randomly go in and out of the chamber, I don’t know, to the bathroom or to get a snack or something. At some point, the balance of votes in the chamber was in his favour by a few votes — and he would call the vote and win, because he had a perfect memory of who was going to vote for him and extremely good predictions there.


But that is just a very badly designed game that was played. There was this one LBJ guy who’s incredibly good at the details and there was not the competing LBJ force trying to be a counterbalance. So I think when the American system works well, it is because there are effective balances and counterbalances. It’s not clear that those are always working well. But it’s also not clear that the American system is the uniquely best balance/counterbalance system we could have.


We do have the potential to have a more well-designed game and training process, more custom for this process. If you get this kind of counterbalancing, then I think you potentially can get through a lot of the problem.


An example is like if you had the other LBJ that was opposed to the first one, that’s saying, “By the way everyone, you realise what he’s doing here? He’s cheating the vote system.” And everyone is like, “That’s ridiculous. That’s clearly unfair. Let’s fix the rule to break that.” I think that intervention would get you so much power over the misaligned components of LBJ that I think it’s within hope to imagine getting that story right.


**Tom Reed:** So that’s an example of a system that’s poorly designed but actually reasonably easy to solve.


**Geoffrey Irving:** Yeah. There’s a more egregious example of this from[another one of Robert Caro’s books](https://en.wikipedia.org/wiki/The_Power_Broker) , which is: Robert Moses would write these bills for the New York state government to pass, which just contained these trick clauses that gave Robert Moses all this power. And then no one noticed the clauses until after they’d all passed the bill, and it was so late that they would have had to lose a tonne of face that they just rolled back the bill. If there had just been another Robert Moses opposed to the first one, saying, “This bill contains this horrible power-grab clause,” that would have been an unworkable strategy on Moses’s part.


So there is this potential for monitoring that’s much more invasive against the AIs, and various kinds of alignment schemes and our ability to intervene all throughout training in a way that you can’t with a human. There’s many affordances we have on this process that, in these examples of people that have gathered a bunch of power in misaligned ways, it just feels a bit fixable, if we get the situation right. Now, whether we’ll get it right, it’s a bit dicey, but there’s hope there.


**Tom Reed:** How many of these latent exploits do you think that human society probably has?


**Geoffrey Irving:** Just tonnes. Absolutely tonnes.


###


Solving Pentago and working at Pixar \[01:29:22\]


**Tom Reed:** I’m curious, how quickly and by what means do you think the first superintelligence would be able to solve[Pentago](https://en.wikipedia.org/wiki/Pentago) ? So you solved it.


**Geoffrey Irving:** I mean, Pentago can be solved in order of 10^17 or 10^18 FLOPS. So pretty fast.


**Tom Reed:** How is it doing that? Imagine it’s not in the training data. Is it literally just chain of thoughting?


**Geoffrey Irving:** Well, no. If I was a superintelligence trying to solve Pentago, if I cared about it, I could just run the whole computation again extremely cheaply using faster software that I was able to write.


There’s a question of like, can it do it? Pentago is a board game. One property of board games is they usually have some heuristic structure which you can intuit, and then below that structure is a huge amount of essentially random calculation. And the only way to see the calculation is doing the calculation.


So the question is, how well is Pentago modellable by heuristics? And I don’t know.


**Tom Reed:** You don’t have an intuition for this?


**Geoffrey Irving:** I have tried to train medium-small-scale neural networks to predict my cached opening at Pentago, and they don’t do as well as I was expecting them to do, like a priori. It’s possible that a fair amount of the structure of Pentago is kind of randomish. It’s also possible that, as you scale up a ways, it kind of phase shifts down to now it understands the heuristics and nails the story. But I don’t have a good cached sense.


**Tom Reed:** That makes sense, yeah. Did your time working on simulations at Pixar give you any kind of greater confidence in the ability to use simulations to understand things like physics or biology?


**Geoffrey Irving:** Certainly. My PhD was in computational physics. There’s a couple of things. One is the reason there is a field called physics which can make a bunch of predictions, like effective field theory or effective physics — which means that you don’t need to understand the high energy, the very fine structure to write down a model of coarse things. You can write down a theory of atoms, a theory of molecules, a theory of steel beams and so on without the theory of the thing below, the theory you’re currently modelling. And that robustly works across a wide variety of scales.


And I think there’s this generic hope that we’ve seen throughout physics and other areas of science that you don’t need to model the substructure a lot of the time.


There’s also hope for alignment, because that kind of intuition also says that maybe there’s theories of say speed-plus-heuristics, which don’t need to understand the architecture that we’re using or the details of the transformer or the like. They’re sort of quite generic, if you make some appropriately kind of creative assumptions about roughly what that substructure might look like.


**Tom Reed:** And that helps us if alignment is computationally reducible in this way, because it’s easier?


**Geoffrey Irving:** No, it helps us write down theories of alignment.


**Tom Reed:** Oh, I see, OK, yeah. Because you don’t need to understand the substructure. You can capture it with a high-level theory.


###


Geoffrey’s best prediction \[01:32:40\]


**Tom Reed:** I’m curious, when did it crystallise for you that this RL+LLMs essentially would be the path to superintelligence? As far as I understand, you were arguing for this already way back in 2019.


**Geoffrey Irving:** Yeah, 2018.


**Tom Reed:** Tell me, what did you see?


**Geoffrey Irving:** This is sad, but I don’t think I saw… It was not that complicated. In some sense I arrived at OpenAI in 2017, and Paul Christiano was already writing down schemes that had this idea of using language and reasoning to decompose things and then write alignment in terms of these language models. In fact, the summer I arrived,[Alec Radford](https://en.wikipedia.org/wiki/Alec_Radford) and Paul had tried to run RLHF on language and it hadn’t worked at that point. So that was kind of in the general area.


Then maybe the critical thing is AlphaGo, because I think we had a sense that we couldn’t do this. You couldn’t model things as explicit reasoning too much, because it’d be too slow, the models would do it a different way. And AlphaGo is just actually doing the tree computations. Mixing explicit reasoning with heuristics does give you the strongest thing on the planet to playing Go.


I think, one, it gave us some emotional licence to write down alignment algorithms based on this. But also it felt like that path of you do a bunch of reasoning, you can write it out, you can compress it, you can iterate in these kinds of environments that are about reasoning. Then you would get the ingredients for that from language models which OpenAI was exploring, and so was Google Brain as well, and a bit of DeepMind that could just take you all the way there.


I think part of this is I have a general take that a lot of this reasoning stuff is not magic. We just have a big bag of heuristics, including heuristics about how to reason, kinds of planning to do, ways to error-correct. Some of the intuition here is that somewhere in this sea of internet text there are a bunch of ways of reasoning that are good, there are a bunch of ways of reasoning that are bad. If you take that initial ingredient and then pick out the good parts of it and strengthen them with RL, you get all the way there.


So that was like 2018, and I told Dario — this is annoying — that I would write a document called “Language is enough to get to AGI” — and then I didn’t write it until 2019. So early 2019 is when I actually wrote the document. That was the way not just me but also other people there were thinking.


**Tom Reed:** What’s your model of why reinforcement learning from verifiable reward took so long to materialise? Why did o1 come out in \[2024\]? Why not before then?


**Geoffrey Irving:** I don’t know. I think some of it is tuning. Some of it is that, if you have this model of you have to get to sufficiently good error-correction to be able to reason for a long time without decaying, then as the pretrained base model improves, you get closer and closer to when you can get to lift off on the ability to do RL over a long reasoning trace.


But I don’t know. In some sense I would have expected it to happen a bit earlier, and I was wrong.


**Tom Reed:** And what’s your model of what RL exactly is doing to the pretrained model? It’s like selecting for parts of the pretraining distribution which already contain useful reasoning traces? Is it teaching it generalisable strategies for reasoning? Which of these matters?


**Geoffrey Irving:** I think a part of it is that a lot of human reasoning strategies as written in language just are generalisable because we’ve learned patterns that apply to a lot of different domains. So the first thing that it does is just down-select modes of behaviour to remove the unworkable kinds of reasoning.


**Tom Reed:** Some of which I’ve created online or something.


**Geoffrey Irving:** But the other thing is people often write down the final answer and not the chain of reasoning that got them there. And if you try to have a model predict the final answer, it’s just going to be forced to hallucinate unless it can do all the reasoning that a human did off stage in its latent pass.


And somehow there was a combination of tuning of RL algorithms plus sufficiently strong base models, and around o1 those started to work well.


###


Geoffrey’s best bets on which alignment techniques will work \[01:37:38\]


**Tom Reed:** If you had to make a bet about what alignment technique is ultimately going to end up working, do you have a spidey sense? Do you have a frontrunner right now?


**Geoffrey Irving:** Some combination of personas and understanding of learning dynamics and scalable oversight. And then I think I mentioned agent foundations and philosophy, and in some sense those both play into how to think about pieces of that story.


A lot of the agent foundations work is thinking about ways of modelling the limit, ways of thinking about path dependency, models reasoning about themselves — in a way that you have to untangle some recursive loop. That understanding could also teach us how to do the other components of scalable oversight or personas or the like, or would replace them in some way or something.


I think an important principle for the org is that I come to this with my inside-view sense of how things could go. Right now maybe that’s like scalable oversight plus personas plus learning theory or learning dynamics kind of coming together and sort of fitting each other’s holes in some way.


But we also as an org will have this outside-view perspective of we’re going to take a lot of different bets. Not everyone should have the same view about how the pieces will fit together. The hope is again that we don’t have to get success in all of the areas to win. We will try a bunch of things, and if we get important insights and algorithms or obstacles from some, or even just one area, that could be enough to account for the entire org.


**Tom Reed:** Is there a future where timelines look so short that you just decide we need to focus all our resources on one single bet, because this approach of trying to aim for lots of things doesn’t make sense anymore?


**Geoffrey Irving:** Here are three reasons. I have a cached answer here of, structurally, why we wouldn’t want to do that.


One is that there’s strong diminishing returns, usually, in token or DPU spend. You’d have to be really confident in a particular area to not want to hedge your bets and give the other areas enough that they can continue to be reasonably well automated. Hopefully in this world, where you’ve established some strong progress in one of your areas, you can raise a tonne of money — but you probably do want to spend a decent chunk of that, just lower, on other areas, to take advantage of this diminishing-return curve.


The next one is that it’s possible we get all the way to the end, or really near the end, where you’ve trained a superintelligent model and people are still bickering about timelines — even within the lab, but certainly one step removed in nonprofits. I found it very fascinating how far we’ve gotten into this AI-takeoff scenario that we’re all living inside and still we have these massive disagreements about whether things are slow or they’re saturating or the like. And somehow my model is like we’re still not going to know what the timelines are maybe a week before someone trains a superintelligent model externally.


Then finally, if you have to make a trade, part of org design at Resolution will be arranging things for psychological safety as we go into this kind of crazy-town world of accelerating AI. And that means if we’re working on automation, prepare so that people know they’re not going to just get snap fired with no warning, that kind of thing; know that we wouldn’t make this horrible trade where we kick them out of the org or whatever, and they have to scramble to find the new thing if they still believe.


Those are all just like bad plans. So I think we’ll want to design Resolution, but also a lot of other companies will face similar challenges of designing the culture and the plans within companies and research labs and so on to prepare for lots of change. One way to prepare for change is to say, “We’re not going to cut you out of all of your resources at the last minute just because we think we’ve got to confidence.”


**Tom Reed:** Beyond not snap firing people, what are the other things that you can do to build a culture in this world?


**Geoffrey Irving:** One thing I’ve learned over time is that it’s very important even just how you craft Slack channels, so that people feel comfortable speaking. You could imagine it’s very bad if, before automation, you have a Slack channel where say a bunch of junior researchers are discussing their details of research and you have a bunch of high-up executives just lurking and observing what’s going on. This inevitably just pushes it into direct messages or something like that.


There should be some intention required to put your thinking, your context into the machines. You should be doing that in a way that you kind of want to do it. You should have the option of having meetings obviously that are not watched by the machines. There’s some designing of a non-dystopian org, which I think is table stakes; it should be easy to do, but you have to do that intentionally.


I think some companies have gone a bit too far in this direction and gotten a bunch of backlash, and could have for unintentional reasons. There is a desire, if you’re trying to automate things, of having all of the context available to machines. But you shouldn’t do that too much, because it would be a bit dystopian.


**Tom Reed:** Not totally indiscriminate.


###


Work with Geoffrey at Resolution \[01:43:34\]


**Tom Reed:** What kinds of talent are you most hoping to get into Resolution?


**Geoffrey Irving:**[We are looking](https://jobs.80000hours.org/?refinementList%5Bcompany_data%5D%5B0%5D=Resolution&utm_source=80k_podcast) for a mixture of very standard ML engineering and research talent for automation for some of the empirics, and then also hopefully a decent number of very strong mathematicians and computer scientists and physicists to push forward these various frontiers of theory research.


I think part of the story, the claim, the founding bet here — and also to some extent when we were doing the AISI alignment project back at AISI — is not much has been tried, so we haven’t really treated, as a world, alignment as a problem worthy of taking the best researchers from various fields and putting them on the problem.


Now that has gotten easier, because everyone is getting more worried — and still I think not enough research has happened to be confident that there isn’t low-hanging fruit. Possibly the definitions are fairly shallow. If you get people who are very good, they can find the right way to model the situation without even that much fancy mathematics, but just some understanding of how we approximate superintelligence on paper. And that might give us the answer to how to make this go well.


I think there is this very important principle of: the shallower the mathematics, the more likelihood there is of fast progress. If we had to do just an enormous amount of incredibly deep theory-building across decades and decades of time, that would be very rough. If it’s like no one has really found a good way of modelling this notion of speed-plus-heuristics and reasoning about the complexity theory of that class of algorithms, that could be a thing that we make progress on in six months or a year.


Then I hope that we can make a very fun environment, where the human creativity part of the problem, or eventually the machine creativity, is finding these definitions, figuring out how to model the situation — both alignment and capabilities of these models.


If you have below you a bunch of automation for expanding out candidate conjectures and proving them correct, or finding counterexamples, or doing numerical experiments — and all of that, the models are very, very good at, because it’s the thing they’re already good at and they’ll keep getting better — then you can kind of play in definition space, play in modelling space.


**Tom Reed:** Which is the most fun thing to do.


**Geoffrey Irving:** Which is the most fun thing to do.


**Tom Reed:** Maybe this doesn’t make sense as a question, but what are the clear definitions that you’d be keen for us to get a better sense of? So one is speed, how to define this speed-and-heuristics model…?


**Geoffrey Irving:** I think speed and heuristics. An example of a toy model I would like to see is: right now, the labs do some pretraining, they take a model, they ask the model to make some data, they train on the data, they iterate this weird process. They might have literally thousands of different modes of asking the model for data. It’s a very complicated object, just like a modern training stack.


But you could imagine distilling this down to some very simple model, which is like you just have again pretraining plus self-generation and you iterate that. Maybe that already captures enough of the flavour of RL that you don’t even need to add RL as a component to that model. If you could build that toy mathematical model such that it represents emergent misalignment and subliminal learning and these other phenomena we’ve seen in the last few years, and then explore them more rigorously — both in theory and in doing maybe very scaled-down empirics — that gives us a playground with which to explore in algorithm space.


**Tom Reed:** And you would use this model to understand subliminal learning?


**Geoffrey Irving:** Yeah, subliminal learning is when you have a personality trait of a model and it generates data for another model and then the next model inherits the trait, even if the data generating it is unrelated to the trait.


**Tom Reed:** But there’s a[model](https://subliminal-learning.com/) which you’ve trained to like owls. You get that to output a bunch of numbers and you train a different model on those bunch of numbers. And it somehow also inherits the fondness for owls.


**Geoffrey Irving:** But I think the “somehow” is actually not that mysterious to a first intuitive approximation. It’s just because there’s this low-dimensional structure, and the liking for owls is correlated with all these random other things, including numbers. Then that structure is flowing through this channel and then showing up in the resulting model.


**Tom Reed:** And your intuition is we have a pretty good chance of understanding how that low-dimensional structure forms on a theoretical level?


**Geoffrey Irving:** Yes. Then if we have that understanding, we can use it as a lens to rule in or out various algorithms as being optimistic or pessimistic. Will they succeed in preserving or mapping that structure in the way that we want across?


One of the traits of a world-class theorist is just definitional creativity. And I think to some degree there’s enough of a chance that can be ported across to this new area of alignment — new to them — that we can make progress quickly.


**Tom Reed:** Sounds like a good deal for them.


**Geoffrey Irving:** I think so. Also we can pay them well, so that’ll be good as well. I guess maybe the big thing to say is, again, we have some inside-view reason why we like each of the individual areas we’re thinking about — like learning theory, scalable oversight, personas, agent foundations, philosophy, this kind of thing — but we may have missed some.


If you have a thing you want to do, if you like theory — and you buy the general story of gains to scale for org scale, of having shared automation and being able to share ideas between areas, and you think this is a good place to work — but it’s not in the list that we’ve given in this podcast, still reach out; still pitch us. I guess an important principle is that we will want to believe in you to some extent, but not that much. Everything here is a bank shot; we’re just trying to spread the probability around a bit more than the current labs are doing.


Also, we don’t need every area to be the same size. If there’s a few people working on a particular area, if we have small critical mass, I think that can still be quite powerful. We expect a lot of the basic understanding of both how to do automation for theory and also just basic things like reward hacking — how to model it, this kind of thing — those may generalise across areas of theory in ways that are quite useful.


**Tom Reed:** So you’ve picked a bunch of fields which you would love to hire for for Resolution. How did you pick those particular fields? What is it about[complexity theory](https://en.wikipedia.org/wiki/Computational_complexity_theory) or other fields that is why you think that’s going to be particularly helpful for your research agenda?


**Geoffrey Irving:** There’s kind of this inside-view case for complexity theory that it’s like modelling superintelligence, and weak and strong amounts of compute and how they relate. But then the outside-view case is that we do just need a more rigorous understanding of this problem as a whole, this problem of alignment. And there’s a bunch of areas that might be relevant for that. We want to try to be a home for a bunch of those in a way that takes advantage of scale by sharing automation and sharing ideas, sharing how to model the basic concepts of reward hacking and misalignment and so on. The hope is that that scale will give us faster progress throughout different areas.


Also, we don’t think we are the only game in town. So we will try to publish things. We want to be a friendly member of the community, feeding back. It’s possible that we have some ideas and then someone else takes those and actually solves the problem in some useful way. So we’ll try to get that balance right as well.


**Tom Reed:** If you do this research, you try and find solutions to these obstacles. But what happens if you don’t find these solutions? You yell. What exactly does that yelling look like?


**Geoffrey Irving:** I think I actually misstated this in the[initial blog post](https://resolution.org/launch/) , where it’s like we might need to yell as if it was like an eventual thing we do. I think rather the thing is try to build a culture and a comms practice and so on where we’re just putting out this mixture of obstacles and glimmers of success throughout all the time.


If we find a way of modelling the alignment problem that says it is hard, that is extremely valuable as a publication and should be celebrated as such. Both because it might tell you that you need to pause, it might tell you need to be more careful, or it might be the thing you need to then filter down and narrow in on the right solution in the long term. I think building a culture of equally celebrating both positive and negative results is key to this whole exercise.


And a hopeful thing there is, in complexity theory, in various areas of physics and mathematics, some of the highest profile results are obstacles. In[complexity theory](https://en.wikipedia.org/wiki/Computational_complexity_theory) , there are actually[three obstacles to P versus NP](https://en.wikipedia.org/wiki/P_versus_NP_problem) called relativisation, algebraisation, and natural proofs. Those are buzzwords, people celebrate them, they’re very famous. In physics, there’s the[Firewall paradox](https://en.wikipedia.org/wiki/Firewall_(physics)) , which is an obstacle about how does quantum gravity work near black holes? Which again is very celebrated, and has this cool name: the Firewall paradox. The hope is that that culture is not something we have to create afresh. It’s a thing that pervades these areas of theory already.


I think bringing that in and finding those obstacles is both a necessary part of the modelling process, and then also either plays into, “Hey, we should slow down even more, because we have these horrible obstacles”; or it tells you to ramp up data or care and time in some algorithm which kind of might work, might not work; or it says, “Here’s the lens your new algorithm has to go through” and it lets you find it faster.


###


The dangerous asymmetry between capabilities and alignment \[01:54:17\]


**Geoffrey Irving:** So we published a paper, “[Automated alignment is harder than you think](https://arxiv.org/abs/2605.06390) .” And the reason why I think this fuzzy evidence problem applies more to alignment is that I think there’s more of a story for how to incrementally work on the problem of improving capabilities across time with capabilities than there is for alignment.


You just get to do[hill-climbing](https://en.wikipedia.org/wiki/Hill_climbing) in some sense on capabilities. And labs mess up; they do sometimes produce models that lie more or more reward hacking. They have to go back and fix the training signal. They do this both internally within labs, but also some deployments have been missteps in this that they had to fix. But they generally get to kind of climb this hill of gradually improving capabilities because we can measure them.


I think, because of this effect, everything could shift as you cross through human-level intelligence. If you want to do a bunch of research with machine automation prior to human-level, you don’t necessarily learn that much — or you learn some, but not as much as you would like — about this future superintelligence. So the worry is you just don’t really see what’s going on. Your experiments you’ve done for prosaic alignment on avoiding current model reward hacking just don’t tell you what you need to know about the superintelligence. So you can automate them, but you haven’t automated this conceptual modelling of when things will break down or not, as you go through this kind of scaleup.


**Tom Reed:** So the reason that the iteration works for capabilities but not for alignment is because the phase shift between subhuman and superhuman applies in alignment, but it doesn’t apply for capabilities?


**Geoffrey Irving:** I think it does. So the question is, say we get to ASI in, I don’t know, five years. I think the skills you will have learned in the meantime on capabilities, they will be skills that got you to the next rung up, and then as you go.


So the question is, when we get up to human level, what will happen? Up to human level you can supervise the model, so you can still be hill-climbing. Even past human level, it’ll become harder, but you still get to, say, run the model for a small amount of time and then supervise it with more human attention, or supervision is a bit easier. So in the areas where you can do this thing, you can still be climbing.


Then the question is, what happens around this point? My intuition is that when you’re doing a very difficult software engineering task, you have to do a tonne of planning and subtle reasoning to be able to, say, do a month’s worth of human-type work as a model over a period of a day or an hour or a week. I don’t know how long it will take. So the question is, if you hill-climb your way up to a model that can do that level of reasoning, are you close enough to the danger point that you’ll get there by proximity, or will you kind of stall out at that point? The reason I think you won’t stall out is that that’s already stronger than humans, so I just expect it to continue basically via momentum up past this level.


Also one thing to say is the way to get to superintelligent alignment is I think at least some component of scalable oversight where the model is supervising the models. All of the labs are doing some version of this. They’re just doing the empirical hill-climbing version. There’s a big space of possible scalable oversight algorithms. My claim is that some of these work for alignment, some of them don’t work, but we will be able to be hill-climbing our way to things that work empirically. That will give us some ability to kind of push past human-level by a fair way, just from kind of this overhang of hill-climbing on scalable oversight algorithms. Then the worry is that we’ve picked the wrong ones.


**Tom Reed:** And we won’t know.


**Geoffrey Irving:** We won’t know. And that will shift. But I think this is an area where some people have very different intuitions that in fact that this effect will cause capabilities to stall. I think it is one of the arguments against the speed.


**Tom Reed:** I guess it kind of relates to the Go example you said before, where you try and train a superintelligent Go model, but against a very bad Go player, it will also learn bad strategies. And you initially said that’s the reason why if you personally don’t understand what’s going on, you might not be able to reward train the model to do what you want it to do, even if you’re doing amounts of compute that would normally generate superintelligent play. It’s occurred to me that would be an argument why capabilities would slow. In that case, the capabilities are slowing, and we’re also not aligning it to what we want.


**Geoffrey Irving:** Notably though, literally what happened in AlphaGo is they did a bunch of iteration. Sometimes they did mess up and they trained a model against itself in a way that overfit to some weird model distribution. It got to apparently superhuman[Elo](https://en.wikipedia.org/wiki/Elo_rating_system) playing against itself and past versions of itself. Then they tried it against the human, and the human wiped the floor with the model, and then they tweaked something and then that was fixed. And then again the model wiped the floor with the human.


So the question is, as you’re playing around in this space trying to do model self-supervision, can you do that kind of iterative tinkering?


I’m more optimistic that that tinkering gets you capabilities, because if you mess up, you get a model which is weak and then you’re like, “This model is shit, I can’t use it to do things.” And you notice that over time — maybe it takes you a little while to realise it because it’s domain-superhuman — then you fix it.


So the failure mode is towards weak models that then almost by definition you just notice that eventually and fix it. It might take some time. The failure mode for alignment is you make a mistake, you deploy the model, it takes over the world and then you’re done.


So I think if you had this model of the world where, say, we imagined they were perfectly symmetric, and there was the same failure rate for a given deployment of an AI model to have failed to get this ad-hoc tuned, scalable oversight right — the same error rate between capabilities and alignment.


Then up at superintelligence land, say 20% of the time you fail and your model is bad — you miss a generation — and then 20% of the time also the model takes over the world. And one of those two things you can iterate and the other one you can’t.


**Tom Reed:** Yeah, OK. That makes sense.
